---
title: Meer informatie over Azure AD Application Proxy connectors | Microsoft Docs
description: Bevat informatie over de basisbeginselen van Azure AD Application Proxy connectors.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: barbkess
ms.reviewer: japere
ms.openlocfilehash: 8b410cc85584f45d4a3e9d7bce180a2c6aa46114
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53134963"
---
# <a name="understand-azure-ad-application-proxy-connectors"></a>Meer informatie over Azure AD Application Proxy connectors

Connectors zijn wat Azure AD Application Proxy mogelijk maken. Ze zijn eenvoudig, gemakkelijk te implementeren en te onderhouden en heel krachtige. In dit artikel wordt beschreven welke connectors zijn, hoe deze werken, en enkele suggesties voor informatie over het optimaliseren van uw implementatie. 

## <a name="what-is-an-application-proxy-connector"></a>Wat is er een Application Proxy-connector?

Connectors zijn lichtgewicht agents die zich on-premises en de uitgaande verbinding met de service voor toepassingsproxy te vergemakkelijken. Connectors moeten worden geïnstalleerd op een Windows-Server die toegang tot de back endtoepassing heeft. U kunt connectors indelen in connectorgroepen, met elke groep verwerken van verkeer voor specifieke toepassingen. Connectors taakverdeling automatisch, en kunnen helpen met het optimaliseren van de netwerkstructuur van uw. 

## <a name="requirements-and-deployment"></a>Vereisten en implementatie

Voor het implementeren van de toepassingsproxy is, moet u ten minste één connector, maar we raden aan twee of meer voor meer flexibiliteit. De connector installeren op een Windows Server 2012 R2 of 2016-machine. De connector moet kunnen communiceren met de service voor toepassingsproxy, evenals de on-premises toepassingen die u publiceert. 

### <a name="windows-server"></a>Windows server
U moet een server met Windows Server 2012 R2 of hoger waarop kunt u de Application Proxy-connector installeren. De server moet verbinding maken met de services voor toepassingsproxy in Azure en de on-premises toepassingen die u publiceert.

De windows-server moet TLS 1.2 is ingeschakeld voordat u de Application Proxy-connector installeert. Bestaande connectors met een versie lager dan 1.5.612.0 blijven werken in eerdere versies van TLS tot nader order van kracht. TLS 1.2 inschakelen:

1. De volgende registersleutels instellen:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

2. De server opnieuw opstarten


Zie voor meer informatie over de netwerkvereisten voor de connectorserver, [aan de slag met Application Proxy en installeren van een connector](application-proxy-add-on-premises-application.md).

## <a name="maintenance"></a>Onderhoud
De connectors en de service zorgen van alle taken die hoge beschikbaarheid. Ze kunnen worden toegevoegd of verwijderd dynamisch. Telkens wanneer die een nieuwe aanvraag binnenkomt wordt deze doorgestuurd naar een van de connectors die momenteel beschikbaar is. Als een connector tijdelijk niet beschikbaar is, heeft niet deze dit verkeer reageren.

De connectors zijn staatloos en geen configuratiegegevens op de computer hebt. De enige gegevens die ze opslaan is de instellingen voor het verbinden van de service en het certificaat voor clientverificatie. Wanneer ze met de service verbinden, ze alle vereiste configuratiegegevens ophalen en elke paar minuten vernieuwd.

Connectors pollen ook de server om erachter te komen of er is een nieuwere versie van de connector. Als er een is gevonden, de connectors worden bijgewerkt.

U kunt uw connectors van de machine waarop ze worden uitgevoerd, controleren met behulp van het gebeurtenislogboek en de prestatiemeteritems. Of u kunt de status van de Application Proxy-pagina van de Azure-portal weergeven:

 ![AzureAD Toepassingsproxyconnectors](./media/application-proxy-connectors/app-proxy-connectors.png)

U hebt geen handmatig verwijderen van connectors die niet gebruikt worden. Wanneer een connector wordt uitgevoerd, blijft deze actief als deze verbinding met de service maakt. Niet-gebruikte connectors zijn gelabeld als _inactieve_ en worden verwijderd na tien dagen van inactiviteit. Als u verwijderen van een connector wilt, maar dan wel verwijdert u zowel de connectorservice en de Updaterservice van de server. Start de computer opnieuw op om de service volledig te verwijderen.

