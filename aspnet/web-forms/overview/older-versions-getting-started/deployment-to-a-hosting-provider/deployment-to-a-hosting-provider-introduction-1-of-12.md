---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: "Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio: Úvod - 1 12 | Microsoft Docs"
author: tdykstra
description: "Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 7c03453e64cfc065d9f424702cc5af373e9bf536
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a><span data-ttu-id="b6349-103">Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio: Úvod - 1 12</span><span class="sxs-lookup"><span data-stu-id="b6349-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio: Introduction - 1 of 12</span></span>
====================
<span data-ttu-id="b6349-104">podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b6349-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="b6349-105">Stáhněte si úvodní projekt</span><span class="sxs-lookup"><span data-stu-id="b6349-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="b6349-106">Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web.</span><span class="sxs-lookup"><span data-stu-id="b6349-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="b6349-107">Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web.</span><span class="sxs-lookup"><span data-stu-id="b6349-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span>
> 
> <span data-ttu-id="b6349-108">Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b6349-108">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>
> 
> <span data-ttu-id="b6349-109">Tyto kurzy vás provede nasazením nejprve do služby IIS ve svém místním vývojovém počítači pro testování a k poskytovateli hostingu třetích stran.</span><span class="sxs-lookup"><span data-stu-id="b6349-109">These tutorials guide you through deploying first to IIS on your local development computer for testing, and then to a third-party hosting provider.</span></span> <span data-ttu-id="b6349-110">Aplikace, která budete nasazovat používá databázi aplikace a databáze členství technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b6349-110">The application that you'll deploy uses an application database and an ASP.NET membership database.</span></span> <span data-ttu-id="b6349-111">Můžete začít používat systém SQL Server Compact a nasazení do systému SQL Server Compact a novější kurzy vám ukážou, jak nasadit změny v databázi a jak migrovat do systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b6349-111">You start off using SQL Server Compact and deploying to SQL Server Compact, and later tutorials show you how to deploy database changes and how to migrate to SQL Server.</span></span>
> 
> <span data-ttu-id="b6349-112">Kurzů k předpokládá, že víte, jak pracovat s technologií ASP.NET v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b6349-112">The tutorials assume you know how to work with ASP.NET in Visual Studio.</span></span> <span data-ttu-id="b6349-113">Pokud to neuděláte, je vhodné oddělení na zahájení [základní technologie ASP.NET Web Forms kurzu](../tailspin-spyworks/tailspin-spyworks-part-1.md) nebo [základní kurz k ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).</span><span class="sxs-lookup"><span data-stu-id="b6349-113">If you don't, a good place to start is a [basic ASP.NET Web Forms Tutorial](../tailspin-spyworks/tailspin-spyworks-part-1.md) or a [basic ASP.NET MVC Tutorial](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).</span></span>
> 
> <span data-ttu-id="b6349-114">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum nasazení ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).</span><span class="sxs-lookup"><span data-stu-id="b6349-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET Deployment forum](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).</span></span>


## <a name="overview"></a><span data-ttu-id="b6349-115">Přehled</span><span class="sxs-lookup"><span data-stu-id="b6349-115">Overview</span></span>

<span data-ttu-id="b6349-116">Tyto kurzy vás provede nasazením nejprve do služby IIS ve svém místním vývojovém počítači pro testování a k poskytovateli hostingu třetích stran.</span><span class="sxs-lookup"><span data-stu-id="b6349-116">These tutorials guide you through deploying first to IIS on your local development computer for testing, and then to a third-party hosting provider.</span></span> <span data-ttu-id="b6349-117">Aplikace, která budete nasazovat používá databázi aplikace a databáze členství technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b6349-117">The application that you'll deploy uses an application database and an ASP.NET membership database.</span></span> <span data-ttu-id="b6349-118">Můžete začít používat systém SQL Server Compact a nasazení do systému SQL Server Compact a novější kurzy vám ukážou, jak nasadit změny v databázi a jak migrovat do systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b6349-118">You start off using SQL Server Compact and deploying to SQL Server Compact, and later tutorials show you how to deploy database changes and how to migrate to SQL Server.</span></span>

