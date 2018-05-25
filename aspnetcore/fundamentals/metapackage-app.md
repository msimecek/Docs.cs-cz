---
title: Microsoft.AspNetCore.App metapackage pro ASP.NET Core 2.1 a novější
author: Rick-Anderson
description: Microsoft.AspNetCore.App metapackage zahrnuje všechny podporované balíčků ASP.NET Core a Entity Framework Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage-app
ms.openlocfilehash: 7c7f69a6176d3f7982a67106cb823ff42200b50e
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/18/2018
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a><span data-ttu-id="e2346-103">Microsoft.AspNetCore.App metapackage pro ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="e2346-103">Microsoft.AspNetCore.App metapackage for ASP.NET Core 2.1</span></span>

<span data-ttu-id="e2346-104">Tato funkce vyžaduje ASP.NET Core, 2,1 a novější cílení na rozhraní .NET Core 2.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e2346-104">This feature requires ASP.NET Core 2.1 and later targeting .NET Core 2.1 and later.</span></span>

<span data-ttu-id="e2346-105">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [metapackage](/dotnet/core/packages#metapackages) pro ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e2346-105">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [metapackage](/dotnet/core/packages#metapackages) for ASP.NET Core:</span></span>

* <span data-ttu-id="e2346-106">Nezahrnuje závislosti třetích stran s výjimkou [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), a [IX asynchronní](https://www.nuget.org/packages/System.Interactive.Async/).</span><span class="sxs-lookup"><span data-stu-id="e2346-106">Does not include third-party dependencies except for [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), and [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/).</span></span> <span data-ttu-id="e2346-107">Tyto závislosti 3. stran se považují za nezbytné pro zajištění architektury hlavní funkce funkce.</span><span class="sxs-lookup"><span data-stu-id="e2346-107">These 3rd-party dependencies are deemed necessary to ensure the major frameworks features function.</span></span>
* <span data-ttu-id="e2346-108">Zahrnuje všechny podporované balíčky tým ASP.NET Core kromě těch, které obsahují závislosti třetích stran (jiné než uvedené výše).</span><span class="sxs-lookup"><span data-stu-id="e2346-108">Includes all supported packages by the ASP.NET Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>
* <span data-ttu-id="e2346-109">Zahrnuje všechny podporované balíčky tým Entity Framework Core kromě těch, které obsahují závislosti třetích stran (jiné než uvedené výše).</span><span class="sxs-lookup"><span data-stu-id="e2346-109">Includes all supported packages by the Entity Framework Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>

<span data-ttu-id="e2346-110">Všechny funkce ASP.NET Core 2.1 a novější a Entity Framework Core 2.1 a vyšší jsou součástí `Microsoft.AspNetCore.App` balíčku.</span><span class="sxs-lookup"><span data-stu-id="e2346-110">All the features of ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later are included in the `Microsoft.AspNetCore.App` package.</span></span> <span data-ttu-id="e2346-111">Výchozí šablony cílení na ASP.NET Core 2.1 projektu a později tento balíček použít.</span><span class="sxs-lookup"><span data-stu-id="e2346-111">The default project templates targeting ASP.NET Core 2.1 and later use this package.</span></span> <span data-ttu-id="e2346-112">Doporučujeme aplikace cílené na ASP.NET Core, 2,1 a novější a Entity Framework Core 2.1 a později `Microsoft.AspNetCore.App` balíčku.</span><span class="sxs-lookup"><span data-stu-id="e2346-112">We recommend applications targeting ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later use the `Microsoft.AspNetCore.App` package.</span></span>

<span data-ttu-id="e2346-113">Číslo verze `Microsoft.AspNetCore.App` metapackage představuje ASP.NET Core verze a verze Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e2346-113">The version number of the `Microsoft.AspNetCore.App` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="e2346-114">Pomocí `Microsoft.AspNetCore.App` metapackage poskytuje verze omezení, které chrání aplikace:</span><span class="sxs-lookup"><span data-stu-id="e2346-114">Using the `Microsoft.AspNetCore.App` metapackage provides version restrictions that protect your app:</span></span>

* <span data-ttu-id="e2346-115">Pokud je balíček zahrnuté, který má přenositelné (ne direct) závislost na balíček v `Microsoft.AspNetCore.App`a tato čísla verze se liší, NuGet dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="e2346-115">If a package is included that has a transitive (not direct) dependency on a package in `Microsoft.AspNetCore.App`, and those version numbers differ, NuGet will generate an error.</span></span>
* <span data-ttu-id="e2346-116">Další balíčky přidat do vaší aplikace nelze změnit verzi balíčků, které jsou součástí `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e2346-116">Other packages added to your app cannot change the version of packages included in `Microsoft.AspNetCore.App`.</span></span>
* <span data-ttu-id="e2346-117">Verze konzistence zajišťuje spolehlivé prostředí.</span><span class="sxs-lookup"><span data-stu-id="e2346-117">Version consistency ensures a reliable experience.</span></span> <span data-ttu-id="e2346-118">`Microsoft.AspNetCore.App` byla navržená tak, aby se zabránilo kombinace netestované verze související služby bits se použít společně ve stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e2346-118">`Microsoft.AspNetCore.App` was designed to prevent untested version combinations of related bits being used together in the same app.</span></span>

<span data-ttu-id="e2346-119">Aplikace, které používají `Microsoft.AspNetCore.App` metapackage automaticky využít sdílené rozhraní ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2346-119">Applications that use the `Microsoft.AspNetCore.App` metapackage automatically take advantage of the ASP.NET Core shared framework.</span></span> <span data-ttu-id="e2346-120">Při použití `Microsoft.AspNetCore.App` metapackage, **žádné** prostředky z odkazované balíčky ASP.NET Core NuGet nasazených aplikací &mdash; sdílené rozhraní ASP.NET Core obsahuje tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="e2346-120">When you use the `Microsoft.AspNetCore.App` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the ASP.NET Core shared framework contains these assets.</span></span> <span data-ttu-id="e2346-121">Prostředky v rámci sdílené jsou předkompilovaných ke zlepšení času spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2346-121">The assets in the shared framework are precompiled to improve application startup time.</span></span> <span data-ttu-id="e2346-122">Další informace najdete v tématu "sdílený framework" v [.NET Core distribuční balení](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="e2346-122">For more information, see "shared framework" in [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

<span data-ttu-id="e2346-123">Následující *.csproj* odkazů na soubor projektu `Microsoft.AspNetCore.App` metapackage pro ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e2346-123">The following *.csproj* project file references the `Microsoft.AspNetCore.App` metapackage for ASP.NET Core:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>

```

<span data-ttu-id="e2346-124">Předchozí kód představuje typické ASP.NET Core 2.1 a novější šablony.</span><span class="sxs-lookup"><span data-stu-id="e2346-124">The preceding markup represents a typical ASP.NET Core 2.1 and later template.</span></span> <span data-ttu-id="e2346-125">Neurčuje číslo verze `Microsoft.AspNetCore.App` balíček odkaz.</span><span class="sxs-lookup"><span data-stu-id="e2346-125">It doesn't specify a version number for the `Microsoft.AspNetCore.App` package reference.</span></span> <span data-ttu-id="e2346-126">Pokud není zadána verze, [implicitní](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) je sadou SDK, který je určená verze `Microsoft.NET.Sdk.Web`.</span><span class="sxs-lookup"><span data-stu-id="e2346-126">When the version is not specified, an [implicit](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) version is specified by the SDK, that is, `Microsoft.NET.Sdk.Web`.</span></span> <span data-ttu-id="e2346-127">Doporučujeme, abyste spoléhat na implicitní verze určeného sady SDK a není explicitně nastavení číslo verze na odkaz na balíček.</span><span class="sxs-lookup"><span data-stu-id="e2346-127">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="e2346-128">Můžete nechat v Githubu komentář [diskusní pro verzi implicitní Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).</span><span class="sxs-lookup"><span data-stu-id="e2346-128">You can leave a GitHub comment at [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430).</span></span>

<span data-ttu-id="e2346-129">Implicitní verze je nastaveno na `major.minor.0` pro přenosné aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2346-129">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="e2346-130">Mechanismus úplné dopředné sdílený framework se spustí aplikace na nejnovější verzi kompatibilní mezi nainstalovaných sdílené architektury.</span><span class="sxs-lookup"><span data-stu-id="e2346-130">The shared framework roll-forward mechanism will run the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="e2346-131">Pokud chcete zajistit, že se stejnou verzí se používá v vývoj, testování a provozním, zkontrolujte, jestli že je stejnou verzi nástroje sdílený framework nainstalované ve všech prostředích.</span><span class="sxs-lookup"><span data-stu-id="e2346-131">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="e2346-132">Pro vlastní aplikace, číslo verze implicitní nastavena na `major.minor.patch` sdílený Framework dodávat v sadě SDK nainstalované.</span><span class="sxs-lookup"><span data-stu-id="e2346-132">For self contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="e2346-133">Zadání číslo verze na `Microsoft.AspNetCore.App` nemá odkaz na **není** zaručit, že tato verze sdílený framework nastavení bude vybrána.</span><span class="sxs-lookup"><span data-stu-id="e2346-133">Specifying a version number on the `Microsoft.AspNetCore.App` reference does **not** guarantee that version of the shared framework will be chosen.</span></span> <span data-ttu-id="e2346-134">Předpokládejme například, je určená verze "2.1.1", ale je nainstalována "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="e2346-134">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="e2346-135">V takovém případě aplikace bude používat "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="e2346-135">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="e2346-136">I když není doporučeno, můžete zakázat dopředné posunutí (opravy a/nebo menší).</span><span class="sxs-lookup"><span data-stu-id="e2346-136">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="e2346-137">Další informace o hostiteli dotnet úplné dopředné a postup konfigurace své chování najdete v tématu [dotnet hostitele dopředné posunutí](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span><span class="sxs-lookup"><span data-stu-id="e2346-137">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="e2346-138">`Microsoft.AspNetCore.App` [Metapackage](/dotnet/core/packages#metapackages) není tradiční balíček, který je aktualizována z NuGet.</span><span class="sxs-lookup"><span data-stu-id="e2346-138">The `Microsoft.AspNetCore.App` [metapackage](/dotnet/core/packages#metapackages) is not a traditional package that is updated from NuGet.</span></span> <span data-ttu-id="e2346-139">Podobně jako `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` představuje sdílený modul runtime, který se má zpracovat mimo NuGet sémantiku speciální správu verzí.</span><span class="sxs-lookup"><span data-stu-id="e2346-139">Similar to `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` represents a shared runtime, which has special versioning semantics handled outside of NuGet.</span></span> <span data-ttu-id="e2346-140">Další informace najdete v tématu [balíčky, metapackages a architektur](/dotnet/core/packages).</span><span class="sxs-lookup"><span data-stu-id="e2346-140">For more information, see [Packages, metapackages and frameworks](/dotnet/core/packages).</span></span>

<span data-ttu-id="e2346-141">Pokud vaše aplikace už dřív použili `Microsoft.AspNetCore.All`, najdete v části [migrace z Microsoft.AspNetCore.All na Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).</span><span class="sxs-lookup"><span data-stu-id="e2346-141">If your application previously used `Microsoft.AspNetCore.All`, see [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).</span></span>