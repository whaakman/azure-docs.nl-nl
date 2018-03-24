---
title: Azure Standard Load Balancer - Diagnostics | Microsoft Docs
description: Met behulp van de beschikbare metrische gegevens en health-informatie voor diagnostische gegevens voor Azure Standard Load Balancer
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: Kumud
ms.openlocfilehash: 1d39cdc13e69740dc99e67f935b60db218536044
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="metrics-and-health-diagnostics-for-standard-load-balancer"></a>Metrische gegevens en de gezondheid van diagnostische gegevens voor de standaard Load Balancer

Azure Standard Load Balancer biedt de volgende diagnostische mogelijkheden voor uw resources:
* Multidimensionale metrische gegevens: Standaard Load Balancer biedt nieuwe mogelijkheden voor meerdere dimensies diagnostische voor openbare en interne Load Balancer-configuraties. Hierdoor kunt u bewaken, beheren en oplossen van uw resources voor de Load Balancer.

* Resourcestatus: De Load Balancer-pagina in de Azure portal en de resourcestatus pagina (Monitor) weer de status van de Resource voor de openbare Load Balancer-configuratie van de standaard Load Balancer.

Dit artikel bevat een kort overzicht van deze mogelijkheden en manieren om deze voor de standaard Load Balancer te gebruiken.

## <a name = "MultiDimensionalMetrics"></a>Multidimensionale metrische gegevens

Azure Load Balancer biedt de nieuwe multidimensionale metrische gegevens via de nieuwe Azure metrische gegevens (preview) in de portal en kunt u realtime diagnostische inzichten in uw resources voor de Load Balancer ophalen. 

Volgende metrische gegevens zijn nu beschikbaar voor verschillende configuraties van de standaard Load Balancer (LB):

| Gegevens | Resourcetype | Beschrijving | Aanbevolen aggregatie |
| --- | --- | --- | --- |
| VIP availability (beschikbaarheid van gegevens-pad) | Openbare Load Balancer | Standaard Load Balancer oefent continu het gegevenspad van binnen een regio voor de Load Balancer-frontend helemaal tot aan de stack SDN die ondersteuning biedt voor uw virtuele machine. Als in orde exemplaren blijft, volgt de meting hetzelfde pad als uw toepassing taakverdeling verkeer. Het gegevenspad die wordt gebruikt door uw klanten ook wordt gevalideerd. De meting is onzichtbaar voor uw toepassing en niet van invloed op een andere bewerkingen.| Gemiddeld |
| DIP beschikbaarheid (Health test status) |  Openbare & interne Load Balancer | Standaard Load Balancer gebruikmaakt van een gedistribueerde health-service die u de status van het toepassingseindpunt van uw volgens de configuratie-instellingen bewaakt te zoeken. Met deze metriek zorgt voor een statistische functie of groep per eindpunt gefilterd-weergave van elk eindpunt afzonderlijke exemplaar in de Load Balancer.  U kunt zien hoe de Load Balancer de status van uw toepassing, zoals aangegeven door uw configuratie van health test bekijkt. |  Gemiddeld |
| SYN-pakketten |  Openbare Load Balancer | Standaard Load Balancer niet beëindigen TCP-verbindingen of interactie met TCP of UDP pakketstromen. Stromen en hun handshakes zijn altijd tussen de bron en de VM-instantie. Als u wilt uw TCP-protocol scenario's op te lossen, kunt u het gebruik van SYN pakketten items om te begrijpen hoeveel TCP-verbinding is geprobeerd. De metriek rapporteert het aantal TCP SYN-pakketten die zijn ontvangen.| Gemiddeld |
| Verbindingen met snat omzetten |  Openbare Load Balancer |Standaard Load Balancer rapporteert het aantal uitgaande stromen die naar het openbare IP-adres frontend zijn masqueraded. Snat omzetten poorten zijn een onuitputtelijk resource. Met deze metriek kunt geven een indicatie van hoe zwaar uw toepassing is vertrouwen op snat omzetten voor uitgaande oorsprong stromen.  Tellers voor geslaagde en mislukte uitgaande snat omzetten stromen worden gerapporteerd, en kunnen worden gebruikt om problemen op te begrijpen van de status van uw uitgaande stromen.| Gemiddeld |
| Byte-prestatiemeteritems |  Openbare & interne Load Balancer | Standaard Load Balancer rapporten de gegevens per frontend verwerkt.| Gemiddeld |
| Pakket-prestatiemeteritems |  Openbare & interne Load Balancer | Standaard Load Balancer rapporteert de pakketten dat per frontend verwerkt.| Gemiddeld |

