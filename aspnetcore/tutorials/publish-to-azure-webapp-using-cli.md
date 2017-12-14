---
title: "Publikování aplikace ASP.NET Core do Azure pomocí pomocí nástroje příkazového řádku | Microsoft Docs"
description: "Postup vytvoření a nasazení aplikace Microsoft Azure pomocí ASP.NET Core a klient příkazového řádku Gitu."
services: multiple
keywords: "ASP.NET Core, Azure, služby App Service, Git, příkazového řádku"
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0bcff4f79356b960f663dcebb1d79a108417dbd2
ms.sourcegitcommit: f017f940a164dbaf84307410c78eb14e0f3ac811
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="96fef-104">Nasazení aplikace ASP.NET Core do služby Azure App Service z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="96fef-104">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="96fef-105">Podle [Soper kamera](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="96fef-105">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="96fef-106">Tento kurz vám ukáže, jak vytvářet a nasazovat aplikaci ASP.NET Core ke službě Microsoft Azure App Service pomocí nástrojů příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="96fef-106">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="96fef-107">Po dokončení, budete mít webovou aplikaci v ASP.NET MVC Core hostované jako webová aplikace Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="96fef-107">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="96fef-108">V tomto kurzu je zapsána pomocí nástroje příkazového řádku Windows, ale můžete použít a Linux prostředí také systému macOS.</span><span class="sxs-lookup"><span data-stu-id="96fef-108">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="96fef-109">V tomto kurzu zjistíte, jak:</span><span class="sxs-lookup"><span data-stu-id="96fef-109">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="96fef-110">Vytvoření webu k službě Azure App Service pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="96fef-110">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="96fef-111">Nasazení aplikace ASP.NET Core Azure App Service pomocí nástroje příkazového řádku Git</span><span class="sxs-lookup"><span data-stu-id="96fef-111">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96fef-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="96fef-112">Prerequisites</span></span>

<span data-ttu-id="96fef-113">K dokončení tohoto kurzu, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="96fef-113">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="96fef-114">A [předplatné Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="96fef-114">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="96fef-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="96fef-115">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="96fef-116">[Git](https://www.git-scm.com/) klienta příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="96fef-116">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="96fef-117">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="96fef-117">Create a web application</span></span>

<span data-ttu-id="96fef-118">Vytvořte nový adresář pro webovou aplikaci, vytvořte novou aplikaci ASP.NET MVC jádra a poté spuštěn místně na webu.</span><span class="sxs-lookup"><span data-stu-id="96fef-118">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="96fef-119">Windows</span><span class="sxs-lookup"><span data-stu-id="96fef-119">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="96fef-120">Jiné</span><span class="sxs-lookup"><span data-stu-id="96fef-120">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Výstup příkazového řádku](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="96fef-122">Testování aplikace procházením http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="96fef-122">Test the application by browsing to http://localhost:5000.</span></span>

![Web spuštěn místně](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="96fef-124">Vytvoření instance služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="96fef-124">Create the Azure App Service instance</span></span>

<span data-ttu-id="96fef-125">Pomocí [prostředí cloudu Azure](/azure/cloud-shell/quickstart), vytvořte skupinu prostředků, plán služby App Service a webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="96fef-125">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="96fef-126">Před nasazením nastavte přihlašovací údaje nasazení úrovni účtu, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="96fef-126">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="96fef-127">Adresa URL nasazení je potřeba k nasazení aplikace pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="96fef-127">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="96fef-128">Získat adresu URL takto.</span><span class="sxs-lookup"><span data-stu-id="96fef-128">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="96fef-129">Poznámka: na uvedené adrese URL končící na `.git`.</span><span class="sxs-lookup"><span data-stu-id="96fef-129">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="96fef-130">Používá se v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="96fef-130">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="96fef-131">Nasazení aplikace pomocí Git</span><span class="sxs-lookup"><span data-stu-id="96fef-131">Deploy the application using Git</span></span>

<span data-ttu-id="96fef-132">Jste připraveni k nasazení z místního počítače pomocí Git.</span><span class="sxs-lookup"><span data-stu-id="96fef-132">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="96fef-133">Je bezpečné ignorovat všechna upozornění z Git o konce řádků.</span><span class="sxs-lookup"><span data-stu-id="96fef-133">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="96fef-134">Windows</span><span class="sxs-lookup"><span data-stu-id="96fef-134">Windows</span></span>](#tab/windows)
```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="96fef-135">Jiné</span><span class="sxs-lookup"><span data-stu-id="96fef-135">Other</span></span>](#tab/other)
```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```
---

<span data-ttu-id="96fef-136">Git zobrazí výzvu pro přihlašovací údaje nasazení, které byly dříve nastavené.</span><span class="sxs-lookup"><span data-stu-id="96fef-136">Git will prompt for the deployment credentials that were set earlier.</span></span>  <span data-ttu-id="96fef-137">Po ověření, aplikace bude se instaluje do vzdáleného umístění, vytvořené a nasazené.</span><span class="sxs-lookup"><span data-stu-id="96fef-137">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Výstup nasazení Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="96fef-139">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="96fef-139">Test the application</span></span>

<span data-ttu-id="96fef-140">Testování aplikace procházením `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="96fef-140">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="96fef-141">Chcete-li zobrazit adresu v prostředí cloudu (nebo Azure CLI), použijte následující:</span><span class="sxs-lookup"><span data-stu-id="96fef-141">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Aplikace běžící v Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="96fef-143">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="96fef-143">Clean up</span></span>

<span data-ttu-id="96fef-144">Po dokončení testování aplikace a zkontrolujete kód a prostředky, odstraňte webovou aplikaci a plán odstraněním skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="96fef-144">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="96fef-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96fef-145">Next steps</span></span>

<span data-ttu-id="96fef-146">V tomto kurzu jste se dozvěděli, jak:</span><span class="sxs-lookup"><span data-stu-id="96fef-146">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="96fef-147">Vytvoření webu k službě Azure App Service pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="96fef-147">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="96fef-148">Nasazení aplikace ASP.NET Core Azure App Service pomocí nástroje příkazového řádku Git</span><span class="sxs-lookup"><span data-stu-id="96fef-148">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="96fef-149">Dále se naučíte, jak nasadit webovou aplikaci, která používá CosmosDB pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="96fef-149">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="96fef-150">Nasazení do Azure z příkazového řádku s .NET Core</span><span class="sxs-lookup"><span data-stu-id="96fef-150">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)