## <a name="automatic-updates"></a>Automatische updates

Azure AD biedt Automatische updates voor alle connectors die u implementeert. Als de Application Proxy Connector Updater-service wordt uitgevoerd, wordt uw connectors automatisch bijgewerkt. Als u niet de Connector Updater-service op uw server ziet, moet u [uw connector opnieuw installeren](application-proxy-add-on-premises-application.md) om eventuele updates te downloaden. 

Als u niet wilt wachten tot een automatische update te komen aan uw connector, kunt u een handmatige upgrade kunt uitvoeren. Ga naar de [connector-downloadpagina](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download) op de server waarop de connector zich bevindt en selecteert u een **downloaden**. Een upgrade voor de lokale connector begint dit proces. 

Voor tenants met meerdere connectors gericht op de automatische updates één connector op een tijdstip in elke groep om te voorkomen dat downtime in uw omgeving. 

U ondervindt mogelijk downtime tijdens uw connector bijgewerkt als:  
- U hoeft slechts één connector. Deze uitvaltijd te voorkomen en de hoge beschikbaarheid te verbeteren, raden we u een tweede connector installeren en [maken van een connectorgroep](application-proxy-connector-groups.md).  
- Er is een connector in het midden van een transactie wanneer de update is begonnen. Hoewel de initiële transactie verloren gegaan is, uw browser moet automatisch probeer het opnieuw of u kunt de pagina vernieuwen. Wanneer de aanvraag is verzonden, wordt het verkeer wordt doorgestuurd naar een back-connector.

## <a name="creating-connector-groups"></a>Het maken van connectorgroepen

Connectorgroepen kunnen u specifieke connectors voor het bieden van specifieke toepassingen toewijzen. U kunt een aantal connectors groeperen en vervolgens elke toepassing toewijzen aan een groep. 

Connectorgroepen wordt het eenvoudiger voor het beheren van grote implementaties. Ze ook verbeteren latentie voor tenants met toepassingen die worden gehost in verschillende regio's, omdat u kunt maken op basis van locatie connectorgroepen om van dienst alleen lokale toepassingen te. 

Zie voor meer informatie over connectorgroepen [publiceren van toepassingen op afzonderlijke netwerken en locaties met connectorgroepen](application-proxy-connector-groups.md).

## <a name="capacity-planning"></a>Capaciteitsplanning 

Terwijl connectors wordt automatisch taakverdeling binnen een connectorgroep, is het ook belangrijk om te controleren of dat u voldoende capaciteit tussen connectors voor het afhandelen van het verwachte verkeersvolume hebt gepland. In het algemeen, des te meer gebruikers hebt, een machine die u moet de groter. Hieronder vindt u een tabel geeft een overzicht van het volume andere machines kan worden verwerkt. Houd er rekening mee is alle gebaseerd op verwachte transacties Per seconde (TPS) in plaats van door gebruiker sinds gebruik patronen variëren en kunnen niet worden gebruikt om te voorspellen laden.  Let ook op dat er enkele verschillen op basis van de grootte van de antwoorden en de reactietijd van de back-end-toepassing zijn-grotere reactieformaat en langzamer reactietijden tot een lagere maximale TPS leiden zal.

|Kerngeheugens|RAM|Verwachte latentie (MS)-P99|Max TPS|
| ----- | ----- | ----- | ----- |
|2|8|325|586|
|4|16|320|1150|
|8|32|270|1190|
|16|64|245|1200*|
\* Deze machine gebruikt een aangepaste instelling om te verheffen enkele van de standaardlimieten voor de verbinding dan de aanbevolen instellingen voor .net. Het is raadzaam om een test uitgevoerd met de standaardinstellingen voor het contact opnemen met ondersteuning om deze limiet voor uw tenant is gewijzigd.
 
>[!NOTE]
>Er is veel prioriteitsbereik bij met de maximale TPS 4, 8 en 16 kernen-machines. Het belangrijkste verschil tussen die zich in de verwachte latentie.  

## <a name="security-and-networking"></a>Beveiliging en netwerken

Connectors kunnen overal worden geïnstalleerd op het netwerk waarmee ze voor het verzenden van aanvragen naar de Application Proxy-service. Wat is het belangrijk is dat de computer met de connector ook toegang tot uw apps. U kunt connectors installeren op uw bedrijfsnetwerk of op een virtuele machine die wordt uitgevoerd in de cloud. Connectors kunnen binnen een gedemilitariseerde zone (DMZ) uitgevoerd, maar het is niet nodig omdat al het verkeer uitgaand, zodat uw netwerk optimaal beveiligd blijft.

