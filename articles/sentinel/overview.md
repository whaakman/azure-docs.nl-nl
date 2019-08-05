---
title: Wat is Azure Sentinel preview? | Microsoft Docs
description: Meer informatie over Azure Sentinel, de belangrijkste mogelijkheden en hoe het werkt.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 10cce91a-421b-4959-acdf-7177d261f6f2
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 15cf770a372c9a1386dd0293abeac01fd3cacf63
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779750"
---
# <a name="what-is-azure-sentinel-preview"></a>Wat is Azure Sentinel preview?

> [!IMPORTANT]
> Azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Microsoft Azure Sentinel is een schaal bare, Cloud-native, SIEM-oplossing (Security **Information Event Management)** en **via (Security Orchestration Automated Response)** . Azure Sentinel levert intelligente beveiligings analyses en bedreigings informatie over de hele onderneming, waardoor er één oplossing is voor waarschuwings detectie, zicht baarheid van bedreigingen, proactieve jacht en reactie op bedreigingen. 

Azure Sentinel is uw vogel weer gave voor de hele onderneming die de stress van steeds meer geavanceerde aanvallen, het verhogen van de volumes van waarschuwingen en de duur van lange resoluties verkort.

- **Verzamel gegevens op Cloud schaal** voor alle gebruikers, apparaten, toepassingen en infra structuur, zowel on-premises als in meerdere clouds. 

- **Detecteer eerder**niet-gedetecteerde bedreigingen en Minimaliseer fout-positieven met behulp van de analytische en ongeëvenaarde bedreigings informatie van micro soft. 

- **Onderzoek bedreigingen met kunst matige intelligentie**en zoek naar verdachte activiteiten op schaal, tik op jaren van Cyber-Security-werk bij micro soft. 

- **Reageer snel op incidenten** met ingebouwde indeling en automatisering van algemene taken.


![Azure Sentinel Core-mogelijkheden](./media/overview/core-capabilities.png)

Azure Sentinel bouwt in op het volledige bereik van bestaande Azure-Services en maakt gebruik van beproefde stichtingen, zoals Log Analytics, en Logic Apps. Azure Sentinel verrijkt uw onderzoek en detectie met AI, en biedt de bedreigings informatie stroom van micro soft en stelt u in staat om uw eigen bedreigings informatie te nemen. 

 
## <a name="connect-to-all-your-data"></a>Verbinding maken met al uw gegevens

U moet eerst [verbinding maken met uw beveiligings bronnen](connect-data-sources.md)om Azure-Sentinel aan de trein te kunnen. Azure Sentinel wordt geleverd met een aantal connectors voor micro soft-oplossingen, die beschikbaar zijn in de doos en die realtime-integratie bieden, waaronder oplossingen voor micro soft Threat Protection, en Microsoft 365 bronnen, waaronder Office 365, Azure AD, Azure ATP en Microsoft Cloud App Security, en nog veel meer. Daarnaast zijn er ingebouwde connectors voor het bredere beveiligings ecosysteem voor niet-micro soft-oplossingen. U kunt ook algemene gebeurtenis indeling, syslog of REST-API gebruiken om uw gegevens bronnen met Azure Sentinel te verbinden.  

![Gegevens verzamelaars](./media/collect-data/collect-data-page.png)

## <a name="dashboards"></a>Dashboards

