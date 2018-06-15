---
title: Diagnostische gegevens van Azure Standard Load Balancer | Microsoft Docs
description: Gebruik de beschikbare metrische gegevens en health-informatie voor diagnostische gegevens voor Azure Standard Load Balancer.
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
ms.openlocfilehash: 9d5d596254f673b86650e8d9754dacdb70be0666
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32179791"
---
# <a name="metrics-and-health-diagnostics-for-standard-load-balancer"></a>Metrische gegevens en de gezondheid van diagnostische gegevens voor de standaard Load Balancer

Azure Standard Load Balancer biedt uw resources van de volgende diagnostische mogelijkheden:
* **Multidimensionale metrische gegevens**: nieuwe multi-dimensionale diagnostische mogelijkheden biedt voor openbare en interne load balancer-configuraties. U kunt controleren, beheren en oplossen van de load balancer-resources.

* **Resourcestatus**: de Load Balancer-pagina in de Azure-portal en de bron Health-pagina (onder Monitor) tonen de resourcestatus sectie voor de openbare load balancer-configuratie van standaard Load Balancer.

Dit artikel bevat een kort overzicht van deze mogelijkheden en manieren om ze te gebruiken voor een standaard Load Balancer biedt.

## <a name = "MultiDimensionalMetrics"></a>Multidimensionale metrische gegevens

Azure Load Balancer biedt nieuwe multidimensionale metrische gegevens via de nieuwe Azure metrische gegevens (preview) in de Azure portal en kunt u de real-time diagnostische inzicht in de load balancer bronnen ophalen. 

De verschillende standaard Load Balancer-configuraties bieden de volgende metrische gegevens:

| Gegevens | Resourcetype | Beschrijving | Aanbevolen aggregatie |
| --- | --- | --- | --- |
| VIP availability (beschikbaarheid van gegevens-pad) | Openbare load balancer | Standaard Load Balancer oefent continu het gegevenspad van binnen een regio voor de load balancer-front-end, helemaal tot aan de stack SDN die ondersteuning biedt voor uw virtuele machine. Als in orde exemplaren blijft, volgt de meting hetzelfde pad als uw toepassing taakverdeling verkeer. Het gegevenspad die gebruikmaken van uw klanten ook wordt gevalideerd. De meting is onzichtbaar voor uw toepassing en niet van invloed op een andere bewerkingen.| Gemiddeld |
| DIP beschikbaarheid (health test status) |  Openbare en interne load balancer | Standaard Load Balancer gebruikmaakt van een gedistribueerde scannen van health service die u de status van het toepassingseindpunt van uw volgens de configuratie-instellingen bewaakt. Met deze metriek biedt een aggregatie of per eindpunt gefilterde weergave van elk eindpunt exemplaar in de load balancer-groep. U kunt zien hoe Load Balancer weergaven voor de status van uw toepassing, zoals aangegeven door uw configuratie van health test. |  Gemiddeld |
| SYN (synchroniseren) pakketten |  Openbare load balancer | Standaard Load Balancer niet beëindigen Transmission Control Protocol (TCP)-verbindingen of interactie met TCP of UDP pakketstromen. Stromen en hun handshakes zijn altijd tussen de bron en de VM-instantie. Als u wilt uw TCP-protocol scenario's op te lossen, kunt u het gebruik van SYN pakketten items om te begrijpen hoeveel TCP-verbinding is geprobeerd. De metriek rapporteert het aantal TCP SYN-pakketten die zijn ontvangen.| Gemiddeld |
| Verbindingen met snat omzetten |  Openbare Load Balancer |Standaard Load Balancer rapporteert het aantal uitgaande stromen die naar de front-end van het openbare IP-adres zijn masqueraded. Bronpoorten netwerk address translation (snat omzetten) zijn een onuitputtelijk resource. Met deze metriek kunt geven een indicatie van hoe zwaar uw toepassing is vertrouwen op snat omzetten voor uitgaande oorsprong stromen. Tellers voor geslaagde en mislukte uitgaande snat omzetten stromen worden gerapporteerd, en kunnen worden gebruikt om problemen op te begrijpen van de status van uw uitgaande stromen.| Gemiddeld |
| Byte-prestatiemeteritems |  Openbare en interne load balancer | Standaard Load Balancer rapporten de gegevens die worden verwerkt per front-end.| Gemiddeld |
| Pakket-prestatiemeteritems |  Openbare en interne load balancer | Standaard Load Balancer rapporteert de pakketten dat per front-end verwerkt.| Gemiddeld |

