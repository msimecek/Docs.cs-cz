---
title: "Stránky Razor Entity Framework základní – tutoriál 1 8"
author: rick-anderson
description: "Ukazuje, jak vytvořit aplikaci stránky Razor pomocí Entity Framework Core"
keywords: Kurz ASP.NET Core Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: 86f9eceb5b8646e371811fa4611a4509ff652231
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="58ebf-104">Začínáme s stránky Razor a Entity Framework Core pomocí sady Visual Studio (1 8)</span><span class="sxs-lookup"><span data-stu-id="58ebf-104">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="58ebf-105">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="58ebf-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="58ebf-106">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC 2.0 základní pomocí základní Entity Framework (EF) 2.0 a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="58ebf-106">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="58ebf-107">Ukázková aplikace je web pro fiktivní vysoké školy Contoso.</span><span class="sxs-lookup"><span data-stu-id="58ebf-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="58ebf-108">Obsahuje funkce, jako je jejich příchodu student, postupu vytvoření a přiřazení lektorem.</span><span class="sxs-lookup"><span data-stu-id="58ebf-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="58ebf-109">Tato stránka je první ze série kurzů, které vysvětlují, jak k vytvoření ukázkové aplikace Contoso univerzity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="58ebf-110">Stažení nebo zobrazení dokončené aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ebf-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="58ebf-111">[Pokyny ke stahování](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="58ebf-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58ebf-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="58ebf-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

