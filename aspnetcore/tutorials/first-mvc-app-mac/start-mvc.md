---
title: "Začínáme s ASP.NET MVC jádra a sady Visual Studio pro Mac"
author: rick-anderson
description: "Začínáme s ASP.NET MVC jádra a sady Visual Studio"
keywords: "Jádro ASP.NET, MVC, Visual Studio pro Mac, rozhraní Entity Framework"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 21f115eec924d5e4b21ad78398c8cbd99e02a0a8
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="3bb9d-104">Začínáme s ASP.NET MVC jádra a sady Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="3bb9d-104">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="3bb9d-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3bb9d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3bb9d-106">V tomto kurzu se dozvíte, jaké základní informace o vytváření ASP.NET MVC základní webové aplikace pomocí [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="3bb9d-106">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="3bb9d-107">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="3bb9d-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="3bb9d-108">systému macOS: [sestavení aplikace ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3bb9d-108">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="3bb9d-109">Windows: [sestavení ASP.NET Core aplikace MVC pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3bb9d-109">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="3bb9d-110">Systému macOS, Linux a Windows: [sestavení aplikace ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3bb9d-110">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3bb9d-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3bb9d-111">Prerequisites</span></span>

<span data-ttu-id="3bb9d-112">Tento kurz vyžaduje [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-112">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="3bb9d-113">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="3bb9d-113">Install the following:</span></span>

- <span data-ttu-id="3bb9d-114">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější</span><span class="sxs-lookup"><span data-stu-id="3bb9d-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="3bb9d-115">Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="3bb9d-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="3bb9d-116">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="3bb9d-116">Create a web app</span></span>

<span data-ttu-id="3bb9d-117">Ze sady Visual Studio, vyberte **soubor > Nový řešení**.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-117">From Visual Studio, select **File > New Solution**.</span></span>

![systému macOS nové řešení](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="3bb9d-119">Vyberte **aplikace .NET Core > ASP.NET Core > Webová aplikace > Další**.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-119">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![systému macOS dialogové okno Nový projekt](start-mvc/1.png)

<span data-ttu-id="3bb9d-121">Název projektu **MvcMovie**a potom vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-121">Name the project **MvcMovie**, and then select **Create**.</span></span>

![systému macOS dialogové okno Nový projekt](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="3bb9d-123">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="3bb9d-123">Launch the app</span></span>

<span data-ttu-id="3bb9d-124">V sadě Visual Studio, vyberte **spustit > Spustit bez ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-124">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="3bb9d-125">Visual Studio spustí [Kestrel](xref:fundamentals/servers/index#kestrel), spustí se prohlížeč a přejde na `http://localhost:port`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-125">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Prohlížeč s nový projekt](start-mvc/b1.png)

* <span data-ttu-id="3bb9d-127">Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-127">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="3bb9d-128">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-128">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="3bb9d-129">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="3bb9d-130">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-130">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="3bb9d-131">Můžete spustit aplikaci v ladění nebo režim bez ladění z **spustit** nabídky.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-131">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="3bb9d-132">Výchozí šablony vám **domácí o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-132">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="3bb9d-133">Na obrázku prohlížeče výše nezobrazí tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-133">The browser image above doesn't show these links.</span></span> <span data-ttu-id="3bb9d-134">V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-134">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Prohlížeč s nový projekt](start-mvc/b2.png)

<span data-ttu-id="3bb9d-136">V další části tohoto kurzu Další informace o MVC a zahájit zápis nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="3bb9d-136">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="3bb9d-137">Další</span><span class="sxs-lookup"><span data-stu-id="3bb9d-137">Next</span></span>](adding-controller.md)  