---
title: Geplande gebeurtenissen voor Windows-VM's in Azure | Microsoft Docs
description: Geplande gebeurtenissen met behulp van de Azure Metadata-service voor op uw Windows-machines.
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
ms.openlocfilehash: 9130bf5c2708f7eecf6fc1b5db2ffbb3c2fffc30
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54201275"
---
# <a name="azure-metadata-service-scheduled-events-for-windows-vms"></a>Azure Metadata Service: Geplande gebeurtenissen voor Windows-VM 's

Geplande gebeurtenissen is een Azure Metadata-Service waarmee uw toepassing de tijd om voor te bereiden voor onderhoud op virtuele machines. Het biedt informatie over geplande onderhoudsgebeurtenissen (bijvoorbeeld opnieuw opstarten), zodat uw toepassing kunt voorbereiden op deze en beperkt wordt onderbroken. Het is beschikbaar voor alle Azure Virtual Machine-typen, inclusief PaaS en IaaS op zowel Windows als Linux. 

Zie voor meer informatie over geplande gebeurtenissen op Linux [geplande gebeurtenissen voor virtuele Linux-machines](../linux/scheduled-events.md).

> [!Note] 
> Geplande gebeurtenissen is algemeen beschikbaar in alle Azure-regio's. Zie [versie en beschikbaarheid in regio](#version-and-region-availability) voor de meest recente release-informatie.

## <a name="why-scheduled-events"></a>Waarom geplande gebeurtenissen?

Veel toepassingen kunnen profiteren van de tijd om voor te bereiden voor onderhoud op virtuele machines. De tijd kan worden gebruikt om uit te voeren van specifieke taken voor toepassingen die betere beschikbaarheid, betrouwbaarheid en onderhoudsvriendelijkheid met inbegrip van: 

- Controlepunt maken en terugzetten
- Verwerkingsstop voor verbindingen
- Primaire replica-failover 
- Verwijdering uit de load balancer-groep
- Gebeurtenisregistratie
- Correct afsluiten 

Met behulp van geplande gebeurtenissen van uw toepassing kan detecteren wanneer onderhoud zal optreden en taken te beperken de gevolgen ervan te activeren. Inschakelen van geplande gebeurtenissen, biedt uw virtuele machine een minimale hoeveelheid tijd voordat de activiteit onderhoud wordt uitgevoerd. Zie de sectie planning van de gebeurtenis hieronder voor meer informatie.

Geplande gebeurtenissen biedt gebeurtenissen in de volgende gevallen gebruik:
- Platform gestart onderhoud (bijvoorbeeld Host Update van het besturingssysteem)
- De gebruiker gestart onderhoud (bijvoorbeeld als gebruiker opnieuw wordt opgestart of implementeert een virtuele machine opnieuw)

## <a name="the-basics"></a>De basisbeginselen  

Informatie over het uitvoeren van virtuele Machines met een REST-eindpunt dat toegankelijk is vanuit de virtuele machine wordt aangegeven dat Azure Metadata-service. De informatie is beschikbaar via een niet-routeerbare IP-adres zodat deze niet zichtbaar buiten de virtuele machine wordt gemaakt.

