---
title: Azure Service Fabric best practices voor bewaking | Microsoft Docs
description: Aanbevolen procedures voor het bewaken van Service Fabric-clusters en toepassingen.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: jeanpaul.connock
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 152a503181bba9e1ed39671794d512258759e251
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54913868"
---
# <a name="monitoring-and-diagnostics"></a>Controle en diagnose

[Controle en diagnose](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) essentieel zijn voor het ontwikkelen, testen en implementeren van workloads in een cloudomgeving. Bijvoorbeeld, kunt u bijhouden hoe uw toepassingen worden gebruikt, de acties die door de Service Fabric-platform, uw gebruik van resources met behulp van prestatiemeteritems en de algemene status van uw cluster. U kunt deze informatie gebruiken om te onderzoeken en oplossen van problemen en te voorkomen dat ze zich voordoet in de toekomst.

## <a name="application-monitoring"></a>Toepassingsbewaking

Bewaking van toepassingen wordt bijgehouden hoe functies en onderdelen van uw toepassing worden gebruikt. Controleren van uw toepassingen om ervoor te zorgen dat problemen die invloed hebben op uw gebruikers zijn opgepikt. Bewaking van toepassingen is de verantwoordelijkheid van de ontwikkeling van de toepassing en de bijbehorende services omdat deze uniek is voor de bedrijfslogica van uw toepassing. Het is raadzaam dat u toepassingsbewaking instelt met [Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-monitoring-aspnet), hulpprogramma voor het controleren van Azure-toepassing.

## <a name="cluster-monitoring"></a>Clusterbewaking

Een van de doelstellingen van Service Fabric is om toepassingen te maken tegen hardwarefouten. Dit doel wordt bereikt door middel van het platform systeemservices mogelijkheid voor het detecteren van problemen met infrastructuur en snel failover-workloads naar andere knooppunten in het cluster. Maar wat als de systeemservices zelf problemen heb? Of als bij een poging om te implementeren of verplaatsen van een werkbelasting, regels voor de plaatsing van services zijn geschonden? Service Fabric biedt diagnostische gegevens voor deze en andere problemen, om ervoor te zorgen dat u wordt geïnformeerd over de interactie van de Service Fabric-platform met uw toepassingen, services, containers en knooppunten.

Voor Windows-clusters, is het aanbevolen dat u instelt met behulp van Clusterbewaking [Diagnostics-Agent](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) en [Log Analytics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-setup).

Log Analytics is voor Linux-clusters, ook de aanbevolen hulpprogramma voor het Azure-platform en infrastructuur controleren. Diagnostische gegevens over Linux-platform vereisen andere configuratie als zijn aangegeven in [Linux Service Fabric-Clustergebeurtenissen in Syslog](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-syslog).

## <a name="infrastructure-monitoring"></a>Infrastructuur bewaken

[Log Analytics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-agent) wordt aanbevolen voor het bewaken van gebeurtenissen op toepassingsniveau voor cluster. Nadat u de Log Analytics-agent met uw werkruimte, configureren zoals beschreven in de vorige koppeling, kunt u zich voor het verzamelen van metrische gegevens voor prestaties zoals CPU-gebruik, de prestatiemeteritems .NET zoals proces niveau CPU-gebruik, prestaties van Service Fabric items zoals het aantal uitzonderingen van een betrouwbare service en de container metrische gegevens zoals CPU-gebruik.  U moet schrijven van Logboeken voor containers naar stdout of stderr, zodat ze beschikbaar zijn in Log Analytics.

## <a name="watchdogs"></a>Watchdogs

Over het algemeen is een watchdog een afzonderlijke service die de status en belasting alle services controleert, pingt eindpunten en rapporteert de gezondheid van onverwachte gebeurtenissen in het cluster. Dit kan helpen voorkomen dat er fouten die mogelijk niet gedetecteerd op basis van alleen op de prestaties van één service. Watchdogs zijn ook een goede plaats om te code kunt hosten die corrigerende actie waarvoor geen tussenkomst van de gebruiker, zoals het opruimen van de logboekbestanden in de opslag op bepaalde tijdsintervallen vereist. Een voorbeeld van service-implementatie in watchdog Zie [Linux Service Fabric-Clustergebeurtenissen in Syslog](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="next-steps"></a>Volgende stappen

* Aan de slag instrumenteren van uw toepassingen: [Toepassing op gebeurtenis- en logboekbestanden genereren](service-fabric-diagnostics-event-generation-app.md).
* Doorloop de stappen voor het instellen van Application Insights voor uw toepassing met [bewaken en diagnosticeren van een ASP.NET Core-toepassing in Service Fabric](service-fabric-tutorial-monitoring-aspnet.md).
* Meer informatie over het controleren van het platform en de Service Fabric u biedt gebeurtenissen: [Platform op gebeurtenis- en logboekbestanden genereren](service-fabric-diagnostics-event-generation-infra.md).
* Log Analytics-integratie met Service Fabric configureren: [Log Analytics instellen voor een cluster](service-fabric-diagnostics-oms-setup.md)
* Meer informatie over het instellen van Log Analytics voor het bewaken van containers: [Bewaking en diagnose voor Windows-Containers in Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).
* Zie het voorbeeld van de diagnostische gegevens over problemen en oplossingen met Service Fabric: [oplossen van veelvoorkomende scenario's](service-fabric-diagnostics-common-scenarios.md)
* Meer informatie over algemene aanbevelingen voor bewaking voor Azure-resources: [Aanbevolen procedures - bewaking en diagnose](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).