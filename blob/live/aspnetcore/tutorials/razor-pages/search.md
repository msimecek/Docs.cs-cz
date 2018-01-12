---
title: "Přidání vyhledávání na stránky Razor jádro ASP.NET"
author: rick-anderson
description: "Ukazuje, jak přidat vyhledávání na stránky ASP.NET Core Razor"
keywords: "ASP.NET Core, hledání, stránky Razor"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/search
ms.openlocfilehash: 3729f351ba7d1e5f566046a619c17e9e1e6614cb
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2018
---
# <a name="adding-search-to-a-razor-pages-app"></a><span data-ttu-id="01126-104">Přidání hledání do aplikace pro stránky Razor</span><span class="sxs-lookup"><span data-stu-id="01126-104">Adding search to a Razor Pages app</span></span>

<span data-ttu-id="01126-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="01126-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="01126-106">V tomto dokumentu je přidána vyhledávací funkci na indexovou stránku, která umožňuje vyhledávání filmy podle *genre* nebo *název*.</span><span class="sxs-lookup"><span data-stu-id="01126-106">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="01126-107">Aktualizovat indexovou stránku `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="01126-107">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="01126-108">První řádek `OnGetAsync` metoda vytvoří [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) dotazu a vyberte filmy:</span><span class="sxs-lookup"><span data-stu-id="01126-108">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="01126-109">Dotaz je *pouze* definované v tomto okamžiku, má **není** byly spuštěny s databází.</span><span class="sxs-lookup"><span data-stu-id="01126-109">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="01126-110">Pokud `searchString` parametr obsahuje řetězec, filmy dotazu je změněno na filtrování na řetězec pro hledání:</span><span class="sxs-lookup"><span data-stu-id="01126-110">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="01126-111">`s => s.Title.Contains()` Kód [výrazu Lambda](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="01126-111">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="01126-112">Lambdas se používají v na základě metod [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) dotazuje jako argumenty pro standardní dotaz operátor metody, jako [kde](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metoda nebo `Contains` (používá se v předchozí kód).</span><span class="sxs-lookup"><span data-stu-id="01126-112">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="01126-113">Dotazy LINQ nebudou provedeny, když jsou definovány nebo když jsou upraveny voláním metody (například `Where`, `Contains` nebo `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="01126-113">LINQ queries are not executed when they are defined or when they are modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="01126-114">Místo toho je odložen spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="01126-114">Rather, query execution is deferred.</span></span> <span data-ttu-id="01126-115">To znamená, že je zpožděno vyhodnocení výrazu, dokud jeho zjištěné hodnota je vstupní přes nebo `ToListAsync` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="01126-115">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="01126-116">V tématu [provádění dotazu](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) Další informace.</span><span class="sxs-lookup"><span data-stu-id="01126-116">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="01126-117">**Poznámka:** [obsahuje](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda běží v databázi, není v kódu jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="01126-117">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="01126-118">Rozlišování velkých a malých písmen na dotaz závisí na databázi a kolace.</span><span class="sxs-lookup"><span data-stu-id="01126-118">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="01126-119">Na serveru SQL Server `Contains` mapuje [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), což je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="01126-119">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="01126-120">V SQLite s výchozí kolace, je velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="01126-120">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="01126-121">Přejděte na stránku filmy a připojte řetězec dotazu, jako `?searchString=Ghost` na adresu URL (například `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="01126-121">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="01126-122">Filtrované filmy jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="01126-122">The filtered movies are displayed.</span></span>

![Zobrazení indexu](search/_static/ghost.png)

<span data-ttu-id="01126-124">Pokud na indexovou stránku se přidá následující šablonu trasy, řetězec pro hledání lze předat jako segment adresy URL (například `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="01126-124">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="01126-125">Předchozí omezení trasy umožňuje hledání názvu jako data trasy (segment adresy URL) místo jako hodnotu řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="01126-125">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="01126-126">`?` v `"{searchString?}"` znamená to je parametr volitelný trasy.</span><span class="sxs-lookup"><span data-stu-id="01126-126">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Index zobrazení s neodstraněných Wordu přidat adresu Url a vrácený film seznam dvou filmy, Ghostbusters a Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="01126-128">Nelze však budou uživatelé chcete upravit adresu URL pro vyhledání film.</span><span class="sxs-lookup"><span data-stu-id="01126-128">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="01126-129">V tomto kroku se přidá uživatelského rozhraní pro filtrování filmy.</span><span class="sxs-lookup"><span data-stu-id="01126-129">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="01126-130">Pokud jste přidali dané omezení trasy `"{searchString?}"`, odeberte ji.</span><span class="sxs-lookup"><span data-stu-id="01126-130">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="01126-131">Otevřete *Pages/Movies/Index.cshtml* souboru a přidejte `<form>` značek zvýrazněných v následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="01126-131">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="01126-132">HTML `<form>` značku používá [pomocná značku formuláře](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="01126-132">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="01126-133">Při odeslání formuláře je odeslán řetězec filtru *stránkách nebo filmy nebo Index* stránky.</span><span class="sxs-lookup"><span data-stu-id="01126-133">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="01126-134">Uložte změny a testovat filtr.</span><span class="sxs-lookup"><span data-stu-id="01126-134">Save the changes and test the filter.</span></span>

![Index zobrazení s neodstraněných word zadali do textového pole Název filtru](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="01126-136">Hledat podle genre</span><span class="sxs-lookup"><span data-stu-id="01126-136">Search by genre</span></span>

<span data-ttu-id="01126-137">Přidat na následující zvýrazněnou vlastnosti, které chcete *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="01126-137">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]

<span data-ttu-id="01126-138">`SelectList Genres` Obsahuje seznam žánry.</span><span class="sxs-lookup"><span data-stu-id="01126-138">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="01126-139">To umožňuje uživateli vybrat genre ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="01126-139">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="01126-140">`MovieGenre` Vlastnost obsahuje konkrétní genre vybere uživatele (například "západní").</span><span class="sxs-lookup"><span data-stu-id="01126-140">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="01126-141">Aktualizace `OnGetAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="01126-141">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="01126-142">Následující kód je dotaz LINQ, který načte všechny žánry z databáze.</span><span class="sxs-lookup"><span data-stu-id="01126-142">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="01126-143">`SelectList` z žánry vytvoří projekce odlišné žánry.</span><span class="sxs-lookup"><span data-stu-id="01126-143">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There is no start line.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="01126-144">Přidání vyhledávání podle genre</span><span class="sxs-lookup"><span data-stu-id="01126-144">Adding search by genre</span></span>

<span data-ttu-id="01126-145">Aktualizace *Index.cshtml* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="01126-145">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="01126-146">Testování aplikací tak, že genre, název filmu a oběma.</span><span class="sxs-lookup"><span data-stu-id="01126-146">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="01126-147">[Předchozí: Aktualizace stránky](xref:tutorials/razor-pages/da1)
[Další: Přidání nové pole](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="01126-147">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>