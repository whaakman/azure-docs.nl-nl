---
title: Een zelfstandig Azure Service Fabric cluster upgraden | Microsoft Docs
description: Meer informatie over het upgraden van de versie of de configuratie van een Azure Service Fabric zelfstandige-cluster.  T
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/12/2018
ms.author: aljo
ms.openlocfilehash: 1d96a2e81917af5e80bb847ea25610ccb71ad70f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661494"
---
# <a name="upgrading-and-updating-a-service-fabric-standalone-cluster"></a>Upgraden en het bijwerken van een zelfstandige Service Fabric-cluster

Voor elk modern systeem is ontwerpen voor kunnen erg belangrijk voor het succes van uw product op lange termijn te bereiken. Een zelfstandige cluster van Azure Service Fabric is een resource waarvan u eigenaar bent. Dit artikel wordt beschreven wat kan worden bijgewerkt of bijgewerkt.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>De fabric-versie die wordt uitgevoerd op uw cluster beheren
Zorg ervoor dat uw cluster altijd voert een [ondersteunde versie van Service Fabric](service-fabric-versions.md). Wanneer Microsoft de release van een nieuwe versie van Service Fabric kondigt, betekent dit dat de vorige versie zich na een minimum van 60 dagen vanaf de datum van de aankondiging is gemarkeerd voor einde van ondersteuning. Nieuwe releases worden aangekondigd [op de blog van het Service Fabric-team](https://blogs.msdn.microsoft.com/azureservicefabric/). De nieuwe versie is beschikbaar om te kiezen op dat moment.

U kunt instellen dat uw cluster voor het ontvangen van automatische infrastructuurupgrades als ze door Microsoft worden uitgegeven of u kunt handmatig een ondersteunde fabric-versie die u wilt dat uw cluster op selecteren. Lees voor meer informatie, [Upgrade van de Service Fabric-versie die wordt uitgevoerd op uw cluster](service-fabric-cluster-upgrade-windows-server.md).

## <a name="customize-configuration-settings"></a>Configuratie-instellingen aanpassen

Veel verschillende [configuratie-instellingen](service-fabric-cluster-manifest.md) kan worden ingesteld in de *ClusterConfig.json* -bestand, zoals het betrouwbaarheidsniveau dat van de cluster- en knooppunt-eigenschappen.  Voor meer informatie lezen [Upgrade van de configuratie van een zelfstandige cluster](service-fabric-cluster-config-upgrade-windows-server.md).  Tal van andere, meer geavanceerde, instellingen kunnen ook worden aangepast.  Lees voor meer informatie, [infrastructuurinstellingen voor Service Fabric-cluster](service-fabric-cluster-fabric-settings.md).

## <a name="define-node-properties"></a>Eigenschappen van het knooppunt definiëren
Soms wilt u mogelijk om ervoor te zorgen dat bepaalde workloads worden uitgevoerd alleen op bepaalde typen knooppunten in het cluster. Bijvoorbeeld enkele workload mogelijk GPU's of SSD's en andere niet. Voor elk van de knooppunttypen in een cluster, kunt u eigenschappen van aangepaste knooppunt toevoegen aan de clusterknooppunten. Plaatsingsbeperkingen zijn de instructies die zijn gekoppeld aan afzonderlijke services ervan die voor een of meer eigenschappen van het knooppunt selecteren. Plaatsingsbeperkingen definiëren waar services moeten worden uitgevoerd.

Lees voor meer informatie over het gebruik van plaatsingsbeperkingen, eigenschappen van het knooppunt en hoe u ze definiëren [knooppunteigenschappen en plaatsingsbeperkingen](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).
 

## <a name="add-capacity-metrics"></a>Metrische gegevens over capaciteit toevoegen
U kunt aangepaste metrische gegevens over capaciteit die u wilt gebruiken in uw toepassingen om te rapporteren over de belasting toevoegen voor elk van de knooppunttypen. Raadpleeg voor meer informatie over het gebruik van metrische gegevens over capaciteit om te rapporteren over de belasting op de Service Fabric Cluster Resource Manager-documenten op [met een beschrijving van uw Cluster](service-fabric-cluster-resource-manager-cluster-description.md) en [metrische gegevens en belasting](service-fabric-cluster-resource-manager-metrics.md).

## <a name="patch-the-os-in-the-cluster-nodes"></a>Patch uitvoeren voor het besturingssysteem in de clusterknooppunten
De patch orchestration-toepassing (POA) is een Service Fabric-toepassing waarmee het besturingssysteem op een Service Fabric-cluster zonder uitvaltijd patches worden geautomatiseerd. De [Patch Orchestration-toepassing voor Windows](service-fabric-patch-orchestration-application.md) kan worden geïmplementeerd in uw cluster om patches te installeren op een gecoördineerde manier terwijl de services die beschikbaar zijn voortdurend. 


## <a name="next-steps"></a>Volgende stappen
* Informatie over het aanpassen van enkele van de [service fabric-cluster infrastructuurinstellingen](service-fabric-cluster-fabric-settings.md)
* Meer informatie over het [in en uit een cluster schalen](service-fabric-cluster-scale-up-down.md)
* Meer informatie over [upgrades van toepassingen](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
