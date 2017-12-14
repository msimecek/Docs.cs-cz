---
title: Stav relace a aplikace v ASP.NET Core
author: rick-anderson
description: "Přístupy k zachování aplikace a stavu uživatele (relace) mezi požadavky."
keywords: "ASP.NET Core, stav aplikací, stav relace, řetězce dotazu, post"
ms.author: riande
manager: wpickett
ms.date: 11/27/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 35b34f1a40e431e59e6b9c1d9bfb4ce3fced35e6
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="609ea-104">Úvod do stavu relace a aplikace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="609ea-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="609ea-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), a [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="609ea-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="609ea-106">HTTP je bezstavový.</span><span class="sxs-lookup"><span data-stu-id="609ea-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="609ea-107">Webový server zpracovává každý požadavek HTTP jako požadavek nezávislé a hodnoty uživatele z předchozích žádostech není zachována.</span><span class="sxs-lookup"><span data-stu-id="609ea-107">A web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="609ea-108">Tento článek popisuje různé způsoby, jak zachovat aplikace a stav relace mezi požadavky.</span><span class="sxs-lookup"><span data-stu-id="609ea-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="609ea-109">Stav relace</span><span class="sxs-lookup"><span data-stu-id="609ea-109">Session state</span></span>

<span data-ttu-id="609ea-110">Stav relace je funkce v ASP.NET Core, který můžete použít k ukládání a uchovávání dat uživatele, když uživatel prohlíží vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="609ea-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="609ea-111">Skládající se z tabulky slovník nebo hodnotu hash na serveru, stav relace trvá dat napříč požadavky z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="609ea-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="609ea-112">Data relace je zálohovaný díky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="609ea-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="609ea-113">ASP.NET Core Udržovat stav relace tím, že klient soubor cookie, který obsahuje Identifikátor relace, která je odeslána na server s každou žádostí.</span><span class="sxs-lookup"><span data-stu-id="609ea-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="609ea-114">Tento server využívá ID relace k načtení dat relace.</span><span class="sxs-lookup"><span data-stu-id="609ea-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="609ea-115">Soubor cookie relace je prohlížeč, a proto nelze sdílet relací mezi prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="609ea-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="609ea-116">Soubory cookie relace se odstraní pouze při ukončení relace prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="609ea-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="609ea-117">Pokud není soubor cookie pro relaci s ukončenou platností, je vytvořit novou relaci, který používá stejný soubor cookie relace.</span><span class="sxs-lookup"><span data-stu-id="609ea-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="609ea-118">Server uchovává relace po omezenou dobu od poslední žádosti.</span><span class="sxs-lookup"><span data-stu-id="609ea-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="609ea-119">Můžete nastavit časový limit relace nebo použijte výchozí hodnotu 20 minut.</span><span class="sxs-lookup"><span data-stu-id="609ea-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="609ea-120">Je ideální pro ukládání uživatelských dat, která je specifická pro konkrétní relace, ale nemusí být trvale jako trvalý stav relace.</span><span class="sxs-lookup"><span data-stu-id="609ea-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="609ea-121">Data odstranit z úložiště zálohování buď při volání `Session.Clear` nebo vypršení platnosti relace v úložišti.</span><span class="sxs-lookup"><span data-stu-id="609ea-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="609ea-122">Server nebude vědět při zavření prohlížeče nebo když se odstraní soubor cookie relace.</span><span class="sxs-lookup"><span data-stu-id="609ea-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="609ea-123">Neukládejte citlivá data v relaci.</span><span class="sxs-lookup"><span data-stu-id="609ea-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="609ea-124">Klient nemusí zavřete prohlížeč a vymazat souboru cookie relace (a některé prohlížeče zachování souborů cookie relací připojení mezi windows).</span><span class="sxs-lookup"><span data-stu-id="609ea-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="609ea-125">Navíc nemusí být relaci omezen na jednoho uživatele; Další uživatel může pokračovat ve stejné relaci.</span><span class="sxs-lookup"><span data-stu-id="609ea-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="609ea-126">Zprostředkovatel relací v paměti ukládá data relace na místním serveru.</span><span class="sxs-lookup"><span data-stu-id="609ea-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="609ea-127">Pokud máte v plánu ke spouštění vaší webové aplikace na serverové farmě, musíte použít trvalé relace ke svázání každou relaci k určitému serveru.</span><span class="sxs-lookup"><span data-stu-id="609ea-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="609ea-128">Platforma Windows Azure webů výchozí trvalé relace (směrování žádostí na aplikace nebo směrování žádostí na aplikace).</span><span class="sxs-lookup"><span data-stu-id="609ea-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="609ea-129">Trvalé relace však může mít vliv na škálovatelnost a zkomplikovat aktualizace webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="609ea-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="609ea-130">Lepší možností je používat Redis nebo SQL Server distribuované ukládá do mezipaměti, které nevyžadují trvalé relace.</span><span class="sxs-lookup"><span data-stu-id="609ea-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="609ea-131">Další informace najdete v tématu [práce s distribuované mezipaměti](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="609ea-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="609ea-132">Podrobnosti o nastavení poskytovatelé služeb najdete v tématu [konfigurace relace](#configuring-session) dále v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="609ea-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="609ea-133">TempData</span><span class="sxs-lookup"><span data-stu-id="609ea-133">TempData</span></span>

<span data-ttu-id="609ea-134">ASP.NET MVC základní zpřístupní [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) vlastnost [řadiče](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="609ea-134">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="609ea-135">Tato vlastnost ukládá data, dokud je pro čtení.</span><span class="sxs-lookup"><span data-stu-id="609ea-135">This property stores data until it is read.</span></span> <span data-ttu-id="609ea-136">`Keep` a `Peek` metody můžete použít k prozkoumání dat bez odstranění.</span><span class="sxs-lookup"><span data-stu-id="609ea-136">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="609ea-137">`TempData`je obzvláště užitečné pro přesměrování, pokud dat je potřeba pro více než jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="609ea-137">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="609ea-138">`TempData`je implementováno modulem TempData poskytovatelů, například pomocí souborů cookie nebo stav relace.</span><span class="sxs-lookup"><span data-stu-id="609ea-138">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="609ea-139">Zprostředkovatelé TempData</span><span class="sxs-lookup"><span data-stu-id="609ea-139">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="609ea-140">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="609ea-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="609ea-141">V technologii ASP.NET Core 2.0 nebo novější TempData zprostředkovatele na základě souborů cookie se používá ve výchozím nastavení pro ukládání TempData v souborech cookie.</span><span class="sxs-lookup"><span data-stu-id="609ea-141">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="609ea-142">Cookie data je zakódovaných pomocí [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="609ea-142">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="609ea-143">Vzhledem k tomu, že soubor cookie je šifrovaný a blokové, velikost jednoho souboru cookie limit v ASP.NET Core 1.x doporučení se netýká nalezen.</span><span class="sxs-lookup"><span data-stu-id="609ea-143">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="609ea-144">Není komprimována cookie data, protože kompresi šifrovaná data může způsobit problémy se zabezpečením, jako [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) a [porušení](https://wikipedia.org/wiki/BREACH_(security_exploit)) útoky.</span><span class="sxs-lookup"><span data-stu-id="609ea-144">The cookie data is not compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="609ea-145">Další informace o poskytovateli TempData na základě souborů cookie najdete v tématu [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="609ea-145">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="609ea-146">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="609ea-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="609ea-147">TempData poskytovatele stavu relace ASP.NET Core 1.0 a 1.1, je výchozí.</span><span class="sxs-lookup"><span data-stu-id="609ea-147">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="609ea-148">Výběr zprostředkovatele TempData</span><span class="sxs-lookup"><span data-stu-id="609ea-148">Choosing a TempData provider</span></span>

<span data-ttu-id="609ea-149">Výběr zprostředkovatele TempData zahrnuje několik důležité informace, například:</span><span class="sxs-lookup"><span data-stu-id="609ea-149">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="609ea-150">Používá aplikace již stav relace pro jiné účely?</span><span class="sxs-lookup"><span data-stu-id="609ea-150">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="609ea-151">Pokud ano, použití TempData poskytovatele stavu relace má bez dalších nákladů na aplikace (kromě zajištění dostatečného množství dat).</span><span class="sxs-lookup"><span data-stu-id="609ea-151">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="609ea-152">Aplikace používá TempData pouze pouze pro relativně malé množství dat (až 500 bajtů)?</span><span class="sxs-lookup"><span data-stu-id="609ea-152">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="609ea-153">Pokud ano, přidá zprostředkovatele TempData souboru cookie malé náklady na každý požadavek, který představuje TempData.</span><span class="sxs-lookup"><span data-stu-id="609ea-153">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="609ea-154">Pokud ne, může být výhodné, aby se zabránilo odezvy velké množství dat v každé žádosti o, dokud TempData obsazením TempData poskytovatele stavu relace.</span><span class="sxs-lookup"><span data-stu-id="609ea-154">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="609ea-155">Spuštění aplikace ve webové farmě (více serverů)?</span><span class="sxs-lookup"><span data-stu-id="609ea-155">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="609ea-156">Pokud ano, neexistuje žádná další konfigurace, které jsou potřeba k použití zprostředkovatele TempData souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="609ea-156">If so, there is no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="609ea-157">Většina webovými klienty (například webové prohlížeče) vynutit omezení pro maximální velikost každého souboru cookie, celkový počet souborů cookie nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="609ea-157">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="609ea-158">Proto při použití zprostředkovatele TempData souboru cookie, ověřte, že aplikace nesmí být větší než tato omezení.</span><span class="sxs-lookup"><span data-stu-id="609ea-158">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="609ea-159">Vezměte v úvahu celková velikost dat, monitorování účtů pro režie šifrování a rozdělování.</span><span class="sxs-lookup"><span data-stu-id="609ea-159">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="609ea-160">Konfigurace zprostředkovatele TempData</span><span class="sxs-lookup"><span data-stu-id="609ea-160">Configure the TempData provider</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="609ea-161">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="609ea-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="609ea-162">Ve výchozím nastavení je povolen TempData zprostředkovatel na základě souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="609ea-162">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="609ea-163">Následující `Startup` kód třídy nakonfiguruje TempData zprostředkovatele na bázi relace:</span><span class="sxs-lookup"><span data-stu-id="609ea-163">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="609ea-164">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="609ea-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="609ea-165">Následující `Startup` kód třídy nakonfiguruje TempData zprostředkovatele na bázi relace:</span><span class="sxs-lookup"><span data-stu-id="609ea-165">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

<span data-ttu-id="609ea-166">Pořadí je důležité pro komponenty middlewaru.</span><span class="sxs-lookup"><span data-stu-id="609ea-166">Ordering is critical for middleware components.</span></span> <span data-ttu-id="609ea-167">V předchozím příkladu, k výjimce typu `InvalidOperationException` dojde při `UseSession` je volána poté, co `UseMvcWithDefaultRoute`.</span><span class="sxs-lookup"><span data-stu-id="609ea-167">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="609ea-168">V tématu [řazení Middleware](xref:fundamentals/middleware#ordering) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="609ea-168">See [Middleware Ordering](xref:fundamentals/middleware#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="609ea-169">Pokud cílení na rozhraní .NET Framework a pomocí zprostředkovatele na bázi relace, přidejte [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) balíček NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="609ea-169">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="609ea-170">Řetězce dotazů</span><span class="sxs-lookup"><span data-stu-id="609ea-170">Query strings</span></span>

<span data-ttu-id="609ea-171">Můžete předat omezené množství dat z jednoho požadavku na jiný přidáním do novou žádost o řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="609ea-171">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="609ea-172">To je užitečné pro zaznamenání stavu trvalé způsobem, který umožňuje propojení s embedded stavu sdílení e-mailu nebo sociálních sítí.</span><span class="sxs-lookup"><span data-stu-id="609ea-172">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="609ea-173">Ale z tohoto důvodu by nikdy používáte řetězce dotazu pro citlivá data.</span><span class="sxs-lookup"><span data-stu-id="609ea-173">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="609ea-174">Kromě se snadno sdílet, včetně dat v řetězcích dotazů můžete vytvořit příležitosti pro [webů požadavku padělání (proti útokům CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) útoků, které můžete obelstít návštěvou škodlivé weby při ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="609ea-174">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="609ea-175">Útočníci mohou pak odcizit data uživatele z vaší aplikace nebo provést škodlivé akce jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="609ea-175">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="609ea-176">Zachovaných stavu application nebo session musí zajistit ochranu proti útokům proti útokům CSRF.</span><span class="sxs-lookup"><span data-stu-id="609ea-176">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="609ea-177">Další informace o útoku proti útokům CSRF najdete v tématu [útoky brání webů požadavku padělání (XSRF/proti útokům CSRF) v ASP.NET Core](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="609ea-177">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="609ea-178">Následná data a skrytá pole</span><span class="sxs-lookup"><span data-stu-id="609ea-178">Post data and hidden fields</span></span>

<span data-ttu-id="609ea-179">Data můžete uložit ve skrytého pole a odeslány zpět na další požadavek.</span><span class="sxs-lookup"><span data-stu-id="609ea-179">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="609ea-180">To je běžné ve formulářích s více stránkami.</span><span class="sxs-lookup"><span data-stu-id="609ea-180">This is common in multi-page forms.</span></span> <span data-ttu-id="609ea-181">Ale vzhledem k tomu, že klient může potenciálně manipulovat s daty, server musí vždy znovu ověřit ho.</span><span class="sxs-lookup"><span data-stu-id="609ea-181">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="609ea-182">Soubory cookie</span><span class="sxs-lookup"><span data-stu-id="609ea-182">Cookies</span></span>

<span data-ttu-id="609ea-183">Soubory cookie poskytují způsob, jak ukládat data specifická pro uživatele ve webových aplikacích.</span><span class="sxs-lookup"><span data-stu-id="609ea-183">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="609ea-184">Soubory cookie jsou odesílány s každou žádost, jejich velikost měli omezit na minimum.</span><span class="sxs-lookup"><span data-stu-id="609ea-184">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="609ea-185">V ideálním případě by měly jenom identifikátor uložené v souboru cookie s dat uložených na serveru.</span><span class="sxs-lookup"><span data-stu-id="609ea-185">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="609ea-186">Většina prohlížečů soubory cookie omezit na 4096 bajtů.</span><span class="sxs-lookup"><span data-stu-id="609ea-186">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="609ea-187">Kromě toho jsou k dispozici pro každou doménu pouze omezený počet souborů cookie.</span><span class="sxs-lookup"><span data-stu-id="609ea-187">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="609ea-188">Protože soubory cookie se vztahují manipulaci, musí být ověřený na serveru.</span><span class="sxs-lookup"><span data-stu-id="609ea-188">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="609ea-189">Odolnost souboru cookie na klientském podléhá zásahu uživatele a vypršení platnosti, jsou obecně nejvíce trvanlivá formu trvalosti dat na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="609ea-189">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="609ea-190">Soubory cookie se často používají pro přizpůsobení, kde je obsah přizpůsobit pro známé uživatele.</span><span class="sxs-lookup"><span data-stu-id="609ea-190">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="609ea-191">Protože uživatel je pouze identifikovat a ve většině případů není ověřen, obvykle můžete zabezpečit souboru cookie uložením uživatelské jméno, název účtu nebo jedinečné ID uživatele (například identifikátor GUID) v souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="609ea-191">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="609ea-192">Pak můžete soubor cookie pro přístup k infrastruktuře přizpůsobení uživatele lokality.</span><span class="sxs-lookup"><span data-stu-id="609ea-192">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="609ea-193">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="609ea-193">HttpContext.Items</span></span>

<span data-ttu-id="609ea-194">`Items` Kolekce je dobré umístění pro uložení dat, která je potřeba při jenom jednoho konkrétního požadavku na zpracování.</span><span class="sxs-lookup"><span data-stu-id="609ea-194">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="609ea-195">Obsah kolekce jsou zahozeny po každé žádosti.</span><span class="sxs-lookup"><span data-stu-id="609ea-195">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="609ea-196">`Items` Kolekce je nejvhodnější jako způsob, jak součásti nebo middleware pro komunikaci, když budou fungovat v různých okamžicích v době žádost a mají přímý způsob, jak předat parametry.</span><span class="sxs-lookup"><span data-stu-id="609ea-196">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="609ea-197">Další informace najdete v tématu [práce s HttpContext.Items](#working-with-httpcontextitems)dál v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="609ea-197">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="609ea-198">Mezipaměti</span><span class="sxs-lookup"><span data-stu-id="609ea-198">Cache</span></span>

<span data-ttu-id="609ea-199">Ukládání do mezipaměti je účinný způsob, jak ukládat a načítat data.</span><span class="sxs-lookup"><span data-stu-id="609ea-199">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="609ea-200">Můžete ovládat, doba platnosti položek v mezipaměti na základě času a další důležité informace.</span><span class="sxs-lookup"><span data-stu-id="609ea-200">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="609ea-201">Další informace o [ukládání do mezipaměti](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="609ea-201">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="609ea-202">Práce s stav relace</span><span class="sxs-lookup"><span data-stu-id="609ea-202">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="609ea-203">Konfigurace relace</span><span class="sxs-lookup"><span data-stu-id="609ea-203">Configuring Session</span></span>

<span data-ttu-id="609ea-204">`Microsoft.AspNetCore.Session` Balíček poskytuje middleware pro správu stavu relace.</span><span class="sxs-lookup"><span data-stu-id="609ea-204">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="609ea-205">Povolit middleware relace `Startup` musí obsahovat:</span><span class="sxs-lookup"><span data-stu-id="609ea-205">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="609ea-206">Některé z [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) mezipaměti paměti.</span><span class="sxs-lookup"><span data-stu-id="609ea-206">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="609ea-207">`IDistributedCache` Implementace slouží jako úložiště zálohování pro relaci.</span><span class="sxs-lookup"><span data-stu-id="609ea-207">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="609ea-208">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) volání, což vyžaduje, aby balíček NuGet "Microsoft.AspNetCore.Session".</span><span class="sxs-lookup"><span data-stu-id="609ea-208">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="609ea-209">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) volání.</span><span class="sxs-lookup"><span data-stu-id="609ea-209">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="609ea-210">Následující kód ukazuje, jak nastavit Zprostředkovatel relací v paměti.</span><span class="sxs-lookup"><span data-stu-id="609ea-210">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="609ea-211">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="609ea-211">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="609ea-212">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="609ea-212">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="609ea-213">Relace, můžete odkazovat `HttpContext` po je nainstalovaný a nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="609ea-213">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="609ea-214">Pokud se pokusíte přístup `Session` před `UseSession` byla volána, výjimka `InvalidOperationException: Session has not been configured for this application or request` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="609ea-214">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="609ea-215">Pokud se pokusíte vytvořit novou `Session` (tedy bez souboru cookie relace byla vytvořena) poté, co jste již byl zahájen zápis do `Response` stream, výjimka `InvalidOperationException: The session cannot be established after the response has started` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="609ea-215">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="609ea-216">Výjimka naleznete v protokolu webového serveru; se nebude zobrazovat v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="609ea-216">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="609ea-217">Asynchronní načítání relace</span><span class="sxs-lookup"><span data-stu-id="609ea-217">Loading Session asynchronously</span></span> 

<span data-ttu-id="609ea-218">Výchozí zprostředkovatel relace v ASP.NET Core načte záznam relace ze základní [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) asynchronně jenom Pokud úložiště [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) metoda je explicitně volána před provedením  `TryGetValue`, `Set`, nebo `Remove` metody.</span><span class="sxs-lookup"><span data-stu-id="609ea-218">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="609ea-219">Pokud `LoadAsync` není jako první, základní záznam relace je načtena synchronně, což může potenciálně ovlivnit aplikace škálování.</span><span class="sxs-lookup"><span data-stu-id="609ea-219">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="609ea-220">Pokud chcete, aby aplikace vynutit tento vzor, zabalení [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) a [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementace s verzemi, které způsobí výjimku, pokud `LoadAsync` metoda není volá se před `TryGetValue`, `Set`, nebo `Remove`.</span><span class="sxs-lookup"><span data-stu-id="609ea-220">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="609ea-221">Zaregistrujte zabalené verze v kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="609ea-221">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="609ea-222">Podrobnosti implementace</span><span class="sxs-lookup"><span data-stu-id="609ea-222">Implementation Details</span></span>

<span data-ttu-id="609ea-223">Relace používá ke sledování a identifikaci požadavků z jednoho prohlížeče do souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="609ea-223">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="609ea-224">Ve výchozím nastavení, je tento soubor cookie s názvem ". AspNet.Session"a používá cestu"/".</span><span class="sxs-lookup"><span data-stu-id="609ea-224">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="609ea-225">Protože výchozí soubor cookie neurčuje doménu, se nebude zpřístupněn pro skript na straně klienta na stránce (protože `CookieHttpOnly` výchozí `true`).</span><span class="sxs-lookup"><span data-stu-id="609ea-225">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="609ea-226">Chcete-li přepsat výchozí hodnoty relace, použijte `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="609ea-226">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="609ea-227">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="609ea-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="609ea-228">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="609ea-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="609ea-229">Tento server využívá `IdleTimeout` vlastnosti k určení, jak dlouho relace může být nečinnosti, než jsou opuštění její obsah.</span><span class="sxs-lookup"><span data-stu-id="609ea-229">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="609ea-230">Tato vlastnost je nezávislá na vypršení platnosti souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="609ea-230">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="609ea-231">Každý požadavek, který předává přes middleware relace (číst nebo zapisovat do) obnoví časový limit.</span><span class="sxs-lookup"><span data-stu-id="609ea-231">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="609ea-232">Protože `Session` je *bez uzamčení*, pokud dva požadavky obou pokusí změnit obsah relace, poslední přepíše první.</span><span class="sxs-lookup"><span data-stu-id="609ea-232">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="609ea-233">`Session`je implementovaný jako *souvislý relace*, což znamená, že se veškerý obsah ukládat společně.</span><span class="sxs-lookup"><span data-stu-id="609ea-233">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="609ea-234">Dva požadavky, které se změnit různé části relace (různé klíče) může stále vzájemně vliv.</span><span class="sxs-lookup"><span data-stu-id="609ea-234">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="609ea-235">Nastavení a získání hodnoty relace</span><span class="sxs-lookup"><span data-stu-id="609ea-235">Setting and getting Session values</span></span>

<span data-ttu-id="609ea-236">Relace je přístupné přes `Session` vlastnost `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="609ea-236">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="609ea-237">Tato vlastnost je [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementace.</span><span class="sxs-lookup"><span data-stu-id="609ea-237">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="609ea-238">Následující příklad ukazuje nastavení a získání int a řetězec:</span><span class="sxs-lookup"><span data-stu-id="609ea-238">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="609ea-239">Pokud přidáte následující metody rozšíření, můžete nastavit a získat serializovatelné objekty do relace:</span><span class="sxs-lookup"><span data-stu-id="609ea-239">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="609ea-240">Následující příklad ukazuje, jak nastavit a získat serializovatelný objekt:</span><span class="sxs-lookup"><span data-stu-id="609ea-240">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="609ea-241">Práce s HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="609ea-241">Working with HttpContext.Items</span></span>

<span data-ttu-id="609ea-242">`HttpContext` Abstrakce zajišťuje podporu pro kolekci slovníku typu `IDictionary<object, object>`, názvem `Items`.</span><span class="sxs-lookup"><span data-stu-id="609ea-242">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="609ea-243">Tato kolekce je k dispozici od začátku *požadavku HTTP* a na konci každého požadavku se zahodí.</span><span class="sxs-lookup"><span data-stu-id="609ea-243">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="609ea-244">Můžete k němu přístup přiřazením hodnoty pro položku s klíčem nebo tím, že požádá hodnotu pro konkrétní klíč.</span><span class="sxs-lookup"><span data-stu-id="609ea-244">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="609ea-245">V ukázce níže [Middleware](middleware.md) přidá `isVerified` k `Items` kolekce.</span><span class="sxs-lookup"><span data-stu-id="609ea-245">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="609ea-246">Později v kanálu další middleware může k němu přístup:</span><span class="sxs-lookup"><span data-stu-id="609ea-246">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="609ea-247">Pro middleware, která bude použita pouze jedna aplikace `string` klíče jsou přijatelné.</span><span class="sxs-lookup"><span data-stu-id="609ea-247">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="609ea-248">Ale middleware, který bude sdílena mezi aplikací používejte jedinečný objekt předejdete pravděpodobné, že kolize klíče.</span><span class="sxs-lookup"><span data-stu-id="609ea-248">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="609ea-249">Pokud vyvíjíte middleware, který musí fungovat na všech několik aplikací, použijte klíč jedinečný objekt definovaný ve třídě middleware, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="609ea-249">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="609ea-250">Jiný kód, můžete přístup s hodnotou uloženou v `HttpContext.Items` pomocí klíče vystavené middleware třídy:</span><span class="sxs-lookup"><span data-stu-id="609ea-250">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="609ea-251">Tento přístup má také výhod odstranění opakování "magic řetězce" na více místech v kódu.</span><span class="sxs-lookup"><span data-stu-id="609ea-251">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="609ea-252">Data stavu aplikace</span><span class="sxs-lookup"><span data-stu-id="609ea-252">Application state data</span></span>

<span data-ttu-id="609ea-253">Použití [vkládání závislostí](xref:fundamentals/dependency-injection) jak zpřístupnit data pro všechny uživatele:</span><span class="sxs-lookup"><span data-stu-id="609ea-253">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="609ea-254">Definovat službu, která obsahuje data (například třídy s názvem `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="609ea-254">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="609ea-255">Přidání třídy pro službu `ConfigureServices` (například `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="609ea-255">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="609ea-256">Třída služby dat v každém řadiči využívat:</span><span class="sxs-lookup"><span data-stu-id="609ea-256">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="609ea-257">Běžné chyby při práci s relací</span><span class="sxs-lookup"><span data-stu-id="609ea-257">Common errors when working with session</span></span>

* <span data-ttu-id="609ea-258">"Nelze přeložit služby pro typ 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' při pokusu o aktivaci 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span><span class="sxs-lookup"><span data-stu-id="609ea-258">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="609ea-259">To je obvykle způsobeno nedaří nakonfigurovat alespoň jednu `IDistributedCache` implementace.</span><span class="sxs-lookup"><span data-stu-id="609ea-259">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="609ea-260">Další informace najdete v tématu [práce s distribuované mezipaměti](xref:performance/caching/distributed) a [v ukládání do mezipaměti](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="609ea-260">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="609ea-261">V případě, který relace middleware nepodaří zachovat relace (například: Pokud databáze není k dispozici), se protokoluje výjimku a swallows ho.</span><span class="sxs-lookup"><span data-stu-id="609ea-261">In the event that the session middleware fails to persist a session (for example: if the database is not available), it logs the exception and swallows it.</span></span> <span data-ttu-id="609ea-262">Žádost se pak normálně pokračovat, což vede k velmi nepředvídatelné chování.</span><span class="sxs-lookup"><span data-stu-id="609ea-262">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="609ea-263">Typickým příkladem:</span><span class="sxs-lookup"><span data-stu-id="609ea-263">A typical example:</span></span>

<span data-ttu-id="609ea-264">Někdo uloží nákupní košík v relaci.</span><span class="sxs-lookup"><span data-stu-id="609ea-264">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="609ea-265">Uživatel přidá položku, ale potvrzení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="609ea-265">The user adds an item but the commit fails.</span></span> <span data-ttu-id="609ea-266">Aplikace neví o selhání, proto sestavy zpráva "položku přidala", což není pravda.</span><span class="sxs-lookup"><span data-stu-id="609ea-266">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="609ea-267">Je doporučeným způsobem, jak zkontrolovat takové chyby pro volání `await feature.Session.CommitAsync();` z kódu aplikace po dokončení zápisu do relace.</span><span class="sxs-lookup"><span data-stu-id="609ea-267">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="609ea-268">Pak můžete udělat, co se vám líbí došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="609ea-268">Then you can do what you like with the error.</span></span> <span data-ttu-id="609ea-269">Funguje stejným způsobem jako při volání metody `LoadAsync`.</span><span class="sxs-lookup"><span data-stu-id="609ea-269">It works the same way when calling `LoadAsync`.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="609ea-270">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="609ea-270">Additional Resources</span></span>


* [<span data-ttu-id="609ea-271">ASP.NET Core 1.x: ukázkový kód v tomto dokumentu</span><span class="sxs-lookup"><span data-stu-id="609ea-271">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="609ea-272">ASP.NET Core 2.x: ukázkový kód v tomto dokumentu</span><span class="sxs-lookup"><span data-stu-id="609ea-272">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)