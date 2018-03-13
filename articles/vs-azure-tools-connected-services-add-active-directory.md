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
ms.date: 03/01/2017
ms.author: kraigb
ms.openlocfilehash: a767c93fb271f3aa33d9556c69c511bcac7cb0d5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Een Azure Active Directory toe te voegen met behulp van verbonden Services in Visual Studio
Met behulp van Azure Active Directory (Azure AD), kunt u eenmalige aanmelding (SSO) voor ASP.NET MVC-webtoepassingen of Active Directory-verificatie te ondersteunen in een Web API-services. Uw gebruikers kunnen hun accounts van Azure Active Directory met Azure Active Directory-verificatie, verbinding maken met uw webtoepassingen gebruiken. De voordelen van Azure Active Directory-verificatie met Web-API bevatten verbeterde gegevensbeveiliging wanneer het blootstellen van een API vanuit een webtoepassing. U hebt met Azure AD niet om een verificatiesysteem afzonderlijke met een eigen account en gebruiker management te beheren.

## <a name="prerequisites"></a>Vereisten
- Azure-account: als u geen Azure-account, kunt u [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Verbinding maken met Azure Active Directory met het dialoogvenster Services verbonden
1. Maken of openen van een ASP.NET MVC-project of een ASP.NET Web API-project in Visual Studio.

1. In de Solution Explorer met de rechtermuisknop op de **verbonden Services** knooppunt, en selecteer in het contextmenu **verbonden Services toevoegen**.

1. Op de **verbonden Services** pagina **verificatie met Azure Active Directory**.
   
    ![Pagina verbonden Services](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Op de **inleiding** pagina van de **configureren Azure AD Authentication** wizard selecteert u **volgende**.
   
    ![Introductiepagina](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. Op de **eenmalige aanmelding op** pagina van de **configureren Azure AD Authentication** wizard, selecteert u een domein van de **domein** vervolgkeuzelijst. De lijst met domeinen bevat alle domeinen die toegankelijk is door de accounts die worden vermeld in het dialoogvenster Accountinstellingen. Als alternatief kunt u een domeinnaam kunt invoeren als u niet kunt vinden die u, zoals zoekt `mydomain.onmicrosoft.com`. U kunt de optie voor het maken van een Azure Active Directory-app of de instellingen van een bestaande Azure Active Directory-app gebruiken. Selecteer **volgende** wanneer u klaar bent.
   
    ![Eenmalige aanmelding op de pagina](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. Op de **maptoegang** pagina van de **configureren Azure AD Authentication** wizard ervoor te zorgen dat de **mapgegevens lezen** optie is ingeschakeld. 
   
    ![Directorypagina-toegang](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Selecteer **voltooien** om toe te voegen van de vereiste configuratiecode en verwijzingen naar uw project voor Azure AD-verificatie inschakelen. U kunt het Active Directory-domein zien op de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Visual Studio, ziet u een [wat is er gebeurd](#how-your-project-is-modified) artikel om weer te geven hoe uw project is gewijzigd. Als u wilt controleren of alles heeft gewerkt, opent u een van de gewijzigde configuratiebestanden en controleer zijn de instellingen vermeld in het artikel er. 

## <a name="how-your-project-is-modified"></a>Hoe uw project is gewijzigd
Wanneer u de wizard uitvoert, wordt in Visual Studio Azure Active Directory en de bijbehorende verwijzingen naar uw project toegevoegd. Configuratiebestanden en codebestanden in uw project ook worden gewijzigd om ondersteuning voor Azure AD. De specifieke wijzigingen die Visual Studio maakt, is afhankelijk van het projecttype. Zie voor gedetailleerde informatie over hoe ASP.NET MVC-projecten zijn gewijzigd, [welke happened – MVC projecten](http://go.microsoft.com/fwlink/p/?LinkID=513809). Zie voor een Web API-projecten [wat is er gebeurd – Web API-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513810).

## <a name="next-steps"></a>Volgende stappen
* [MSDN-Forum voor Azure Active Directory](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [Azure Active Directory-documentatie](https://azure.microsoft.com/documentation/services/active-directory/)
* [Blogbericht: Inleiding tot Azure Active Directory](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

