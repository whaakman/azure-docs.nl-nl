---
title: Toevoegen van een Azure Active Directory met behulp van Connected Services in Visual Studio
description: Een Azure Active Directory toevoegen via het dialoogvenster Visual Studio verbonden Services toevoegen
services: active-directory
author: ghogen
manager: douge
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.openlocfilehash: 2e446bad042965e8466e74b9b1abd8661ea88f8b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057263"
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Toevoegen van een Azure Active Directory met behulp van Connected Services in Visual Studio

Met behulp van Azure Active Directory (Azure AD), kunt u eenmalige aanmelding (SSO) voor ASP.NET MVC-web-apps of Active Directory-verificatie te ondersteunen in Web-API-services. Uw gebruikers kunnen hun accounts uit Azure Active Directory gebruiken om verbinding maken met uw webtoepassingen met Azure AD-verificatie. De voordelen van Azure AD-verificatie met Web-API opnemen verbeterde gegevensbeveiliging wanneer blootstellen van een API vanuit een webtoepassing. Met Azure AD hoeft u niet voor het beheren van een afzonderlijke verificatiesysteem met een eigen account en de gebruiker.

In dit artikel en de bijbehorende artikelen vindt u informatie van het gebruik van de Visual Studio Connected Service-functie voor Active Directory. De mogelijkheid is beschikbaar in Visual Studio 2017 en Visual Studio 2015.

De verbonden Active Directory-service wordt op dit moment geen ondersteuning voor ASP.NET Core-toepassingen.

## <a name="prerequisites"></a>Vereisten

- Azure-account: als u een Azure-account niet hebt, kunt u [zich registreren voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
- **Visual Studio 2015** of hoger. [Download Visual Studio 2017 nu](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Verbinding maken met Azure Active Directory met het dialoogvenster Connected Services

1. Maak of open een ASP.NET MVC-project, of een ASP.NET Web API-project in Visual Studio. U kunt de MVC, Web-API toepassing met één pagina, Azure API App, mobiele Apps van Azure en Azure Mobile Service-sjablonen gebruiken.

1. Selecteer de **Project > Connected Service toevoegen...**  opdracht of dubbelklik op de **Connected Services** knooppunt gevonden onder het project in Solution Explorer.

1. Op de **Connected Services** weergeeft, schakelt **verificatie met Azure Active Directory**.

    ![Pagina met verbonden Services](./media/vs-azure-active-directory/connected-services-add-active-directory.png)

1. Op de **inleiding** weergeeft, schakelt **volgende**. Als u fouten op deze pagina ziet, raadpleeg dan [diagnose van fouten met de Azure Active Directory Connected Service](vs-active-directory-error.md).

    ![Introductiepagina](./media/vs-azure-active-directory/configure-azure-ad-wizard-1.png)

1. Op de **Single Sign On** pagina, selecteert u een domein van de **domein** vervolgkeuzelijst. De lijst bevat alle domeinen die toegankelijk is door de accounts die worden vermeld in het dialoogvenster Instellingen voor rekening van Visual Studio (**bestand > Accountinstellingen...** ). Als alternatief kunt u een domeinnaam kunt invoeren als u niet kunt vinden die u, zoals zoekt `mydomain.onmicrosoft.com`. U kunt de optie voor het maken van een Azure Active Directory-app of de instellingen van een bestaande Azure Active Directory-app gebruiken. Selecteer **volgende** wanneer u klaar bent.

    ![Eenmalige aanmelding op pagina](./media/vs-azure-active-directory/configure-azure-ad-wizard-2.png)

1. Op de **maptoegang** weergeeft, schakelt de **mapgegevens lezen** optie naar wens. Ontwikkelaars bevatten doorgaans deze optie.

    ![Pagina voor Directory-toegang](./media/vs-azure-active-directory/configure-azure-ad-wizard-3.png)

1. Selecteer **voltooien** wijzigingen in uw project, zodat Azure AD-verificatie te starten. Visual Studio ziet u uitgevoerd gedurende deze tijd:

    ![Active Directory verbonden service wordt uitgevoerd](./media/vs-azure-active-directory/active-directory-connected-service-output.png)

1. Wanneer het proces voltooid is, wordt uw browser op een van de volgende artikelen, afhankelijk van het projecttype geopend in Visual Studio:

    - [Aan de slag met .NET MVC-projecten](vs-active-directory-dotnet-getting-started.md)
    - [Aan de slag met WebAPI-projecten](vs-active-directory-webapi-getting-started.md)

1. U ziet ook de Active Directory-domein op het [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

## <a name="how-your-project-is-modified"></a>Hoe uw project is gewijzigd

Wanneer u de gekoppelde service de wizard toevoegt, wordt in Visual Studio Azure Active Directory en gekoppelde verwijzingen naar uw project toegevoegd. Configuratiebestanden en codebestanden in uw project zijn ook gewijzigd als u wilt toevoegen van ondersteuning voor Azure AD. De specifieke wijzigingen die Visual Studio maakt, is afhankelijk van het projecttype. Zie de volgende artikelen voor meer informatie:

- [Wat is er gebeurd met mijn .NET MVC-project?](vs-active-directory-dotnet-what-happened.md)
- [Wat is er gebeurd met mijn Web-API-project?](vs-active-directory-webapi-what-happened.md)

## <a name="next-steps"></a>Volgende stappen

- [Verificatiescenario's voor Azure Active Directory](authentication-scenarios.md)
- [Aanmelden bij Microsoft toevoegen aan een ASP.NET-web-app](quickstart-v1-aspnet-webapp.md)
