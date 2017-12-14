---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: "Sbalení a rozšiřování panely z jazyka JavaScript (C#) | Microsoft Docs"
author: wenz
description: "CollapsiblePanel ovládacího prvku ASP.NET AJAX Control Toolkit rozšiřuje panelu a poskytuje schopnost Sbalit obsah a rozbalte ho..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 666f3e212ccdd5b26b466f4672134ce751dc5dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="2a6b1-103">Sbalení a rozšiřování panely z jazyka JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="2a6b1-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>
====================
<span data-ttu-id="2a6b1-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2a6b1-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2a6b1-105">[Stáhněte si kód](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2a6b1-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="2a6b1-106">CollapsiblePanel ovládacího prvku ASP.NET AJAX Control Toolkit rozšiřuje panelu a poskytuje schopnost Sbalit obsah a rozbalte ho znovu.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="2a6b1-107">Tyto dvě akce se dá taky spustit z vlastního kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="2a6b1-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="2a6b1-108">Overview</span></span>

<span data-ttu-id="2a6b1-109">CollapsiblePanel ovládacího prvku ASP.NET AJAX Control Toolkit rozšiřuje panelu a poskytuje schopnost Sbalit obsah a rozbalte ho znovu.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="2a6b1-110">Tyto dvě akce se dá taky spustit z vlastního kódu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="2a6b1-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="2a6b1-111">Steps</span></span>

<span data-ttu-id="2a6b1-112">Nejdřív všech, vytvořte novou stránku ASP.NET a zahrnout `ScriptManager` v rámci ten `<form>` elementu.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="2a6b1-113">Tento kód načte knihovny ASP.NET AJAX, který je požadován Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="2a6b1-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="2a6b1-114">Pak vytvořte panelu s nějaký text tak, aby si můžete prohlédnout účinek rozbalit nebo sbalit:</span><span class="sxs-lookup"><span data-stu-id="2a6b1-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="2a6b1-115">Jak je vidět na panelu odkazuje na třídu CSS, která se zobrazí zde (a v podstatě definuje barvu pozadí a šířka panelu):</span><span class="sxs-lookup"><span data-stu-id="2a6b1-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="2a6b1-116">`CollapsiblePanelExtender` Vyžaduje ovládací prvek `TargetControlID` atributů tak, aby sada nástrojů zná panelu sbalit nebo rozšířit na žádost:</span><span class="sxs-lookup"><span data-stu-id="2a6b1-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="2a6b1-117">Bohužel rozšiřujícího objektu aktuálně nevystavuje konkrétní rozhraní API pro sbalení nebo rozbalení panelu, ale některé nedokumentovanými metody provede.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="2a6b1-118">Nejprve přidejte tři tlačítka HTML na stránku, který se potom aktivuje JavaScript na straně klienta sbalit nebo rozbalte obsah panelu:</span><span class="sxs-lookup"><span data-stu-id="2a6b1-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="2a6b1-119">V kódu jazyka JavaScript na straně klienta (práce s `<script type="text/javascript">`), `$find()` metoda se musí použít pro přístup k `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="2a6b1-120">`$find("cpe")`Vrátí odkaz na něj.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="2a6b1-121">Odtud na konkrétní metody vyřeší na prováděné úloze.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="2a6b1-122">Metoda pro otevření (rozšíření) se nazývá panelu `_doOpen()`; následující kód implementuje `doOpen()` funkce volána při kliknutí na první tlačítko:</span><span class="sxs-lookup"><span data-stu-id="2a6b1-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="2a6b1-123">Zavření nebo sbalení panelu, `_doClose()` metoda je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="2a6b1-124">Proto když uživatel klikne na tlačítko druhé, se nazývá následující kód v JavaScriptu:</span><span class="sxs-lookup"><span data-stu-id="2a6b1-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="2a6b1-125">Tlačítko třetí přepne stav panelu: z sbaleny do rozšiřovat a naopak.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="2a6b1-126">`CollapsiblePanelExtender` Zpřístupní `toggle()` metoda, která zajišťuje přesně který: obrátí stav panelu.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="2a6b1-127">Je však také jiné přístup (která vnitřně používá `toggle()` metoda): `get_Collapsed()` metodu `CollapsiblePanelExtender()` nám oznamuje, zda je panel sbalený nebo ne.</span><span class="sxs-lookup"><span data-stu-id="2a6b1-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="2a6b1-128">V závislosti na návratovou hodnotu této funkce, panelu je pak buď rozšířit (`_doOpen()` metoda) nebo sbalené (`_doClose()`) metoda:</span><span class="sxs-lookup"><span data-stu-id="2a6b1-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


<span data-ttu-id="2a6b1-129">[![Tlačítko třetí změní stav panelu: z sbalené rozšířené a zpět](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2a6b1-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="2a6b1-130">Tlačítko třetí změní stav panelu: z sbalené rozšířené a pozadí ([Kliknutím zobrazit obrázek v plné velikosti](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2a6b1-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="2a6b1-131">Další</span><span class="sxs-lookup"><span data-stu-id="2a6b1-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)