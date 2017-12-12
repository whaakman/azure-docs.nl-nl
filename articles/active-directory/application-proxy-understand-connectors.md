---
title: Azure AD-toepassingsproxy connectors begrijpen | Microsoft Docs
description: Bevat informatie over de basisbeginselen van Azure AD-toepassingsproxy connectors.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: billmath
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: fe8d5c40249431be60dc8844adf7efa1b8e87c5f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="understand-azure-ad-application-proxy-connectors"></a>Azure AD-toepassingsproxy connectors begrijpen

Connectors zijn wat Azure AD-toepassingsproxy mogelijk maken. Dit zijn eenvoudige, eenvoudig te implementeren en onderhouden en super krachtige. In dit artikel wordt beschreven welke connectors zijn, hoe deze werken, en enkele suggesties voor het optimaliseren van uw implementatie. 

## <a name="what-is-an-application-proxy-connector"></a>Wat is een connector voor toepassingsproxy?

Verbindingslijnen zijn lichtgewicht agents die zich bevinden op lokale en het faciliteren van de uitgaande verbinding met de service voor toepassingsproxy. Connectors moeten worden geïnstalleerd op een Windows-Server die toegang tot de back-end-toepassing heeft. U kunt connectors indelen in groepen van de connector, met elke groep verwerken van verkeer voor specifieke toepassingen. Connectors verdelen automatisch, en kunt u de netwerkstructuur van uw optimaliseren. 

## <a name="requirements-and-deployment"></a>Vereisten en implementatie

Als u wilt implementeren toepassingsproxy is, moet u ten minste één connector, maar we raden twee of meer voor groter tolerantie. De connector installeren op een Windows Server 2012 R2 of 2016 machine. De connector moet communiceren met de service voor toepassingsproxy, evenals de on-premises toepassingen die u publiceert. 

Zie voor meer informatie over de netwerkvereisten voor de server-connector, [aan de slag met Application Proxy en het installeren van een connector](active-directory-application-proxy-enable.md).

## <a name="maintenance"></a>Onderhoud
De connectors en de service zorgt voor alle taken voor hoge beschikbaarheid. Ze kunnen worden toegevoegd of verwijderd dynamisch. Telkens wanneer die een nieuwe aanvraag ontvangt wordt het doorgestuurd naar een van de connectors die momenteel beschikbaar is. Als u een connector tijdelijk niet beschikbaar is, deze reageert niet op dit verkeer.

De connectors zijn staatloze en er geen configuratiegegevens op de machine. De enige gegevens die ze opslaan is de instellingen voor het verbinden van de service en het certificaat voor clientverificatie. Wanneer ze met de service verbinden, ze alle vereiste configuratiegegevens pull en elke paar minuten vernieuwd.

Connectors pollen ook de server te bepalen of er is een nieuwere versie van de connector. Als er een is gevonden, de connectors worden bijgewerkt.

U kunt uw connectors van de machine die ze worden uitgevoerd, controleren met behulp van het gebeurtenislogboek en de prestatiemeteritems. Of u kunt de status van de pagina Application Proxy van de Azure portal weergeven:

 ![AzureAD Application Proxy Connectors](./media/application-proxy-understand-connectors/app-proxy-connectors.png)

U hoeft niet te handmatig verwijderen van connectors die niet gebruikt worden. Wanneer een connector wordt uitgevoerd, blijft het actief als deze verbinding met de service maakt. Ongebruikte connectors zijn gelabeld als _inactieve_ en na 10 dagen inactief zijn verwijderd. Als u verwijderen van een connector wilt, verwijdert u zowel de connectorservice en de Updaterservice van de server. Start de computer opnieuw op om de service volledig te verwijderen.

## <a name="automatic-updates"></a>Automatische updates

Azure AD levert de automatische updates voor alle connectors die u implementeert. Als de service Application Proxy Connector Updater wordt uitgevoerd, wordt uw connectors automatisch bijgewerkt. Als de Connector Updater-service niet wordt weergegeven op uw server, moet u [de connector opnieuw installeren](active-directory-application-proxy-enable.md) om eventuele updates te downloaden. 

