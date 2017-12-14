---
title: Netwerk-Prestatiemeter-oplossing in Azure Log Analytics | Microsoft Docs
description: De Prestatiemeter netwerk in Azure Log Analytics, helpt u bij het controleren van de prestaties van uw netwerken in bijna de real-eenmalig aan detecteren en zoek netwerk knelpunten.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: banders
ms.openlocfilehash: d5d5ec1b524fa455c8d2231c7c16fd7942f713c4
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="network-performance-monitor-solution-in-log-analytics"></a>Netwerk-Prestatiemeter-oplossing in Log Analytics

![Netwerk Prestatiemeter symbool](./media/log-analytics-network-performance-monitor/npm-symbol.png)

Dit document wordt beschreven hoe wilt instellen en gebruiken de netwerk-Prestatiemeter-oplossing in Log Analytics, die helpt u bij het controleren van de prestaties van uw netwerken in bijna de real-eenmalig aan detecteren en zoek prestatieknelpunten netwerk. U kunt controleren en de netwerklatentie tussen twee netwerken, subnetten of servers met de netwerk-Prestatiemeter-oplossing. Prestatiemeter netwerk detecteert netwerkproblemen zoals verkeer blackholing, routering fouten en problemen die controlemethoden conventionele netwerk niet detecteren. Netwerk Performance Monitor genereert waarschuwingen en ontvangt een melding als en wanneer een drempelwaarde voor een netwerkverbinding is geschonden. Deze drempels automatisch kunnen worden geleerd door het systeem of kunt u ze voor het gebruik van aangepaste regels voor waarschuwingen configureren. Prestatiemeter-netwerk wordt gewaarborgd tijdige detectie van problemen met de netwerkprestaties en de bron van het probleem met een bepaald netwerksegment of het apparaat is vertaald.

Netwerkproblemen met het dashboard van de oplossing die samengevatte geeft informatie weer over uw netwerk waaronder recente netwerk health gebeurtenissen, slechte netwerkkoppelingen en subnetwerkkoppelingen die met hoge pakketverlies en latentie kampen, kunt u detecteren. U kunt inzoomen in een netwerkkoppeling om de huidige status van subnetwerkkoppelingen alsook knooppunt naar links weer te geven. U kunt ook de historische trend van en netwerklatentie op het netwerk, subnetwerk en knooppunt naar niveau weergeven. U kunt tijdelijke netwerkproblemen detecteren door het bekijken van historische trendgrafieken voor pakketverlies en latentie en zoek netwerkknelpunten op een topologiekaart. De grafiek interactieve topologie kunt u de netwerkroutes hop door hop visualiseren en de bron van het probleem te bepalen. Net als andere oplossingen, kunt u logboek zoeken naar verschillende analytics-vereisten voor het maken van aangepaste rapporten op basis van de gegevens die worden verzameld door Netwerkcontrole prestaties.

De oplossing maakt gebruik van synthetische transacties als een primaire methode voor het detecteren van netwerkfouten. U kunt het geval is, gebruiken zonder rekening wordt gehouden met de leverancier van een bepaald netwerkapparaat of het model. Voor on-premises, cloud (IaaS) en hybride omgevingen werkt. De oplossing detecteert automatisch de netwerktopologie en verschillende routes in uw netwerk.

Typische netwerk bewaking producten zich richten op het apparaat (routers, switches, enzovoort) netwerk status bewaken, maar bieden inzicht in de werkelijke kwaliteit van de netwerkverbinding tussen de twee punten Prestatiemeter netwerk heeft geen.

### <a name="using-the-solution-standalone"></a>Met behulp van de zelfstandige oplossing
Als u bewaken van de kwaliteit van netwerkverbindingen tussen hun kritieke werkbelastingen wilt, kunt netwerken, datacenters of office sites, dan hebt u de netwerk-Prestatiemeter-oplossing zelfstandig gebruiken voor het bewaken van de status connectiviteit tussen:

* meerdere datacenters of office-sites die zijn verbonden via een openbaar of particulier netwerk
* kritieke werkbelastingen die line-of-business-toepassingen worden uitgevoerd
* openbare cloud-services zoals Microsoft Azure of Amazon Web Services (AWS) en on-premises netwerken, als u IaaS (VM) beschikbaar is en u gateways die zijn geconfigureerd hebt, zodat de communicatie tussen on-premises netwerken en cloud-netwerken
* Azure en on-premises netwerken wanneer u Express Route gebruikt

### <a name="using-the-solution-with-other-networking-tools"></a>Met behulp van de oplossing met andere hulpprogramma's voor netwerken
Als u bewaken van een line-of-business-toepassing wilt, kunt u de netwerk-Prestatiemeter-oplossing als andere hulpprogramma's voor de network companion oplossing. Een traag netwerk kan leiden tot een trage toepassingen en netwerk Prestatiemeter kan u helpen bij het onderzoeken van prestatieproblemen toepassing die worden veroorzaakt door de onderliggende netwerkproblemen. Omdat de oplossing geen toegang hebben tot de netwerkapparaten niet vereist, moet de beheerder van de toepassing niet afhankelijk zijn van een team netwerken om informatie over hoe het netwerk is voor de toepassingen te geven.

