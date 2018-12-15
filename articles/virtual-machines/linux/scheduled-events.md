---
title: Geplande gebeurtenissen voor virtuele Linux-machines in Azure | Microsoft Docs
description: Gebeurtenissen plannen met behulp van Azure Metadata-Service voor uw virtuele Linux-machines.
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: ''
author: ericrad
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ericrad
ms.openlocfilehash: fc9ceb06eb51d1e88306f0971ad055facd05f9fb
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53437262"
---
# <a name="azure-metadata-service-scheduled-events-for-linux-vms"></a>Azure Metadata Service: Geplande gebeurtenissen voor virtuele Linux-machines

Geplande gebeurtenissen is een Azure Metadata-Service waarmee uw toepassing de tijd om voor te bereiden voor onderhoud op virtuele machines (VM). Het biedt informatie over geplande onderhoudsgebeurtenissen (bijvoorbeeld opnieuw opstarten) zodat uw toepassing kunt voorbereiden op deze en beperkt wordt onderbroken. Het is beschikbaar voor alle Azure Virtual Machines-typen, inclusief PaaS en IaaS op zowel Windows als Linux. 

Zie voor meer informatie over geplande gebeurtenissen op Windows [geplande gebeurtenissen voor Windows-VM's](../windows/scheduled-events.md).

> [!Note] 
> Geplande gebeurtenissen is algemeen beschikbaar in alle Azure-regio's. Zie [versie en beschikbaarheid in regio](#version-and-region-availability) voor de meest recente release-informatie.

## <a name="why-use-scheduled-events"></a>Waarom geplande gebeurtenissen gebruiken?

Veel toepassingen kunnen profiteren van de tijd om voor te bereiden voor onderhoud van VM's. De tijd kan worden gebruikt om de toepassingsspecifieke taken uitvoeren die betere beschikbaarheid, betrouwbaarheid en onderhoud, met inbegrip van: 

- Controlepunt en terugzetten.
- Verwerkingsstop voor verbindingen.
- Failover van de primaire replica.
- Het verwijderen van een load balancer-groep.
- Logboekregistratie.
- Correct afsluiten.

Met geplande gebeurtenissen, kan uw toepassing detecteren wanneer onderhoud zal optreden en taken te beperken de gevolgen ervan te activeren.  

Geplande gebeurtenissen biedt gebeurtenissen in de volgende gevallen gebruik:

- Platform gestart onderhoud (bijvoorbeeld een host met update)
- Gebruiker gestart onderhoud (bijvoorbeeld, als gebruiker opnieuw wordt opgestart of implementeert een virtuele machine opnieuw)

## <a name="the-basics"></a>De basisbeginselen  

  Metadata-Service wordt aangegeven dat informatie over het uitvoeren van virtuele machines met behulp van een REST-eindpunten die toegankelijk is vanuit de virtuele machine. De informatie is beschikbaar via een IP-adres nonroutable zodat deze niet zichtbaar buiten de virtuele machine wordt gemaakt.

### <a name="scope"></a>Bereik
Geplande gebeurtenissen worden geleverd aan:

- Alle virtuele machines in een cloudservice.
- Alle virtuele machines in een beschikbaarheidsset.
- Alle virtuele machines in een schaalset instellen plaatsingsgroep. 

Als gevolg hiervan, Controleer de `Resources` veld in de gebeurtenis te identificeren welke virtuele machines worden beïnvloed.

### <a name="endpoint-discovery"></a>Eindpuntdetectie
Voor VNET ingeschakeld virtuele machines, Metadata-Service is beschikbaar via een nonroutable statische IP-adres, `169.254.169.254`. De volledige-eindpunt voor de nieuwste versie van geplande gebeurtenissen is: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01`

Als de virtuele machine niet binnen een Virtueelnetwerk, de standaard-aanvragen voor cloudservices en klassieke virtuele machines gemaakt is, worden aanvullende logica is vereist voor het detecteren van het IP-adres te gebruiken. Voor meer informatie over hoe u [detecteren van het eindpunt van de host](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm), Zie het in dit voorbeeld.

### <a name="version-and-region-availability"></a>Versie en beschikbaarheid in regio
De geplande gebeurtenissen-service is samengesteld. Versies zijn verplicht. de huidige versie is `2017-08-01`.

| Versie | Releasetype | Regio's | Releaseopmerkingen | 
| - | - | - | - | 
| 2017-08-01 | Algemene beschikbaarheid | Alle | <li> Voorafgegaan onderstrepingsteken verwijderd uit de namen van voorbeeldresources voor Iaas-VM 's<br><li>Metagegevens-header vereiste afgedwongen voor alle aanvragen | 
| 2017-03-01 | Preview | Alle | <li>Eerste release


> [!NOTE] 
> Eerdere versies van de Preview-versie van geplande gebeurtenissen {nieuwste} wordt ondersteund als de api-versie. Deze indeling wordt niet meer ondersteund en wordt in de toekomst afgeschaft.

### <a name="enabling-and-disabling-scheduled-events"></a>Inschakelen en uitschakelen van geplande gebeurtenissen
Geplande gebeurtenissen is ingeschakeld voor uw service de eerste keer dat u een aanvraag voor gebeurtenissen. U moet een vertraagde reactie verwachten in de eerste aanroep van twee minuten.

Geplande gebeurtenissen is uitgeschakeld voor uw service als deze niet een aanvraag voor 24 uur.

### <a name="user-initiated-maintenance"></a>Gebruiker gestart onderhoud
Gebruiker gestart onderhoud van VM's via de Azure portal, API, CLI of PowerShell resulteert in een geplande gebeurtenis. U kunt de logica van de voorbereiding van onderhoud vervolgens testen in uw toepassing en uw toepassing kunt voorbereiden voor de gebruiker gestart onderhoud.

Als u een virtuele machine, een gebeurtenis met het type opnieuw starten `Reboot` is gepland. Als u een virtuele machine, een gebeurtenis met het type opnieuw implementeren `Redeploy` is gepland.

## <a name="use-the-api"></a>De API gebruiken

### <a name="headers"></a>Headers
Wanneer u een query Metadata-Service, moet u de header `Metadata:true` om te controleren of de aanvraag is niet per ongeluk omgeleid. De `Metadata:true` -header is vereist voor alle aanvragen van geplande gebeurtenissen. De header bevatten in de aanvraag is mislukt, resulteert in een reactie "Ongeldige aanvraag" van Metadata-Service.

### <a name="query-for-events"></a>Query voor gebeurtenissen
U kunt een query voor geplande gebeurtenissen door de volgende oproep verzenden:

#### <a name="bash"></a>Bash
```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

Een antwoord bevat een matrix met geplande gebeurtenissen. Een lege matrix betekent dat op dit moment geen gebeurtenissen worden gepland.
In het geval waarbij er geplande gebeurtenissen, het antwoord bevat een matrix van gebeurtenissen. 
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
|Eigenschap  |  Description |
| - | - |
| Gebeurtenis-id | Unieke id voor deze gebeurtenis. <br><br> Voorbeeld: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Type gebeurtenis | Impact die deze gebeurtenis is veroorzaakt. <br><br> Waarden: <br><ul><li> `Freeze`: De virtuele machine is gepland voor een paar seconden onderbreken. De CPU wordt onderbroken, maar er is geen invloed op geheugen, geopende bestanden of netwerkverbindingen. <li>`Reboot`: De virtuele machine is gepland voor opnieuw opstarten. (Niet-persistente geheugen is verbroken.) <li>`Redeploy`: De virtuele machine is gepland om te verplaatsen naar een ander knooppunt. (Tijdelijke schijven zijn verbroken.) |
| ResourceType | Het type resource dat deze gebeurtenis is van invloed op. <br><br> Waarden: <ul><li>`VirtualMachine`|
| Resources| Lijst met resources die deze gebeurtenis is van invloed op. De lijst kan worden gegarandeerd machines van maximaal één bevatten [updatedomein](manage-availability.md), maar deze bevat mogelijk niet alle machines in de UD. <br><br> Voorbeeld: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| EventStatus | De status van deze gebeurtenis. <br><br> Waarden: <ul><li>`Scheduled`: Deze gebeurtenis is gepland om te starten na de tijd die is opgegeven de `NotBefore` eigenschap.<li>`Started`: Deze gebeurtenis is gestart.</ul> Geen `Completed` of soortgelijke status ooit wordt aangeboden. De gebeurtenis wordt niet meer worden geretourneerd wanneer de gebeurtenis is voltooid.
| NotBefore| De tijd waarna deze gebeurtenis kunt starten. <br><br> Voorbeeld: <br><ul><li> Ma, 19 Sep 2016 18:29:47 GMT  |

### <a name="event-scheduling"></a>Gebeurtenis plannen
Elke gebeurtenis is gepland een minimale hoeveelheid tijd in de toekomst op basis van het gebeurtenistype. Dit moment wordt weergegeven in van een gebeurtenis `NotBefore` eigenschap. 

|Type gebeurtenis  | Minimale kennisgeving |
| - | - |
| Blokkeren| 15 minuten |
| Opnieuw opstarten | 15 minuten |
| Opnieuw implementeren | 10 minuten |

### <a name="start-an-event"></a>Een gebeurtenis starten 

Nadat u meer over een aanstaande agendagebeurtenis en voltooien van de logica voor het correct afsluiten, kunt u de openstaande gebeurtenis goedkeuren door vooraf een `POST` aanroep naar de Metadata-Service met `EventId`. Deze aanroep geeft aan dat Azure dat deze de minimale melding kunt verkorten tijd (indien mogelijk). 

De volgende JSON-voorbeeld wordt verwacht in de `POST` aanvraagtekst. De aanvraag moet bevatten een lijst met `StartRequests`. Elke `StartRequest` bevat `EventId` voor de gebeurtenis die u wilt versnellen:
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

#### <a name="bash-sample"></a>Bash-voorbeeld
```
curl -H Metadata:true -X POST -d '{"StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

> [!NOTE] 
> De gebeurtenis om door te gaan voor alle verzoeken een gebeurtenis kunt `Resources` in het geval, niet alleen de virtuele machine erkent dat de gebeurtenis. Daarom kunt u ervoor kiest een leider is geselecteerd voor de coördinatie van de bevestiging, dit kan net zo eenvoudig als het eerste virtuele machine in de `Resources` veld.

## <a name="python-sample"></a>Voorbeeld van Python 

Het volgende voorbeeld Metadata-Service zoekt naar geplande gebeurtenissen en keurt deze goed elke openstaande gebeurtenis:

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host " + this_host + " is scheduled for " + eventtype + " not before " + notbefore
            # Add logic for handling events here


def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)

if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Volgende stappen 
- Bekijk [geplande gebeurtenissen op Azure Friday](https://channel9.msdn.com/Shows/Azure-Friday/Using-Azure-Scheduled-Events-to-Prepare-for-VM-Maintenance) om te zien van een demo. 
- Controleer de geplande gebeurtenissen-codevoorbeelden in de [Azure Instance Metadata geplande gebeurtenissen GitHub-opslagplaats](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm).
- Meer informatie over de API's die beschikbaar zijn in de [Instance Metadata Service](instance-metadata-service.md).
- Meer informatie over [gepland onderhoud voor virtuele Linux-machines in Azure](planned-maintenance.md).