### <a name="view-load-balancer-metrics-in-the-portal"></a>Load Balancer metrische gegevens weergeven in de portal

Azure-portal wordt de Load Balancer metrische gegevens via de pagina metrische gegevens (Preview), die beschikbaar op de pagina Load Balancerresource voor een bepaalde bron van de Load Balancer en op de pagina Azure bewaken is. 

Om weer te geven de metrische gegevens voor uw resources standaard Load Balancer, bladeren van metrische gegevens (preview) op een van deze locaties, selecteert u de (load balancerresource als onder pagina Monitor) typt u metrische gegevens in de vervolgkeuzelijst het juiste samenvoegingstype ingesteld en, optioneel, Configureer de vereiste voor het filteren en groeperen en u zult kunnen zien van de historische weergave van de metrische gegevens onder de opgegeven voorwaarden.

![Voorbeeld van de metrische gegevens voor de standaard Load Balancer](./media/load-balancer-standard-diagnostics/LBMetrics1.png)

*Afbeelding - DIP beschikbaarheid / Health status metrische gegevens voor de Load Balancer-test*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>Multidimensionale metrische gegevens via een programma via API's ophalen

API-richtlijnen voor het ophalen van de multidimensionale metrische gegevens definities en -waarden is beschikbaar op [bewaking REST API Walkthrouh > multidimensionale API](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api)

### <a name = "DiagnosticScenarios"></a>Algemene scenario's voor diagnostische en aanbevolen weergaven

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>Het gegevenspad up-to-date en beschikbaar is voor mijn Load Balancer-VIP?

VIP beschikbaarheidsmetriek vereist beschrijft de status van het gegevenspad binnen de regio voor de compute-host waar uw virtuele machines zich bevinden. Het is de status van een reflectie van de Azure-infrastructuur. U kunt deze metriek te gebruiken:
- De externe beschikbaarheid van uw service bewaken
- Verdiepen en begrijpen of het platform waarop uw service wordt geïmplementeerd in orde is of dat het gastbesturingssysteem of toepassingsexemplaar zijn in orde
- Bepaal of een gebeurtenis is gerelateerd aan uw service of de onderliggende gegevens vlak. Verwar dit niet met de status van test ('DIP beschikbaarheid').

De VIP-beschikbaarheid ophalen voor uw resources standaard Load Balancer:
- Zorg ervoor dat de juiste bron van de Load Balancer is geselecteerd. 
- Selecteer **VIP beschikbaarheid** van de **metriek** vervolgkeuzelijst. 
- Selecteer **Gem** voor **aggregatie**. 
- Bovendien filter op de VIP-adres of de VIP-poort als de dimensie met de vereiste frontend-IP-adres of de front-endpoort en de groep toevoegen door de geselecteerde dimensie.