Ook als u al in andere hulpprogramma's voor netwerk investeert, klikt u vervolgens de oplossing kan een aanvulling op deze hulpprogramma's omdat meest traditioneel netwerk bewakingsoplossingen geen inzicht in de end-to-end netwerk maatstaven voor prestaties zoals en netwerklatentie bieden.  De netwerk-Prestatiemeter-oplossing kunt die ruimte te vullen.

## <a name="installing-and-configuring-agents-for-the-solution"></a>Installeren en configureren van agents voor de oplossing
De fundamentele processen gebruiken voor het installeren van agents op [verbinding maken met Windows-computers met logboekanalyse](log-analytics-windows-agent.md) en [Operations Manager verbinden met Log Analytics](log-analytics-om-agents.md).

> [!NOTE]
> U moet ten minste 2 om agents te installeren om voldoende gegevens om te detecteren en bewaken van uw netwerkbronnen. Anders wordt blijft de oplossing configureren status totdat installeert en configureert u aanvullende agents.
>
>

### <a name="where-to-install-the-agents"></a>Waar u de agents installeert
Voordat u agents installeren, houd rekening met de topologie van uw netwerk en welke onderdelen van het netwerk die u wilt bewaken. Het is raadzaam dat u meer dan één agent voor elk subnet dat u wilt bewaken installeert. Met andere woorden, twee of meer servers of virtuele machines kiezen voor elk subnet dat u wilt bewaken, en installeer de agent op deze.

Als u over de topologie van uw netwerk weet, moet u de agents installeren op servers met kritieke werkbelastingen, waar u wilt bewaken van de prestaties van het netwerk. U wilt bijhouden van een netwerkverbinding tussen een webserver en een server met SQL Server. In dit voorbeeld zou u een agent installeren op beide servers.

Agents bewaken netwerkverbinding (koppelingen) tussen hosts--niet de hosts zelf. Dus als u wilt bewaken van een netwerkverbinding, u agents op beide eindpunten van die koppeling installeren moet.

### <a name="configure-agents"></a>Agents configureren

