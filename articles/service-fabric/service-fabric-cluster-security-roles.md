---
title: 'Beveiliging voor service Fabric-cluster: client rollen | Microsoft Docs'
description: In dit artikel beschrijft de twee client-rollen en de machtigingen die aan de rollen.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: coreysa
editor: 
ms.assetid: 7bc808d9-3609-46a1-ac12-b4f53bff98dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 85935e60bba4b27972282700e2e9c9a22b403bdb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="role-based-access-control-for-service-fabric-clients"></a>Toegangsbeheer op basis van rollen voor Service Fabric-clients
Azure Service Fabric ondersteunt twee verschillende toegangsrechten besturingselementtypen voor clients die zijn verbonden met een Service Fabric-cluster: beheerder en gebruiker. Toegangsbeheer kan de Clusterbeheerder om te beperken van toegang tot bepaalde clusterbewerkingen voor verschillende groepen gebruikers, zodat het cluster beter te beveiligen.  

**Beheerders** hebben volledige toegang tot beheermogelijkheden (inclusief lezen/schrijven-mogelijkheden). Standaard **gebruikers** hebben alleen leestoegang tot beheermogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.

U opgeven de twee client-rollen (administrator en client) op het moment van het maken van het cluster door afzonderlijke certificaten voor elk. Zie [Service Fabric-clusterbeveiliging](service-fabric-cluster-security.md) voor meer informatie over het instellen van een beveiligde Service Fabric-cluster.

## <a name="default-access-control-settings"></a>Instellingen voor toegangsbeheer standaard
Het type beheerder toegang besturingselement heeft volledige toegang to alle FabricClient-APIs. Het kunt lees- en schrijfbewerkingen op de Service Fabric-cluster, met inbegrip van de volgende bewerkingen uitvoeren:

### <a name="application-and-service-operations"></a>Toepassing en service-activiteiten
* **CreateService**: service maken                             
* **CreateServiceFromTemplate**: service gemaakt op basis van sjabloon                             
* **UpdateService**: service-updates                             
* **DeleteService**: verwijderen van een service                             
* **ProvisionApplicationType**: toepassing type inrichting                             
* **SubmitMetadata**: maken van de toepassing                               
* **DeleteApplication**: toepassing verwijderen                             
* **UpgradeApplication**: starten of toepassingsupgrades onderbreken                             
* **UnprovisionApplicationType**: toepassing type hierbij                             
* **MoveNextUpgradeDomain**: toepassingsupgrades met een expliciete updatedomein hervatten                             
* **ReportUpgradeHealth**: toepassingsupgrades met de voortgang van de huidige upgrade hervatten                             
* **ReportHealth**: reporting health                             
* **PredeployPackageToNode**: voorafgaand aan implementatie API                            
* **CodePackageControl**: code pakketten opnieuw te starten                             
* **RecoverPartition**: herstellen van een partitie                             
* **RecoverPartitions**: partities herstellen                             
* **RecoverServicePartitions**: servicepartities herstellen                             
* **RecoverSystemPartitions**: service systeempartities herstellen                             

### <a name="cluster-operations"></a>Bewerkingen voor een cluster
* **ProvisionFabric**: MSI en de clusternaambron manifest inrichten                             
* **UpgradeFabric**: beginnen cluster                             
* **UnprovisionFabric**: MSI en de clusternaambron manifest hierbij                         
* **MoveNextFabricUpgradeDomain**: upgrades van de cluster met een expliciete updatedomein hervatten                             
* **ReportFabricUpgradeHealth**: cluster-upgrades met de voortgang van de huidige upgrade hervatten                             
* **StartInfrastructureTask**: infrastructuurtaken starten                             
* **FinishInfrastructureTask**: infrastructuurtaken is voltooid                             
* **InvokeInfrastructureCommand**: infrastructuur-opdrachten voor het beheer van taak                              
* **ActivateNode**: activeren van een knooppunt                             
* **DeactivateNode**: deactiveren van een knooppunt                             
* **DeactivateNodesBatch**: meerdere knooppunten deactiveren                             
* **RemoveNodeDeactivations**: omzetten deactivering op meerdere knooppunten                             
* **GetNodeDeactivationStatus**: deactivering van de status controleren                             
* **NodeStateRemoved**: status van knooppunt reporting verwijderd                             
* **ReportFault**: fout met betrekking tot rapportage                             
* **FileContent**: image store client bestandsoverdracht (extern naar cluster)                             
* **FileDownload**: image store client bestand downloaden Inleiding (externe aan cluster)                             
* **InternalList**: image store bestand lijst clientbewerking (intern)                             
* **Verwijderen**: image store client delete-bewerking                              
* **Uploaden**: image store-clientbewerking uploaden                             
* **NodeControl**: starten, stoppen en opnieuw starten van de knooppunten                             
* **MoveReplicaControl**: replica's van het ene knooppunt naar een andere verplaatsen                             

### <a name="miscellaneous-operations"></a>Verschillende bewerkingen
* **Ping**: client-pings                             
* **Query**: alle query's zijn toegestaan
* **NameExists**: naming URI bestaan controles                             

Het besturingselementtype van de gebruiker toegang is standaard beperkt tot de volgende bewerkingen: 

* **EnumerateSubnames**: naming URI opsomming                             
* **EnumerateProperties**: eigenschap opsomming naming                             
* **PropertyReadBatch**: eigenschap naming leesbewerkingen                             
* **GetServiceDescription**: long poll servicemeldingen en lezen service beschrijvingen                             
* **ResolveService**: compatibele gebaseerde service resolutie                             
* **ResolveNameOwner**: het omzetten van namen URI eigenaar                             
* **ResolvePartition**: het omzetten van systeemservices                             
* **ServiceNotifications**: servicemeldingen op basis van gebeurtenissen                             
* **GetUpgradeStatus**: polling upgradestatus van toepassing                             
* **GetFabricUpgradeStatus**: polling upgradestatus van cluster                             
* **InvokeInfrastructureQuery**: infrastructuurtaken opvragen                             
* **Lijst**: image store client bestandsbewerking lijst                             
* **ResetPartitionLoad**: opnieuw instellen van belasting van een failover-eenheid                             
* **ToggleVerboseServicePlacementHealthReporting**: schakelen uitgebreide plaatsing health servicerapportages                             

Het toegangsbeheer voor de beheerder heeft ook toegang tot de voorgaande bewerkingen.

## <a name="changing-default-settings-for-client-roles"></a>Standaardinstellingen wijzigen voor client-functies
In het manifestbestand van de cluster kunt u admin mogelijkheden opgeven naar de client, indien nodig. U kunt de standaardinstellingen wijzigen door te gaan naar de **Fabric instellingen** optie tijdens [cluster maken](service-fabric-cluster-creation-via-portal.md), en het geven van de voorgaande instellingen in de **naam**, **admin**, **gebruiker**, en **waarde** velden.

## <a name="next-steps"></a>Volgende stappen
[Beveiliging voor service Fabric-cluster](service-fabric-cluster-security.md)

[Service Fabric-cluster maken](service-fabric-cluster-creation-via-portal.md)

