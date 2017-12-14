---
title: "Stránky Razor EF základní - CRUD - 2 8"
author: rick-anderson
description: "Ukazuje, jak vytvořit, číst, aktualizovat, odstraňovat s EF jádra"
keywords: "ASP.NET Core, Entity Framework Core, CRUD, vytvářet, číst, aktualizovat, odstranit"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 410829608540697ac4563f1399c8e72d28a13cf2
ms.sourcegitcommit: 703593d5fd14076e79be2ba75a5b8da12a60ab15
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="ebfe6-104">Vytvářet, číst, aktualizovat a odstraňovat – základní EF s stránky Razor (2 8)</span><span class="sxs-lookup"><span data-stu-id="ebfe6-104">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="ebfe6-105">Podle [tní Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ebfe6-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="ebfe6-106">V tomto kurzu vygenerované CRUD (vytvořit, číst, aktualizovat, odstraňovat) kód je zkontrolovat a upravit.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="ebfe6-107">Poznámka: Minimalizovat složitost a zachovat tyto kurzy zaměřené na základní EF, EF základní kód se používá v souborech kódu stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-107">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages code-behind files.</span></span> <span data-ttu-id="ebfe6-108">Někteří vývojáři použít k vytvoření abstraktní vrstvu mezi uživatelského rozhraní (Razor stránky) a vrstva přístupu k datům služby vrstvy nebo úložiště vzoru v.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="ebfe6-109">V tomto kurzu, vytvořit, upravit, odstranit a stránky Razor podrobnosti v *Student* složky jsou upraveny.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="ebfe6-110">Automaticky generovaný kód používá následující vzor pro Create, Edit a Delete stránky:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="ebfe6-111">Získání a zobrazit požadovaná data pomocí metody GET protokolu HTTP `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="ebfe6-112">Uložit změny do data s metodou HTTP POST `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="ebfe6-113">Stránky indexu a podrobnosti o získání a jejich zobrazení požadovaná data pomocí metody GET protokolu HTTP`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="ebfe6-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="ebfe6-114">Nahraďte SingleOrDefaultAsync FirstOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="ebfe6-114">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="ebfe6-115">Generovaný kód používá [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) načíst požadovaná entita.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-115">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="ebfe6-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) je efektivnější v načítání jedna entita:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="ebfe6-117">Pokud kód je potřeba ověřit, zda není více než jedna entita vrácená z dotazu.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-117">Unless the code needs to verify that there is not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="ebfe6-118">`SingleOrDefaultAsync`načte více dat a nepotřebné funguje.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="ebfe6-119">`SingleOrDefaultAsync`vyvolá výjimku, pokud existuje více než jedna entita, která odpovídá část filtru.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-119">`SingleOrDefaultAsync` throws an exception if there is more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="ebfe6-120">`FirstOrDefaultAsync`není výjimku, pokud existuje více než jedna entita, která odpovídá část filtru.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-120">`FirstOrDefaultAsync` doesn't throw if there is more than one entity that fits the filter part.</span></span>

<span data-ttu-id="ebfe6-121">Globálně nahradit `SingleOrDefaultAsync` s `FirstOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-121">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="ebfe6-122">`SingleOrDefaultAsync`se používá na 5 místech:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-122">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="ebfe6-123">`OnGetAsync`na stránce Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-123">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="ebfe6-124">`OnGetAsync`a `OnPostAsync` na stránkách upravit a odstranit.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-124">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="ebfe6-125">Asynchronně vyhledá</span><span class="sxs-lookup"><span data-stu-id="ebfe6-125">FindAsync</span></span>

<span data-ttu-id="ebfe6-126">Hodně automaticky generovaný kód [asynchronně vyhledá](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) lze místě `FirstOrDefaultAsync` nebo `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-126">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="ebfe6-127">`FindAsync`:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-127">`FindAsync`:</span></span>