Connectors alleen verzenden uitgaande aanvragen. Het uitgaande verkeer wordt verzonden naar de Application Proxy-service en de gepubliceerde toepassingen. U hebt geen inkomende poorten openen omdat verkeer beide richtingen stroomt wanneer een sessie tot stand is gebracht. U hebt geen taakverdeling tussen de connectors instellen of binnenkomende toegang via uw firewalls configureren. 

Zie voor meer informatie over het configureren van uitgaande firewallregels [werken met bestaande on-premises proxy-servers](application-proxy-configure-connectors-with-proxy-servers.md).


## <a name="performance-and-scalability"></a>Prestaties en schaalbaarheid

Schaal voor de service voor toepassingsproxy is transparant, maar de schaal is een factor voor connectors. U moet voldoende connectors voor het afhandelen van verkeer. U hoeft echter te configureren van taakverdeling, omdat alle connectors binnen een connectorgroep automatisch worden verdeeld.

Aangezien connectors stateless, worden ze niet beïnvloed door het aantal gebruikers en sessies. In plaats daarvan reageren ze op het aantal aanvragen en de grootte van de nettolading. Met standaard webverkeer te genereren, kan een gemiddelde virtuele machine een paar duizend aanvragen per seconde verwerken. De opgegeven capaciteit is afhankelijk van de kenmerken van de exacte machine. 

De connectorprestaties is gebonden aan CPU- en netwerkresources. CPU-prestaties nodig is voor SSL-versleuteling en ontsleuteling, terwijl de netwerken is het belangrijk om snelle verbinding met de toepassingen en de online-service in Azure.

Daarentegen is geheugen kleiner van een probleem voor connectors. De online-service zorgt voor veel van de verwerking en niet-geverifieerde al het verkeer. Alles dat kan worden uitgevoerd in de cloud wordt uitgevoerd in de cloud. 

De load balancing plaatsvindt tussen connectors van een gegeven connector-groep. We doen een variant van een round-robin om te bepalen welke connector in de groep een bepaalde aanvraag fungeert. Als voor een bepaalde reden dat connector of de machine niet meer beschikbaar is, het verkeer naar een andere connector in de groep gaat. Deze flexibiliteit is ook waarom we raden u aan meerdere connectors.

Een andere factor die van invloed op prestaties wordt de kwaliteit van de netwerken tussen de connectors, met inbegrip van: 

* **De onlineservice**: traag of hoge latentie verbindingen met de Application Proxy-service in Azure invloed van de connectorprestaties. Voor de beste prestaties, verbinding maken met uw organisatie naar Azure met Express Route. Anders hebt u uw netwerken team ervoor te zorgen dat de verbindingen met Azure zo efficiënt mogelijk worden verwerkt. 
* **De back-endtoepassingen**: In sommige gevallen moet u er extra proxy tussen de connector en de back endtoepassingen die kunnen vertragen of te voorkomen dat de verbindingen zijn. Open een browser van de connectorserver voor het oplossen van dit scenario, en probeert te krijgen tot de toepassing. Als u de connectors in Azure uitvoert, maar de toepassingen die zich on-premises, de ervaring mogelijk niet wat uw gebruikers verwachten.
* **De domeincontrollers**: als de connectors met behulp van Kerberos-beperkte overdracht SSO uitvoeren, ze contact op met de domeincontrollers voordat de aanvraag wordt verzonden naar de back-end. De connectors hebben een cache van Kerberos-tickets, maar in een omgeving met Bezig de reactietijd van de domeincontrollers kan beïnvloeden. Dit probleem komt vaker voor voor connectors die worden uitgevoerd in Azure, maar communiceren met domeincontrollers die zich on-premises. 

Zie voor meer informatie over het optimaliseren van uw netwerk [topologie overwegingen met betrekking tot het netwerk bij het gebruik van Azure Active Directory-toepassingsproxy](application-proxy-network-topology.md).

## <a name="domain-joining"></a>Toevoegen aan het domein