### <a name="endpoint-discovery"></a>Eindpuntdetectie
Voor VNET ingeschakeld virtuele machines, de metadata-service is beschikbaar via een statische niet-routeerbare IP-adres, `169.254.169.254`. De volledige-eindpunt voor de nieuwste versie van geplande gebeurtenissen is: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01`

Als de virtuele Machine niet binnen een Virtueelnetwerk, de standaard-aanvragen voor cloudservices en klassieke virtuele machines gemaakt is, worden aanvullende logica is vereist voor het detecteren van het IP-adres te gebruiken. Verwijzen naar dit voorbeeld om te leren hoe u [detecteren van het eindpunt van de host](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="version-and-region-availability"></a>Versie en beschikbaarheid in regio
De geplande gebeurtenissen-Service is samengesteld. Versies zijn verplicht en de huidige versie is `2017-08-01`.

| Versie | Releasetype | Regio's | Releaseopmerkingen | 
| - | - | - | - |
| 2017-08-01 | Algemene beschikbaarheid | Alle | <li> Voorafgegaan onderstrepingsteken verwijderd uit de namen van voorbeeldresources voor IaaS-VM 's<br><li>Koptekst met metagegevens vereiste afgedwongen voor alle aanvragen | 
| 2017-03-01 | Preview | Alle |<li>Eerste release

> [!NOTE] 
> Eerdere versies van de Preview-versie van geplande gebeurtenissen {nieuwste} wordt ondersteund als de api-versie. Deze indeling wordt niet meer ondersteund en wordt in de toekomst afgeschaft.

### <a name="enabling-and-disabling-scheduled-events"></a>Inschakelen en uitschakelen van geplande gebeurtenissen
Geplande gebeurtenissen is ingeschakeld voor uw service de eerste keer dat u een aanvraag voor gebeurtenissen. U moet een vertraagde reactie verwachten in de eerste aanroep van twee minuten. U moet een query uitvoeren voor het eindpunt van de tijd tot tijd voor het detecteren van geplande onderhoudsgebeurtenissen, evenals de status van onderhoudsactiviteiten die worden uitgevoerd.

Geplande gebeurtenissen is uitgeschakeld voor uw service als deze niet een aanvraag voor 24 uur.

### <a name="user-initiated-maintenance"></a>De gebruiker gestart onderhoud
De gebruiker gestart onderhoud op virtuele machines via de Azure portal, API, CLI of PowerShell resulteert in een geplande gebeurtenis. Hiermee kunt u de voorbereiding van onderhoud logica in uw toepassing testen en kan uw toepassing om voor te bereiden voor de gebruiker gestart onderhoud.

Een virtuele machine opnieuw opstarten plant u een gebeurtenis met het type `Reboot`. Een virtuele machine opnieuw implementeren plant u een gebeurtenis met het type `Redeploy`.

## <a name="using-the-api"></a>Met behulp van de API

### <a name="headers"></a>Headers
Wanneer u een query de Metadata-Service, moet u de header `Metadata:true` om te controleren of de aanvraag is niet per ongeluk wordt omgeleid. De `Metadata:true` -header is vereist voor alle aanvragen van geplande gebeurtenissen. De header bevatten in de aanvraag is mislukt resulteert in een ongeldige aanvraag-reactie van de Metadata-Service.

### <a name="query-for-events"></a>Query voor gebeurtenissen
U kunt een query voor geplande gebeurtenissen gewoon door de volgende oproep verzenden:

#### <a name="powershell"></a>PowerShell
```
curl http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01 -H @{"Metadata"="true"}
```

Een antwoord bevat een matrix met geplande gebeurtenissen. Lege matrix betekent dat er momenteel geen geplande gebeurtenissen.
In het geval waarbij er geplande gebeurtenissen, het antwoord bevat een reeks gebeurtenissen: 
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
De DocumentIncarnation is een ETag en biedt een eenvoudige manier om te controleren als de nettolading van de gebeurtenissen is gewijzigd sinds de laatste query.

### <a name="event-properties"></a>Eigenschappen van gebeurtenis
|Eigenschap  |  Description |
| - | - |
| Gebeurtenis-id | Unieke id voor deze gebeurtenis. <br><br> Voorbeeld: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Type gebeurtenis | Impact die deze gebeurtenis is veroorzaakt. <br><br> Waarden: <br><ul><li> `Freeze`: De virtuele Machine is gepland voor een paar seconden onderbreken. De CPU wordt onderbroken, maar er is geen invloed op geheugen, geopende bestanden of netwerkverbindingen. <li>`Reboot`: De virtuele Machine is gepland voor opnieuw opstarten (niet-permanent geheugen is verloren gegaan). <li>`Redeploy`: De virtuele Machine is gepland om te verplaatsen naar een ander knooppunt (tijdelijke schijven zijn verloren). |
| ResourceType | Het type resource dat gevolgen heeft voor deze gebeurtenis. <br><br> Waarden: <ul><li>`VirtualMachine`|
| Resources| Lijst met resources die gevolgen heeft voor deze gebeurtenis. Dit kan worden gegarandeerd machines van maximaal één bevatten [Updatedomein](manage-availability.md), maar niet alle machines in de UD kan bevatten. <br><br> Voorbeeld: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Gebeurtenisstatus | De status van deze gebeurtenis. <br><br> Waarden: <ul><li>`Scheduled`: Deze gebeurtenis is gepland om te starten na de tijd die is opgegeven de `NotBefore` eigenschap.<li>`Started`: Deze gebeurtenis is gestart.</ul> Geen `Completed` of soortgelijke status ooit wordt geleverd; de gebeurtenis wordt niet meer worden geretourneerd wanneer de gebeurtenis is voltooid.
| NotBefore| De tijd waarna deze gebeurtenis wordt gestart. <br><br> Voorbeeld: <br><ul><li> Ma, 19 Sep 2016 18:29:47 GMT  |

### <a name="event-scheduling"></a>Gebeurtenis plannen
Elke gebeurtenis is gepland op basis van een minimale hoeveelheid tijd in de toekomst op gebeurtenistype. Dit moment wordt weergegeven in van een gebeurtenis `NotBefore` eigenschap. 

|Type gebeurtenis  | Minimale kennisgeving |
| - | - |
| Blokkeren| 15 minuten |
| Opnieuw opstarten | 15 minuten |
| Opnieuw implementeren | 10 minuten |

### <a name="event-scope"></a>Bereik van gebeurtenis     
Geplande gebeurtenissen worden geleverd aan:        
 - Alle virtuele Machines in een Cloudservice      
 - Alle virtuele Machines in een Beschikbaarheidsset      
 - Alle virtuele Machines in een Schaalset instellen Plaatsingsgroep.         

Als gevolg hiervan moet u controleren de `Resources` veld in de gebeurtenis te identificeren welke virtuele machines worden hierdoor worden beïnvloed. 

### <a name="starting-an-event"></a>Een gebeurtenis starten 

Nadat u hebt geleerd van een aanstaande agendagebeurtenis en de logica voor het correct afsluiten voltooid, kunt u de openstaande gebeurtenis goedkeuren door vooraf een `POST` aanroepen naar de metadata-service met de `EventId`. Hiermee geeft u aan Azure dat deze de minimale melding kunt verkorten tijd (indien mogelijk). 

Hieronder volgt de json die wordt verwacht in de `POST` aanvraagtekst. De aanvraag moet bevatten een lijst met `StartRequests`. Elke `StartRequest` bevat de `EventId` voor de gebeurtenis die u wilt versnellen:
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
curl -H @{"Metadata"="true"} -Method POST -Body '{"StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' -Uri http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

> [!NOTE] 
> De gebeurtenis om door te gaan voor alle verzoeken een gebeurtenis kunt `Resources` in het geval, niet alleen de virtuele machine erkent dat de gebeurtenis. U kunt daarom een leider voor de coördinatie van de bevestiging die net zo eenvoudig als het eerste virtuele machine in de `Resources` veld.


## <a name="powershell-sample"></a>Voorbeeld van PowerShell 

Het volgende voorbeeld zoekt de metadata-service voor geplande gebeurtenissen en keurt deze goed elke openstaande gebeurtenis.

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
function Approve-ScheduledEvent($eventId, $uri)
{    
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests} 
    
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
        Approve-ScheduledEvent $event.EventId $scheduledEventURI 
    }
}
``` 

## <a name="next-steps"></a>Volgende stappen 

- Bekijk een [geplande gebeurtenissen Demo](https://channel9.msdn.com/Shows/Azure-Friday/Using-Azure-Scheduled-Events-to-Prepare-for-VM-Maintenance) op Azure Friday. 
- Controleer de geplande gebeurtenissen-codevoorbeelden in de [Azure Instance Metadata geplande gebeurtenissen GitHub-opslagplaats](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm)
- Meer informatie over de beschikbare API's in de [Instance Metadata service](instance-metadata-service.md).
- Meer informatie over [gepland onderhoud voor Windows-machines in Azure](planned-maintenance.md).
