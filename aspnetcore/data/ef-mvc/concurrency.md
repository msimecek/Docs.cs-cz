---
title: "Jádro ASP.NET MVC s EF Core - souběžnosti - 8, 10"
author: tdykstra
description: "Tento kurz ukazuje způsobu řešení konfliktů, když se více uživatelů aktualizace stejné entity ve stejnou dobu."
keywords: "ASP.NET Core Entity Framework Core, souběžnosti"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 15e79e15-bda5-441d-80c7-8032a2628605
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: ffe8ef968d7bde9755d5c55389f6f1548f03ffec
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/02/2017
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a><span data-ttu-id="254e5-104">Zpracování konfliktů souběžnosti – základní EF s kurz k ASP.NET MVC jádra (8 10)</span><span class="sxs-lookup"><span data-stu-id="254e5-104">Handling concurrency conflicts - EF Core with ASP.NET Core MVC tutorial (8 of 10)</span></span>

<span data-ttu-id="254e5-105">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="254e5-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="254e5-106">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="254e5-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="254e5-107">Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).</span><span class="sxs-lookup"><span data-stu-id="254e5-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="254e5-108">V dřívějších kurzy zjistili, jak aktualizovat data.</span><span class="sxs-lookup"><span data-stu-id="254e5-108">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="254e5-109">Tento kurz ukazuje způsobu řešení konfliktů, když se více uživatelů aktualizace stejné entity ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="254e5-109">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="254e5-110">Vytvoříte webové stránky, které pracovat entity oddělení a zpracování chyb souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-110">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="254e5-111">Na následujících obrázcích je upravit a odstranit stránky, včetně některé zprávy, které se zobrazí, pokud dojde ke konfliktu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-111">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Stránka Upravit oddělení](concurrency/_static/edit-error.png)

