---
title: Diagnostische gegevens van Azure Standard Load Balancer | Microsoft Docs
description: Gebruik de beschikbare metrische gegevens en de gezondheid van informatie voor diagnostische gegevens voor Azure Standard Load Balancer.
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
ms.date: 08/27/2018
ms.author: Kumud
ms.openlocfilehash: 43945dc8810151eb701aa9e1aa1be47d4fbb0491
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125641"
---
# <a name="metrics-and-health-diagnostics-for-standard-load-balancer"></a>Metrische gegevens en de gezondheid van diagnostische gegevens voor de standaardversie van Load Balancer

Azure Standard Load Balancer biedt uw resources met de volgende diagnostische mogelijkheden:
* **Multi-dimensionale metrische gegevens**: nieuwe multi-dimensionale diagnostische mogelijkheden biedt voor openbare en interne load balancer-configuraties. U kunt controleren, beheren en oplossen van uw resources voor load balancer.

* **Resourcestatus**: de Load Balancer-pagina in de Azure portal en de Resource Health-pagina (onder Monitor) tonen de resourcestatus sectie voor de openbare load balancer-configuratie van de standaardversie van Load Balancer.

Dit artikel bevat een kort overzicht van deze mogelijkheden en manieren om ze te gebruiken voor de standaardversie van Load Balancer biedt.

## <a name = "MultiDimensionalMetrics"></a>Multi-dimensionale metrische gegevens

Azure Load Balancer biedt nieuwe multi-dimensionale metrische gegevens via de nieuwe Azure-statistieken (preview) in Azure portal en Hiermee kunt u realtime diagnostische inzicht verkrijgen in uw load balancer-resources. 

De verschillende configuraties voor de standaardversie van Load Balancer bieden de volgende metrische gegevens:

| Gegevens | Resourcetype | Beschrijving | Aanbevolen aggregatie |
| --- | --- | --- | --- |
| VIP-beschikbaarheid (beschikbaarheid van gegevens-pad) | Openbare load balancer | Standaardversie van Load Balancer oefeningen continu het gegevenspad van binnen een regio voor de load balancer front-end, helemaal tot aan de SDN-stack die ondersteuning biedt voor uw virtuele machine. Als in orde exemplaren blijven, volgt de meting hetzelfde pad als verkeer met netwerktaakverdeling van uw toepassing. Het gegevenspad die gebruikmaken van uw klanten wordt ook gevalideerd. De meting is zichtbaar voor uw toepassing en niet van invloed op andere bewerkingen.| Gemiddeld |
| Beschikbaarheid van DIP (status health test) |  Openbare en interne load balancer | Standard Load Balancer maakt gebruik van een gedistribueerde statustesten service die de status van het toepassingseindpunt van uw op basis van uw configuratie-instellingen controleert. Met deze metriek biedt een aggregatie of per eindpunt gefilterde weergave van elk eindpunt exemplaar in de load balancer-groep. U kunt zien hoe de Load Balancer de status van uw toepassing bekijkt zoals aangegeven door de configuratie van de health test. |  Gemiddeld |
| SYN (synchroniseren) pakketten |  Openbare load balancer | Standard Load Balancer niet beëindigen Transmission Control Protocol (TCP)-verbindingen of ermee TCP of UDP-pakketten stromen. Stromen en hun aantal-handshakes zijn altijd tussen de bron en het VM-exemplaar. Als u wilt uw TCP-protocol-scenario's op te lossen, kunt u het gebruik van SYN pakketten items om te begrijpen hoeveel TCP-verbinding pogingen worden uitgevoerd. De metrische gegevens rapporteert het aantal TCP SYN-pakketten die zijn ontvangen.| Gemiddeld |
| SNAT-verbindingen |  Openbare Load Balancer |Standard Load Balancer rapporteert het aantal uitgaande stromen die zijn masqueraded aan de front-end van het openbare IP-adres. Adres bron-NAT (SNAT) netwerkpoorten vormen een onuitputtelijk resource. Met deze metriek krijgt een indicatie van de mate waarin uw toepassing wordt afhankelijk zijn van SNAT voor uitgaande stromen. Tellers voor geslaagde en mislukte uitgaande SNAT-stromen worden gerapporteerd en kunnen worden gebruikt om problemen op te begrijpen van de status van uw uitgaande stromen.| Gemiddeld |
| Byte-prestatiemeteritems |  Openbare en interne load balancer | Standard Load Balancer rapporten de gegevens verwerkte per front-end.| Gemiddeld |
| Pakket-prestatiemeteritems |  Openbare en interne load balancer | Standard Load Balancer rapporteert de pakketten dat per front-end wordt verwerkt.| Gemiddeld |

