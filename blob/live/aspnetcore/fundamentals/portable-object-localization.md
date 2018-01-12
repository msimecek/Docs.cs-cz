---
title: "Konfigurace přenosné objekt lokalizace"
author: sebastienros
description: "Tento článek představuje soubory přenosné objektů a popisuje kroky pro jejich používání v aplikaci ASP.NET Core s framework Orchard jádra."
keywords: "ASP.NET Core, lokalizace, jazykovou verzi, jazyka, přenosné objektu"
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: 4fa73ae08b10217de657681a27f6097fc3443737
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="a64ec-104">Lokalizace přenosné objekt nakonfigurovat Orchard jádra</span><span class="sxs-lookup"><span data-stu-id="a64ec-104">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="a64ec-105">Podle [Sébastien železnic](https://github.com/sebastienros) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="a64ec-105">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="a64ec-106">Tento článek vás provede kroky pro použití přenosných objektu (PO) souborů v aplikaci ASP.NET Core s [Orchard základní](https://github.com/OrchardCMS/OrchardCore) framework.</span><span class="sxs-lookup"><span data-stu-id="a64ec-106">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="a64ec-107">**Poznámka:** Orchard jádra není produktu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a64ec-107">**Note:** Orchard Core is not a Microsoft product.</span></span> <span data-ttu-id="a64ec-108">V důsledku toho společnost Microsoft poskytuje žádná podpora pro tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="a64ec-108">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="a64ec-109">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a64ec-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="a64ec-110">Co je soubor SP?</span><span class="sxs-lookup"><span data-stu-id="a64ec-110">What is a PO file?</span></span>

<span data-ttu-id="a64ec-111">Soubory SP distribuují jako textové soubory s přeloženými řetězci pro daný jazyk.</span><span class="sxs-lookup"><span data-stu-id="a64ec-111">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="a64ec-112">Některé výhody používání SP soubory místo toho *RESX* soubory zahrnují:</span><span class="sxs-lookup"><span data-stu-id="a64ec-112">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="a64ec-113">Soubory SP podporovat pluralizační; *RESX* soubory nepodporují pluralizační.</span><span class="sxs-lookup"><span data-stu-id="a64ec-113">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="a64ec-114">SP soubory nejsou kompilovány jako *RESX* soubory.</span><span class="sxs-lookup"><span data-stu-id="a64ec-114">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="a64ec-115">Jako takový nejsou vyžaduje speciální kroky nástrojů a sestavení.</span><span class="sxs-lookup"><span data-stu-id="a64ec-115">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="a64ec-116">Soubory SP fungují dobře u spolupráce online nástrojů pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="a64ec-116">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="a64ec-117">Příklad</span><span class="sxs-lookup"><span data-stu-id="a64ec-117">Example</span></span>

<span data-ttu-id="a64ec-118">Tady je ukázkový soubor SP obsahující překlad pro dva řetězce ve francouzštině, včetně s jeho množném čísle:</span><span class="sxs-lookup"><span data-stu-id="a64ec-118">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="a64ec-119">*FR.po*</span><span class="sxs-lookup"><span data-stu-id="a64ec-119">*fr.po*</span></span>

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

<span data-ttu-id="a64ec-120">Tento příklad používá následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="a64ec-120">This example uses the following syntax:</span></span>

- <span data-ttu-id="a64ec-121">`#:`: Komentář označující kontextu řetězec, který má být přeložit.</span><span class="sxs-lookup"><span data-stu-id="a64ec-121">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="a64ec-122">Do jednoho řetězce může přeložit odlišně v závislosti na tom, kde se používá.</span><span class="sxs-lookup"><span data-stu-id="a64ec-122">The same string might be translated differently depending on where it is being used.</span></span>
- <span data-ttu-id="a64ec-123">`msgid`: Nepřeložený řetězec.</span><span class="sxs-lookup"><span data-stu-id="a64ec-123">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="a64ec-124">`msgstr`: Přeložené řetězce.</span><span class="sxs-lookup"><span data-stu-id="a64ec-124">`msgstr`: The translated string.</span></span>

<span data-ttu-id="a64ec-125">V případě podpory pluralizační lze definovat další položky.</span><span class="sxs-lookup"><span data-stu-id="a64ec-125">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="a64ec-126">`msgid_plural`: Nepřeložený množném čísle řetězec.</span><span class="sxs-lookup"><span data-stu-id="a64ec-126">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="a64ec-127">`msgstr[0]`: Přeložené řetězce pro případ 0.</span><span class="sxs-lookup"><span data-stu-id="a64ec-127">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="a64ec-128">`msgstr[N]`: Přeložené řetězce pro případu N.</span><span class="sxs-lookup"><span data-stu-id="a64ec-128">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="a64ec-129">Naleznete specifikaci souboru SP [zde](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="a64ec-129">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="a64ec-130">Konfigurace SP podpora souborů v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a64ec-130">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="a64ec-131">Tento příklad vychází z aplikace ASP.NET MVC základní vygeneroval ze šablony projektů Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a64ec-131">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="a64ec-132">Odkazování na balíček</span><span class="sxs-lookup"><span data-stu-id="a64ec-132">Referencing the package</span></span>

<span data-ttu-id="a64ec-133">Přidat odkaz na `OrchardCore.Localization.Core` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="a64ec-133">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="a64ec-134">Je k dispozici na [MyGet](https://www.myget.org/) u následující zdroje balíčků: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="a64ec-134">It is available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="a64ec-135">*.Csproj* soubor nyní obsahuje řádek podobný následujícímu (číslo verze se liší):</span><span class="sxs-lookup"><span data-stu-id="a64ec-135">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="a64ec-136">Registrace služby</span><span class="sxs-lookup"><span data-stu-id="a64ec-136">Registering the service</span></span>

<span data-ttu-id="a64ec-137">Přidat požadované služby, aby `ConfigureServices` metodu *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a64ec-137">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="a64ec-138">Přidejte požadované middlewaru, který má `Configure` metodu *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a64ec-138">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="a64ec-139">Přidejte následující kód do zobrazení Razor výběru.</span><span class="sxs-lookup"><span data-stu-id="a64ec-139">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="a64ec-140">*About.cshtml* se používá v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="a64ec-140">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="a64ec-141">`IViewLocalizer` Instance je vložit a použita k převodu text "Hello, world!".</span><span class="sxs-lookup"><span data-stu-id="a64ec-141">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="a64ec-142">Vytvoření souboru SP</span><span class="sxs-lookup"><span data-stu-id="a64ec-142">Creating a PO file</span></span>

<span data-ttu-id="a64ec-143">Vytvořte soubor s názvem  *<culture code>.po* v kořenové složce aplikace.</span><span class="sxs-lookup"><span data-stu-id="a64ec-143">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="a64ec-144">V tomto příkladu je název souboru *fr.po* vzhledem k tomu, že se používá francouzštině:</span><span class="sxs-lookup"><span data-stu-id="a64ec-144">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="a64ec-145">Tento soubor obsahuje řetězec přeložit i řetězec francouzština přeložit.</span><span class="sxs-lookup"><span data-stu-id="a64ec-145">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="a64ec-146">Překlady vraťte se k jejich nadřazené jazykové verzi, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a64ec-146">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="a64ec-147">V tomto příkladu *fr.po* soubor se používá, pokud je požadovaná jazyková verze `fr-FR` nebo `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="a64ec-147">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="a64ec-148">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="a64ec-148">Testing the application</span></span>

<span data-ttu-id="a64ec-149">Spusťte aplikaci a přejděte na adresu URL `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="a64ec-149">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="a64ec-150">Text **Hello, world!**</span><span class="sxs-lookup"><span data-stu-id="a64ec-150">The text **Hello world!**</span></span> <span data-ttu-id="a64ec-151">Zobrazí se.</span><span class="sxs-lookup"><span data-stu-id="a64ec-151">is displayed.</span></span>

<span data-ttu-id="a64ec-152">Přejděte na adresu URL `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="a64ec-152">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="a64ec-153">Text **Bonjour the World!**</span><span class="sxs-lookup"><span data-stu-id="a64ec-153">The text **Bonjour le monde!**</span></span> <span data-ttu-id="a64ec-154">Zobrazí se.</span><span class="sxs-lookup"><span data-stu-id="a64ec-154">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="a64ec-155">Pluralizační</span><span class="sxs-lookup"><span data-stu-id="a64ec-155">Pluralization</span></span>

<span data-ttu-id="a64ec-156">Soubory SP podporovat pluralizační formulářů, což je užitečné, když se do jednoho řetězce potřebuje přeložit různě v závislosti na kardinalitu.</span><span class="sxs-lookup"><span data-stu-id="a64ec-156">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="a64ec-157">Tato úloha se provádí složitý fakt, že každý jazyk definuje vlastní pravidla vyberte řetězec, který se má použít podle nastavení kardinality.</span><span class="sxs-lookup"><span data-stu-id="a64ec-157">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="a64ec-158">Balíček Orchard lokalizace poskytuje rozhraní API pro vyvolání tyto různé množném čísle formuláře automaticky.</span><span class="sxs-lookup"><span data-stu-id="a64ec-158">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="a64ec-159">Vytváření pluralizační SP souborů</span><span class="sxs-lookup"><span data-stu-id="a64ec-159">Creating pluralization PO files</span></span>

<span data-ttu-id="a64ec-160">Přidejte do něj následující obsah do výše uvedených *fr.po* souboru:</span><span class="sxs-lookup"><span data-stu-id="a64ec-160">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="a64ec-161">V tématu [co je soubor SP?](#what-is-a-po-file) pro vysvětlení, co každá položka v tomto příkladu představuje.</span><span class="sxs-lookup"><span data-stu-id="a64ec-161">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="a64ec-162">Přidání jazyk pomocí různých pluralizační formulářů</span><span class="sxs-lookup"><span data-stu-id="a64ec-162">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="a64ec-163">V předchozím příkladu byly použity řetězce angličtinu a francouzštinu.</span><span class="sxs-lookup"><span data-stu-id="a64ec-163">English and French strings were used in the previous example.</span></span> <span data-ttu-id="a64ec-164">Angličtinu a francouzštinu máte jenom dva pluralizační formuláře a sdílet stejná pravidla formulář, který je, že kardinalitu jednoho je namapována na první množném čísle.</span><span class="sxs-lookup"><span data-stu-id="a64ec-164">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="a64ec-165">Další Mohutnost je namapována na druhý množném čísle.</span><span class="sxs-lookup"><span data-stu-id="a64ec-165">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="a64ec-166">Ne všechny jazyky sdílet stejná pravidla.</span><span class="sxs-lookup"><span data-stu-id="a64ec-166">Not all languages share the same rules.</span></span> <span data-ttu-id="a64ec-167">Tento koncept je znázorněn s České jazyk, který má tři množném čísle forms.</span><span class="sxs-lookup"><span data-stu-id="a64ec-167">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="a64ec-168">Vytvořte `cs.po` následujícím způsobem a Všimněte si, jak pluralizační potřebuje tři různé překlady:</span><span class="sxs-lookup"><span data-stu-id="a64ec-168">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="a64ec-169">Přijmout České lokalizace, přidejte `"cs"` do seznamu podporovaných jazykových verzí v `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="a64ec-169">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

<span data-ttu-id="a64ec-170">Upravit *Views/Home/About.cshtml* souboru k vykreslení množném čísle, lokalizované řetězce pro několik cardinalities:</span><span class="sxs-lookup"><span data-stu-id="a64ec-170">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="a64ec-171">**Poznámka:** ve scénáři skutečném světě by proměnnou použít k reprezentování počet.</span><span class="sxs-lookup"><span data-stu-id="a64ec-171">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="a64ec-172">Zde jsme zopakujte stejný kód s tři různé hodnoty vystavit velmi konkrétní případ.</span><span class="sxs-lookup"><span data-stu-id="a64ec-172">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="a64ec-173">Při přepínání jazykové verze, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="a64ec-173">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="a64ec-174">Pro `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="a64ec-174">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="a64ec-175">Pro `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="a64ec-175">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="a64ec-176">Pro `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="a64ec-176">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="a64ec-177">Všimněte si, že pro Česká jazyková verze, tři překlady se liší.</span><span class="sxs-lookup"><span data-stu-id="a64ec-177">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="a64ec-178">Francouzská a anglické jazykové verze sdílet stejné konstrukce pro dvě poslední přeložené řetězce.</span><span class="sxs-lookup"><span data-stu-id="a64ec-178">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="a64ec-179">Pokročilé úlohy</span><span class="sxs-lookup"><span data-stu-id="a64ec-179">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="a64ec-180">Contextualizing řetězce</span><span class="sxs-lookup"><span data-stu-id="a64ec-180">Contextualizing strings</span></span>

<span data-ttu-id="a64ec-181">Aplikace často obsahují řetězce k převodu na několika místech.</span><span class="sxs-lookup"><span data-stu-id="a64ec-181">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="a64ec-182">Do jednoho řetězce může mít různé posunutí v určitých umístění v rámci aplikace (zobrazení syntaxe Razor nebo soubory třídy).</span><span class="sxs-lookup"><span data-stu-id="a64ec-182">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="a64ec-183">Soubor SP podporuje představu o soubor kontext, který slouží ke kategorizaci reprezentované řetězec.</span><span class="sxs-lookup"><span data-stu-id="a64ec-183">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="a64ec-184">Použitím odlišného kontextu, soubor, řetězec lze přeložit odlišně, v závislosti na kontextu souboru (nebo nedostatečná kontextu souboru).</span><span class="sxs-lookup"><span data-stu-id="a64ec-184">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="a64ec-185">Službu lokalizaci SP použijte název úplné třídy nebo zobrazení, která se používá při překladu řetězec.</span><span class="sxs-lookup"><span data-stu-id="a64ec-185">The PO localization services use the name of the full class or the view that is used when translating a string.</span></span> <span data-ttu-id="a64ec-186">To se provádí nastavením hodnoty na `msgctxt` položku.</span><span class="sxs-lookup"><span data-stu-id="a64ec-186">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="a64ec-187">Vezměte v úvahu menší přidání na předchozí *fr.po* příklad.</span><span class="sxs-lookup"><span data-stu-id="a64ec-187">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="a64ec-188">Zobrazení syntaxe Razor nacházející se v *Views/Home/About.cshtml* může být definováno jako kontext souboru nastavením vyhrazený `msgctxt` položka hodnotu:</span><span class="sxs-lookup"><span data-stu-id="a64ec-188">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="a64ec-189">Pomocí `msgctxt` nastavit jako takový, text dochází k převodu při navigaci na `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="a64ec-189">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="a64ec-190">Překlad nedojde při navigaci na `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="a64ec-190">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="a64ec-191">Pokud žádná konkrétní položka je nalezena shoda s kontextem daný soubor, Orchard základní nouzový mechanismus, který vyhledá odpovídající soubor SP bez kontextu.</span><span class="sxs-lookup"><span data-stu-id="a64ec-191">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="a64ec-192">Za předpokladu, že se žádný konkrétní soubor kontext definované pro *Views/Home/Contact.cshtml*, navigační k `/Home/Contact?culture=fr-FR` načte soubor SP jako například:</span><span class="sxs-lookup"><span data-stu-id="a64ec-192">Assuming there is no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="a64ec-193">Změna umístění souborů PO</span><span class="sxs-lookup"><span data-stu-id="a64ec-193">Changing the location of PO files</span></span>

<span data-ttu-id="a64ec-194">Výchozí umístění souborů PO lze změnit v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a64ec-194">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="a64ec-195">V tomto příkladu jsou soubory SP načteny z *lokalizace* složky.</span><span class="sxs-lookup"><span data-stu-id="a64ec-195">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="a64ec-196">Implementace vlastní logiky pro hledání lokalizační soubory</span><span class="sxs-lookup"><span data-stu-id="a64ec-196">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="a64ec-197">Potřeby složitější logikou, vyhledejte SP soubory `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` rozhraní můžete implementovat a zaregistrován jako služba.</span><span class="sxs-lookup"><span data-stu-id="a64ec-197">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="a64ec-198">To je užitečné, když SP soubory mohou být uloženy v různých umístěních, nebo pokud soubory lze nalézt v rámci hierarchie složek.</span><span class="sxs-lookup"><span data-stu-id="a64ec-198">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="a64ec-199">Pomocí pluralized jazyk</span><span class="sxs-lookup"><span data-stu-id="a64ec-199">Using a different default pluralized language</span></span>

<span data-ttu-id="a64ec-200">Balíček obsahuje `Plural` rozšíření metod, které jsou specifické pro dva množném čísle formuláře.</span><span class="sxs-lookup"><span data-stu-id="a64ec-200">The package includes a `Plural` extension method that is specific to two plural forms.</span></span> <span data-ttu-id="a64ec-201">Pro jazyky, které vyžadují další množném vytvořte metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a64ec-201">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="a64ec-202">Pomocí metody rozšíření, nebude nutné zadávat jakékoli lokalizace souboru pro výchozí jazyk &mdash; původní řetězce jsou již k dispozici přímo v kódu.</span><span class="sxs-lookup"><span data-stu-id="a64ec-202">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="a64ec-203">Můžete použít další Obecné `Plural(int count, string[] pluralForms, params object[] arguments)` přetížení, které přijímá pole řetězců překladů.</span><span class="sxs-lookup"><span data-stu-id="a64ec-203">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>