![Odstranit stránky oddělení](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="254e5-114">Konflikty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="254e5-114">Concurrency conflicts</span></span>

<span data-ttu-id="254e5-115">Concurrency dojde ke konfliktu jeden uživatel zobrazí entity data ji Pokud chcete upravit, a pak jiný uživatel aktualizuje stejné entity data před první uživatel změnu je zapsána do databáze.</span><span class="sxs-lookup"><span data-stu-id="254e5-115">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="254e5-116">Pokud nepovolíte detekce takové konflikty, kdo aktualizuje databázi přepíše poslední změny jiného uživatele.</span><span class="sxs-lookup"><span data-stu-id="254e5-116">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="254e5-117">V mnoha aplikacích, je přijatelné toto riziko: Pokud existuje několik uživatelů nebo několik aktualizací, nebo pokud není opravdu důležité, pokud jsou některé změny přepsány, náklady na programování pro concurrency vyváží výhody.</span><span class="sxs-lookup"><span data-stu-id="254e5-117">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="254e5-118">V takovém případě nemáte konfigurace aplikace pro zpracování konfliktů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-118">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="254e5-119">Pesimistické souběžnosti (uzamčení)</span><span class="sxs-lookup"><span data-stu-id="254e5-119">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="254e5-120">Pokud aplikace třeba zabránit, aby před náhodnou ztrátou dat v concurrency scénáře, je jeden ze způsobů použití uzamčení databáze.</span><span class="sxs-lookup"><span data-stu-id="254e5-120">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="254e5-121">Tomu se říká pesimistické souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-121">This is called pessimistic concurrency.</span></span> <span data-ttu-id="254e5-122">Například předtím, než se pustíte do čtení řádek z databáze, můžete požádat o zámku pro jen pro čtení nebo pro přístup k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="254e5-122">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="254e5-123">Pokud zamknete řádek pro přístup k aktualizaci, je povoleno uzamčení řádek buď pro žádní jiní uživatelé jen pro čtení nebo aktualizace přístup, protože by získají kopii dat, která je právě probíhá změnit.</span><span class="sxs-lookup"><span data-stu-id="254e5-123">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="254e5-124">Pokud zamknete řádek pro oprávnění jen pro čtení, ostatní také možné zamknout ho pro přístup jen pro čtení, ale není pro aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="254e5-124">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="254e5-125">Správa zámky má nevýhody.</span><span class="sxs-lookup"><span data-stu-id="254e5-125">Managing locks has disadvantages.</span></span> <span data-ttu-id="254e5-126">Může být složité programu.</span><span class="sxs-lookup"><span data-stu-id="254e5-126">It can be complex to program.</span></span> <span data-ttu-id="254e5-127">Vyžaduje významné databáze správy prostředků, a jako počet uživatelů, aplikace, může to způsobit problémy s výkonem zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="254e5-127">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="254e5-128">Z těchto důvodů ne všechny databázové systémy podporovat pesimistické souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-128">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="254e5-129">Entity Framework Core poskytuje žádné integrovanou podporu pro něj, a v tomto kurzu není ukazují, jak ho implementovat.</span><span class="sxs-lookup"><span data-stu-id="254e5-129">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="254e5-130">Optimistickou metodu souběžného zpracování</span><span class="sxs-lookup"><span data-stu-id="254e5-130">Optimistic Concurrency</span></span>

<span data-ttu-id="254e5-131">Alternativa k pesimistické souběžnosti je optimistickou metodu souběžného.</span><span class="sxs-lookup"><span data-stu-id="254e5-131">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="254e5-132">Optimistickou metodu souběžného znamená povolení konfliktů souběžnosti provést a reaktivní správně, pokud tomu tak je.</span><span class="sxs-lookup"><span data-stu-id="254e5-132">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="254e5-133">Například Jana navštíví oddělení upravit stránku a změní velikost nároky pro angličtinu oddělení z $350,000.00 na 0,00 Kč.</span><span class="sxs-lookup"><span data-stu-id="254e5-133">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![Změna nároky na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="254e5-135">Před kliknutím na Jana **Uložit**, Jan navštíví stejné stránce a změny pole Počáteční datum 9/1/2013 z 9/1/2007.</span><span class="sxs-lookup"><span data-stu-id="254e5-135">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Změna počáteční datum na 2013](concurrency/_static/change-date.png)

<span data-ttu-id="254e5-137">Jana klikne **Uložit** první a zobrazí ji změnit, když prohlížeč vrátí na indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="254e5-137">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![Nároky změnit tak, aby nula.](concurrency/_static/budget-zero.png)

<span data-ttu-id="254e5-139">Pak klikne na tlačítko Jan **Uložit** na stránku úpravy, která se zobrazuje nároky $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="254e5-139">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="254e5-140">Co se stane dále je určen jak zpracování konfliktů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-140">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="254e5-141">Mezi možnosti patří následující:</span><span class="sxs-lookup"><span data-stu-id="254e5-141">Some of the options include the following:</span></span>

* <span data-ttu-id="254e5-142">Můžete udržovat přehled o vlastností, které uživatel změnil a aktualizovat na odpovídající sloupce v databázi.</span><span class="sxs-lookup"><span data-stu-id="254e5-142">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="254e5-143">V ukázkovém scénáři by byl žádná data ztratí, protože různé vlastnosti aktualizoval dva uživatele.</span><span class="sxs-lookup"><span data-stu-id="254e5-143">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="254e5-144">Při příštím někdo umožňuje anglické oddělení, se zobrazí na Jana i Jan pro změny – počáteční datum 9/1/2013 a nároky nulové dolarů.</span><span class="sxs-lookup"><span data-stu-id="254e5-144">The next time someone browses the English department, they'll see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="254e5-145">Tato metoda aktualizace může snížit počet konfliktům, ke kterým může dojít ke ztrátě dat, ale nemůže vyhnuli ztrátě dat, pokud konkurenční změn stejnou vlastnost entity.</span><span class="sxs-lookup"><span data-stu-id="254e5-145">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="254e5-146">Jestli Entity Framework funguje takto závisí na tom, jak implementovat aktualizace kódu.</span><span class="sxs-lookup"><span data-stu-id="254e5-146">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="254e5-147">Je často není praktické ve webové aplikaci, protože může vyžadovat, aby udržení přehledu o všechny původní hodnoty vlastností pro entitu a také nové hodnoty udržovat velkých objemů stavu.</span><span class="sxs-lookup"><span data-stu-id="254e5-147">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="254e5-148">Zachování velkých objemů stavu může ovlivnit výkon aplikace, protože buď vyžaduje prostředky serveru nebo musí být součástí webové stránky (například v skrytá pole) nebo do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="254e5-148">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="254e5-149">Můžete je nechat Jan pro změnu Jana změna přepsána.</span><span class="sxs-lookup"><span data-stu-id="254e5-149">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="254e5-150">Při příštím někdo umožňuje anglické oddělení, se zobrazí 9/1/2013 a obnovený $350,000.00 hodnotu.</span><span class="sxs-lookup"><span data-stu-id="254e5-150">The next time someone browses the English department, they'll see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="254e5-151">Tento postup se nazývá *klienta Wins* nebo *poslední ve službě Wins* scénář.</span><span class="sxs-lookup"><span data-stu-id="254e5-151">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="254e5-152">(Všechny hodnoty z klienta přednost co je v úložišti.) Jak jsme uvedli v Úvod do této části, pokud tak učiníte jakéhokoli kódování pro zpracování souběžnosti, to se stane automaticky.</span><span class="sxs-lookup"><span data-stu-id="254e5-152">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="254e5-153">Jan pro změnu může zabránit aktualizaci v databázi.</span><span class="sxs-lookup"><span data-stu-id="254e5-153">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="254e5-154">By obvykle, zobrazí se chybová zpráva, zobrazit jeho aktuální stav data a povolit mu jeho změny znovu použijte, pokud chce je provést.</span><span class="sxs-lookup"><span data-stu-id="254e5-154">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="254e5-155">Tento postup se nazývá *Wins úložiště* scénář.</span><span class="sxs-lookup"><span data-stu-id="254e5-155">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="254e5-156">(Hodnoty úložiště dat mají přednost před odeslané klientem hodnoty.) V tomto kurzu budete implementovat scénář úložiště služby Wins.</span><span class="sxs-lookup"><span data-stu-id="254e5-156">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="254e5-157">Tato metoda zajišťuje, že jsou bez uživatele se zobrazí upozornění, na co se děje přepsat žádné změny.</span><span class="sxs-lookup"><span data-stu-id="254e5-157">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="254e5-158">Zjišťování konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="254e5-158">Detecting concurrency conflicts</span></span>

<span data-ttu-id="254e5-159">Můžete vyřešit konflikty zpracování `DbConcurrencyException` výjimky, které vyvolá rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="254e5-159">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="254e5-160">Chcete-li vědět, kdy má být vyvolána tyto výjimky, musí být schopna zjistit konflikty rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="254e5-160">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="254e5-161">Proto musíte nakonfigurovat databázi a datový model správně.</span><span class="sxs-lookup"><span data-stu-id="254e5-161">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="254e5-162">Některé možnosti aktivace zjišťování konfliktů, patří:</span><span class="sxs-lookup"><span data-stu-id="254e5-162">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="254e5-163">V tabulce databáze patří sledování sloupec, který slouží k určení, kdy se změnil na řádek.</span><span class="sxs-lookup"><span data-stu-id="254e5-163">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="254e5-164">Potom můžete nakonfigurovat rozhraní Entity Framework zahrnovat tento sloupec v Where klauzule SQL aktualizace nebo odstranění příkazy.</span><span class="sxs-lookup"><span data-stu-id="254e5-164">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="254e5-165">Datový typ sloupce sledování je obvykle `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="254e5-165">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="254e5-166">`rowversion` Hodnota je pořadové číslo, které se zvýší pokaždé, když se aktualizuje na řádek.</span><span class="sxs-lookup"><span data-stu-id="254e5-166">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="254e5-167">V příkazu Update nebo Delete Where klauzule obsahuje původní hodnota sloupce sledování (původní verze řádku).</span><span class="sxs-lookup"><span data-stu-id="254e5-167">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="254e5-168">Pokud se změnila řádek aktualizován jiným uživatelem, hodnota v `rowversion` sloupec je jiná než původní hodnota, takže příkaz Update nebo Delete nelze najít řádek, abyste aktualizovat z důvodu Where klauzule.</span><span class="sxs-lookup"><span data-stu-id="254e5-168">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="254e5-169">Když najde Entity Framework, že byly aktualizovány žádné řádky aktualizace nebo odstranění příkazů (to znamená, když počet ovlivněných řádků je nulová), interpretuje, jako konflikt souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-169">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="254e5-170">Nakonfigurujte rozhraní Entity Framework zahrnují původní hodnoty každý sloupec v tabulce v Where klauzuli Update a Delete.</span><span class="sxs-lookup"><span data-stu-id="254e5-170">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="254e5-171">Jako první možnost, pokud se nic v řádku změnila vzhledem k tomu, že byl řádek nejdřív přečíst Where klauzule nevrátí řádek aktualizace, které rozhraní Entity Framework interpretuje jako konflikt souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-171">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="254e5-172">Pro tabulky databáze, které mají mnoho sloupců, tento přístup má za následek velké tam, kde klauzule a může vyžadovat udržovat velkých objemů stavu.</span><span class="sxs-lookup"><span data-stu-id="254e5-172">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="254e5-173">Jak již bylo uvedeno dříve, údržbu velkých objemů stavu může ovlivnit výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="254e5-173">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="254e5-174">Proto se obecně nedoporučuje tento přístup, a není to metoda použitá v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="254e5-174">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="254e5-175">Pokud chcete implementovat tento přístup k concurrency, je nutné označit všechny vlastnosti primárního klíče entity, kterou chcete sledovat souběžnosti pro přidáním `ConcurrencyCheck` je atribut.</span><span class="sxs-lookup"><span data-stu-id="254e5-175">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="254e5-176">Tato změna umožňuje zahrnout všechny sloupce v klauzuli Where příkazu SQL příkazy Update a Delete rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="254e5-176">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="254e5-177">Ve zbývající části tohoto kurzu přidáte `rowversion` sledování vlastnost entity oddělení, vytvořte řadič a zobrazení a otestovat a ověřit, že všechno funguje správně.</span><span class="sxs-lookup"><span data-stu-id="254e5-177">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="254e5-178">Přidat vlastnost sledování do oddělení entity</span><span class="sxs-lookup"><span data-stu-id="254e5-178">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="254e5-179">V *Models/Department.cs*, přidejte sledování vlastnost s názvem RowVersion:</span><span class="sxs-lookup"><span data-stu-id="254e5-179">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="254e5-180">`Timestamp` Atribut určuje, že v tomto sloupci, budou zahrnuty v Where klauzuli Update a Delete odeslal do databáze.</span><span class="sxs-lookup"><span data-stu-id="254e5-180">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="254e5-181">Atribut se nazývá `Timestamp` protože předchozí verze systému SQL Server používá SQL `timestamp` datového typu než SQL `rowversion` jej nahradit.</span><span class="sxs-lookup"><span data-stu-id="254e5-181">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="254e5-182">Typ formátu .NET pro `rowversion` je bajtové pole.</span><span class="sxs-lookup"><span data-stu-id="254e5-182">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="254e5-183">Pokud dáváte přednost použijte rozhraní fluent API, můžete použít `IsConcurrencyToken` – metoda (v *Data/SchoolContext.cs*) k určení vlastnosti, sledování, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="254e5-183">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="254e5-184">Přidáním vlastnosti jste změnili model databáze, takže je třeba provést další migraci.</span><span class="sxs-lookup"><span data-stu-id="254e5-184">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="254e5-185">Uložte změny a sestavte projekt a potom zadejte následující příkazy v příkazovém okně:</span><span class="sxs-lookup"><span data-stu-id="254e5-185">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a><span data-ttu-id="254e5-186">Vytvořit řadič oddělení a zobrazení</span><span class="sxs-lookup"><span data-stu-id="254e5-186">Create a Departments controller and views</span></span>

<span data-ttu-id="254e5-187">Stejně jako dříve pro studenty, kurzy a vyučující vygenerujte řadič oddělení a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="254e5-187">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![Oddělení vygenerované uživatelské rozhraní](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="254e5-189">V *DepartmentsController.cs* souboru, změňte všechny čtyři výskyty "FirstMidName" na "FullName" tak, aby oddělení správce rozevírací seznamy bude obsahovat celý název lektorem a nikoli pouze poslední název.</span><span class="sxs-lookup"><span data-stu-id="254e5-189">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a><span data-ttu-id="254e5-190">Aktualizace zobrazení Index oddělení</span><span class="sxs-lookup"><span data-stu-id="254e5-190">Update the Departments Index view</span></span>

<span data-ttu-id="254e5-191">Generování uživatelského rozhraní stroj vytvořil RowVersion sloupec v indexu zobrazení, ale toto pole by se neměly zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="254e5-191">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="254e5-192">Nahraďte kód v *Views/Departments/Index.cshtml* následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="254e5-192">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

<span data-ttu-id="254e5-193">To změní záhlaví "Oddělení", odstraní sloupec RowVersion a zobrazuje úplný název místo křestní jméno pro správce.</span><span class="sxs-lookup"><span data-stu-id="254e5-193">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-the-edit-methods-in-the-departments-controller"></a><span data-ttu-id="254e5-194">Aktualizace metod úpravy v kontroleru oddělení</span><span class="sxs-lookup"><span data-stu-id="254e5-194">Update the Edit methods in the Departments controller</span></span>

<span data-ttu-id="254e5-195">V obou třídy MetadataExchangeClientMode `Edit` metoda a `Details` metody přidat `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="254e5-195">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="254e5-196">V třídy MetadataExchangeClientMode `Edit` metody přidat přes načítání pro správce.</span><span class="sxs-lookup"><span data-stu-id="254e5-196">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

<span data-ttu-id="254e5-197">Nahraďte stávající kód httppost `Edit` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="254e5-197">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

<span data-ttu-id="254e5-198">Kód začíná při pokusu o čtení oddělení aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="254e5-198">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="254e5-199">Pokud `SingleOrDefaultAsync` metoda vrátí hodnotu null, z oddělení byla odstraněna jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="254e5-199">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="254e5-200">V takovém případě kód používá hodnoty odeslaného formuláře vytvořit entitu oddělení tak, aby stránce Upravit můžete zobrazí znovu, zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="254e5-200">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="254e5-201">Jako alternativu nebude muset znovu vytvořit entitu oddělení, pokud se zobrazí pouze se chybová zpráva bez opakované zobrazování pole oddělení.</span><span class="sxs-lookup"><span data-stu-id="254e5-201">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="254e5-202">Zobrazení ukládá původní `RowVersion` obdrží tuto hodnotu v hodnotě ve skrytém poli a tato metoda `rowVersion` parametr.</span><span class="sxs-lookup"><span data-stu-id="254e5-202">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="254e5-203">Před voláním `SaveChanges`, budete muset uvést, původní `RowVersion` hodnoty vlastností v `OriginalValues` kolekce pro entitu.</span><span class="sxs-lookup"><span data-stu-id="254e5-203">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="254e5-204">Pak když rozhraní Entity Framework vytvoří příkaz SQL aktualizace, tento příkaz bude obsahovat klauzuli WHERE, která vypadá pro řádek, který má původní `RowVersion` hodnota.</span><span class="sxs-lookup"><span data-stu-id="254e5-204">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="254e5-205">Pokud se příkaz UPDATE žádné řádky (žádné řádky mají původní `RowVersion` hodnotu), vyvolá rozhraní Entity Framework `DbUpdateConcurrencyException` výjimka.</span><span class="sxs-lookup"><span data-stu-id="254e5-205">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="254e5-206">Kód v bloku catch pro této výjimky získá ovlivněných oddělení entita, která má aktualizovanými hodnotami z `Entries` vlastnost na objekt výjimky.</span><span class="sxs-lookup"><span data-stu-id="254e5-206">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

<span data-ttu-id="254e5-207">`Entries` Kolekce budou mít pouze jeden `EntityEntry` objektu.</span><span class="sxs-lookup"><span data-stu-id="254e5-207">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="254e5-208">Tento objekt můžete získat nové hodnoty zadané uživatelem a hodnot v aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="254e5-208">You can use that object to get the new values entered by the user and the current database values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

<span data-ttu-id="254e5-209">Kód přidá vlastní chybové zprávy pro každý sloupec, který má jinou hodnot v databázi z jaké zadané uživatelem na úpravy stránky (pouze jedno pole je tady zobrazené jako stručný výtah).</span><span class="sxs-lookup"><span data-stu-id="254e5-209">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

<span data-ttu-id="254e5-210">Nakonec kód nastaví `RowVersion` hodnotu `departmentToUpdate` na novou hodnotu načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="254e5-210">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="254e5-211">Tento nový `RowVersion` hodnota bude uložena ve skrytém poli, když úpravy stránka se zobrazí znovu a další čas uživatel klikne na **Uložit**, pouze souběžného zpracování chyb, které dojít, protože vzniká, redisplay upravit stránky.</span><span class="sxs-lookup"><span data-stu-id="254e5-211">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

<span data-ttu-id="254e5-212">`ModelState.Remove` Příkaz není nutná, protože `ModelState` má starý `RowVersion` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="254e5-212">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="254e5-213">V zobrazení `ModelState` hodnota pole má přednost před hodnoty vlastností modelu, pokud obě existuje.</span><span class="sxs-lookup"><span data-stu-id="254e5-213">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-department-edit-view"></a><span data-ttu-id="254e5-214">Aktualizace zobrazení upravit oddělení</span><span class="sxs-lookup"><span data-stu-id="254e5-214">Update the Department Edit view</span></span>

<span data-ttu-id="254e5-215">V *Views/Departments/Edit.cshtml*, proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="254e5-215">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="254e5-216">Přidání skrytá pole Uložit `RowVersion` hodnotu vlastnosti, hned za skryté pole pro `DepartmentID` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="254e5-216">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="254e5-217">Přidejte do seznamu rozevíracího seznamu možnost "Vyberte Administrator".</span><span class="sxs-lookup"><span data-stu-id="254e5-217">Add a "Select Administrator" option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a><span data-ttu-id="254e5-218">Test souběžnosti konfliktů na stránce Upravit</span><span class="sxs-lookup"><span data-stu-id="254e5-218">Test concurrency conflicts in the Edit page</span></span>

<span data-ttu-id="254e5-219">Spusťte aplikaci a přejděte na stránku oddělení Index.</span><span class="sxs-lookup"><span data-stu-id="254e5-219">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="254e5-220">Klikněte pravým tlačítkem myši **upravit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít na nové kartě**, klikněte **upravit** hypertextový odkaz pro angličtinu oddělení.</span><span class="sxs-lookup"><span data-stu-id="254e5-220">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="254e5-221">Karty dvě prohlížeče zobrazuje teď stejné informace.</span><span class="sxs-lookup"><span data-stu-id="254e5-221">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="254e5-222">Změňte pole na první kartě prohlížeče a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="254e5-222">Change a field in the first browser tab and click **Save**.</span></span>

![Upravit oddělení stránka 1 po změně](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="254e5-224">Prohlížeč zobrazí stránku Index s změněné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="254e5-224">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="254e5-225">Změňte pole v druhé kartu prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="254e5-225">Change a field in the second browser tab.</span></span>

![Upravit oddělení stránka 2 po změně](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="254e5-227">Klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="254e5-227">Click **Save**.</span></span> <span data-ttu-id="254e5-228">Zobrazí chybovou zprávu:</span><span class="sxs-lookup"><span data-stu-id="254e5-228">You see an error message:</span></span>

![Oddělení upravit stránku chybová zpráva](concurrency/_static/edit-error.png)

<span data-ttu-id="254e5-230">Klikněte na tlačítko **Uložit** znovu.</span><span class="sxs-lookup"><span data-stu-id="254e5-230">Click **Save** again.</span></span> <span data-ttu-id="254e5-231">Hodnota, kterou jste zadali na kartě druhý prohlížeče je uložit.</span><span class="sxs-lookup"><span data-stu-id="254e5-231">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="254e5-232">Zobrazí uložené hodnoty, když se zobrazí stránka indexu.</span><span class="sxs-lookup"><span data-stu-id="254e5-232">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="254e5-233">Odstranit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="254e5-233">Update the Delete page</span></span>

<span data-ttu-id="254e5-234">Rozhraní Entity Framework pro stránku odstranit zjistí souběžnosti konflikty způsobené někdo jinak úpravy oddělení podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="254e5-234">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="254e5-235">Když třídy MetadataExchangeClientMode `Delete` metoda zobrazí potvrzení zobrazení, zobrazení zahrnuje původní `RowVersion` hodnota ve skrytém poli.</span><span class="sxs-lookup"><span data-stu-id="254e5-235">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="254e5-236">Hodnota je pak možné HttpPost `Delete` metoda, která je volána, když uživatel potvrdí odstranění.</span><span class="sxs-lookup"><span data-stu-id="254e5-236">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="254e5-237">Rozhraní Entity Framework vytvoří příkaz SQL odstranit, obsahuje klauzuli WHERE s původní `RowVersion` hodnota.</span><span class="sxs-lookup"><span data-stu-id="254e5-237">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="254e5-238">Pokud výsledky příkazu v nulový počet řádků (tj. řádek byl změněn, jakmile se zobrazí stránka potvrzení odstranění), je vyvolána výjimka souběžnosti a třídy MetadataExchangeClientMode `Delete` metoda je volána k chybě příznak nastaven na hodnotu true, chcete-li znovu zobrazit potvrzovací stránku s chybovou zprávou.</span><span class="sxs-lookup"><span data-stu-id="254e5-238">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="254e5-239">Je také možné, že byly ovlivňuje nulový počet řádků, protože řádek byl odstraněn jiným uživatelem, tak v takovém případě se zobrazí žádná chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="254e5-239">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="254e5-240">Aktualizace metod odstranit v kontroleru oddělení</span><span class="sxs-lookup"><span data-stu-id="254e5-240">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="254e5-241">V *DepartmentsController.cs*, nahraďte třídy MetadataExchangeClientMode `Delete` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="254e5-241">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

<span data-ttu-id="254e5-242">Metodu je možné zadat volitelný parametr, který označuje, zda stránky se se zobrazí znovu po chybě souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-242">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="254e5-243">Pokud tento příznak má hodnotu true a oddělení zadaný už existuje, byla odstraněna jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="254e5-243">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="254e5-244">V takovém případě kód přesměruje na indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="254e5-244">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="254e5-245">Pokud tento příznak má hodnotu true a oddělení neexistuje, bylo změněno jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="254e5-245">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="254e5-246">V takovém případě kód odešle chybovou zprávu pomocí zobrazení `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="254e5-246">In that case, the code sends an error message to the view using `ViewData`.</span></span>  

<span data-ttu-id="254e5-247">Nahraďte kód v HttpPost `Delete` – metoda (s názvem `DeleteConfirmed`) s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="254e5-247">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

<span data-ttu-id="254e5-248">V automaticky generovaný kód, který právě nahrazen tato metoda povoleny pouze ID záznamu:</span><span class="sxs-lookup"><span data-stu-id="254e5-248">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="254e5-249">Změnili jste tento parametr k oddělení instanci entity vytvořené vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="254e5-249">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="254e5-250">To umožňuje přístup EF hodnota vlastnosti RowVersion kromě klíč záznamu.</span><span class="sxs-lookup"><span data-stu-id="254e5-250">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="254e5-251">Také jste změnili název metody akce z `DeleteConfirmed` k `Delete`.</span><span class="sxs-lookup"><span data-stu-id="254e5-251">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="254e5-252">Automaticky generovaný kód používá název `DeleteConfirmed` umožnit metoda HttpPost jedinečný podpis.</span><span class="sxs-lookup"><span data-stu-id="254e5-252">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="254e5-253">(Modulu CLR vyžaduje přetížené metody, mít parametry jinou metodu.) Teď, když podpisy jsou jedinečné, můžete přilepit s konvence MVC a použít pro odstranění metody HttpPost a třídy MetadataExchangeClientMode stejný název.</span><span class="sxs-lookup"><span data-stu-id="254e5-253">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="254e5-254">Pokud oddělení byl již odstraněn, `AnyAsync` metoda vrátí hodnotu false a aplikace právě přejde zpět na metodu Index.</span><span class="sxs-lookup"><span data-stu-id="254e5-254">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="254e5-255">Pokud je chyba souběžnosti zachycena, kód znovu zobrazí stránka potvrzení odstranění a poskytuje příznak, které označují, že by měl zobrazit chybovou zprávu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-255">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="254e5-256">Aktualizace zobrazení odstranění</span><span class="sxs-lookup"><span data-stu-id="254e5-256">Update the Delete view</span></span>

<span data-ttu-id="254e5-257">V *Views/Departments/Delete.cshtml*, nahraďte následující kód, který přidá na pole zpráva Chyba a skrytá pole vlastností DepartmentID a RowVersion automaticky generovaný kód.</span><span class="sxs-lookup"><span data-stu-id="254e5-257">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="254e5-258">Změny se zvýrazněnou.</span><span class="sxs-lookup"><span data-stu-id="254e5-258">The changes are highlighted.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

<span data-ttu-id="254e5-259">Díky následující změny:</span><span class="sxs-lookup"><span data-stu-id="254e5-259">This makes the following changes:</span></span>

* <span data-ttu-id="254e5-260">Přidá chybovou zprávu mezi `h2` a `h3` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="254e5-260">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="254e5-261">Nahradí FullName v FirstMidName **správce** pole.</span><span class="sxs-lookup"><span data-stu-id="254e5-261">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="254e5-262">Odebere pole RowVersion.</span><span class="sxs-lookup"><span data-stu-id="254e5-262">Removes the RowVersion field.</span></span>

* <span data-ttu-id="254e5-263">Přidá skryté pole pro `RowVersion` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="254e5-263">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="254e5-264">Spusťte aplikaci a přejděte na stránku oddělení Index.</span><span class="sxs-lookup"><span data-stu-id="254e5-264">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="254e5-265">Klikněte pravým tlačítkem myši **odstranit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít na nové kartě**, na první kartě klikněte na **upravit** hypertextový odkaz pro angličtinu oddělení.</span><span class="sxs-lookup"><span data-stu-id="254e5-265">Right-click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="254e5-266">V prvním okně, změňte jednu z hodnot a klikněte na tlačítko **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="254e5-266">In the first window, change one of the values, and click **Save**:</span></span>

![Stránka Upravit oddělení po změně před odstranění](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="254e5-268">Na kartě druhý klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="254e5-268">In the second tab, click **Delete**.</span></span> <span data-ttu-id="254e5-269">Zobrazí se zpráva Chyba souběžnosti a hodnoty oddělení aktualizovaly s co je aktuálně v databázi.</span><span class="sxs-lookup"><span data-stu-id="254e5-269">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Stránka potvrzení odstranění oddělení s chybou souběžnosti](concurrency/_static/delete-error.png)

<span data-ttu-id="254e5-271">Pokud kliknete na tlačítko **odstranit** znovu, budete přesměrováni na indexovou stránku, která ukazuje, že byla odstraněna z oddělení.</span><span class="sxs-lookup"><span data-stu-id="254e5-271">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="254e5-272">Podrobné informace o aktualizaci a vytvořit zobrazení</span><span class="sxs-lookup"><span data-stu-id="254e5-272">Update Details and Create views</span></span>

<span data-ttu-id="254e5-273">Můžete volitelně vyčistit automaticky generovaný kód v podrobnostech a vytvořit zobrazení.</span><span class="sxs-lookup"><span data-stu-id="254e5-273">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="254e5-274">Nahraďte kód v *Views/Departments/Details.cshtml* odstranění RowVersion sloupce a zobrazit úplný název tohoto správce.</span><span class="sxs-lookup"><span data-stu-id="254e5-274">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

<span data-ttu-id="254e5-275">Nahraďte kód v *Views/Departments/Create.cshtml* pro přidání do rozevíracího seznamu vyberte možnost.</span><span class="sxs-lookup"><span data-stu-id="254e5-275">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a><span data-ttu-id="254e5-276">Souhrn</span><span class="sxs-lookup"><span data-stu-id="254e5-276">Summary</span></span>

<span data-ttu-id="254e5-277">Tím dokončíte Úvod pro zpracování konfliktů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="254e5-277">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="254e5-278">Další informace o způsobu zpracování souběžnost v EF jádra najdete v tématu [konfliktů souběžnosti](https://docs.microsoft.com/ef/core/saving/concurrency).</span><span class="sxs-lookup"><span data-stu-id="254e5-278">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](https://docs.microsoft.com/ef/core/saving/concurrency).</span></span> <span data-ttu-id="254e5-279">Další kurz ukazuje, jak implementovat tabulky za hierarchie dědičnosti pro lektorem a Student entity.</span><span class="sxs-lookup"><span data-stu-id="254e5-279">The next tutorial shows how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="254e5-280">[Předchozí](update-related-data.md)
[další](inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="254e5-280">[Previous](update-related-data.md)
[Next](inheritance.md)</span></span>  