---
title: Gebeurtenissen gepland voor de virtuele Linux-machines in Azure | Microsoft Docs
description: Gebeurtenissen plannen met behulp van Azure metagegevens Service voor uw virtuele Linux-machines.
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: ae9955253647f3277729e7905baf7bb07645de42
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="azure-metadata-service-scheduled-events-preview-for-linux-vms"></a>Azure Metadata-Service: Geplande gebeurtenissen (preview) voor virtuele Linux-machines

> [!NOTE] 
> Voorbeelden zijn beschikbaar gemaakt voor u op voorwaarde dat u akkoord met de gebruiksvoorwaarden gaat. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>

Geplande gebeurtenissen is een subservice onder Azure-Service voor metagegevens die uw toepassing tijd om voor te bereiden voor het onderhoud van de virtuele machine (VM). Biedt informatie over de aanstaande onderhoud gebeurtenissen (bijvoorbeeld opstarten vereist) zodat uw toepassing kunt voor ze voorbereiden en beperkt wordt onderbroken. Het is beschikbaar voor alle typen van de virtuele Machines van Azure, met inbegrip van PaaS en IaaS op Windows- en Linux. 

Zie voor meer informatie over gebeurtenissen in Windows gepland [gepland gebeurtenissen voor VM's van Windows](../windows/scheduled-events.md).

## <a name="why-use-scheduled-events"></a>Waarom gebruiken gebeurtenissen gepland?

Veel toepassingen kunnen profiteren van de tijd om voor te bereiden voor het onderhoud van de virtuele machine. De tijd kan worden gebruikt voor de toepassingsspecifieke taken uitvoeren die betere beschikbaarheid, betrouwbaarheid en onderhoudsvriendelijkheid, met inbegrip van: 

- Controlepunt en herstel.
- De verbinding is een verwerkingsstop.
- Failover van de primaire replica.
- Het verwijderen van een load balancer-groep.
- Logboekregistratie van gebeurtenissen.
- Correct afsluiten.

Uw toepassing kan met gebeurtenissen voor gepland detecteren wanneer onderhoud wordt uitgevoerd en taken voor het beperken van de gevolgen ervan wordt geactiveerd.  

Geplande gebeurtenissen biedt gebeurtenissen in de volgende gevallen:

- Platform geïnitieerde onderhoud (bijvoorbeeld een host-OS-update)
- Onderhoud gebruiker gestart (bijvoorbeeld als gebruiker opnieuw wordt opgestart of een virtuele machine redeploys)

## <a name="the-basics"></a>De basisbeginselen  

  Metadata-Service geeft informatie over het uitvoeren van virtuele machines met behulp van een REST-eindpunt is toegankelijk vanuit de virtuele machine. De informatie is beschikbaar via een nonroutable IP-adres, zodat deze niet buiten de virtuele machine wordt weergegeven.

### <a name="scope"></a>Bereik
Geplande gebeurtenissen worden afgeleverd bij:

- Alle VM's in een cloudservice.
- Alle VM's in een beschikbaarheidsset.
- Alle VM's in een groep scale set plaatsing. 

Als gevolg hiervan, Controleer de `Resources` veld in de gebeurtenis te identificeren welke virtuele machines worden beïnvloed.

### <a name="discover-the-endpoint"></a>Het eindpunt detecteren
Voor virtuele machines die zijn ingeschakeld voor virtuele netwerken, is het volledig eindpunt voor de nieuwste versie van gebeurtenissen voor gepland: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01`

In het geval waarbij een virtuele machine is gemaakt vanuit een virtueel netwerk Metadata-Service is beschikbaar via een nonroutable statische IP-adres, `169.254.169.254`.
Als de virtuele machine niet vanuit een virtueel netwerk, de standaard gevallen voor cloudservices en klassieke virtuele machines gemaakt is, worden extra logica is vereist voor het detecteren van het IP-adres te gebruiken. Voor meer informatie over hoe [de host-eindpunt detecteren](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm), raadpleegt u dit voorbeeld.

### <a name="versioning"></a>Versiebeheer 
De service gepland gebeurtenissen is samengesteld. Versies zijn verplicht en de huidige versie is `2017-08-01`.

| Versie | Releaseopmerkingen | 
| - | - | 
| 2017-08-01 | <li> De functienaam geplaatst onderstrepingsteken verwijderd uit resourcenamen voor Iaas VM 's<br><li>Metagegevens header vereiste afgedwongen voor alle aanvragen | 
| 2017-03-01 | <li>Openbare preview-versie


> [!NOTE] 
> Eerdere versies van de preview van geplande gebeurtenissen {laatste} wordt ondersteund als de api-versie. Deze indeling wordt niet meer ondersteund en in de toekomst wordt afgeschaft.

### <a name="use-headers"></a>Headers gebruiken
Wanneer u query Metadata Service uitvoert, moet u de header geven `Metadata:true` om te controleren of de aanvraag is niet per ongeluk omgeleid. De `Metadata:true` -header is vereist voor alle geplande gebeurtenissen aanvragen. De header opnemen in de aanvraag is mislukt, resulteert in een reactie 'Onjuiste aanvraag' van de Metadata-Service.

### <a name="enable-scheduled-events"></a>Geplande gebeurtenissen inschakelen
De eerste keer dat u een aanvraag voor geplande gebeurtenissen schakelt Azure impliciet de functie in op de virtuele machine. Een vertraagde antwoord als gevolg hiervan verwachten in de eerste aanroep van twee minuten.

> [!NOTE]
> Geplande gebeurtenissen wordt automatisch uitgeschakeld voor uw service als uw service het eindpunt niet aanroepen voor één dag. Na geplande gebeurtenissen voor uw service is uitgeschakeld, worden er zijn geen gebeurtenissen gemaakt voor onderhoud gebruiker gestart.

### <a name="user-initiated-maintenance"></a>Onderhoud gebruiker gestart
VM-onderhoud gebruiker gestart via de Azure-portal, API, CLI of PowerShell resulteert in een geplande gebeurtenis. U kunt de logica van de voorbereiding van onderhoud vervolgens testen in uw toepassing en uw toepassing kan worden voorbereid voor het onderhoud van gebruiker gestart.

Als u een virtuele machine, een gebeurtenis met het type opnieuw starten `Reboot` is gepland. Als u een virtuele machine, een gebeurtenis met het type opnieuw implementeren `Redeploy` is gepland.

> [!NOTE] 
> Op dit moment kan maximaal 100 gebruikers geïnitieerde onderhoudsbewerkingen worden tegelijkertijd gepland.

> [!NOTE] 
> Gebruiker gestart onderhoud die in geplande gebeurtenissen resulteert kan op dit moment niet worden geconfigureerd. Configuratiemogelijkheden is gepland voor een toekomstige release.

## <a name="use-the-api"></a>Gebruik de API

### <a name="query-for-events"></a>Query voor gebeurtenissen
U kunt een query voor geplande gebeurtenissen door de volgende oproep te plaatsen:

#### <a name="bash"></a>Bash
```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

Een antwoord bevat een matrix van geplande gebeurtenissen. Een lege matrix betekent dat momenteel geen gebeurtenissen worden gepland.
In het geval wanneer er geplande gebeurtenissen, het antwoord bevat een matrix van gebeurtenissen. 
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
| EventType | Impact die deze gebeurtenis wordt veroorzaakt. <br><br> Waarden: <br><ul><li> `Freeze`: De virtuele machine is gepland voor wacht een paar seconden. De CPU wordt onderbroken, maar er is geen invloed op geheugen, open bestanden of netwerkverbindingen. <li>`Reboot`: De virtuele machine is gepland voor opnieuw opstarten. (Niet-persistente geheugen is verbroken.) <li>`Redeploy`: De virtuele machine is gepland om te verplaatsen naar een ander knooppunt. (Tijdelijke schijven gaan verloren.) |
| ResourceType | Type resource dat deze gebeurtenis is van invloed op. <br><br> Waarden: <ul><li>`VirtualMachine`|
| Resources| Overzicht van deze gebeurtenis is van invloed op resources. De lijst is gegarandeerd bevatten machines van maximaal één [updatedomein](manage-availability.md), maar mogelijk niet alle machines in de UD bevat. <br><br> Voorbeeld: <br><ul><li> ['FrontEnd_IN_0', 'BackEnd_IN_0'] |
| EventStatus | Status van deze gebeurtenis. <br><br> Waarden: <ul><li>`Scheduled`: Deze gebeurtenis is gepland om te starten na de tijd die is opgegeven in de `NotBefore` eigenschap.<li>`Started`: Deze gebeurtenis is gestart.</ul> Geen `Completed` of soortgelijke status ooit wordt geleverd. De gebeurtenis wordt niet meer worden geretourneerd wanneer de gebeurtenis is voltooid.
| NotBefore| De tijd waarna deze gebeurtenis kunt starten. <br><br> Voorbeeld: <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>Gebeurtenis plannen
Elke gebeurtenis is gepland de minimale hoeveelheid tijd in de toekomst op basis van het gebeurtenistype. Deze tijd wordt weergegeven in een gebeurtenis `NotBefore` eigenschap. 

|EventType  | Minimale kennisgeving |
| - | - |
| Blokkeren| 15 minuten |
| Opnieuw opstarten | 15 minuten |
| Opnieuw implementeren | 10 minuten |

### <a name="start-an-event"></a>Start een gebeurtenis 

Nadat u meer informatie over van een aanstaande gebeurtenis en voltooien van de logica voor het correct afsluiten, kunt u de openstaande gebeurtenis goedkeuren door het maken van een `POST` aanroepen naar Metadata-Service met `EventId`. Deze aanroep geeft aan dat Azure dat deze de minimale melding kunt inkorten tijd (indien mogelijk). 

Het volgende JSON-voorbeeld wordt verwacht in de `POST` aanvraagtekst. De aanvraag moet bevatten een lijst met `StartRequests`. Elke `StartRequest` bevat `EventId` voor de gebeurtenis die u wilt versnellen:
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
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

> [!NOTE] 
> Een gebeurtenis zijn bevestigd, kan de gebeurtenis om door te gaan voor alle `Resources` in het gebeurtenislogboek, niet alleen de virtuele machine bevestigt dat de gebeurtenis. Daarom kunt u ervoor kiest een opvulteken voor het coördineren van de bevestiging die net zo eenvoudig als het eerste virtuele machine in de `Resources` veld.

## <a name="python-sample"></a>Python-voorbeeld 

Het volgende voorbeeld query Metadata-Service voor geplande gebeurtenissen en keurt deze goed elke openstaande gebeurtenis:

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
- Bekijk de codevoorbeelden gepland gebeurtenissen in de [Azure exemplaar metagegevens gepland gebeurtenissen Github-opslagplaats](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm).
- Meer informatie over de API's die beschikbaar zijn in de [exemplaar metagegevens Service](instance-metadata-service.md).
- Meer informatie over [gepland onderhoud voor Linux virtuele machines in Azure](planned-maintenance.md).
