---
title: Prestaties bewaken functie in Network Performance Monitor-oplossing in Azure Log Analytics | Microsoft Docs
description: De mogelijkheid Prestatiemeter van Network Performance Monitor kunt u netwerkconnectiviteit controleren voor verschillende punten in uw netwerk. U kunt controleren cloudimplementaties en on-premises locaties, meerdere datacenters en filialen, en met meerdere lagen, essentiële toepassingen of microservices.
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
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: abshamsft
ms.component: ''
ms.openlocfilehash: 1254afc1ad1c513c18d565be8a6543a6ee0ae94b
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48040588"
---
# <a name="network-performance-monitor-solution-performance-monitoring"></a>Network Performance Monitor-oplossing: prestatiebewaking

De mogelijkheid van Prestatiemeter [Network Performance Monitor](log-analytics-network-performance-monitor.md) helpt u het controleren van de netwerkverbinding tussen verschillende punten in uw netwerk. U kunt controleren cloudimplementaties en on-premises locaties, meerdere datacenters en filialen, en met meerdere lagen, essentiële toepassingen of microservices. Met Prestatiemeter, kunt u netwerkproblemen detecteren voordat uw gebruikers klagen. Belangrijkste voordelen zijn dat u kunt: 

- Verlies en latentie controleren in verschillende subnetten en waarschuwingen kunt instellen.
- Bewaken alle paden (inclusief redundante paden) op het netwerk.
- Problemen met tijdelijke en point-in-time-netwerk, die moeilijk te repliceren.
- Bepaal het specifieke segment van het netwerk, die verantwoordelijk is voor de verslechterde prestaties.
- Bewaak de status van het netwerk, zonder dat SNMP nodig.


