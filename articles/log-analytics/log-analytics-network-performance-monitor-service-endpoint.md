---
title: Netwerk-Prestatiemeter-oplossing in Azure Log Analytics | Microsoft Docs
description: De mogelijkheid servicebeheer eindpunt in Prestatiemeter netwerk gebruiken om te controleren van netwerkverbindingen met een willekeurig eindpunt met een open TCP-poort.
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
ms.component: na
ms.openlocfilehash: c260371043ddcb8a9ea5952760377282596fff7c
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127190"
---
# <a name="service-connectivity-monitor"></a>Bewaking van serviceverbinding

Kunt u de mogelijkheid van de Service verbinding Monitor in [netwerk Prestatiemeter](log-analytics-network-performance-monitor.md) voor het bewaken van de netwerkverbinding met een willekeurig eindpunt met een open TCP-poort. Dergelijke eindpunten zijn websites, SaaS-toepassingen, PaaS-toepassingen en SQL-databases. 

U kunt de volgende functies met de Service verbinding Monitor uitvoeren: 

- Controleer de netwerkverbinding met uw toepassingen en de netwerkservices van meerdere filialen of locaties. Toepassingen en netwerkservices bevatten Office 365, Dynamics CRM, interne line-of-business-toepassingen en SQL-databases.
- Gebruik de ingebouwde tests voor het bewaken van de netwerkverbinding met Office 365 en Dynamics 365 eindpunten. 
- Bepaal de reactietijd, netwerklatentie en pakketverlies ervaren wanneer u verbinding met het eindpunt.
- Bepalen of slechte prestaties vanwege het netwerk of vanwege een probleem aan de kant van de aanbieder van de toepassing is.
- Identificeer hotspots op het netwerk die slechte prestaties veroorzaakt mogelijk door de latentie die is bijgedragen door elke hop op een topologiekaart weer te geven.


