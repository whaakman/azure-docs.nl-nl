---
title: Performance Monitor functie in Prestatiemeter netwerk oplossing in Azure Log Analytics | Microsoft Docs
description: De mogelijkheid van de Prestatiemeter in Prestatiemeter netwerk kunt u verbinding met het netwerk te controleren op verschillende momenten in uw netwerk. U kunt bewaken cloudimplementaties en lokale locaties, meerdere datacenters en filialen en bedrijfskritieke met meerdere lagen toepassingen of microservices.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 65497548d0b8066627be25520c28d39491918d09
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
ms.locfileid: "30241442"
---
# <a name="network-performance-monitor-solution-performance-monitoring"></a>Netwerk-Prestatiemeter-oplossing: prestatiebewaking

De mogelijkheid van de Prestatiemeter in [netwerk Prestatiemeter](log-analytics-network-performance-monitor.md) netwerkverbinding helpt u bij het controleren op verschillende momenten in uw netwerk. U kunt bewaken cloudimplementaties en lokale locaties, meerdere datacenters en filialen en bedrijfskritieke met meerdere lagen toepassingen of microservices. Met Prestatiemeter, kunt u netwerkproblemen detecteren voordat uw gebruikers klagen. Belangrijkste voordelen zijn dat u kunt: 

- Bewaken en netwerklatentie via verschillende subnetten en waarschuwingen instellen.
- Bewaken alle paden (inclusief redundante paden) op het netwerk.
- Problemen met tijdelijke en punt in tijd netwerken, die moeilijk te repliceren.
- Bepaal het specifieke segment in het netwerk, die verantwoordelijk is voor de prestaties verslechteren.
- De status van het netwerk, zonder de noodzaak voor SNMP worden bewaakt.


