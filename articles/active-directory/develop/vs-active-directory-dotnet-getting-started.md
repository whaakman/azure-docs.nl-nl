---
title: Aan de slag met Azure AD in Visual Studio .NET MVC-projecten
description: Hoe u aan de slag met Azure Active Directory in .NET MVC-projecten nadat verbinding maken met of maken van een Azure AD met Visual Studio services verbonden
services: active-directory
author: ghogen
manager: douge
ms.assetid: 1c8b6a58-5144-4965-a905-625b9ee7b22b
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: d521b8c85fb66b6c50d1b9f07e5f4d653e9e57b9
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31784068"
---
# <a name="getting-started-with-azure-active-directory-aspnet-mvc-projects"></a>Aan de slag met Azure Active Directory (ASP.NET MVC-projecten)

> [!div class="op_single_selector"]
> - [Aan de slag](vs-active-directory-dotnet-getting-started.md)
> - [Wat is er gebeurd](vs-active-directory-dotnet-what-happened.md)

Dit artikel vindt u aanvullende richtlijnen wanneer u Active Directory hebt toegevoegd aan een ASP.NET MVC-project via de **Project > verbonden Services** opdracht van Visual Studio. Als u de service nog niet aan uw project hebt toegevoegd, kunt u dat doen op elk gewenst moment.

Zie [wat is er gebeurd met mijn MVC-project?](vs-active-directory-dotnet-what-happened.md) de wijzigingen aan uw project in bij het toevoegen van de gekoppelde service.

## <a name="requiring-authentication-to-access-controllers"></a>Verificatie te vereisen voor toegang tot domeincontrollers

Alle domeincontrollers in uw project zijn adorned met de `[Authorize]` kenmerk. Dit kenmerk moet de gebruiker moet worden geverifieerd voordat toegang tot deze controllers. Verwijder dit kenmerk van de controller zodat de controller voor anonieme toegang. Als u de machtigingen instelt op een meer gedetailleerd niveau wilt, toepassen van het kenmerk voor elke methode waarvoor de autorisatie in plaats van aan de controllerklasse toepast.

## <a name="adding-signin--signout-controls"></a>Toevoegen van aanmelding / SignOut besturingselementen

Als u wilt de aanmelding/SignOut-besturingselementen toevoegen aan uw weergave, kunt u de `_LoginPartial.cshtml` gedeeltelijke weergave de functionaliteit toevoegen aan een van uw weergaven. Hier volgt een voorbeeld van de functionaliteit toegevoegd aan de standaard `_Layout.cshtml` weergeven. (Houd rekening met het laatste element in de div met de klasse navigatiebalk samenvouwen):

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

- [Verificatie-scenario's voor Azure Active Directory](active-directory-authentication-scenarios.md)
- [Aanmelden met Microsoft toevoegen aan een ASP.NET-web-app](guidedsetups/active-directory-aspnetwebapp-v1.md)
