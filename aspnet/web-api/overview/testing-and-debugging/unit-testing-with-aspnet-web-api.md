---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: "Rozhraní ASP.NET Web API 2 testování částí | Microsoft Docs"
author: tfitzmac
description: "Tento pokyny a aplikace ukazují, jak vytvářet testy částí jednoduché pro vaši aplikaci webovém rozhraní API 2. Tento kurz ukazuje, jak zahrnout proj test jednotky..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 13211ee4543e17a4bfb2f83495f4041880f37df2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="30d89-104">Rozhraní ASP.NET Web API 2 testování částí</span><span class="sxs-lookup"><span data-stu-id="30d89-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="30d89-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="30d89-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="30d89-106">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="30d89-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="30d89-107">Tento pokyny a aplikace ukazují, jak vytvářet testy částí jednoduché pro vaši aplikaci webovém rozhraní API 2.</span><span class="sxs-lookup"><span data-stu-id="30d89-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="30d89-108">Tento kurz ukazuje, jak chcete zahrnout projektu testů jednotek ve vašem řešení a zapisovat zkušební metody, které zkontrolujte vrácené hodnoty z metody kontroleru.</span><span class="sxs-lookup"><span data-stu-id="30d89-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="30d89-109">Tento kurz předpokládá, že se seznámíte se základními koncepcemi rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="30d89-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="30d89-110">Úvodní kurz, najdete v části [Začínáme s ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="30d89-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="30d89-111">Testování částí v tomto tématu jsou záměrně omezeny na jednoduché datové scénáře.</span><span class="sxs-lookup"><span data-stu-id="30d89-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="30d89-112">Testování pokročilejší scénáře datových částí, najdete v části [Mocking Entity Framework při jednotky testování ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="30d89-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="30d89-113">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="30d89-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="30d89-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="30d89-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="30d89-115">Webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="30d89-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="30d89-116">V tomto tématu</span><span class="sxs-lookup"><span data-stu-id="30d89-116">In this topic</span></span>

<span data-ttu-id="30d89-117">Toto téma obsahuje následující oddíly:</span><span class="sxs-lookup"><span data-stu-id="30d89-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="30d89-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="30d89-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="30d89-119">Stáhněte si kód</span><span class="sxs-lookup"><span data-stu-id="30d89-119">Download code</span></span>](#download)
- [<span data-ttu-id="30d89-120">Vytvoření aplikace pomocí projektu testování částí</span><span class="sxs-lookup"><span data-stu-id="30d89-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="30d89-121">Přidání projektu testů jednotek při vytváření aplikace</span><span class="sxs-lookup"><span data-stu-id="30d89-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="30d89-122">Přidání projektu testů jednotek do existující aplikace</span><span class="sxs-lookup"><span data-stu-id="30d89-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="30d89-123">Nastavení aplikace webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="30d89-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="30d89-124">Instalace balíčků NuGet v testovacího projektu</span><span class="sxs-lookup"><span data-stu-id="30d89-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="30d89-125">Vytváření testů</span><span class="sxs-lookup"><span data-stu-id="30d89-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="30d89-126">Spouštění testů</span><span class="sxs-lookup"><span data-stu-id="30d89-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="30d89-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="30d89-127">Prerequisites</span></span>

<span data-ttu-id="30d89-128">Visual Studio 2017 Community, Professional nebo Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="30d89-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="30d89-129">Stáhněte si kód</span><span class="sxs-lookup"><span data-stu-id="30d89-129">Download code</span></span>

<span data-ttu-id="30d89-130">Stažení [dokončený projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="30d89-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="30d89-131">Ke stažení projekt zahrnuje jednotka testovacího kódu pro toto téma a [Mocking Entity Framework při jednotky testování rozhraní ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="30d89-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="30d89-132">Vytvoření aplikace pomocí projektu testování částí</span><span class="sxs-lookup"><span data-stu-id="30d89-132">Create application with unit test project</span></span>

<span data-ttu-id="30d89-133">Můžete buď vytvoření projektu testů jednotek při vytváření aplikace nebo přidání projektu testů jednotek do existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="30d89-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="30d89-134">Tento kurz ukazuje obě metody pro vytvoření projektu testování částí.</span><span class="sxs-lookup"><span data-stu-id="30d89-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="30d89-135">Chcete-li v tomto kurzu, můžete použít buď způsob.</span><span class="sxs-lookup"><span data-stu-id="30d89-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="30d89-136">Přidání projektu testů jednotek při vytváření aplikace</span><span class="sxs-lookup"><span data-stu-id="30d89-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="30d89-137">Vytvoření nové webové aplikace ASP.NET s názvem **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="30d89-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Vytvoření projektu](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="30d89-139">V systému windows nový projekt ASP.NET, vyberte **prázdný** šablony a přidat složky a základní odkazy pro webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="30d89-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="30d89-140">Vyberte **přidat testování částí** možnost.</span><span class="sxs-lookup"><span data-stu-id="30d89-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="30d89-141">Automaticky s názvem projektu testování částí **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="30d89-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="30d89-142">Můžete ponechat tento název.</span><span class="sxs-lookup"><span data-stu-id="30d89-142">You can keep this name.</span></span>

![Vytvoření projektu testování částí](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="30d89-144">Po vytvoření aplikace, zobrazí se, že obsahuje dva projekty.</span><span class="sxs-lookup"><span data-stu-id="30d89-144">After creating the application, you will see it contains two projects.</span></span>

![dva projekty](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="30d89-146">Přidání projektu testů jednotek do existující aplikace</span><span class="sxs-lookup"><span data-stu-id="30d89-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="30d89-147">Pokud jste nevytvořili projektu testů jednotek při vytváření aplikace, můžete přidat jeden kdykoli.</span><span class="sxs-lookup"><span data-stu-id="30d89-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="30d89-148">Předpokládejme například, že již máte aplikaci s názvem StoreApp a chcete přidat testování částí.</span><span class="sxs-lookup"><span data-stu-id="30d89-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="30d89-149">Chcete-li přidat projektu testů jednotek, klikněte pravým tlačítkem na řešení a vyberte **přidat** a **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="30d89-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Přidat nový projekt do řešení](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="30d89-151">Vyberte **Test** v levém podokně a vyberte **projektu testování částí** pro typ projektu.</span><span class="sxs-lookup"><span data-stu-id="30d89-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="30d89-152">Název projektu **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="30d89-152">Name the project **StoreApp.Tests**.</span></span>

![Přidání projektu testování částí](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="30d89-154">Zobrazí se projektu testů jednotek ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="30d89-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="30d89-155">V projektu testování částí přidáte odkaz na projekt na původní projekt.</span><span class="sxs-lookup"><span data-stu-id="30d89-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="30d89-156">Nastavení aplikace webové rozhraní API 2</span><span class="sxs-lookup"><span data-stu-id="30d89-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="30d89-157">V projektu StoreApp přidat třídu soubor, který chcete **modely** složku s názvem **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="30d89-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="30d89-158">Obsah souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="30d89-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="30d89-159">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="30d89-159">Build the solution.</span></span>

<span data-ttu-id="30d89-160">Klikněte pravým tlačítkem na složku řadiče a vyberte **přidat** a **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="30d89-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="30d89-161">Vyberte **prázdný kontroler – rozhraní Web API 2**.</span><span class="sxs-lookup"><span data-stu-id="30d89-161">Select **Web API 2 Controller - Empty**.</span></span>

![Přidat nový řadič](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="30d89-163">Nastavte název řadiče na **SimpleProductController**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="30d89-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Zadejte řadič](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="30d89-165">Existujícího kódu nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="30d89-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="30d89-166">Pro zjednodušení tento příklad, jsou data uložena v seznamu místo databáze.</span><span class="sxs-lookup"><span data-stu-id="30d89-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="30d89-167">V seznamu definované v této třídě představuje provozními daty.</span><span class="sxs-lookup"><span data-stu-id="30d89-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="30d89-168">Všimněte si, že je řadič obsahuje konstruktor, který přebírá jako parametr seznam objektů produktu.</span><span class="sxs-lookup"><span data-stu-id="30d89-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="30d89-169">Tento konstruktor umožňuje předat testovací data při testování částí.</span><span class="sxs-lookup"><span data-stu-id="30d89-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="30d89-170">Řadičem také zahrnuje dvě **asynchronní** metody pro ilustraci asynchronních metod testování částí.</span><span class="sxs-lookup"><span data-stu-id="30d89-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="30d89-171">Tyto asynchronní metody, které byly implementovány voláním **Task.FromResult** minimalizovat nadbytečné kód, ale obvykle metody by mělo zahrnovat náročná operace.</span><span class="sxs-lookup"><span data-stu-id="30d89-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="30d89-172">Metoda GetProduct vrací instanci třídy **IHttpActionResult** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="30d89-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="30d89-173">IHttpActionResult je jednou z nových funkcí ve webovém rozhraní API 2 a usnadňují vývoj testů jednotek.</span><span class="sxs-lookup"><span data-stu-id="30d89-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="30d89-174">Třídy, které implementují rozhraní IHttpActionResult se nacházejí v [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="30d89-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="30d89-175">Tyto třídy představují možné odpovědí z akcí žádosti a odpovídají stavové kódy HTTP.</span><span class="sxs-lookup"><span data-stu-id="30d89-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="30d89-176">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="30d89-176">Build the solution.</span></span>

<span data-ttu-id="30d89-177">Teď jste připravení nastavit k testovacímu projektu.</span><span class="sxs-lookup"><span data-stu-id="30d89-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="30d89-178">Instalace balíčků NuGet v testovacího projektu</span><span class="sxs-lookup"><span data-stu-id="30d89-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="30d89-179">Použijete-li vytvořit aplikaci prázdnou šablonou, projektu testování částí (StoreApp.Tests) neobsahuje žádné nainstalované balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="30d89-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="30d89-180">Další šablony, jako je například šabloně webového rozhraní API zahrnout některé balíčky NuGet do projektu testování částí.</span><span class="sxs-lookup"><span data-stu-id="30d89-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="30d89-181">V tomto kurzu musí zahrnovat balíček Microsoft ASP.NET Web API 2 jádra pro projekt test.</span><span class="sxs-lookup"><span data-stu-id="30d89-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="30d89-182">Klikněte pravým tlačítkem na projekt StoreApp.Tests a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="30d89-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="30d89-183">Je třeba vybrat StoreApp.Tests projektu přidat balíčky do daného projektu.</span><span class="sxs-lookup"><span data-stu-id="30d89-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Správa balíčků](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="30d89-185">Najít a nainstalovat balíček Microsoft ASP.NET Web API 2 jádra.</span><span class="sxs-lookup"><span data-stu-id="30d89-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![instalovat balíček základní webové rozhraní api](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="30d89-187">Zavřete okno Spravovat balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="30d89-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="30d89-188">Vytváření testů</span><span class="sxs-lookup"><span data-stu-id="30d89-188">Create tests</span></span>

<span data-ttu-id="30d89-189">Ve výchozím nastavení zahrnuje soubor prázdný test s názvem UnitTest1.cs testovacího projektu.</span><span class="sxs-lookup"><span data-stu-id="30d89-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="30d89-190">Tento soubor obsahuje atributy že použijete k vytvoření testovací metody.</span><span class="sxs-lookup"><span data-stu-id="30d89-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="30d89-191">Testy jednotek můžete použít tento soubor nebo je možné vytvořit svůj vlastní soubor.</span><span class="sxs-lookup"><span data-stu-id="30d89-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="30d89-193">V tomto kurzu vytvoříte vlastní třídu testu.</span><span class="sxs-lookup"><span data-stu-id="30d89-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="30d89-194">Soubor UnitTest1.cs můžete odstranit.</span><span class="sxs-lookup"><span data-stu-id="30d89-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="30d89-195">Přidejte třídu s názvem **TestSimpleProductController.cs**a kódu nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="30d89-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="30d89-196">Spouštění testů</span><span class="sxs-lookup"><span data-stu-id="30d89-196">Run tests</span></span>

<span data-ttu-id="30d89-197">Nyní jste připraveni ke spuštění testů.</span><span class="sxs-lookup"><span data-stu-id="30d89-197">You are now ready to run the tests.</span></span> <span data-ttu-id="30d89-198">Všechny metody, které jsou označené **TestMethod** atribut bude být testována.</span><span class="sxs-lookup"><span data-stu-id="30d89-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="30d89-199">Z **Test** položky nabídky, spusťte testy.</span><span class="sxs-lookup"><span data-stu-id="30d89-199">From the **Test** menu item, run the tests.</span></span>

![Provádění testů](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="30d89-201">Otevřete **Průzkumníka testů** okně a Všimněte si výsledky testů.</span><span class="sxs-lookup"><span data-stu-id="30d89-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![výsledky testu](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="30d89-203">Souhrn</span><span class="sxs-lookup"><span data-stu-id="30d89-203">Summary</span></span>

<span data-ttu-id="30d89-204">Dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="30d89-204">You have completed this tutorial.</span></span> <span data-ttu-id="30d89-205">Data v tomto kurzu se záměrně zjednodušené zaměřit se na jednotce testování podmínky.</span><span class="sxs-lookup"><span data-stu-id="30d89-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="30d89-206">Testování pokročilejší scénáře datových částí, najdete v části [Mocking Entity Framework při jednotky testování ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="30d89-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>