<span data-ttu-id="b6349-119">Číslo kurzů – 11 ve všech a řešení potíží stránky – provést proces nasazení pravděpodobně složitý.</span><span class="sxs-lookup"><span data-stu-id="b6349-119">The number of tutorials – 11 in all plus a troubleshooting page – might make the deployment process seem daunting.</span></span> <span data-ttu-id="b6349-120">Ve skutečnosti ale základní postupy pro nasazení webu tvoří relativně malou část kurzu sady.</span><span class="sxs-lookup"><span data-stu-id="b6349-120">In fact, the basic procedures for deploying a site make up a relatively small part of the tutorial set.</span></span> <span data-ttu-id="b6349-121">Ale v situacích, reálného, často potřebujete informace o některé další aspekt malý, ale důležité nasazení – například nastavení oprávnění složky na cílovém serveru.</span><span class="sxs-lookup"><span data-stu-id="b6349-121">However, in real-world situations, you often need information about some small but important extra aspect of deployment — for example, setting folder permissions on the target server.</span></span> <span data-ttu-id="b6349-122">Mnoho z těchto postupů další jsme zahrnuli v kurzech, s naděje, který kurzů k nenechávejte informace, které mohou zabránit úspěšně nasazení reálné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b6349-122">We've included many of these additional techniques in the tutorials, with the hope that the tutorials don't leave out information that might prevent you from successfully deploying a real application.</span></span>

<span data-ttu-id="b6349-123">Kurzy jsou navrženy pro spouštění v pořadí, a každou část vytvoří v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="b6349-123">The tutorials are designed to run in sequence, and each part builds on the previous part.</span></span> <span data-ttu-id="b6349-124">Nicméně, můžete přeskočit částí, které nejsou relevantní pro vaši situaci.</span><span class="sxs-lookup"><span data-stu-id="b6349-124">However, you can skip parts that aren't relevant to your situation.</span></span> <span data-ttu-id="b6349-125">(Přeskočení částí může vyžadovat upravit postupy v dalších kurzech.)</span><span class="sxs-lookup"><span data-stu-id="b6349-125">(Skipping parts might require you to adjust the procedures in later tutorials.)</span></span>

## <a name="intended-audience"></a><span data-ttu-id="b6349-126">Cílová skupina</span><span class="sxs-lookup"><span data-stu-id="b6349-126">Intended Audience</span></span>

<span data-ttu-id="b6349-127">Kurzy jsou zaměřené na vývojáře využívající technologii ASP.NET, kteří pracují v malé organizace nebo jiných prostředích kde:</span><span class="sxs-lookup"><span data-stu-id="b6349-127">The tutorials are aimed at ASP.NET developers who work in small organizations or other environments where:</span></span>

- <span data-ttu-id="b6349-128">Proces průběžnou integraci (automatizovaných sestaveních a nasazení), se nepoužije.</span><span class="sxs-lookup"><span data-stu-id="b6349-128">A continuous integration process (automated builds and deployment) is not used.</span></span>
- <span data-ttu-id="b6349-129">V provozním prostředí je poskytovatele hostitelských služeb třetích stran.</span><span class="sxs-lookup"><span data-stu-id="b6349-129">The production environment is a third-party hosting provider.</span></span>
- <span data-ttu-id="b6349-130">Jedna osoba obvykle doplní víc rolí (stejná osoba sama vyvinula, testuje a nasadí).</span><span class="sxs-lookup"><span data-stu-id="b6349-130">One person typically fills multiple roles (the same person develops, tests, and deploys).</span></span>

