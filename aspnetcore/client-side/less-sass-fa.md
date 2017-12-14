---
title: "Menší Sass a písma Super v ASP.NET Core"
author: ardalis
description: "Další informace o použití menší Sass a písma Super v aplikacích ASP.NET Core."
keywords: "ASP.NET Core méně Sass, písmo Super, preprocesorů"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 94c988f9-95fd-425d-b37e-7f846598c6d4
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/less-sass-fa
ms.openlocfilehash: 159377300d33e98393fd6705d0fec578f8f6b735
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="a3f0b-104">Úvod do stylů aplikace s méně, Sass a písma Super v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a3f0b-104">Introduction to styling applications with Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="a3f0b-105">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a3f0b-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a3f0b-106">Uživatelé webových aplikací mít stále vysoké očekávání při rozhodování o stylu a celkové zkušenosti.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-106">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="a3f0b-107">Moderních webových aplikací často využívat bohaté nástroje a rozhraní pro definování a správu jejich vzhled a chování konzistentním způsobem.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-107">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="a3f0b-108">Rozhraní, jako [Bootstrap](http://getbootstrap.com/) můžete vyhledat celou směrem k definování společnou sadu možností rozložení pro weby a stylů.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-108">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="a3f0b-109">Většina netriviální lokality však také těžit z schopnost efektivně definovat a udržovat stylů a soubory kaskádových stylů (CSS) a také s snadný přístup k ikon bitové kopie, které pomáhají intuitivnější webového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-109">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="a3f0b-110">Kde je jazyky a nástroje, které podporují [menší](http://lesscss.org/) a [Sass](http://sass-lang.com/), a knihovny, jako jsou [písma Super](http://fontawesome.io/), mají.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-110">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="a3f0b-111">Jazyky preprocesoru šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="a3f0b-111">CSS preprocessor languages</span></span>

<span data-ttu-id="a3f0b-112">Jazyky, které jsou zkompilovány do jiných jazyků, za účelem zlepšení možností práce s základní jazyk, se označují jako preprocesorů.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-112">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="a3f0b-113">Existují dvě oblíbených preprocesorů pro šablon stylů CSS: menší a Sass.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-113">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="a3f0b-114">Tyto preprocesorů přidání funkcí do šablon stylů CSS, jako je podpora pro proměnné a vnořené pravidla, která zlepšit udržovatelnosti složitých šablony stylů.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-114">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="a3f0b-115">CSS jako jazyk je velmi základní postrádá podporu i pro něco stejně jednoduché jako proměnné, a to může být souborů CSS opakovaných a opakovaném.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-115">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="a3f0b-116">Přidávání skutečných programovací jazyk funkcí pomocí preprocesorů může pomoci snížit duplikování a poskytují lepší uspořádání pravidel stylů.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-116">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="a3f0b-117">Visual Studio poskytuje integrovanou podporu pro obě méně a Sass, jakož i rozšíření, která lze dále zvýšit vývojového prostředí při práci s tyto jazyky.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-117">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="a3f0b-118">Jako zběžný příklad jak vylepšit preprocesorů přehlednosti a udržovatelnosti informace o stylu vezměte v úvahu tato šablon stylů CSS:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-118">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

<span data-ttu-id="a3f0b-119">Pomocí menší, to může být přepsána pro odstranění všech duplikace, použití *mixin* (takže s názvem, protože umožňuje "kombinovat" vlastnosti z jedné třídy nebo do jiné sady pravidel):</span><span class="sxs-lookup"><span data-stu-id="a3f0b-119">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a><span data-ttu-id="a3f0b-120">menší</span><span class="sxs-lookup"><span data-stu-id="a3f0b-120">Less</span></span>

<span data-ttu-id="a3f0b-121">Spustí preprocesoru méně šablon stylů CSS pomocí Node.js.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-121">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="a3f0b-122">K instalaci menší použijte uzel Správce balíčků (npm) z příkazového řádku (-g znamená "globální"):</span><span class="sxs-lookup"><span data-stu-id="a3f0b-122">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="a3f0b-123">Pokud používáte Visual Studio, můžete začít pracovat s méně přidáním menší soubory do projektu a potom nakonfigurovat Gulp (nebo Grunt) pro jejich zpracování při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-123">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="a3f0b-124">Přidat *styly* složky do projektu a pak přidejte nový menší soubor s názvem *main.less* do této složky.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-124">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Přidat menší soubor](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="a3f0b-126">Po přidání, struktury složky by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-126">Once added, your folder structure should look something like this:</span></span>

![Struktura složek](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="a3f0b-128">Teď můžete přidat některé základní stylů do souboru, který se zkompiluje do šablon stylů CSS a nasazení do složky wwwroot Gulp.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-128">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="a3f0b-129">Upravit *main.less* k obsahovat následující obsah, který vytvoří z jedné základní barvy palety barev jednoduché.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-129">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

<span data-ttu-id="a3f0b-130">`@base`objekt a druhá @-prefixed položky jsou proměnné.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-130">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="a3f0b-131">Každý z nich představuje barvu.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-131">Each of them represents a color.</span></span> <span data-ttu-id="a3f0b-132">S výjimkou `@base`, jsou nastavená pomocí funkcí barev: zjednodušit ztmavení a číselníku.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-132">Except for `@base`, they are set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="a3f0b-133">Zjednodušit a ztmavení provést podstatě by očekávat; Otočení upraví odstín barvy se o počet stupňů (kolem barevné kolo).</span><span class="sxs-lookup"><span data-stu-id="a3f0b-133">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="a3f0b-134">Menší procesoru je dostatečně inteligentní ignorovat proměnné, které se nepoužívají, takže ukazují, jak tyto proměnné fungují, potřebujeme používat místo.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-134">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="a3f0b-135">Třídy `.baseColor`, popisuje počítané hodnoty těchto proměnných v souboru CSS, která je vytvořena atd.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-135">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that is produced.</span></span>

### <a name="getting-started"></a><span data-ttu-id="a3f0b-136">Začínáme</span><span class="sxs-lookup"><span data-stu-id="a3f0b-136">Getting started</span></span>

<span data-ttu-id="a3f0b-137">Vytvoření **konfigurační soubor npm** (*package.json*) ve složce projektu a upravit ho tak, aby odkazovaly `gulp` a `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-137">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

<span data-ttu-id="a3f0b-138">Nainstalujte závislosti buď na příkazovém řádku ve složce projektu, nebo v sadě Visual Studio **Průzkumníku řešení** (**závislosti > npm > Obnovení balíčků**).</span><span class="sxs-lookup"><span data-stu-id="a3f0b-138">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![VS obnovení balíčků](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="a3f0b-140">Ve složce projektu vytvořte **Gulp konfigurační soubor** (*gulpfile.js*) k definování automatizovaného procesu.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-140">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="a3f0b-141">Přidejte proměnnou v horní části souboru představují menší a menší spuštění úlohy:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-141">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="a3f0b-142">Otevřete **Průzkumník Spouštěče úloh** (**zobrazení > jiných Windows > Průzkumník Spouštěče úloh**).</span><span class="sxs-lookup"><span data-stu-id="a3f0b-142">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="a3f0b-143">Mezi úlohy, měli byste vidět novou úlohu s názvem `less`.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-143">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="a3f0b-144">Můžete chtít aktualizujte okno nástroje.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-144">You might have to refresh the window.</span></span>

<span data-ttu-id="a3f0b-145">Spustit `less` úloh a v tématu výstup podobný co je znázorněno zde:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-145">Run the `less` task, and you see output similar to what is shown here:</span></span>

![menší Spouštěče úloh](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="a3f0b-147">*Wwwroot/css* složka nyní obsahuje nový soubor *main.css*:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-147">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![vytvořit hlavní css](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="a3f0b-149">Otevřete *main.css* a vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-149">Open *main.css* and you see something like the following:</span></span>

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

<span data-ttu-id="a3f0b-150">Jednoduché stránku HTML, kterou chcete přidat *wwwroot* složku a referenční dokumentace *main.css* zobrazíte paletu barev v akci.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-150">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

<span data-ttu-id="a3f0b-151">Uvidíte, 180 stupňů číselníku na `@base` používají k vytvoření `@background` výsledkem proti barvu barevného kola `@base`:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-151">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![Příklad méně testování](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="a3f0b-153">Menší taky poskytuje podporu pro vnořené pravidla a také dotazy vnořené média.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-153">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="a3f0b-154">Například definující vnořené hierarchií, jako jsou nabídky může mít za následek podrobné pravidla šablon stylů CSS, například tyto:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-154">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

<span data-ttu-id="a3f0b-155">V ideálním případě všechny související stylová pravidla, která bude umístěna společně v rámci tohoto souboru CSS, ale v praxi není nic vynucování toto pravidlo s výjimkou konvence a případně komentáře bloku.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-155">Ideally all of the related style rules will be placed together within the CSS file, but in practice there is nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="a3f0b-156">Definování těchto stejná pravidla pomocí menší vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-156">Defining these same rules using Less looks like this:</span></span>

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

<span data-ttu-id="a3f0b-157">Všimněte si, že se v tomto případě všechny podřízené elementy `nav` jsou obsaženy v rámci svého oboru.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-157">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="a3f0b-158">Již není žádné opakování nadřazené elementy (`nav`, `li`, `a`), a počet celkový počet řádků má také vyřadit (i když některé z které je výsledkem vložení hodnoty na stejné řádky v druhém příkladu).</span><span class="sxs-lookup"><span data-stu-id="a3f0b-158">There is no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that is a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="a3f0b-159">Může být velmi užitečná, organizací, pokud chcete zobrazit všechna pravidla pro daný prvek uživatelského rozhraní v rámci explicitně ohraničené oboru, v takovém případě nastavte ze zbytek souboru ve složených závorkách.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-159">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="a3f0b-160">`&` Syntaxe je menší selektor funkce, s & představující aktuální nadřazené selektor.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-160">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="a3f0b-161">Ano, v rámci {...}</span><span class="sxs-lookup"><span data-stu-id="a3f0b-161">So, within the a {...}</span></span> <span data-ttu-id="a3f0b-162">blok, `&` představuje `a` značky a proto `&:link` je ekvivalentní `a:link`.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-162">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="a3f0b-163">Dotazy na média, velmi užitečné při vytváření přizpůsobivý návrhů, může taky výraznou přispívat k opakování a složitost v jazyce CSS.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-163">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="a3f0b-164">Menší umožňuje dotazy na média se Nested v rámci třídy, aby definice celou třídu není potřeba zopakovat v rámci různých nejvyšší úrovně `@media` elementy.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-164">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="a3f0b-165">Zde je ukázka, šablon stylů CSS přizpůsobivý nabídky:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-165">For example, here is CSS for a responsive menu:</span></span>

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="a3f0b-166">To je možné lépe definovat v menší jako:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-166">This can be better defined in Less as:</span></span>

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="a3f0b-167">Jiné funkce méně jsme již viděli je podpora pro matematické operace a atributy stylu zkonstruovat z předdefinovaných proměnných.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-167">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="a3f0b-168">Díky aktualizace související styly mnohem jednodušší, protože je možné upravit proměnnou základní a všechny závislé hodnoty automaticky změnit.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-168">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="a3f0b-169">Soubory šablon stylů CSS, hlavně pro velké weby (a hlavně v případě, že se používají dotazy na média), zpravidla získat poměrně značnou časem provedení práce s nimi nepraktické.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-169">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="a3f0b-170">Menší soubory se může definovat samostatně, pak vyžádat pomocí `@import` direktivy.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-170">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="a3f0b-171">Menší mohou sloužit také k importu jednotlivých souborů CSS, taky v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-171">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="a3f0b-172">*Mixins* může přijmout parametry a menší podporuje podmíněnou logiku ve formě mixin chrání, které poskytují deklarativní způsob, jak definovat při určitých mixins vstoupila v platnost.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-172">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="a3f0b-173">Běžně používá pro mixin chrání je upravit barvy založené na tom, jak slabá nebo tmavý zdrojové barvy.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-173">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="a3f0b-174">Zadané mixin, který přijme parametr pro barvu, mixin ochranu lze použít k úpravě mixin na základě této barvy:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-174">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

<span data-ttu-id="a3f0b-175">Zadané naše aktuální `@base` hodnotu `#663333`, menší tento skript vytvoří následující CSS:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-175">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="a3f0b-176">Menší poskytuje celou řadu dalších funkcí, ale to by měl získali přehled napájení tohoto předběžného zpracování jazyka.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-176">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="a3f0b-177">Sass</span><span class="sxs-lookup"><span data-stu-id="a3f0b-177">Sass</span></span>

<span data-ttu-id="a3f0b-178">Sass je podobná menší, zajištění podpory pro mnoho stejných funkcí, ale syntaxí mírně lišit.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-178">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="a3f0b-179">Je vytvořen pomocí Ruby místo JavaScript, a proto nemá různé instalační požadavky.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-179">It is built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="a3f0b-180">Původní jazyk Sass nepoužili složené závorky nebo středníky, ale místo definuje obor pomocí mezer a odsazení.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-180">The original Sass language did not use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="a3f0b-181">Ve verzi 3 Sass byla zavedená Nová syntaxe, **SCSS** (dále jen "Sassy CSS").</span><span class="sxs-lookup"><span data-stu-id="a3f0b-181">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="a3f0b-182">SCSS je podobná šablon stylů CSS v, že ignoruje úrovně odsazení a prázdný znak a místo toho používá středníky a složené závorky.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-182">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="a3f0b-183">K instalaci Sass, obvykle můžete by nejprve nainstalujte Ruby (předinstalované na Mac) a spusťte:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-183">To install Sass, typically you would first install Ruby (pre-installed on Mac), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="a3f0b-184">Ale pokud používáte Visual Studio, můžete začít s Sass prakticky stejným způsobem jako stejně jako s menší.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-184">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="a3f0b-185">Otevřete *package.json* a přidejte balíček "gulp-sass" `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-185">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="a3f0b-186">V dalším kroku změnit *gulpfile.js* k přidání proměnné sass a úlohu pro kompilaci Sass soubory a umístěte do složky wwwroot výsledky:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-186">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="a3f0b-187">Teď můžete přidat soubor Sass *main2.scss* k *styly* složku v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-187">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![Přidejte soubor scss](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="a3f0b-189">Otevřete *main2.scss* a přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-189">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="a3f0b-190">Uložte všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-190">Save all of your files.</span></span> <span data-ttu-id="a3f0b-191">Nyní když aktualizujete **Průzkumník Spouštěče úloh**, uvidíte `sass` úloh.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-191">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="a3f0b-192">Spusťte ji a hledat v */wwwroot/css* složky.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-192">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="a3f0b-193">Nyní je *main2.css* soubor s tyto obsah:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-193">There is now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="a3f0b-194">Sass podporuje vnoření téměř stejný se, že menší nemá, poskytuje podobné výhody.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-194">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="a3f0b-195">Soubory je možné rozdělit podle funkce a zahrnuté pomocí `@import` – direktiva:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-195">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="a3f0b-196">Sass podporuje mixins taky pomocí `@mixin` – klíčové slovo se definovat a `@include` pro zahrnutí, jako v následujícím příkladě z [sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="a3f0b-196">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="a3f0b-197">Kromě mixins Sass podporuje také koncepci dědičnosti, povolení jednu třídu k jiné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-197">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="a3f0b-198">Je koncepčně podobá mixin, ale má za následek méně kódu šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-198">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="a3f0b-199">Dosahuje pomocí `@extend` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-199">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="a3f0b-200">Můžete vyzkoušet na mixins, přidejte následující příkaz pro vaše *main2.scss* souboru:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-200">To try out mixins, add the following to your *main2.scss* file:</span></span>

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="a3f0b-201">Zkontrolujte výstup v *main2.css* po spuštění `sass` úloh v **Průzkumník Spouštěče úloh**:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-201">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="a3f0b-202">Všimněte si, že jsou všechny běžné vlastnosti výstrahy mixin opakována v každé třídě.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-202">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="a3f0b-203">Mixin nebyla dobrou práci, což pomáhá eliminovat duplikace v době vývoje, ale je stále vytváří šablon stylů CSS s velkým množstvím duplikace v, což vede k větší než potřebné soubory šablon stylů CSS – potenciální problém výkonu.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-203">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="a3f0b-204">Nyní nahraďte výstrahy mixin s `.alert` třídy a změňte `@include` k `@extend` (Nezapomeňte rozšířit `.alert`, nikoli `alert`):</span><span class="sxs-lookup"><span data-stu-id="a3f0b-204">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="a3f0b-205">Spusťte Sass ještě jednou a zkontrolujte výsledné CSS:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-205">Run Sass once more, and examine the resulting CSS:</span></span>

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="a3f0b-206">Nyní jsou definovány vlastnosti jenom jako podle potřeby a lépe se vygeneruje šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-206">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="a3f0b-207">Sass také obsahuje funkce a operace podmíněnou logiku, podobně jako menší.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-207">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="a3f0b-208">Ve skutečnosti jsou velmi podobné možnosti dvou jazyků.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-208">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="a3f0b-209">Menší nebo Sass?</span><span class="sxs-lookup"><span data-stu-id="a3f0b-209">Less or Sass?</span></span>

<span data-ttu-id="a3f0b-210">Je stále shody o tom, jestli je obecně lepší použít menší nebo Sass, (nebo i jestli se má přednost původní Sass nebo novější syntaxe SCSS v rámci Sass).</span><span class="sxs-lookup"><span data-stu-id="a3f0b-210">There is still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="a3f0b-211">Pravděpodobně je nejdůležitější rozhodnutí **použijte jednu z těchto nástrojů**, nikoli jen kódování souborů CSS dlaně.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-211">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="a3f0b-212">Jakmile jste provedli, které rozhodnutím, jak méně a Sass se velmi dobře hodí.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-212">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="a3f0b-213">Skvělá písma</span><span class="sxs-lookup"><span data-stu-id="a3f0b-213">Font Awesome</span></span>

<span data-ttu-id="a3f0b-214">Kromě preprocesorů šablon stylů CSS je to Super písma jiné skvělým zdrojem pro stylů moderních webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-214">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="a3f0b-215">Písmo Super je sada nástrojů, který poskytuje více než 500 škálovatelné vektoru ikony, které můžete volně používat ve vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-215">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="a3f0b-216">Byl původně navržený na práci s Bootstrap však nemá žádné závislosti na dané platformy nebo na žádné knihovny jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-216">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="a3f0b-217">Nejjednodušší způsob, jak začít pracovat s písma Super je přidat odkaz na něj pomocí veřejného doručování obsahu (CDN) síťové umístění:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-217">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="a3f0b-218">Můžete také ho přidat do projektu sady Visual Studio přidejte jej do "závislosti" v *bower.json*:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-218">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

<span data-ttu-id="a3f0b-219">Až budete mít odkaz na písmo Super na stránce, můžete přidat ikony do vaší aplikace s použitím písma Super třídy, obvykle předponu "DM-" pro vložené HTML elementy (například `<span>` nebo `<i>`).</span><span class="sxs-lookup"><span data-stu-id="a3f0b-219">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="a3f0b-220">Například můžete přidat ikony jednoduché seznamy a nabídky pomocí kódu takto:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-220">For example, you can add icons to simple lists and menus using code like this:</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

<span data-ttu-id="a3f0b-221">Tímto se vytvoří následující v prohlížeči – Poznámka: na ikonu vedle jednotlivých položek:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-221">This produces the following in the browser - note the icon beside each item:</span></span>

![ikony v seznamu](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="a3f0b-223">Můžete zobrazit úplný seznam dostupných ikon:</span><span class="sxs-lookup"><span data-stu-id="a3f0b-223">You can view a complete list of the available icons here:</span></span>

<span data-ttu-id="a3f0b-224">http://fontawesome.IO/icons/</span><span class="sxs-lookup"><span data-stu-id="a3f0b-224">http://fontawesome.io/icons/</span></span>

## <a name="summary"></a><span data-ttu-id="a3f0b-225">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a3f0b-225">Summary</span></span>

<span data-ttu-id="a3f0b-226">Moderní webové aplikace stále potřebují reakce, plynulá práce návrhy, které jsou čistá, intuitivní a snadno použít z různých zařízení.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-226">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="a3f0b-227">Správa složitosti šablony stylů CSS, potřebné k dosažení těchto cílů se nejlépe provádí pomocí preprocesoru jako menší nebo Sass.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-227">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="a3f0b-228">Kromě toho sad nástrojů, jako je písmo Super rychle zadejte dobře známé ikony pro textovou navigační nabídky a tlačítka, zvýšení celkové uživatelské prostředí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a3f0b-228">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>