![VIP scannen](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Afbeelding - Load Balancer-VIP details zoeken*

De meetwaarde is gegenereerd door een actief, in-band-meting. Een zoek service binnen de regio afkomstig is van verkeer voor deze meting en wordt geactiveerd zodra u een implementatie met een openbaar front-end en blijft totdat u de frontend verwijdert. 

>[!NOTE]
>Interne frontends worden niet ondersteund op dit moment. 

Een pakket die overeenkomt met uw implementatie front-end- en regel wordt periodiek gegenereerd. Deze passeert de regio van de bron naar de host waarop een virtuele machine in de back-endpool zich bevindt. De Load Balancer-infrastructuur wordt dezelfde load balancing en vertaling bewerkingen uitvoeren als voor al het andere verkeer. Deze test wordt in-band op uw load balanced eindpunt. Zodra de test op de host Compute waar een gezonde VM in de back-endpool zich bevindt binnenkomt, wordt de Compute-host een reactie op de zoek service gegenereerd. Uw VM dit verkeer niet te zien.
Beschikbaarheid van de VIP is mislukt vanwege de volgende redenen:
- Uw implementatie heeft geen goede VM's resterend in de back-endpool. 
- Een infrastructuur storing is opgetreden dat ervoor zorgt dat de beschikbaarheid van de VIP mislukken.

U kunt de [VIP beschikbaarheidsmetriek vereist samen met de status van de test voor diagnostische doeleinden](https://aka.ms/lbdiagnostics#vipavailabilityandhealthprobes).

Gebruik **gemiddelde** als de aggregatie voor de meeste scenario's.

#### <a name="are-the-backend-instances-for-my-vip-responding-to-probes"></a>De back-end-exemplaren voor mijn VIP reageert op tests?

Health test status metriek beschrijft de status van de implementatie van uw toepassing geconfigureerd door u bij het configureren van de statuscontrole van de Load Balancer. Load Balancer gebruikmaakt van de status van de health test om te bepalen waar nieuwe stromen verzenden. Statuscontroles afkomstig zijn van het adres van een Azure-infrastructuur en zijn zichtbaar in het gastbesturingssysteem van de virtuele machine.

De beschikbaarheid van de DIP voor uw resources standaard Load Balancer ophalen
- Selecteer de **DIP beschikbaarheid** metrische met **Gem** samenvoegingstype. 
- Een filter toepassen op de vereiste IP-VIP-adres of -poort (of beide).

![Beschikbaarheid van de DIP](./media/load-balancer-standard-diagnostics/LBMetrics-DIPAvailability.png)

*Afbeelding - beschikbaarheid voor de Load Balancer VIP*

Statuscontroles is mislukt vanwege de volgende redenen:
- Als u een health test aan een poort die niet wordt geluisterd of reageert niet of met de verkeerde protocol configureert. Als uw service DSR (zwevend IP) regels gebruikt, moet u ervoor zorgen dat uw service op het IP-adres van de NIC IP-configuratie luistert en niet alleen op de loopback geconfigureerd met de frontend-IP-adres.
- Als uw test is niet toegestaan door de Netwerkbeveiligingsgroep, wordt de VM-Gast OS firewall of de toepassingslaag filtert.

Gebruik **gemiddelde** als de aggregatie voor de meeste scenario's.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>Hoe kan ik mijn statistieken uitgaande verbinding controleren? 

Verbindingen met snat omzetten metriek wordt het volume van geslaagde en mislukte beschreven (voor [uitgaande stromen](https://aka.ms/lboutbound)).

Een mislukte verbindingen volume groter is dan nul geeft aan uitputting van de poort snat omzetten. U moet verder onderzoek en bepalen wat de oorzaak van deze fouten. Uitputting van de poort snat omzetten manifesten als codefouten tot stand brengen een [uitgaande stromen](https://aka.ms/lboutbound). Lees het artikel op uitgaande verbindingen om te begrijpen van de scenario's en mechanismen op het werk en het verhelpen / ontwerpen om te voorkomen dat uitputting van de poort snat omzetten. 

Snat omzetten verbinding statistics ophalen
- Selecteer **snat omzetten verbindingen** type metrische gegevens en **som** als aggregatie. 
- groep **verbindingsstatus** voor geslaagde en mislukte snat omzetten verbinding telt dat wordt vertegenwoordigd door andere regels. 

![Verbinding met snat omzetten](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Afbeelding - snat omzetten verbinding aantal voor de Load Balancer*


#### <a name="how-do-i-check-inbound--outbound-connection-attempts-for-my-service"></a>Hoe kan ik voor mijn service Inkomend / uitgaand verbindingspogingen controleren?

SYN-pakketten metriek wordt het volume van TCP SYN-pakketten die zijn ontvangen of verzonden zijn beschreven (voor [uitgaande stromen](https://aka.ms/lboutbound)) die zijn gekoppeld aan een bepaalde frontend. Met deze metriek kan worden gebruikt om te begrijpen TCP-verbinding met uw service.
Totaal aantal gebruiken als de aggregatie voor de meeste scenario's.

![SYN verbinding](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Afbeelding - SYN aantal voor de Load Balancer*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>Hoe kan ik mijn gebruik van netwerkbandbreedte controleren? 

Byte / pakket tellers metriek beschrijft het volume van bytes en pakketten die door uw service op basis van de frontend-per verzonden of ontvangen.
Gebruik **totale** als de aggregatie voor de meeste scenario's.

Ophalen van pakket of Byte count-statistieken
- Selecteer de **aantal Bytes** en / of **aantal** type met metrische gegevens **Gem** als de aggregatie. 
- Filter toepassen op een specifieke frontend-IP-, front-endpoort of back-end-IP- of -poort in kwestie. 
- of algemene statistieken voor Load Balancer Resource zonder filters.


Sommige weergaven voorbeeld van metrische gegevens die met verschillende configuraties-

![Aantal bytes](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Afbeelding - aantal bytes voor de Load Balancer*

#### <a name = "vipavailabilityandhealthprobes"></a>Hoe ik mijn Load Balancer-implementatie onderzoeken?

De combinatie van de beschikbaarheid van de VIP & metrische gegevens op een afzonderlijke grafiek statuscontroles kunt u bepalen waar u zoekt u het probleem en het probleem is opgelost. U kunt krijgen zekerheid dat Azure correct werkt en afdoende bepaalt u de configuratie of toepassing de hoofdoorzaak is.

U kunt health test metrische gegevens gebruiken om te begrijpen hoe Azure weergaven voor de status van uw implementatie aan de hand van de configuratie die u hebt opgegeven. Kijken naar statuscontroles is altijd de eerste stap in de bewaking of een oorzaak te bepalen.

U kunt een stap meer uitvoeren en VIP beschikbaarheid metrische gegevens voor meer inzicht krijgen in hoe de Azure weergaven voor de status van de onderliggende gegevens vlak die verantwoordelijk is voor uw specifieke implementatie. Als u beide metrische gegevens combineert, kunt u vaststellen waar de fout mogelijk zoals geïllustreerd in dit voorbeeld:

![VIP diagnostische gegevens](./media/load-balancer-standard-diagnostics/LBMetrics-DIPnVIPAvailability.png)

*Afbeelding - combineren DIP en VIP beschikbaarheid metrische gegevens*

De grafiek bevat de volgende informatie:
- De infrastructuur zelf in orde is, de infrastructuur van uw virtuele machines host bereikbaar is en meer dan één virtuele machine in de back-end is geplaatst. Hiermee wordt aangegeven door de blauwe tracering voor beschikbaarheid van VIP op 100 toont %. 
- Test gezondheidsstatus (DIP beschikbaarheid) is echter bij 0% aan het begin van de grafiek zoals aangegeven door de oranje tracering. Het omcirkeld gebied in rood licht waar statuscontroles (DIP beschikbaarheid) is in orde geworden en op welk moment van de klant implementatie is nieuwe stromen accepteren.

De grafiek mag de klant oplossen van problemen met de implementatie op hun eigen zonder dat kan worden geraden of vraag ondersteuning of er zijn andere problemen optreden. De service van de klant is niet beschikbaar omdat statuscontroles zijn mislukt vanwege een onjuiste configuratie of een mislukte toepassing.

### <a name = "Limitations"></a>Beperkingen

- Beschikbaarheid van de VIP is momenteel alleen beschikbaar voor openbare frontends.

## <a name = "ResourceHealth"></a>De resourcestatus

Health-status voor de resources van de standaard Load Balancer is beschikbaar gemaakt via de bestaande **resourcestatus** onder **Monitor > servicestatus**.

>[!NOTE]
>De status van de Resource voor de Load Balancer is momenteel beschikbaar voor de openbare configuratie van alleen de standaard Load Balancer. Interne Load Balancer-bronnen of Basic SKU Load Balancer resources zichtbaar resourcestatus niet.

Om de status van uw openbare standaard Load Balancer-resources
 - Blader naar **Monitor > Health Service**.

  ![Pagina-monitor](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Afbeelding - servicestatus op Azure-Monitor*

 - Selecteer **resourcestatus** en zorg ervoor dat de juiste **abonnements-ID** en **resourcetype = Load Balancer** is geselecteerd.

  ![De resourcestatus](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Afbeelding - resource voor health weergave selecteren*

 - Klik op de Load Balancer-resource in de lijst om hun historische status weer te geven.

    ![Load Balancer gezondheidsstatus](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Afbeelding - Load Balancer resource status weergeven*
 
De volgende tabel bevat de verschillende resource-status en de bijbehorende beschrijvingen. 

| De resourcestatus | Beschrijving |
| --- | --- |
| Beschikbaar | Uw openbare standaard Load Balancer-resource is in orde en beschikbaar is |
| Niet beschikbaar | Uw openbare standaard Load Balancer-resource is niet in orde. De status via Azure Monitor onderzoeken > metrische gegevens. (*Dit kan ook betekenen bron is niet openbaar standaard Load Balancer*) |
| Onbekend | Status van resources voor uw openbare standaard Load Balancer is nog niet bijgewerkt. (*Dit kan ook betekenen bron is niet openbaar standaard Load Balancer*)  |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [standaard Load Balancer](load-balancer-standard-overview.md)
- Meer informatie over [Load Balancer uitgaande verbinding](https://aka.ms/lboutbound)


