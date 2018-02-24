---
title: Netwerk-Prestatiemeter-oplossing in Azure Log Analytics | Microsoft Docs
description: De Service-eindpunt beheer in Prestatiemeter netwerk mogelijkheid voor het bewaken van de netwerkverbinding met een willekeurig eindpunt met een open TCP-poort.
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
ms.openlocfilehash: 7eb31b91480b6e57135581cfa2f5503de3189e10
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="service-endpoint-manager"></a>Eindpunt van servicebeheer

De mogelijkheden voor Service-eindpunt beheer in [netwerk Prestatiemeter](log-analytics-network-performance-monitor.md) kunt u netwerkverbinding met een willekeurig eindpunt met een open TCP-poort te bewaken. Dergelijke eindpunten zijn websites, SaaS-toepassingen, PaaS-toepassingen en SQL-databases. 

U kunt de volgende functies met uitvoeren **Monitor voor Service-eindpunt**: 

- De netwerkverbinding tot uw toepassingen en services van het netwerk (zoals Office 365, Dynamics CRM, interne line-of-business-toepassingen, SQL-database, enzovoort) bewaken vanaf meerdere kantoren/filialen 
- Ingebouwde tests voor het bewaken van de netwerkverbinding met Office365 en Dynamics365 eindpunten 
- De reactietijd, netwerklatentie, pakketverlies is opgetreden bij het verbinden met het eindpunt bepalen 
- Bepalen of slechte prestaties vanwege het netwerk of vanwege een probleem aan einde van de aanbieder van de toepassing 
- Identificeer hotspots op het netwerk die kan worden veroorzaakt door slechte prestaties door de latentie die is bijgedragen door elke hop op een topologiekaart weer te geven. 


