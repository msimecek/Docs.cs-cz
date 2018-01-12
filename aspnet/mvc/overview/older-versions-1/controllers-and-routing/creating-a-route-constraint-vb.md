---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: "Vytváření omezení trasy (VB) | Microsoft Docs"
author: StephenWalther
description: "V tomto kurzu Stephen Walther ukazuje, jak můžete řídit, jak prohlížeč požaduje shodu trasy vytvořením omezení trasy s použitím regulárních výrazů."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ff2666f4558abd4f8d9bddffd7aef8bb68d7bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-route-constraint-vb"></a>Vytváření omezení trasy (VB)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> V tomto kurzu Stephen Walther ukazuje, jak můžete řídit, jak prohlížeč požaduje shodu trasy vytvořením omezení trasy s použitím regulárních výrazů.


Pomocí omezení trasy omezit požadavky prohlížeče, které odpovídají příslušné cesty. Regulární výraz můžete zadat omezení trasy.

Představte si například, že jste definovali trasy v výpis 1 v souboru Global.asax.

**Výpis 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Výpis 1 obsahuje trasu s názvem produktu. Trasy produktu můžete použít k mapování požadavků prohlížeče na ProductController obsažené v výpis 2.

**Výpis 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Všimněte si, že akce Details() vystavené kontroler produktu přijímá jeden parametr s názvem productId. Tento parametr je parametr celé číslo.

Trasy definované v výpis 1 se bude shodovat s některou z těchto adres URL:

- / Produktu nebo 23
- / Produktu/7

Trasy bohužel se bude shodovat následujícím adresám URL:

- / Produktu nebo Bla
- / Produktu nebo apple

Vzhledem k tomu, že akce Details() očekává celé číslo parametru, provedení požadavek, který obsahuje něco jiného než celočíselnou hodnotu způsobí chybu. Například pokud zadáte /Product/apple adresu URL do prohlížeče pak zobrazí se chybová stránka na obrázku 1.


[![Dialogové okno Nový projekt](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Obrázek 01**: zobrazuje stránku Rozbalit ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-route-constraint-vb/_static/image2.png))


Co Opravdu chcete provést je odpovídá pouze adresy URL, které obsahují productId správné celé číslo. Omezení můžete použít při definování trasy omezoval adresy URL, které odpovídají trasy. Upravené trasy produktu ve výpisu 3 obsahuje omezení regulárního výrazu, která pouze odpovídá celých čísel.

**Výpis 3 – Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

Regulární výraz \d+ odpovídá jeden nebo více celých čísel. Toto omezení způsobí, že produkt trasy, která má odpovídat následující adresy URL:

- / Produktu/3
- / Produktu nebo 8999

Ale není následující adresy URL:

- / Produktu nebo apple
- / Produktu

Tyto požadavky prohlížeče bude zpracovávat jiné cestě, nebo pokud je k dispozici žádné odpovídající tras *prostředek nebyl nalezen* bude vrácena chyba.

>[!div class="step-by-step"]
[Předchozí](creating-custom-routes-vb.md)
[další](creating-a-custom-route-constraint-vb.md)