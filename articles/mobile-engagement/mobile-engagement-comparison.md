---
title: Azure Mobile Engagement vergelijken met andere vergelijkbare Azure-services
description: Azure Mobile Engagement vergelijken met andere vergelijkbare Azure-services - HockeyApp, AppInsights, Notification Hubs
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f114775-3a9a-4dd4-8d59-b10d1da9a68b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 7df2eb9ecebe3313dad9c15171552a084787f6b8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Azure Mobile Engagement vergelijken met andere vergelijkbare Azure-services
De lijst met services die worden aangeboden door Microsoft Azure is ooit uitbreidbare en op tijdstippen u vraagt zich misschien af hoe verschilt van Azure Mobile Engagement deze service die u zojuist hebt gelezen of gehoord. In dit artikel, probeert de verwarring wissen en stuurt u Azure Mobile Engagement kiezen wanneer is het meest geschikt voor gebruik van uw. 

Azure Mobile Engagement is een service die specifiek gericht **voor digitale marketeers/CMOs** , maar kan worden gebruikt door een **eigenaar van de mobiele app of publisher** die wil de retentie-gebruik verhogen en monetization van hun mobiele apps. 

*Het is een software-as-a-service (SaaS) gebruiker-platform waarmee biedt: gegevensgestuurde inzichten in het gebruik van apps, segmentering van gebruikers in real, en kunt u contextbewuste pushmeldingen en in-app-berichten.* 

De definitie voor dit te analyseren, hebben we de volgende belangrijke kenmerken die u ook markeert de toegevoegde unieke waarde:

1. **Contextbewuste pushmeldingen en in-app-berichten**
   
   Dit is de focus core van het product - gerichte en gepersonaliseerde pushmeldingen uitvoeren. En dit gebeurt, verzamelen wij uitgebreide gedragsanalyse gegevens. 
2. **Gegevensgestuurde inzichten in app-gebruik**
   
   We bieden platformoverschrijdende SDK's voor het verzamelen van de gedragsanalyses over de app-gebruikers. Noteer de term gedragsanalyse (tegenover prestaties analytics) omdat er zich richten op hoe de app-gebruikers de app gebruiken. We worden basisprestaties analytische gegevens over fouten, crashes enzovoort maar dat is niet de focus core van het product verzameld. 
3. **Gebruikers in real-segmentering**
   
   Nadat u app-gebruikers gedragsanalyse gegevens hebt verzameld, kunt we u segmenten uw doelgroep op basis van verschillende parameters en verzamelde gegevens waarmee u gerichte pushcampagnes uitvoeren. 
4. **Software-as-a-service (SaaS):**
   
   We hebben een gescheiden van de Azure-beheerportal dat is geoptimaliseerd om te communiceren en uitgebreide gedragsanalyse over de app-gebruikers weergeven en uitvoeren push marketingcampagnes portal. Het product is afgestemd op u die in geen enkel moment gaan!   

Met deze reeks van differentiatie in hand Hier ziet u hoe we vergeleken op basis van andere Azure-schijnbaar vergelijkbare producten - aanpak om te definiëren welke product werkt het beste op basis van het artikel bevat een vergelijking van functies niveau niet, maar duurt een meer scenario:

1. [HockeyApp](https://azure.microsoft.com/services/hockeyapp/) is de Microsoft mobiele DevOps-oplossing. Met HockeyApp kunt u builds naar bètatesters distribueren, crashgegevens verzamelen en feedback van gebruikers ophalen. Het is ook geïntegreerd met Visual Studio Team Services, zodat u gemakkelijk builds kunt implementeren en werkitems kunt integreren. 
   
   Hier worden op DevOps en het verzamelen van prestaties analytische gegevens over de mobiele apps. U kan eindigen met het integreren van beide HockeyApps en Mobile Engagement in uw app en die kan niet worden ongebruikelijke omdat Hoewel sommige elkaar overlappen in de verzamelde gegevens, de focus core producten verschilt en ze helpen bij het bereiken van verschillende doelstellingen voor u hebt.  
2. [Application Insights](../application-insights/app-insights-overview.md) als uw mobiele app een serverzijde heeft en u Application Insights gebruiken voor het bewaken van de webserverzijde van uw app, maar voor de client side mobiele apps, moet u HockeyApp. 
3. [Notification Hubs](https://azure.microsoft.com/services/notification-hubs/) biedt een lichtgewicht service in de zin dat een geïntegreerd in de mobiele app SDK hoeft u niet en er volledig beheer van uw mobiele app en kunt u pushmeldingen verzenden met de basisfuncties van labels. Dit is ideaal voor een app-eigenaar op die kleiner over doelen en meer gegevens verzenden/communiceren direct voor hun app-gebruikers (broadcast met een grote populatie) is belangrijk. 
   
   Noteer de focus bij verzenden razendsnelle snelle meldingen met basisindeling-functionaliteit. 

We gaan een aantal scenario's:

1. TIM maakt deel uit van de digitale marketing team van Adventure Works store gepubliceerd mobiele apps voor gebruikers. De TIM doel is om ervoor te zorgen dat de gebruikers die hun mobiele apps downloaden hiervan gebruik blijven en regelmatig. Dit verhoogt niet alleen een merk koppelen met de app-gebruikers, maar ook toeneemt monetization wanneer de app-gebruikers met de mobiele app aankopen aanbrengt. Voor dit nodig Tim gerichte om meldingen te verzenden naar de app-gebruikers die ze nuttig vinden om te open de app en keert u terug naar dit vaak. Dit is waar Tim nuttig Mobile Engagement. 
2. Joann maakt deel uit van het ontwikkelingsteam van de mobiele apps bij Adventure Works en een product aan te melden gegevens over crashes, uitzonderingen, en prestatietelemetrie ophalen uit een app zoekt. Dit is waar Joann nuttig HockeyApp. Joann kan zowel HockeyApp voor haar scenario's voor ontwikkelaars die zijn gericht en Azure Mobile Engagement voor Tim integreren in dezelfde app om op te halen van het beste van beide. 
3. Robin maakt deel uit van het ontwikkelingsteam van de mobiele apps op het netwerk van Contoso nieuws en alle die ze wil verzenden van meldingen op alle gebruikers zonder veel segmentering op te splitsen zodra de nieuws waarschuwing wordt geactiveerd. Dit is waar Robin nuttig Notification Hubs. 
   In dit scenario wordt is het echter mogelijk dat naarmate de mobiele app meer vormt krijgt, er is een vereiste veel uitgebreidere segmentering doen en meer informatie over het gedrag van de gebruiker van de app. Op dit moment moet Robin Azure Mobile Engagement te evalueren. 

Als u wilt samenvatting, het doel van Mobile Engagement is niet alleen wilt verzamelen analytics - niet 'nog een andere Analytics product van Microsoft'. Het is over gerichte pushmeldingen verzenden en voor deze doelen we verzamelen van gegevens van gedragsanalyses maar blijft de focus op het verzenden van pushmeldingen zodat het is verstandig aan de app-gebruikers, zodat wordt niet geleverd op als spam. 

Voor meer informatie - Kijk eens naar dit [korte video](mobile-engagement-overview.md) over Mobile Engagement in een nutshell. 