Als u van plan bent met het ICMP-protocol voor synthetische transacties, moet u de volgende firewallregels voor ICMP betrouwbare manier gebruik inschakelen:

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow
```


Als u wilt gebruiken het TCP-protocol moet u open firewallpoorten voor computers om ervoor te zorgen dat de agents kunnen communiceren. U moet downloaden en voer vervolgens de [EnableRules.ps1 PowerShell-script](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) zonder parameters in een PowerShell-venster met beheerdersbevoegdheden.

Het script maakt de netwerk-Prestatiemeter vereiste registersleutels en Windows firewall-regels zodat agents TCP-verbindingen maken met elkaar wordt gemaakt. De registersleutels gemaakt door het script ook opgeven of om de logboeken voor foutopsporing en het pad voor het bestand logboeken te leggen. Het definieert ook de agent TCP-poort gebruikt voor communicatie. De waarden voor deze sleutels worden automatisch ingesteld door het script, zodat deze sleutels niet handmatig te wijzigen.

De poort die standaard geopend is 8084. U kunt een aangepaste poort gebruiken door de parameter `portNumber` aan het script. Echter moet dezelfde poort worden gebruikt op alle computers waarop het script wordt uitgevoerd.

> [!NOTE]
> Het script EnableRules.ps1 configureert Windows firewall-regels alleen op de computer waarop het script wordt uitgevoerd. Als u een netwerkfirewall hebt, moet u ervoor zorgen dat verkeer dat is bestemd voor de TCP-poort wordt gebruikt door Netwerkcontrole prestaties kunnen.
>
>

## <a name="configuring-the-solution"></a>De oplossing configureren
Gebruik de volgende informatie om te installeren en configureren van de oplossing.

1. De netwerk-Prestatiemeter-oplossing verkrijgt gegevens van computers met Windows Server 2008 SP 1 of hoger of Windows 7 SP1 of hoger, waarop dezelfde vereisten als de Microsoft Monitoring Agent (MMA) zijn. NPM-agents kunnen ook uitvoeren op Windows bureaublad-client-besturingssystemen (Windows 10, Windows 8.1, Windows 8 en Windows 7).
    >[!NOTE]
    >De agents voor Windows server-besturingssystemen ondersteunen zowel ICMP als TCP-als de protocollen voor synthetische transactie. De agents voor Windows-besturingssystemen ondersteunen echter alleen ICMP als protocol voor synthetische transactie.

2. De netwerk-Prestatiemeter-oplossing toevoegen aan uw werkruimte van [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).<br><br> ![Netwerk Prestatiemeter symbool](./media/log-analytics-network-performance-monitor/npm-symbol.png)  
3. In de OMS-portal, ziet u een nieuwe tegel met de titel **netwerk Prestatiemeter** met het bericht *oplossing is aanvullende configuratie vereist*. Klik op de tegel om te navigeren naar de **implementatie** tabblad en selecteer het protocol moet worden gebruikt voor het maken van de synthetische transacties voor het bewaken van uw netwerk.  Bekijk [kiest u het juiste protocol-ICMP- of TCP](#choose-the-right-protocol-icmp-or-tcp) bij het kiezen van het juiste protocol geschikt zijn voor uw netwerk.<br><br> ![oplossing vereist Protocolselectie](media/log-analytics-network-performance-monitor/log-analytics-netmon-perf-welcome.png)<br><br>

4. Nadat u hebt gekozen het protocol wordt u omgeleid naar de **OMS overzicht** pagina. Terwijl de oplossing maakt een aggregatie van gegevens van uw netwerk, de Prestatiemeter netwerk overzicht-tegel wordt weergegeven de met de melding *gegevens worden verzameld*.<br><br> ![oplossing aggregeren van gegevens](media/log-analytics-network-performance-monitor/log-analytics-netmon-tile-status-01.png)<br><br>
5. Nadat de gegevens worden verzameld en worden geïndexeerd, wordt de overzichttegel wijzigen en geven moet u een aanvullende configuratie uitvoeren.<br><br> ![tegel oplossing is aanvullende configuratie vereist.](media/log-analytics-network-performance-monitor/log-analytics-netmon-tile-status-02.png)<br><br>
6. Klik op de tegel en beginnen met de configuratie van de oplossing voor de onderstaande stappen te volgen.

### <a name="create-new-networks"></a>Nieuwe netwerken maken
Een netwerk in Prestatiemeter netwerk is een logische container voor subnetten. U kunt een netwerk maken met een beschrijvende naam en subnetten volgens uw bedrijfslogica aan toe te voegen. U kunt bijvoorbeeld een netwerk met de naam maken *Londen* en voegt alle subnetten in uw datacenter Londen of een netwerk met de naam *ContosoFrontEnd* en alle subnetten voor de front-end van uw app met de naam Contoso toevoegen met dit netwerk.
Op de configuratiepagina ziet u een netwerk met de naam **standaard** in het tabblad netwerken. Als u geen netwerken hebt gemaakt, worden alle subnetten automatisch worden gedetecteerd in het netwerk standaard geplaatst.
Wanneer u een netwerk maakt, toevoegen van een subnet met en dat subnet uit de standaardnetwerk is verwijderd. Als u een netwerk verwijdert, worden automatisch alle bijbehorende subnetten geretourneerd met het standaard-netwerk.
Dus fungeert het standaardnetwerk als een container voor alle subnetten die niet zijn opgenomen in een door de gebruiker gedefinieerde netwerk. U niet kunt bewerken of verwijderen van het standaard-netwerk. Het blijft altijd in het systeem. U kunt echter zo veel aangepaste netwerken behoefte maken.
In de meeste gevallen de subnetten in uw organisatie worden gerangschikt in meer dan één netwerk en moet u een of meer netwerken om uw subnetten volgens uw bedrijfslogica groep maken

#### <a name="to-create-a-new-network"></a>Een nieuwe netwerk maken
1. Klik op **toevoegen netwerk** en typ de netwerknaam en beschrijving.
2. Selecteer een of meer subnetten en klik vervolgens op **toevoegen**.
3. Klik op **opslaan** aan de configuratie op te slaan.<br><br> ![netwerk toevoegen](./media/log-analytics-network-performance-monitor/npm-add-network.png)

### <a name="wait-for-data-aggregation"></a>Wacht tot de gegevens worden verzameld
Nadat u de configuratie voor de eerste keer hebt opgeslagen, begint de oplossing voor het verzamelen van netwerk en netwerklatentie pakketgegevens tussen de knooppunten waarop agents zijn geïnstalleerd. Dit proces kan even duren, soms gedurende 30 minuten. Tijdens deze toestand is, in de tegel prestaties Netwerkcontrole op de overzichtspagina zien een bericht weergegeven *gegevens worden verzameld in proces*.

![de gegevens worden verzameld](./media/log-analytics-network-performance-monitor/npm-aggregation.png)

Wanneer de gegevens zijn geüpload, ziet u de gegevens van de Prestatiemeter netwerk tegel bijgewerkt weergeeft.

![Netwerk Prestatiemeter tegel](./media/log-analytics-network-performance-monitor/npm-tile.png)

Klik op de tegel om de netwerk-Prestatiemeter-dashboard weer te geven.

![Netwerk Prestatiemeter-dashboard](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Controle-instellingen voor subnetten bewerken
Alle subnetten waarin ten minste één agent is geïnstalleerd, worden vermeld op de **subnetwerken** tabblad in de configuratiepagina.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>In-of schakel bewaking voor bepaalde subnetwerken
1. Selecteren of schakel het selectievakje naast de **subnetwerk ID** en zorg ervoor dat **gebruiken voor bewaking** is ingeschakeld of uitgeschakeld, indien nodig. U kunt selecteren of wissen van meerdere subnetten. Wanneer deze is uitgeschakeld, worden de subnetwerken niet bewaakt als de agents worden bijgewerkt om te stoppen met het pingen van andere agents.
2. Kies de knooppunten die u voor een bepaalde subnetwerk door het subnetwerk selecteren in de lijst en de vereiste knooppunten verplaatsen tussen de lijsten met niet-beheerd en gecontroleerd knooppunten wilt bewaken.
   U kunt een aangepaste toevoegen **beschrijving** naar het subnetwerk, als u tevreden bent.
3. Klik op **opslaan** aan de configuratie op te slaan.<br><br> ![bewerken van het subnet](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Kies knooppunten om te controleren
Alle knooppunten die een agent is geïnstalleerd op deze worden vermeld in de **knooppunten** tabblad.

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Naar of schakel bewaking voor knooppunten
1. Selecteer of schakel de knooppunten die u wilt controleren of de bewaking stopt.
2. Klik op **gebruiken voor bewaking**, in- of uitschakelen, naar gelang van toepassing.
3. Klik op **Opslaan**.<br><br> ![Schakel de bewaking van knooppunt](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)

### <a name="set-monitoring-rules"></a>Set regels controleren
Netwerk Performance Monitor genereert health gebeurtenissen als de drempelwaarde van de prestaties van netwerkverbindingen tussen 2 subnetwerken of tussen 2 netwerken is geschonden. Deze drempels automatisch kunnen worden geleerd door het systeem of u kunt aangepaste drempelwaarden.
Het systeem maakt automatisch een standaardregel waarmee een statusgebeurtenis wanneer schendingen verlies of latentie tussen elk paar netwerk/subnetwerk koppelingen genereert de drempelwaarde system geleerd. Dit helpt de oplossing voor uw netwerkinfrastructuur bewaken totdat u alle bewakingsregels expliciet dat nog niet hebt gemaakt. Als de standaardregel is ingeschakeld, verzenden alle knooppunten synthetische transacties voor alle andere knooppunten die u hebt ingeschakeld voor bewaking. De standaardregel is handig in geval van een kleine netwerken, bijvoorbeeld in een scenario waarbij u een klein aantal servers met een microservice hebt en u wilt ervoor zorgen dat alle o de servers verbonden met zijn elkaar.

>[!NOTE]
>Het is raadzaam dat u de standaardregel uitschakelen en aangepaste regels voor controle, vooral bij grote netwerken als u gebruikmaakt van een groot aantal knooppunten voor het bewaken van maken. Zo beperkt u het verkeer dat is gegenereerd door de oplossing en help ordenen van de bewaking van uw netwerk.

Aangepaste bewaking regels volgens uw bedrijfslogica maken. Bijvoorbeeld, als u bewaken van de prestaties van de netwerkverbinding 2 office sites headquarter wilt, vervolgens groep alle subnetten in office site1 in netwerk O1, alle subnetten in office site2 in netwerk O2 en alle subnetten in de headquarter in netwerk H. maken bewaking van regels-een tussen O1 en H en de andere tussen O2 en H. 2


#### <a name="to-create-custom-monitoring-rules"></a>Aangepaste bewaking regels maken
1. Klik op **regel toevoegen** in de **Monitor** tabblad en voer de naam en beschrijving.
2. Het paar van netwerk- of subnetwerk koppelingen om te controleren in de lijst selecteren.
3. Selecteert u eerst het netwerk waarin de eerste subnetwerk (s) van belang is opgenomen in de vervolgkeuzelijst van het netwerk, en selecteer vervolgens de subnetwerk (s) uit de bijbehorende subnetwerk vervolgkeuzelijst.
   Selecteer **alle subnetwerken** als u wilt bewaken van de subnetwerken in een netwerkkoppeling. Selecteer op dezelfde manier de andere subnetwerk (s) van belang. En klikt u op **Voeg uitzondering** moeten worden uitgesloten van bewaking voor bepaalde subnetwerkkoppelingen uit de selectie die u hebt aangebracht.
4. [Kiezen tussen het ICMP- en TCP-protocollen](#choose-the-right-protocol-icmp-or-tcp) voor het uitvoeren van de synthetische transacties.
5. Als u niet wilt maken van de gezondheid van gebeurtenissen voor de items die u hebt geselecteerd, klikt u vervolgens wissen **inschakelen statuscontrole op de koppelingen die door deze regel wordt gedekt**.
6. Kies de bewaking van de voorwaarden.
   U kunt aangepaste drempelwaarden instellen voor het genereren van gebeurtenis health door drempelwaarden in te voeren. Wanneer de waarde van de voorwaarde hoger dan de geselecteerde drempelwaarde voor het geselecteerde netwerk/subnetwerk paar is, wordt een health-gebeurtenis gegenereerd.
7. Klik op **opslaan** aan de configuratie op te slaan.<br><br> ![aangepaste bewaking regel maken](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)

Nadat u een regel voor bewaking hebt opgeslagen, kunt u die regel integreren met beheer van waarschuwingen door te klikken op **waarschuwingen maken**. Een waarschuwingsregel wordt automatisch gemaakt met de zoekopdracht en andere vereiste parameters automatisch ingevuld. Een waarschuwingsregel kunt u waarschuwingen op basis van e-mailbericht naast de bestaande waarschuwingen binnen NPM ontvangen. Waarschuwingen kunnen ook corrigerende acties met runbooks activeren of ze kunnen worden geïntegreerd met bestaande beheersystemen voor service met behulp van webhooks. U kunt klikken op **waarschuwing beheren** bewerken van de instellingen voor waarschuwingen.

### <a name="choose-the-right-protocol-icmp-or-tcp"></a>Kies de juiste-ICMP-protocol of TCP

Netwerk Performance Monitor (NPM) maakt gebruik van synthetische transacties voor het berekenen van de prestatiewaarden netwerk zoals pakket verlies en koppeling latentie. Overweeg om deze beter te begrijpen, een NPM-agent is verbonden met één end van een netwerkverbinding. Deze agent NPM verzendt testpakketten naar een tweede NPM-agent is verbonden met een andere einde van het netwerk. De tweede agent antwoorden met response-pakketten. Dit proces wordt een paar keer herhaald. De eerste NPM agent evalueert koppeling latentie door het meten van het aantal antwoorden en de tijd die nodig is voor het ontvangen van elk antwoord en verloren pakketten.

De indeling, de grootte en de volgorde van deze pakketten wordt bepaald door het protocol dat u bij het maken van regels voor controle. Op basis van het protocol van de pakketten, de tussenliggende netwerkapparaten (routers, switches, enzovoort) deze pakketten anders kan verwerken. Hierdoor kan uw keuze protocol is van invloed op de nauwkeurigheid van de resultaten. En uw keuze protocol bepaalt ook of u een handmatige stappen uitvoeren moet nadat u de NPM-oplossing implementeert.

NPM biedt u de keuze tussen ICMP- en TCP-protocollen voor het uitvoeren van de synthetische transacties.
Als u ICMP kiezen wanneer u een regel synthetische transactie maken, gebruiken de NPM-agents van ICMP-ECHO-berichten voor het berekenen van de netwerklatentie en pakketverlies. ICMP ECHO maakt gebruik van hetzelfde bericht dat door de conventionele opdracht ping wordt verzonden. Wanneer u TCP als protocol gebruikt, verzenden NPM agents TCP SYN-pakket via het netwerk. Dit wordt gevolgd door een TCP-handshake is voltooid en vervolgens verwijdert de verbinding met de eerste pakketten.

#### <a name="points-to-consider-before-choosing-the-protocol"></a>Punten in overweging moet nemen voordat u het protocol
Houd rekening met de volgende informatie voordat u een te gebruiken protocol kiezen:

##### <a name="discovering-multiple-network-routes"></a>Meerdere netwerkroutes detecteren
TCP is nauwkeuriger wanneer meerdere routes detecteren en moet minder agents in elk subnet. Een of twee agents die met TCP kunnen bijvoorbeeld alle redundante paden tussen subnetten te detecteren. U moet echter verschillende agents met ICMP vergelijkbare resultaten bereiken. Met ICMP, hebt u *N* van routes tussen twee subnetten, u meer dan 5 moet*N* agents in het subnet van een bron- of doelserver.

##### <a name="accuracy-of-results"></a>Nauwkeurigheid van resultaten
Routers en switches vaak lagere prioriteit toewijzen aan ICMP ECHO-pakketten in vergelijking met TCP-pakketten. In bepaalde situaties wanneer netwerkapparaten zwaar worden geladen, weerspiegelt de gegevens die zijn verkregen door het TCP beter en de netwerklatentie opgetreden door toepassingen. Dit gebeurt omdat het meeste verkeer toepassing loopt via TCP. In dergelijke gevallen biedt ICMP minder nauwkeurige resultaten vergeleken met TCP.

##### <a name="firewall-configuration"></a>Firewall-configuratie
TCP-protocol wordt vereist dat de TCP-pakketten worden verzonden naar een doelpoort. De standaardpoort gebruikt door de NPM-agents is 8084, maar u dit wijzigen kunt bij het configureren van agents. Daarom moet u ervoor zorgen dat uw netwerkfirewalls of het NSG-regels (in Azure) bieden de mogelijkheid verkeer op de poort. U moet ook om ervoor te zorgen dat de lokale firewall op de computers waarop agents zijn geïnstalleerd, is geconfigureerd om verkeer op deze poort te staan.

PowerShell-scripts kunt u firewallregels configureren op uw computers met Windows, maar u moet de firewall van uw netwerk handmatig configureren.

ICMP werkt echter niet met behulp van poort. ICMP-verkeer is in de meeste zakelijke scenario's, kunt u gebruikmaken van netwerk diagnostische hulpprogramma's zoals het hulpprogramma Ping toegestaan via de firewalls. Dus als u één machine van een andere pingen kunt, u het ICMP-protocol gebruiken kunt zonder het handmatig configureren van firewalls.

> [!NOTE]
> Sommige firewalls blokkeren ICMP, wat tot herverzending leiden kan, wat resulteert in een groot aantal gebeurtenissen in uw systeem security information en event management. Zorg ervoor dat het protocol dat u kiest niet wordt geblokkeerd door een firewall/NSG, anders NPM kan niet worden voor het bewaken van het netwerksegment.  Daarom raden wij u TCP gebruiken voor bewaking. U moet ICMP gebruiken in scenario's waarin u ze niet om TCP te gebruiken, zoals wanneer:
> * U gebruikt Windows-client op basis van knooppunten, omdat de onbewerkte sockets TCP zijn niet toegestaan in Windows-client
> * Uw netwerk firewall/NSG blokken TCP


#### <a name="how-to-switch-the-protocol"></a>Het wijzigen van het protocol

Als u kiest voor het gebruik van ICMP tijdens de implementatie, kunt u overschakelen naar TCP op elk gewenst moment door de standaardinstellingen voor bewaking van de regel te bewerken.

##### <a name="to-edit-the-default-monitoring-rule"></a>De standaardinstellingen voor bewaking van de regel bewerken
1.  Navigeer naar **netwerkprestaties** > **Monitor** > **configureren** > **Monitor** en klik vervolgens op **standaardregel**.
2.  Schuif naar de **Protocol** sectie en selecteer het protocol dat u wilt gebruiken.
3.  Klik op **opslaan** de instelling wilt toepassen.

Zelfs als de standaardregel voor een specifieke protocol wordt gebruikt, kunt u nieuwe regels maken met een ander protocol. U kunt ook een combinatie van regels waar sommige regels ICMP gebruiken en de andere met TCP maken.


## <a name="data-collection-details"></a>Details van de verzameling gegevens
Prestaties Netwerkcontrole gebruikt TCP SYN-SYNACK-ACK handshake pakketten wanneer TCP wordt gekozen en ICMP ECHO ICMP-ECHOANTWOORD wanneer ICMP is gekozen als het protocol en netwerklatentie informatie te verzamelen. Traceroute wordt ook gebruikt voor het ophalen van gegevens over de topologie.

De volgende tabel bevat gegevens verzameling methoden en andere informatie over hoe gegevens worden verzameld voor netwerk-Prestatiemeter.

| Platform | Directe Agent | SCOM-agents | Azure Storage | SCOM vereist? | SCOM-agent gegevens die worden verzonden via de beheergroep | Frequentie van de verzameling |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP-handshakes/ICMP-echoberichten om de vijf seconden gegevens verzonden om de 3 minuten |

De oplossing maakt gebruik van synthetische transacties om de status van het netwerk te beoordelen. OMS-agents die zijn geïnstalleerd op verschillende punt in de TCP-netwerkpakketten exchange of ICMP Echo (afhankelijk van het protocol geselecteerd voor bewaking) met elkaar. In het proces agents meer informatie over de round trip time en pakket verloren zijn gegaan, indien van toepassing. Elke agent voert regelmatig ook een opdracht Traceroute naar andere agents vinden van de verschillende routes in het netwerk dat moet worden getest. Met deze gegevens kunnen de agents deduceren de netwerklatentie en pakket verliescijfers. De tests worden herhaald op elke vijf seconden en de gegevens worden samengevoegd voor een periode van drie minuten door de agents voordat u dit uploadt naar de Log Analytics-service.

> [!NOTE]
> Hoewel vaak agents met elkaar communiceren, ze tijdens de uitvoering van de test geen veel netwerkverkeer genereren. Agents afhankelijk zijn van alleen TCP SYN-SYNACK-ACK handshake pakketten om te bepalen en de netwerklatentie--er worden geen gegevens pakketten worden uitgewisseld. Tijdens dit proces agents met elkaar communiceren alleen wanneer deze nodig is en de agent communicatie-topologie wordt geoptimaliseerd om het netwerkverkeer te verminderen.
>
>

## <a name="using-the-solution"></a>De oplossing gebruiken
Deze sectie wordt uitgelegd dat het dashboard functies en hoe ze te gebruiken.

### <a name="solution-overview-tile"></a>Overzichttegel oplossing
Nadat u de netwerk-Prestatiemeter-oplossing hebt ingeschakeld, biedt de tegel oplossing op de pagina overzicht van OMS een snel overzicht van de status van het netwerk. Een ringdiagram met het aantal in orde en slechte subnetwerkkoppelingen wordt weergegeven. Wanneer u klikt op de tegel, het dashboard van de oplossing geopend.

![Netwerk Prestatiemeter tegel](./media/log-analytics-network-performance-monitor/npm-tile.png)

### <a name="network-performance-monitor-solution-dashboard"></a>Dashboard van de oplossing netwerk Prestatiemeter
De **netwerk samenvatting** blade bevat een overzicht van de netwerken samen met de relatieve grootte. Dit wordt gevolgd door tegels totaal aantal netwerkkoppelingen, subnet koppelingen en paden worden weergegeven in het systeem (een pad bestaat uit de IP-adressen van de twee hosts met agents en alle hops tussen deze twee).

De **boven netwerk Health gebeurtenissen** blade bevat een lijst van de meest recente status gebeurtenissen en waarschuwingen in het systeem en de tijd sinds de gebeurtenis actief is geweest. Een statusgebeurtenis of een waarschuwing wordt gegenereerd wanneer de pakketten verloren gaan of de latentie van een koppeling met het netwerk of subnetwerk een drempelwaarde overschrijdt.

De **boven slechte netwerkkoppelingen** blade ziet u een lijst met slechte netwerkkoppelingen. Dit zijn de netwerkkoppelingen met een of meer nadelige statusgebeurtenis voor ze op dit moment.

De **boven Subnetwerkkoppelingen met het meeste verlies** en **Subnetwerkkoppelingen met de meeste latentie** blades de bovenste subnetwerkkoppelingen pakketverlies weergeven en subnetwerkkoppelingen respectievelijk top door latentie. Hoge latentie of een bepaalde hoeveelheid pakketverlies mogelijk op bepaalde netwerkkoppelingen worden verwacht. Deze koppelingen worden weergegeven in de top tien-lijsten, maar zijn niet gemarkeerd als beschadigd.

De **algemene query's** blade bevat een reeks zoekquery's ophalen van onbewerkte netwerk rechtstreeks met het bewaken van gegevens. U kunt deze query's als uitgangspunt gebruiken voor het maken van uw eigen query's voor aangepaste rapportage.

![Netwerk Prestatiemeter-dashboard](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="drill-down-for-depth"></a>Inzoomen voor diepte
U kunt klikken op verschillende koppelingen op de oplossing dashboard om inzoomen dieper in de gedeelten van belang. Bijvoorbeeld, wanneer er een waarschuwing of een onjuiste netwerkkoppeling worden weergegeven op het dashboard, kunt u deze verder onderzoeken. U geleid tot een pagina met een lijst met alle subnetwerkkoppelingen voor de koppeling bepaald netwerk. U wordt mogelijk de status van het gegevensverlies, latentie en de gezondheid van elke koppeling subnetwerk zien en snel uitzoeken wat subnetwerkkoppelingen het probleem worden veroorzaakt. U kunt vervolgens klikken op **knooppuntkoppelingen weergeven** voor een overzicht van alle knooppuntkoppelingen voor de koppeling slecht subnet. Vervolgens kunt u afzonderlijke knooppunt naar koppelingen weergeven en de slechte knooppuntkoppelingen vinden.

U kunt klikken op **weergave topologie** om weer te geven van de topologie hop door hop van de routes tussen de bron- en doelserver knooppunten. De slechte routes of hops zijn in rood weergegeven zodat u snel het probleem naar een bepaald deel van het netwerk kunt identificeren.

![Inzoomen-gegevens](./media/log-analytics-network-performance-monitor/npm-drill.png)

### <a name="network-state-recorder"></a>Netwerk status doos

Elke weergave toont een momentopname van de status van uw netwerk op een bepaald punt in tijd. Standaard zijn de meest recente status wordt weergegeven. De balk aan de bovenkant van de pagina geeft het punt in tijd waarvoor de status wordt weergegeven. U kunt teruggaan in tijd en weergeven van de momentopname van de status van uw netwerk door te klikken op de balk op **acties**. U kunt ook of automatisch vernieuwen voor alle pagina's uitschakelen terwijl u de meest recente status weergeven.

![de status van het netwerk](./media/log-analytics-network-performance-monitor/network-state.png)

#### <a name="trend-charts"></a>Van trendgrafieken
Op elk niveau dat u inzoomen, ziet u de trend van en netwerklatentie voor een netwerkverbinding. Van trendgrafieken zijn ook beschikbaar voor knooppunt en subnetwerk koppelingen. U kunt het interval voor de grafiek moet worden getekend met behulp van het besturingselement tijd aan het begin van de grafiek wijzigen.

Van trendgrafieken tonen een historisch perspectief van de prestaties van een netwerkverbinding. Sommige netwerkproblemen tijdelijke aard zijn en zijn moeilijk te vangen alleen door te kijken naar de huidige status van het netwerk. Dit is omdat problemen kunnen snel surface en verdwijnen voordat iedereen meldingen, alleen op een later tijdstip in de tijd weer. Dergelijke problemen van voorbijgaande aard kunnen ook lastig voor toepassingsbeheerders die vaak oppervlak als Onverklaarbaar toename in de reactietijd van toepassing, zelfs als alle toepassingsonderdelen weergegeven probleemloos uitgeeft.

Eenvoudig kunt u dit soort problemen detecteren door te kijken op een trend grafiek waar het probleem worden weergegeven als een plotselinge piek in de netwerklatentie of pakketverlies.

![trendgrafiek](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>hop door hop topologiekaart
Netwerk Prestatiemeter toont de topologie hop door hop van routes tussen twee knooppunten op een topologiekaart interactieve. U kunt de topologiekaart bekijken door een koppeling knooppunt selecteren en vervolgens te klikken op **weergave topologie**. U kunt ook de topologiekaart weergeven door te klikken op **paden** tegel op het dashboard. Wanneer u klikt op **paden** op het dashboard, hebt u de bron- en doelserver knooppunten in het Configuratiescherm van de linkerkant en klik vervolgens op **tekent** uitzetten van de routes tussen de twee knooppunten.

De topologiekaart toont het aantal routes tussen de twee knooppunten en wat zijn de gegevenspakketten nemen paden. Knelpunten in de prestaties zijn rood gemarkeerd in op de topologiekaart. U kunt een defecte netwerkverbinding of een defecte netwerkapparaat vinden door te kijken rode gekleurde elementen op de topologiekaart.

Wanneer u een knooppunt of aanwijzen eroverheen op de topologiekaart klikt, ziet u de eigenschappen van het knooppunt als FQDN-naam en IP-adres. Klik op een hop om te zien dat het IP-adres. U kunt bepaalde routes filteren met behulp van de filters in het deelvenster samenvouwbare actie. En kunt u ook de netwerktopologieën vereenvoudigen door het verbergen van de tussenliggende hops met behulp van de schuifregelaar in het actiedeelvenster. U kunt inzoomen in of buiten de topologiekaart met behulp van het muiswiel.

Houd er rekening mee dat de topologie die wordt weergegeven in de kaart laag 3-topologie is en bevat geen laag 2-apparaten en verbindingen.

![hop door hop topologiekaart](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Lokaliseren van fouten
Prestatiemeter-netwerk kan de netwerkknelpunten vinden zonder verbinding met de netwerkapparaten. Op basis van de gegevens die worden verzameld, vanaf het netwerk en door het toepassen van geavanceerde algoritmen in de grafiek netwerk, maakt de Prestatiemeter netwerk een probabilistische schatting van de onderdelen van het netwerk die waarschijnlijk de bron van het probleem.

Deze methode is handig voor het bepalen van de netwerkknelpunten wanneer toegang tot het aantal hops is niet beschikbaar omdat er geen gegevens worden verzameld van de netwerkapparaten zoals routers of switches niet nodig. Dit is ook handig wanneer de hops tussen twee knooppunten zich niet in uw beheer. Het aantal hops is bijvoorbeeld mogelijk ISP-routers.

### <a name="log-analytics-search"></a>Log Analytics zoeken
Alle gegevens die pagina's grafisch weergegeven via het netwerk-Prestatiemeter-dashboard en inzoomen is ook beschikbaar systeemeigen in logboekanalyse zoeken. U kunt met behulp van de zoekopdracht querytaal gegevens opvragen en aangepaste rapporten maken door de gegevens exporteren naar Excel of Power BI. De **algemene query's** blade in het dashboard bevat enkele handige query's die u als startpunt gebruiken kunt voor het maken van uw eigen query's en rapporten.

![zoekopdrachten](./media/log-analytics-network-performance-monitor/npm-queries.png)

## <a name="investigate-the-root-cause-of-a-health-alert"></a>De hoofdoorzaak van een health-waarschuwing onderzoeken
Nu dat u hebt gelezen over prestaties Netwerkcontrole, bekijken we een eenvoudige onderzoek naar de hoofdoorzaak van een health-gebeurtenis.

1. Op de pagina overzicht krijgt u een snelle momentopname van de status van uw netwerk met inachtneming van de **netwerk Prestatiemeter** tegel. U ziet dat buiten de 6 subnetwerken koppelingen worden bewaakt, 2 zijn slecht. Dit garandeert onderzoek. Klik op de tegel om het dashboard van de oplossing weer te geven.<br><br> ![Netwerk Prestatiemeter tegel](./media/log-analytics-network-performance-monitor/npm-investigation01.png)  
2. In de onderstaande voorbeeldafbeelding ziet u dat er is een statusgebeurtenis een netwerkkoppeling die niet in orde is. U wilt het probleem te onderzoeken en klik op de **DMZ2 DMZ1** netwerkkoppeling om erachter te komen de hoofdmap van het probleem.<br><br> ![Voorbeeld van een slecht netwerk](./media/log-analytics-network-performance-monitor/npm-investigation02.png)  
3. De pagina inzoomen toont alle subnetwerkkoppelingen **DMZ2 DMZ1** netwerkkoppeling. U zult zien dat voor beide de subnetwerkkoppelingen de latentie is gepasseerd de drempelwaarde voor de netwerkkoppeling slecht maken. U ziet ook de subnetwerkkoppelingen trends latentie. U kunt de Tijdselectie besturingselement in de grafiek te concentreren op de vereiste periode. Hier ziet u de tijd van de dag wanneer latentie de piek heeft bereikt. De logboeken voor deze periode voor het onderzoeken van het probleem kunt u later zoeken. Klik op **knooppuntkoppelingen weergeven** naar inzoomen verder.<br><br> ![Voorbeeld van slecht subnet koppelingen](./media/log-analytics-network-performance-monitor/npm-investigation03.png) 
4. Net als bij de vorige pagina, Inzoomen voor de koppeling bepaalde subnetwerk worden op deze pagina omlaag bijbehorende knooppuntkoppelingen samenstellende. U kunt vergelijkbare bewerkingen uitvoeren hier net als bij de vorige stap. Klik op **weergave topologie** om weer te geven van de topologie tussen de 2 knooppunten.<br><br> ![Voorbeeld van slecht knooppunten koppelingen](./media/log-analytics-network-performance-monitor/npm-investigation04.png)  
5. Alle paden tussen de 2 geselecteerde knooppunten worden getekend in de topologiekaart. U kunt de topologie hop door hop van routes tussen twee knooppunten op de topologiekaart visualiseren. Dit biedt u een duidelijk beeld van hoeveel routes bestaan tussen de twee knooppunten en wat de gegevenspakketten duurt paden. Knelpunten in de prestaties zijn in rood gemarkeerd. U kunt een defecte netwerkverbinding of een defecte netwerkapparaat vinden door te kijken rode gekleurde elementen op de topologiekaart.<br><br> ![Voorbeeld van slecht topologie weergeven](./media/log-analytics-network-performance-monitor/npm-investigation05.png)  
6. Het verlies, latentie en het aantal hops in elk pad kunnen worden gecontroleerd de **actie** deelvenster. Gebruik de schuifbalk om de details van die niet in orde paden weer te geven.  De filters gebruiken om de paden met de slechte hop selecteren zodat de topologie voor alleen de geselecteerde paden wordt getekend. Het muiswiel kunt u de topologiekaart in- of uitzoomen.

   In de onderstaande afbeelding duidelijk ziet u de hoofdoorzaak van de probleemgebieden naar de specifieke sectie van het netwerk door te kijken naar de paden en hops in rood. De eigenschappen van het knooppunt, met inbegrip van de FQDN-naam, te klikken op een knooppunt in de topologiekaart blijkt en IP-adres. Te klikken op een hop bevat het IP-adres van de hop.<br><br> ![slechte topologie - pad details voorbeeld](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="provide-feedback"></a>Feedback geven

- **UserVoice** -kunt u uw ideeën voor netwerk-Prestatiemeter-functies die u laten werken wilt op boeken. Ga naar onze [UserVoice pagina](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring).
- **Deelnemen aan onze cohort** -we altijd geïnteresseerd in nieuwe klanten ons cohort join hebben. Als onderdeel van het, past u vroege toegang krijgen tot nieuwe functies en helpt ons Netwerkcontrole prestaties te verbeteren. Als u geïnteresseerd in lid te worden bent, invullen van dit [Snelle enquête](https://aka.ms/npmcohort).

## <a name="next-steps"></a>Volgende stappen
* [Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde netwerk prestaties gegevensrecords weer te geven.