<span data-ttu-id="b6349-131">V podnikových prostředích je typičtější pro implementaci procesů průběžnou integraci a provozním prostředí je obvykle hostované servery vlastní společnosti.</span><span class="sxs-lookup"><span data-stu-id="b6349-131">In enterprise environments, it's more typical to implement continuous integration processes, and the production environment is usually hosted by the company's own servers.</span></span> <span data-ttu-id="b6349-132">Jiné osoby také běžně provádět různé role.</span><span class="sxs-lookup"><span data-stu-id="b6349-132">Different people also typically perform different roles.</span></span> <span data-ttu-id="b6349-133">Informace o podnikové nasazení najdete v tématu [nasazení webové aplikace v podnikové scénáře](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="b6349-133">For information about enterprise deployment, see [Deploying Web Applications in Enterprise Scenarios](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span></span>

<span data-ttu-id="b6349-134">Organizací všech velikostí můžete taky nasadit webové aplikace do Azure a většina postupy uvedené v těchto kurzech platí také pro Azure App Services Web Apps.</span><span class="sxs-lookup"><span data-stu-id="b6349-134">Organizations of all sizes can also deploy web applications to Azure, and most of the procedures shown in these tutorials apply also to Azure App Services Web Apps.</span></span> <span data-ttu-id="b6349-135">Úvod do Azure, najdete v části [https://azure.microsoft.com](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b6349-135">For an introduction to Azure, see [https://azure.microsoft.com](https://azure.microsoft.com).</span></span>

## <a name="the-hosting-provider-shown-in-the-tutorials"></a><span data-ttu-id="b6349-136">Poskytovatel hostingu ukazuje kurzů k</span><span class="sxs-lookup"><span data-stu-id="b6349-136">The Hosting Provider Shown in the Tutorials</span></span>

<span data-ttu-id="b6349-137">Kurzy vás provede procesem vytvoření účtu s hostingové společnosti a nasazení aplikace do tohoto poskytovatele hostitelských služeb.</span><span class="sxs-lookup"><span data-stu-id="b6349-137">The tutorials take you through the process of setting up an account with a hosting company and deploying the application to that hosting provider.</span></span> <span data-ttu-id="b6349-138">Konkrétní hostingové společnosti jste vybrali, takže může kurzů k objasnění dokončení prostředí nasazení na živý web.</span><span class="sxs-lookup"><span data-stu-id="b6349-138">A specific hosting company was chosen so that the tutorials could illustrate the complete experience of deploying to a live website.</span></span> <span data-ttu-id="b6349-139">Každý hostující společnost poskytuje různých funkcí a možností nasazení serverů se liší poněkud; proces popsaný v tomto kurzu se ale typické pro celý proces.</span><span class="sxs-lookup"><span data-stu-id="b6349-139">Each hosting company provides different features, and the experience of deploying to their servers varies somewhat; however, the process described in this tutorial is typical for the overall process.</span></span>

<span data-ttu-id="b6349-140">Poskytovatel hostingu používá pro účely tohoto kurzu, Cytanium.com, je jedním z mnoha, které jsou k dispozici, a jeho použití v tomto kurzu nepředstavuje ke schválení nebo doporučení.</span><span class="sxs-lookup"><span data-stu-id="b6349-140">The hosting provider used for this tutorial, Cytanium.com, is one of many that are available, and its use in this tutorial does not constitute an endorsement or recommendation.</span></span>

## <a name="deploying-web-site-projects"></a><span data-ttu-id="b6349-141">Nasazení webové projekty</span><span class="sxs-lookup"><span data-stu-id="b6349-141">Deploying Web Site Projects</span></span>

<span data-ttu-id="b6349-142">Contoso univerzity je projekt webové aplikace Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b6349-142">Contoso University is a Visual Studio web application project.</span></span> <span data-ttu-id="b6349-143">Většina metody nasazení a nástroje ukázáno v tomto kurzu se nevztahují na [webové projekty](https://msdn.microsoft.com/en-us/library/dd547590.aspx).</span><span class="sxs-lookup"><span data-stu-id="b6349-143">Most of the deployment methods and tools demonstrated in this tutorial do not apply to [Web Site Projects](https://msdn.microsoft.com/en-us/library/dd547590.aspx).</span></span> <span data-ttu-id="b6349-144">Informace o tom, jak nasadit webové projekty najdete v tématu [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/en-us/library/bb386521.aspx#deployment_for_web_site_projects).</span><span class="sxs-lookup"><span data-stu-id="b6349-144">For information about how to deploy web site projects, see [ASP.NET Deployment Content Map](https://msdn.microsoft.com/en-us/library/bb386521.aspx#deployment_for_web_site_projects).</span></span>

## <a name="deploying-aspnet-mvc-projects"></a><span data-ttu-id="b6349-145">Nasazení projekty ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b6349-145">Deploying ASP.NET MVC Projects</span></span>

<span data-ttu-id="b6349-146">V tomto kurzu nasazujete projekt webových formulářů ASP.NET, ale všechno, co zjistíte, jak to provést je také použít na rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b6349-146">For this tutorial you deploy an ASP.NET Web Forms project, but everything you learn how to do is applicable to ASP.NET MVC as well.</span></span> <span data-ttu-id="b6349-147">Projekt Visual Studio MVC je právě jinou formu projekt webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6349-147">A Visual Studio MVC project is just another form of web application project.</span></span> <span data-ttu-id="b6349-148">Jediným rozdílem je, že pokud nasazujete do poskytovatele hostitelských služeb, která nepodporuje rozhraní ASP.NET MVC nebo vaší je cílové verzi, je nutné vybrat jistotu, že jste nainstalovali odpovídající ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) nebo [MVC 4](http://nuget.org/packages/aspnetmvc)) Balíček NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="b6349-148">The only difference is that if you're deploying to a hosting provider that does not support ASP.NET MVC or your target version of it, you must make sure that you have installed the appropriate ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) or [MVC 4](http://nuget.org/packages/aspnetmvc)) NuGet package in your project.</span></span>

## <a name="programming-language"></a><span data-ttu-id="b6349-149">Programovací jazyk</span><span class="sxs-lookup"><span data-stu-id="b6349-149">Programming Language</span></span>

<span data-ttu-id="b6349-150">Ukázková aplikace používá C#, ale kurzů k nevyžadují znalosti jazyka C# a techniky nasazení zobrazí kurzů k nejsou specifické pro jazyk.</span><span class="sxs-lookup"><span data-stu-id="b6349-150">The sample application uses C# but the tutorials do not require knowledge of C#, and the deployment techniques shown by the tutorials are not language-specific.</span></span>

## <a name="troubleshooting-during-this-tutorial"></a><span data-ttu-id="b6349-151">Řešení potíží s při tomto kurzu</span><span class="sxs-lookup"><span data-stu-id="b6349-151">Troubleshooting During this Tutorial</span></span>

<span data-ttu-id="b6349-152">Když dojde k chybě během nasazení, nebo bude web nefunguje správně, neposkytují chybové zprávy vždy řešení.</span><span class="sxs-lookup"><span data-stu-id="b6349-152">When an error happens during deployment, or if the deployed site does not run correctly, the error messages don't always provide a solution.</span></span> <span data-ttu-id="b6349-153">Které vám pomohou s některé běžné scénáře problém, [stránka s referencemi k řešení potíží s](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b6349-153">To help you with some common problem scenarios, a [troubleshooting reference page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) is available.</span></span> <span data-ttu-id="b6349-154">Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak projděte následující kurzy, nezapomeňte zkontrolovat stránce řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="b6349-154">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the troubleshooting page.</span></span>

## <a name="comments-welcome"></a><span data-ttu-id="b6349-155">Vítá komentáře</span><span class="sxs-lookup"><span data-stu-id="b6349-155">Comments Welcome</span></span>

<span data-ttu-id="b6349-156">Komentáře k kurzů k jsou úvodní a při aktualizaci kurzu veškeré úsilí, budou provedeny brát v účtu opravy nebo návrhy na vylepšení, které jsou k dispozici v kurzu komentáře.</span><span class="sxs-lookup"><span data-stu-id="b6349-156">Comments on the tutorials are welcome, and when the tutorial is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in tutorial comments.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6349-157">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b6349-157">Prerequisites</span></span>

<span data-ttu-id="b6349-158">Než začnete, ujistěte se, že máte Windows 7 nebo novější a ve vašem počítači nainstalována jedna z následujících produktů:</span><span class="sxs-lookup"><span data-stu-id="b6349-158">Before you start, make sure that you have Windows 7 or later and one of the following products installed on your computer:</span></span>

- [<span data-ttu-id="b6349-159">Visual Studio 2010 SP1</span><span class="sxs-lookup"><span data-stu-id="b6349-159">Visual Studio 2010 SP1</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [<span data-ttu-id="b6349-160">Visual Web Developer Express 2010 SP1</span><span class="sxs-lookup"><span data-stu-id="b6349-160">Visual Web Developer Express 2010 SP1</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [<span data-ttu-id="b6349-161">Visual Studio 2012 RC a Visual Studio Express 2012 RC pro Web</span><span class="sxs-lookup"><span data-stu-id="b6349-161">Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web</span></span>](https://go.microsoft.com/fwlink/?LinkId=240162)

<span data-ttu-id="b6349-162">Pokud máte Visual Studio 2010 SP1 nebo Visual Web Developer Express 2010 SP1, nainstalujte také následující produkty:</span><span class="sxs-lookup"><span data-stu-id="b6349-162">If you have Visual Studio 2010 SP1 or Visual Web Developer Express 2010 SP1, install the following products also:</span></span>

- <span data-ttu-id="b6349-163">[Azure SDK pro platformu .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (zahrnuje aktualizace Publikovat Web)</span><span class="sxs-lookup"><span data-stu-id="b6349-163">[Azure SDK for .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (includes the Web Publish Update)</span></span>
- [<span data-ttu-id="b6349-164">Nástroje Microsoft Visual Studio 2010 SP1 pro systém SQL Server Compact 4.0</span><span class="sxs-lookup"><span data-stu-id="b6349-164">Microsoft Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

<span data-ttu-id="b6349-165">Některé další software je vyžadováno k dokončení tohoto kurzu, ale nemáte nastaven, který dosud načtena.</span><span class="sxs-lookup"><span data-stu-id="b6349-165">Some other software is required in order to complete the tutorial, but you don't have to have that loaded yet.</span></span> <span data-ttu-id="b6349-166">Tento kurz vás provede kroky pro instalaci se v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="b6349-166">The tutorial will walk you through the steps for installing it when you need it.</span></span>

## <a name="downloading-the-sample-application"></a><span data-ttu-id="b6349-167">Stažení ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="b6349-167">Downloading the Sample Application</span></span>

<span data-ttu-id="b6349-168">Aplikace, které budete nasazovat jmenuje Contoso univerzity a již bylo vytvořeno za vás.</span><span class="sxs-lookup"><span data-stu-id="b6349-168">The application you'll deploy is named Contoso University and has already been created for you.</span></span> <span data-ttu-id="b6349-169">Je zjednodušenou verzi webu univerzity, volně podle aplikace Contoso univerzity podrobněji popsaná [Entity Framework – kurzy na webu technologie ASP.NET](https://asp.net/entity-framework/tutorials).</span><span class="sxs-lookup"><span data-stu-id="b6349-169">It's a simplified version of a university web site, based loosely on the Contoso University application described in the [Entity Framework tutorials on the ASP.NET site](https://asp.net/entity-framework/tutorials).</span></span>

<span data-ttu-id="b6349-170">Až budete mít nainstalovány požadované součásti, stáhněte si [webové aplikace Contoso univerzity](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b).</span><span class="sxs-lookup"><span data-stu-id="b6349-170">When you have the prerequisites installed, download the [Contoso University web application](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b).</span></span> <span data-ttu-id="b6349-171">*.Zip* soubor obsahuje více verzí projekt a soubor PDF, který obsahuje všechny 12 kurzy.</span><span class="sxs-lookup"><span data-stu-id="b6349-171">The *.zip* file contains multiple versions of the project and a PDF file that contains all 12 tutorials.</span></span> <span data-ttu-id="b6349-172">Chcete-li provede kroky kurzu, začněte ContosoUniversity Begin.</span><span class="sxs-lookup"><span data-stu-id="b6349-172">To work through the steps of the tutorial, start with ContosoUniversity-Begin.</span></span> <span data-ttu-id="b6349-173">Pokud chcete zjistit, co projektu vypadá na konci kurzů k, otevřete ContosoUniversity-End.</span><span class="sxs-lookup"><span data-stu-id="b6349-173">To see what the project looks like at the end of the tutorials, open ContosoUniversity-End.</span></span> <span data-ttu-id="b6349-174">Pokud chcete zobrazit, co projektu vypadá jako před migrací na plnou instalaci systému SQL Server v kurzu 10, otevřete ContosoUniversity AfterTutorial09.</span><span class="sxs-lookup"><span data-stu-id="b6349-174">To see what the project looks like before the migration to full SQL Server in tutorial 10, open ContosoUniversity-AfterTutorial09.</span></span>

<span data-ttu-id="b6349-175">Pokud chcete připravit na kurz kroky spolupracovat, uložte ContosoUniversity Begin do libovolné složky použijete pro práci s projektů sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b6349-175">To prepare to work through the tutorial steps, save ContosoUniversity-Begin to whatever folder you use for working with Visual Studio projects.</span></span> <span data-ttu-id="b6349-176">Ve výchozím nastavení je to následující složku:</span><span class="sxs-lookup"><span data-stu-id="b6349-176">By default this is the following folder:</span></span>

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

<span data-ttu-id="b6349-177">(Pro snímky obrazovky v tomto kurzu, složce projektu se nachází v kořenovém adresáři na `C`: jednotku.)</span><span class="sxs-lookup"><span data-stu-id="b6349-177">(For the screen shots in this tutorial, the project folder is located in the root directory on the `C`: drive.)</span></span>

<span data-ttu-id="b6349-178">Spuštění sady Visual Studio, otevřete projekt a stiskněte klávesu CTRL + F5 ji spustit.</span><span class="sxs-lookup"><span data-stu-id="b6349-178">Start Visual Studio, open the project, and press CTRL-F5 to run it.</span></span>

<span data-ttu-id="b6349-179">[![Home_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b6349-179">[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)</span></span>

<span data-ttu-id="b6349-180">Webových stránek jsou přístupné z řádku nabídek a umožňují provádět následující funkce:</span><span class="sxs-lookup"><span data-stu-id="b6349-180">The website pages are accessible from the menu bar and let you perform the following functions:</span></span>

- <span data-ttu-id="b6349-181">Zobrazí statistické údaje o student (stránku o).</span><span class="sxs-lookup"><span data-stu-id="b6349-181">Display student statistics (the About page).</span></span>
- <span data-ttu-id="b6349-182">Zobrazit, upravit, odstranit a přidejte studenty.</span><span class="sxs-lookup"><span data-stu-id="b6349-182">Display, edit, delete, and add students.</span></span>
- <span data-ttu-id="b6349-183">Zobrazit a upravit kurzy.</span><span class="sxs-lookup"><span data-stu-id="b6349-183">Display and edit courses.</span></span>
- <span data-ttu-id="b6349-184">Zobrazení a úpravě vyučující.</span><span class="sxs-lookup"><span data-stu-id="b6349-184">Display and edit instructors.</span></span>
- <span data-ttu-id="b6349-185">Zobrazení a úpravě oddělení.</span><span class="sxs-lookup"><span data-stu-id="b6349-185">Display and edit departments.</span></span>

<span data-ttu-id="b6349-186">Tady jsou snímky obrazovky několik reprezentativní stránek.</span><span class="sxs-lookup"><span data-stu-id="b6349-186">Following are screen shots of a few representative pages.</span></span>

<span data-ttu-id="b6349-187">[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b6349-187">[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)</span></span>

<span data-ttu-id="b6349-188">[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b6349-188">[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)</span></span>

## <a name="reviewing-application-features-that-affect-deployment"></a><span data-ttu-id="b6349-189">Kontrola funkce aplikace, které ovlivňují nasazení</span><span class="sxs-lookup"><span data-stu-id="b6349-189">Reviewing Application Features that Affect Deployment</span></span>

<span data-ttu-id="b6349-190">Následující funkce aplikace budou mít vliv nasazení nebo co musíte udělat, aby ho nasadit.</span><span class="sxs-lookup"><span data-stu-id="b6349-190">The following features of the application affect how you deploy it or what you have to do to deploy it.</span></span> <span data-ttu-id="b6349-191">Každý z nich je vysvětlené podrobněji v následujících kurzech v řadě.</span><span class="sxs-lookup"><span data-stu-id="b6349-191">Each of these is explained in more detail in the following tutorials in the series.</span></span>

- <span data-ttu-id="b6349-192">Contoso univerzity používá systém SQL Server Compact databázi k ukládání dat aplikací, například studenty a lektorem názvy.</span><span class="sxs-lookup"><span data-stu-id="b6349-192">Contoso University uses a SQL Server Compact database to store application data such as student and instructor names.</span></span> <span data-ttu-id="b6349-193">Databáze obsahuje směs testovací data a provozními daty, a při nasazení do produkčního prostředí je potřeba vyloučit v testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="b6349-193">The database contains a mix of test data and production data, and when you deploy to production you need to exclude the test data.</span></span> <span data-ttu-id="b6349-194">Později v kurzu řady budete migraci ze systému SQL Server Compact do systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b6349-194">Later in the tutorial series you'll migrate from SQL Server Compact to SQL Server.</span></span>
- <span data-ttu-id="b6349-195">Aplikace používá systém členství technologie ASP.NET, které jsou uloženy informace o uživatelském účtu v databázi systému SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="b6349-195">The application uses the ASP.NET membership system, which stores user account information in a SQL Server Compact database.</span></span> <span data-ttu-id="b6349-196">Aplikace definuje uživatel s oprávněním správce, který má přístup k určité omezené informace.</span><span class="sxs-lookup"><span data-stu-id="b6349-196">The application defines an administrator user who has access to some restricted information.</span></span> <span data-ttu-id="b6349-197">Je třeba nasadit bez testovací účty, ale jeden účet správce databáze členství.</span><span class="sxs-lookup"><span data-stu-id="b6349-197">You need to deploy the membership database without test accounts but with one administrator account.</span></span>
- <span data-ttu-id="b6349-198">Vzhledem k databázi aplikace a databáze členství používá systém SQL Server Compact jako databázový stroj, je nutné nasadit databázový stroj pro poskytovatele hostingu, jakož i samotných databázích.</span><span class="sxs-lookup"><span data-stu-id="b6349-198">Because the application database and the membership database use SQL Server Compact as the database engine, you have to deploy the database engine to the hosting provider, as well as the databases themselves.</span></span>
- <span data-ttu-id="b6349-199">Aplikace používá zprostředkovatelů universal členství prostředí ASP.NET, aby systém členství můžete uložit svá data v databázi systému SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="b6349-199">The application uses ASP.NET universal membership providers so that the membership system can store its data in a SQL Server Compact database.</span></span> <span data-ttu-id="b6349-200">Sestavení, které obsahuje universal členství zprostředkovatele musí být nasazen s aplikací.</span><span class="sxs-lookup"><span data-stu-id="b6349-200">The assembly that contains the universal membership providers must be deployed with the application.</span></span>
- <span data-ttu-id="b6349-201">Aplikace používá Entity Framework 5.0 pro přístup k datům v databázi aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6349-201">The application uses the Entity Framework 5.0 to access data in the application database.</span></span> <span data-ttu-id="b6349-202">Sestavení, které obsahuje Entity Framework 5.0 musí být nasazen s aplikací.</span><span class="sxs-lookup"><span data-stu-id="b6349-202">The assembly that contains Entity Framework 5.0 must be deployed with the application.</span></span>
- <span data-ttu-id="b6349-203">Aplikace používá chyba třetích stran, protokolování a vytváření sestav nástroje.</span><span class="sxs-lookup"><span data-stu-id="b6349-203">The application uses a third-party error logging and reporting utility.</span></span> <span data-ttu-id="b6349-204">Tento nástroj je součástí sestavení, které musí být nasazeno s aplikací.</span><span class="sxs-lookup"><span data-stu-id="b6349-204">This utility is provided in an assembly which must be deployed with the application.</span></span>
- <span data-ttu-id="b6349-205">Nástroj protokolování chyba zapisuje informace o chybě v souborů XML do sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="b6349-205">The error logging utility writes error information in XML files to a file folder.</span></span> <span data-ttu-id="b6349-206">Máte a ujistěte se, že účet, který technologie ASP.NET spuštěna pod v nasazené lokality má oprávnění k zápisu do této složky a musíte tuto složku vyloučit z nasazení.</span><span class="sxs-lookup"><span data-stu-id="b6349-206">You have to make sure that the account that ASP.NET runs under in the deployed site has write permission to this folder, and you have to exclude this folder from deployment.</span></span> <span data-ttu-id="b6349-207">(, Jinak chyba data protokolu z testovacího prostředí je možno nasadit do produkčního prostředí nebo soubory protokolů chyba produkční může odstranit.)</span><span class="sxs-lookup"><span data-stu-id="b6349-207">(Otherwise, error log data from the test environment might be deployed to production and/or production error log files might be deleted.)</span></span>
- <span data-ttu-id="b6349-208">Aplikace obsahuje některá nastavení, které je potřeba změnit v v nasazené *Web.config* souboru v závislosti na cílovém prostředí (testu nebo produkční) a další nastavení, které je potřeba změnit v závislosti na sestavení konfigurace (ladění nebo verze).</span><span class="sxs-lookup"><span data-stu-id="b6349-208">The application includes some settings that must be changed in in the deployed *Web.config* file depending on the destination environment (test or production), and other settings that must be changed depending on the build configuration (Debug or Release).</span></span>
- <span data-ttu-id="b6349-209">Řešení nástroje Visual Studio obsahuje projektu knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="b6349-209">The Visual Studio solution includes a class library project.</span></span> <span data-ttu-id="b6349-210">Pouze sestavení, které generuje tento projekt by měl být nasazen, nikoli projekt.</span><span class="sxs-lookup"><span data-stu-id="b6349-210">Only the assembly that this project generates should be deployed, not the project itself.</span></span>

<span data-ttu-id="b6349-211">V tomto prvním kurzu v řadě byly staženy ukázkový projekt sady Visual Studio a zkontrolovat lokality funkce, které ovlivňují nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6349-211">In this first tutorial in the series, you have downloaded the sample Visual Studio project and reviewed site features that affect how you deploy the application.</span></span> <span data-ttu-id="b6349-212">V následujících kurzech připravíte nasazení nastavením některé z těchto věcí automaticky zpracovávat.</span><span class="sxs-lookup"><span data-stu-id="b6349-212">In the following tutorials, you prepare for deployment by setting up some of these things to be handled automatically.</span></span> <span data-ttu-id="b6349-213">Ostatní můžete postará o ručně.</span><span class="sxs-lookup"><span data-stu-id="b6349-213">Others you take care of manually.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="b6349-214">Další</span><span class="sxs-lookup"><span data-stu-id="b6349-214">Next</span></span>](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)