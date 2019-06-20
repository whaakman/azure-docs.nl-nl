---
title: Azure Service Fabric-releases
description: Opmerkingen bij de release voor de nieuwste functies en verbeteringen in Service Fabric.
author: athinanthny
manager: chackdan
ms.author: atsenthi
ms.date: 6/10/2019
ms.topic: conceptual
ms.service: service-fabric
hide_comments: true
hideEdit: true
ms.openlocfilehash: 78fb96cae3d3d128da608183f37771b2ecf66dcf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165361"
---
# <a name="service-fabric-releases"></a>Service Fabric-versies

| <a href="https://github.com/Azure/Service-Fabric-Troubleshooting-Guides" target="blank">Troubleshooting Guide</a> 
| <a href="https://github.com/Azure/service-fabric-issues" target="blank">bijhouden van problemen</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-support" target="blank">ondersteuningsopties</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-versions" target="blank">ondersteunde versies</a>  
|  <a href="https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0" target="blank">Codevoorbeelden</a>

In dit artikel vindt u meer informatie over de meest recente versies en updates voor de Service Fabric-runtime en SDK's.

## <a name="whats-new-in-service-fabric"></a>**Wat is er nieuw in Service Fabric**

### <a name="service-fabric-65"></a>Service Fabric 6.5

De meest recente Service Fabric-versie bevat ondersteuning, betrouwbaarheid en prestatieverbeteringen, nieuwe functies, oplossingen voor problemen en verbeteringen om het cluster en de toepassing levenscyclusbeheer vereenvoudigen.

> [!IMPORTANT]
> Service Fabric 6.5 is de definitieve versie met Service Fabric-ondersteuning voor hulpprogramma's in Visual Studio 2015. Klanten wordt aangeraden om te verplaatsen naar [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) voortaan.

Dit is wat is er nieuw in Service Fabric 6.5:

- Service Fabric Explorer bevat een [Store Afbeeldingsviewer](service-fabric-visualizing-your-cluster.md#image-store-viewer) voor de controle van toepassingen die u hebt geüpload naar de installatiekopieopslag.

- [Patch Orchestration-toepassing (POA)](service-fabric-patch-orchestration-application.md) versie [1.4.0](https://github.com/microsoft/Service-Fabric-POA/releases/tag/v1.4.0) bevat veel verbeteringen in de basisproblemen. Klanten van POA worden aanbevolen om te verplaatsen naar deze versie.

- [EventStore-Service is standaard ingeschakeld](service-fabric-visualizing-your-cluster.md#event-store) voor 6.5 van Service Fabric-clusters, tenzij u ervoor hebt gekozen uit.

- Toegevoegd [levenscyclusgebeurtenissen voor replica](service-fabric-diagnostics-event-generation-operational.md#replica-events) voor stateful services.

- [Betere zichtbaarheid van de status van de seed-knooppunt](service-fabric-understand-and-troubleshoot-with-system-health-reports.md#seed-node-status), met inbegrip van waarschuwingen als de cluster als een seed-knooppunt niet in orde is (*omlaag*, *verwijderd* of *onbekende*).

- [Service Fabric-toepassing Disaster Recovery Tool](https://github.com/Microsoft/Service-Fabric-AppDRTool) kunt stateful Service Fabric-services te snel worden hersteld wanneer de primaire cluster een ramp optreedt. Gegevens van primaire cluster worden continu gesynchroniseerd op de secundaire stand-by-toepassing met behulp van periodieke back-up en herstel.

- Visual Studio-ondersteuning voor [.NET Core-apps publiceren op Linux gebaseerde clusters](service-fabric-how-to-publish-linux-app-vs.md).

- [Azure Service Fabric CLI (SFCTL)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) automatisch voor Service Fabric 6.5 (en hoger) worden geïnstalleerd wanneer u een upgrade uitvoert of een nieuwe Linux-cluster maken in Azure.

- [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) is standaard geïnstalleerd op Mac OS-/ Linux-OneBox-clusters.

Zie voor meer details over de [releaseopmerkingen voor Service Fabric-6.5](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf).

## <a name="previous-versions"></a>Vorige versies

### <a name="service-fabric-64-releases"></a>Service Fabric 6.4 releases

| Releasedatum | Release | Meer informatie |
|---|---|---|
| En met 30 november 2018 | [Azure Service Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2018/11/30/azure-service-fabric-6-4-release/)  | [Releaseopmerkingen](https://msdnshared.blob.core.windows.net/media/2018/12/Service-Fabric-6.4-Release.pdf)|
| 12 december 2018 | [Azure Service Fabric 6.4 vernieuwen Release voor Windows-clusters](https://blogs.msdn.microsoft.com/azureservicefabric/2018/12/12/azure-service-fabric-6-4-refresh-for-windows-clusters/)  | [Releaseopmerkingen](https://msdnshared.blob.core.windows.net/media/2018/12/Links.pdf)  |
| 4 februari 2019 | [Azure Service Fabric 6.4 vernieuwen Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/02/04/azure-service-fabric-6-4-refresh-release/) | [Releaseopmerkingen](https://msdnshared.blob.core.windows.net/media/2019/02/Service-Fabric-6.4CU3-Release-Notes.pdf) |
| 4 maart 2019 | [Azure Service Fabric 6.4 vernieuwen Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/03/12/azure-service-fabric-6-4-refresh-release-2/) | [Releaseopmerkingen](https://msdnshared.blob.core.windows.net/media/2019/03/Service-Fabric-6.4CU4-Release-Notes.pdf)
| 8 april 2019 | [Azure Service Fabric 6.4 vernieuwen Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/04/08/azure-service-fabric-6-4-refresh-release-5/) | [Releaseopmerkingen](https://msdnshared.blob.core.windows.net/media/2019/04/Service-Fabric-6.4CU5-ReleaseNotes3.pdf)
| 2 mei 2019 | [Azure Service Fabric 6.4 vernieuwen Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/02/azure-service-fabric-6-4-refresh-release-3/) | [Releaseopmerkingen](https://msdnshared.blob.core.windows.net/media/2019/05/Service-Fabric-64CU6-Release-Notes-V2.pdf)
| 28 mei 2019 | [Azure Service Fabric 6.4 vernieuwen Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/28/azure-service-fabric-6-4-refresh-release-4/) | [Releaseopmerkingen](https://msdnshared.blob.core.windows.net/media/2019/05/Service_Fabric_64CU7_Release_Notes1.pdf)