Als u niet wilt wachten tot een automatische update te komen met de connector, kunt u een handmatige upgrade kunt uitvoeren. Ga naar de [connector downloadpagina](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download) op de server waarop de connector zich bevindt en selecteer **downloaden**. Dit proces is serversysteemstatus van een upgrade voor de lokale connector. 

Voor tenants met meerdere connectors richten automatische updates één connector tegelijk in elke groep om te voorkomen dat de downtime in uw omgeving. 

U kunt uitvaltijd kan optreden wanneer uw connector bijgewerkt als:  
- U hoeft slechts één connector. Als u wilt deze uitvaltijd te voorkomen en de hoge beschikbaarheid te verbeteren, wordt aangeraden een tweede connector te installeren en [maakt u een groep connector](active-directory-application-proxy-connectors-azure-portal.md).  
- Er is een verbindingslijn in het midden van een transactie aanvang van de update. Hoewel de initiële transactie verloren gegaan is, moet uw browser automatisch of probeer het opnieuw kunt u uw pagina vernieuwen. Wanneer de aanvraag is verzonden, wordt het verkeer wordt doorgestuurd naar een back-connector.

## <a name="creating-connector-groups"></a>Connector-groepen maken

Connector-groepen kunnen u specifieke connectors voor specifieke toepassingen toewijzen. U kunt een aantal connectors te groeperen en vervolgens elke toepassing toewijzen aan een groep. 

Connector-groepen kunnen gemakkelijker grote implementaties beheren. Bovendien de latentie voor tenants die toepassingen die worden gehost in verschillende regio's, omdat u groepen om in te dienen alleen lokale toepassingen op basis van locatie-connector kunt maken. 

Zie voor meer informatie over groepen connector, [publiceren van toepassingen op afzonderlijke netwerken en locaties met connector groepen](active-directory-application-proxy-connectors-azure-portal.md).

## <a name="capacity-planning"></a>Capaciteitsplanning 

Terwijl connectors wordt automatisch taakverdeling binnen een connector-groep, is het ook belangrijk om ervoor te zorgen dat u hebt gepland voldoende capaciteit tussen connectors voor het afhandelen van de omvang van het verwachte verkeer. In het algemeen de meer gebruikers die u hebt een machine die u moet de grootste. Hieronder vindt u een tabel geeft een overzicht van het volume andere machines kan verwerken. Houd er rekening mee is alle gebaseerd op verwachte transacties Per tweede (TPS) in plaats van door gebruiker sinds gebruik patronen variëren en kunnen niet worden gebruikt om te voorspellen laden.  Rekening mee dat er enkele verschillen op basis van de grootte van de antwoorden en de reactietijd van de back-end-toepassing zijn-grotere antwoord en tragere responstijden in een lagere maximale TPS resulteert.

|Cores|RAM|Latentie (MS) verwacht-P99|Maximale TPS|
| ----- | ----- | ----- | ----- |
|2|8|325|586|
|4|16|320|1150|
|8|32|270|1190|
|16|64|245|1200*|
\*Deze computer heeft een verbindingslimiet van 800. We gebruiken de standaard 200 verbindingslimiet voor alle andere machines.
 
>[!NOTE]
>Er is veel verschil in de maximale TPS tussen 4, 8 en 16 core-machines. Het belangrijkste verschil tussen die zich in de verwachte latentie.  

## <a name="security-and-networking"></a>Beveiliging en netwerken

Connectors kunnen overal worden geïnstalleerd op het netwerk waarmee ze verzenden van aanvragen naar de service-toepassingsproxy. Wat is het belangrijk is dat de computer waarop de connector ook uitgevoerd toegang tot uw apps heeft. U kunt connectors installeren binnen uw bedrijfsnetwerk of op een virtuele machine die wordt uitgevoerd in de cloud. Connectors kunnen worden uitgevoerd binnen een gedemilitariseerde zone (DMZ), maar het is niet nodig omdat het al het verkeer uitgaand is, zodat uw netwerk beveiligd blijft.

Connectors alleen verzenden uitgaande aanvragen. Het uitgaande verkeer wordt verzonden naar de service Application Proxy en gepubliceerde toepassingen. U hoeft niet te openen van poorten voor inkomend verkeer omdat beide manieren verkeersstromen wanneer een sessie tot stand is gebracht. U hoeft niet te u taakverdeling tussen de connectors instelt of binnenkomende toegang via uw firewalls configureren. 