### <a name="view-your-load-balancer-metrics-in-the-azure-portal"></a>Uw load balancer metrische gegevens weergeven in Azure portal

De Azure-portal wordt aangegeven dat de load balancer metrische gegevens via de pagina met metrische gegevens (preview), die beschikbaar voor zowel de load balancer resource-pagina voor een bepaalde resource en de pagina Azure Monitor is. 

De metrische gegevens voor de standaardversie van Load Balancer-resources weergeven:
1. Ga naar de pagina metrische gegevens (preview) en een van de volgende handelingen uit:
   * Selecteer op de pagina van de resource in de load balancer, het type van metrische gegevens in de vervolgkeuzelijst.
   * Selecteer op de pagina Azure Monitor, de load balancer-resource.
2. Stel de juiste samenvoegingstype.
3. (Optioneel) Configureer de vereiste filteren en groeperen.

![Voorbeeld van metrische gegevens voor de standaardversie van Load Balancer](./media/load-balancer-standard-diagnostics/LBMetrics1.png)

*Afbeelding: De beschikbaarheid van DIP en health test status van metrische gegevens voor de standaardversie van Load Balancer*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>Multi-dimensionale metrische gegevens via een programma via API's ophalen

Zie voor instructies voor het ophalen van multi-dimensionale metrische definities en waarden API [REST-API voor Azure-bewaking scenario](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api).

### <a name = "DiagnosticScenarios"></a>Algemene scenario's voor diagnostische en aanbevolen weergaven

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>Het gegevenspad actief en beschikbaar is voor de load balancer-VIP?

De beschikbaarheid van VIP metriek beschrijving van de status van het gegevenspad binnen de regio voor de compute-host waar uw VM's zich bevinden. De metrische gegevens is een weerspiegeling van de status van de Azure-infrastructuur. U kunt de metrische gegevens om te gebruiken:
- De externe beschikbaarheid van uw service bewaken
- Dieper graven en te begrijpen of het platform waarop de service is geïmplementeerd in orde is of of de Gast-OS of het exemplaar van toepassing in orde is.
- Isoleren of een gebeurtenis is gekoppeld aan uw service of het vlak van onderliggende gegevens. Verwar deze metrische gegevens met de status van test ('beschikbaarheid van DIP') niet.

De VIP-beschikbaarheid voor de standaardversie van Load Balancer-resources ophalen:
1. Zorg ervoor dat de juiste load balancer-resource is geselecteerd. 
2. In de **Metric** vervolgkeuzelijst, selecteer **VIP beschikbaarheid**. 
3. In de **aggregatie** vervolgkeuzelijst, selecteer **Avg**. 
4. Bovendien een filter op de VIP-adres of de VIP-poort als de dimensie met de vereiste front-end-IP-adres of de front-endpoort toe te voegen en ze vervolgens te groeperen door de geselecteerde dimensie.

