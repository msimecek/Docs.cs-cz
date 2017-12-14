---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: "Přidání ověřování do modelu | Microsoft Docs"
author: Rick-Anderson
description: "Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 73332d168e2f22621cb234a6591f3ce0eeed802f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="142dd-104">Přidání ověřování do modelu</span><span class="sxs-lookup"><span data-stu-id="142dd-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="142dd-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="142dd-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="142dd-106">Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="142dd-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="142dd-107">Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.</span><span class="sxs-lookup"><span data-stu-id="142dd-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="142dd-108">V tomto v této části přidáte logiku ověření pro `Movie` modelu a budete ujistěte, že ověřovací pravidla se uplatňují vždy, když uživatel se pokusí vytvořit nebo upravit film pomocí aplikace.</span><span class="sxs-lookup"><span data-stu-id="142dd-108">In this this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user attempts to create or edit a movie using the application.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="142dd-109">Zachování SUCHÝCH věcí</span><span class="sxs-lookup"><span data-stu-id="142dd-109">Keeping Things DRY</span></span>

<span data-ttu-id="142dd-110">Jeden ze základních principů návrhu architektury ASP.NET MVC je suchého (&quot;nemáte opakujte sami&quot;).</span><span class="sxs-lookup"><span data-stu-id="142dd-110">One of the core design tenets of ASP.NET MVC is DRY (&quot;Don't Repeat Yourself&quot;).</span></span> <span data-ttu-id="142dd-111">ASP.NET MVC umožňuje zadejte funkce nebo chování jenom jednou a potom ho v aplikaci projeví everywhere mít.</span><span class="sxs-lookup"><span data-stu-id="142dd-111">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an application.</span></span> <span data-ttu-id="142dd-112">To snižuje množství kódu, které potřebujete k zápisu a umožňuje kód, který menší chyba náchylné k chybám a jednodušší správu zápisu.</span><span class="sxs-lookup"><span data-stu-id="142dd-112">This reduces the amount of code you need to write and makes the code you do write less error prone and easier to maintain.</span></span>

<span data-ttu-id="142dd-113">Podpora ověřování poskytované ASP.NET MVC a Entity Framework Code First je skvělým příklad SUCHÝCH zásadu v akci.</span><span class="sxs-lookup"><span data-stu-id="142dd-113">The validation support provided by ASP.NET MVC and Entity Framework Code First is a great example of the DRY principle in action.</span></span> <span data-ttu-id="142dd-114">V jednom místě (ve třídě modelu) můžete určit deklarativně ověřovací pravidla a pravidla jsou vynucována everywhere v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="142dd-114">You can declaratively specify validation rules in one place (in the model class) and the rules are enforced everywhere in the application.</span></span>

<span data-ttu-id="142dd-115">Podíváme, jak můžete využít výhod tato podpora ověřování v aplikaci film.</span><span class="sxs-lookup"><span data-stu-id="142dd-115">Let's look at how you can take advantage of this validation support in the movie application.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="142dd-116">Přidávání pravidel ověřování modelu filmu</span><span class="sxs-lookup"><span data-stu-id="142dd-116">Adding Validation Rules to the Movie Model</span></span>

<span data-ttu-id="142dd-117">Budete začněte tím, že některé logiku ověření pro přidání `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="142dd-117">You'll begin by adding some validation logic to the `Movie` class.</span></span>

<span data-ttu-id="142dd-118">Otevřete *Movie.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="142dd-118">Open the *Movie.cs* file.</span></span> <span data-ttu-id="142dd-119">Přidat `using` příkaz v horní části souboru, který odkazuje [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) obor názvů:</span><span class="sxs-lookup"><span data-stu-id="142dd-119">Add a `using` statement at the top of the file that references the [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

<span data-ttu-id="142dd-120">Všimněte si, obor názvů neobsahuje `System.Web`.</span><span class="sxs-lookup"><span data-stu-id="142dd-120">Notice the namespace does not contain `System.Web`.</span></span> <span data-ttu-id="142dd-121">DataAnnotations poskytuje integrovanou sadu atributů ověření, které můžete provést deklarativně všechny třídy nebo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="142dd-121">DataAnnotations provides a built-in set of validation attributes that you can apply declaratively to any class or property.</span></span>

<span data-ttu-id="142dd-122">Nyní aktualizovat `Movie` třída využít předdefinované [ `Required` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), a [ `Range` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributů ověření .</span><span class="sxs-lookup"><span data-stu-id="142dd-122">Now update the `Movie` class to take advantage of the built-in [`Required`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), and [`Range`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) validation attributes.</span></span> <span data-ttu-id="142dd-123">Použijte následující kód jako příklad umístění pro použití atributy.</span><span class="sxs-lookup"><span data-stu-id="142dd-123">Use the following code as an example of where to apply the attributes.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

<span data-ttu-id="142dd-124">Spusťte aplikaci a zobrazí se znovu k následující chybě čas spuštění:</span><span class="sxs-lookup"><span data-stu-id="142dd-124">Run the application and you will again get the following run time error:</span></span>

<span data-ttu-id="142dd-125">***Model zálohování kontext 'MovieDBContext' se od vytvoření databáze změnil. Zvažte použití migrace Code First k aktualizaci databáze ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***</span><span class="sxs-lookup"><span data-stu-id="142dd-125">***The model backing the 'MovieDBContext' context has changed since the database was created. Consider using Code First Migrations to update the database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***</span></span>

<span data-ttu-id="142dd-126">Migrace použijeme k aktualizaci schématu.</span><span class="sxs-lookup"><span data-stu-id="142dd-126">We will use migrations to update the schema.</span></span> <span data-ttu-id="142dd-127">Sestavte řešení a pak otevřete **Konzola správce balíčků** okna a zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="142dd-127">Build the solution, and then open the **Package Manager Console** window and enter the following commands:</span></span>

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

<span data-ttu-id="142dd-128">Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMIgration` odvozené třídy se zadaným názvem (*AddDataAnnotationsMig*) a v `Up` metoda vidíte kód, který aktualizuje omezení schématu.</span><span class="sxs-lookup"><span data-stu-id="142dd-128">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class with the name specified (*AddDataAnnotationsMig*), and in the `Up` method you can see the code that updates the schema constraints.</span></span> <span data-ttu-id="142dd-129">`Title` a `Genre` pole jsou již s možnou hodnotou Null (to znamená, je třeba zadat hodnotu) a `Rating` pole má maximální délku 5.</span><span class="sxs-lookup"><span data-stu-id="142dd-129">The `Title` and `Genre` fields are no longer nullable (that is, you must enter a value) and the `Rating` field has a maximum length of 5.</span></span>

<span data-ttu-id="142dd-130">Atributy ověření zadejte chování, které chcete vynutit ve vlastnostech modelu, které se použijí k.</span><span class="sxs-lookup"><span data-stu-id="142dd-130">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="142dd-131">`Required` Atribut uvádí, že vlastnost musí mít hodnotu; v této ukázce, musí mít hodnoty pro film `Title`, `ReleaseDate`, `Genre`, a `Price` vlastnosti, aby byla platná.</span><span class="sxs-lookup"><span data-stu-id="142dd-131">The `Required` attribute indicates that a property must have a value; in this sample, a movie has to have values for the `Title`, `ReleaseDate`, `Genre`, and `Price` properties in order to be valid.</span></span> <span data-ttu-id="142dd-132">`Range` Atribut omezuje hodnota v zadaném rozsahu.</span><span class="sxs-lookup"><span data-stu-id="142dd-132">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="142dd-133">`StringLength` Atribut umožňuje nastavit maximální délka ve vlastnosti string a volitelně jeho minimální délka.</span><span class="sxs-lookup"><span data-stu-id="142dd-133">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span> <span data-ttu-id="142dd-134">Vnitřní typy (například `decimal, int, float, DateTime`) se vyžadují ve výchozím nastavení a nepotřebujete `Required` atribut.</span><span class="sxs-lookup"><span data-stu-id="142dd-134">Intrinsic types (such as `decimal, int, float, DateTime`) are required by default and don't need the `Required` attribute.</span></span>

<span data-ttu-id="142dd-135">Kód nejprve zajistí, že ověřovací pravidla, které jste zadali na třídu modelu, jsou vyžadována před aplikace uloží změny v databázi.</span><span class="sxs-lookup"><span data-stu-id="142dd-135">Code First ensures that the validation rules you specify on a model class are enforced before the application saves changes in the database.</span></span> <span data-ttu-id="142dd-136">Například následující kód vyvolá výjimku při `SaveChanges` metoda je volána, protože některé požadované `Movie` chybí hodnoty vlastností a je nulová (což je mimo platný rozsah).</span><span class="sxs-lookup"><span data-stu-id="142dd-136">For example, the code below will throw an exception when the `SaveChanges` method is called, because several required `Movie` property values are missing and the price is zero (which is out of the valid range).</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

<span data-ttu-id="142dd-137">S ověřovacích pravidel automaticky vynucováno rozhraní .NET Framework pomáhá zkontrolujte aplikace robustnější.</span><span class="sxs-lookup"><span data-stu-id="142dd-137">Having validation rules automatically enforced by the .NET Framework helps make your application more robust.</span></span> <span data-ttu-id="142dd-138">Také zajistí, že nelze zapomenete a ověřit něco nechtěně umožní chybná data do databáze.</span><span class="sxs-lookup"><span data-stu-id="142dd-138">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

<span data-ttu-id="142dd-139">Tady je úplný výpis pro aktualizaci kódu *Movie.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="142dd-139">Here's a complete code listing for the updated *Movie.cs* file:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a><span data-ttu-id="142dd-140">Chyba ověření uživatelského rozhraní v architektuře ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="142dd-140">Validation Error UI in ASP.NET MVC</span></span>

<span data-ttu-id="142dd-141">Znovu spusťte aplikaci a přejděte do */Movies* adresy URL.</span><span class="sxs-lookup"><span data-stu-id="142dd-141">Re-run the application and navigate to the */Movies* URL.</span></span>

<span data-ttu-id="142dd-142">Klikněte **vytvořit nový** odkaz na přidání nové videa.</span><span class="sxs-lookup"><span data-stu-id="142dd-142">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="142dd-143">Vyplňte formulář s některé neplatné hodnoty a klikněte **vytvořit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="142dd-143">Fill out the form with some invalid values and then click the **Create** button.</span></span>

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="142dd-145">pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárkou (&quot;,&quot;) desetinné čárky, je třeba zahrnout *globalize.js* a konkrétní *cultures/globalize.cultures.js* souboru (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) a JavaScript používat `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="142dd-145">to support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="142dd-146">Následující kód ukazuje změny souboru Views\Movies\Edit.cshtml pro práci s &quot;fr-FR&quot; jazyková verze:</span><span class="sxs-lookup"><span data-stu-id="142dd-146">The following code shows the modifications to the Views\Movies\Edit.cshtml file to work with the &quot;fr-FR&quot; culture:</span></span>


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

<span data-ttu-id="142dd-147">Všimněte si, jak formulář automaticky použila barvu červené ohraničení zvýrazněte textová pole, které obsahují neplatná data a má vygenerované chybovou zprávu ověření příslušné vedle každé z nich.</span><span class="sxs-lookup"><span data-stu-id="142dd-147">Notice how the form has automatically used a red border color to highlight the text boxes that contain invalid data and has emitted an appropriate validation error message next to each one.</span></span> <span data-ttu-id="142dd-148">Chyby se vynucují (pomocí jazyka JavaScript a jQuery) klientské a serverové (v případě, že uživatel, který má JavaScript zakázaná).</span><span class="sxs-lookup"><span data-stu-id="142dd-148">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="142dd-149">Skutečné výhodou je, že nebyla budete muset změnit jeden řádek kódu `MoviesController` třídy nebo v *Create.cshtml* zobrazení, aby bylo možné povolit toto ověření uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="142dd-149">A real benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.cshtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="142dd-150">Řadič a zobrazení, které jste vytvořili dříve v tomto kurzu automaticky zachyceny až ověření pravidla, kterou jste zadali pomocí atributů ověření ve vlastnostech `Movie` třída modelu.</span><span class="sxs-lookup"><span data-stu-id="142dd-150">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified by using validation attributes on the properties of the `Movie` model class.</span></span>

<span data-ttu-id="142dd-151">Možná jste si všimli vlastností `Title` a `Genre`, požadovaný atribut nevynucuje, dokud odesláním formuláře (stiskněte tlačítko **vytvořit** tlačítko), nebo zadejte text do vstupní pole a odebrat ji.</span><span class="sxs-lookup"><span data-stu-id="142dd-151">You might have noticed for the properties `Title` and `Genre`, the required attribute is not enforced until you submit the form (hit the **Create** button), or enter text into the input field and removed it.</span></span> <span data-ttu-id="142dd-152">V poli což je původně prázdné (například pole pro vytvoření zobrazení) a který má požadovaný atribut a žádné jiné atributy ověření můžete provést následující příkaz a spustit ověření:</span><span class="sxs-lookup"><span data-stu-id="142dd-152">For a field which is initially empty (such as the fields on the Create view) and which has only the required attribute and no other validation attributes, you can do the following to trigger validation:</span></span>

1. <span data-ttu-id="142dd-153">Klikněte do pole.</span><span class="sxs-lookup"><span data-stu-id="142dd-153">Tab into the field.</span></span>
2. <span data-ttu-id="142dd-154">Zadejte nějaký text.</span><span class="sxs-lookup"><span data-stu-id="142dd-154">Enter some text.</span></span>
3. <span data-ttu-id="142dd-155">Klikněte na.</span><span class="sxs-lookup"><span data-stu-id="142dd-155">Tab out.</span></span>
4. <span data-ttu-id="142dd-156">Kartě zpět do pole.</span><span class="sxs-lookup"><span data-stu-id="142dd-156">Tab back into the field.</span></span>
5. <span data-ttu-id="142dd-157">Odeberte text.</span><span class="sxs-lookup"><span data-stu-id="142dd-157">Remove the text.</span></span>
6. <span data-ttu-id="142dd-158">Klikněte na.</span><span class="sxs-lookup"><span data-stu-id="142dd-158">Tab out.</span></span>

<span data-ttu-id="142dd-159">Výše uvedené pořadí aktivují požadované ověření bez stiskne tlačítko pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="142dd-159">The above sequence will trigger the required validation without hitting the submit button.</span></span> <span data-ttu-id="142dd-160">Jednoduše stiskne tlačítko pro odeslání bez nutnosti zadávat žádná pole aktivují ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="142dd-160">Simply hitting the submit button without entering any of the fields will trigger client side validation.</span></span> <span data-ttu-id="142dd-161">Data formuláře není odesílat na server, dokud nejsou žádné chyby ověření straně klienta.</span><span class="sxs-lookup"><span data-stu-id="142dd-161">The form data is not sent to the server until there are no client side validation errors.</span></span> <span data-ttu-id="142dd-162">Toto můžete otestovat uvedení přerušení v metodu Post protokolu HTTP nebo pomocí [nástroj fiddler](http://fiddler2.com/fiddler2/) nebo aplikace Internet Explorer 9 [nástroje pro vývojáře F12](https://msdn.microsoft.com/en-us/ie/aa740478).</span><span class="sxs-lookup"><span data-stu-id="142dd-162">You can test this by putting a break point in the HTTP Post method or using the [fiddler tool](http://fiddler2.com/fiddler2/) or the IE 9 [F12 developer tools](https://msdn.microsoft.com/en-us/ie/aa740478).</span></span>

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a><span data-ttu-id="142dd-163">Dojde k ověření v vytvořením zobrazování a vytváření metody akce</span><span class="sxs-lookup"><span data-stu-id="142dd-163">How Validation Occurs in the Create View and Create Action Method</span></span>

<span data-ttu-id="142dd-164">Může vás zajímat, jak byl vygenerován ověření uživatelské rozhraní bez jakýchkoli aktualizací kód v kontroleru nebo zobrazení.</span><span class="sxs-lookup"><span data-stu-id="142dd-164">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="142dd-165">Další výpis znázorňuje, co `Create` metody v `MovieController` třída vypadají.</span><span class="sxs-lookup"><span data-stu-id="142dd-165">The next listing shows what the `Create` methods in the `MovieController` class look like.</span></span> <span data-ttu-id="142dd-166">Jsou stejné jako u jak je vytvořen v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="142dd-166">They're unchanged from how you created them earlier in this tutorial.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

<span data-ttu-id="142dd-167">První (HTTP GET) `Create` metoda akce zobrazí počáteční vytvořit formulář.</span><span class="sxs-lookup"><span data-stu-id="142dd-167">The first (HTTP GET) `Create` action method displays the initial Create form.</span></span> <span data-ttu-id="142dd-168">Druhý (`[HttpPost]`) verze zpracovává post formuláře.</span><span class="sxs-lookup"><span data-stu-id="142dd-168">The second (`[HttpPost]`) version handles the form post.</span></span> <span data-ttu-id="142dd-169">Druhý `Create` – metoda ( `HttpPost` verze) volání `ModelState.IsValid` zkontrolujte, zda video má všechny chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="142dd-169">The second `Create` method (The `HttpPost` version) calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="142dd-170">Voláním této metody vyhodnotí všechny atributy ověřování, které byly použity k objektu.</span><span class="sxs-lookup"><span data-stu-id="142dd-170">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="142dd-171">Pokud objekt má chyby ověření, `Create` metoda znovu zobrazí formulář.</span><span class="sxs-lookup"><span data-stu-id="142dd-171">If the object has validation errors, the `Create` method re-displays the form.</span></span> <span data-ttu-id="142dd-172">Pokud nejsou žádné chyby, metoda nové film uloží v databázi.</span><span class="sxs-lookup"><span data-stu-id="142dd-172">If there are no errors, the method saves the new movie in the database.</span></span> <span data-ttu-id="142dd-173">V našem příkladu film používáme **formuláře není odeslána na server, pokud nejsou chyby ověření na straně klienta; zjištěna druhá** `Create` **metoda je volána nikdy**.</span><span class="sxs-lookup"><span data-stu-id="142dd-173">In our movie example we are using, **the form is not posted to the server when their are validation errors detected on the client side; the second** `Create`**method is never called**.</span></span> <span data-ttu-id="142dd-174">Pokud zakážete JavaScript v prohlížeči, ověření klienta je zakázané a HTTP POST `Create` volání metod `ModelState.IsValid` zkontrolujte, zda video má všechny chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="142dd-174">If you disable JavaScript in your browser, client validation is disabled and the HTTP POST `Create` method calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span>

<span data-ttu-id="142dd-175">Můžete nastavit bod přerušení v `HttpPost Create` metoda a ověřte nikdy volání metody, ověřování na straně klienta nebude odesílat data formuláře, pokud jsou zjištěny chyby ověřování.</span><span class="sxs-lookup"><span data-stu-id="142dd-175">You can set a break point in the `HttpPost Create` method and verify the method is never called, client side validation will not submit the form data when validation errors are detected.</span></span> <span data-ttu-id="142dd-176">Pokud zakázat JavaScript v prohlížeči a pak odeslání formuláře s chybami, se setkají místo přerušení.</span><span class="sxs-lookup"><span data-stu-id="142dd-176">If you disable JavaScript in your browser, then submit the form with errors, the break point will be hit.</span></span> <span data-ttu-id="142dd-177">Se stále objevuje úplného ověření bez JavaScript.</span><span class="sxs-lookup"><span data-stu-id="142dd-177">You still get full validation without JavaScript.</span></span> <span data-ttu-id="142dd-178">Následující obrázek ukazuje, jak zakázat JavaScript v aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="142dd-178">The following image shows how to disable JavaScript in Internet Explorer.</span></span>

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

<span data-ttu-id="142dd-179">Následující obrázek ukazuje, jak zakázat JavaScript v prohlížeči FireFox.</span><span class="sxs-lookup"><span data-stu-id="142dd-179">The following image shows how to disable JavaScript in the FireFox browser.</span></span>

![](adding-validation-to-the-model/_static/image5.png)

<span data-ttu-id="142dd-180">Následující obrázek ukazuje, jak zakázat jazyka JavaScript pomocí prohlížeče Chrome.</span><span class="sxs-lookup"><span data-stu-id="142dd-180">The following image shows how to disable JavaScript with the Chrome browser.</span></span>

![](adding-validation-to-the-model/_static/image6.png)

<span data-ttu-id="142dd-181">Níže je *Create.cshtml* zobrazit šablonu, která vygeneroval dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="142dd-181">Below is the *Create.cshtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="142dd-182">Použije se uvedené výše obě metody akce k zobrazení původního formuláře a znovu ji zobrazit v případě chyby.</span><span class="sxs-lookup"><span data-stu-id="142dd-182">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

<span data-ttu-id="142dd-183">Všimněte si, jak kód používá `Html.EditorFor` pomocná rutina pro výstup `<input>` element pro každou `Movie` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="142dd-183">Notice how the code uses an `Html.EditorFor` helper to output the `<input>` element for each `Movie` property.</span></span> <span data-ttu-id="142dd-184">U tohoto pomocníka je volání `Html.ValidationMessageFor` metodu helper.</span><span class="sxs-lookup"><span data-stu-id="142dd-184">Next to this helper is a call to the `Html.ValidationMessageFor` helper method.</span></span> <span data-ttu-id="142dd-185">Tyto dvě metody helper pracovat objekt modelu, který je předán zobrazení řadičem (v tomto případě `Movie` objekt).</span><span class="sxs-lookup"><span data-stu-id="142dd-185">These two helper methods work with the model object that's passed by the controller to the view (in this case, a `Movie` object).</span></span> <span data-ttu-id="142dd-186">Budou automaticky hledat atributů ověření zadané v modelu a zobrazí chybové zprávy podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="142dd-186">They automatically look for validation attributes specified on the model and display error messages as appropriate.</span></span>

<span data-ttu-id="142dd-187">Co je skutečně dobrý o tento přístup je, že řadičem ani vytvořit šablony zobrazení nic o skutečné ověřovacích pravidel vynucován nebo zná o specifické chybové zprávy zobrazují.</span><span class="sxs-lookup"><span data-stu-id="142dd-187">What's really nice about this approach is that neither the controller nor the Create view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="142dd-188">Ověřovací pravidla a řetězce chyby se zadávají pouze v `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="142dd-188">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="142dd-189">Tyto stejné ověřovací pravidla budou automaticky použita k zobrazení upravit a jakékoli další zobrazení šablony, které můžete vytvořit které upravit modelu.</span><span class="sxs-lookup"><span data-stu-id="142dd-189">These same validation rules are automatically applied to the Edit view and any other views templates you might create that edit your model.</span></span>

<span data-ttu-id="142dd-190">Pokud chcete později změnit logiku ověření, můžete tak učinit na jednom místě přidáním atributů ověření modelu (v tomto příkladu `movie` třídy).</span><span class="sxs-lookup"><span data-stu-id="142dd-190">If you want to change the validation logic later, you can do so in exactly one place by adding validation attributes to the model (in this example, the `movie` class).</span></span> <span data-ttu-id="142dd-191">Nebudete mít starat o různých částech aplikace je nekonzistentní s jak se vynucují pravidla – veškerou logiku ověřování definované na jednom místě se použije everywhere.</span><span class="sxs-lookup"><span data-stu-id="142dd-191">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="142dd-192">To zajišťuje kód velmi vyčištění a umožňuje snadno spravovat a momentální.</span><span class="sxs-lookup"><span data-stu-id="142dd-192">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="142dd-193">. To znamená, že budete plně ctít zásady SUCHÝCH Princip.</span><span class="sxs-lookup"><span data-stu-id="142dd-193">And it means that that you'll be fully honoring the DRY principle.</span></span>

## <a name="adding-formatting-to-the-movie-model"></a><span data-ttu-id="142dd-194">Přidání formátování ke film modelu</span><span class="sxs-lookup"><span data-stu-id="142dd-194">Adding Formatting to the Movie Model</span></span>

<span data-ttu-id="142dd-195">Otevřete *Movie.cs* soubor a zkontrolujte `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="142dd-195">Open the *Movie.cs* file and examine the `Movie` class.</span></span> <span data-ttu-id="142dd-196">[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) Obor názvů poskytuje atributy formátování kromě integrovanou sadu atributů ověření.</span><span class="sxs-lookup"><span data-stu-id="142dd-196">The [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="142dd-197">Provedli jsme již [ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) Výčtová hodnota, datum vydání a pole cena.</span><span class="sxs-lookup"><span data-stu-id="142dd-197">We've already applied a [`DataType`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="142dd-198">Následující kód ukazuje `ReleaseDate` a `Price` vlastnosti s příslušnou [ `DisplayFormat` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut.</span><span class="sxs-lookup"><span data-stu-id="142dd-198">The following code shows the `ReleaseDate` and `Price` properties with the appropriate [`DisplayFormat`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

<span data-ttu-id="142dd-199">[ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) Atributy nejsou atributů ověření, používají se k informování zobrazovací modul, jak k vykreslení kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="142dd-199">The [`DataType`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) attributes are not validation attributes, they are used to tell the view engine how to render the HTML.</span></span> <span data-ttu-id="142dd-200">V příkladu nahoře `DataType.Date` atribut zobrazí data film jako kalendářní data pouze bez čas.</span><span class="sxs-lookup"><span data-stu-id="142dd-200">In the example above, the `DataType.Date` attribute displays the movie dates as dates only, without time.</span></span> <span data-ttu-id="142dd-201">Například následující [ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atributy nemáte ověření formátu dat:</span><span class="sxs-lookup"><span data-stu-id="142dd-201">For example, the following [`DataType`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) attributes don't validate the format of the data:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

<span data-ttu-id="142dd-202">Atributy uvedené výše pouze poskytovat pro modul zobrazení pro zobrazení dat (a zadejte atributy, jako &lt;&gt; pro adresy URL a &lt;href =&quot;mailto:EmailAddress.com&quot; &gt; e-mailu.</span><span class="sxs-lookup"><span data-stu-id="142dd-202">The attributes listed above only provide hints for the view engine to format the data (and supply attributes such as &lt;a&gt; for URL's and &lt;a href=&quot;mailto:EmailAddress.com&quot;&gt; for email.</span></span> <span data-ttu-id="142dd-203">Můžete použít [regulární výraz](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atribut pro ověření formátu dat.</span><span class="sxs-lookup"><span data-stu-id="142dd-203">You can use the [RegularExpression](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attribute to validate the format of the data.</span></span>

<span data-ttu-id="142dd-204">Alternativní způsob použití `DataType` atributy, můžete explicitně nastavit [ `DataFormatString` ](https://msdn.microsoft.com/en-us/library/system.string.format.aspx) hodnotu.</span><span class="sxs-lookup"><span data-stu-id="142dd-204">An alternative approach to using the `DataType` attributes, you could explicitly set a [`DataFormatString`](https://msdn.microsoft.com/en-us/library/system.string.format.aspx) value.</span></span> <span data-ttu-id="142dd-205">Následující kód ukazuje vlastnost datum vydání s řetězec formátu datum (konkrétně, &quot;d&quot;).</span><span class="sxs-lookup"><span data-stu-id="142dd-205">The following code shows the release date property with a date format string (namely, &quot;d&quot;).</span></span> <span data-ttu-id="142dd-206">To byste použili k určení, že nechcete, aby čas jako součást datum vydání.</span><span class="sxs-lookup"><span data-stu-id="142dd-206">You'd use this to specify that you don't want to time as part of the release date.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

<span data-ttu-id="142dd-207">Kompletní `Movie` třídy jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="142dd-207">The complete `Movie` class is shown below.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

<span data-ttu-id="142dd-208">Spusťte aplikaci a přejděte do `Movies` řadiče.</span><span class="sxs-lookup"><span data-stu-id="142dd-208">Run the application and browse to the `Movies` controller.</span></span> <span data-ttu-id="142dd-209">Datum vydání a ceny jsou vhodně formátovaná.</span><span class="sxs-lookup"><span data-stu-id="142dd-209">The release date and price are nicely formatted.</span></span> <span data-ttu-id="142dd-210">Následující obrázek ukazuje datum vydání a cena pomocí &quot;fr-FR&quot; jako jazykovou verzi.</span><span class="sxs-lookup"><span data-stu-id="142dd-210">The image below shows the release date and price using &quot;fr-FR&quot; as the culture.</span></span>

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

<span data-ttu-id="142dd-212">Následující obrázek ukazuje stejná data, zobrazí se výchozí jazykovou verzi (anglické US).</span><span class="sxs-lookup"><span data-stu-id="142dd-212">The image below shows the same data displayed with the default culture (English US).</span></span>

![](adding-validation-to-the-model/_static/image8.png)

<span data-ttu-id="142dd-213">V další části řady, jsme budete zkontrolujte, zda aplikace a některá vylepšení pro automaticky generované `Details` a `Delete` metody.</span><span class="sxs-lookup"><span data-stu-id="142dd-213">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="142dd-214">[Předchozí](adding-a-new-field-to-the-movie-model-and-table.md)
[další](examining-the-details-and-delete-methods.md)</span><span class="sxs-lookup"><span data-stu-id="142dd-214">[Previous](adding-a-new-field-to-the-movie-model-and-table.md)
[Next](examining-the-details-and-delete-methods.md)</span></span>