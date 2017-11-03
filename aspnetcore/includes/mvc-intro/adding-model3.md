
## <a name="test-the-app"></a><span data-ttu-id="39b43-101">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="39b43-101">Test the app</span></span>

* <span data-ttu-id="39b43-102">Spusťte aplikaci a klepněte **Mvc film** odkaz.</span><span class="sxs-lookup"><span data-stu-id="39b43-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="39b43-103">Klepněte **vytvořit nový** propojit a vytvořit film.</span><span class="sxs-lookup"><span data-stu-id="39b43-103">Tap the **Create New** link and create a movie.</span></span>

  ![Vytvoření zobrazení v polích pro genre, ceny, datum vydání a název](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="39b43-105">Nelze zadat desetinných míst nebo čárkami v `Price` pole.</span><span class="sxs-lookup"><span data-stu-id="39b43-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="39b43-106">Pro podporu [k ověřování jQuery](https://jqueryvalidation.org/) pro neanglická národní prostředí, které používají čárkou (",") pro desetinné čárky a formát data neanglických USA, musíte provést kroky globalizace aplikace.</span><span class="sxs-lookup"><span data-stu-id="39b43-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="39b43-107">V tématu https://github.com/aspnet/Docs/issues/4076 a [další prostředky](#additional-resources) Další informace.</span><span class="sxs-lookup"><span data-stu-id="39b43-107">See https://github.com/aspnet/Docs/issues/4076 and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="39b43-108">Teď zadejte jenom celá čísla, jako je 10.</span><span class="sxs-lookup"><span data-stu-id="39b43-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="39b43-109">V některých národní prostředí budete muset zadat formát data.</span><span class="sxs-lookup"><span data-stu-id="39b43-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="39b43-110">Viz následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="39b43-110">See the highlighted code below.</span></span>

<span data-ttu-id="39b43-111">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]</span><span class="sxs-lookup"><span data-stu-id="39b43-111">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]</span></span>

<span data-ttu-id="39b43-112">Budeme mluvit o `DataAnnotations` dál v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="39b43-112">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="39b43-113">Klepnutím **vytvořit** způsobí, že formulář odeslat na server, kde film informace se ukládají do databáze.</span><span class="sxs-lookup"><span data-stu-id="39b43-113">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="39b43-114">Aplikace provede přesměrování */Movies* adresu URL, kde se zobrazují informace nově vytvořený film.</span><span class="sxs-lookup"><span data-stu-id="39b43-114">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![Výpis film zobrazení zobrazující nově vytvořený filmy](../../tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="39b43-116">Vytvořte několik další film položky.</span><span class="sxs-lookup"><span data-stu-id="39b43-116">Create a couple more movie entries.</span></span> <span data-ttu-id="39b43-117">Zkuste **upravit**, **podrobnosti**, a **odstranit** odkazy, které jsou všechny funkční.</span><span class="sxs-lookup"><span data-stu-id="39b43-117">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>