<span data-ttu-id="58ebf-113">Znalost [stránky Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="58ebf-113">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="58ebf-114">By se měla dokončit programátory [začít pracovat s stránky Razor](xref:tutorials/razor-pages/razor-pages-start) před zahájením této série.</span><span class="sxs-lookup"><span data-stu-id="58ebf-114">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="58ebf-115">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="58ebf-115">Troubleshooting</span></span>

<span data-ttu-id="58ebf-116">Pokud narazíte na problém nevyřešíte, obvykle můžete najít řešení tak, že porovnáte svůj kód [dokončit fáze](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) nebo [dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="58ebf-116">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="58ebf-117">Seznam běžných chyb a jak je vyřešit, najdete v části [části Poradce při potížích s poslední kurz v této sérii](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="58ebf-117">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="58ebf-118">Pokud se nepodařilo najít, co potřebujete existuje, můžete odeslat dotaz na StackOverflow.com pro [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) nebo [EF základní](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="58ebf-118">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="58ebf-119">Tato série kurzů staví na co se provádí v dřívější kurzy.</span><span class="sxs-lookup"><span data-stu-id="58ebf-119">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="58ebf-120">Zvažte možnost uložení kopie projektu po každém úspěšném dokončení kurzu.</span><span class="sxs-lookup"><span data-stu-id="58ebf-120">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="58ebf-121">Pokud narazíte na problémy, můžete spustit přes z předchozí kurzu místo přejdete zpět na začátku.</span><span class="sxs-lookup"><span data-stu-id="58ebf-121">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="58ebf-122">Alternativně můžete stáhnout [dokončit fáze](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) a začít od začátku pomocí fázi dokončené.</span><span class="sxs-lookup"><span data-stu-id="58ebf-122">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="58ebf-123">Contoso univerzity webové aplikace</span><span class="sxs-lookup"><span data-stu-id="58ebf-123">The Contoso University web app</span></span>

<span data-ttu-id="58ebf-124">Aplikace vytvořené v těchto kurzech je základní univerzity webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="58ebf-124">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="58ebf-125">Uživatelé mohou zobrazit a aktualizovat studenty, během a lektorem informace.</span><span class="sxs-lookup"><span data-stu-id="58ebf-125">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="58ebf-126">Zde najdete několik z obrazovky v tomto kurzu vytvořili.</span><span class="sxs-lookup"><span data-stu-id="58ebf-126">Here are a few of the screens created in the tutorial.</span></span>

![Studenti, kteří indexovou stránku](intro/_static/students-index.png)

![Stránka upravit studenty](intro/_static/student-edit.png)

<span data-ttu-id="58ebf-129">Styl uživatelského rozhraní této lokality je blízko co je generován integrované šablony.</span><span class="sxs-lookup"><span data-stu-id="58ebf-129">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="58ebf-130">Kurz zaměřuje se na základní EF s stránky Razor, uživatelské rozhraní ne.</span><span class="sxs-lookup"><span data-stu-id="58ebf-130">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="58ebf-131">Vytvoření stránky Razor webové aplikace</span><span class="sxs-lookup"><span data-stu-id="58ebf-131">Create a Razor Pages web app</span></span>

* <span data-ttu-id="58ebf-132">Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="58ebf-132">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="58ebf-133">Vytvořte novou webovou aplikaci ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58ebf-133">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="58ebf-134">Název projektu **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="58ebf-134">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="58ebf-135">Je třeba název projektu *ContosoUniversity* tak shoda s obory názvů, pokud kód je zkopírované a vložené.</span><span class="sxs-lookup"><span data-stu-id="58ebf-135">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="58ebf-136">![nové webové aplikace ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="58ebf-136">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="58ebf-137">Vyberte **technologii ASP.NET 2.0 základní** v rozevírací nabídce a potom vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="58ebf-137">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="58ebf-138">![Webové aplikace (stránky Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="58ebf-138">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="58ebf-139">Stiskněte klávesu **F5** a spusťte aplikaci v režimu ladění nebo **Ctrl + F5** běžet bez připojení ladicího programu</span><span class="sxs-lookup"><span data-stu-id="58ebf-139">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="58ebf-140">Nastavení stylu lokality</span><span class="sxs-lookup"><span data-stu-id="58ebf-140">Set up the site style</span></span>

<span data-ttu-id="58ebf-141">Několik změny nastavení lokality nabídky, rozložení a domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="58ebf-141">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="58ebf-142">Otevřete *Pages/_Layout.cshtml* a proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="58ebf-142">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="58ebf-143">Změňte všechny výskyty "ContosoUniversity" na "Contoso univerzity."</span><span class="sxs-lookup"><span data-stu-id="58ebf-143">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="58ebf-144">Existují tři události.</span><span class="sxs-lookup"><span data-stu-id="58ebf-144">There are three occurrences.</span></span>

* <span data-ttu-id="58ebf-145">Přidání položek nabídky pro **studenty**, **kurzy**, **vyučující**, a **oddělení**a odstranit **kontaktujte** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="58ebf-145">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="58ebf-146">Změny se zvýrazněnou.</span><span class="sxs-lookup"><span data-stu-id="58ebf-146">The changes are highlighted.</span></span> <span data-ttu-id="58ebf-147">(Všechny značky se *není* zobrazit.)</span><span class="sxs-lookup"><span data-stu-id="58ebf-147">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="58ebf-148">V *Pages/Index.cshtml*, nahraďte obsah souboru následující kód, který nahradí text o ASP.NET a MVC o této aplikaci:</span><span class="sxs-lookup"><span data-stu-id="58ebf-148">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="58ebf-149">Stisknutím kombinace kláves CTRL + F5 a spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="58ebf-149">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="58ebf-150">Domovská stránka se zobrazí s kartami vytvořené v následujících kurzech:</span><span class="sxs-lookup"><span data-stu-id="58ebf-150">The home page is displayed with tabs created in the following tutorials:</span></span>

![Contoso univerzity domovské stránky](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="58ebf-152">Vytvoření datového modelu</span><span class="sxs-lookup"><span data-stu-id="58ebf-152">Create the data model</span></span>

<span data-ttu-id="58ebf-153">Vytvoření třídy entity pro aplikaci univerzity Contoso.</span><span class="sxs-lookup"><span data-stu-id="58ebf-153">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="58ebf-154">Můžete začít s následující tři entity:</span><span class="sxs-lookup"><span data-stu-id="58ebf-154">Start with the following three entities:</span></span>

![Diagram modelu dat během. registrace studenty](intro/_static/data-model-diagram.png)

<span data-ttu-id="58ebf-156">Je vztah jeden mnoho mezi `Student` a `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-156">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="58ebf-157">Je vztah jeden mnoho mezi `Course` a `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-157">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="58ebf-158">Student může zaregistrovat libovolný počet kurzy.</span><span class="sxs-lookup"><span data-stu-id="58ebf-158">A student can enroll in any number of courses.</span></span> <span data-ttu-id="58ebf-159">Kurz může mít libovolný počet studenti, kteří jsou v ní zaregistrované.</span><span class="sxs-lookup"><span data-stu-id="58ebf-159">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="58ebf-160">V následujících částech se vytvoří třídu pro každé z nich o těchto entitách.</span><span class="sxs-lookup"><span data-stu-id="58ebf-160">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="58ebf-161">Student entity</span><span class="sxs-lookup"><span data-stu-id="58ebf-161">The Student entity</span></span>

![Student entity diagram](intro/_static/student-entity.png)

<span data-ttu-id="58ebf-163">Vytvoření *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="58ebf-163">Create a *Models* folder.</span></span> <span data-ttu-id="58ebf-164">V *modely* složky, vytvořte soubor třídy s názvem *Student.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="58ebf-164">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="58ebf-165">`ID` Vlastnost stane sloupec primárního klíče tabulky databáze (databáze), která odpovídá této třídy.</span><span class="sxs-lookup"><span data-stu-id="58ebf-165">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="58ebf-166">Ve výchozím nastavení, EF základní interpretuje vlastnost s názvem `ID` nebo `classnameID` jako primární klíč.</span><span class="sxs-lookup"><span data-stu-id="58ebf-166">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="58ebf-167">`Enrollments` Je navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="58ebf-167">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="58ebf-168">Navigační vlastnosti propojit s dalšími subjekty, které se vztahují k této entity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-168">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="58ebf-169">V takovém případě `Enrollments` vlastnost `Student entity` obsahuje všechny `Enrollment` entit, které se vztahují k které `Student`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-169">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="58ebf-170">Například pokud studentů řádek v databázi existují dvě související registrace řádky, `Enrollments` navigační vlastnost obsahuje tyto dvě `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-170">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="58ebf-171">Související `Enrollment` řádek je řádek, který obsahuje hodnotu primárního klíče tohoto Studentova v `StudentID` sloupce.</span><span class="sxs-lookup"><span data-stu-id="58ebf-171">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="58ebf-172">Předpokládejme například, student s ID = 1, má dva řádky v `Enrollment` tabulky.</span><span class="sxs-lookup"><span data-stu-id="58ebf-172">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="58ebf-173">`Enrollment` Tabulka má dva řádky s `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="58ebf-173">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="58ebf-174">`StudentID`cizí klíč v `Enrollment` tabulku, která určuje studenty ve `Student` tabulky.</span><span class="sxs-lookup"><span data-stu-id="58ebf-174">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="58ebf-175">Pokud navigační vlastnost může obsahovat více entit, navigační vlastnost musí být typu seznamu, jako například `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-175">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="58ebf-176">`ICollection<T>`lze zadat, nebo typu, jako `List<T>` nebo `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-176">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="58ebf-177">Když `ICollection<T>` se používá, vytvoří základní EF `HashSet<T>` kolekce ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="58ebf-177">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="58ebf-178">Navigační vlastnosti, které mají více entit pocházet z relace m: n a jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="58ebf-178">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="58ebf-179">Registrace entity</span><span class="sxs-lookup"><span data-stu-id="58ebf-179">The Enrollment entity</span></span>

![Diagram entity registrace](intro/_static/enrollment-entity.png)

<span data-ttu-id="58ebf-181">V *modely* složku vytvořit *Enrollment.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="58ebf-181">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="58ebf-182">`EnrollmentID` Vlastnost je primární klíč.</span><span class="sxs-lookup"><span data-stu-id="58ebf-182">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="58ebf-183">Používá tato entita `classnameID` vzor místo `ID` jako `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-183">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="58ebf-184">Vývojáři obvykle zvolte jeden vzor a použít ho v rámci datového modelu.</span><span class="sxs-lookup"><span data-stu-id="58ebf-184">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="58ebf-185">Novější kurzu pomocí ID bez classname se zobrazí usnadňují implementace dědičnosti v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="58ebf-185">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="58ebf-186">`Grade` Vlastnost je `enum`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-186">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="58ebf-187">Otazník po `Grade` deklaraci typu znamená, že `Grade` vlastnost umožňuje hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="58ebf-187">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="58ebf-188">Třída, která má hodnotu null se liší od nulové úrovni – null znamená úrovni není známý nebo ještě nebyly přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="58ebf-188">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="58ebf-189">`StudentID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Student`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-189">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="58ebf-190">`Enrollment` Entita je spojen s jednou `Student` entit, tak vlastnost obsahuje jediný `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-190">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="58ebf-191">`Student` Entity se liší od `Student.Enrollments` navigační vlastnost, která obsahuje více `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-191">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="58ebf-192">`CourseID` Vlastnost je cizí klíč a odpovídající navigační vlastnost `Course`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="58ebf-193">`Enrollment` Entita je spojen s jednou `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="58ebf-194">Vlastnost EF základní interpretuje jako cizí klíč, pokud je název `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-194">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="58ebf-195">Například`StudentID` pro `Student` navigační vlastnost, protože `Student` primárního klíče entity je `ID`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-195">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="58ebf-196">Vlastnosti cizího klíče může nazývat také `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-196">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="58ebf-197">Například `CourseID` vzhledem k tomu `Course` je primární klíč entity `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-197">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="58ebf-198">Během entity</span><span class="sxs-lookup"><span data-stu-id="58ebf-198">The Course entity</span></span>

![Diagram postupu entity](intro/_static/course-entity.png)

<span data-ttu-id="58ebf-200">V *modely* složku vytvořit *Course.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="58ebf-200">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="58ebf-201">`Enrollments` Je navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="58ebf-201">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="58ebf-202">A `Course` entity může souviset s libovolný počet `Enrollment` entity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-202">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="58ebf-203">`DatabaseGenerated` Místo nutnosti databáze generovat atribut umožňuje aplikacím určit primární klíč.</span><span class="sxs-lookup"><span data-stu-id="58ebf-203">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="58ebf-204">Vytvoření kontextu SchoolContext DB</span><span class="sxs-lookup"><span data-stu-id="58ebf-204">Create the SchoolContext DB context</span></span>

<span data-ttu-id="58ebf-205">Hlavní třída, která koordinuje EF základní funkce pro daný datový model je třídy kontextu databáze.</span><span class="sxs-lookup"><span data-stu-id="58ebf-205">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="58ebf-206">Data kontextu je odvozený od `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-206">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="58ebf-207">Data kontextu určuje entit, které jsou zahrnuty v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="58ebf-207">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="58ebf-208">V tomto projektu je třída s názvem `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="58ebf-209">Ve složce projektu vytvořte složku s názvem *Data*.</span><span class="sxs-lookup"><span data-stu-id="58ebf-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="58ebf-210">V *Data* vytvořit složku *SchoolContext.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="58ebf-210">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="58ebf-211">Tento kód vytvoří `DbSet` vlastnosti pro každou sadu entit.</span><span class="sxs-lookup"><span data-stu-id="58ebf-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="58ebf-212">V terminologii EF jádra:</span><span class="sxs-lookup"><span data-stu-id="58ebf-212">In EF Core terminology:</span></span>

* <span data-ttu-id="58ebf-213">Obvykle sadu entit odpovídá Databázové tabulce.</span><span class="sxs-lookup"><span data-stu-id="58ebf-213">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="58ebf-214">Entity odpovídá na řádek v tabulce.</span><span class="sxs-lookup"><span data-stu-id="58ebf-214">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="58ebf-215">`DbSet<Enrollment>`a `DbSet<Course>` lze vynechat.</span><span class="sxs-lookup"><span data-stu-id="58ebf-215">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="58ebf-216">Základní EF zahrnuje je implicitně v protože `Student` odkazy na entity `Enrollment` entity a `Enrollment` odkazy na entity `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="58ebf-216">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="58ebf-217">V tomto kurzu zachovat `DbSet<Enrollment>` a `DbSet<Course>` v `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-217">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="58ebf-218">Při vytváření databáze EF základní vytvoří tabulek, které jsou stejné jako názvy `DbSet` názvy vlastností.</span><span class="sxs-lookup"><span data-stu-id="58ebf-218">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="58ebf-219">Názvy vlastností pro kolekce jsou obvykle množném čísle (studenty spíše než Student).</span><span class="sxs-lookup"><span data-stu-id="58ebf-219">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="58ebf-220">Vývojáři Nesouhlasím o tom, jestli by měla být názvy tabulek množném čísle.</span><span class="sxs-lookup"><span data-stu-id="58ebf-220">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="58ebf-221">Pro tyto kurzy je výchozí chování přepsat zadáním názvů singulární tabulek v DbContext.</span><span class="sxs-lookup"><span data-stu-id="58ebf-221">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="58ebf-222">Pokud chcete zadat názvy singulární tabulek, přidejte následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="58ebf-222">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="58ebf-223">Zaregistrovat kontext vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="58ebf-223">Register the context with dependency injection</span></span>

<span data-ttu-id="58ebf-224">Zahrnuje ASP.NET Core [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="58ebf-224">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="58ebf-225">Služby (například kontext databáze základní EF) jsou registrovány vkládání závislostí při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="58ebf-225">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="58ebf-226">Součásti, které vyžadují tyto služby (například stránky Razor) jsou k dispozici tyto služby prostřednictvím konstruktor parametry.</span><span class="sxs-lookup"><span data-stu-id="58ebf-226">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="58ebf-227">Později v tomto kurzu se zobrazí kód konstruktor, který získá kontext instanci databáze.</span><span class="sxs-lookup"><span data-stu-id="58ebf-227">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="58ebf-228">K registraci `SchoolContext` jako služby, otevřete *Startup.cs*a přidejte zvýrazněné řádky, které se `ConfigureServices` metoda.</span><span class="sxs-lookup"><span data-stu-id="58ebf-228">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="58ebf-229">Název připojovacího řetězce, je předaná do kontextu voláním metody na `DbContextOptionsBuilder` objektu.</span><span class="sxs-lookup"><span data-stu-id="58ebf-229">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="58ebf-230">Pro místní vývoj [ASP.NET Core konfigurační systém](xref:fundamentals/configuration/index) čte připojovací řetězec z *appSettings.JSON určený* souboru.</span><span class="sxs-lookup"><span data-stu-id="58ebf-230">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="58ebf-231">Přidat `using` příkazy pro `ContosoUniversity.Data` a `Microsoft.EntityFrameworkCore` obory názvů.</span><span class="sxs-lookup"><span data-stu-id="58ebf-231">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="58ebf-232">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="58ebf-232">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="58ebf-233">Otevřete *appSettings.JSON určený* souboru a přidat připojovací řetězec, jak je znázorněno v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="58ebf-233">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="58ebf-234">Předchozí připojovací řetězec používá `ConnectRetryCount=0` aby [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) z ukotvených.</span><span class="sxs-lookup"><span data-stu-id="58ebf-234">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="58ebf-235">Databáze SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="58ebf-235">SQL Server Express LocalDB</span></span>

<span data-ttu-id="58ebf-236">Určuje připojovací řetězec databáze SQL Server LocalDB DB.</span><span class="sxs-lookup"><span data-stu-id="58ebf-236">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="58ebf-237">LocalDB je Odlehčená verze SQL Server Express Database Engine a je určen pro vývoj aplikací, není použití v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="58ebf-237">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="58ebf-238">LocalDB spustí na vyžádání a běží v uživatelském režimu, takže není žádná komplexní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="58ebf-238">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="58ebf-239">Ve výchozím nastavení, vytvoří instanci LocalDB *.mdf* DB soubory `C:/Users/<user>` adresáře.</span><span class="sxs-lookup"><span data-stu-id="58ebf-239">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="58ebf-240">Přidat kód pro inicializaci databáze s testovací data</span><span class="sxs-lookup"><span data-stu-id="58ebf-240">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="58ebf-241">Základní EF vytvoří prázdná databáze.</span><span class="sxs-lookup"><span data-stu-id="58ebf-241">EF Core creates an empty DB.</span></span> <span data-ttu-id="58ebf-242">V této části *počáteční hodnoty* metoda je zapsán do jeho naplnění testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="58ebf-242">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="58ebf-243">V *Data* složky, vytvořte nový soubor třídy s názvem *DbInitializer.cs* a přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="58ebf-243">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="58ebf-244">Kód kontroluje, zda existují jakékoli studenty v databázi.</span><span class="sxs-lookup"><span data-stu-id="58ebf-244">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="58ebf-245">Pokud nejsou žádné studenty v databázi, databázi osadit testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="58ebf-245">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="58ebf-246">Načte testovací data do pole místo `List<T>` kolekce za účelem optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="58ebf-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="58ebf-247">`EnsureCreated` Metoda automaticky vytvoří databázi pro kontext databáze.</span><span class="sxs-lookup"><span data-stu-id="58ebf-247">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="58ebf-248">Pokud existuje databáze, `EnsureCreated` vrátí beze změny databáze.</span><span class="sxs-lookup"><span data-stu-id="58ebf-248">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="58ebf-249">V *Program.cs*, změnit `Main` metoda proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="58ebf-249">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="58ebf-250">Zjištění instance kontextu DB z kontejneru pro vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="58ebf-250">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="58ebf-251">Volejte metodu počáteční hodnoty, předání kontextu.</span><span class="sxs-lookup"><span data-stu-id="58ebf-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="58ebf-252">Kontext Dispose po dokončení metodu počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="58ebf-252">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="58ebf-253">Následující kód ukazuje aktualizovaný *Program.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="58ebf-253">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="58ebf-254">Při prvním spuštění aplikace, databáze se vytvoří a nasadí se testovací data.</span><span class="sxs-lookup"><span data-stu-id="58ebf-254">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="58ebf-255">Při aktualizaci datového modelu:</span><span class="sxs-lookup"><span data-stu-id="58ebf-255">When the data model is updated:</span></span>
* <span data-ttu-id="58ebf-256">Odstranění databáze.</span><span class="sxs-lookup"><span data-stu-id="58ebf-256">Delete the DB.</span></span>
* <span data-ttu-id="58ebf-257">Aktualizujte metodu počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="58ebf-257">Update the seed method.</span></span>
* <span data-ttu-id="58ebf-258">Spusťte aplikaci a je vytvořen nový dosazená DB.</span><span class="sxs-lookup"><span data-stu-id="58ebf-258">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="58ebf-259">V dalších kurzech aktualizaci databáze při data modelu změny, bez odstranit a znovu vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="58ebf-259">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="58ebf-260">Přidat vygenerované uživatelské rozhraní nástroje</span><span class="sxs-lookup"><span data-stu-id="58ebf-260">Add scaffold tooling</span></span>

<span data-ttu-id="58ebf-261">V této části konzoly Správce balíčků (PMC) slouží k přidání balíčku generování kódu webové sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58ebf-261">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="58ebf-262">Tento balíček je potřeba spustit modul generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="58ebf-262">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="58ebf-263">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="58ebf-263">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="58ebf-264">V balíček správce konzoly (pomocí PMC), zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="58ebf-264">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="58ebf-265">Předchozí příkaz přidá do souboru \*.csproj balíčky NuGet:</span><span class="sxs-lookup"><span data-stu-id="58ebf-265">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="58ebf-266">Vygenerované uživatelské rozhraní modelu</span><span class="sxs-lookup"><span data-stu-id="58ebf-266">Scaffold the model</span></span>

* <span data-ttu-id="58ebf-267">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="58ebf-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="58ebf-268">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="58ebf-268">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="58ebf-269">Pokud je generována k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="58ebf-269">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="58ebf-270">Spusťte příkaz znovu a komentář v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="58ebf-270">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="58ebf-271">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="58ebf-271">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="58ebf-272">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="58ebf-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="58ebf-273">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="58ebf-273">Build the project.</span></span> <span data-ttu-id="58ebf-274">Sestavení generuje chyby takto:</span><span class="sxs-lookup"><span data-stu-id="58ebf-274">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="58ebf-275">Globálně změnit `_context.Student` k `_context.Students` (tedy "s" přidat do `Student`).</span><span class="sxs-lookup"><span data-stu-id="58ebf-275">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="58ebf-276">7 výskytů jsou vyhledána a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="58ebf-276">7 occurrences are found and updated.</span></span> <span data-ttu-id="58ebf-277">Doufáme, opravte [této chyby](https://github.com/aspnet/Scaffolding/issues/633) v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="58ebf-277">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="58ebf-278">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="58ebf-278">Test the app</span></span>

<span data-ttu-id="58ebf-279">Spusťte aplikaci a vyberte **studenty** odkaz.</span><span class="sxs-lookup"><span data-stu-id="58ebf-279">Run the app and select the **Students** link.</span></span> <span data-ttu-id="58ebf-280">V závislosti na šířku prohlížeče **studenty** se zobrazí v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="58ebf-280">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="58ebf-281">Pokud **studenty** odkaz se nezobrazuje, klikněte na ikonu Navigace v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="58ebf-281">If the **Students** link is not visible, click the navigation icon in the upper right corner.</span></span>

![Domovská stránka Contoso univerzity úzké](intro/_static/home-page-narrow.png)

<span data-ttu-id="58ebf-283">Testovací **vytvořit**, **upravit**, a **podrobnosti** odkazy.</span><span class="sxs-lookup"><span data-stu-id="58ebf-283">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="58ebf-284">Zobrazení databáze</span><span class="sxs-lookup"><span data-stu-id="58ebf-284">View the DB</span></span>

<span data-ttu-id="58ebf-285">Když se aplikace spustí, `DbInitializer.Initialize` volání `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-285">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="58ebf-286">`EnsureCreated`zjistí, pokud existuje databáze a v případě potřeby jej vytvoří.</span><span class="sxs-lookup"><span data-stu-id="58ebf-286">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="58ebf-287">Pokud v databázi, nejsou žádné studenty `Initialize` metoda přidá studenty.</span><span class="sxs-lookup"><span data-stu-id="58ebf-287">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="58ebf-288">Otevřete **Průzkumník objektů systému SQL Server** (SSOX) z **zobrazení** nabídky v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58ebf-288">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="58ebf-289">V SSOX, klikněte na **\MSSQLLocalDB (localdb) > databáze > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="58ebf-289">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="58ebf-290">Rozbalte **tabulky** uzlu.</span><span class="sxs-lookup"><span data-stu-id="58ebf-290">Expand the **Tables** node.</span></span>

<span data-ttu-id="58ebf-291">Klikněte pravým tlačítkem myši **Student** tabulky a klikněte na tlačítko **Data zobrazení** zobrazíte vytvořit sloupce a řádky vloženy do tabulky.</span><span class="sxs-lookup"><span data-stu-id="58ebf-291">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="58ebf-292">*.Mdf* a *.ldf* soubory databáze jsou v *C:\Users\\ <yourusername>*  složky.</span><span class="sxs-lookup"><span data-stu-id="58ebf-292">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="58ebf-293">`EnsureCreated`je volána při spuštění aplikace, která umožňuje následující pracovní postup:</span><span class="sxs-lookup"><span data-stu-id="58ebf-293">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="58ebf-294">Odstranění databáze.</span><span class="sxs-lookup"><span data-stu-id="58ebf-294">Delete the DB.</span></span>
* <span data-ttu-id="58ebf-295">Změnit schéma databáze (například přidat `EmailAddress` pole).</span><span class="sxs-lookup"><span data-stu-id="58ebf-295">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="58ebf-296">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="58ebf-296">Run the app.</span></span>

<span data-ttu-id="58ebf-297">`EnsureCreated`vytvoří databáze s`EmailAddress` sloupce.</span><span class="sxs-lookup"><span data-stu-id="58ebf-297">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="58ebf-298">Konvence</span><span class="sxs-lookup"><span data-stu-id="58ebf-298">Conventions</span></span>

<span data-ttu-id="58ebf-299">Kvůli použití konvence nebo předpoklady, které umožňuje v EF základní je minimální množství kód napsaný v pořadí pro základní EF vytvořit úplný DB.</span><span class="sxs-lookup"><span data-stu-id="58ebf-299">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="58ebf-300">Názvy `DbSet` vlastnosti jsou použity jako názvy tabulek.</span><span class="sxs-lookup"><span data-stu-id="58ebf-300">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="58ebf-301">Pro entity není odkazuje `DbSet` vlastnost třídy entita názvy jsou použity jako názvy tabulek.</span><span class="sxs-lookup"><span data-stu-id="58ebf-301">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="58ebf-302">Názvy vlastností entity se používají pro názvy sloupců.</span><span class="sxs-lookup"><span data-stu-id="58ebf-302">Entity property names are used for column names.</span></span>

* <span data-ttu-id="58ebf-303">Vlastnosti entity, které jsou s názvem ID nebo classnameID jsou rozpoznat jako vlastnosti primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="58ebf-303">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="58ebf-304">Vlastnost interpretována jako vlastností cizího klíče, pokud je název  *<navigation property name> <primary key property name>*  (například `StudentID` pro `Student` navigační vlastnost, protože `Student` je primární klíč entity `ID`).</span><span class="sxs-lookup"><span data-stu-id="58ebf-304">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="58ebf-305">Může mít název vlastnosti cizího klíče  *<primary key property name>*  (například `EnrollmentID` vzhledem k tomu `Enrollment` je primární klíč entity `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="58ebf-305">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="58ebf-306">Konvenční chování lze přepsat.</span><span class="sxs-lookup"><span data-stu-id="58ebf-306">Conventional behavior can be overridden.</span></span> <span data-ttu-id="58ebf-307">Například názvy tabulek lze explicitně zadat, jak je znázorněno v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="58ebf-307">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="58ebf-308">Názvy sloupců může být explicitně nastaveny.</span><span class="sxs-lookup"><span data-stu-id="58ebf-308">The column names can be explicitly set.</span></span> <span data-ttu-id="58ebf-309">Primární a cizí klíče může být explicitně nastaveny.</span><span class="sxs-lookup"><span data-stu-id="58ebf-309">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="58ebf-310">Asynchronní kódu</span><span class="sxs-lookup"><span data-stu-id="58ebf-310">Asynchronous code</span></span>

<span data-ttu-id="58ebf-311">Asynchronní programování je výchozí režim pro ASP.NET Core a EF jádra.</span><span class="sxs-lookup"><span data-stu-id="58ebf-311">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="58ebf-312">Webový server má omezený počet vláken, které jsou k dispozici, a v situacích, vysokého zatížení všech dostupných vláken může být používán.</span><span class="sxs-lookup"><span data-stu-id="58ebf-312">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="58ebf-313">Pokud k tomu dojde, server nemůže zpracovat nové žádosti o, dokud se uvolní vláken.</span><span class="sxs-lookup"><span data-stu-id="58ebf-313">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="58ebf-314">Synchronní kód může být vázáno mnoho vláken při jejich nejsou to skutečně veškerou práci, protože se čeká vstupně-výstupních operací na dokončení.</span><span class="sxs-lookup"><span data-stu-id="58ebf-314">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="58ebf-315">Asynchronní kód když proces čeká na vstupně-výstupních operací na dokončení, je jeho přístup z více vláken uvolněno pro server, aby používal pro zpracování dalších požadavků.</span><span class="sxs-lookup"><span data-stu-id="58ebf-315">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="58ebf-316">V důsledku toho asynchronní kódu umožňuje prostředky serveru použije efektivněji a serveru zapnutá, abyste zvládli větší provoz bez zpoždění.</span><span class="sxs-lookup"><span data-stu-id="58ebf-316">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="58ebf-317">Asynchronní kódu v době běhu zavést malé množství režijní náklady.</span><span class="sxs-lookup"><span data-stu-id="58ebf-317">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="58ebf-318">Pro situace, nízkým provozem stiskněte klávesu výkonu je nepatrné při pro intenzivní provoz situace, potenciální zlepšení výkonu je výrazně.</span><span class="sxs-lookup"><span data-stu-id="58ebf-318">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="58ebf-319">V následujícím kódu `async` – klíčové slovo, `Task<T>` vrátit hodnotu, `await` – klíčové slovo, a `ToListAsync` metoda zkontrolujte kód provést asynchronně.</span><span class="sxs-lookup"><span data-stu-id="58ebf-319">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="58ebf-320">`async` – Klíčové slovo instruuje kompilátor, aby:</span><span class="sxs-lookup"><span data-stu-id="58ebf-320">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="58ebf-321">Generovat zpětných volání pro části těla metody.</span><span class="sxs-lookup"><span data-stu-id="58ebf-321">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="58ebf-322">Automaticky vytvářet [úloh](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) objekt, který je vrácen.</span><span class="sxs-lookup"><span data-stu-id="58ebf-322">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that is returned.</span></span> <span data-ttu-id="58ebf-323">Další informace najdete v tématu [úloh návratového typu](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="58ebf-323">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="58ebf-324">Implicitní návratový typ `Task` reprezentuje probíhající práce.</span><span class="sxs-lookup"><span data-stu-id="58ebf-324">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="58ebf-325">`await` – Klíčové slovo způsobí, že kompilátor metodu rozdělit na dvě části.</span><span class="sxs-lookup"><span data-stu-id="58ebf-325">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="58ebf-326">První část končí operace, které se spustí asynchronně.</span><span class="sxs-lookup"><span data-stu-id="58ebf-326">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="58ebf-327">Druhá část je uvést do metody zpětného volání, která je volána po dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="58ebf-327">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="58ebf-328">`ToListAsync`je asynchronní verzi `ToList` metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="58ebf-328">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="58ebf-329">Třeba mít na paměti při zápisu asynchronní kód, který používá základní EF několik věcí:</span><span class="sxs-lookup"><span data-stu-id="58ebf-329">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="58ebf-330">Pouze příkazy, které způsobily dotazy nebo příkazy k odeslání do databáze se spustí asynchronně.</span><span class="sxs-lookup"><span data-stu-id="58ebf-330">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="58ebf-331">Zahrnující, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, a `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-331">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="58ebf-332">Nezahrnuje příkazy, které právě změnit `IQueryable`, jako například `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="58ebf-332">It does not include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="58ebf-333">Kontextu EF jádra není zařazování bezpečné: nemáte pokusí provést více operací paralelně.</span><span class="sxs-lookup"><span data-stu-id="58ebf-333">An EF Core context is not threaded safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="58ebf-334">Abyste mohli využívat výhod výkonu asynchronní kódu, ověřte, že knihovna balíčky (například pro stránkování) používat asynchronní, pokud volají EF základní metody, které odesílají dotazy do databáze.</span><span class="sxs-lookup"><span data-stu-id="58ebf-334">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="58ebf-335">Další informace o asynchronní programování v rozhraní .NET najdete v tématu [přehled asynchronních](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="58ebf-335">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="58ebf-336">V dalším kurzu základní CRUD (vytvořit, číst, aktualizovat, odstraňovat) byla.</span><span class="sxs-lookup"><span data-stu-id="58ebf-336">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="58ebf-337">Next</span><span class="sxs-lookup"><span data-stu-id="58ebf-337">Next</span></span>](xref:data/ef-rp/crud)