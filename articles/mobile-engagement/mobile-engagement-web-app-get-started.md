---
title: Aan de slag met Azure Mobile Engagement voor web-apps | Microsoft Docs
description: Ontdek hoe u Azure Mobile Engagement gebruikt met analyses en pushmeldingen voor web-apps.
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04afe53a-4caf-4c80-bd75-20cc630cd75c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: hero-article
ms.date: 06/01/2016
ms.author: piyushjo
ms.openlocfilehash: abcb04e4e0a3ae4fdba3a4ded20b3846ac3b21e6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Aan de slag met Azure Mobile Engagement voor web-apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp wordt beschreven hoe u met Azure Mobile Engagement uw gebruik van web-apps leert te begrijpen.

> [!NOTE]
> De Azure Mobile Engagement-service wordt in maart 2018 beëindigd en is momenteel alleen beschikbaar voor bestaande klanten. Zie [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/) voor meer informatie.

Voor deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of een andere editor van uw keuze
* [Web SDK](http://aka.ms/P7b453)

Deze Web SDK bevindt zich nog in de previewfase en ondersteunt momenteel alleen Analytics en biedt geen ondersteuning voor het verzenden van pushmeldingen in browsers of apps. 

> [!NOTE]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started) voor meer informatie.
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a>Mobile Engagement instellen voor uw web-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Uw app verbinden met de back-end van Mobile Engagement
Deze zelfstudie toont een 'basisintegratie'. Dit is de minimale set die vereist is voor het verzamelen van gegevens.

We gaan een elementaire web-app maken met Visual Studio ter illustratie van de integratie, maar u kunt de stappen ook volgen met elke webtoepassing die buiten Visual Studio is gemaakt. 

### <a name="create-a-new-web-app"></a>Een nieuwe web-app maken
In de volgende stappen wordt ervan uitgegaan dat u Visual Studio 2015 gebruikt, hoewel de stappen hetzelfde zijn in eerdere versies van Visual Studio. 

1. Start Visual Studio en selecteer **New Project** in het scherm **Home**.
2. Selecteer **Web** -> **ASP.Net Web Application** (ASP.Net-webtoepassing) in de pop-up. Vul de **app-naam**, **locatie** en **oplossingsnaam** in en klik op **OK**.
3. Selecteer in de pop-up **Select a template** (Selecteer een sjabloon) de optie **Empty** (Leeg) onder **ASP.Net 4.5 Templates** (ASP.Net 4.5-sjablonen) en klik op **OK**. 

U hebt nu een nieuw, leeg web-app-project gemaakt waarin wij de Web SDK van Azure Mobile Engagement gaan integreren.

### <a name="connect-your-app-to-mobile-engagement-backend"></a>Uw app verbinden met de back-end van Mobile Engagement
1. Maak een nieuwe map genaamd **javascript** in uw oplossing en voeg het Web SDK JS-bestand **azure-engagement.js** eraan toe. 
2. Voeg met de volgende code een nieuw bestand met de naam **main.js** toe aan deze javascript-map. Zorg ervoor dat u de verbindingstekenreeks bijwerkt. Dit `azureEngagement`-object wordt gebruikt voor toegang tot Web SDK-methoden. 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Visual Studio met js-bestanden][1]

## <a name="enable-real-time-monitoring"></a>Realtime-bewaking inschakelen
U dient ten minste één activiteit naar de back-end van Mobile Engagement te sturen om te beginnen met het verzenden van gegevens en ervoor te zorgen dat de gebruikers actief zijn. Een activiteit in de context van een web-app is een webpagina. 

1. Maak een nieuwe pagina met de naam **home.html** in uw oplossing en stel deze in als de startpagina van uw web-app. 
2. Neem er de twee javascripts in op die we eerder aan deze pagina hebben toegevoegd door het volgende in de <body>-tag toe te voegen. 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. Werk de <body>-tag bij zodat de `startActivity`-methode van EngagementAgent wordt aangeroepen
   
        <body onload="engagement.agent.startActivity('Home')">
4. Zo ziet uw **home.html**-pagina eruit
   
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

## <a name="connect-app-with-real-time-monitoring"></a>App verbinden met realtime-bewaking
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

  ![][2]

## <a name="extend-analytics"></a>Analyse uitbreiden
Dit zijn alle methoden die momenteel bij Web SDK beschikbaar zijn die u voor analyses kunt gebruiken:

1. Activiteiten/webpagina's:
   
        engagement.agent.startActivity(name);
        engagement.agent.endActivity();
2. Gebeurtenissen
   
        engagement.agent.sendEvent(name, extras);
3. Fouten
   
        engagement.agent.sendError(name, extras);
4. Taken
   
        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

