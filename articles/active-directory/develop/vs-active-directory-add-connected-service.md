---
title: Een Azure Active Directory toe te voegen met behulp van verbonden Services in Visual Studio | Microsoft Docs
description: Een Azure Active Directory toevoegen met behulp van de Visual Studio verbonden dialoogvenster Services toevoegen
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/12/2018
ms.author: kraigb
ms.openlocfilehash: b21761b6fc166ecbb2fec9c13e5e207481fa9a39
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Een Azure Active Directory toe te voegen met behulp van verbonden Services in Visual Studio

Met behulp van Azure Active Directory (Azure AD), kunt u eenmalige aanmelding (SSO) voor ASP.NET MVC-webtoepassingen of Active Directory-verificatie te ondersteunen in een Web API-services. Uw gebruikers kunnen hun accounts van Azure Active Directory gebruiken om verbinding maken met uw webtoepassingen met Azure AD-verificatie. De voordelen van Azure AD-verificatie met Web-API bevatten verbeterde gegevensbeveiliging wanneer het blootstellen van een API vanuit een webtoepassing. U hebt met Azure AD niet om een verificatiesysteem afzonderlijke met een eigen account en gebruiker management te beheren.

In dit artikel en de bijbehorende artikelen vindt informatie van het gebruik van de functie voor Visual Studio Connected-Service voor Active Directory. De mogelijkheid is beschikbaar in Visual Studio 2017 en Visual Studio 2015.

De service Active Directory verbonden ondersteunt momenteel geen ASP.NET Core toepassingen.

## <a name="prerequisites"></a>Vereisten

- Azure-account: als u geen Azure-account hebt, kunt u [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Verbinding maken met Azure Active Directory met het dialoogvenster Services verbonden

1. Maken of openen van een ASP.NET MVC-project of een ASP.NET Web API-project in Visual Studio. U kunt de MVC, Web-API, één pagina toepassing, Azure API-App, mobiele Apps van Azure en Azure Mobile Service-sjablonen.

1. Selecteer de **Project > verbonden Service toevoegen...**  menuopdracht of dubbelklik op de **verbonden Services** knooppunt gevonden onder het project in Solution Explorer.

1. Op de **verbonden Services** pagina **verificatie met Azure Active Directory**.

    ![Pagina verbonden Services](./media/vs-azure-active-directory/connected-services-add-active-directory.png)

1. Op de **inleiding** pagina **volgende**. Als u fouten op deze pagina ziet, raadpleeg dan [oplossen van problemen met de Azure Active Directory verbonden Service](vs-active-directory-error.md).

    ![Introductiepagina](./media/vs-azure-active-directory/configure-azure-ad-wizard-1.png)

1. Op de **eenmalige aanmelding op** pagina, selecteert u een domein via de **domein** vervolgkeuzelijst. De lijst bevat alle domeinen die toegankelijk is door de accounts die worden vermeld in het dialoogvenster Accountinstellingen van Visual Studio (**bestand > Accountinstellingen...** ). Als alternatief kunt u een domeinnaam kunt invoeren als u niet kunt vinden die u, zoals zoekt `mydomain.onmicrosoft.com`. U kunt de optie voor het maken van een Azure Active Directory-app of de instellingen van een bestaande Azure Active Directory-app gebruiken. Selecteer **volgende** wanneer u klaar bent.

    ![Eenmalige aanmelding op de pagina](./media/vs-azure-active-directory/configure-azure-ad-wizard-2.png)

1. Op de **maptoegang** pagina de **mapgegevens lezen** als de gewenste optie. Ontwikkelaars omvatten deze optie.

    ![Directorypagina-toegang](./media/vs-azure-active-directory/configure-azure-ad-wizard-3.png)

1. Selecteer **voltooien** starten wijzigingen aan uw project om Azure AD-verificatie inschakelen. Visual Studio geeft de voortgang tijdens deze periode:

    ![Voortgang van de service Active Directory verbonden](./media/vs-azure-active-directory/active-directory-connected-service-output.png)

1. Wanneer het proces voltooid is, wordt uw browser op een van de volgende artikelen, afhankelijk van het projecttype van uw in Visual Studio geopend:

    - [Aan de slag met .NET MVC-projecten](vs-active-directory-dotnet-getting-started.md)
    - [Aan de slag met WebAPI-projecten](vs-active-directory-webapi-getting-started.md)

1. U kunt het Active Directory-domein ook bekijken op de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

## <a name="how-your-project-is-modified"></a>Hoe uw project is gewijzigd

Wanneer u de gekoppelde service van de wizard toevoegt, wordt in Visual Studio Azure Active Directory en bijbehorende verwijzingen naar uw project toegevoegd. Configuratiebestanden en codebestanden in uw project ook worden gewijzigd om ondersteuning voor Azure AD. De specifieke wijzigingen die Visual Studio maakt, is afhankelijk van het projecttype. Zie de volgende artikelen voor meer informatie:

- [Wat is er gebeurd met mijn .NET MVC-project?](vs-active-directory-dotnet-what-happened.md)
- [Wat is er gebeurd met mijn Web API-project?](vs-active-directory-webapi-what-happened.md)

## <a name="next-steps"></a>Volgende stappen

- [Verificatie-scenario's voor Azure Active Directory](active-directory-authentication-scenarios.md)
- [Aanmelden met Microsoft toevoegen aan een ASP.NET-web-app](guidedsetups/active-directory-aspnetwebapp-v1.md)
