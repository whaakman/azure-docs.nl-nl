---
title: Wat is Azure Sentinel Preview? | Microsoft Docs
description: Meer informatie over Azure Sentinel, de belangrijkste mogelijkheden en hoe het werkt.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 10cce91a-421b-4959-acdf-7177d261f6f2
ms.service: sentinel
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 2859df94b1418cfa9fbfaa341e6bf08b257875bf
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57245282"
---
# <a name="what-is-azure-sentinel-preview"></a>Wat is Azure Sentinel Preview?

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Microsoft Azure Sentinel is een schaalbare cloud-native, **gebeurtenis het beheer van beveiligingsinformatie (SIEM)** en **security orchestration geautomatiseerd antwoord (ZWEEF)** oplossing. Azure Sentinel biedt intelligente analyses en bedreigingsinformatie beveiligingsinformatie binnen de onderneming, voorzien in één oplossing waarschuwing detectie, zichtbaarheid van de bedreiging, proactieve jacht en bedreigingen te kunnen reageren. 

Azure Sentinel is de weergave dieren ogen binnen de hele organisatie die de belasting van het steeds geavanceerder wordende aanvallen te verlichten grotere hoeveelheden van waarschuwingen, en het lang resolutie tijdsbestek.

- **Verzamelen van gegevens in de cloud** tussen alle gebruikers, apparaten, toepassingen en infrastructuur, zowel on-premises en in meerdere clouds. 

- **Bedreigingen in een eerder onopgemerkte**, en met behulp van de analyses en daarmee een ongeëvenaarde bedreigingsinformatie van Microsoft fout-positieven te minimaliseren. 

- **Bedreigingen met kunstmatige intelligentie onderzoeken**, en het opsporen van verdachte activiteiten op schaal, profiteren van jarenlange cyber beveiliging werken bij Microsoft. 

- **Snel reageren op incidenten** met ingebouwde indeling en automatisering van algemene taken.


![Azure Sentinel belangrijkste mogelijkheden](./media/overview/core-capabilities.png)

Voortbouwend op het volledige bereik van de bestaande Azure-services, Azure Sentinel systeemeigen is uitgerust met bewezen fundamenten, zoals Log Analytics en Logic Apps. Azure Sentinel verrijkt uw onderzoek en detectie met AI, en biedt Microsoft threat intelligence stream en kunt u om uw eigen bedreigingsinformatie. 

 
## <a name="connect-to-all-your-data"></a>Verbinding maken met al uw gegevens

Aan het ingebouwde Azure-Sentinel, moet u eerst [verbinding maken met de bronnen van uw beveiliging](connect-data-sources.md). Azure Sentinel wordt geleverd met een aantal connectors voor Microsoft-oplossingen, beschikbaar is buiten het vak en biedt realtime-integratie, met inbegrip van oplossingen van Microsoft Threat Protection en Microsoft 365-bronnen, zoals Office 365, Azure AD, Azure ATP, en Microsoft Cloud App Security, en meer. Er zijn bovendien ingebouwde connectors voor het complete ecosysteem van de beveiliging voor niet-Microsoft-oplossingen. U kunt ook de algemene indeling voor gebeurtenissen, Syslog of REST-API om uw gegevensbronnen ook verbinding met Azure Sentinel te gebruiken.  

![Gegevens collectors](./media/collect-data/collect-data-page.png)

## <a name="dashboards"></a>Dashboards