![Bewaking van serviceverbinding](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Configuratie 
Open de configuratie voor netwerk-Prestatiemeter te openen de [netwerk Prestatiemeter oplossing](log-analytics-network-performance-monitor.md) en selecteer **configureren**.

![Prestatiemeter netwerk configureren](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-operations-management-suite-agents-for-monitoring"></a>Operations Management Suite-agents voor bewaking configureren
Schakel de volgende firewallregels op de knooppunten die wordt gebruikt voor het bewaken van zodat de oplossing kan de topologie van uw knooppunten met de service-eindpunt detecteren: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```

### <a name="create-service-connectivity-monitor-tests"></a>Maken van de Service verbinding Monitor tests 

Beginnen met het maken van uw tests uit om te controleren van de netwerkverbinding met de service-eindpunten.

1. Selecteer de **Service connectiviteit Monitor** tabblad.
2. Selecteer **toevoegen testen**, en voer de naam van test en de beschrijving. 
3. Selecteer het type van test:<br>

    * Selecteer **Web** voor het bewaken van de verbinding met een service die op HTTP/S-aanvragen, zoals outlook.office365.com of bing.com reageert.<br>
    * Selecteer **netwerk** voor het bewaken van de verbinding met een service die reageert op aanvragen voor TCP, maar niet reageert op HTTP/S-aanvragen, zoals een SQL server, de FTP-server of de SSH-poort. 
4. Als u niet wilt uitvoeren van metingen voor netwerk, zoals netwerklatentie pakketverlies en topologie-detectie, schakelt u de **netwerk metingen uitvoeren** selectievakje. Houd die er maximaal profiteren ophalen van de mogelijkheid hebt geselecteerd. 
5. In **doel**, voer de FQDN-URL/IP-adres waarnaar u wilt bewaken van de verbinding met het netwerk.
6. In **poortnummer**, voer het poortnummer van de doelservice. 
7. In **testen frequentie**, voer een waarde voor hoe vaak u wilt dat de test uit te voeren. 
8. Selecteer de knooppunten van waaruit u wilt bewaken van de netwerkverbinding met de service. 

    >[!NOTE]
    > Voor Windows server-knooppunten gebruikt de mogelijkheid TCP-aanvragen uit te voeren van de metingen van het netwerk. Voor Windows client-knooppunten gebruikt de mogelijkheid ICMP-aanvragen uit te voeren van de metingen van het netwerk. In sommige gevallen blokkeert de doeltoepassing binnenkomende ICMP-aanvragen wanneer de knooppunten Windows client-gebaseerde zijn. De oplossing is geen metingen netwerk uitvoeren. U wordt aangeraden Windows server gebaseerde knooppunten in dergelijke gevallen. 

9. Als u niet wilt maken van health-gebeurtenissen voor de items die u selecteert, schakel **statuscontrole inschakelen in de doelen wordt gedekt door deze test**. 
10. Kies de bewaking van de voorwaarden. U kunt aangepaste drempelwaarden voor het genereren van de health-gebeurtenis instellen door te voeren drempelwaarden. Wanneer de waarde van de voorwaarde hoger dan de geselecteerde drempelwaarde voor het geselecteerde netwerk of subnetwerk paar is, wordt een health-gebeurtenis gegenereerd. 
11. Selecteer **opslaan** aan de configuratie op te slaan. 

    ![Monitor voor service-eindpunt test configuraties](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Walkthrough 

Ga naar de dashboardweergave voor netwerk-Prestatiemeter. Als u een samenvatting van de status van de verschillende tests die u hebt gemaakt, bekijk de **Service connectiviteit Monitor** pagina. 

![Monitor voor service-eindpunt pagina](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Selecteer de tegel om de details van de tests weergeven op de **Tests** pagina. U kunt de status van de punt in tijd en de waarde van de reactietijd van de service, netwerklatentie en pakketverlies voor alle tests weergeven in de tabel aan de linkerkant. Het besturingselement netwerk staat doos gebruiken om de momentopname van het netwerk op een later tijdstip in het verleden weer te geven. Selecteer de test in de tabel die u wilt onderzoeken. U kunt de historische trend van het gegevensverlies, latentie en antwoord tijdwaarden weergeven in de grafieken in het deelvenster aan de rechterkant. Selecteer de **Test Details** koppeling om de prestaties van elk knooppunt weer te geven.

![Monitor voor service-eindpunt tests](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

In de **Test knooppunten** weergave ziet u de netwerkverbinding van elk knooppunt. Selecteer het knooppunt met verminderde prestaties. Dit is het knooppunt waar de toepassing is waargenomen te langzaam worden uitgevoerd.

Bepalen of slechte prestaties vanwege een probleem aan de kant van de aanbieder van de toepassing of het netwerk door de correlatie tussen de reactietijd van de toepassing en de netwerklatentie observeren. 

* **Toepassingsprobleem:** een piek in de reactietijd maar consistentie in de netwerklatentie stelt voor dat het netwerk goed werkt en het probleem wordt mogelijk veroorzaakt door een probleem aan het einde van de toepassing. 

    ![Monitor voor service-eindpunt toepassingsprobleem](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

* **Probleem met het netwerk:** een piek in de reactietijd gepaard met een overeenkomstige piek in de netwerklatentie wijst erop dat de toename in responstijd veroorzaakt door een toename van de netwerklatentie worden kan. 

    ![Monitor voor service-eindpunt netwerkprobleem](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

Nadat u hebt vastgesteld dat het probleem vanwege het netwerk is, selecteert u de **topologie** koppeling voor weergave van de lastige hop op de topologiekaart te identificeren. Een voorbeeld wordt weergegeven in de volgende afbeelding. Buiten de 105 ms totale latentie tussen het knooppunt en het toepassingseindpunt is 96 ms vanwege de hop rood gemarkeerd in. Nadat u de lastige hop geïdentificeerd, kunt u corrigerende maatregelen kunt nemen. 

![Monitor voor service-eindpunt tests](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Diagnostiek 

Als u een afwijking ziet, volg deze stappen:

* Als de reactietijd van de service, wegvallend netwerk en netwerklatentie worden weergegeven als NA, een of meer van de volgende redenen is mogelijk de oorzaak:

    - De toepassing is niet beschikbaar.
    - Het knooppunt dat wordt gebruikt voor het controleren van de netwerkverbinding met de service is niet actief.
    - Het doel is ingevoerd in de Testconfiguratie van de is onjuist.
    - Het knooppunt hebben niet een netwerkverbinding.

* Als een geldige service reactietijd wordt weergegeven, maar wegvallend netwerk, evenals de latentie als NA worden weergegeven, zijn een of meer van de volgende redenen de oorzaak:

    - Als het knooppunt dat wordt gebruikt voor het controleren van de netwerkverbinding met de service een Windows client-computer is, de doelservice ICMP-aanvragen is geblokkeerd of een netwerkfirewall ICMP-aanvragen die afkomstig uit het knooppunt zijn is geblokkeerd.
    - De **netwerk metingen uitvoeren** selectievakje is uitgeschakeld in de Testconfiguratie. 

* Als de reactietijd van de service is NA maar wegvallend netwerk, evenals latentie geldig zijn, mogelijk de doelservice een webtoepassing niet. De Testconfiguratie bewerken en kies het testtype als **netwerk** in plaats van **Web**. 

* Als de toepassing wordt traag uitgevoerd, moet u bepalen of slechte prestaties vanwege een probleem aan de kant van de aanbieder van de toepassing of het netwerk.


## <a name="next-steps"></a>Volgende stappen
[Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde netwerk prestaties gegevensrecords weer te geven.