Connectors kunnen uitvoeren op een computer die is niet-domein. Als u eenmalige aanmelding (SSO) voor toepassingen die gebruikmaken van geïntegreerde Windows-verificatie (IWA) wilt, moet u echter een domein-machine. In dit geval de connector-machines moeten worden toegevoegd aan een domein dat u kunt uitvoeren [Kerberos](https://web.mit.edu/kerberos) beperkte delegering namens de gebruikers voor de gepubliceerde toepassingen.

Connectors kunnen ook worden gekoppeld, domeinen of forests die u een gedeeltelijk vertrouwen hebt, of alleen-lezen domeincontrollers.

## <a name="connector-deployments-on-hardened-environments"></a>Connector-implementaties op beveiligde omgevingen

Implementatie van de connector wordt normaal gesproken is eenvoudig en is geen speciale configuratie vereist. Er zijn echter enkele unieke voorwaarden die u moeten overwegen:

* Organisaties die het uitgaande verkeer beperken moeten [vereiste poorten openen](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment).
* FIPS-compatibele computers kunnen worden verplicht om hun configuratie zodat de connector-processen voor het genereren en opslaan van een certificaat te wijzigen.
* Organisaties die hun omgeving op basis van de processen die het netwerk aanvragen vergrendelen hebben om ervoor te zorgen dat beide connectorservices voor toegang tot alle vereiste poorten en IP-adressen zijn ingeschakeld.
* In sommige gevallen kunnen uitgaande forward proxy's de wederzijdse certificaatverificatie opsplitsen en ervoor zorgen dat de communicatie mislukt.

## <a name="connector-authentication"></a>Connector-verificatie

Voor een veilige service connectors hebben om te verifiëren en de service en de service is om te verifiëren naar de connector. Deze verificatie wordt gedaan met behulp van de client en server certificaten wanneer de connectors de verbinding initieert. Op deze manier de gebruikersnaam en het wachtwoord van de beheerder worden niet opgeslagen op de computer van de connector.

De certificaten die worden gebruikt, zijn specifiek voor de Application Proxy-service. Ze worden gemaakt tijdens de registratie en worden automatisch vernieuwd door de connectors elke aantal maanden. 

Als u een connector is niet verbonden met de service voor enkele maanden, is het mogelijk dat de certificaten verouderd. In dit geval verwijderen en opnieuw installeren van de connector triggerregistratie. U kunt de volgende PowerShell-opdrachten uitvoeren:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

## <a name="under-the-hood"></a>Achter de schermen

Connectors zijn gebaseerd op Windows Server Web Application Proxy, zodat ze hebben de meeste van de dezelfde beheerhulpprogramma's waaronder Windows-gebeurtenislogboeken

 ![Logboeken met de Event Viewer beheren](./media/application-proxy-connectors/event-view-window.png)

en Windows-prestatiemeteritems. 

 ![Items toevoegen aan de connector met de Prestatiemeter](./media/application-proxy-connectors/performance-monitor.png)

De connectors zijn zowel de beheerder als de sessie Logboeken. De beheerder-logboeken bevatten belangrijke gebeurtenissen en hun fouten. De sessielogboeken bevatten alle transacties en de bijbehorende verwerkingsgegevens. 

Als u wilt de logboeken ziet, gaat u naar de Event Viewer, open de **weergave** menu en schakelt u **logboeken en foutopsporing weergeven analytische**. Vervolgens kunnen ze beginnen met het verzamelen van gebeurtenissen. Deze logboeken worden niet weergegeven in de Web Application Proxy in Windows Server 2012 R2, zoals de connectors zijn gebaseerd op een meer recente versie.

U kunt de status van de service in het venster Services controleren. De connector bestaat uit twee Windows-Services: de werkelijke connector en de updater. Van beide moeten altijd worden uitgevoerd.

 ![AzureAD Services lokaal](./media/application-proxy-connectors/aad-connector-services.png)

## <a name="next-steps"></a>Volgende stappen


* [Publiceren van toepassingen op afzonderlijke netwerken en locaties met behulp van connectorgroepen](application-proxy-connector-groups.md)
* [Werken met bestaande on-premises proxy-servers](application-proxy-configure-connectors-with-proxy-servers.md)
* [Application Proxy en de connector-fouten oplossen](application-proxy-troubleshoot.md)
* [De Azure AD Application Proxy Connector op de achtergrond installeren](application-proxy-register-connector-powershell.md)

