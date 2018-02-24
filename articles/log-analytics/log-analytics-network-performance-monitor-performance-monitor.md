---
title: Performance Monitor functie in Prestatiemeter netwerk oplossing in Azure Log Analytics | Microsoft Docs
description: De mogelijkheid van de Prestatiemeter in Prestatiemeter netwerk kunt u netwerkverbinding controleren op verschillende momenten in uw netwerk, zoals een cloudimplementaties en lokale locaties, meerdere datacenters en filialen bedrijfskritieke meerdere lagen toepassingen/micro-services.
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: a90ab3bc857b704d9d94daf96d17611844abb1a6
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="network-performance-monitor-solution---performance-monitoring"></a>Netwerk-oplossing voor Prestatiemeter - bewaking van toepassingsprestaties

De mogelijkheid van de Prestatiemeter in [netwerk Prestatiemeter](log-analytics-network-performance-monitor.md) helpt u bij het controleren van netwerkverbinding tussen de verschillende beheerpunten in uw netwerk, zoals cloudimplementaties en lokale locaties, meerdere datacenters en filialen, missie kritieke meerdere lagen toepassingen/micro-services. Met Prestatiemeter, kunt u netwerkproblemen detecteren voordat uw gebruikers klagen. Belangrijkste voordelen zijn: 

- Bewaken en netwerklatentie over verschillende subnetten en waarschuwingen instellen 
- Bewaken alle paden (inclusief redundante paden) op het netwerk 
- Problemen met tijdelijke & punt in tijd netwerk, die moeilijk te repliceren 
- Het specifieke segment in het netwerk, die verantwoordelijk is voor de prestaties verslechteren bepalen 
- De status van het netwerk, zonder de noodzaak van SNMP 