Nadat u verbinding hebt gemaakt met gegevens bronnen, kunt u kiezen uit een galerie met [deskundig gemaakte Dash boards](quickstart-get-visibility.md#dashboards) die inzicht krijgen in uw gegevens bronnen. Elk dash board is volledig aanpasbaar: u kunt uw eigen logica toevoegen of query's wijzigen, maar u kunt ook een volledig dash board maken.

De Dash boards bieden een interactieve visualisatie met behulp van geavanceerde analyse om uw beveiligings analisten te helpen beter inzicht te krijgen in wat er gebeurt tijdens een aanval. De hulpprogram ma's voor onderzoek bieden u de mogelijkheid om op elk wille keurig veld te gaan, van alle gegevens, om snel een risico context te ontwikkelen. 

![Dashboards](./media/overview/dashboards.png)

## <a name="analytics"></a>Analyse

Om het geluid te verminderen en het aantal waarschuwingen dat u moet controleren en te minimaliseren, gebruikt Azure Sentinel [Analytics om waarschuwingen in incidenten te correleren](tutorial-detect-threats.md). **Incidenten** zijn groepen gerelateerde waarschuwingen die samen een mogelijke bedreiging maken die kan worden onderzocht en opgelost. Gebruik de ingebouwde correlatie regels als-is of gebruik deze als uitgangs punt om uw eigen samen te stellen. Azure Sentinel biedt ook machine learning regels om uw netwerk gedrag toe te wijzen en vervolgens te zoeken naar afwijkingen in uw resources. Deze analyse verbindt de punten door waarschuwingen met een laag niveau te combi neren over verschillende entiteiten in potentiële beveiligings incidenten met hoge betrouw baarheid.

![Meldingen](./media/overview/cases.png)

## <a name="user-analytics"></a>Gebruikersanalyses

Met de systeem eigen integratie van machine learning (ML) en [gebruikers analyse](user-analytics.md)kan Azure Sentinel snel bedreigingen helpen detecteren. Azure Sentinel kan naadloos worden geïntegreerd met Azure Advanced Threat Protection om gebruikers gedrag te analyseren en te bepalen welke gebruikers u het eerst moet onderzoeken, op basis van hun waarschuwingen en verdachte activiteiten patronen in azure Sentinel en Microsoft 365.

![Gebruikersanalyses](./media/overview/user-analytics.png)


## <a name="security-automation--orchestration"></a>Integratie van & voor beveiligings automatisering

Automatiseer uw algemene taken en [Vereenvoudig de beveiligings indeling met playbooks](tutorial-respond-threats-playbook.md) die integreren met Azure-Services en uw bestaande hulpprogram ma's. De automatiserings-en Orchestration-oplossing van Azure Sentinel is gebouwd op basis van Azure Logic Apps en biedt een uiterst uitbreid bare architectuur waarmee schaal bare automatisering kan worden ingeschakeld als nieuwe technologieën en bedreigingen. Als u playbooks met Azure Logic Apps wilt maken, kunt u kiezen uit een groeiende galerie met ingebouwde playbooks. Dit zijn onder meer [200 en connectors](https://docs.microsoft.com/azure/connectors/apis-list) voor services, zoals Azure functions. Met de connectors kunt u aangepaste logica Toep assen in code, ServiceNow, Jira, Zendesk, HTTP-aanvragen, micro soft-teams, toegestane vertraging, Windows Defender ATP en Cloud App Security.

Als u bijvoorbeeld het ServiceNow-ticket systeem gebruikt, kunt u de hulpprogram ma's gebruiken die Azure Logic Apps gebruiken om uw werk stromen te automatiseren en een ticket in ServiceNow te openen telkens wanneer een bepaalde gebeurtenis wordt gedetecteerd.

![Playbooks](./media/tutorial-respond-threats-playbook/logic-app.png)



## <a name="investigation"></a>Onderzoek

De hulpprogram ma's voor [grondige onderzoek](tutorial-investigate-cases.md) van Azure geven u een beter inzicht in het bereik en vinden de hoofd oorzaak van een mogelijke beveiligings risico. U kunt een entiteit kiezen in de interactieve grafiek om interessante vragen te stellen voor een specifieke entiteit, en inzoomen op die entiteit en de bijbehorende verbindingen om de hoofd oorzaak van de dreiging te bereiken. 

![Onderzoek](./media/overview/investigation.png)


## <a name="hunting"></a>Zoeken

Gebruik de [krachtige hulpprogram ma's voor zoeken en](hunting.md)doorzoeken van Azure Sentinel, op basis van het Mitre-Framework, waarmee u op proactieve wijze beveiligings Risico's in de gegevens bronnen van uw organisatie kunt vinden voordat een waarschuwing wordt geactiveerd. Nadat u hebt gedetecteerd welke Zoek query hoge waarden biedt voor mogelijke aanvallen, kunt u ook aangepaste detectie regels maken op basis van uw query, en deze inzichten als waarschuwingen voor uw beveiligings incident responders aanwijzen. Tijdens de jacht kunt u blad wijzers voor interessante gebeurtenissen maken, zodat u ze later weer terug kunt gebruiken, delen met anderen en ze groeperen met andere correlatie gebeurtenissen om een aantrekkelijke Case voor onderzoek te maken.

![Zoeken](./media/overview/hunting.png)

## <a name="community"></a>Community

De Azure Sentinel Community is een krachtige bron voor detectie en automatisering van bedreigingen. Onze micro soft-beveiligings analisten maken en voegen voortdurend nieuwe Dash boards, playbooks, jacht-query's en meer toe, die u in uw omgeving kunt gebruiken om ze in de community te plaatsen. U kunt voorbeeld inhoud downloaden van de GitHub- [opslag plaats](https://aka.ms/asicommunity) van de privé-community om aangepaste Dash boards, jacht-query's, notitie blokken en Playbooks voor Azure-Sentinel te maken. 

![Community](./media/overview/community.png)

## <a name="next-steps"></a>Volgende stappen

- U hebt een abonnement op Microsoft Azure nodig om aan de slag te gaan met Azure Sentinel. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/free/).
- Meer informatie over het [voorbereiden van uw gegevens naar Azure Sentinel](quickstart-onboard.md)en [inzicht krijgen in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
