---
title: Network Performance Monitor-oplossing in Azure Log Analytics | Microsoft Docs
description: De Connectiviteitsmonitor voor Service-mogelijkheid in Network Performance Monitor gebruiken voor het bewaken van de netwerkverbinding met een willekeurig eindpunt dat een open TCP-poort is.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 9f881071559eaba0f6b1c86e8827993d52470baf
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53186205"
---
# <a name="service-connectivity-monitor"></a>Bewaking van serviceverbinding

Kunt u de mogelijkheid van Service-Connectiviteitsmonitor [Network Performance Monitor](network-performance-monitor.md) voor het bewaken van de netwerkverbinding met een willekeurig eindpunt dat een open TCP-poort is. Dergelijke eindpunten zijn websites, SaaS-toepassingen, PaaS-toepassingen en SQL-databases. 

U kunt de volgende functies met Service-Connectiviteitsmonitor uitvoeren: 

- Controleer de netwerkverbinding met uw toepassingen en de netwerkservices van meerdere filialen of locaties. Toepassingen en -services omvatten Office 365, Dynamics CRM, interne line-of-business-toepassingen en SQL-databases.
- Gebruik ingebouwde tests om te controleren van de netwerkverbinding met Office 365 en Dynamics 365-eindpunten. 
- Bepaal de reactietijd, netwerklatentie en pakketverlies ervaren wanneer u verbinding maakt met het eindpunt.
- Bepalen of de prestaties slecht presteert vanwege het netwerk of vanwege een probleem opgetreden tijdens aan het uiteinde van de provider van de toepassing is.
- Identificeer hotspots in het netwerk die slecht presteert prestaties veroorzaakt mogelijk door de latentie bijgedragen door elke hop op een topologie-kaart weer te geven.


![Bewaking van serviceverbinding](media/network-performance-monitor-service-endpoint/service-endpoint-intro.png)


## <a name="configuration"></a>Configuratie 
Open de configuratie voor Network Performance Monitor te openen de [Network Performance Monitor oplossing](network-performance-monitor.md) en selecteer **configureren**.

![Network Performance Monitor configureren](media/network-performance-monitor-service-endpoint/npm-configure-button.png)


