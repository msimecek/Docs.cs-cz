---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: "Databázi EF nejprve s architekturou ASP.NET MVC: přizpůsobení zobrazení | Microsoft Docs"
author: tfitzmac
description: "Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: af9609396cff18b08824732731ddb9c5cca578fa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="e4ca6-104">Databázi EF nejprve s architekturou ASP.NET MVC: přizpůsobení zobrazení</span><span class="sxs-lookup"><span data-stu-id="e4ca6-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="e4ca6-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e4ca6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e4ca6-106">Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="e4ca6-107">Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="e4ca6-108">Generovaný kód odpovídá sloupců v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="e4ca6-109">Tato část řady se zaměřuje na změnu zobrazení automaticky generované pro zlepšení prezentaci.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="e4ca6-110">Přidat zaregistrovaná kurzy student podrobnosti o</span><span class="sxs-lookup"><span data-stu-id="e4ca6-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="e4ca6-111">Generovaný kód poskytuje dobrý výchozí bod pro vaši aplikaci, ale neposkytuje nutně všechny funkce, které potřebujete ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="e4ca6-112">Můžete upravit kód, který splňovat požadavky na konkrétní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="e4ca6-113">V současné době aplikace zaregistrované kurzy pro vybrané student nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="e4ca6-114">V této části přidáte zaregistrovaná kurzy pro každý studenty do **podrobnosti** zobrazení pro student.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="e4ca6-115">Otevřete **Students/Details.cshtml**a pod poslední &lt;/dl&gt; kartě, ale před uzavírací &lt;/div&gt; značky, přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="e4ca6-116">Tento kód vytvoří tabulku, která zobrazí řádek pro každý záznam v tabulce registrace pro vybrané student.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="e4ca6-117">**Zobrazení** metoda vykreslí HTML pro objekt (typem modelItem), který reprezentuje výraz.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="e4ca6-118">Použít metodu zobrazení (nikoli jednoduše vložení hodnoty vlastnosti v kódu) a ujistěte se, hodnota formátována správně na základě jeho typu a šablonu pro daný typ.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="e4ca6-119">V tomto příkladu každý výraz vrací jedinou vlastnost z aktuální záznam v smyčky a hodnoty jsou primitivní typy, které se vykresluje jako text.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="e4ca6-120">Přejděte do zobrazení studenty nebo Index znovu a vyberte **podrobnosti** pro jednu na studentů.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="e4ca6-121">Uvidíte, že neobsahuje zaregistrovaná kurzy v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e4ca6-121">You will see the enrolled courses have been included in the view.</span></span>

![Student s registrací.](customizing-a-view/_static/image1.png)

>[!div class="step-by-step"]
<span data-ttu-id="e4ca6-123">[Předchozí](changing-the-database.md)
[další](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="e4ca6-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>