![Netwerkprestatiemeter](media/log-analytics-network-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Configuratie
Open de configuratie voor Network Performance Monitor te openen de [Network Performance Monitor oplossing](log-analytics-network-performance-monitor.md), en selecteer **configureren**.

![Network Performance Monitor configureren](media/log-analytics-network-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Nieuwe netwerken maken

Een netwerk in Network Performance Monitor is een logische container voor subnetten. Hiermee kunt u de bewaking van de infrastructuur van uw netwerk op basis van uw behoeften organiseren. U kunt maken van een netwerk met een beschrijvende naam en subnetten aan toe te voegen op basis van uw bedrijfslogica. U kunt bijvoorbeeld een netwerk met de naam Londen maken en toevoegen van alle subnetten in uw datacenter Londen. Kunt u een netwerk met de naam *ContosoFrontEnd* en alle subnetten met de naam Contoso die de front-end van uw app dienen toe te voegen aan dit netwerk. De oplossing maakt automatisch een standaard-netwerk waarin alle subnetten in uw omgeving gedetecteerd. 

Wanneer u een netwerk maakt, voegt u een subnet toe. Vervolgens wordt dat subnet verwijderd uit de standaard-netwerk. Als u een netwerk verwijdert, worden alle bijbehorende subnetten automatisch geretourneerd naar de standaard-netwerk. Het standaardnetwerk fungeert als een container voor alle subnetten die niet zijn opgenomen in een door de gebruiker gedefinieerde netwerk. U niet bewerken of verwijderen van de standaard-netwerk. Het blijft altijd in het systeem. U kunt zoveel aangepaste netwerken als u wilt maken. In de meeste gevallen zijn de subnetten in uw organisatie zijn gerangschikt in meer dan één netwerk. Maak een of meer netwerken om te groeperen van uw subnetten voor uw bedrijfslogica.

Een nieuw netwerk te maken:


1. Selecteer de **netwerken** tabblad.
1. Selecteer **toevoegen netwerk**, en voer vervolgens de netwerknaam en de beschrijving. 
2. Selecteer een of meer subnetten, en selecteer vervolgens **toevoegen**. 
3. Selecteer **opslaan** aan de configuratie op te slaan. 


### <a name="create-monitoring-rules"></a>Maken van regels voor bewaking 

Prestatiemeter genereert statusgebeurtenissen wanneer de drempel van de prestaties van netwerkverbindingen tussen twee subnetwerken of tussen twee netwerken is geschonden. Het systeem kunt automatisch deze drempels leren. U kunt ook aangepaste drempels opgeven. Het systeem maakt automatisch een standaardregel, die een statusgebeurtenis wanneer schendingen verlies of latentie tussen elk paar netwerk of subnet koppelingen genereert de drempelwaarde system geleerd. Dit proces zorgt de oplossing die uw netwerkinfrastructuur te controleren totdat u alle regels voor bewaking expliciet nog niet hebt gemaakt. Als de standaardregel is ingeschakeld, verzendt alle knooppunten synthetische transacties voor alle andere knooppunten dat u hebt ingeschakeld voor bewaking. De standaardregel is handig voor kleine netwerken. Een voorbeeld is een scenario waarin u een klein aantal servers met een microservice hebt en u wilt om ervoor te zorgen dat alle servers verbonden met elkaar zijn.

>[!NOTE]
> U wordt aangeraden dat u de standaardregel uitschakelen en het maken van aangepaste regels voor bewaking, vooral bij grote netwerken waarin u een groot aantal knooppunten voor bewaking gebruiken. Aangepaste bewakingsregels kan het verkeer dat wordt gegenereerd door de oplossing en help die u organiseert de bewaking van uw netwerk te verminderen.

Regels voor bewaking op basis van uw bedrijfslogica maken. Een voorbeeld is als u wilt bewaken van de prestaties van de netwerkverbinding van twee office-sites met hoofdkantoor. Groep alle subnetten in office site1 in netwerk O1. Alle subnetten in office site2 in netwerk O2 groeperen. Ten slotte groep alle subnetten in het hoofdkantoor in netwerk h maakt twee regels voor bewaking--één tussen O1 en H en de andere tussen O2 en h 

Aangepaste regels voor bewaking maken:

1. Selecteer **regel toevoegen** op de **Monitor** tabblad en voer de naam en beschrijving.
2. Selecteer het paar van netwerk- of subnetwerk koppelingen om te controleren in de lijst. 
3. Selecteer het netwerk met de subnetwerken die u wilt dat uit de vervolgkeuzelijst van het netwerk. Selecteer vervolgens de subnetwerken in de vervolgkeuzelijst bijbehorende subnetwerk. Als u controleren van de subnetwerken in een netwerkverbinding wilt, selecteert u **alle subnetwerken**. Op dezelfde manier, selecteert u de andere subnetwerken die u wilt. Als u wilt uitsluiten van bewaking voor bepaalde subnetwerkkoppelingen van de selecties die u hebt aangebracht, selecteert u **uitzondering toevoegen**. 
4. Kiezen tussen ICMP- en TCP-protocollen voor het uitvoeren van de synthetische transacties. 
5. Als u niet wilt maken van health-gebeurtenissen voor de items die u hebt geselecteerd, schakel **statuscontrole inschakelen op de koppelingen wordt gedekt door deze regel**. 
6. Kies voorwaarden controleren. Om in te stellen aangepaste drempelwaarden voor het genereren van health-gebeurtenis, voer drempelwaarden. Wanneer de waarde van de voorwaarde van de geselecteerde drempelwaarde voor het geselecteerde netwerk of subnet paar overschrijdt, wordt een statusgebeurtenis wordt gegenereerd. 
7. Selecteer **opslaan** aan de configuratie op te slaan. 

Nadat u een bewakingsregel hebt opgeslagen, kunt u die regel integreren met waarschuwingen te beheren door te selecteren **waarschuwing maken**. Een waarschuwingsregel wordt automatisch gemaakt met de zoekquery. Andere vereiste parameters worden automatisch ingevuld. U kunt waarschuwingen op basis van e-mail naast de bestaande waarschuwingen in Network Performance Monitor met behulp van een waarschuwingsregel, ontvangen. Waarschuwingen ook corrigerende acties met runbooks kunnen activeren, of ze kunnen integreren met bestaande oplossingen voor service-beheer met behulp van webhooks. Selecteer **waarschuwing beheren** bewerken van de instellingen voor meldingen. 

U kunt nu meer Prestatiemeter-regels maken of verplaatsen naar het dashboard van de oplossing de mogelijkheid te gebruiken.

### <a name="choose-the-protocol"></a>Kies het protocol

Network Performance Monitor maakt gebruik van synthetische transacties voor het berekenen van network performance metrische gegevens zoals pakket koppeling beschermd tegen verlies en latentie. Dit concept beter begrijpen, houd rekening met een Network Performance Monitor-agent die is verbonden met één end van een netwerkverbinding. Deze agent Network Performance Monitor stuurt testpakketten naar een tweede Network Performance Monitor agent verbonden met een andere einde van het netwerk. De tweede agent antwoorden met response-pakketten. Dit proces wordt een paar keer herhaald. Door te meten of het aantal antwoorden en de tijd die elk antwoord ontvangen, de eerste Network Performance Monitor agent beoordeelt de latentie van de verbinding en verloren pakketten. 

De indeling, de grootte en de volgorde van deze pakketten wordt bepaald door het protocol dat u bij het maken van regels voor bewaking. Op basis van het protocol van de pakketten, de tussenliggende netwerkapparaten, zoals routers en switches, kunnen deze pakketten verwerken anders. Uw keuze protocol is als gevolg daarvan kan van invloed op de nauwkeurigheid van de resultaten. Uw keuze protocol bepaalt ook of u eventuele handmatige stappen uitvoeren moet nadat u de oplossing Netwerkprestatiemeter implementeert. 

Network Performance Monitor biedt u de keuze tussen ICMP- en TCP-protocollen voor het uitvoeren van synthetische transacties. Als u ICMP wanneer u een regel synthetische transactie maken, de agents Network Performance Monitor ICMP ECHO berichten wordt gebruikt voor het berekenen van de netwerklatentie en pakketverlies. ICMP-ECHO maakt gebruik van hetzelfde bericht dat wordt verzonden door het hulpprogramma conventionele ping. Als u TCP als protocol gebruikt, verzenden Network Performance Monitor agents TCP SYN pakketten via het netwerk. Deze stap wordt gevolgd door een TCP-handshake is voltooid en de verbinding is verwijderd met behulp van de eerste pakketten. 

Houd rekening met de volgende informatie voordat u een protocol kiezen: 

* **Detectie van meerdere netwerkroutes.** TCP is meer nauwkeurige wanneer meerdere routes detecteren en er minder agents in elk subnet nodig. Een of twee agents die gebruikmaken van TCP kunnen bijvoorbeeld alle redundante paden tussen subnetten te detecteren. U moet verschillende agents die gebruikmaken van ICMP om dezelfde resultaten te behalen. Via ICMP, hebt u een aantal routes tussen twee subnetten, moet u meer dan 5N agents in het subnet van een bron- of doelservers.

* **De nauwkeurigheid van de resultaten.** Routers en switches vaak lagere prioriteit toewijzen aan ICMP-ECHO-pakketten in vergelijking met TCP-pakketten. In bepaalde situaties wanneer netwerkapparaten zwaar worden geladen, weerspiegelt de gegevens die zijn verkregen door het TCP beter aansluiten de verlies en latentie die wordt ervaren door toepassingen. Dit gebeurt omdat de meeste van de toepassingsverkeer stroomt via TCP. In dergelijke gevallen biedt ICMP minder nauwkeurige resultaten in vergelijking met TCP. 

* **Firewall-configuratie.** TCP-protocol is vereist dat de TCP-pakketten worden verzonden naar een bestemmingspoort. De standaardpoort gebruikt door Network Performance Monitor agents is 8084. U kunt de poort wijzigen bij het configureren van agents. Zorg ervoor dat uw netwerkfirewalls of (NSG) regels voor netwerkbeveiligingsgroepen (in Azure) verkeer op de gegevenspoort toestaan. U moet ook om ervoor te zorgen dat de lokale firewall op de computers waarop agents zijn geïnstalleerd is geconfigureerd voor het toestaan van verkeer op deze poort. U kunt PowerShell-scripts gebruiken firewallregels configureren op uw Windows-computers, maar u moet de firewall van uw netwerk handmatig configureren. ICMP werkt echter niet met behulp van een poort. ICMP-verkeer is in de meeste zakelijke scenario's, kunt u gebruikmaken van netwerk diagnostische hulpprogramma's, zoals het hulpprogramma ping toegestaan via de firewalls. Als u één VM van elkaar pingen kan, kunt u het ICMP-protocol gebruiken zonder handmatig configureren van firewalls.

>[!NOTE] 
> Sommige firewalls blokkeren mogelijk de ICMP, wat kan leiden tot opnieuw verzenden die in een groot aantal gebeurtenissen in uw gegevens en gebeurtenissen beheersysteem voor informatiebeveiliging resulteert. Zorg ervoor dat het protocol dat u niet wordt geblokkeerd door een firewall of de NSG. Network Performance Monitor kan niet anders het netwerksegment bewaken. U wordt aangeraden dat u TCP voor bewaking gebruiken. ICMP gebruiken in scenario's waarin u TCP, bijvoorbeeld wanneer niet gebruiken: 
>
> - U gebruikt Windows client-knooppunten, omdat het onbewerkte TCP-sockets zijn niet toegestaan in Windows-clients.
> - TCP wordt geblokkeerd door uw firewall of de NSG.
> - Weet u het wijzigen van het protocol niet.

Als u ICMP gebruiken tijdens de implementatie, kunt u overschakelen naar TCP op elk gewenst moment door de standaardinstellingen voor bewaking van de regel te bewerken.

1. Ga naar **netwerkprestaties** > **Monitor** > **configureren**   >  **Monitor**. Selecteer vervolgens **standaardregel**. 
2. Schuif naar de **Protocol** uit en selecteer het protocol dat u wilt gebruiken. 
3. Selecteer **opslaan** om toe te passen van de instelling. 

Zelfs als de standaardregel voor een specifieke protocol gebruikt, kunt u nieuwe regels maken met een ander protocol. U kunt zelfs een combinatie van regels waarbij sommige regels ICMP gebruiken en andere TCP gebruiken maken. 

## <a name="walkthrough"></a>Walkthrough 

Bekijk nu een eenvoudige onderzoek naar de hoofdoorzaak van een statusgebeurtenis in.

Op het dashboard van de oplossing toont een statusgebeurtenis dat er een netwerkverbinding beschadigd is. U kunt het probleem onderzoeken, selecteert u de **netwerk koppelingen die worden bewaakt** tegel.

De Inzoomen op pagina ziet u dat de **DMZ2 DMZ1** netwerkkoppeling niet in orde is. Selecteer **subnetkoppelingen weergeven** voor deze netwerkkoppeling. 


De Inzoomen op pagina ziet u alle subnetwerkkoppelingen in de **DMZ2 DMZ1** netwerkverbinding. Voor beide subnetwerkkoppelingen Gekruiste de latentie van de drempelwaarde, waardoor de netwerkkoppeling niet in orde. U kunt ook de latentie-trends van beide subnetwerkkoppelingen zien. Gebruik de Tijdselectie beheren in de grafiek om zich te richten op het bereik van de tijd die nodig is. Hier ziet u het tijdstip waarop die bij de piek het bereiken van latentie. Zoeken in de logboeken later voor deze periode is het probleem kunnen onderzoeken. Selecteer **knooppuntkoppelingen weergeven** verder inzoomen. 
 
 ![Pagina met koppelingen subnetwerk](media/log-analytics-network-performance-monitor/subnetwork-links.png) 

Net als bij de vorige pagina, Inzoomen op voor de specifieke subnetwerkkoppeling worden op deze pagina de samenstellende knooppuntkoppelingen. U kunt dezelfde acties uitvoeren hier als u in de vorige stap hebt gedaan. Selecteer **topologie weergeven** om weer te geven van de topologie tussen de twee knooppunten. 
 
 ![Pagina met koppelingen knooppunt](media/log-analytics-network-performance-monitor/node-links.png) 

Alle paden tussen de twee geselecteerde knooppunten worden in de topologie-kaart getekend. U kunt de hop-by-hop '-topologie van routes tussen twee knooppunten op de kaart topologie kunt visualiseren. Het biedt u een helder beeld van het aantal routes bestaan tussen de twee knooppunten en wat de gegevenspakketten nemen paden. Netwerk-knelpunten worden weergegeven in het rood. Als u wilt een defecte netwerkverbinding of een foutieve netwerkapparaat vinden, bekijkt u de rode elementen op de kaart topologie. 

 ![Topologiedashboard met topologiekaart weer](media/log-analytics-network-performance-monitor/topology-dashboard.png) 

U kunt bekijken, het verlies van gegevens, latentie en het aantal hops in elk pad in de **actie** deelvenster. Gebruik de schuifbalk om de details van de paden niet in orde weer te geven. De filters gebruiken om te selecteren van de paden met de slechte hops zodat de topologie voor alleen de geselecteerde paden wordt getekend. Als u wilt in- of buiten de topologiekaart weer wilt uitzoomen, gebruikt u het muiswiel. 

In de volgende afbeelding is de hoofdoorzaak van het probleemgebieden naar het specifieke gedeelte van het netwerk worden weergegeven in de rode paden en hops. Selecteer een knooppunt in de kaart topologie om de eigenschappen van het knooppunt, waaronder de FQDN-naam en IP-adres weer te geven. Selecteren van een hop bevat het IP-adres van de hop. 
 
![Topologiekaart weer met de eigenschappen van het knooppunt geselecteerd](media/log-analytics-network-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Volgende stappen
[Zoeken in logboeken](log-analytics-log-searches.md) om gedetailleerde gegevens prestatierecords weer te geven.
