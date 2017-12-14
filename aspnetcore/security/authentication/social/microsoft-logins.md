---
title: "Nastavení Microsoft Account externí přihlášení"
author: rick-anderson
description: "Tento kurz představuje integraci ověřování uživatele účtu Microsoft do existující aplikace ASP.NET Core."
keywords: "ASP.NET Core, účet Microsoft, přihlášení, ověřování"
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.assetid: 66DB4B94-C78C-4005-BA03-3D982B87C268
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 77c16e3ae93c9bfe1f569d0a5888c5b765d04241
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-microsoft-account-authentication"></a><span data-ttu-id="1e7f5-104">Konfigurace ověřování Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="1e7f5-104">Configuring Microsoft Account authentication</span></span>

<span data-ttu-id="1e7f5-105">Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1e7f5-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1e7f5-106">V tomto kurzu se dozvíte, jak povolit uživatelům přihlásit se pomocí svého účtu Microsoft pomocí projektu ASP.NET 2.0 základní ukázka na vytvořit [předchozí stránce](index.md).</span><span class="sxs-lookup"><span data-stu-id="1e7f5-106">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="1e7f5-107">Vytvoření aplikace v portálu pro vývojáře společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="1e7f5-107">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="1e7f5-108">Přejděte na [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) a vytvořte nebo se přihlaste účtem Microsoft:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-108">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Přihlaste se dialogové okno](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="1e7f5-110">Pokud nemáte účet Microsoft, klepněte na  **[vytvořit!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="1e7f5-110">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="1e7f5-111">Po přihlášení budete přesměrováni na **Moje aplikace** stránky:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-111">After signing in you are redirected to **My applications** page:</span></span>

![Otevřete v Microsoft Edge portál pro vývojáře společnosti Microsoft](index/_static/MicrosoftDev.png)

* <span data-ttu-id="1e7f5-113">Klepněte na **přidat aplikaci** v pravém horním rohu a zadejte vaše **název aplikace** a **e-mailu kontaktujte**:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-113">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Dialogové okno Nový registraci aplikace](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="1e7f5-115">Pro účely tohoto kurzu, zrušte **instalace na základě** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-115">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="1e7f5-116">Klepněte na **vytvořit** nadále **registrace** stránky.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-116">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="1e7f5-117">Zadejte **název** a poznamenejte si hodnotu **Id aplikace**, který použijete jako `ClientId` dál v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-117">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Registrační stránce](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="1e7f5-119">Klepněte na **přidejte platformu** v **platformy** a vyberte **webové** platformy:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-119">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Přidejte platformu dialogové okno](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="1e7f5-121">V novém **webové** platformy zadejte URL vývoj s */signin-microsoft* připojí do **adres URL pro přesměrování** pole (například: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="1e7f5-121">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="1e7f5-122">Schéma ověřování Microsoft nakonfigurované později v tomto kurzu bude automaticky zpracovávat požadavky na */signin-microsoft* trasy k implementaci toku OAuth:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-122">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![Webové části platforma](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="1e7f5-124">Klepněte na **přidat adresu URL** aby adresa URL byla přidána.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-124">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="1e7f5-125">V případě potřeby zadejte další nastavení aplikace a klepněte na **Uložit** v dolní části stránky se uložit změny do konfigurace aplikací.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-125">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="1e7f5-126">Při nasazování webu budete potřebovat k pokroku **registrace** stránky a nastavte novou veřejnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-126">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="1e7f5-127">Uložit aplikaci Microsoft Id a heslo</span><span class="sxs-lookup"><span data-stu-id="1e7f5-127">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="1e7f5-128">Poznámka: `Application Id` zobrazí na **registrace** stránky.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-128">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="1e7f5-129">Klepněte na **generovat nové heslo** v **tajné klíče aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-129">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="1e7f5-130">Zobrazí se v poli, kde můžete kopírovat heslo aplikace:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-130">This displays a box where you can copy the application password:</span></span>

![Dialogové okno Nový hesla generovaného](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="1e7f5-132">Odkaz citlivá nastavení, jako je Microsoft `Application ID` a `Password` do konfigurace vaší aplikace pomocí [tajný klíč správce](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="1e7f5-132">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="1e7f5-133">Pro účely tohoto kurzu, název tokeny `Authentication:Microsoft:ApplicationId` a `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-133">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="1e7f5-134">Konfigurovat ověřování účet Microsoft</span><span class="sxs-lookup"><span data-stu-id="1e7f5-134">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="1e7f5-135">Šablona projektu použili v tomto kurzu zajistí, že [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) balíček je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-135">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="1e7f5-136">Chcete-li nainstalovat tento balíček s Visual Studio 2017, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-136">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="1e7f5-137">Chcete-li nainstalovat s .NET Core rozhraní příkazového řádku, spusťte následující v adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-137">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1e7f5-138">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="1e7f5-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1e7f5-139">Přidání služby Account Microsoft `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1e7f5-140">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="1e7f5-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1e7f5-141">Přidat middlewaru Microsoft Account v `Configure` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-141">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="1e7f5-142">I když tyto tokeny názvy technologiím použitým na portál pro vývojáře společnosti Microsoft `ApplicationId` a `Password`, jsou zveřejněné jako `ClientId` a `ClientSecret` v konfiguraci rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-142">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they are exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="1e7f5-143">Najdete v článku [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) referenční dokumentace rozhraní API pro další informace o možností konfigurace podporovanou Account Microsoft ověřování.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-143">See the [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="1e7f5-144">To slouží k požadavku na jiné informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-144">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="1e7f5-145">Přihlaste se pomocí účtu Microsoft</span><span class="sxs-lookup"><span data-stu-id="1e7f5-145">Sign in with Microsoft Account</span></span>

<span data-ttu-id="1e7f5-146">Spusťte aplikaci a klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="1e7f5-147">Zobrazí se možnost přihlásit se pomocí Microsoft:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-147">An option to sign in with Microsoft appears:</span></span>

![Webovou aplikaci protokolu na stránce: uživatel není ověřen.](index/_static/DoneMicrosoft.png)

<span data-ttu-id="1e7f5-149">Když kliknete na Microsoft, budete přesměrováni do společnosti Microsoft pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-149">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="1e7f5-150">Po přihlášení pomocí Account Microsoft (pokud ještě není přihlášení) se zobrazí výzva k umožňuje aplikaci přístup k informacím:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-150">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Dialogové okno Microsoft ověřování](index/_static/MicrosoftLogin.png)

<span data-ttu-id="1e7f5-152">Klepněte na **Ano** a budete přesměrováni zpět na webovou stránku, kde můžete nastavit e-mailu.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-152">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="1e7f5-153">Nyní jste se přihlásili pomocí přihlašovacích údajů společnosti Microsoft:</span><span class="sxs-lookup"><span data-stu-id="1e7f5-153">You are now logged in using your Microsoft credentials:</span></span>

![Webové aplikace: uživatel ověřený](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="1e7f5-155">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="1e7f5-155">Troubleshooting</span></span>

* <span data-ttu-id="1e7f5-156">Pokud zprostředkovatel Account Microsoft vás přesměruje na přihlašovací stránce chyba, vezměte na vědomí chyba název a popis parametrů řetězce dotazu přímo následující `#` (hashtag) v identifikátoru Uri.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-156">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="1e7f5-157">I když se chybová zpráva se zdá, že znamenat problém související s ověřováním Microsoft, nejčastější příčinou je identifikátor Uri neodpovídá žádné z vaší aplikace **identifikátory URI přesměrování** zadaná pro **webové** platformy .</span><span class="sxs-lookup"><span data-stu-id="1e7f5-157">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="1e7f5-158">**ASP.NET Core 2.x pouze:** pokud identita není nastavena voláním `services.AddIdentity` v `ConfigureServices`, pokusu o ověření bude mít za následek *ArgumentException –: musí být použita volba 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-158">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="1e7f5-159">Šablona projektu použili v tomto kurzu zajistí, že to probíhá.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="1e7f5-160">Pokud databázi lokality s použitím počáteční migrace vytvořena nebyla, zobrazí se *databázová operace se nezdařila při zpracování požadavku* chyby.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-160">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="1e7f5-161">Klepněte na **použít migrace** vytvořit databázi a aktualizujte pokračujte dále chyba.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e7f5-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e7f5-162">Next steps</span></span>

* <span data-ttu-id="1e7f5-163">Tento článek vám ukázal, jak můžete ověřovat se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-163">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="1e7f5-164">Můžete postupovat podle podobný postup k ověření pomocí jiných poskytovatelů uvedené na [předchozí stránce](index.md).</span><span class="sxs-lookup"><span data-stu-id="1e7f5-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="1e7f5-165">Jakmile budete publikovat web vaší webové aplikace Azure, měli byste vytvořit nový `Password` v portálu pro vývojáře společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-165">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="1e7f5-166">Nastavte `Authentication:Microsoft:ApplicationId` a `Authentication:Microsoft:Password` jako nastavení aplikace v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-166">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="1e7f5-167">Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="1e7f5-167">The configuration system is set up to read keys from environment variables.</span></span>