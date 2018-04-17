---
title: Gebeurtenissen gepland voor de Windows virtuele machines in Azure | Microsoft Docs
description: Gepland gebeurtenissen met de metagegevens van de Azure-service voor op uw Windows virtuele machines.
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: ''
author: ericrad
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ericrad
ms.openlocfilehash: 63318b78607802d7d70d65a186a396cbc655c40b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="azure-metadata-service-scheduled-events-for-windows-vms"></a>Azure metagegevens Service: Geplande gebeurtenissen voor VM's van Windows

Geplande gebeurtenissen is een Azure-metagegevens-Service waarmee uw toepassing de tijd om voor te bereiden voor onderhoud op virtuele machines. Biedt informatie over de aanstaande onderhoud gebeurtenissen (bijvoorbeeld opstarten vereist) zodat uw toepassing kunt voor ze voorbereiden en beperkt wordt onderbroken. Het is beschikbaar voor alle virtuele Machine van Azure-typen, met inbegrip van PaaS en IaaS op Windows- en Linux. 

Zie voor meer informatie over gebeurtenissen voor gepland op Linux [gepland gebeurtenissen voor virtuele Linux-machines](../linux/scheduled-events.md).

> [!Note] 
> Geplande gebeurtenissen is algemeen beschikbaar op alle Azure-regio's. Zie [versie en beschikbaarheid in regio's](#version-and-region-availability) voor de meest recente release-informatie.

## <a name="why-scheduled-events"></a>Waarom gebeurtenissen gepland?

Veel toepassingen kunnen profiteren van de tijd om voor te bereiden voor onderhoud op virtuele machines. De tijd kan worden gebruikt voor toepassing specifieke taken uitvoeren die betere beschikbaarheid, betrouwbaarheid en onderhoudsvriendelijkheid inclusief: 

- Controlepunt en terugzetten
- Is een verwerkingsstop voor verbinding
- Primaire replica-failover 
- Verwijdering van de load balancer-groep
- Logboekregistratie van gebeurtenissen
- Correct afsluiten 

Het gebruik van geplande gebeurtenissen van uw toepassing kan detecteren wanneer onderhoud wordt uitgevoerd en taken voor het beperken van de gevolgen ervan wordt geactiveerd.  

Geplande gebeurtenissen biedt gebeurtenissen in de volgende gevallen:
- Platform geïnitieerd onderhoud (bijvoorbeeld Host-OS Update)
- Gebruiker gestart onderhoud (bijvoorbeeld als gebruiker opnieuw wordt opgestart of redeploys van een virtuele machine)

## <a name="the-basics"></a>De basisbeginselen  

Azure service van de metagegevens wordt informatie over het uitvoeren van virtuele Machines met een REST-eindpunt toegankelijk vanuit de virtuele machine. De informatie is beschikbaar via een niet-routeerbare IP-adres, zodat deze niet buiten de virtuele machine wordt weergegeven.

### <a name="endpoint-discovery"></a>Detectie van het eindpunt
Voor VNET ingeschakeld voor virtuele machines, de metagegevensservice is beschikbaar via een statische niet-routeerbare IP-adres `169.254.169.254`. Het volledige eindpunt voor de nieuwste versie van gebeurtenissen voor gepland is: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01`

Als de virtuele Machine niet vanuit een virtueel netwerk, de standaard gevallen voor cloudservices en klassieke virtuele machines gemaakt is, worden extra logica is vereist voor het detecteren van het IP-adres te gebruiken. Raadpleeg dit voorbeeld voor meer informatie over hoe [de host-eindpunt detecteren](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="version-and-region-availability"></a>Versie en beschikbaarheid in regio 's
De gebeurtenissen geplande Service is samengesteld. Versies zijn verplicht en de huidige versie is `2017-08-01`.

| Versie | Releasetype | Regio's | Releaseopmerkingen | 
| - | - | - | - |
| 2017-08-01 | Algemene beschikbaarheid | Alle | <li> De functienaam geplaatst onderstrepingsteken verwijderd uit resourcenamen voor Iaas VM 's<br><li>Metagegevenskoptekst vereiste afgedwongen voor alle aanvragen | 
| 2017-03-01 | Preview | Alle |<li>Eerste release

> [!NOTE] 
> Eerdere versies van de preview van geplande gebeurtenissen {laatste} wordt ondersteund als de api-versie. Deze indeling wordt niet meer ondersteund en in de toekomst wordt afgeschaft.

### <a name="enabling-and-disabling-scheduled-events"></a>Inschakelen en uitschakelen van geplande gebeurtenissen
Geplande gebeurtenissen is ingeschakeld voor uw service de eerste keer dat u een aanvraag voor gebeurtenissen. U moet een vertraagde antwoord verwacht in de eerste aanroep van twee minuten.

Geplande gebeurtenissen is uitgeschakeld voor uw service als maakt geen een aanvraag voor 24 uur.

### <a name="user-initiated-maintenance"></a>Gebruiker gestart onderhoud
Gebruiker gestart onderhoud op virtuele machines via de Azure-portal API, CLI of PowerShell resulteert in een geplande gebeurtenis. Dit kunt u de voorbereiding van onderhoud logica in uw toepassing testen en kan de toepassing om voor te bereiden voor het onderhoud van de gebruiker worden gestart.

Een virtuele machine opnieuw plant u een gebeurtenis met type `Reboot`. Een virtuele machine opnieuw plant u een gebeurtenis met type `Redeploy`.

## <a name="using-the-api"></a>Met behulp van de API

### <a name="headers"></a>Headers
Wanneer u een query de Metadata-Service, moet u de header geven `Metadata:true` om te controleren of de aanvraag is niet per ongeluk wordt omgeleid. De `Metadata:true` -header is vereist voor alle geplande gebeurtenissen aanvragen. De header opnemen in de aanvraag is mislukt leidt tot een onjuiste aanvraag reactie van de Metadata-Service.

### <a name="query-for-events"></a>Query voor gebeurtenissen
U kunt een query voor geplande gebeurtenissen gewoon door de volgende oproep verzenden:

#### <a name="powershell"></a>PowerShell
```
curl http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01 -H @{"Metadata"="true"}
```

Een antwoord bevat een matrix van geplande gebeurtenissen. Een lege matrix betekent zijn momenteel geen gebeurtenissen die zijn gepland.
In het geval wanneer er geplande gebeurtenissen, het antwoord bevat een matrix van gebeurtenissen: 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a>Eigenschappen van gebeurtenis
|Eigenschap  |  Beschrijving |
| - | - |
| Gebeurtenis-id | Globaal unieke id voor deze gebeurtenis. <br><br> Voorbeeld: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| EventType | Impact die deze gebeurtenis wordt veroorzaakt. <br><br> Waarden: <br><ul><li> `Freeze`: De virtuele Machine is gepland voor wacht u enkele seconden. De CPU wordt onderbroken, maar er zijn geen gevolgen voor geheugen, open bestanden of netwerkverbindingen. <li>`Reboot`: De virtuele Machine is gepland voor opnieuw opstarten (niet-permanente geheugen is verloren gegaan). <li>`Redeploy`: Gepland de virtuele Machine naar een ander knooppunt verplaatsen (kortstondige schijven gaan verloren). |
| ResourceType | Type resource dat van invloed is op deze gebeurtenis. <br><br> Waarden: <ul><li>`VirtualMachine`|
| Resources| Lijst met bronnen die van invloed is op deze gebeurtenis. Dit kan worden gegarandeerd bevat machines van maximaal één [Updatedomein](manage-availability.md), maar mag niet alle machines in de UD bevatten. <br><br> Voorbeeld: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Gebeurtenisstatus | Status van deze gebeurtenis. <br><br> Waarden: <ul><li>`Scheduled`: Deze gebeurtenis is gepland om te starten na de tijd die is opgegeven in de `NotBefore` eigenschap.<li>`Started`: Deze gebeurtenis is gestart.</ul> Geen `Completed` of soortgelijke status ooit wordt geleverd; de gebeurtenis wordt niet meer worden geretourneerd wanneer de gebeurtenis is voltooid.
| NotBefore| De tijd waarna deze gebeurtenis wordt gestart. <br><br> Voorbeeld: <br><ul><li> Ma, 19 Sep 2016 18:29:47 GMT  |

### <a name="event-scheduling"></a>Gebeurtenis plannen
Elke gebeurtenis is gepland op basis van de minimale hoeveelheid tijd in de toekomst op gebeurtenistype. Deze tijd wordt weergegeven in een gebeurtenis `NotBefore` eigenschap. 

|EventType  | Minimale kennisgeving |
| - | - |
| Blokkeren| 15 minuten |
| Opnieuw opstarten | 15 minuten |
| Opnieuw implementeren | 10 minuten |

### <a name="event-scope"></a>Bereik van gebeurtenis     
Geplande gebeurtenissen worden afgeleverd bij:        
 - Alle virtuele Machines in een Cloudservice      
 - Alle virtuele Machines in een Beschikbaarheidsset      
 - Alle virtuele Machines in een groep Scale Set plaatsing.         

Als gevolg hiervan moet u controleren de `Resources` veld in de gebeurtenis geven aan welke VM's gaan worden beïnvloed. 

### <a name="starting-an-event"></a>Een gebeurtenis wordt gestart 

Zodra u hebt geleerd van een aanstaande gebeurtenis en de logica voor het correct afsluiten voltooid, kunt u de openstaande gebeurtenis goedkeuren door het maken van een `POST` aanroep van de metagegevensservice met de `EventId`. Hiermee geeft u aan Azure dat deze de minimale melding kunt inkorten tijd (indien mogelijk). 

Hieronder volgt de json verwacht in de `POST` aanvraagtekst. De aanvraag moet bevatten een lijst met `StartRequests`. Elke `StartRequest` bevat de `EventId` voor de gebeurtenis die u wilt versnellen:
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

#### <a name="powershell"></a>PowerShell
```
curl -H @{"Metadata"="true"} -Method POST -Body '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' -Uri http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

> [!NOTE] 
> Een gebeurtenis zijn bevestigd, kan de gebeurtenis om door te gaan voor alle `Resources` in het gebeurtenislogboek, niet alleen de virtuele machine bevestigt dat de gebeurtenis. Daarom kunt u ervoor kiest een opvulteken voor het coördineren van de bevestiging die net zo eenvoudig als het eerste virtuele machine in de `Resources` veld.


## <a name="powershell-sample"></a>PowerShell-voorbeeld 

Het volgende voorbeeld query op de metagegevensservice voor geplande gebeurtenissen en keurt deze goed elke openstaande gebeurtenis.

```PowerShell
# How to get scheduled events 
function Get-ScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function Approve-ScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert to JSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function Handle-ScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-08-01' -f $localHostIP 

# Get events
$scheduledEvents = Get-ScheduledEvents $scheduledEventURI

# Handle events however is best for your service
Handle-ScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        Approve-ScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 

## <a name="next-steps"></a>Volgende stappen 

- Bekijk een [gebeurtenissen Demo gepland](https://channel9.msdn.com/Shows/Azure-Friday/Using-Azure-Scheduled-Events-to-Prepare-for-VM-Maintenance) op Azure vrijdag. 
- Bekijk de codevoorbeelden gepland gebeurtenissen in de [Azure exemplaar metagegevens gepland gebeurtenissen Github-opslagplaats](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm)
- Meer informatie over de beschikbare API's in de [service-exemplaar metagegevens](instance-metadata-service.md).
- Meer informatie over [gepland onderhoud voor Windows virtuele machines in Azure](planned-maintenance.md).