### <a name="view-your-load-balancer-metrics-in-the-azure-portal"></a>De load balancer metrische gegevens weergeven in de Azure-portal

De Azure-portal wordt de load balancer metrische gegevens via de pagina metrische gegevens (preview), die beschikbaar is op zowel de load balancer resourcepagina voor een bepaalde bron- en de pagina Azure bewaken. 

Om weer te geven de metrische gegevens voor uw resources standaard Load Balancer:
1. Ga naar de pagina metrische gegevens (preview) en het volgende doen:
   * Selecteer op de pagina van de resource in de load balancer, het type metrische gegevens in de vervolgkeuzelijst.
   * Selecteer op de pagina Azure Monitor de load balancer-resource.
2. Stel de juiste samenvoegingstype.
3. Configureer desgewenst de vereiste voor het filteren en groeperen.

![Voorbeeld van de metrische gegevens voor de standaard Load Balancer](./media/load-balancer-standard-diagnostics/LBMetrics1.png)

*Afbeelding: De DIP beschikbaarheid en health test status meetwaarde voor standaard Load Balancer*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>Multidimensionale metrische gegevens via een programma via API's ophalen

Zie voor instructies voor het ophalen van multidimensionale metrische definities en waarden API [REST-API voor Azure-bewaking scenario](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api).

### <a name = "DiagnosticScenarios"></a>Algemene scenario's voor diagnostische en aanbevolen weergaven

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>Het gegevenspad en beschikbaar is voor mijn load balancer-VIP?

De VIP beschikbaarheid metriek beschrijft de status van het gegevenspad binnen de regio voor de compute-host waar uw virtuele machines zich bevinden. De meetwaarde is een reflectie van de status van de Azure-infrastructuur. U kunt de metrische gegevens om te gebruiken:
- De externe beschikbaarheid van uw service bewaken
- Verdiepen en begrijpen of het platform waarop uw service wordt geïmplementeerd in orde of of uw gastbesturingssysteem of een toepassingsexemplaar in orde is.
- Bepaal of een gebeurtenis is gerelateerd aan uw service of de onderliggende gegevens vlak. Verwar niet met deze metriek met de status van test ('DIP beschikbaarheid').

De VIP-beschikbaarheid ophalen voor uw resources standaard Load Balancer:
1. Zorg ervoor dat de juiste load balancer-resource is geselecteerd. 
2. In de **metriek** vervolgkeuzelijst, selecteer **VIP beschikbaarheid**. 
3. In de **aggregatie** vervolgkeuzelijst, selecteer **Gem**. 
4. Bovendien een filter op de VIP-adres of de VIP-poort als de dimensie met de vereiste front-end-IP-adres of de front-endpoort toevoegen en vervolgens gegroepeerd op de geselecteerde dimensie.