### <a name="configure-log-analytics-agents-for-monitoring"></a>Log Analytics-agents configureren voor bewaking
Schakelt u de volgende firewallregels op de knooppunten die worden gebruikt voor het bewaken van zodat de oplossing kunt u de topologie van uw knooppunten aan het service-eindpunt detecteren: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```

### <a name="create-service-connectivity-monitor-tests"></a>Connectiviteitsmonitor voor Service-tests maken 

Beginnen met het maken van de tests uitgevoerd voor het controleren van de netwerkverbinding met de service-eindpunten.

1. Selecteer de **Service Connectiviteitsmonitor** tabblad.
2. Selecteer **toevoegen testen**, en voer de testnaam en beschrijving. 
3. Selecteer het type van de test:<br>

    * Selecteer **Web** voor het bewaken van de verbinding met een service die op HTTP/S-aanvragen, zoals outlook.office365.com of bing.com reageert.<br>
    * Selecteer **netwerk** voor het bewaken van de verbinding met een service die reageert op aanvragen voor TCP, maar niet reageert op HTTP/S-aanvragen, zoals een SQL server, de FTP-server of de SSH-poort. 
4. Als u niet wilt netwerkmetingen, zoals netwerklatentie en pakketverlies topologie-detectie, schakelt u de **Netwerkmetingen** selectievakje. Het geselecteerd om op te halen van maximaal profiteren van de mogelijkheid houden. 
5. In **doel**, voer de URL/FQDN-naam/IP-adres waarnaar u wilt bewaken van de verbinding met het netwerk.
6. In **poortnummer**, voer het poortnummer van de doelservice. 
7. In **testen frequentie**, voer een waarde voor hoe vaak u wilt dat de test om uit te voeren. 
8. Selecteer de knooppunten van waaruit u wilt bewaken van de netwerkverbinding met de service. 

    >[!NOTE]
    > Voor Windows server-knooppunten gebruikt de mogelijkheid TCP-aanvragen naar de netwerkmetingen uitschakelen. Voor Windows-knooppunten op basis van een client gebruikt de mogelijkheid ICMP-aanvragen naar de netwerkmetingen uitschakelen. In sommige gevallen blokkeert de doeltoepassing binnenkomende ICMP-aanvragen wanneer de knooppunten Windows client-gebaseerde zijn. De oplossing is niet netwerkmetingen uitschakelen. U wordt aangeraden dat u Windows server-gebaseerde knooppunten in dergelijke gevallen gebruiken. 

9. Als u niet wilt maken van health-gebeurtenissen voor de items die u selecteert, schakel **statuscontrole inschakelen in de doelen waarop deze test betrekking**. 
10. Kies voorwaarden controleren. U kunt aangepaste drempelwaarden voor het genereren van health-gebeurtenis instellen door te voeren drempelwaarden. Wanneer de waarde van de voorwaarde hoger dan de geselecteerde drempelwaarde voor het geselecteerde netwerk of subnet paar gaat, wordt een statusgebeurtenis wordt gegenereerd. 
11. Selecteer **opslaan** aan de configuratie op te slaan. 

    ![Connectiviteitsmonitor voor service-testconfiguraties](media/network-performance-monitor-service-endpoint/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Walkthrough 

Ga naar de dashboardweergave Network Performance Monitor. Als u een overzicht van de status van de verschillende tests die u hebt gemaakt, bekijk de **Service Connectiviteitsmonitor** pagina. 

![Connectiviteitsmonitor voor service-pagina](media/network-performance-monitor-service-endpoint/service-endpoint-blade.png)

Selecteer de tegel om de details van de tests weer op te geven de **Tests** pagina. U kunt de status van de punt-in-time en de waarde van de reactietijd van service, netwerklatentie en pakketverlies voor alle tests weergeven in de tabel aan de linkerkant. Het besturingselement netwerk staat Recorder gebruiken om de momentopname van het netwerk op een later tijdstip in het verleden weer te geven. Selecteer de test in de tabel die u wilt onderzoeken. In de grafieken in het deelvenster aan de rechterkant vindt u de historische trend van het gegevensverlies, latentie en tijdwaarden van de reactie. Selecteer de **Test Details** koppeling om de prestaties van elk knooppunt weer te geven.

![Connectiviteitsmonitor voor service-tests](media/network-performance-monitor-service-endpoint/service-endpoint-tests.png)

In de **Testknooppunten** weergeven, kunt u de netwerkverbinding van elk knooppunt observeren. Selecteer het knooppunt met verminderde prestaties. Dit is het knooppunt waar de toepassing is waargenomen langzaam worden uitgevoerd.

Bepalen of de prestaties slecht presteert vanwege een probleem aan het uiteinde van de provider van de toepassing of het netwerk met inachtneming van de correlatie tussen de reactietijd van de toepassing en de netwerklatentie. 

* **Toepassingsprobleem met de:** Een piek in de reactietijd, maar consistentie in de netwerklatentie kan erop wijzen dat het netwerk goed werkt en het probleem wordt mogelijk veroorzaakt door een probleem aan het einde van de toepassing. 

    ![Connectiviteitsmonitor voor service-toepassingsprobleem](media/network-performance-monitor-service-endpoint/service-endpoint-application-issue.png)

* **Netwerkprobleem:** Een piek in de reactietijd, samen met een bijbehorende piek in de netwerklatentie kan erop wijzen dat de toename van reactietijden veroorzaakt door een toename van de netwerklatentie worden kan. 

    ![Connectiviteitsmonitor voor service-netwerkprobleem](media/network-performance-monitor-service-endpoint/service-endpoint-network-issue.png)

Nadat u hebt vastgesteld dat het probleem vanwege het netwerk is, selecteert u de **topologie** weergavekoppeling voor het identificeren van de lastige hop op de kaart topologie. Een voorbeeld wordt weergegeven in de volgende afbeelding. Buiten de 105 ms totale latentie tussen het knooppunt en het toepassingseindpunt van de is 96 ms vanwege de hop in rood gemarkeerd. Nadat u de lastige hop geïdentificeerd, kunt u corrigerende maatregelen kunt nemen. 

![Connectiviteitsmonitor voor service-tests](media/network-performance-monitor-service-endpoint/service-endpoint-topology.png)

## <a name="diagnostics"></a>Diagnostiek 

Als u een afwijking ziet, volgt u deze stappen:

* Als de reactietijd van service, de wegvallend netwerk en de latentie als NA worden weergegeven, kan een of meer van de volgende redenen de oorzaak zijn:

    - De toepassing is niet beschikbaar.
    - Het knooppunt dat wordt gebruikt voor het controleren van de netwerkverbinding met de service is niet actief.
    - Het doel is opgegeven in de Testconfiguratie van de is onjuist.
    - Het knooppunt beschikt niet over een netwerkverbinding.

* Als de reactietijd van een geldige service wordt weergegeven, maar wegvallend netwerk, evenals de latentie als NA worden weergegeven, kan een of meer van de volgende redenen de oorzaak zijn:

    - Als het knooppunt dat wordt gebruikt voor het controleren van de netwerkverbinding met de service een Windows client-computer is, de doelservice ICMP-aanvragen blokkeert of een netwerkfirewall ICMP-aanvragen die afkomstig uit het knooppunt zijn blokkeert.
    - De **Netwerkmetingen** selectievakje is uitgeschakeld in de Testconfiguratie. 

* Als de reactietijd van de service is NA maar wegvallend netwerk, evenals de latentie geldig zijn, kan een web-App niet in de doelservice zijn. De Testconfiguratie van de bewerken, en kies het testtype als **netwerk** in plaats van **Web**. 

* Als de toepassing wordt traag uitgevoerd, moet u bepalen of de prestaties slecht presteert vanwege een probleem aan het uiteinde van de provider van de toepassing of het netwerk.


## <a name="next-steps"></a>Volgende stappen
[Zoeken in logboeken](../../azure-monitor/log-query/log-query-overview.md) om gedetailleerde gegevens prestatierecords weer te geven.