![Netwerkprestatiemeter](media/log-analytics-network-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Configuratie
Open de configuratie voor netwerk-Prestatiemeter te openen de [netwerk Prestatiemeter oplossing](log-analytics-network-performance-monitor.md), en selecteer **configureren**.

![Prestatiemeter netwerk configureren](media/log-analytics-network-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Nieuwe netwerken maken

Een netwerk in Prestatiemeter netwerk is een logische container voor subnetten. Dit helpt u bij de bewaking van de infrastructuur van uw netwerk volgens uw behoeften ordenen. U kunt een netwerk maken met een beschrijvende naam en subnetten volgens uw bedrijfslogica aan toe te voegen. U kunt bijvoorbeeld een netwerk met de naam Londen maken en toevoegen van alle subnetten in uw datacentrum Londen. Kunt u een netwerk met de naam *ContosoFrontEnd* en alle de subnetten met de naam Contoso die de front-end van uw app dienen toe te voegen aan dit netwerk. De oplossing maakt automatisch een standaardnetwerk waarin alle subnetten in uw omgeving zijn gedetecteerd. 

Wanneer u een netwerk maakt, voegt u een subnet toe. Vervolgens wordt dat subnet van het standaardnetwerk verwijderd. Als u een netwerk verwijdert, worden automatisch alle bijbehorende subnetten geretourneerd met het standaardnetwerk. Het standaardnetwerk fungeert als een container voor alle subnetten die niet zijn opgenomen in een door de gebruiker gedefinieerde netwerk. U niet kunt bewerken of verwijderen van het standaardnetwerk. Het blijft altijd in het systeem. U kunt zoveel aangepaste netwerken behoefte maken. In de meeste gevallen worden de subnetten in uw organisatie meer dan één netwerk gerangschikt. Een of meer netwerken uw subnetten voor bedrijfslogica op de groep maken.

Maken van een nieuw netwerk:


1. Selecteer de **netwerken** tabblad.
1. Selecteer **toevoegen netwerk**, en voer vervolgens de netwerknaam en de beschrijving. 
2. Selecteer een of meer subnetten, en selecteer vervolgens **toevoegen**. 
3. Selecteer **opslaan** aan de configuratie op te slaan. 


### <a name="create-monitoring-rules"></a>Bewaking regels maken 

Prestatiemeter genereert health gebeurtenissen als de drempelwaarde van de prestaties van netwerkverbindingen tussen twee subnetwerken of tussen twee netwerken is geschonden. Het systeem kan deze drempels automatisch informatie over. U kunt ook aangepaste drempelwaarden opgeven. Het systeem maakt automatisch een standaardregel waarmee een statusgebeurtenis wanneer schendingen verlies of latentie tussen elk paar netwerk of subnetwerk koppelingen genereert de drempelwaarde system geleerd. Dit proces kunt u de oplossing voor uw netwerkinfrastructuur bewaken totdat u alle bewakingsregels expliciet dat nog niet hebt gemaakt. Als de standaardregel is ingeschakeld, verzenden alle knooppunten synthetische transacties voor alle andere knooppunten die u voor de bewaking ingeschakeld. De standaardregel is handig voor kleine netwerken. Een voorbeeld is een scenario waarbij u een klein aantal servers met een microservice hebt en u wilt ervoor zorgen dat alle servers verbonden met elkaar zijn.

>[!NOTE]
> U wordt aangeraden dat u de standaardregel uitschakelen en aangepaste regels voor controle, vooral bij grote netwerken waar u een groot aantal knooppunten voor het bewaken van gebruiken maken. Aangepaste regels bewaking kan verminderen het verkeer dat is gegenereerd door de oplossing en help ordenen van de bewaking van uw netwerk.

Bewaking regels volgens uw bedrijfslogica maken. Een voorbeeld is als u wilt bewaken van de prestaties van de netwerkverbinding van twee office-sites met hoofdkantoor. Groep alle subnetten in office site1 in netwerk O1. Alle subnetten in office site2 in netwerk O2 groeperen. Ten slotte groep alle subnetten in het hoofdkantoor in netwerk H. maakt twee bewakingsregels--één tussen O1 en H en andere tussen O2 en H. 

Aangepaste bewaking regels maken:

1. Selecteer **regel toevoegen** op de **Monitor** tabblad en voer de naam en beschrijving.
2. Het paar van netwerk- of subnetwerk koppelingen om te controleren in de lijst selecteren. 
3. Selecteer het netwerk met de subnetwerken die u wilt dat uit de vervolgkeuzelijst van het netwerk. Selecteer vervolgens de subnetwerken in de vervolgkeuzelijst bijbehorende subnetwerk. Als u bewaken van de subnetwerken in een netwerkkoppeling wilt, selecteert u **alle subnetwerken**. Op deze manier het andere subnetwerken die u wilt selecteren. Als u wilt uitsluiten van bewaking voor bepaalde subnetwerkkoppelingen van de selecties die u hebt aangebracht, selecteert u **Voeg uitzondering**. 
4. Kiezen tussen ICMP- en TCP-protocollen uit te voeren van synthetische transacties. 
5. Als u niet wilt maken van health-gebeurtenissen voor de items die u hebt geselecteerd, schakel **statuscontrole inschakelen op de koppelingen wordt gedekt door deze regel**. 
6. Kies de bewaking van de voorwaarden. Aangepaste drempelwaarden voor het genereren van de health-gebeurtenis stelt u typen drempelwaarden. Wanneer de waarde van de conditie van de geselecteerde drempelwaarde voor het geselecteerde netwerk of subnetwerk paar overschrijdt, kan een health-gebeurtenis wordt gegenereerd. 
7. Selecteer **opslaan** aan de configuratie op te slaan. 

Nadat u een regel voor bewaking hebt opgeslagen, kunt u die regel integreren met beheer van waarschuwingen door te selecteren **waarschuwingen maken**. Een waarschuwingsregel wordt automatisch gemaakt met de zoekopdracht. Andere vereiste parameters worden automatisch ingevuld. Een waarschuwingsregel kunt u waarschuwingen op basis van e-mail naast de bestaande waarschuwingen in Prestatiemeter netwerk ontvangen. Waarschuwingen ook corrigerende acties met runbooks kunnen activeren of kunnen integreren met bestaande oplossingen voor service-beheer met behulp van webhooks. Selecteer **waarschuwing beheren** bewerken van de instellingen voor waarschuwingen. 

U kunt nu meer Prestatiemeter regels maken of verplaatsen naar het dashboard van de oplossing de mogelijkheid te gebruiken.

### <a name="choose-the-protocol"></a>Kies het protocol

De Prestatiemeter netwerk maakt gebruik van synthetische transacties voor het berekenen van de prestatiewaarden netwerk zoals pakket verlies en koppeling latentie. Om dit concept beter te begrijpen, houd rekening met een netwerk-Prestatiemeter-agent is verbonden met één end van een netwerkverbinding. Dit netwerk Prestatiemeter-agent verzendt testpakketten naar een tweede netwerk Prestatiemeter-agent is verbonden met een andere einde van het netwerk. De tweede agent antwoorden met response-pakketten. Dit proces wordt herhaald een paar keer. De eerste netwerk Prestatiemeter agent evalueert koppeling latentie en verloren pakketten door het meten van het aantal antwoorden en de tijd die elke antwoord ontvangen. 

De indeling, de grootte en de volgorde van deze pakketten wordt bepaald door het protocol dat u bij het maken van regels voor controle. Op basis van het protocol van de pakketten, de tussenliggende netwerkapparaten zoals routers en switches, kunnen deze pakketten verwerken anders. Hierdoor kan uw keuze protocol is van invloed op de nauwkeurigheid van de resultaten. Uw keuze protocol bepaalt ook of u alle handmatige stappen uitvoeren moet nadat u de netwerk-Prestatiemeter-oplossing implementeert. 

Prestaties Netwerkcontrole biedt u de keuze tussen ICMP- en TCP-protocollen voor het uitvoeren van de synthetische transacties. Als u ICMP kiezen wanneer u een regel synthetische transactie maken, gebruiken de netwerk-Prestatiemeter-agents van ICMP-ECHO-berichten voor het berekenen van de netwerklatentie en pakketverlies. ICMP ECHO maakt gebruik van hetzelfde bericht dat door het hulpprogramma ping conventionele wordt verzonden. Wanneer u TCP als protocol gebruikt, verzenden netwerk-Prestatiemeter agents TCP SYN pakketten via het netwerk. Deze stap wordt gevolgd door een TCP-handshake is voltooid en de verbinding is verwijderd met behulp van de eerste pakketten. 

Houd rekening met de volgende informatie voordat u een protocol kiezen: 

* **Detectie van meerdere netwerkroutes.** TCP is nauwkeuriger wanneer meerdere routes detecteren en moet minder agents in elk subnet. Een of twee agents die gebruikmaken van TCP kunnen bijvoorbeeld alle redundante paden tussen subnetten te detecteren. U moet verschillende agents die gebruikmaken van ICMP om vergelijkbare resultaten te bereiken. Via ICMP, als u een aantal routes tussen twee subnetten hebt, moet u meer dan 5N agents in het subnet van een bron- of doelserver.

* **De nauwkeurigheid van de resultaten.** Routers en switches vaak lagere prioriteit toewijzen aan ICMP ECHO-pakketten in vergelijking met TCP-pakketten. In bepaalde situaties wanneer netwerkapparaten zwaar worden geladen, weerspiegelt de gegevens die zijn verkregen door het TCP beter en de netwerklatentie opgetreden door toepassingen. Dit gebeurt omdat het meeste verkeer toepassing loopt via TCP. In dergelijke gevallen biedt ICMP minder nauwkeurige resultaten vergeleken met TCP. 

* **Firewall-configuratie.** TCP-protocol wordt vereist dat de TCP-pakketten worden verzonden naar een doelpoort. De standaardpoort gebruikt door de agents netwerk Prestatiemeter is 8084. U kunt de poort wijzigen bij het configureren van agents. Zorg ervoor dat uw netwerkfirewalls of (NSG) netwerkbeveiligingsgroepen (in Azure)-verkeer op poort toestaan. U moet ook om ervoor te zorgen dat de lokale firewall op de computers waarop agents zijn geïnstalleerd, is geconfigureerd om verkeer op deze poort te staan. U kunt PowerShell-scripts firewallregels configureren op uw computers waarop Windows wordt uitgevoerd, maar u moet handmatig configureren van de firewall van uw netwerk. Daarentegen werken ICMP niet met behulp van een poort. ICMP-verkeer is in de meeste zakelijke scenario's, kunt u gebruikmaken van netwerk diagnostische hulpprogramma's zoals het hulpprogramma ping toegestaan via de firewalls. Als u één machine van een andere pingen kunt, kunt u het ICMP-protocol gebruiken zonder handmatig configureren van firewalls.

>[!NOTE] 
> Sommige firewalls blokkeren mogelijk de ICMP, wat kan leiden tot een herverzending die in een groot aantal gebeurtenissen in uw security information en event management-systeem resulteert. Zorg ervoor dat het protocol dat u kiest, wordt niet geblokkeerd door een netwerkfirewall of het NSG. Prestatiemeter-netwerk kan niet anders het netwerksegment bewaken. U wordt aangeraden TCP voor bewaking. Gebruik ICMP in scenario's waarbij u TCP, bijvoorbeeld wanneer niet gebruiken: 
>
> - U gebruikt Windows client-knooppunten, omdat de onbewerkte sockets TCP zijn niet toegestaan in Windows-clients.
> - Uw netwerkfirewall of het NSG blokkeert TCP.
> - U kunt het wijzigen van het protocol niet weet.

Als u kiest voor het gebruik van ICMP tijdens de implementatie, kunt u overschakelen naar TCP op elk gewenst moment door de standaardinstellingen voor bewaking van de regel te bewerken.

1. Ga naar **netwerkprestaties** > **Monitor** > **configureren**   >  **Monitor**. Selecteer vervolgens **standaardregel**. 
2. Schuif naar de **Protocol** uit en selecteer het protocol dat u wilt gebruiken. 
3. Selecteer **opslaan** de instelling wilt toepassen. 

Zelfs als de standaardregel voor een specifieke protocol gebruikt, kunt u nieuwe regels maken met een ander protocol. U kunt ook een combinatie van regels waar sommige regels gebruiken ICMP en andere TCP maken. 

## <a name="walkthrough"></a>Walkthrough 

Bekijk nu een eenvoudige onderzoek naar de hoofdoorzaak van een health-gebeurtenis.

Op het dashboard oplossing toont een statusgebeurtenis een netwerkverbinding is beschadigd. Voor het onderzoeken van het probleem, selecteer de **netwerk koppelingen bewaakt** tegel.

De pagina inzoomen ziet u dat de **DMZ2 DMZ1** netwerkkoppeling niet in orde is. Selecteer **subnet koppelingen weergeven** voor deze netwerkkoppeling. 


De pagina inzoomen toont alle subnetwerkkoppelingen in de **DMZ2 DMZ1** netwerkkoppeling. De latentie overschreden voor beide subnetwerkkoppelingen de drempelwaarde, waardoor de netwerkkoppeling slecht. U kunt ook beide subnetwerkkoppelingen latentie trends zien. Gebruik de Tijdselectie beheren in de grafiek te concentreren op de vereiste periode. Hier ziet u het tijdstip waarop latentie de piek bereikt. Zoek de logboeken voor deze periode voor het onderzoeken van het probleem later. Selecteer **knooppuntkoppelingen weergeven** verdere detailanalyse. 
 
 ![De pagina koppelingen subnetwerk](media/log-analytics-network-performance-monitor/subnetwork-links.png) 

Net als bij de vorige pagina, Inzoomen voor de koppeling bepaalde subnetwerk worden op deze pagina de samenstellende knooppuntkoppelingen. U kunt vergelijkbare bewerkingen uitvoeren hier net als bij de vorige stap. Selecteer **weergave topologie** om weer te geven van de topologie tussen de twee knooppunten. 
 
 ![De pagina koppelingen knooppunt](media/log-analytics-network-performance-monitor/node-links.png) 

Alle paden tussen de twee geselecteerde knooppunten worden getekend in de topologiekaart. U kunt de topologie hop door hop van routes tussen twee knooppunten op de topologiekaart visualiseren. Dit biedt u een duidelijk beeld van hoeveel routes bestaan tussen de twee knooppunten en wat de gegevenspakketten nemen paden. Knelpunten in de prestaties worden in rood weergegeven. Om te zoeken in een defecte netwerkverbinding of een defecte netwerkapparaat, bekijkt u de rode elementen op de topologiekaart. 

 ![Topologie Dashboard met topologiekaart](media/log-analytics-network-performance-monitor/topology-dashboard.png) 

U kunt bekijken verlies, latentie en het aantal hops in elk pad in de **actie** deelvenster. Gebruik de schuifbalk om de details van de slechte paden weer te geven. De filters gebruiken om de paden met de slechte hop selecteren zodat de topologie voor alleen de geselecteerde paden wordt getekend. Als u wilt inzoomen in of buiten de topologiekaart, gebruiken het muiswiel. 

In de volgende afbeelding is de oorzaak van de probleemgebieden naar de specifieke sectie van het netwerk worden weergegeven in de rode paden en hops. Selecteer een knooppunt in de topologiekaart om de eigenschappen van het knooppunt, waaronder de FQDN-naam en IP-adres zichtbaar te maken. Als u een hop bevat het IP-adres van de hop. 
 
![Topologiekaart met eigenschappen van het knooppunt geselecteerd](media/log-analytics-network-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Volgende stappen
[Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde netwerk prestaties gegevensrecords weer te geven.
