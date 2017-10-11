---
title: Mobile Services toevoegen met behulp van verbonden Services in Visual Studio | Microsoft Docs
description: Mobile Services toevoegen met behulp van de Visual Studio verbonden dialoogvenster Services toevoegen
services: visual-studio-online
documentationcenter: na
author: mlhoop
manager: douge
editor: 
ms.assetid: 75c3cb93-88e1-476d-a416-f34caa3608e3
ms.service: visual-studio-online
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: mobile
ms.date: 12/16/2015
ms.author: mlearned
ms.openlocfilehash: d185fdafebad56f8970e390b2a0672c3fb84df8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Mobile Services toevoegen met behulp van Visual Studio verbonden Services
Met Visual Studio 2015, kunt u verbinden met Azure Mobile Services via de **verbonden Service toevoegen** dialoogvenster. U kunt verbinding maken vanaf elke clienttoepassing C#, een JavaScript-app of platformoverschrijdende Cordova-app. Als u verbinding hebt gemaakt, kunt u maken en toegang tot gegevens, aangepaste API's en geplande taken maken of toevoegen van ondersteuning voor pushmeldingen.  De bewerking verbonden services voegt alle juiste verwijzingen en code van de verbinding. U kunt ook te profiteren van ingebouwde ondersteuning voor verificatie met tal van populaire identity-schema's, zoals Azure AD, Facebook, Twitter en Microsoft-Accounts.

## <a name="supported-project-types"></a>Ondersteunde projecttypen
> [!NOTE]
> In Visual Studio 2015 wordt Azure Mobile Services toevoegen aan een universele Windows-(Windows 10) projecten via het dialoogvenster verbonden Services toevoegen niet ondersteund. U kunt Azure Mobile Services toevoegen door het installeren van de juiste pakketten met NuGet Package Manager voor uw project.
> 
> 

Verbinding maken met Azure Mobile Services in de volgende projecttypen kunt u het dialoogvenster Services verbonden.

* .NET Windows 8.1 Store en Phone universele App-projecten
* JavaScript Windows 8.1 Store en Phone universele App-projecten
* Projecten gemaakt met behulp van Visual Studio Tools voor Apache Cordova

## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Verbinding maken met Azure Mobile Services met het dialoogvenster verbonden Services toevoegen
1. Zorg ervoor dat u hebt een Azure-account. Als u geen Azure-account hebt, kunt u zich aanmelden voor een [gratis proefversie](http://go.microsoft.com/fwlink/?LinkId=518146).
2. Open de **verbonden Services toevoegen** in het dialoogvenster.
   
   * Open het project in Visual Studio voor .NET-toepassingen, opent u het snelmenu voor het **verwijzingen** knooppunt in Solution Explorer en kies vervolgens **verbonden Service toevoegen**
     
        ![Verbinding maken met Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)
   * Apache Cordova-app-projecten, opent u het project in Visual Studio, opent u het snelmenu voor het projectknooppunt in Solution Explorer en kies vervolgens **verbonden Service toevoegen**.
3. In de **verbonden Service toevoegen** dialoogvenster Kies **Azure Mobile Services**, en kies vervolgens de **configureren** knop. U wordt mogelijk gevraagd aan te melden bij Azure als u dat nog niet hebt gedaan.
   
    ![Toevoegen van een mobiele Service van Azure](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)
4. In de **Azure Mobile Services** dialoogvenster Kies een bestaande mobiele service als er een. Als u maken van een nieuwe mobiele service van Azure wilt, volgt u de onderstaande procedure om te doen. Ga anders naar de volgende stap.
   
    Een nieuwe mobiele service-account maken:
   
   1. Kies de ** maken van een Service ** koppelen aan de onderkant van het dialoogvenster.
       ![Nieuwe mobiele service die verbonden toevoegen](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)
   2. Op de **mobiele Service maken** in het dialoogvenster kunt u een JavaScript back-end voor mobiele service, of een mobiele service van .NET-back-end van de **Runtime** vervolgkeuzelijst. 
      
       ![Een mobiele service maken](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)
      
       Een back-endservice JavaScript is een eenvoudige en krachtige. Als u een JavaScript back-end voor mobiele service maakt, wordt de serverzijde JavaScript-code is opgeslagen in de cloud, maar u kunt de server-scripts bewerken met behulp van Server Explorer of de Azure-beheerportal. 
      
       Een .NET-back-end voor mobiele service biedt u de volledige kracht en flexibiliteit van Web-API en Entity Framework. Als u een .NET-back-end voor mobiele service maakt, wordt een project voor u gemaakt en toegevoegd aan uw oplossing. 
   3. Kies de **regio** waar de mobiele service en voer vervolgens een gebruikersnaam en wachtwoord in voor de server.
   4. Nadat u de vereiste gegevens hebt ingevoerd, kiest u de **maken** om te maken van de mobiele service.
   5. De nieuwe mobiele service moet worden weergegeven in de lijst met Services op de **Azure Mobile Services** in het dialoogvenster. Kies de nieuwe mobiele service in de lijst en kies vervolgens de **toevoegen** om toe te voegen van de service aan uw project.
5. Bekijk de ophalen gestart pagina die verschijnt en weten hoe uw project is gewijzigd. Wanneer u een gekoppelde service toevoegt, verschijnt er een pagina aan de slag in uw browser. U kunt de voorgestelde Vervolgstappen en codevoorbeelden bekijken of overschakelen naar de pagina wat er gebeurd is om te zien welke referenties zijn toegevoegd aan uw project en hoe uw code en configuratie-bestanden die zijn aangepast.
6. Start met behulp van de codevoorbeelden als richtlijn schrijven van code voor toegang tot uw mobiele service.

## <a name="how-your-project-is-modified"></a>Hoe uw project is gewijzigd
Hoe uw project in Visual Studio wordt gewijzigd, is afhankelijk van het projecttype. Zie voor C# client-apps, [wat is er gebeurd – C#-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513119). Zie voor JavaScript-client-apps [wat is er gebeurd – JavaScript-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513120). Zie voor Cordova-apps [wat is er gebeurd – Cordova-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513116).

## <a name="next-steps"></a>Volgende stappen
Vragen stellen en hulp te krijgen: 

* [MSDN-Forum: Azure Mobile Services](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)
* [Azure Mobile Services op de Microsoft Azure-teamblog](https://azure.microsoft.com/blog/topics/mobile/)
* [Azure Mobile Services op azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)
* [Azure Mobile Services-documentatie op azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)

