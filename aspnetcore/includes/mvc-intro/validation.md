# <a name="adding-validation"></a><span data-ttu-id="bc0aa-101">Přidání ověřování</span><span class="sxs-lookup"><span data-stu-id="bc0aa-101">Adding validation</span></span>

<span data-ttu-id="bc0aa-102">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bc0aa-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bc0aa-103">V této části přidáte logiku ověření pro `Movie` modelu a budete ujistěte, že ověřovací pravidla se uplatňují vždy, když uživatel vytvoří nebo upraví film.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-103">In this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="bc0aa-104">Zachování věcí suchého</span><span class="sxs-lookup"><span data-stu-id="bc0aa-104">Keeping things DRY</span></span>

<span data-ttu-id="bc0aa-105">Jeden z návrhu principů MVC je [SUCHÝCH](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("nemáte opakujte sami").</span><span class="sxs-lookup"><span data-stu-id="bc0aa-105">One of the design tenets of MVC is [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("Don't Repeat Yourself").</span></span> <span data-ttu-id="bc0aa-106">ASP.NET MVC umožňuje zadejte funkce nebo chování jenom jednou a potom ho v aplikaci projeví everywhere mít.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-106">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an app.</span></span> <span data-ttu-id="bc0aa-107">To snižuje množství kódu, které potřebujete k zápisu a umožňuje kód, který menší chyba náchylné k chybám, usnadnění testování a jednodušší správu zápisu.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-107">This reduces the amount of code you need to write and makes the code you do write less error prone, easier to test, and easier to maintain.</span></span>

<span data-ttu-id="bc0aa-108">Podpora ověřování poskytované MVC a Entity Framework Core Code First je dobrým příkladem SUCHÝCH Princip v akci.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-108">The validation support provided by MVC and Entity Framework Core Code First is a good example of the DRY principle in action.</span></span> <span data-ttu-id="bc0aa-109">V jednom místě (ve třídě modelu) můžete určit deklarativně ověřovací pravidla a pravidla jsou vynucována everywhere v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-109">You can declaratively specify validation rules in one place (in the model class) and the rules are enforced everywhere in the app.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="bc0aa-110">Přidávání pravidel ověřování modelu filmu</span><span class="sxs-lookup"><span data-stu-id="bc0aa-110">Adding validation rules to the movie model</span></span>

<span data-ttu-id="bc0aa-111">Otevřete *Movie.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-111">Open the *Movie.cs* file.</span></span> <span data-ttu-id="bc0aa-112">DataAnnotations poskytuje integrovanou sadu atributů ověření, které se vztahují deklarativně všechny třídy nebo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-112">DataAnnotations provides a built-in set of validation attributes that you apply declaratively to any class or property.</span></span> <span data-ttu-id="bc0aa-113">(Také obsahuje formátování atributů, například `DataType` který usnadní formátování a neposkytují žádné ověření.)</span><span class="sxs-lookup"><span data-stu-id="bc0aa-113">(It also contains formatting attributes like `DataType` that help with formatting and don't provide any validation.)</span></span>

<span data-ttu-id="bc0aa-114">Aktualizace `Movie` třída využít předdefinované `Required`, `StringLength`, `RegularExpression`, a `Range` atributů ověření.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-114">Update the `Movie` class to take advantage of the built-in `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="bc0aa-115">Atributy ověření zadejte chování, které chcete vynutit ve vlastnostech modelu, které se použijí k.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-115">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="bc0aa-116">`Required` a `MinimumLength` atributy znamená, že vlastnost musí mít hodnotu; ale nic zabrání zadat mezer splňovat toto ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-116">The `Required` and `MinimumLength` attributes indicates that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span> <span data-ttu-id="bc0aa-117">`RegularExpression` Atribut se používá k omezení znaků, může být vstup.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-117">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="bc0aa-118">Ve výše, kódu `Genre` a `Rating` musí používat pouze písmena (bílé místo, číslice a speciální znaky nejsou povoleny).</span><span class="sxs-lookup"><span data-stu-id="bc0aa-118">In the code above, `Genre` and `Rating` must use only letters (white space, numbers and special characters are not allowed).</span></span> <span data-ttu-id="bc0aa-119">`Range` Atribut omezuje hodnota v zadaném rozsahu.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-119">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="bc0aa-120">`StringLength` Atribut umožňuje nastavit maximální délka ve vlastnosti string a volitelně jeho minimální délka.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-120">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span> <span data-ttu-id="bc0aa-121">Typů hodnot (například `decimal`, `int`, `float`, `DateTime`) jsou ze své podstaty potřeba a nepotřebujete `[Required]` atribut.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-121">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="bc0aa-122">S ověřovacích pravidel automaticky vynucuje technologie ASP.NET pomáhá zkontrolujte robustnější aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-122">Having validation rules automatically enforced by ASP.NET helps make your app more robust.</span></span> <span data-ttu-id="bc0aa-123">Také zajistí, že nelze zapomenete a ověřit něco nechtěně umožní chybná data do databáze.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-123">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

## <a name="validation-error-ui-in-mvc"></a><span data-ttu-id="bc0aa-124">Chyba ověření uživatelského rozhraní v MVC</span><span class="sxs-lookup"><span data-stu-id="bc0aa-124">Validation Error UI in MVC</span></span>

<span data-ttu-id="bc0aa-125">Spusťte aplikaci a přejděte na filmy kontroleru.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-125">Run the app and navigate to the Movies controller.</span></span>

<span data-ttu-id="bc0aa-126">Klepněte **vytvořit nový** odkaz na přidání nové videa.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-126">Tap the **Create New** link to add a new movie.</span></span> <span data-ttu-id="bc0aa-127">Vyplňte formulář s některé neplatné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-127">Fill out the form with some invalid values.</span></span> <span data-ttu-id="bc0aa-128">Jakmile jQuery ověřování na straně klienta zjistí chybu, zobrazí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-128">As soon as jQuery client side validation detects the error, it displays an error message.</span></span>

![Film zobrazit formulář s více chyby ověření klienta straně jQuery](../../tutorials/first-mvc-app/validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="bc0aa-130">Nemusí být možné je zadat desetinné čárky ve `Price` pole.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-130">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="bc0aa-131">Pro podporu [k ověřování jQuery](https://jqueryvalidation.org/) pro neanglická národní prostředí, které používají čárkou (",") pro desetinné čárky a formát data neanglických USA, musíte provést kroky globalizace aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-131">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="bc0aa-132">To [potíže Githubu 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) postup pro přidání desetinnou čárkou.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-132">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span> 

<span data-ttu-id="bc0aa-133">Všimněte si, jak formulář automaticky vykreslí příslušné ověření chybovou zprávu do jednotlivých polí, která obsahuje neplatnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-133">Notice how the form has automatically rendered an appropriate validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="bc0aa-134">Chyby se vynucují (pomocí jazyka JavaScript a jQuery) klientské a serverové (v případě, že uživatel, který má JavaScript zakázaná).</span><span class="sxs-lookup"><span data-stu-id="bc0aa-134">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="bc0aa-135">Významné výhodou je, že nemám potřebu změnit jeden řádek kódu `MoviesController` třídy nebo v *Create.cshtml* zobrazení, aby bylo možné povolit toto ověření uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-135">A significant benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.cshtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="bc0aa-136">Řadič a zobrazení, které jste vytvořili dříve v tomto kurzu automaticky zachyceny až ověření pravidla, kterou jste zadali pomocí atributů ověření ve vlastnostech `Movie` třída modelu.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-136">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified by using validation attributes on the properties of the `Movie` model class.</span></span> <span data-ttu-id="bc0aa-137">Test ověření pomocí `Edit` metody akce a stejné ověření platí.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-137">Test validation using the `Edit` action method, and the same validation is applied.</span></span>

<span data-ttu-id="bc0aa-138">Data formuláře není odesílat na server, dokud nejsou žádné chyby ověření straně klienta.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-138">The form data is not sent to the server until there are no client side validation errors.</span></span> <span data-ttu-id="bc0aa-139">Můžete to ověřit umístěním přerušení `HTTP Post` metodu, pomocí [nástroj Fiddler](http://www.telerik.com/fiddler) , nebo [nástroje pro vývojáře F12](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span><span class="sxs-lookup"><span data-stu-id="bc0aa-139">You can verify this by putting a break point in the `HTTP Post` method, by using the [Fiddler tool](http://www.telerik.com/fiddler) , or the [F12 Developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span></span>

## <a name="how-validation-works"></a><span data-ttu-id="bc0aa-140">Jak funguje ověření</span><span class="sxs-lookup"><span data-stu-id="bc0aa-140">How validation works</span></span>

<span data-ttu-id="bc0aa-141">Může vás zajímat, jak byl vygenerován ověření uživatelské rozhraní bez jakýchkoli aktualizací kód v kontroleru nebo zobrazení.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-141">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="bc0aa-142">Následující kód ukazuje dva `Create` metody.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-142">The following code shows the two `Create` methods.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

<span data-ttu-id="bc0aa-143">První (HTTP GET) `Create` metoda akce zobrazí počáteční vytvořit formulář.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-143">The first (HTTP GET) `Create` action method displays the initial Create form.</span></span> <span data-ttu-id="bc0aa-144">Druhý (`[HttpPost]`) verze zpracovává post formuláře.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-144">The second (`[HttpPost]`) version handles the form post.</span></span> <span data-ttu-id="bc0aa-145">Druhý `Create` – metoda ( `[HttpPost]` verze) volání `ModelState.IsValid` zkontrolujte, zda video má všechny chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-145">The second `Create` method (The `[HttpPost]` version) calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="bc0aa-146">Voláním této metody vyhodnotí všechny atributy ověřování, které byly použity k objektu.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-146">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="bc0aa-147">Pokud objekt má chyby ověření, `Create` metoda znovu zobrazí formulář.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-147">If the object has validation errors, the `Create` method re-displays the form.</span></span> <span data-ttu-id="bc0aa-148">Pokud nejsou žádné chyby, metoda nové film uloží v databázi.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-148">If there are no errors, the method saves the new movie in the database.</span></span> <span data-ttu-id="bc0aa-149">V našem příkladu film formuláře není odeslána na server po ověření chyb zjištěných na straně klienta; druhý `Create` metoda je volána nikdy, když jsou chyby ověření straně klienta.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-149">In our movie example, the form is not posted to the server when there are validation errors detected on the client side; the second `Create` method is never called when there are client side validation errors.</span></span> <span data-ttu-id="bc0aa-150">Pokud zakážete JavaScript v prohlížeči, ověření klienta je zakázán a můžete otestovat HTTP POST `Create` metoda `ModelState.IsValid` zjišťování všechny chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-150">If you disable JavaScript in your browser, client validation is disabled and you can test the HTTP POST `Create` method `ModelState.IsValid` detecting any validation errors.</span></span>

<span data-ttu-id="bc0aa-151">Můžete nastavit bod přerušení v `[HttpPost] Create` metoda a ověřte nikdy volání metody, ověřování na straně klienta nebude odesílat data formuláře, pokud jsou zjištěny chyby ověřování.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-151">You can set a break point in the `[HttpPost] Create` method and verify the method is never called, client side validation will not submit the form data when validation errors are detected.</span></span> <span data-ttu-id="bc0aa-152">Pokud zakázat JavaScript v prohlížeči a pak odeslání formuláře s chybami, se setkají místo přerušení.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-152">If you disable JavaScript in your browser, then submit the form with errors, the break point will be hit.</span></span> <span data-ttu-id="bc0aa-153">Se stále objevuje úplného ověření bez JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-153">You still get full validation without JavaScript.</span></span> 

<span data-ttu-id="bc0aa-154">Následující obrázek ukazuje, jak zakázat JavaScript v prohlížeči FireFox.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-154">The following image shows how to disable JavaScript in the FireFox browser.</span></span>

![Firefox:, Zrušte zaškrtnutí políčka Povolit Javascript na kartě Možnosti v obsahu.](../../tutorials/first-mvc-app/validation/_static/ff.png)

<span data-ttu-id="bc0aa-156">Následující obrázek ukazuje, jak zakázat JavaScript v prohlížeči Chrome.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-156">The following image shows how to disable JavaScript in the Chrome browser.</span></span>

![Google Chrome: Část v Javascript v nastavení obsahu a vyberte zakázat libovolnou lokalitu spouštět JavaScript.](../../tutorials/first-mvc-app/validation/_static/chrome.png)

<span data-ttu-id="bc0aa-158">Po zakázání JavaScript post neplatná data a projděte ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-158">After you disable JavaScript, post invalid data and step through the debugger.</span></span>

![Při ladění na post neplatných dat, Intellisense v ModelState.IsValid ukazuje, že je hodnota false.](../../tutorials/first-mvc-app/validation/_static/ms.png)

<span data-ttu-id="bc0aa-160">Níže uvádíme část *Create.cshtml* zobrazit šablonu, která vygeneroval dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-160">Below is portion of the *Create.cshtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="bc0aa-161">Použije se uvedené výše obě metody akce k zobrazení původního formuláře a znovu ji zobrazit v případě chyby.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-161">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]

<span data-ttu-id="bc0aa-162">[Vstupní značka pomocná](xref:mvc/views/working-with-forms) používá [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atributů a atributů HTML, které jsou potřebné pro architekturu jQuery ověření na straně klienta vytváří.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-162">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client side.</span></span> <span data-ttu-id="bc0aa-163">[Pomocná rutina pro ověření značky](xref:mvc/views/working-with-forms#the-validation-tag-helpers) zobrazí chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-163">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="bc0aa-164">V tématu [ověření](xref:mvc/models/validation) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-164">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="bc0aa-165">Co je skutečně dobrý o tento přístup je, že ani řadičem ani `Create` zobrazit šablonu zná nic o skutečné ověřovacích pravidel vynucován nebo o specifické chybové zprávy zobrazují.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-165">What's really nice about this approach is that neither the controller nor the `Create` view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="bc0aa-166">Ověřovací pravidla a řetězce chyby se zadávají pouze v `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-166">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="bc0aa-167">Tyto stejné ověřovací pravidla budou automaticky použita pro `Edit` zobrazení a jakékoli další zobrazení šablony můžete vytvořit které upravit modelu.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-167">These same validation rules are automatically applied to the `Edit` view and any other views templates you might create that edit your model.</span></span>

<span data-ttu-id="bc0aa-168">Pokud potřebujete změnit logiku ověření, můžete tak učinit na jednom místě přidáním atributů ověření modelu (v tomto příkladu `Movie` třídy).</span><span class="sxs-lookup"><span data-stu-id="bc0aa-168">When you need to change validation logic, you can do so in exactly one place by adding validation attributes to the model (in this example, the `Movie` class).</span></span> <span data-ttu-id="bc0aa-169">Nebudete mít starat o různých částech aplikace je nekonzistentní s jak se vynucují pravidla – veškerou logiku ověřování definované na jednom místě se použije everywhere.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-169">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="bc0aa-170">To zajišťuje kód velmi vyčištění a umožňuje snadno spravovat a momentální.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-170">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="bc0aa-171">. To znamená, že je budete mít plně ctít zásady SUCHÝCH Princip.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-171">And it means that you'll be fully honoring the DRY principle.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="bc0aa-172">Pomocí atributů datový typ</span><span class="sxs-lookup"><span data-stu-id="bc0aa-172">Using DataType Attributes</span></span>

<span data-ttu-id="bc0aa-173">Otevřete *Movie.cs* soubor a zkontrolujte `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-173">Open the *Movie.cs* file and examine the `Movie` class.</span></span> <span data-ttu-id="bc0aa-174">`System.ComponentModel.DataAnnotations` Obor názvů poskytuje atributy formátování kromě integrovanou sadu atributů ověření.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-174">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="bc0aa-175">Provedli jsme již `DataType` Výčtová hodnota, datum vydání a pole cena.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-175">We've already applied a `DataType` enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="bc0aa-176">Následující kód ukazuje `ReleaseDate` a `Price` vlastnosti s příslušnou `DataType` atribut.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-176">The following code shows the `ReleaseDate` and `Price` properties with the appropriate `DataType` attribute.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="bc0aa-177">`DataType` Atributy pouze poskytovat pro modul zobrazení pro zobrazení dat (a zadejte atributy, jako `<a>` pro adresy URL a `<a href="mailto:EmailAddress.com">` e-mailu.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-177">The `DataType` attributes only provide hints for the view engine to format the data (and supply attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email.</span></span> <span data-ttu-id="bc0aa-178">Můžete použít `RegularExpression` atribut pro ověření formátu dat.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-178">You can use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="bc0aa-179">`DataType` Atribut slouží k určení datový typ, který je specifičtější než vnitřní typ databáze, nejsou atributů ověření.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-179">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type, they are not validation attributes.</span></span> <span data-ttu-id="bc0aa-180">V tomto případě chceme jenom udržování přehledu o datum, není čas.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-180">In this case we only want to keep track of the date, not the time.</span></span> <span data-ttu-id="bc0aa-181">`DataType` Výčtu poskytuje pro mnoho typů dat, jako je například datum, čas, telefonní číslo, měny, EmailAddress a další.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-181">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress and more.</span></span> <span data-ttu-id="bc0aa-182">`DataType` Atributu můžete také povolit aplikace automaticky poskytnout konkrétní typ funkce.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-182">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="bc0aa-183">Například `mailto:` může vytvořit odkaz pro `DataType.EmailAddress`, a datum selektor lze zadat pro `DataType.Date` v prohlížečích podporujících HTML5.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-183">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="bc0aa-184">`DataType` Atributy vysílá standardu HTML 5 `data-` (výrazný data dash) atributy, které můžete porozumět standardu HTML 5 prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-184">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="bc0aa-185">`DataType` Atributy provést **není** žádné ověřování.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-185">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="bc0aa-186">`DataType.Date`neurčuje formát data, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-186">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="bc0aa-187">Ve výchozím nastavení, zobrazí se pole dat podle výchozích formátů podle serveru `CultureInfo`.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-187">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="bc0aa-188">`DisplayFormat` Atribut slouží k explicitnímu zadání formát data:</span><span class="sxs-lookup"><span data-stu-id="bc0aa-188">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="bc0aa-189">`ApplyFormatInEditMode` Nastavení určuje, že formátování, které také bude použito při hodnota je zobrazena v textovém poli pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-189">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="bc0aa-190">(Který není vhodné pro některá pole – například hodnoty měny, pravděpodobně nechcete symbolu měny do textového pole pro úpravy.)</span><span class="sxs-lookup"><span data-stu-id="bc0aa-190">(You might not want that for some fields — for example, for currency values, you probably do not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="bc0aa-191">Můžete použít `DisplayFormat` atribut podle sám sebe, ale obecně je vhodné používat `DataType` atribut.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-191">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="bc0aa-192">`DataType` Atribut přenese tak sémantika data a jak vykreslit ho na obrazovce a nabízí následující výhody, které není dostupná s DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="bc0aa-192">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="bc0aa-193">V prohlížeči můžete povolit funkce HTML5 (např. k zobrazení ovládacího prvku kalendář, symbolu měny vhodné národního prostředí, e-mailu odkazy, atd.)</span><span class="sxs-lookup"><span data-stu-id="bc0aa-193">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>

* <span data-ttu-id="bc0aa-194">Ve výchozím nastavení bude v prohlížeči vykreslovat data ve správném formátu podle národního prostředí.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-194">By default, the browser will render data using the correct format based on your locale.</span></span>

* <span data-ttu-id="bc0aa-195">`DataType` Atributu můžete povolit MVC na výběr šablony pravé pole k poskytnutí dat ( `DisplayFormat` pokud používá samotné používá šablonu řetězec).</span><span class="sxs-lookup"><span data-stu-id="bc0aa-195">The `DataType` attribute can enable MVC to choose the right field template to render the data (the `DisplayFormat` if used by itself uses the string template).</span></span>

> [!NOTE]
> <span data-ttu-id="bc0aa-196">k ověřování jQuery nefunguje s `Range` atribut a `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-196">jQuery validation does not work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="bc0aa-197">Například následující kód bude vždy zobrazovat chyby ověření straně klienta, i v případě, že datum je v zadaném rozsahu:</span><span class="sxs-lookup"><span data-stu-id="bc0aa-197">For example, the following code will always display a client side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="bc0aa-198">Budete muset zakázat ověřování jQuery datum používat `Range` atribut s `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-198">You will need to disable jQuery date validation to use the `Range` attribute with `DateTime`.</span></span> <span data-ttu-id="bc0aa-199">Je obecně není vhodné zkompilovat pevného data v modely, takže pomocí `Range` atribut a `DateTime` se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-199">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="bc0aa-200">Následující kód ukazuje kombinování atributy na jeden řádek:</span><span class="sxs-lookup"><span data-stu-id="bc0aa-200">The following code shows combining attributes on one line:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

<span data-ttu-id="bc0aa-201">V další části řady, jsme budete zkontrolujte, zda aplikace a některá vylepšení pro automaticky generované `Details` a `Delete` metody.</span><span class="sxs-lookup"><span data-stu-id="bc0aa-201">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc0aa-202">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bc0aa-202">Additional resources</span></span>

* [<span data-ttu-id="bc0aa-203">Práce s formuláři</span><span class="sxs-lookup"><span data-stu-id="bc0aa-203">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="bc0aa-204">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="bc0aa-204">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="bc0aa-205">Úvod do pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="bc0aa-205">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="bc0aa-206">Vytváření Pomocníci značky</span><span class="sxs-lookup"><span data-stu-id="bc0aa-206">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)