Zie voor meer informatie over het configureren van uitgaande firewallregels [werken met bestaande lokale proxyservers](application-proxy-working-with-proxy-servers.md).

Gebruik de [Azure AD Application Proxy Connector poorten hulpprogramma Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) om te controleren of de connector de service voor toepassingsproxy kan bereiken. Ten minste Zorg ervoor dat de regio VS-midden en de regio die het dichtst bij u alle een groen vinkje. Daarna betekent meer een groen vinkje groter tolerantie. 

## <a name="performance-and-scalability"></a>Prestaties en schaalbaarheid

Schaal voor de service voor toepassingsproxy is transparant, maar de schaal is een factor voor connectors. U moet voldoende connectoren piekverkeer dan verwerken. U hoeft echter niet te configureren van taakverdeling omdat alle connectors binnen een connector-groep automatisch worden verdeeld.

Aangezien connectors staatloze, zijn ze niet beïnvloed door het aantal gebruikers of -sessies. In plaats daarvan ze niet reageren op het aantal aanvragen en hun nettolading. Met standaard internetverkeer, kan een gemiddelde machine enkele duizenden aanvragen per seconde verwerken. De opgegeven capaciteit is afhankelijk van de exacte machine-kenmerken. 

De connectorprestaties is gebonden aan CPU- en netwerken. CPU-prestaties is vereist voor SSL-versleuteling en ontsleuteling, terwijl netwerken is het belangrijk om snelle verbinding met de toepassingen en de online-service in Azure.

Daarentegen is geheugen kleiner van een probleem voor connectors. De onlineservice zorgt voor veel van de verwerking en alle niet-geverifieerd verkeer. Alles die kan worden uitgevoerd in de cloud wordt uitgevoerd in de cloud. 

De taakverdeling gebeurt tussen connectors van een gegeven connector-groep. We doen een variant van een round robin om te bepalen welke connector in de groep fungeert een bepaald verzoek. Nadat u hebt gekozen een de connector onderhouden we een sessie affiniteit tussen gebruiker en de toepassing voor de duur van de sessie. Als voor een of andere reden connector of de machine niet beschikbaar, gaat het verkeer naar een andere connector in de groep. Deze tolerantie is ook waarom kunt het beste meerdere connectors.

Een andere factor die van invloed op prestaties wordt de kwaliteit van het netwerk tussen de connectors, inclusief: 

* **De onlineservice**: trage of hoge latentie verbindingen naar de toepassingsproxy-service in Azure invloed van de connectorprestaties. Verbinding maken uw organisatie in Azure met Express Route voor de beste prestaties. Anders hebben uw netwerken team ervoor te zorgen dat de verbindingen naar Azure zo efficiënt mogelijk worden verwerkt. 
* **De back-end-toepassingen**: In sommige gevallen zijn aanvullende proxy's tussen de connector en de back-end-toepassingen die kunnen trage of verbindingen voorkomen. Open een browser van de connectorserver en probeer het toegang tot de toepassing voor het oplossen van dit scenario. Als u de connectors in Azure uitvoeren, maar de toepassingen die zich on-premises, de ervaring mogelijk niet wat uw gebruikers verwachten.
* **De domeincontrollers**: als de connectors met behulp van Kerberos-beperkte overdracht SSO uitvoeren, ze contact op met de domeincontrollers voordat de aanvraag wordt verzonden naar de back-end. De connectors een cache van Kerberos-tickets hebben, maar in een omgeving met Bezig de reactietijd van de domeincontrollers kan invloed hebben op prestaties. Dit probleem is vaker voor connectors die worden uitgevoerd in Azure, maar communicatie met domeincontrollers die zich on-premises. 

Zie voor meer informatie over het optimaliseren van uw netwerk [aandachtspunten voor topologie netwerk bij gebruik van Azure Active Directory-toepassingsproxy](application-proxy-network-topology-considerations.md).

## <a name="domain-joining"></a>Lid worden van domein

