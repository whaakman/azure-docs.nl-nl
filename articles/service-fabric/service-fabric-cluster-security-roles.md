---
title: 'Beveiliging van service Fabric-clusters: clientrollen | Microsoft Docs'
description: Dit artikel beschrijft de functies van de twee client en de machtigingen die aan de rollen.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: coreysa
editor: ''
ms.assetid: 7bc808d9-3609-46a1-ac12-b4f53bff98dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: ed000dc4be1ae45382d688d4a596ec745c69d0bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60711150"
---
# <a name="role-based-access-control-for-service-fabric-clients"></a>Op rollen gebaseerd toegangsbeheer voor Service Fabric-clients
Azure Service Fabric ondersteunt twee verschillende typen voor clients die zijn verbonden met een Service Fabric-cluster: beheerder en gebruiker. Toegangsbeheer kan de Clusterbeheerder om te beperken van toegang tot bepaalde bewerkingen voor een cluster voor verschillende groepen gebruikers, waardoor het cluster beter te beveiligen.  

**Beheerders** hebben volledige toegang tot de mogelijkheden voor beheer (met inbegrip van de mogelijkheden voor lezen/schrijven). Standaard **gebruikers** hebben alleen leestoegang tot de mogelijkheden (bijvoorbeeld querymogelijkheden), en de mogelijkheid om op te lossen, toepassingen en services.

U opgeven de twee clientrollen (de beheerder en client) op het moment van het maken van clusters door afzonderlijke certificaten voor elk. Zie [beveiliging van Service Fabric-clusters](service-fabric-cluster-security.md) voor meer informatie over het instellen van een beveiligd Service Fabric-cluster.

## <a name="default-access-control-settings"></a>Instellingen voor toegangsbeheer standaard
Het besturingselementtype van de beheerder toegang heeft volledige toegang to alle FabricClient-APIs. Het kunt lees- en schrijfbewerkingen op de Service Fabric-cluster, met inbegrip van de volgende bewerkingen uitvoeren:

### <a name="application-and-service-operations"></a>Toepassing en service-bewerkingen
* **CreateService**: service maken                             
* **CreateServiceFromTemplate**: service van het maken van sjabloon                             
* **UpdateService**: service-updates                             
* **DeleteService**: service verwijderen                             
* **ProvisionApplicationType**: type toepassingsinrichting                             
* **SubmitMetadata**: maken van de toepassing                               
* **DeleteApplication**: toepassing verwijderen                             
* **UpgradeApplication**: starten of onderbreken van upgrades van toepassingen                             
* **UnprovisionApplicationType**: toepassing type hierbij                             
* **MoveNextUpgradeDomain**: upgrades van toepassingen met een expliciete updatedomein wordt hervat                             
* **ReportUpgradeHealth**: het hervatten van de upgrades van toepassingen met de voortgang van de huidige upgrade                             
* **ReportHealth**: status rapporteren                             
* **PredeployPackageToNode**: voorafgaand aan implementatie-API                            
* **CodePackageControl**: codepakketten opnieuw te starten                             
* **RecoverPartition**: herstellen van een partitie                             
* **RecoverPartitions**: partities herstellen                             
* **RecoverServicePartitions**: servicepartities worden hersteld                             
* **RecoverSystemPartitions**: service systeempartities herstellen                             

### <a name="cluster-operations"></a>Clusterbewerkingen
* **ProvisionFabric**: MSI-bestand en/of cluster manifest van de inrichting                             
* **UpgradeFabric**: upgraden van clusters starten                             
* **UnprovisionFabric**: MSI-bestand en/of cluster manifest hierbij                         
* **MoveNextFabricUpgradeDomain**: upgraden van clusters met een expliciete updatedomein wordt hervat                             
* **ReportFabricUpgradeHealth**: upgraden van clusters met de voortgang van de huidige upgrade hervatten                             
* **StartInfrastructureTask**: infrastructuurtaken starten                             
* **FinishInfrastructureTask**: infrastructuurtaken is voltooid                             
* **InvokeInfrastructureCommand**: infrastructuur-opdrachten voor het beheer van taak                              
* **ActivateNode**: activeren van een knooppunt                             
* **DeactivateNode**: het deactiveren van een knooppunt                             
* **DeactivateNodesBatch**: het deactiveren van meerdere knooppunten                             
* **RemoveNodeDeactivations**: herstellen deactiveren op meerdere knooppunten                             
* **GetNodeDeactivationStatus**: deactiveringsstatus controleren                             
* **NodeStateRemoved**: status clusterknooppunt reporting verwijderd                             
* **ReportFault**: fout melden                             
* **FileContent**: image bestandsoverdracht store-client (extern aan cluster)                             
* **FileDownload**: image store-client-bestand downloaden Inleiding (externe aan cluster)                             
* **InternalList**: image store bestand lijst clientbewerking (intern)                             
* **Verwijder**: image bewerking voor store-client verwijderen                              
* **Uploaden**: image uploadbewerking voor store-client                             
* **NodeControl**: starten, stoppen en opnieuw starten van knooppunten                             
* **MoveReplicaControl**: replica's van het ene knooppunt naar de andere verplaatsen                             

### <a name="miscellaneous-operations"></a>Verschillende bewerkingen
* **Ping**: client-pings                             
* **Query**: alle query's zijn toegestaan
* **NameExists**: naamgeving URI bestaan controles                             

Het type gebruiker toegang besturingselement is standaard beperkt tot de volgende bewerkingen: 

* **EnumerateSubnames**: naamgeving van URI-inventarisatie                             
* **EnumerateProperties**: naamgeving van inventarisatie van eigenschappen                             
* **PropertyReadBatch**: eigenschap naming leesbewerkingen                             
* **GetServiceDescription**: service beschrijvingen servicemeldingen long-polling en lezen                             
* **ResolveService**: op basis van een klacht service-oplossing                             
* **ResolveNameOwner**: herleidende naming URI-eigenaar                             
* **ResolvePartition**: het omzetten van systeemservices                             
* **ServiceNotifications**: servicemeldingen op basis van gebeurtenissen                             
* **GetUpgradeStatus**: status van de toepassing upgrade polling                             
* **GetFabricUpgradeStatus**: polling-status van de cluster-upgrade                             
* **InvokeInfrastructureQuery**: infrastructuurtaken uitvoeren van query's                             
* **Lijst met**: store client bestandsbewerking lijst met installatiekopieën                             
* **ResetPartitionLoad**: opnieuw laden in te stellen voor een failover-eenheid                             
* **ToggleVerboseServicePlacementHealthReporting**: bij het omschakelen van uitgebreide service plaatsing status rapporteren                             

Het toegangsbeheer voor de beheerder heeft ook toegang tot de voorgaande bewerkingen.

## <a name="changing-default-settings-for-client-roles"></a>Wijzigen van de standaardinstellingen voor client-functies
In het manifestbestand van het cluster, kunt u beheerfuncties opgeven naar de client, indien nodig. U kunt de standaardinstellingen wijzigen door te gaan naar de **Infrastructuurinstellingen** optie tijdens [cluster maken](service-fabric-cluster-creation-via-portal.md), en het geven van de voorgaande instellingen in de **naam**,  **beheerder**, **gebruiker**, en **waarde** velden.

## <a name="next-steps"></a>Volgende stappen
[Beveiliging van service Fabric-clusters](service-fabric-cluster-security.md)

[Service Fabric-cluster maken](service-fabric-cluster-creation-via-portal.md)

