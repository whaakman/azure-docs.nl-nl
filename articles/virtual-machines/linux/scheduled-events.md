---
title: Gebeurtenissen gepland voor de virtuele Linux-machines in Azure | Microsoft Docs
description: Gepland gebeurtenissen met de metagegevens van de Azure-service voor op uw virtuele Linux-machines.
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
ms.openlocfilehash: 2df39c64470e28bdf664d388041ae1b17d80db69
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-linux-vms"></a>Azure Service metagegevens: Geplande gebeurtenissen (Preview) voor virtuele Linux-machines

> [!NOTE] 
> Voorbeelden zijn beschikbaar gemaakt voor u op voorwaarde dat u akkoord met de gebruiksvoorwaarden gaat. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>

Geplande gebeurtenissen is een Azure-metagegevens-Service waarmee uw toepassing de tijd om voor te bereiden voor onderhoud op virtuele machines. Biedt informatie over de aanstaande onderhoud gebeurtenissen (bijvoorbeeld opstarten vereist) zodat uw toepassing kunt voor ze voorbereiden en beperkt wordt onderbroken. Het is beschikbaar voor alle virtuele Machine van Azure-typen, met inbegrip van PaaS en IaaS op Windows- en Linux. 

Zie voor meer informatie over gebeurtenissen in Windows gepland [gepland gebeurtenissen voor VM's van Windows](../windows/scheduled-events.md).

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

### <a name="scope"></a>Bereik
Geplande gebeurtenissen worden afgeleverd bij:
- Alle virtuele Machines in een Cloudservice
- Alle virtuele Machines in een Beschikbaarheidsset
- Alle virtuele Machines in een groep Scale Set plaatsing. 

Als gevolg hiervan moet u controleren de `Resources` veld in de gebeurtenis geven aan welke VM's gaan worden beïnvloed.

### <a name="discovering-the-endpoint"></a>Het eindpunt detecteren
Voor VNET ingeschakeld voor virtuele machines, is het volledig eindpunt voor de nieuwste versie van gebeurtenissen voor gepland: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01`

In het geval waarbij een virtuele Machine is gemaakt vanuit een virtueel netwerk (VNet), de metagegevensservice is beschikbaar via een statische niet-routeerbare IP-adres `169.254.169.254`.
Als de virtuele Machine niet vanuit een virtueel netwerk, de standaard gevallen voor cloudservices en klassieke virtuele machines gemaakt is, worden extra logica is vereist voor het detecteren van het IP-adres te gebruiken. Raadpleeg dit voorbeeld voor meer informatie over hoe [de host-eindpunt detecteren](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="versioning"></a>Versiebeheer 
De gebeurtenissen geplande Service is samengesteld. Versies zijn verplicht en de huidige versie is `2017-08-01`.

| Versie | Releaseopmerkingen | 
| - | - | 
| 2017-08-01 | <li> De functienaam geplaatst onderstrepingsteken verwijderd uit resourcenamen voor Iaas VM 's<br><li>Metagegevenskoptekst vereiste afgedwongen voor alle aanvragen | 
| 2017-03-01 | <li>Openbare Preview-versie


> [!NOTE] 
> Eerdere versies van de preview van geplande gebeurtenissen {laatste} wordt ondersteund als de api-versie. Deze indeling wordt niet meer ondersteund en in de toekomst wordt afgeschaft.

### <a name="using-headers"></a>Met behulp van headers
Wanneer u een query de Metadata-Service, moet u de header geven `Metadata:true` om te controleren of de aanvraag is niet per ongeluk wordt omgeleid. De `Metadata:true` -header is vereist voor alle geplande gebeurtenissen aanvragen. De header opnemen in de aanvraag is mislukt leidt tot een onjuiste aanvraag reactie van de Metadata-Service.

### <a name="enabling-scheduled-events"></a>Geplande gebeurtenissen inschakelen
De eerste keer dat u een aanvraag voor geplande gebeurtenissen schakelt Azure impliciet de functie in op de virtuele Machine. U moet een vertraagde antwoord als gevolg hiervan verwacht in de eerste aanroep van twee minuten.

> [!NOTE]
> Geplande gebeurtenissen wordt automatisch uitgeschakeld voor uw service als uw service het eindpunt niet aanroepen voor 1 dag. Zodra de geplande gebeurtenissen voor uw service is uitgeschakeld, zullen niet er gebeurtenissen die zijn gemaakt voor het onderhoud van de gebruiker worden gestart.

### <a name="user-initiated-maintenance"></a>Onderhoud van de gebruiker gestarte
Gebruiker gestart onderhoud op virtuele machines via de Azure-portal API, CLI of PowerShell resulteert in een geplande gebeurtenis. Dit kunt u de voorbereiding van onderhoud logica in uw toepassing testen en kan de toepassing om voor te bereiden voor het onderhoud van de gebruiker worden gestart.

Een virtuele machine opnieuw plant u een gebeurtenis met type `Reboot`. Een virtuele machine opnieuw plant u een gebeurtenis met type `Redeploy`.

> [!NOTE] 
> Op dit moment kan maximaal 100 gebruiker gestarte onderhoudsbewerkingen worden tegelijkertijd gepland.

> [!NOTE] 
> Gebruiker gestarte onderhoud gepland gebeurtenissen ertoe kan momenteel niet worden geconfigureerd. Configuratiemogelijkheden is gepland voor een toekomstige release.

## <a name="using-the-api"></a>Met behulp van de API

### <a name="query-for-events"></a>Query voor gebeurtenissen
U kunt een query voor geplande gebeurtenissen gewoon door de volgende oproep verzenden:

#### <a name="bash"></a>Bash
```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
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
| Resources| Lijst met bronnen die van invloed is op deze gebeurtenis. Dit kan worden gegarandeerd bevat machines van maximaal één [Updatedomein](manage-availability.md), maar mag niet alle machines in de UD bevatten. <br><br> Voorbeeld: <br><ul><li> ['FrontEnd_IN_0', 'BackEnd_IN_0'] |
| Gebeurtenisstatus | Status van deze gebeurtenis. <br><br> Waarden: <ul><li>`Scheduled`: Deze gebeurtenis is gepland om te starten na de tijd die is opgegeven in de `NotBefore` eigenschap.<li>`Started`: Deze gebeurtenis is gestart.</ul> Geen `Completed` of soortgelijke status ooit wordt geleverd; de gebeurtenis wordt niet meer worden geretourneerd wanneer de gebeurtenis is voltooid.
| NotBefore| De tijd waarna deze gebeurtenis wordt gestart. <br><br> Voorbeeld: <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>Gebeurtenis plannen
Elke gebeurtenis is gepland op basis van de minimale hoeveelheid tijd in de toekomst op gebeurtenistype. Deze tijd wordt weergegeven in een gebeurtenis `NotBefore` eigenschap. 

|EventType  | Minimale kennisgeving |
| - | - |
| Blokkeren| 15 minuten |
| Opnieuw opstarten | 15 minuten |
| Opnieuw implementeren | 10 minuten |

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

#### <a name="bash-sample"></a>Bash-voorbeeld
```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-08-01
```

> [!NOTE] 
> Een gebeurtenis zijn bevestigd, kan de gebeurtenis om door te gaan voor alle `Resources` in het gebeurtenislogboek, niet alleen de virtuele machine bevestigt dat de gebeurtenis. Daarom kunt u ervoor kiest een opvulteken voor het coördineren van de bevestiging die net zo eenvoudig als het eerste virtuele machine in de `Resources` veld.

## <a name="python-sample"></a>Python-voorbeeld 

Het volgende voorbeeld query op de metagegevensservice voor geplande gebeurtenissen en keurt deze goed elke openstaande gebeurtenis.

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
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
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)
   
if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Volgende stappen 
- Bekijk de codevoorbeelden gepland gebeurtenissen in de [Azure exemplaar metagegevens gepland gebeurtenissen Github-opslagplaats](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm)
- Meer informatie over de beschikbare API's in de [service-exemplaar metagegevens](instance-metadata-service.md).
- Meer informatie over [gepland onderhoud voor Linux virtuele machines in Azure](planned-maintenance.md).
