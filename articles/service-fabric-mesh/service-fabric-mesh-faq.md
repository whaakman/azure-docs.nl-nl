---
title: Veelgestelde vragen voor Azure Service Fabric NET | Microsoft Docs
description: Meer informatie over veelgestelde vragen en antwoorden voor Azure Service Fabric NET.
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 12/12/2018
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: jeanpaul.connock
ms.openlocfilehash: 27cf4d31f11eaf861d1cafc093d912aa15c8bec0
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55979748"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Veelgestelde vragen over Service Fabric NET

Azure Service Fabric Mesh is een volledig beheerde service waarmee ontwikkelaars microservices-toepassingen kunnen implementeren zonder virtuele machines, opslag of netwerken hoeven te beheren. In dit artikel vindt u antwoorden op veelgestelde vragen.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>Hoe ik een probleem melden of een vraag stellen?

Stel vragen, krijg antwoorden van Microsoft-technici en rapporteren van problemen in de [GitHub-opslagplaats voor service-fabric-mesh-preview](https://aka.ms/sfmeshissues).

## <a name="quota-and-cost"></a>Quota en kosten

### <a name="what-is-the-cost-of-participating-in-the-preview"></a>Wat zijn de kosten die deel uitmaken van de Preview-versie?

Er zijn momenteel geen kosten voor het implementeren van toepassingen of containers naar de Mesh-preview. Maar we raden u aan de resources die u implementeert en niet laten verwijderen, tenzij u ze actief test uitgevoerd.

### <a name="is-there-a-quota-limit-of-the-number-of-cores-and-ram"></a>Is er een limiet van het aantal cores en RAM-geheugen?

Ja. De quota voor elk abonnement zijn:

- Het aantal toepassingen: 5
- Kernen per toepassing: 12
- Totale RAM per toepassing: 48 GB
- Netwerk- en Uitgangsclaims eindpunten: 5
- Azure Volumes die u kunt koppelen: 10
- Het aantal Service-replica's: 3
- De grootste container die u kunt implementeren, is beperkt tot 4 kernen en 16GB RAM-geheugen.
- U kunt gedeeltelijke kerngeheugens toewijzen aan uw containers in stappen van 0,5 kernen, tot maximaal 6 kernen.

### <a name="how-long-can-i-leave-my-application-deployed"></a>Hoe lang laat ik mijn toepassing geïmplementeerd?

We hebben momenteel beperkt de levensduur van een toepassing in twee dagen. Dit is om optimaal gebruik te maken van de vrije kernen toegewezen aan de Preview-versie. Als gevolg hiervan u zijn alleen toegestaan om uit te voeren van een bepaalde implementatie continu gedurende 48 uur, waarna deze worden afgesloten.

Als dit gebeurt, kunt u controleren dat het systeem door het uitvoeren van afsluiten de `az mesh app show` opdracht in de Azure CLI. Als u wilt zien als het resultaat controleren `"status": "Failed", "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue."` 

Bijvoorbeeld: 

```cli
~$ az mesh app show --resource-group myResourceGroup --name helloWorldApp
{
  "debugParams": null,
  "description": "Service Fabric Mesh HelloWorld Application!",
  "diagnostics": null,
  "healthState": "Ok",
  "id": "/subscriptions/1134234-b756-4979-84re-09d671c0c345/resourcegroups/myResourceGroup/providers/Microsoft.ServiceFabricMesh/applications/helloWorldApp",
  "location": "eastus",
  "name": "helloWorldApp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "serviceNames": [
    "helloWorldService"
  ],
  "services": null,
  "status": "Failed",
  "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue.",
  "tags": {},
  "type": "Microsoft.ServiceFabricMesh/applications",
  "unhealthyEvaluation": null
}
```

Als u wilt verwijderen van de resourcegroep, gebruikt u de `az group delete <nameOfResourceGroup>` opdracht.

## <a name="supported-container-os-images"></a>Ondersteunde OS containerinstallatiekopieën

Als u ontwikkelt op een computer met Windows Fall Creators Update (versie 1709), kunt u alleen Windows-versie 1709 docker-installatiekopieën.

Als u ontwikkelt op een Windows-10 April 2018 update (versie 1803) machine, kunt u Windows-versie 1709 of Windows-versie 1803 docker-installatiekopieën.

De volgende container OS-installatiekopieën kunnen worden gebruikt om services te implementeren:

- Windows - windowsservercore en nanoserver
    - Windows Server versie 1709
    - Windows Server versie 1803
- Linux
    - Er zijn geen bekende beperkingen

## <a name="developer-experience-issues"></a>Problemen met Developer-ervaring

### <a name="dns-resolution-from-a-container-doesnt-work"></a>DNS-omzetting uit een container werkt niet

Uitgaande van DNS-query's uit een container met de Service Fabric-DNS-service mogelijk niet onder bepaalde omstandigheden. Dit wordt onderzocht. Om te verhelpen:

- Gebruik Windows Fall Creators update (versie 1709) of hoger als basis om de containerinstallatiekopie.
- Als u alleen de naam van de service niet werkt, probeert u de volledig gekwalificeerde naam: ServiceName.ApplicationName.
- Voeg in het Docker-bestand voor uw service, `EXPOSE <port>` waar de poort is u uw service wordt weergegeven op. Bijvoorbeeld:

```Dockerfile
EXPOSE 80
```

### <a name="dns-does-not-work-the-same-as-it-does-for-service-fabric-development-clusters-and-in-mesh"></a>DNS werkt niet hetzelfde als voor Service Fabric-clusters voor ontwikkeling en in Mesh

U moet mogelijk anders in uw lokale ontwikkelcluster dan in Azure net services verwijzen.

Gebruik in uw lokale ontwikkelcluster `{serviceName}.{applicationName}`. Gebruik in Azure Service Fabric NET `{servicename}`. 

Azure NET ondersteunt momenteel geen DNS-omzetting voor toepassingen.

Zie voor andere bekende DNS-problemen bij het uitvoeren van een Service Fabric-cluster voor ontwikkeling in Windows 10: [Fouten opsporen in Windows containers](/azure/service-fabric/service-fabric-how-to-debug-windows-containers) en [bekende DNS-problemen](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice#known-issues).

### <a name="networking"></a>Netwerken

Het service fabric-netwerk NAT kan verdwijnen tijdens het gebruik van uw app op uw lokale computer uitgevoerd. Als u wilt vaststellen of dit is gebeurd, voert u de volgende vanaf een opdrachtprompt:

`docker network ls` en opmerking of `servicefabric_nat` wordt vermeld.  Als dat niet het geval is, voer de volgende opdracht: `docker network create -d=nat --subnet 10.128.0.0/24 --gateway 10.128.0.1 servicefabric_nat`

Hiermee wordt het probleem op te lossen, zelfs als de app lokaal en in een slechte staat al is geïmplementeerd.

### <a name="issues-running-multiple-apps"></a>Problemen met het uitvoeren van meerdere apps

U kunt tegenkomen beschikbaarheid van de CPU en limieten voor alle toepassingen die worden opgelost. Om te verhelpen:
- Een cluster met vijf knooppunten maken.
- Minder CPU-gebruik in services via de app die is geïmplementeerd. Wijzig bijvoorbeeld in van uw service service.yaml bestand `cpu: 1.0` naar `cpu: 0.5`

Meerdere toepassingen kunnen niet worden geïmplementeerd op een cluster met één knooppunt. Om te verhelpen:
- Gebruik een cluster met vijf knooppunten bij het implementeren van meerdere apps in een lokaal cluster.
- Alle apps die u momenteel geen test verwijderd.

## <a name="feature-gaps-and-other-known-issues"></a>Functiehiaten en andere bekende problemen

### <a name="after-deploying-my-application-the-network-resource-associated-with-it-does-not-have-an-ip-address"></a>Na de implementatie van mijn toepassing heeft de netwerk-resource die is gekoppeld aan deze geen een IP-adres

Er is een bekend probleem waarbij het IP-adres niet beschikbaar meteen. Controleer de status van de netwerk-resource in een paar minuten om te zien van het bijbehorende IP-adres.

### <a name="my-application-fails-to-access-the-right-networkvolume-resource"></a>Mijn toepassing heeft geen toegang tot de juiste netwerk of het volume-resource

Gebruik de volledige resource-ID voor netwerken en volumes moet toegang hebben tot de gekoppelde resource in uw toepassingsmodel. Hier volgt een voorbeeld van het quickstart-voorbeeld:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

### <a name="when-i-scale-out-all-of-my-containers-are-affected-including-running-ones"></a>Wanneer ik de schaal vergroten, zijn alle van de containers betrokken, inclusief het uitvoeren van resources

Dit is een fout en is een oplossing wordt geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over Service Fabric NET de [overzicht](service-fabric-mesh-overview.md).