![Netwerkprestatiemeter](media/log-analytics-network-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Configuratie
Open de configuratie voor netwerk-Prestatiemeter te openen de [netwerk Prestatiemeter oplossing](log-analytics-network-performance-monitor.md) en klik op de **configureren** knop.

![Prestatiemeter netwerk configureren](media/log-analytics-network-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Nieuwe netwerken maken

Een netwerk in NPM is een logische container voor subnetten. Het helpt u bij de bewaking van uw netwerkinfrastructuur naar wens bewaking organiseren. U kunt een netwerk maken met een beschrijvende naam en subnetten volgens uw bedrijfslogica aan toe te voegen. Bijvoorbeeld, u kunt een netwerk met de naam Londen maken en toevoegen van alle subnetten in uw datacenter Londen, of een netwerk met de naam *ContosoFrontEnd* en alle subnetten voor de front-end van uw app met de naam Contoso met dit netwerk toe te voegen. De oplossing maakt automatisch een standaardnetwerk waarin alle subnetten in uw omgeving zijn gedetecteerd. Wanneer u een netwerk maakt, toevoegen van een subnet met en dat subnet uit de standaardnetwerk is verwijderd. Als u een netwerk verwijdert, worden automatisch alle bijbehorende subnetten geretourneerd met het standaard-netwerk. Dus fungeert het standaardnetwerk als een container voor alle subnetten die niet zijn opgenomen in een door de gebruiker gedefinieerde netwerk. U niet kunt bewerken of verwijderen van het standaard-netwerk. Het blijft altijd in het systeem. U kunt echter zo veel aangepaste netwerken behoefte maken. In de meeste gevallen de subnetten in uw organisatie worden gerangschikt in meer dan één netwerk en u een of meer netwerken uw subnetten voor bedrijfslogica op de groep moet maken.

Maken van een nieuw netwerk:


1. Klik op de **tabblad netwerken**.
1. Klik op **toevoegen netwerk** en typ de netwerknaam en beschrijving. 
2. Selecteer een of meer subnetten en klik vervolgens op **toevoegen**. 
3. Klik op **opslaan** aan de configuratie op te slaan. 


### <a name="create-monitoring-rules"></a>Bewaking regels maken 

Prestatiemeter genereert health gebeurtenissen als de drempelwaarde van de prestaties van netwerkverbindingen tussen twee subnetwerken of tussen twee netwerken is geschonden. Deze drempels automatisch kunnen worden geleerd door het systeem of u kunt aangepaste drempelwaarden. Het systeem maakt automatisch een standaardregel waarmee een statusgebeurtenis wanneer schendingen verlies of latentie tussen elk paar netwerk/subnetwerk koppelingen genereert de drempelwaarde system geleerd. Dit helpt de oplossing voor uw netwerkinfrastructuur bewaken totdat u alle bewakingsregels expliciet dat nog niet hebt gemaakt. Als de **standaardregel** is ingeschakeld, verzenden alle knooppunten synthetische transacties voor alle andere knooppunten die u hebt ingeschakeld voor bewaking. De standaardregel is handig voor kleine netwerken, bijvoorbeeld in een scenario waarbij u een klein aantal servers met een microservice hebt en u wilt ervoor zorgen dat alle servers verbonden met elkaar zijn. 

>[!NOTE]
> Het is raadzaam dat u uitschakelen de **standaardregel** en aangepaste regels voor controle, vooral bij grote netwerken als u van een groot aantal knooppunten voor bewaking gebruikmaakt te maken. Dit vermindert het verkeer dat is gegenereerd door de oplossing en help ordenen van de bewaking van uw netwerk. 

Bewaking regels volgens uw bedrijfslogica maken. Bijvoorbeeld, als u bewaken van de prestaties van de netwerkverbinding van twee office sites headquarter wilt, vervolgens groep alle subnetten in office site1 in netwerk O1, alle subnetten in office site2 in netwerk O2 en alle subnetten in de headquarter in netwerk H. Bewaking van regels-een tussen O1 en H en de andere tussen O2 en H. 2 maken 

Aangepaste bewaking regels maken:

1. Klik op **regel toevoegen** in de **Monitor** tabblad en voer de naam en beschrijving. 
2. Het paar van netwerk- of subnetwerk koppelingen om te controleren in de lijst selecteren. 
3. Selecteert u eerst het netwerk waarin de eerste subnetwerk (s) van belang is opgenomen in de vervolgkeuzelijst van het netwerk, en selecteer vervolgens de subnetwerk (s) uit de bijbehorende subnetwerk vervolgkeuzelijst. Selecteer **alle subnetwerken** als u wilt bewaken van de subnetwerken in een netwerkkoppeling. Selecteer op dezelfde manier de andere subnetwerk (s) van belang. En klikt u op **Voeg uitzondering** moeten worden uitgesloten van bewaking voor bepaalde subnetwerkkoppelingen uit de selectie die u hebt aangebracht. 
4. Kiezen tussen ICMP- en TCP-protocollen voor het uitvoeren van de synthetische transacties. 
5. Als u niet wilt maken van de gezondheid van gebeurtenissen voor de items die u hebt geselecteerd, klikt u vervolgens wissen **inschakelen statuscontrole op de koppelingen die door deze regel wordt gedekt**. 
6. Kies de bewaking van de voorwaarden. U kunt aangepaste drempelwaarden instellen voor het genereren van gebeurtenis health door drempelwaarden in te voeren. Wanneer de waarde van de voorwaarde hoger dan de geselecteerde drempelwaarde voor het geselecteerde netwerk/subnetwerk paar is, wordt een health-gebeurtenis gegenereerd. 
7. Klik op **opslaan** aan de configuratie op te slaan. 

Nadat u een regel voor bewaking hebt opgeslagen, kunt u die regel integreren met beheer van waarschuwingen door te klikken op **waarschuwingen maken**. Een waarschuwingsregel wordt automatisch gemaakt met de zoekopdracht en andere vereiste parameters automatisch ingevuld. Een waarschuwingsregel kunt u waarschuwingen op basis van e-mailbericht naast de bestaande waarschuwingen binnen NPM ontvangen. Waarschuwingen kunnen ook corrigerende acties met runbooks activeren of ze kunnen worden geïntegreerd met bestaande beheersystemen voor service met behulp van webhooks. U kunt klikken op **waarschuwing beheren** bewerken van de instellingen voor waarschuwingen. 

U kunt nu meer Prestatiemeter regels maken of verplaatsen naar het dashboard van de oplossing te starten met de mogelijkheid 

### <a name="choose-the-protocol"></a>Kies het protocol

Netwerk Performance Monitor (NPM) maakt gebruik van synthetische transacties voor het berekenen van de prestatiewaarden netwerk zoals pakket verlies en koppeling latentie. Overweeg om deze beter te begrijpen, een NPM-agent is verbonden met één end van een netwerkverbinding. Deze agent NPM verzendt testpakketten naar een tweede NPM-agent is verbonden met een andere einde van het netwerk. De tweede agent antwoorden met response-pakketten. Dit proces wordt een paar keer herhaald. De eerste NPM agent evalueert koppeling latentie door het meten van het aantal antwoorden en de tijd die nodig is voor het ontvangen van elk antwoord en verloren pakketten. 

De indeling, de grootte en de volgorde van deze pakketten wordt bepaald door het protocol dat u bij het maken van regels voor controle. Op basis van het protocol van de pakketten, de tussenliggende netwerkapparaten (routers, switches, enzovoort) deze pakketten anders kan verwerken. Hierdoor kan uw keuze protocol is van invloed op de nauwkeurigheid van de resultaten. En uw keuze protocol bepaalt ook of u een handmatige stappen uitvoeren moet nadat u de NPM-oplossing implementeert. 

NPM biedt u de keuze tussen ICMP- en TCP-protocollen voor het uitvoeren van de synthetische transacties. Als u ICMP kiezen wanneer u een regel synthetische transactie maken, gebruiken de NPM-agents van ICMP-ECHO-berichten voor het berekenen van de netwerklatentie en pakketverlies. ICMP ECHO maakt gebruik van hetzelfde bericht dat door de conventionele opdracht ping wordt verzonden. Wanneer u TCP als protocol gebruikt, verzenden NPM agents TCP SYN-pakket via het netwerk. Dit wordt gevolgd door een TCP-handshake is voltooid en vervolgens verwijdert de verbinding met de eerste pakketten. 

Houd rekening met de volgende informatie voordat u een te gebruiken protocol kiezen: 

**Detectie van meerdere netwerkroutes.**  TCP is nauwkeuriger wanneer meerdere routes detecteren en moet minder agents in elk subnet. Een of twee agents die met TCP kunnen bijvoorbeeld alle redundante paden tussen subnetten te detecteren. U moet echter verschillende agents met ICMP vergelijkbare resultaten bereiken. Via ICMP, hebt u een aantal routes tussen twee subnetten moet u meer dan 5N agents in het subnet van een bron- of doelserver. 

**De nauwkeurigheid van de resultaten.** Routers en switches vaak lagere prioriteit toewijzen aan ICMP ECHO-pakketten in vergelijking met TCP-pakketten. In bepaalde situaties wanneer netwerkapparaten zwaar worden geladen, weerspiegelt de gegevens die zijn verkregen door het TCP beter en de netwerklatentie opgetreden door toepassingen. Dit gebeurt omdat het meeste verkeer toepassing loopt via TCP. In dergelijke gevallen biedt ICMP minder nauwkeurige resultaten vergeleken met TCP. 

**Firewall-configuratie.** TCP-protocol wordt vereist dat de TCP-pakketten worden verzonden naar een doelpoort. De standaardpoort gebruikt door de NPM-agents is 8084, maar u dit wijzigen kunt bij het configureren van agents. Daarom moet u ervoor zorgen dat uw netwerkfirewalls of het NSG-regels (in Azure) bieden de mogelijkheid verkeer op de poort. U moet ook om ervoor te zorgen dat de lokale firewall op de computers waarop agents zijn geïnstalleerd, is geconfigureerd om verkeer op deze poort te staan. PowerShell-scripts kunt u firewallregels configureren op uw computers met Windows, maar u moet de firewall van uw netwerk handmatig configureren. ICMP werkt echter niet met behulp van poort. ICMP-verkeer is in de meeste zakelijke scenario's, kunt u gebruikmaken van netwerk diagnostische hulpprogramma's zoals het hulpprogramma Ping toegestaan via de firewalls. Dus als u één machine van een andere pingen kunt, u het ICMP-protocol gebruiken kunt zonder het handmatig configureren van firewalls. 

>[!NOTE] 
> Sommige firewalls blokkeren ICMP, wat tot herverzending leiden kan, wat resulteert in een groot aantal gebeurtenissen in uw systeem security information en event management. Zorg ervoor dat het protocol dat u kiest niet wordt geblokkeerd door een firewall/NSG, anders NPM is niet tot het netwerksegment bewaken. Daarom raden wij u TCP gebruiken voor bewaking. U moet ICMP gebruiken in scenario's waarin u ze niet om TCP te gebruiken, zoals wanneer: 
>
> - U gebruikt Windows client-knooppunten, omdat de onbewerkte sockets TCP zijn niet toegestaan in Windows-client
> - Uw netwerk firewall/NSG blokken TCP
> - Het wijzigen van het protocol 

Als u kiest voor het gebruik van ICMP tijdens de implementatie, kunt u overschakelen naar TCP op elk gewenst moment door de standaardinstellingen voor bewaking van de regel te bewerken:

1. Navigeer naar **netwerkprestaties** > **Monitor** > **configureren**   >  **Monitor** en klik vervolgens op **standaardregel**. 
2. Schuif naar de **Protocol** sectie en selecteer het protocol dat u wilt gebruiken. 
3. Klik op **opslaan** de instelling wilt toepassen. 

Zelfs als de standaardregel voor een specifieke protocol wordt gebruikt, kunt u nieuwe regels maken met een ander protocol. U kunt ook een combinatie van regels waar sommige regels ICMP gebruiken en de andere met TCP maken. 

## <a name="walkthrough"></a>Walkthrough 

Nu dat u hebt gelezen over Prestatiemeter, bekijken we een eenvoudige onderzoek naar de hoofdoorzaak van een health-gebeurtenis.  

Op het dashboard van de oplossing merkt u dat er een statusgebeurtenis is: een netwerkverbinding beschadigd is. U wilt het probleem te onderzoeken en klik op **netwerk koppelingen bewaakt** tegel.

Op de pagina drilldown u ziet u dat de **DMZ2 DMZ1** netwerkkoppeling niet in orde is. U klikt u op de **subnet koppelingen weergeven** koppeling voor deze netwerkkoppeling. 


De pagina inzoomen toont alle subnetwerkkoppelingen **DMZ2 DMZ1** netwerkkoppeling. U merkt dat voor beide de subnetwerkkoppelingen de latentie is gepasseerd de drempelwaarde voor de netwerkkoppeling slecht maken. U ziet ook de subnetwerkkoppelingen trends latentie. U kunt de Tijdselectie besturingselement in de grafiek te concentreren op de vereiste periode. Hier ziet u de tijd van de dag wanneer latentie de piek heeft bereikt. De logboeken voor deze periode voor het onderzoeken van het probleem kunt u later zoeken. Klik op **knooppuntkoppelingen weergeven** verdere detailanalyse. 
 
 ![Subnetwerkkoppelingen](media/log-analytics-network-performance-monitor/subnetwork-links.png) 

Net als bij de vorige pagina, Inzoomen voor de koppeling bepaalde subnetwerk worden op deze pagina omlaag bijbehorende knooppuntkoppelingen samenstellende. U kunt vergelijkbare bewerkingen uitvoeren hier net als bij de vorige stap. Klik op **weergave topologie** om weer te geven van de topologie tussen de twee knooppunten. 
 
 ![Knooppuntkoppelingen](media/log-analytics-network-performance-monitor/node-links.png) 

Alle paden tussen de twee geselecteerde knooppunten worden getekend in de topologiekaart. U kunt de topologie hop door hop van routes tussen twee knooppunten op de topologiekaart visualiseren. Dit biedt u een duidelijk beeld van hoeveel routes bestaan tussen de twee knooppunten en wat de gegevenspakketten duurt paden. Knelpunten in de prestaties zijn in rood gemarkeerd. U kunt een defecte netwerkverbinding of een defecte netwerkapparaat vinden door te kijken rode gekleurde elementen op de topologiekaart. 

 ![Topologie-Dashboard](media/log-analytics-network-performance-monitor/topology-dashboard.png) 

Het verlies, latentie en het aantal hops in elk pad kunnen worden gecontroleerd de **actie** deelvenster. Gebruik de schuifbalk om de details van die niet in orde paden weer te geven. De filters gebruiken om de paden met de slechte hop selecteren zodat de topologie voor alleen de geselecteerde paden wordt getekend. Het muiswiel kunt u de topologiekaart in- of uitzoomen. 

In de onderstaande afbeelding, duidelijk ziet u de hoofdoorzaak van de probleemgebieden naar de specifieke sectie van het netwerk door te kijken naar de paden en hops in rood. De eigenschappen van het knooppunt, met inbegrip van de FQDN-naam, te klikken op een knooppunt in de topologiekaart blijkt en IP-adres. Te klikken op een hop bevat het IP-adres van de hop. 
 
![Topologie-Dashboard](media/log-analytics-network-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Volgende stappen
* [Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde netwerk prestaties gegevensrecords weer te geven.
