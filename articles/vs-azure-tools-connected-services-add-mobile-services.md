---
title: Mobile Services toe te voegen met behulp van Connected Services in Visual Studio | Microsoft Docs
description: Mobile Services toevoegen met behulp van het dialoogvenster Visual Studio verbonden Services toevoegen
services: visual-studio-online
documentationcenter: na
author: mlhoop
manager: douge
editor: ''
ms.assetid: 75c3cb93-88e1-476d-a416-f34caa3608e3
ms.service: visual-studio-online
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: mobile
ms.custom: vs-azure
ms.date: 12/16/2015
ms.author: mlearned
ms.openlocfilehash: a1b2524b059601e1f6f999af7a9d9e063f7c0420
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233443"
---
# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Mobile Services toe te voegen met behulp van Visual Studio verbonden Services
Visual Studio 2015, u kunt verbinding maken met Azure Mobile Services met behulp van de **Add Connected Service** dialoogvenster. U kunt verbinding maken van een C#-client-app, een JavaScript-app of Cordova-app voor meerdere platformen. Wanneer u verbinding hebt gemaakt, kunt u maken en toegang tot gegevens, aangepaste API's en geplande taken maken of toevoegen van ondersteuning voor pushmeldingen.  De bewerking verbonden services voegt alle juiste verwijzingen en code van de verbinding. U kunt ook profiteren van de ingebouwde ondersteuning voor verificatie met een scala aan populaire id-schema's, zoals Azure AD, Facebook, Twitter en Microsoft-Accounts.

## <a name="supported-project-types"></a>Ondersteunde projecttypen
> [!NOTE]
> In Visual Studio 2015, wordt Azure Mobile Services toe te voegen aan een Windows Universal (Windows 10) projecten met behulp van het dialoogvenster Connected Services toevoegen niet ondersteund. U kunt Azure Mobile Services toevoegen door het installeren van de juiste pakketten met behulp van de NuGet Package Manager voor uw project.
> 
> 

Het dialoogvenster Connected Services kunt u verbinding maken met Azure Mobile Services in de volgende projecttypen.

* .NET Windows 8.1-Store, Phone en universele App-projecten
* JavaScript Windows 8.1-Store, Phone en universele App-projecten
* Projecten die zijn gemaakt met behulp van Visual Studio Tools voor Apache Cordova

## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Verbinding maken met Azure Mobile Services met behulp van het dialoogvenster Connected Services toevoegen
1. Zorg ervoor dat u hebt een Azure-account. Als u nog geen Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](http://go.microsoft.com/fwlink/?LinkId=518146).
2. Open de **Connected Services toevoegen** in het dialoogvenster.
   
   * Voor .NET-apps, opent u uw project in Visual Studio, open het contextmenu voor de **verwijzingen** knooppunt in Solution Explorer en kies vervolgens **Add Connected Service**
     
        ![Verbinding maken met de mobiele Service van Azure](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)
   * Projecten met Apache Cordova-app, opent u het project in Visual Studio, open het contextmenu voor het projectknooppunt in Solution Explorer en kies vervolgens **Add Connected Service**.
3. In de **Add Connected Service** dialoogvenster vak, kiest u **Azure Mobile Services**, en kies vervolgens de **configureren** knop. U mogelijk gevraagd om aan te melden bij Azure als u dat nog niet hebt gedaan.
   
    ![Toevoegen van een mobiele Service van Azure](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)
4. In de **Azure Mobile Services** dialoogvenster vak, kiest u een bestaande mobiele service als u die hebt. Als u nodig hebt voor het maken van een nieuwe mobiele service van Azure, volgt u de procedure hieronder om dit te doen. Ga anders naar de volgende stap.
   
    Een nieuwe mobiele service-account maken:
   
   1. Kies de ** maken van een Service ** koppeling aan de onderkant van het dialoogvenster.
       ![Nieuwe mobiele verbonden service toevoegen](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)
   2. Op de **mobiele Service maken** in het dialoogvenster kunt u een JavaScript-back-end mobiele service, of een mobiele service van .NET-back-end van de **Runtime** vervolgkeuzelijst. 
      
       ![Het maken van een mobiele service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)
      
       Een JavaScript-back-end-service is eenvoudig en krachtige. Als u een mobiele JavaScript back-end-service maakt, wordt de serverzijde JavaScript-code is opgeslagen in de cloud, maar u kunt serverscripts bewerken met behulp van Server Explorer of de Azure management portal. 
      
       Een mobiele .NET-back-end-service biedt u de volledige kracht en flexibiliteit van de Web-API en Entity Framework. Als u een .NET-back-end mobiele service maakt, wordt er een project voor u gemaakt en wordt deze toegevoegd aan uw oplossing. 
   3. Kies de **regio** waar u wilt de mobiele service, en voer vervolgens een gebruikersnaam en wachtwoord in voor de server.
   4. Nadat u de vereiste gegevens hebt ingevoerd, kiest u de **maken** om te maken van de mobiele service.
   5. De nieuwe mobiele service moet worden weergegeven in de lijst met Services op de **Azure Mobile Services** in het dialoogvenster. Kies de nieuwe mobiele service in de lijst en kies vervolgens de **toevoegen** om toe te voegen van de service aan uw project.
5. Controleer de aan de slag te gaan pagina die wordt weergegeven en ontdek hoe uw project is gewijzigd. Wanneer u een verbonden service toevoegt, wordt er in uw browser een aan de slag-pagina weergegeven. U kunt de voorgestelde volgende stappen en voorbeelden van code bekijken of schakel over naar de pagina Wat is er gebeurd om te zien welke referenties zijn toegevoegd aan uw project, en hoe uw code en configuratie van bestanden die zijn aangepast.
6. Start met behulp van de codevoorbeelden als richtlijn voor het schrijven van code voor toegang tot uw mobiele service!

## <a name="how-your-project-is-modified"></a>Hoe uw project is gewijzigd
Hoe uw project in Visual Studio wordt gewijzigd, is afhankelijk van het projecttype. Voor C# client-apps, Zie [wat is er gebeurd: C# projecten](http://go.microsoft.com/fwlink/p/?LinkId=513119). Zie voor JavaScript-client-apps, [wat is er gebeurd: JavaScript-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513120). Zie voor Cordova-apps, [wat is er gebeurd: Cordova projecten](http://go.microsoft.com/fwlink/p/?LinkId=513116).

## <a name="next-steps"></a>Volgende stappen
Vragen stellen en hulp bij: 

* [MSDN-Forum: Azure Mobile Services](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)
* [Azure Mobile Services op het teamblog van Microsoft Azure](https://azure.microsoft.com/blog/topics/mobile/)
* [Azure Mobile Services op azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)
* [Azure Mobile Services-documentatie op azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)

