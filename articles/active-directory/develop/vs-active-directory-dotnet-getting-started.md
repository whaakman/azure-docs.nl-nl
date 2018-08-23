---
title: Aan de slag met Azure AD in Visual Studio .NET MVC-projecten
description: Aan de slag met Azure Active Directory in .NET MVC-projecten nadat u verbinding maken met of het maken van een Azure AD met behulp van Visual Studio services verbonden
services: active-directory
author: ghogen
manager: douge
ms.assetid: 1c8b6a58-5144-4965-a905-625b9ee7b22b
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: 437cb8b26cb33dfbb9c259e2842a59a765b78903
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42058422"
---
# <a name="getting-started-with-azure-active-directory-aspnet-mvc-projects"></a>Aan de slag met Azure Active Directory (ASP.NET MVC-projecten)

> [!div class="op_single_selector"]
> - [Aan de slag](vs-active-directory-dotnet-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-dotnet-what-happened.md)

In dit artikel vindt u aanvullende richtlijnen nadat u Active Directory hebt toegevoegd aan een ASP.NET MVC-project via de **Project > Connected Services** opdracht van Visual Studio. Als u hebt de service nog niet is toegevoegd aan uw project, kunt u dit doen op elk gewenst moment.

Zie [wat is er gebeurd met mijn MVC-project?](vs-active-directory-dotnet-what-happened.md) voor de wijzigingen aan uw project bij het toevoegen van de gekoppelde service.

## <a name="requiring-authentication-to-access-controllers"></a>Verificatie te vereisen voor toegang tot domeincontrollers

Alle domeincontrollers in uw project zijn adorned met de `[Authorize]` kenmerk. Dit kenmerk moet de gebruiker moet worden geverifieerd voordat u toegang tot deze domeincontrollers. Als u wilt toestaan dat de controller voor anonieme toegang, dit kenmerk van de netwerkcontroller te verwijderen. Als u de machtigingen op een meer gedetailleerd niveau instellen wilt, moet u het kenmerk toepassen op elke methode waarvoor autorisatie in plaats van toe te passen op de controllerklasse.

## <a name="adding-signin--signout-controls"></a>Aanmelding toevoegen / besturingselementen die afmelden wordt weergegeven

Als u wilt toevoegen de besturingselementen voor aanmelding/die afmelden wordt weergegeven in de weergave, kunt u de `_LoginPartial.cshtml` gedeeltelijke weergave zodat u de functionaliteit toevoegen aan een van uw weergaven. Hier volgt een voorbeeld van de functionaliteit die wordt toegevoegd aan de standaard `_Layout.cshtml` weergeven. (Houd rekening met het laatste element in het div-element met de klasse navigatiebalk samenvouwen):

```html
<!DOCTYPE html>
 <html>
 <head>
     <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@ViewBag.Title - My ASP.NET Application</title>
    @Styles.Render("~/Content/css")
    @Scripts.Render("~/bundles/modernizr")
</head>
<body>
    <div class="navbar navbar-inverse navbar-fixed-top">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
            </div>
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                </ul>
                @Html.Partial("_LoginPartial")
            </div>
        </div>
    </div>
    <div class="container body-content">
        @RenderBody() 
        <hr />
        <footer>
            <p>&copy; @DateTime.Now.Year - My ASP.NET Application</p>
        </footer>
    </div>
    @Scripts.Render("~/bundles/jquery")
    @Scripts.Render("~/bundles/bootstrap")
    @RenderSection("scripts", required: false)
</body>
</html>
```

## <a name="next-steps"></a>Volgende stappen

- [Verificatiescenario's voor Azure Active Directory](authentication-scenarios.md)
- [Aanmelden bij Microsoft toevoegen aan een ASP.NET-web-app](quickstart-v1-aspnet-webapp.md)