* <span data-ttu-id="ebfe6-128">Vyhledá entitu s primární klíč (Primárníklíč).</span><span class="sxs-lookup"><span data-stu-id="ebfe6-128">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="ebfe6-129">Pokud entity s primárnímu Klíči je sledován pomocí kontextu, je vrácen bez požadavek do databáze.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-129">If an entity with the PK is being tracked by the context, it is returned without a request to the DB.</span></span>
* <span data-ttu-id="ebfe6-130">Je jednoduchý a stručné sdělení.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-130">Is simple and concise.</span></span>
* <span data-ttu-id="ebfe6-131">Je optimalizován pro vyhledání jedné entity.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-131">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="ebfe6-132">Můžete mít výhody výkonu v některých situacích, ale zřídka se do play pro scénáře webového normální.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-132">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="ebfe6-133">Implicitně používá [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) místo [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="ebfe6-133">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="ebfe6-134">Ale pokud chcete zahrnout ostatní entity, pak najít již není vhodné.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-134">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="ebfe6-135">To znamená, že budete muset abandon najít a přesunout do dotazu průběhu vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-135">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="ebfe6-136">Stránce s podrobnostmi o přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="ebfe6-136">Customize the Details page</span></span>

<span data-ttu-id="ebfe6-137">Přejděte do `Pages/Students` stránky.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-137">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="ebfe6-138">**Upravit**, **podrobnosti**, a **odstranit** generované odkazy [pomocná značka ukotvení](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) v *stránek/studenty / Index.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-138">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="ebfe6-139">Vyberte podrobnosti odkaz.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-139">Select a Details link.</span></span> <span data-ttu-id="ebfe6-140">Adresa URL je ve formátu `http://localhost:5000/Students/Details?id=2`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-140">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="ebfe6-141">Student ID je předán pomocí řetězce dotazu (`?id=2`).</span><span class="sxs-lookup"><span data-stu-id="ebfe6-141">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="ebfe6-142">Aktualizovat úpravy, podrobnosti a odstranit stránky Razor k použití `"{id:int}"` šablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-142">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="ebfe6-143">Změňte direktivu stránky pro každou tyto stránek z `@page` k `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-143">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="ebfe6-144">Požadavek na stránku s šablonou cesty "{id: int}", která nemá **není** zahrnují celé číslo trasy hodnota vrátí protokolu HTTP (nenalezen) chybu 404.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-144">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="ebfe6-145">Například `http://localhost:5000/Students/Details` vrátí chybu 404.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-145">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="ebfe6-146">Chcete-li nastavit ID volitelný, připojte `?` pro dané omezení trasy:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-146">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="ebfe6-147">Spusťte aplikaci, klikněte na odkaz podrobnosti a ověřte adresu URL je předávání ID jako data trasy (`http://localhost:5000/Students/Details/2`).</span><span class="sxs-lookup"><span data-stu-id="ebfe6-147">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="ebfe6-148">Neměnit globálně `@page` k `@page "{id:int}"`, provádění Ano zalomení odkazy na domovský uzel a vytvořte stránky.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-148">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="ebfe6-149">Přidání souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="ebfe6-149">Add related data</span></span>

<span data-ttu-id="ebfe6-150">Automaticky generovaný kód pro studenty indexovou stránku nezahrnuje `Enrollments` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-150">The scaffolded code for the Students Index page does not include the `Enrollments` property.</span></span> <span data-ttu-id="ebfe6-151">V této části, obsah `Enrollments` kolekce se zobrazí na stránce Podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-151">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="ebfe6-152">`OnGetAsync` Metodu *Pages/Students/Details.cshtml.cs* používá `FirstOrDefaultAsync` metoda pro načtení jedné `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-152">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="ebfe6-153">Přidejte následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-153">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="ebfe6-154">`Include` a `ThenInclude` metody způsobit kontext k načtení `Student.Enrollments` navigační vlastnost a v rámci každé registrace `Enrollment.Course` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-154">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="ebfe6-155">Tyto metody jsou examinied podrobně v tomto kurzu data související s čtení.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-155">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="ebfe6-156">`AsNoTracking` Metoda zvyšuje výkon v situacích, když entity vrátil se neaktualizují v aktuálním kontextu.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-156">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="ebfe6-157">`AsNoTracking`je popsána dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-157">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="ebfe6-158">Zobrazit související registrace na stránce podrobností</span><span class="sxs-lookup"><span data-stu-id="ebfe6-158">Display related enrollments on the Details page</span></span>

<span data-ttu-id="ebfe6-159">Otevřete *Pages/Students/Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-159">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="ebfe6-160">Přidejte následující zvýrazněný kód zobrazíte seznam registrace:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-160">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=35-53)]

<span data-ttu-id="ebfe6-161">Pokud odsazení kód je nesprávný po se vloží kód, stiskněte CTRL-tisíc-D opravte ho.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="ebfe6-162">Předchozí kód prochází entity v `Enrollments` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="ebfe6-163">Pro každou registraci zobrazí název kurzu a úrovni.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="ebfe6-164">Název postupu načten z kurzu entita, která je uložena v `Course` navigační vlastnost entity registrace.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="ebfe6-165">Spuštění aplikace, vyberte **studenty** a klikněte **podrobnosti** odkaz pro student.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="ebfe6-166">Zobrazí se seznam kurzů a tříd pro vybrané student.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="ebfe6-167">Vytvořit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="ebfe6-167">Update the Create page</span></span>

<span data-ttu-id="ebfe6-168">Aktualizace `OnPostAsync` metoda v *Pages/Students/Create.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="ebfe6-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="ebfe6-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="ebfe6-170">Zkontrolujte [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kódu:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-170">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="ebfe6-171">V předchozí kód `TryUpdateModelAsync<Student>` pokusit o aktualizaci `emptyStudent` objektu z hodnot odeslaného formuláře [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) vlastnost v [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="ebfe6-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="ebfe6-172">`TryUpdateModelAsync`pouze aktualizace vlastností uvedených (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span><span class="sxs-lookup"><span data-stu-id="ebfe6-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="ebfe6-173">V předchozím příkladu:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-173">In the preceding sample:</span></span>

* <span data-ttu-id="ebfe6-174">Druhý argument (` "student", // Prefix`) je předponu používá pro vyhledávání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="ebfe6-175">Není velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-175">It's not case sensitive.</span></span>
* <span data-ttu-id="ebfe6-176">Hodnoty odeslaného formuláře jsou převedeny na typy v `Student` model pomocí [model vazby](xref:mvc/models/model-binding#how-model-binding-works).</span><span class="sxs-lookup"><span data-stu-id="ebfe6-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="ebfe6-177">Overposting</span><span class="sxs-lookup"><span data-stu-id="ebfe6-177">Overposting</span></span>

<span data-ttu-id="ebfe6-178">Pomocí `TryUpdateModel` aktualizovat pole odeslaných hodnot je nejlepším postupem zabezpečení, protože zabraňuje overposting.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="ebfe6-179">Předpokládejme například, zahrnuje Student entity `Secret` vlastnost, která tato webová stránka nesmí aktualizovat nebo přidat:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-179">For example, suppose the Student entity includes a `Secret` property that this web page should not update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="ebfe6-180">I v případě, že aplikace nemá `Secret` pole na vytvoření nebo aktualizaci stránky Razor, se hacker může nastavené `Secret` hodnoty overposting.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="ebfe6-181">Hackerům může použijte například nástroj Fiddler, nebo si napsat určitého kódu JavaScript ke zveřejnění `Secret` formuláři hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="ebfe6-182">Původní kód není omezit pole, která při vytváření Student instance používá vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="ebfe6-183">Ať hacker zadaný pro hodnotu `Secret` pole formuláře je aktualizována v databázi.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="ebfe6-184">Následující obrázek ukazuje přidání nástroj Fiddler `Secret` pole (s hodnotou "OverPost") na hodnoty odeslaného formuláře.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Přidání tajný pole Fiddler](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="ebfe6-186">Hodnota "OverPost" úspěšně přidán do `Secret` vlastnost vloženého řádku.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="ebfe6-187">Návrhář aplikace nikdy určená `Secret` vlastnost na stránce vytvořit nastavit.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="ebfe6-188">Model zobrazení</span><span class="sxs-lookup"><span data-stu-id="ebfe6-188">View model</span></span>

<span data-ttu-id="ebfe6-189">Model zobrazení obvykle obsahuje podmnožinu vlastností obsažených v modelu používá aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="ebfe6-190">Aplikační model se často nazývá modelu domény.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-190">The application model is often called the domain model.</span></span> <span data-ttu-id="ebfe6-191">Model domény obvykle obsahuje všechny vlastnosti požadované odpovídající entita v databázi.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="ebfe6-192">Model zobrazení obsahuje pouze vlastnosti, které jsou nutné pro vrstvu uživatelského rozhraní (například stránka vytvořit).</span><span class="sxs-lookup"><span data-stu-id="ebfe6-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="ebfe6-193">Kromě zobrazení modelu některé aplikace použít vazby modelu nebo vstupní modelu k předávání dat mezi třídou kódu stránky Razor a v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages code-behind class and the browser.</span></span> <span data-ttu-id="ebfe6-194">Vezměte v úvahu následující `Student` modelu zobrazení:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-194">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="ebfe6-195">Zobrazit modely poskytnout alternativní způsob zabránit overposting.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="ebfe6-196">Model zobrazení obsahuje pouze vlastnosti zobrazení (zobrazení) a aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="ebfe6-197">Následující kód používá `StudentVM` zobrazení modelu vytvářet nové student:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="ebfe6-198">[SetValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metoda nastaví hodnoty tohoto objektu ve čtení hodnoty z jiné [PropertyValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) objektu.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-198">The [SetValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/ dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="ebfe6-199">`SetValues`používá vlastnost s odpovídajícím názvem.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="ebfe6-200">Typ modelu zobrazení nemusí být související s typem modelu, pouze musí mít vlastnosti, které odpovídají.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="ebfe6-201">Pomocí `StudentVM` vyžaduje [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) aktualizovat, a použít `StudentVM` místo `Student`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="ebfe6-202">Na stránkách Razor `PageModel` odvozené třídy je zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="ebfe6-203">Upravit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="ebfe6-203">Update the Edit page</span></span>

<span data-ttu-id="ebfe6-204">Aktualizace souboru kódu stránky upravit:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-204">Update the Edit page code-behind file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="ebfe6-205">Změny kódu jsou podobná stránce vytvořit s několika výjimkami:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-205">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="ebfe6-206">`OnPostAsync`má volitelný `id` parametr.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-206">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="ebfe6-207">Aktuální student jsou načtena z databáze, místo vytvoření prázdné student.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-207">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="ebfe6-208">`FirstOrDefaultAsync`byla nahrazena [asynchronně vyhledá](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="ebfe6-208">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="ebfe6-209">`FindAsync`je vhodné použít při výběru entity z primární klíč.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-209">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="ebfe6-210">V tématu [asynchronně vyhledá](#FindAsync) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-210">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="ebfe6-211">Testování úpravy a vytvářet stránky</span><span class="sxs-lookup"><span data-stu-id="ebfe6-211">Test the Edit and Create pages</span></span>

<span data-ttu-id="ebfe6-212">Vytvořit a upravit několik student entit.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-212">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="ebfe6-213">Stav entity</span><span class="sxs-lookup"><span data-stu-id="ebfe6-213">Entity States</span></span>

<span data-ttu-id="ebfe6-214">Kontext databáze uchovává informace o, jestli jsou synchronizované s jejich odpovídající řádky v databázi entity v paměti.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-214">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="ebfe6-215">Informace o databázi kontext synchronizace určuje, co se stane, když `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-215">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="ebfe6-216">Například když novou entitu je předána `Add` metoda, která je nastavena do stavu entity `Added`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-216">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="ebfe6-217">Když `SaveChanges` nazývá databáze kontextu problémy příkazu INSERT jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-217">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="ebfe6-218">Entita může být v jednom z následujících stavů:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-218">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="ebfe6-219">`Added`: Entita ještě neexistuje v databázi.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-219">`Added`: The entity does not yet exist in the DB.</span></span> <span data-ttu-id="ebfe6-220">`SaveChanges` Metoda problémy příkazu INSERT.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-220">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="ebfe6-221">`Unchanged`: Žádné změny měly být uloženy s tuto entitu.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-221">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="ebfe6-222">Entita nemá tento stav, pokud je pro čtení z databáze.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-222">An entity has this status when it is read from the DB.</span></span>

* <span data-ttu-id="ebfe6-223">`Modified`: Bylo upraveno některé nebo všechny hodnoty vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-223">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="ebfe6-224">`SaveChanges` Metoda problémy příkazu UPDATE.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-224">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="ebfe6-225">`Deleted`: Entita byla označena pro odstranění.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-225">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="ebfe6-226">`SaveChanges` Metoda problémy příkazech DELETE.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-226">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="ebfe6-227">`Detached`: Entity není sledována aplikací kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-227">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="ebfe6-228">V aplikace na ploše změny stavu se obvykle nastavuje automaticky.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-228">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="ebfe6-229">Entita je pro čtení, změny a stav entity automaticky změnit tak, aby `Modified`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-229">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="ebfe6-230">Volání metody `SaveChanges` vytvoří prohlášení aktualizace SQL, který aktualizuje pouze změněné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-230">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="ebfe6-231">Ve webové aplikaci `DbContext` entity a zobrazí data je zrušen po vykreslení stránky, který čte.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-231">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="ebfe6-232">Když stránkách `OnPostAsync` metoda je volána, Přišla žádost o nový web s novou instanci třídy `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-232">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="ebfe6-233">Znovu čtení entita v tomto kontextu nové simuluje plochy zpracování.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-233">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="ebfe6-234">Odstranit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="ebfe6-234">Update the Delete page</span></span>

<span data-ttu-id="ebfe6-235">V této části je přidán kód k implementaci vlastních chybových zpráv při volání `SaveChanges` selže.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-235">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="ebfe6-236">Přidejte řetězec tak, aby obsahovala possile chybové zprávy:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-236">Add a string to contain possile error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="ebfe6-237">Nahraďte `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-237">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="ebfe6-238">Předchozí kód obsahuje volitelný parametr `saveChangesError`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-238">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="ebfe6-239">`saveChangesError`Určuje, zda byla volána metoda po selhání student objekt odstranit.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-239">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="ebfe6-240">Operace odstranění se nemusí podařit kvůli přechodným potížím se sítí.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-240">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="ebfe6-241">Přechodné chyby síťovým budou s větší pravděpodobností v cloudu.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-241">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="ebfe6-242">`saveChangesError`je hodnota false, pokud stránka odstranění `OnGetAsync` je volána z uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-242">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="ebfe6-243">Když `OnGetAsync` volá `OnPostAsync` (protože operace odstranění se nezdařila), `saveChangesError` parametr hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-243">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="ebfe6-244">Metoda odstranění stránky OnPostAsync</span><span class="sxs-lookup"><span data-stu-id="ebfe6-244">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="ebfe6-245">Nahraďte `OnPostAsync` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-245">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="ebfe6-246">Předchozí kód načte vybrané entity, pak zavolá `Remove` metodu a nastavit stav entity `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-246">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="ebfe6-247">Když `SaveChanges` nazývá SQL odstranit se vygeneruje příkaz.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-247">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="ebfe6-248">Pokud `Remove` selže:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-248">If `Remove` fails:</span></span>

* <span data-ttu-id="ebfe6-249">Je DB výjimka zachycena.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-249">The DB exception is caught.</span></span>
* <span data-ttu-id="ebfe6-250">Odstranit stránky `OnGetAsync` metoda je volána s `saveChangesError=true`.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-250">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="ebfe6-251">Aktualizace stránky Razor odstranění</span><span class="sxs-lookup"><span data-stu-id="ebfe6-251">Update the Delete Razor Page</span></span>

<span data-ttu-id="ebfe6-252">Přidejte následující zvýrazněný chybová zpráva na stránce odstranit Razor.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-252">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="ebfe6-253">Otestujte odstranit.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-253">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="ebfe6-254">Běžné chyby</span><span class="sxs-lookup"><span data-stu-id="ebfe6-254">Common errors</span></span>

<span data-ttu-id="ebfe6-255">Nefungují student domů nebo jiné odkazy:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-255">Student/Home or other links don't work:</span></span>

<span data-ttu-id="ebfe6-256">Ověřte stránky Razor obsahuje správné `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-256">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="ebfe6-257">Například by měla Student nebo Domovská stránka Razor **není** obsahovat šablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="ebfe6-257">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="ebfe6-258">Musí zahrnovat každé stránce Razor `@page` – direktiva.</span><span class="sxs-lookup"><span data-stu-id="ebfe6-258">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ebfe6-259">[Předchozí](xref:data/ef-rp/intro)
[další](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="ebfe6-259">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>