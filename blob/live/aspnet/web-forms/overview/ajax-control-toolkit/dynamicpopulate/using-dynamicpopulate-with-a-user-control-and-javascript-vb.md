---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: "DynamicPopulate pomocí uživatelského ovládacího prvku a JavaScript (VB) | Microsoft Docs"
author: wenz
description: "DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 69066edc18b58cc3148a738fe8dd48cb92a84f11
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="98d4d-103">DynamicPopulate pomocí uživatelského ovládacího prvku a JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="98d4d-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>
====================
<span data-ttu-id="98d4d-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="98d4d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="98d4d-105">[Stáhněte si kód](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="98d4d-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="98d4d-106">DynamicPopulate ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky.</span><span class="sxs-lookup"><span data-stu-id="98d4d-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="98d4d-107">Je také možné aktivovat naplnění pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="98d4d-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="98d4d-108">Zvláštní pozornost ale musí být provedeny, když rozšiřujícího objektu se nachází v uživatelského ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="98d4d-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="98d4d-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="98d4d-109">Overview</span></span>

<span data-ttu-id="98d4d-110">`DynamicPopulate` Ovládacího prvku ASP.NET AJAX Control Toolkit volání webové služby (nebo metodu stránky) a výplní výslednou hodnotu do cílový ovládací prvek na stránce bez aktualizaci stránky.</span><span class="sxs-lookup"><span data-stu-id="98d4d-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="98d4d-111">Je také možné aktivovat naplnění pomocí vlastního kódu jazyka JavaScript na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="98d4d-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="98d4d-112">Zvláštní pozornost ale musí být provedeny, když rozšiřujícího objektu se nachází v uživatelského ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="98d4d-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="98d4d-113">Kroky</span><span class="sxs-lookup"><span data-stu-id="98d4d-113">Steps</span></span>

<span data-ttu-id="98d4d-114">Je třeba nejprve všech, webové služby ASP.NET, která implementuje metoda má být volána `DynamicPopulateExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="98d4d-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="98d4d-115">Webová služba implementuje metodu `getDate()` , očekává jeden argument typu řetězec, nazývá `contextKey`, vzhledem k tomu, `DynamicPopulate` řízení odešle jednu část kontextu informací s každou volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="98d4d-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="98d4d-116">Zde je kód (soubory `DynamicPopulate.vb.asmx`) který načte aktuální datum v jednom ze tří formátů:</span><span class="sxs-lookup"><span data-stu-id="98d4d-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="98d4d-117">V dalším kroku, vytvořte nový uživatelský ovládací prvek (`.ascx` soubor), označen následující deklarace v jeho první řádek:</span><span class="sxs-lookup"><span data-stu-id="98d4d-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="98d4d-118">A &lt; `label` &gt; element se použije k zobrazení dat ze serveru.</span><span class="sxs-lookup"><span data-stu-id="98d4d-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="98d4d-119">Také v řídicím souboru uživatele, budeme používat přepínače tři, každý z nich představující mezi tři možné data formátů podporovaných webovou službu.</span><span class="sxs-lookup"><span data-stu-id="98d4d-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="98d4d-120">Když uživatel klikne na jednom z přepínačů, spustí prohlížeč kód JavaScript, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="98d4d-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="98d4d-121">Tento kód přistupuje `DynamicPopulateExtender` (Nedělejte si starosti o neobvyklé ID ještě to se budeme později na) a aktivuje dynamické naplnění s daty.</span><span class="sxs-lookup"><span data-stu-id="98d4d-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="98d4d-122">V kontextu aktuálního přepínač `this.value` odkazuje na jeho hodnotu, která je `format1`, `format2` nebo `format3` přesně co webové metody očekává.</span><span class="sxs-lookup"><span data-stu-id="98d4d-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="98d4d-123">Jediné, co chybí v ovládacím prvku uživatele ještě není `DynamicPopulateExtender` ovládací prvek, který odkazuje rádiová tlačítka na webovou službu.</span><span class="sxs-lookup"><span data-stu-id="98d4d-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="98d4d-124">Znovu si poznamenejte neobvyklé ID použité v ovládacím prvku: `mcd1$myDate` místo `myDate`.</span><span class="sxs-lookup"><span data-stu-id="98d4d-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="98d4d-125">Dříve, kód jazyka JavaScript používá `mcd1_dpe1` k přístupu `DynamicPopulateExtender` místo `dpe1`. Tato strategie vytváření názvů je speciální požadavky při použití `DynamicPopulateExtender` v rámci uživatelského ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="98d4d-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="98d4d-126">Kromě toho budete muset Vložení kontrolních uživatele konkrétní způsobem, aby bylo fungovat.</span><span class="sxs-lookup"><span data-stu-id="98d4d-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="98d4d-127">Vytvořte novou stránku ASP.NET a zaregistrovat předponu značky uživatelského ovládacího prvku, který jste právě implementovali:</span><span class="sxs-lookup"><span data-stu-id="98d4d-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="98d4d-128">Pak musíte uvést prvku ASP.NET AJAX `ScriptManager` ovládacího prvku na novou stránku:</span><span class="sxs-lookup"><span data-stu-id="98d4d-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="98d4d-129">Nakonec přidejte uživatelského ovládacího prvku na stránku.</span><span class="sxs-lookup"><span data-stu-id="98d4d-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="98d4d-130">Budete muset nastavit jeho `ID` atribut (a `runat="server"`, samozřejmě), ale máte také nastavit na konkrétní název: `mcd1` vzhledem k tomu, že toto je Předpona použitá v rámci uživatelského ovládacího prvku pro přístup k ní pomocí jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98d4d-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="98d4d-131">A je to!</span><span class="sxs-lookup"><span data-stu-id="98d4d-131">And that's it!</span></span> <span data-ttu-id="98d4d-132">Stránce chová podle očekávání: uživatel klikne na na přepínacích tlačítek, ovládací prvek v sadě nástrojů volání webové služby a zobrazí aktuální datum v požadovaném formátu.</span><span class="sxs-lookup"><span data-stu-id="98d4d-132">The page behaves as expected: A user clicks on on of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="98d4d-133">[![Přepínací tlačítka se nacházejí v uživatelského ovládacího prvku](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="98d4d-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="98d4d-134">Přepínací tlačítka se nacházejí v uživatelského ovládacího prvku ([Kliknutím zobrazit obrázek v plné velikosti](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="98d4d-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="98d4d-135">Předchozí</span><span class="sxs-lookup"><span data-stu-id="98d4d-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)