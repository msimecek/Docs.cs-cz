---
title: Modul ASP.NET Core
author: tdykstra
description: "Představuje ASP.NET Core modulu (ANCM), modul služby IIS, která umožní Kestrel webový server použít jako reverzní proxy server služby IIS nebo IIS Express."
keywords: "Základní modul ASP.NET Core, IIS, IIS Express,ASP.NET, UseIISIntegration"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: 4661af33-34c5-4d71-93a0-8c7632f43580
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d1f551dbde5f3dd6e71808154c2e5885d588d7c
ms.sourcegitcommit: 282f69e8dd63c39bde97a6d72783af2970d92040
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="introduction-to-aspnet-core-module"></a><span data-ttu-id="1272b-104">Úvod do modulu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1272b-104">Introduction to ASP.NET Core Module</span></span>

<span data-ttu-id="1272b-105">Podle [tní Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), a [Ross Jan](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="1272b-105">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="1272b-106">ASP.NET Core modulu (ANCM) umožňuje spouštění aplikací za služby IIS, ASP.NET Core pomocí služby IIS pro co je vhodný v (zabezpečení, správy a mnoha Další) a pomocí [Kestrel](kestrel.md) pro co je vhodný v (se skutečně rychlé) a získávání výhody z obě technologie současně.</span><span class="sxs-lookup"><span data-stu-id="1272b-106">ASP.NET Core Module (ANCM) lets you run ASP.NET Core applications behind IIS, using IIS for what it's good at (security, manageability, and lots more) and using [Kestrel](kestrel.md) for what it's good at (being really fast), and getting the benefits from both technologies at once.</span></span> <span data-ttu-id="1272b-107">**ANCM pracuje pouze s Kestrel; není kompatibilní s WebListener (v ASP.NET Core 1.x) nebo ovladač HTTP.sys (v 2.x).**</span><span class="sxs-lookup"><span data-stu-id="1272b-107">**ANCM works only with Kestrel; it isn't compatible with WebListener (in ASP.NET Core 1.x) or HTTP.sys (in 2.x).**</span></span> 

<span data-ttu-id="1272b-108">Podporované verze systému Windows:</span><span class="sxs-lookup"><span data-stu-id="1272b-108">Supported Windows versions:</span></span>

* <span data-ttu-id="1272b-109">Windows 7 a Windows Server 2008 R2 a novější</span><span class="sxs-lookup"><span data-stu-id="1272b-109">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="1272b-110">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1272b-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-aspnet-core-module-does"></a><span data-ttu-id="1272b-111">Jaké jsou modulu jádro ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1272b-111">What ASP.NET Core Module does</span></span>

<span data-ttu-id="1272b-112">ANCM je nativní modul služby IIS, který zachytí do kanálu služby IIS a přesměrování provozu do back-end aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1272b-112">ANCM is a native IIS module that hooks into the IIS pipeline and redirects traffic to the backend ASP.NET Core application.</span></span> <span data-ttu-id="1272b-113">Většině ostatních modulů, jako je například ověřování systému windows, získat stále možné spustit.</span><span class="sxs-lookup"><span data-stu-id="1272b-113">Most other modules, such as windows authentication, still get a chance to run.</span></span> <span data-ttu-id="1272b-114">ANCM pouze přebírá řízení, pokud obslužná rutina je vybrán pro požadavek a mapování obslužné rutiny je definována v aplikaci *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="1272b-114">ANCM only takes control when a handler is selected for the request, and handler mapping is defined in the application *web.config* file.</span></span>

<span data-ttu-id="1272b-115">Protože se spouští v procesu aplikace ASP.NET Core samostatné z pracovní proces služby IIS, ANCM také zpracovat správy.</span><span class="sxs-lookup"><span data-stu-id="1272b-115">Because ASP.NET Core applications run in a process separate from the IIS worker process, ANCM also does process management.</span></span> <span data-ttu-id="1272b-116">ANCM spustí proces pro aplikace ASP.NET Core, když první požadavek odeslán a ho restartuje, když ho dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="1272b-116">ANCM starts the process for the ASP.NET Core application when the first request comes in and restarts it when it crashes.</span></span> <span data-ttu-id="1272b-117">Toto je v podstatě stejné chování jako classic aplikace ASP.NET, spusťte v procesu ve službě IIS a spravovaná službou WAS (Windows Activation Service).</span><span class="sxs-lookup"><span data-stu-id="1272b-117">This is essentially the same behavior as classic ASP.NET applications that run in-process in IIS and are managed by WAS (Windows Activation Service).</span></span>

<span data-ttu-id="1272b-118">Zde je diagram, který ukazuje vztah mezi aplikací služby IIS, ANCM a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1272b-118">Here's a diagram that illustrates the relationship between IIS, ANCM, and ASP.NET Core applications.</span></span>

![Modul ASP.NET Core](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="1272b-120">Požadavky přichází z webu a stiskněte tlačítko ovladač Http.Sys režimu jádra, který směruje je do služby IIS na primární port (80) nebo SSL port (443).</span><span class="sxs-lookup"><span data-stu-id="1272b-120">Requests come in from the Web and hit the kernel mode Http.Sys driver which routes them into IIS on the primary port (80) or SSL port (443).</span></span> <span data-ttu-id="1272b-121">ANCM předává požadavky na aplikace ASP.NET Core na port HTTP, který je nakonfigurovaný pro aplikaci, které není portu 80/443.</span><span class="sxs-lookup"><span data-stu-id="1272b-121">ANCM forwards the requests to the ASP.NET Core application on the HTTP port configured for the application, which is not port 80/443.</span></span>

<span data-ttu-id="1272b-122">Kestrel naslouchá pro provoz přicházející ze ANCM.</span><span class="sxs-lookup"><span data-stu-id="1272b-122">Kestrel listens for traffic coming from ANCM.</span></span>  <span data-ttu-id="1272b-123">ANCM Určuje port, přes proměnné prostředí při spuštění a [UseIISIntegration](#call-useiisintegration) metoda nakonfiguruje server tak, aby naslouchala na `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="1272b-123">ANCM specifies the port via environment variable at startup, and the [UseIISIntegration](#call-useiisintegration) method configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="1272b-124">Existují další kontroly, aby zamítal požadavky, nikoli z ANCM.</span><span class="sxs-lookup"><span data-stu-id="1272b-124">There are additional checks to reject requests not from ANCM.</span></span> <span data-ttu-id="1272b-125">(ANCM nepodporuje předávání protokolu HTTPS, takže jsou předávány požadavky prostřednictvím protokolu HTTP i v případě, že přijatých službou IIS prostřednictvím protokolu HTTPS.)</span><span class="sxs-lookup"><span data-stu-id="1272b-125">(ANCM does not support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.)</span></span>

<span data-ttu-id="1272b-126">Kestrel převezme požadavky z ANCM a nabízených oznámení je do kanálu ASP.NET Core middlewaru, který poté je zpracovává a předává je jako `HttpContext` instance, které chcete aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1272b-126">Kestrel picks up requests from ANCM and pushes them into the ASP.NET Core middleware pipeline, which then handles them and passes them on as `HttpContext` instances to application logic.</span></span> <span data-ttu-id="1272b-127">Odpovědi aplikací jsou pak předán zpět do služby IIS, které nabízených oznámení je zpět do klienta protokolu HTTP, který spustil žádosti.</span><span class="sxs-lookup"><span data-stu-id="1272b-127">The application's responses are then passed back to IIS, which pushes them back out to the HTTP client that initiated the requests.</span></span>

<span data-ttu-id="1272b-128">ANCM má několik dalších funkcí také:</span><span class="sxs-lookup"><span data-stu-id="1272b-128">ANCM has a few other functions as well:</span></span>

* <span data-ttu-id="1272b-129">Nastaví proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="1272b-129">Sets environment variables.</span></span>
* <span data-ttu-id="1272b-130">Protokoly `stdout` výstup k úložišti souborů.</span><span class="sxs-lookup"><span data-stu-id="1272b-130">Logs `stdout` output to file storage.</span></span>
* <span data-ttu-id="1272b-131">Předává tokeny ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="1272b-131">Forwards Windows authentication tokens.</span></span>

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a><span data-ttu-id="1272b-132">Jak používat ANCM v aplikacích ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1272b-132">How to use ANCM in ASP.NET Core apps</span></span>

<span data-ttu-id="1272b-133">Tato část obsahuje přehled procesu pro nastavení serveru služby IIS a ASP.NET Core aplikace.</span><span class="sxs-lookup"><span data-stu-id="1272b-133">This section provides an overview of the process for setting up an IIS server and ASP.NET Core application.</span></span> <span data-ttu-id="1272b-134">Podrobné pokyny najdete v tématu [publikování do služby IIS](../../publishing/iis.md).</span><span class="sxs-lookup"><span data-stu-id="1272b-134">For detailed instructions, see [Publishing to IIS](../../publishing/iis.md).</span></span>

### <a name="install-ancm"></a><span data-ttu-id="1272b-135">Nainstalujte ANCM</span><span class="sxs-lookup"><span data-stu-id="1272b-135">Install ANCM</span></span>


<span data-ttu-id="1272b-136">Základní modul ASP.NET musí být nainstalovaná ve službě IIS na serverech a ve službě IIS Express na vašich počítačích vývoj.</span><span class="sxs-lookup"><span data-stu-id="1272b-136">The ASP.NET Core Module has to be installed in IIS on your servers and in IIS Express on your development machines.</span></span> <span data-ttu-id="1272b-137">Pro servery, je součástí ANCM [.NET jádra Windows serveru, který hostuje sady](https://aka.ms/dotnetcore-2-windowshosting).</span><span class="sxs-lookup"><span data-stu-id="1272b-137">For servers, ANCM is included in the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting).</span></span> <span data-ttu-id="1272b-138">Pro počítače, vývoj Visual Studio automaticky nainstaluje ANCM ve službě IIS Express a ve službě IIS Pokud je již nainstalován na počítači.</span><span class="sxs-lookup"><span data-stu-id="1272b-138">For development machines, Visual Studio automatically installs ANCM in IIS Express, and in IIS if it is already installed on the machine.</span></span>

### <a name="install-the-iisintegration-nuget-package"></a><span data-ttu-id="1272b-139">Nainstalujte balíček IISIntegration NuGet</span><span class="sxs-lookup"><span data-stu-id="1272b-139">Install the IISIntegration NuGet package</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1272b-140">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1272b-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1272b-141">[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) balíček je součástí metapackages ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) a [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ).</span><span class="sxs-lookup"><span data-stu-id="1272b-141">The [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package is included in the ASP.NET Core metapackages ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) and [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)).</span></span> <span data-ttu-id="1272b-142">Pokud nepoužíváte jeden z metapackages, nainstalujte `Microsoft.AspNetCore.Server.IISIntegration` samostatně.</span><span class="sxs-lookup"><span data-stu-id="1272b-142">If you don't use one of the metapackages, install `Microsoft.AspNetCore.Server.IISIntegration` separately.</span></span> <span data-ttu-id="1272b-143">`IISIntegration` Balíčku je balík vzájemná funkční spolupráce, který čte proměnné prostředí všesměrového vysílání pomocí ANCM k nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1272b-143">The `IISIntegration` package is an interoperability pack that reads environment variables broadcast by ANCM to set up your app.</span></span> <span data-ttu-id="1272b-144">Proměnné prostředí poskytují informace o konfiguraci, jako je port pro naslouchání.</span><span class="sxs-lookup"><span data-stu-id="1272b-144">The environment variables provide configuration information, such as the port to listen on.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1272b-145">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="1272b-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1272b-146">V aplikaci nainstalovat [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/).</span><span class="sxs-lookup"><span data-stu-id="1272b-146">In your application, install [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/).</span></span> <span data-ttu-id="1272b-147">`IISIntegration` Balíčku je balík vzájemná funkční spolupráce, který čte proměnné prostředí všesměrového vysílání pomocí ANCM k nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1272b-147">The `IISIntegration` package  is an interoperability pack that reads environment variables broadcast by ANCM to set up your app.</span></span> <span data-ttu-id="1272b-148">Proměnné prostředí poskytují informace o konfiguraci, jako je port pro naslouchání.</span><span class="sxs-lookup"><span data-stu-id="1272b-148">The environment variables provide configuration information, such as the port to listen on.</span></span> 

---

### <a name="call-useiisintegration"></a><span data-ttu-id="1272b-149">Volání UseIISIntegration</span><span class="sxs-lookup"><span data-stu-id="1272b-149">Call UseIISIntegration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1272b-150">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1272b-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1272b-151">`UseIISIntegration` Rozšiřující metody na [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) je zavolána automaticky při spuštění pomocí služby IIS.</span><span class="sxs-lookup"><span data-stu-id="1272b-151">The `UseIISIntegration` extension method on [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) is called automatically when you run with IIS.</span></span>

<span data-ttu-id="1272b-152">Pokud nepoužíváte jeden metapackages ASP.NET Core a nenainstalovali `Microsoft.AspNetCore.Server.IISIntegration` balíčku, dojde k chybě běhového prostředí.</span><span class="sxs-lookup"><span data-stu-id="1272b-152">If you aren't using one of the ASP.NET Core metapackages and haven't installed the  `Microsoft.AspNetCore.Server.IISIntegration` package, you get a runtime error.</span></span> <span data-ttu-id="1272b-153">Když zavoláte `UseIISIntegration` explicitně, zobrazí chybu v době kompilace Pokud balíček není nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="1272b-153">If you call `UseIISIntegration` explicitly, you get a compile time error if the package isn't installed.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1272b-154">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="1272b-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1272b-155">Ve vaší aplikaci `Main` metoda, volání `UseIISIntegration` rozšiřující metody na [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1272b-155">In your application's `Main` method, call the `UseIISIntegration` extension method on [`WebHostBuilder`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

<span data-ttu-id="1272b-156">`UseIISIntegration` Metoda vyhledá proměnné prostředí, které nastaví ANCM ale ne ops, pokud nejsou nalezeny.</span><span class="sxs-lookup"><span data-stu-id="1272b-156">The `UseIISIntegration` method looks for environment variables that ANCM sets, and it no-ops if they aren't found.</span></span> <span data-ttu-id="1272b-157">Toto chování usnadňuje scénáře jako vývoj a testování v systému macOS nebo Linux a nasazení na server, který spouští IIS.</span><span class="sxs-lookup"><span data-stu-id="1272b-157">This behavior facilitates scenarios like developing and testing on macOS or Linux and deploying to a server that runs IIS.</span></span> <span data-ttu-id="1272b-158">Při spouštění v systému macOS nebo Linux, Kestrel funguje jako webový server; ale když se aplikace nasadí prostředí služby IIS, automaticky používá ANCM a služby IIS.</span><span class="sxs-lookup"><span data-stu-id="1272b-158">While running on macOS or Linux, Kestrel acts as the web server; but when the app is deployed to the IIS environment, it automatically uses ANCM and IIS.</span></span>

### <a name="ancm-port-binding-overrides-other-port-bindings"></a><span data-ttu-id="1272b-159">Vazbou portu ANCM přepíše ostatní port vazby</span><span class="sxs-lookup"><span data-stu-id="1272b-159">ANCM port binding overrides other port bindings</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1272b-160">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1272b-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1272b-161">ANCM generuje dynamický port přiřadit pro proces back-end.</span><span class="sxs-lookup"><span data-stu-id="1272b-161">ANCM generates a dynamic port to assign to the back-end process.</span></span> <span data-ttu-id="1272b-162">`UseIISIntegration` Metoda převezme tento dynamický port a nakonfiguruje Kestrel tak, aby naslouchala na `http://locahost:{dynamicPort}/`.</span><span class="sxs-lookup"><span data-stu-id="1272b-162">The `UseIISIntegration` method picks up this dynamic port and configures Kestrel to listen on `http://locahost:{dynamicPort}/`.</span></span> <span data-ttu-id="1272b-163">Přepíše ostatní konfigurace adresy URL, například volání `UseUrls` nebo [API naslouchat na Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="1272b-163">This overrides other URL configurations, such as calls to `UseUrls` or [Kestrel's Listen API](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span> <span data-ttu-id="1272b-164">Proto nemusíte volání `UseUrls` nebo na Kestrel `Listen` rozhraní API při použití ANCM.</span><span class="sxs-lookup"><span data-stu-id="1272b-164">Therefore, you don't need to call `UseUrls` or Kestrel's `Listen` API when you use ANCM.</span></span> <span data-ttu-id="1272b-165">Když zavoláte `UseUrls` nebo `Listen`, Kestrel naslouchá na portu zadejte při spuštění aplikace bez služby IIS.</span><span class="sxs-lookup"><span data-stu-id="1272b-165">If you do call `UseUrls` or `Listen`, Kestrel listens on the port you specify when you run the app without IIS.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1272b-166">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="1272b-166">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1272b-167">ANCM generuje dynamický port přiřadit pro proces back-end.</span><span class="sxs-lookup"><span data-stu-id="1272b-167">ANCM generates a dynamic port to assign to the back-end process.</span></span> <span data-ttu-id="1272b-168">`UseIISIntegration` Metoda převezme tento dynamický port a nakonfiguruje Kestrel tak, aby naslouchala na `http://locahost:{dynamicPort}/`.</span><span class="sxs-lookup"><span data-stu-id="1272b-168">The `UseIISIntegration` method picks up this dynamic port and configures Kestrel to listen on `http://locahost:{dynamicPort}/`.</span></span> <span data-ttu-id="1272b-169">Přepíše ostatní konfigurace adresy URL, například volání `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="1272b-169">This overrides other URL configurations, such as calls to `UseUrls`.</span></span> <span data-ttu-id="1272b-170">Proto nemusíte volání `UseUrls` při použití ANCM.</span><span class="sxs-lookup"><span data-stu-id="1272b-170">Therefore, you don't need to call `UseUrls` when you use ANCM.</span></span> <span data-ttu-id="1272b-171">Když zavoláte `UseUrls`, Kestrel naslouchá na portu zadejte při spuštění aplikace bez služby IIS.</span><span class="sxs-lookup"><span data-stu-id="1272b-171">If you do call `UseUrls`, Kestrel listens on the port you specify when you run the app without IIS.</span></span>

<span data-ttu-id="1272b-172">V technologii ASP.NET Core 1.0, když zavoláte `UseUrls`, volání **před** zavoláte `UseIISIntegration` tak, aby port nakonfigurovaný ANCM není přepsán.</span><span class="sxs-lookup"><span data-stu-id="1272b-172">In ASP.NET Core 1.0, if you call `UseUrls`, call it **before** you call `UseIISIntegration` so that the ANCM-configured port doesn't get overwritten.</span></span> <span data-ttu-id="1272b-173">Toto pořadí volání nevyžaduje v technologii ASP.NET Core 1.1, protože přepsání nastavení ANCM `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="1272b-173">This calling order isn't required in ASP.NET Core 1.1, because the ANCM setting overrides `UseUrls`.</span></span>

---

### <a name="configure-ancm-options-in-webconfig"></a><span data-ttu-id="1272b-174">Konfigurace možností ANCM v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="1272b-174">Configure ANCM options in Web.config</span></span>

<span data-ttu-id="1272b-175">Konfigurace modulu jádra ASP.NET je uložená v *Web.config* soubor, který se nachází v kořenové složce aplikace.</span><span class="sxs-lookup"><span data-stu-id="1272b-175">Configuration for the ASP.NET Core Module is stored in the *Web.config* file that is located in the application's root folder.</span></span> <span data-ttu-id="1272b-176">Nastavení v tomto souboru, přejděte na spuštění příkaz i jeho argumenty, které spustí vaše aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1272b-176">Settings in this file point to the startup command and arguments that start your ASP.NET Core app.</span></span> <span data-ttu-id="1272b-177">Ukázkový kód Web.config a informace o možnostech konfigurace naleznete v tématu [odkazu na modul Konfigurace ASP.NET Core](../../hosting/aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="1272b-177">For sample Web.config code and guidance on configuration options, see [ASP.NET Core Module Configuration Reference](../../hosting/aspnet-core-module.md).</span></span>

### <a name="run-with-iis-express-in-development"></a><span data-ttu-id="1272b-178">Spustit v vývoj službou IIS Express</span><span class="sxs-lookup"><span data-stu-id="1272b-178">Run with IIS Express in development</span></span>

<span data-ttu-id="1272b-179">Služba IIS Express, může být spuštěn Visual Studio pomocí výchozí profil definované šablony ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1272b-179">IIS Express can be launched by Visual Studio using the default profile defined by the ASP.NET Core templates.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="1272b-180">Konfigurace proxy serveru používá protokol HTTP a párovací token</span><span class="sxs-lookup"><span data-stu-id="1272b-180">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="1272b-181">Proxy server mezi ANCM a Kestrel vytvořen používá protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="1272b-181">The proxy created between the ANCM and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="1272b-182">Pomocí protokolu HTTP je optimalizace výkonu, kde provoz mezi ANCM a Kestrel probíhá na adresu zpětné smyčky z síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1272b-182">Using HTTP is a performance optimization where the traffic between the ANCM and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="1272b-183">Neexistuje žádné riziko odposlouchávání provoz mezi ANCM a Kestrel z umístění od serveru.</span><span class="sxs-lookup"><span data-stu-id="1272b-183">There's no risk of eavesdropping the traffic between the ANCM and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="1272b-184">Token párovací se používá k zaručit, že požadavků přijatých službou Kestrel byly směrovány přes proxy server službou IIS a nebyla pochází z jiného zdroje.</span><span class="sxs-lookup"><span data-stu-id="1272b-184">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="1272b-185">Párovací token se vytvoří a nastavení do proměnné prostředí (`ASPNETCORE_TOKEN`) pomocí ANCM.</span><span class="sxs-lookup"><span data-stu-id="1272b-185">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the ANCM.</span></span> <span data-ttu-id="1272b-186">Také nastavení párovací tokenu do záhlaví (`MSAspNetCoreToken`) u každého požadavku směrovány přes proxy server.</span><span class="sxs-lookup"><span data-stu-id="1272b-186">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="1272b-187">Služba IIS Middleware kontroly požadavku že obdrží potvrďte, že párovací hodnota tokenu hlavičky odpovídá hodnotu proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="1272b-187">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="1272b-188">Pokud hodnoty tokenu se neshodují, žádost je zaznamenána a odmítnut.</span><span class="sxs-lookup"><span data-stu-id="1272b-188">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="1272b-189">Proměnnou párovací tokenu prostředí a přenosy dat mezi ANCM a Kestrel nejsou dostupné z umístění od serveru.</span><span class="sxs-lookup"><span data-stu-id="1272b-189">The pairing token environment variable and the traffic between the ANCM and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="1272b-190">Bez znalosti párovací hodnota tokenu, útočník nemohou odesílat požadavky, které obejít kontrolu v IIS Middleware.</span><span class="sxs-lookup"><span data-stu-id="1272b-190">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1272b-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1272b-191">Next steps</span></span>

<span data-ttu-id="1272b-192">Další informace naleznete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="1272b-192">For more information, see the following resources:</span></span>

* [<span data-ttu-id="1272b-193">Ukázková aplikace pro tohoto článku</span><span class="sxs-lookup"><span data-stu-id="1272b-193">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [<span data-ttu-id="1272b-194">ASP.NET Core modulu zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="1272b-194">ASP.NET Core Module source code</span></span>](https://github.com/aspnet/AspNetCoreModule)
* [<span data-ttu-id="1272b-195">Konfigurace odkaz k modulu jádra ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1272b-195">ASP.NET Core Module Configuration Reference</span></span>](../../hosting/aspnet-core-module.md)
* [<span data-ttu-id="1272b-196">Publikování do služby IIS</span><span class="sxs-lookup"><span data-stu-id="1272b-196">Publishing to IIS</span></span>](../../publishing/iis.md)