![Monitor voor service-eindpunt](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Configuratie 
Open de configuratie voor netwerk-Prestatiemeter te openen de [netwerk Prestatiemeter oplossing](log-analytics-network-performance-monitor.md) en klik op de **configureren** knop.

![Prestatiemeter netwerk configureren](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-oms-agents-for-the-monitoring"></a>OMS-agents configureren voor de bewaking.  
Schakel de volgende firewallregels op de knooppunten die wordt gebruikt voor het bewaken van zodat de oplossing kan de topologie van uw knooppunten met de service-eindpunt detecteren: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action 
```

### <a name="create-service-endpoint-monitor-tests"></a>Monitor voor Service-eindpunt tests maken 

Beginnen met het maken van uw tests uit om te controleren van de netwerkverbinding met de service-eindpunten 

1. Klik op de **Monitor voor Service-eindpunt** tabblad.
2. Klik op **Test toevoegen** en voer de naam van Test en de beschrijving. 
3. Selecteer het type van test:<br>Selecteer **WebTest** als u verbinding met een service die op HTTP/S-aanvragen, zoals outlook.office365.com, bing.com reageert bewaakt.<br>Selecteer **Netwerktest** als u een verbinding met een service die op TCP-aanvraag wordt gereageerd, maar niet reageert op HTTP/S-aanvraag, zoals een SQL-server, controleert FTP-server, een SSH-poort enzovoort. 
4. Als u niet wilt uitvoeren netwerk metingen (netwerklatentie, pakketverlies, topologie-detectie), schakelt u het tekstvak. We adviseren u deze gecontroleerd om te profiteren van de mogelijkheid krijgen. 
5. Geef het doel FQDN-URL/IP-adres waarnaar u wilt bewaken van de verbinding met het netwerk.  
6. Voer het poortnummer van de doelservice. 
7. Voer de frequentie die u wilt dat de test uit te voeren. 
8. Selecteer de knooppunten van waaruit u wilt bewaken van de netwerkverbinding met de service. 

    >[!NOTE]
    > Voor Windows server-knooppunten gebruikt de mogelijkheid TCP-aanvragen uit te voeren van de metingen van het netwerk. Voor Windows client-knooppunten gebruikt de mogelijkheid ICMP-aanvragen uit te voeren van de metingen van het netwerk. In sommige gevallen blokkeert de doeltoepassing binnenkomende op basis van ICMP-aanvraag vanwege die bij de knooppunten Windows client-gebaseerde, zijn de oplossing kan niet is worden uitgevoerd netwerk metingen. Daarom is het aanbevolen gebruik van Windows server gebaseerde knooppunten in dergelijke gevallen. 

9. Als u niet wilt maken van de gezondheid van gebeurtenissen voor de items die u hebt geselecteerd, klikt u vervolgens wissen **inschakelen statuscontrole op de doelen wordt gedekt door deze test**. 
10. Kies de bewaking van de voorwaarden. U kunt aangepaste drempelwaarden instellen voor het genereren van gebeurtenis health door drempelwaarden in te voeren. Wanneer de waarde van de voorwaarde hoger dan de geselecteerde drempelwaarde voor het geselecteerde netwerk/subnetwerk paar is, wordt een health-gebeurtenis gegenereerd. 
11. Klik op **opslaan** aan de configuratie op te slaan. 

 ![Service-eindpunt monitorconfiguratie](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Walkthrough 

Verplaatsen naar de weergave dashboard Netwerkbewaking voor prestaties en bekijk de **Monitor voor Service-eindpunt** pagina om een samenvatting van de status van de verschillende tests die u hebt gemaakt.  

![Monitor pagina van de service-eindpunt](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Klik op de tegel voor inzoomen en de details van de tests weergeven op de **Tests** pagina. U kunt de status van de punt in tijd en de waarde van de reactietijd van de service, netwerklatentie en pakketverlies voor alle tests weergeven in de tabel LHS. U kunt het netwerk staat doos besturingselement om weer te geven van de momentopname van het netwerk op een andere tijd in het verleden. Klik op de test in de tabel die u wilt onderzoeken. U kunt de historische trend van het gegevensverlies, latentie en tijdwaarden van de reactie van de grafieken in het deelvenster RHS weergeven. Klik op de Details van de koppeling om de prestaties van elk knooppunt weer te geven. 

![Service-eindpunt Monitor Tests](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

Op de **Test knooppunten** weergave ziet u de netwerkverbinding van elk knooppunt. Klik op het knooppunt dat verminderde prestaties.  Dit is het knooppunt uit waar de toepassing is waargenomen te langzaam worden uitgevoerd. 

Bepalen of slechte prestaties vanwege het netwerk of vanwege een probleem aan einde van de aanbieder van de toepassing door de correlatie tussen de reactietijd van de toepassing en de latentie van netwerk - observeren 

**Toepassingsprobleem:** als er een piek in de reactietijd, maar de netwerklatentie consistent is, wordt dit kan erop wijzen dat het netwerk goed werkt en het probleem veroorzaakt door een probleem aan het einde van de toepassing wordt.  

![Service-eindpunt Monitor Toepassingsprobleem](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

**Probleem met het netwerk:** als een piek in de reactietijd wordt geleverd met een overeenkomstige piek in de netwerklatentie, wordt dit kan erop wijzen dat de toename van de reactietijd vanwege een toename van de netwerklatentie is.  

![Service-eindpunt Monitor netwerkprobleem](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

Nadat u hebt vastgesteld dat het probleem vanwege het netwerk is, kunt u klikken op de **topologie** koppeling voor weergave van de lastige hop op de kaart topologie te identificeren. Bijvoorbeeld, ziet in de afbeelding hieronder u dat buiten de 105 ms totale latentie tussen het knooppunt en het toepassingseindpunt, 96 ms vanwege de hop rood gemarkeerd in. Zodra u de lastige hop hebt geïdentificeerd, kunt u corrigerende maatregelen kunt nemen.  

![Service-eindpunt Monitor Tests](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Diagnostiek 

Als u een afwijking ziet, volgt u deze stappen:

Als de reactietijd van de service wegvallend netwerk en netwerklatentie worden weergegeven als NA, en vervolgens het vanwege een of meer van de volgende redenen zijn kan:
- De toepassing is niet beschikbaar.
- Het knooppunt dat wordt gebruikt voor het controleren van de netwerkverbinding met de service is niet actief.
- Het doel is ingevoerd in de Testconfiguratie van de is onjuist.
- Het knooppunt heeft geen een netwerkverbinding.

Als een geldige service reactietijd wordt weergegeven, maar wegvallend netwerk, evenals de latentie als NA worden weergegeven, kan zijn vanwege een of meer van de volgende redenen:
- Als het knooppunt dat wordt gebruikt voor het controleren van de netwerkverbinding met de service Windows client-computer, de doelservice ICMP-aanvragen is geblokkeerd of ICMP-aanvragen die afkomstig zijn van het knooppunt is geblokkeerd door een netwerkfirewall.
- Het selectievakje voor **netwerk metingen uitvoeren** is uitgeschakeld in de Testconfiguratie. 

Als de reactietijd van de service is NA maar wegvallend netwerk, evenals latentie geldig zijn, klikt u vervolgens stelt het de doelservice is niet een webtoepassing. De Testconfiguratie bewerken en kies het testtype als Netwerktest in plaats van de Webtest. 

Als de toepassing wordt traag uitgevoerd, moet u bepalen of slechte prestaties vanwege het netwerk of vanwege een probleem aan einde van de aanbieder van de toepassing is.


## <a name="next-steps"></a>Volgende stappen
* [Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde netwerk prestaties gegevensrecords weer te geven.