Connectors kunnen uitvoeren op een computer die is geen lid van een domein. Als u eenmalige aanmelding (SSO) voor toepassingen die gebruikmaken van geïntegreerde Windows-verificatie (IWA) wilt, moet u een domein-machine. In dit geval de machines connector moeten worden toegevoegd aan een domein kan uitvoeren [Kerberos](https://web.mit.edu/kerberos) beperkte overdracht namens de gebruikers voor de gepubliceerde toepassingen.

Connectors kunnen ook worden gekoppeld, domeinen of forests die u een gedeeltelijk vertrouwen hebt of alleen-lezen domeincontrollers.

## <a name="connector-deployments-on-hardened-environments"></a>Connector-implementaties op beperkte omgevingen

Normaal gesproken implementatie van de connector is eenvoudig en is geen speciale configuratie vereist. Er zijn echter een aantal unieke voorwaarden die u moeten overwegen:

* Organisaties die het uitgaande verkeer beperkt moeten [vereiste poorten openen](active-directory-application-proxy-enable.md#open-your-ports).
* FIPS-compatibele computers kunnen worden verplicht om hun configuratie zodat de connector-processen voor het genereren en opslaan van een certificaat te wijzigen.
* Organisaties die hun omgeving op basis van de processen die de netwerkaanvragen verlenen vergrendelen hebben om ervoor te zorgen dat beide connectorservices voor toegang tot alle vereiste poorten en IP-adressen zijn ingeschakeld.
* In sommige gevallen uitgaande forward proxy's de wederzijdse certificaatverificatie opsplitsen en ervoor zorgen dat de communicatie mislukt.

## <a name="connector-authentication"></a>Connector-verificatie

Een beveiligde om service te bieden, connectors hebben om te verifiëren naar de service en de service heeft om te verifiëren naar de connector. Deze verificatie wordt gedaan met behulp van certificaten van client en server wanneer de connectors de verbinding initieert. Op deze manier de gebruikersnaam en het wachtwoord van de beheerder worden niet opgeslagen op de connector-machine.

De certificaten die worden gebruikt, zijn specifiek voor de service-toepassingsproxy. Ze worden gemaakt tijdens de initiële registratie en worden automatisch vernieuwd door de connectors elke aantal maanden. 

Als een connector niet is verbonden met de service voor meerdere maanden, zijn de certificaten verouderd. In dit geval verwijderen en opnieuw installeren van de connector triggerregistratie. U kunt de volgende PowerShell-opdrachten kunt uitvoeren:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

## <a name="under-the-hood"></a>Onderhuids

Connectors zijn gebaseerd op Windows Server Web Application Proxy, zodat ze zijn de meeste van de dezelfde beheertools, waaronder Windows-gebeurtenislogboeken

 ![Gebeurtenislogboeken met logboeken beheren](./media/application-proxy-understand-connectors/event-view-window.png)

en Windows-prestatiemeteritems. 

 ![Items toevoegen aan de connector met de Prestatiemeter](./media/application-proxy-understand-connectors/performance-monitor.png)

De connectors admin en sessie hebt Logboeken. De beheerlogboeken bevatten belangrijke gebeurtenissen en hun fouten. De sessielogboeken bevatten alle transacties en de bijbehorende verwerkingsgegevens. 

De logboeken, Ga naar de Event Viewer, open de **weergave** menu en schakel **logboeken en foutopsporing weergeven analytische**. Schakel vervolgens deze begint met het verzamelen van gebeurtenissen. Deze logboeken worden niet weergegeven in de Web Application Proxy in Windows Server 2012 R2, zoals de connectors zijn gebaseerd op een meer recente versie.

U kunt de status van de service in het venster Services controleren. De connector bestaat uit twee Windows-Services: de werkelijke connector en de updater. Beide parameters moeten de tijd worden uitgevoerd.

 ![AzureAD Services lokale](./media/application-proxy-understand-connectors/aad-connector-services.png)

## <a name="next-steps"></a>Volgende stappen


* [Publiceren van toepassingen op afzonderlijke netwerken en locaties met groepen van de connector](active-directory-application-proxy-connectors-azure-portal.md)
* [Werken met bestaande lokale proxyservers](application-proxy-working-with-proxy-servers.md)
* [Toepassingsproxy en connector fouten oplossen](active-directory-application-proxy-troubleshoot.md)
* [Het installeren van de Azure AD Application Proxy Connector achtergrond](active-directory-application-proxy-silent-installation.md)