Na de verbinding met gegevensbronnen die u kunt kiezen uit een galerie met [vakkundig gemaakt dashboards](quickstart-get-visibility.md#dashboards) Maak die inzichten duidelijk uit uw gegevensbronnen. Elk dashboard is volledig aanpasbaar: u kunt uw eigen logica toevoegen of wijzigen van query's of kunt u een dashboard maken.

De dashboards bieden interactieve visualisatie uw beveiligingsanalisten inzicht in wat er gebeurt tijdens een aanval met behulp van geavanceerde analyses. De hulpprogramma's voor onderzoek kunnen u gedetailleerde informatie over elk veld in alle gegevens, voor het snel ontwikkelen van threat context. 

![Dashboards](./media/overview/dashboards.png)

## <a name="analytics"></a>Analyse

Om te helpen u ruis verlagen en Beperk het aantal waarschuwingen dat u hebt om te controleren en onderzoeken, gebruikt Azure Sentinel [analytics voor het correleren van waarschuwingen in gevallen](tutorial-detect-threats.md). **Gevallen** zijn groepen van gerelateerde waarschuwingen die samen een bruikbare mogelijk-bedreiging die u kunt onderzoeken en oplossen van maken. Gebruik de ingebouwde correlatie regels als-is, of het gebruik als een beginpunt voor het bouwen van uw eigen. Azure Sentinel biedt ook een machine learning-regels voor het toewijzen van het gedrag van uw netwerk en vervolgens zoeken naar afwijkingen in uw resources. Deze analyses verbinding maken met de punten door onbetrouwbare waarschuwingen over verschillende entiteiten combineren in mogelijke uiterst betrouwbare en beveiligingsincidenten.

![Aanvragen](./media/overview/cases.png)

## <a name="user-analytics"></a>Gebruikersanalyses

Met systeemeigen integratie van machine learning (ML), en [gebruiker analytics](user-analytics.md), Azure Sentinel kunnen helpen snel bedreigingen detecteren. Azure Sentinel wordt naadloos geïntegreerd met Azure Advanced Threat Protection op gebruikersgedrag te analyseren en prioriteiten te stellen welke gebruikers dat u eerst moet onderzoeken op basis van hun waarschuwingen en patronen van verdachte activiteiten in Azure Sentinel en Microsoft 365.

![Gebruikersanalyses](./media/overview/user-analytics.png)


## <a name="security-automation--orchestration"></a>Beveiliging-automatisering en indeling

Uw algemene taken automatiseren en [vereenvoudigen security orchestration met playbooks](tutorial-respond-threats-playbook.md) die zijn geïntegreerd met Azure-services, evenals uw bestaande hulpprogramma's. Gebouwd op basis van Azure Logic Apps, van Azure Sentinel automatisering en indeling van de oplossing biedt een uiterst extensible architectuur waarmee schaalbare automation als nieuwe technologieën en bedreigingen indelingsversies beschikbaar komen. Playbooks met Azure Logic Apps bouwen, kunt u kiezen uit een steeds groter wordende galerie van ingebouwde playbooks. Het gaat hierbij [meer dan 200 connectoren](https://docs.microsoft.com/azure/connectors/apis-list) voor services zoals Azure functions. De connectors kunnen u om een aangepaste logica in de code, ServiceNow, Jira, Zendesk, HTTP-aanvragen, Microsoft Teams, Slack, Windows Defender ATP en Cloud App Security te passen.

Bijvoorbeeld, als u de ServiceNow-ticketsysteem gebruikt, kunt u de hulpprogramma's voor Azure Logic Apps gebruiken voor het automatiseren van uw werkstromen en opent u een ticket in ServiceNow telkens wanneer een bepaalde gebeurtenis wordt gedetecteerd.

![Playbooks](./media/tutorial-respond-threats-playbook/logic-app.png)



## <a name="investigation"></a>Onderzoek

Azure Sentinel [deep onderzoek](tutorial-investigate-cases.md) hulpprogramma's kunnen u inzicht in het bereik en de hoofdoorzaak van een potentieel beveiligingsrisico. U kunt een entiteit in de interactieve grafiek interessante vragen voor een specifieke entiteit en inzoomen op die entiteit en de bijbehorende verbindingen om te gaan naar de hoofdoorzaak van de bedreiging. 

![Onderzoek](./media/overview/investigation.png)


## <a name="hunting"></a>Zoeken

Gebruik van Azure Sentinel [jacht krachtige hulpprogramma's zoeken en query's](hunting.md), op basis van het framework MITRE, zodat u proactief te zoeken op beveiligingsrisico's in uw organisatie gegevensbronnen, voordat een waarschuwing wordt geactiveerd. Nadat u welke query jacht biedt waardevolle inzichten in het mogelijke aanvallen ontdekt, kunt u ook aangepaste detectieregels op basis van uw query maken, en maak die inzichten duidelijk als waarschuwingen aan uw security incident beantwoorders. Tijdens het zoeken, kunt u bladwijzers maken voor interessante gebeurtenissen, zodat u kunt later naar wilt kunnen terugkeren, ze delen met anderen en ze te groeperen met andere correlerende gebeurtenissen om een aantrekkelijke aanvraag voor onderzoek te maken.

![Zoeken](./media/overview/hunting.png)

## <a name="community"></a>Community

De Azure-Sentinel community is een krachtige resource voor detectie van bedreigingen en automatisering. Onze beveiligingsanalisten Microsoft wordt voortdurend maken en toevoegen van nieuwe dashboards, playbooks, jacht query's en meer, plaatsen ze aan de community voor u in uw omgeving. U kunt voorbeelden voor inhoud downloaden van het privé-community GitHub [opslagplaats](https://aka.ms/asicommunity) aangepaste dashboards, jacht query's, laptops en playbooks voor Azure Sentinel maken. 

![Community](./media/overview/community.png)

## <a name="next-steps"></a>Volgende stappen

- Als u wilt aan de slag met Azure Sentinel, moet u een abonnement op Microsoft Azure. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/free/).
- Meer informatie over het [onboarding uw gegevens naar Azure Sentinel](quickstart-onboard.md), en [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