![VIP scannen](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Afbeelding: Load Balancer-VIP details zoeken*

De meetwaarde is gegenereerd door een actief, in-band-meting. Een zoek service binnen de regio is afkomstig van verkeer voor de meting. De service is geactiveerd zodra u een implementatie met een openbaar front-end en wordt herhaald totdat u de front-end verwijderen. 

>[!NOTE]
>Interne front-ends worden niet ondersteund op dit moment. 

Een pakket die overeenkomt met de front-end van uw implementatie en de regel wordt periodiek gegenereerd. Deze passeert de regio van de bron naar de host waarop een virtuele machine in de groep back-end zich bevindt. De load balancer-infrastructuur voert dezelfde load balancing en vertaling bewerkingen als voor al het andere verkeer. Deze test wordt in-band op uw eindpunt taakverdeling. Nadat de test op de host compute binnenkomt, waar een gezonde VM in de groep back-end zich bevindt, genereert de compute-host een reactie op de zoek service. Uw VM dit verkeer niet te zien.

Beschikbaarheid van de VIP is mislukt vanwege de volgende redenen:
- Uw implementatie heeft geen goede VM's resterend in de groep back-end. 
- Een infrastructuur storing is opgetreden.

Voor diagnostische doeleinden, kunt u de [VIP beschikbaarheidsmetriek vereist samen met de status van de test](#vipavailabilityandhealthprobes).

Gebruik **gemiddelde** als de aggregatie voor de meeste scenario's.

#### <a name="are-the-back-end-instances-for-my-vip-responding-to-probes"></a>De back-end-exemplaren voor mijn VIP reageert op tests?

De metriek health test status beschrijft de status van de implementatie van uw toepassing geconfigureerd door u bij het configureren van de statuscontrole van de load balancer. De load balancer gebruikmaakt van de status van de health test om te bepalen waar nieuwe stromen verzenden. Statuscontroles afkomstig zijn van het adres van een Azure-infrastructuur en zijn zichtbaar in het gastbesturingssysteem van de virtuele machine.

De beschikbaarheid van de DIP ophalen voor uw resources standaard Load Balancer:
1. Selecteer de **DIP beschikbaarheid** metrische met **Gem** samenvoegingstype. 
2. Een filter toepassen op de vereiste IP-VIP-adres of -poort (of beide).

![Beschikbaarheid van de DIP](./media/load-balancer-standard-diagnostics/LBMetrics-DIPAvailability.png)

*Afbeelding: Beschikbaarheid van de Load Balancer-VIP*

Statuscontroles is mislukt vanwege de volgende redenen:
- Configureren van een health test met een poort die niet wordt geluisterd of reageert niet of het juiste protocol wordt gebruikt. Als uw service gebruikmaakt van rechtstreekse serverretournering (DSR of zwevend IP) worden de regels, zorg ervoor dat de service op het IP-adres van de NIC IP-configuratie luistert en niet alleen op de loopback die geconfigureerd met het front-end-IP-adres.
- De test is niet toegestaan door de Netwerkbeveiligingsgroep, van de VM-Gast OS-firewall of de toepassing laag filters.

Gebruik **gemiddelde** als de aggregatie voor de meeste scenario's.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>Hoe kan ik mijn statistieken uitgaande verbinding controleren? 

De verbindingen snat omzetten metriek wordt het volume van geslaagde en mislukte verbindingen voor beschreven [uitgaande stromen](https://aka.ms/lboutbound).

Een mislukte verbindingen volume groter is dan nul geeft aan uitputting van de poort snat omzetten. U moet verder onderzoeken om te bepalen wat de oorzaak van deze fouten. Uitputting van de poort snat omzetten manifesten als een fout tot stand brengen een [uitgaande stroom](https://aka.ms/lboutbound). Lees het artikel over uitgaande verbindingen voor inzicht in de scenario's en -mechanismen op het werk, en voor meer informatie over het beperken van en ontwerpen om te voorkomen dat uitputting van de poort snat omzetten. 

Snat omzetten verbindingsstatistieken ophalen:
1. Selecteer **snat omzetten verbindingen** type metrische gegevens en **som** als aggregatie. 
2. Groeperen op **verbindingsstatus** voor geslaagde en mislukte snat omzetten verbinding aantallen die worden vertegenwoordigd door andere regels. 

![Verbinding met snat omzetten](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Afbeelding: Aantal voor Load Balancer snat omzetten verbindingen*


#### <a name="how-do-i-check-inboundoutbound-connection-attempts-for-my-service"></a>Hoe kan ik voor mijn service binnenkomend en uitgaand verbindingspogingen controleren?

Een SYN pakketten metriek wordt het volume van TCP SYN-pakketten die zijn ontvangen of verzonden beschreven (voor [uitgaande stromen](https://aka.ms/lboutbound)) die zijn gekoppeld aan een specifieke front-end. Met deze metriek kunt u inzicht in TCP verbindingspogingen met uw service.

Gebruik **totale** als de aggregatie voor de meeste scenario's.

![SYN verbinding](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Afbeelding: Load Balancer SYN tellen*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>Hoe kan ik mijn gebruik van netwerkbandbreedte controleren? 

De bytes en het pakket tellers metriek wordt het volume van bytes en pakketten die zijn verzonden of ontvangen door de service op basis van de per-front-end beschreven.

Gebruik **totale** als de aggregatie voor de meeste scenario's.

Statistieken voor byte of pakket aantal ophalen:
1. Selecteer de **aantal Bytes** en/of **aantal** metrische type met **Gem** als de aggregatie. 
2. Voer een van de volgende bewerkingen uit:
   * Een filter toepassen op een specifiek front-end-IP, front-endpoort, back-end-IP- of back-end-poort.
   * Algemene statistieken voor de load balancer-bron zonder filters niet ophalen.

![Aantal bytes](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Afbeelding: Aantal bytes Load Balancer*

#### <a name = "vipavailabilityandhealthprobes"></a>Hoe ik mijn load balancer-implementatie onderzoeken?

U kunt met behulp van een combinatie van de VIP-beschikbaarheid en health test metrische gegevens op een afzonderlijke grafiek waar u zoekt u het probleem en los het probleem identificeren. U kunt krijgen zekerheid dat Azure correct werkt en gebruiken van deze kennis afdoende bepalen dat de configuratie of de toepassing de hoofdoorzaak is.

U kunt health test metrische gegevens gebruiken om te begrijpen hoe Azure weergaven voor de status van uw implementatie aan de hand van de configuratie die u hebt opgegeven. Kijken naar statuscontroles is altijd de eerste stap in de bewaking of een oorzaak te bepalen.

U kunt gaan een stapje verder en VIP beschikbaarheid metrische gegevens voor meer inzicht krijgen in hoe Azure weergaven voor de status van de onderliggende gegevens vlak die verantwoordelijk is voor uw specifieke implementatie. Als u beide metrische gegevens combineert, kunt u vaststellen waar de fout kan zijn, zoals geïllustreerd in dit voorbeeld:

![VIP diagnostische gegevens](./media/load-balancer-standard-diagnostics/LBMetrics-DIPnVIPAvailability.png)

*Afbeelding: Combineren DIP en VIP beschikbaarheid metrische gegevens*

De grafiek bevat de volgende informatie:
- De infrastructuur zelf in orde is, de infrastructuur van uw virtuele machines host bereikbaar is en meer dan één virtuele machine in de back-end is geplaatst. Deze informatie wordt aangegeven door de blauwe tracering voor VIP-beschikbaarheid, 100 procent. 
- De status van test (DIP beschikbaarheid) is echter op 0 procent aan het begin van de grafiek, zoals aangegeven door de oranje tracering. Het omcirkeld gebied in groen licht waarin de status (DIP beschikbaarheid) is geworden orde is, en op welk van de klant implementatie moment kon worden nieuwe stromen accepteren.

De grafiek kan klanten oplossen van problemen met de implementatie op hun eigen zonder dat kan worden geraden of vraag ondersteuning of andere problemen optreden. De service is niet beschikbaar omdat statuscontroles zijn mislukt vanwege een onjuiste configuratie of een mislukte toepassing.

### <a name = "Limitations"></a>Beperkingen

Beschikbaarheid van de VIP is momenteel alleen beschikbaar voor openbare-front-ends.

## <a name = "ResourceHealth"></a>De resourcestatus

Health-status voor de resources van de standaard Load Balancer is beschikbaar gemaakt via de bestaande **resourcestatus** onder **Monitor > servicestatus**.

>[!NOTE]
>De resourcestatus voor Load Balancer is momenteel beschikbaar is voor alleen openbare configuratie van standaard Load Balancer. Interne load balancer resources of de basis-SKU's van een Load Balancer resources zichtbaar resourcestatus niet.

De status van uw openbare standaard Load Balancer-resources weergeven:
1. Selecteer **Monitor** > **Health Service**.

   ![Pagina-monitor](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Afbeelding: De status van de Service-koppeling op Azure-Monitor*

2. Selecteer **resourcestatus**, en zorg ervoor dat **abonnements-ID** en **resourcetype = Load Balancer** zijn geselecteerd.

   ![De resourcestatus](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Afbeelding: Selecteer een resource voor status weergeven*

3. Selecteer de Load Balancer-resource om de historische status weer te geven in de lijst.

    ![Load Balancer gezondheidsstatus](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Afbeelding: Load Balancer resourceweergave health*
 
De statussen van de resource-status en de bijbehorende beschrijvingen worden in de volgende tabel vermeld: 

| De resourcestatus | Beschrijving |
| --- | --- |
| Beschikbaar | Uw openbare standaard load balancer-resource is in orde en beschikbaar. |
| Niet beschikbaar | Uw openbare standaard load balancer-resource is niet in orde. Diagnose van de status door te selecteren **Azure Monitor** > **metrische gegevens**.<br>(*Niet beschikbaar* status kan ook betekenen dat de resource niet is verbonden met uw openbare standaard load balancer.) |
| Onbekend | De resourcestatus voor uw openbare standaard load balancer-resource is nog niet bijgewerkt.<br>(*Onbekende* status kan ook betekenen dat de resource niet is verbonden met uw openbare standaard load balancer.)  |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Standard Load Balancer](load-balancer-standard-overview.md).
- Meer informatie over uw [Load balancer uitgaande verbinding](https://aka.ms/lboutbound).