![Scannen van VIP](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Afbeelding: Scannen van details van Load Balancer-VIP*

De metrische gegevens wordt gegenereerd door een actieve, in-band-meting. Een testinterval service binnen de regio is afkomstig van verkeer voor de meting. De service wordt geactiveerd zodra u een implementatie met een openbare-front-end maken en gaat door totdat u de front-end verwijdert. 

>[!NOTE]
>Interne front-ends worden niet ondersteund op dit moment. 

Een pakket die overeenkomt met de webfront-end van uw implementatie en de regel wordt periodiek gegenereerd. Deze passeert de regio van de bron naar de host waar een virtuele machine in de back-end-adrespool zich bevindt. De load balancer-infrastructuur voert dezelfde load balancing en vertaling bewerkingen als voor al het andere verkeer. Deze test wordt in-band op uw eindpunt met load balancing. Nadat de test wordt ontvangen op de host compute, waar een gezonde virtuele machine in de back-end-adrespool zich bevindt, genereert de compute-host een antwoord naar de testinterval-service. Dit verkeer niet wordt weergegeven in uw virtuele machine.

Beschikbaarheid van VIP is mislukt voor de volgende redenen:
- Uw implementatie heeft geen goede virtuele machines blijven in de back-endpool. 
- Een infrastructuur storing is opgetreden.

Voor diagnostische doeleinden, kunt u de [VIP beschikbaarheid metrische gegevens, samen met de status van de test](#vipavailabilityandhealthprobes).

Gebruik **gemiddelde** als de aggregatie voor de meeste scenario's.

#### <a name="are-the-back-end-instances-for-my-vip-responding-to-probes"></a>Zijn de back-end-exemplaren voor mijn VIP op tests reageert?

De metriek health test status wordt de status van de implementatie van uw toepassing beschreven, zoals is geconfigureerd door u bij het configureren van de statustest van de load balancer. De load balancer maakt gebruik van de status van de statustest om te bepalen waar voor het verzenden van nieuwe stromen. Statuscontroles afkomstig zijn van het adres van een Azure-infrastructuur en zijn zichtbaar in het gastbesturingssysteem van de virtuele machine.

De beschikbaarheid van DIP voor de standaardversie van Load Balancer-resources ophalen:
1. Selecteer de **beschikbaarheid van DIP** metrische met **Avg** samenvoegingstype. 
2. Een filter toepassen op de vereiste IP-adres of -poort (of beide).

![Beschikbaarheid van DIP](./media/load-balancer-standard-diagnostics/LBMetrics-DIPAvailability.png)

*Afbeelding: Beschikbaarheid van de Load Balancer-VIP*

Statuscontroles mislukken om de volgende redenen:
- Configureren van een statustest aan een poort die wordt niet geluisterd of reageert niet of het juiste protocol wordt gebruikt. Als uw service met behulp van directe server retourneren (DSR of zwevend IP) worden de regels, zorg ervoor dat de service op IP-adres van de IP-configuratie van de NIC luistert en niet alleen op de loopback die geconfigureerd met de front-end-IP-adres.
- De test is niet toegestaan door de Netwerkbeveiligingsgroep, firewall van de virtuele machine Gast-besturingssysteem of de toepassing laag filters.

Gebruik **gemiddelde** als de aggregatie voor de meeste scenario's.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>Hoe kan ik mijn Uitgaande verbindingsstatistieken controleren? 

De metriek SNAT-verbindingen wordt het volume van geslaagde en mislukte verbindingen voor beschreven [uitgaande stromen](https://aka.ms/lboutbound).

Een mislukte verbindingen volume groter is dan nul geeft aan dat de poortuitputting SNAT. U moet verder onderzoeken om te bepalen wat de oorzaak van deze fouten. Poortuitputting SNAT manifesten als een fout opgetreden bij het tot stand brengen een [uitgaande stroom](https://aka.ms/lboutbound). Lees het artikel over uitgaande verbindingen voor informatie over de scenario's en methoden op het werk, en voor meer informatie over het beperken en ontwerpen ter voorkoming van SNAT poortuitputting. 

Verbindingsstatistieken SNAT ophalen:
1. Selecteer **SNAT-verbindingen** type metrische gegevens en **som** als aggregatie. 
2. Groeperen op **verbindingsstatus** voor geslaagde en mislukte SNAT verbinding aantallen die worden weergegeven door andere regels. 

![SNAT verbinding](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Afbeelding: Aantal Load Balancer SNAT-verbindingen*


#### <a name="how-do-i-check-inboundoutbound-connection-attempts-for-my-service"></a>Hoe kan ik voor mijn service binnenkomend en uitgaand verbindingspogingen controleren?

Een SYN pakketten metrische waarde beschrijving van het volume van TCP SYN-pakketten die zijn ontvangen of zijn verzonden (voor [uitgaande stromen](https://aka.ms/lboutbound)) die zijn gekoppeld aan een specifieke front-end. U kunt met deze metriek gebruiken om te begrijpen van TCP-verbindingspogingen met uw service.

Gebruik **totale** als de aggregatie voor de meeste scenario's.

![SYN verbinding](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Afbeelding: Load Balancer SYN tellen*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>Hoe controleer ik mijn gebruik van netwerkbandbreedte? 

De bytes en de pakket-prestatiemeteritems metriek wordt het aantal bytes en pakketten die worden verzonden of ontvangen van de service op basis van de per-front-end beschreven.

Gebruik **totale** als de aggregatie voor de meeste scenario's.

Byte of pakket count-statistieken ophalen:
1. Selecteer de **Bytes-telling** en/of **pakket aantal** type metrische gegevens, met **Avg** als de aggregatie. 
2. Voer een van de volgende bewerkingen uit:
   * Een filter toepassen op een specifieke front-end-IP, front-endpoort, back-end-IP- of back-endpoort.
   * Aan de algemene statistieken voor de load balancer-resource zonder een filter.

![Aantal bytes](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Afbeelding: Load Balancer bytes-telling*

#### <a name = "vipavailabilityandhealthprobes"></a>Hoe ik mijn load balancer-implementatie vast?

Met behulp van een combinatie van de VIP-beschikbaarheid en health test metrische gegevens op een afzonderlijke grafiek kunt u identificeren waar u zoeken naar het probleem en het probleem is opgelost. U kunt krijgen van de zekerheid dat Azure correct werkt en deze informatie gebruiken om afdoende te bepalen dat de configuratie of de toepassing de hoofdoorzaak is.

U kunt health test metrische gegevens gebruiken om te begrijpen hoe Azure bekijkt de status van uw implementatie aan de hand van de configuratie die u hebt opgegeven. Statuscontroles kijken is altijd een belangrijke eerste stap in de controleren of de oorzaak te bepalen.

U kunt zet een stap verder en VIP-metrische gegevens over beschikbaarheid gebruiken meer inzicht krijgen in hoe Azure de status van de onderliggende gegevensvlak dat verantwoordelijk is voor uw specifieke implementatie bekijkt. Wanneer u beide metrische gegevens combineert, kunt u isoleren waar de fout kan zijn, zoals wordt geïllustreerd in het volgende voorbeeld:

![Diagnostische gegevens van VIP](./media/load-balancer-standard-diagnostics/LBMetrics-DIPnVIPAvailability.png)

*Afbeelding: Combineren DIP en VIP metrische gegevens over beschikbaarheid*

De grafiek bevat de volgende informatie:
- De infrastructuur zelf in orde is, de infrastructuur die als host fungeert voor uw virtuele machines is bereikbaar is en meer dan één virtuele machine werd geplaatst in de back-end. Deze informatie wordt aangegeven door de blauwe tracering voor beschikbaarheid van VIP 100 procent is. 
- De integriteitsstatus test (beschikbaarheid van DIP) is echter op 0 procent aan het begin van de grafiek, zoals aangegeven door de oranje tracering. Het omcirkeld gebied in groen hoogtepunten waar de status (beschikbaarheid van DIP) is geworden in orde is, en op welk moment van implementatie van de klant kan accepteren van nieuwe stromen.

De grafiek kan klanten om op te lossen van de implementatie van hun eigen zonder te achterhalen of vraag ondersteuning of andere problemen optreden. De service is niet beschikbaar omdat statuscontroles zijn mislukt vanwege een onjuiste configuratie of een mislukte toepassing.

### <a name = "Limitations"></a>Beperkingen

Beschikbaarheid van de VIP is momenteel alleen beschikbaar voor openbare-front-ends.

## <a name = "ResourceHealth"></a>Integriteitsstatus van de resource

De status voor de standaardversie van Load Balancer-resources is beschikbaar via de bestaande **resourcestatus** onder **Monitor > servicestatus**.

>[!NOTE]
>Integriteitsstatus van de resource voor Load Balancer is momenteel beschikbaar voor alleen openbare configuratie van Standard Load Balancer. Interne load balancer resources of Basic-SKU's van een Load Balancer resources geven geen resource health.

De status van uw openbare Standard Load Balancer-resources weergeven:
1. Selecteer **Monitor** > **servicestatus**.

   ![Pagina-monitor](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Afbeelding: De koppeling voor servicestatus van Azure Monitor*

2. Selecteer **Resource Health**, en zorg ervoor dat **abonnements-ID** en **resourcetype = Load Balancer** zijn geselecteerd.

   ![Integriteitsstatus van de resource](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Afbeelding: Resource voor de statusweergave selecteren*

3. Selecteer in de lijst met de Load Balancer-resource om de historische status weer te geven.

    ![Status van Load Balancer health](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Afbeelding: Load Balancer resource health weergeven*
 
De verschillende statussen van de resource-status en de bijbehorende beschrijvingen worden vermeld in de volgende tabel: 

| Integriteitsstatus van de resource | Beschrijving |
| --- | --- |
| Beschikbaar | Uw openbare standard load balancer-resource is in orde en beschikbaar is. |
| Niet beschikbaar | Uw openbare standard load balancer-resource is niet in orde. De status vaststellen door te selecteren **Azure Monitor** > **metrische gegevens**.<br>(*Niet beschikbaar* status kan ook betekenen dat de resource niet is verbonden met uw openbare standard load balancer.) |
| Onbekend | Integriteitsstatus van de resource voor uw openbare standard load balancer-resource is nog niet bijgewerkt.<br>(*Onbekende* status kan ook betekenen dat de resource niet is verbonden met uw openbare standard load balancer.)  |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Standard Load Balancer](load-balancer-standard-overview.md).
- Meer informatie over uw [Load balancer uitgaande connectiviteit](https://aka.ms/lboutbound).
- Meer informatie over de [metrische gegevens van Azure Monitor REST-API](https://docs.microsoft.com/rest/api/monitor/metrics/).


