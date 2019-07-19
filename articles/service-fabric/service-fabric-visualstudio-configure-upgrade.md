---
title: De upgrade van een Service Fabric toepassing configureren | Microsoft Docs
description: Meer informatie over het configureren van de instellingen voor het upgraden van een Service Fabric-toepassing met behulp van micro soft Visual Studio.
services: service-fabric
documentationcenter: na
author: mikkelhegn
manager: mfussell
editor: tglee
ms.assetid: 1757ba85-0b7b-4f16-8a23-2ddaa61c86c6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/29/2017
ms.author: mikhegn
ms.openlocfilehash: 5979541146b7cd7b854f35c5bf204e71208f066b
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67876759"
---
# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>De upgrade van een Service Fabric-toepassing in Visual Studio configureren
Visual Studio Tools voor Azure Service Fabric bieden upgrade ondersteuning voor het publiceren naar lokale of externe clusters. Er zijn drie scenario's waarin u uw toepassing wilt bijwerken naar een nieuwere versie in plaats van de toepassing te vervangen tijdens het testen en fout opsporing:

* Toepassings gegevens gaan verloren tijdens de upgrade.
* De beschik baarheid blijft hoog, waardoor er tijdens de upgrade geen onderbreking van de service is, als er voldoende service-exemplaren over zijn verdeeld over upgrade domeinen.
* Tests kunnen worden uitgevoerd op een toepassing tijdens de upgrade.

## <a name="parameters-needed-to-upgrade"></a>Vereiste para meters voor upgrade
U kunt kiezen uit twee typen implementatie: standaard of upgrade. Bij een reguliere implementatie worden alle eerdere implementatie-informatie en-gegevens op het cluster gewist, terwijl de implementatie van een upgrade wordt behouden. Wanneer u een Service Fabric-toepassing in Visual Studio bijwerkt, moet u de para meters voor de toepassings upgrade en het status controlebeleid opgeven. De para meters voor de upgrade van de toepassing helpen de upgrade te beheren, terwijl het status controlebeleid bepaalt of de upgrade is geslaagd. Zie [service Fabric Application upgrade: upgrade-para meters](service-fabric-application-upgrade-parameters.md) voor meer informatie.

Er zijn drie upgrade modi: *Bewaakt*, *UnmonitoredAuto*en *UnmonitoredManual*.

* Met een bewaakte upgrade wordt de upgrade en de status controle van de toepassing geautomatiseerd.
* Met een UnmonitoredAuto-upgrade wordt de upgrade geautomatiseerd, maar wordt de status controle van de toepassing overgeslagen.
* Wanneer u een UnmonitoredManual-upgrade uitvoert, moet u elk upgrade domein hand matig bijwerken.

Voor elke upgrade modus zijn verschillende sets para meters vereist. Zie [para meters voor toepassings upgrades](service-fabric-application-upgrade-parameters.md) voor meer informatie over de beschik bare upgrade opties.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Een Service Fabric-toepassing bijwerken in Visual Studio
Als u de Service Fabric-hulpprogram ma's van Visual Studio gebruikt om een Service Fabric toepassing te upgraden, kunt u een publicatie proces opgeven als een upgrade in plaats van een normale implementatie door het selectie vakje **upgrade van de toepassing** in te scha kelen.

### <a name="to-configure-the-upgrade-parameters"></a>De upgrade parameters configureren
1. Klik op de knop **instellingen** naast het selectie vakje. Het dialoog venster **upgrade parameters bewerken** wordt weer gegeven. In het dialoog venster **upgrade parameters bewerken** worden de bewaakte, UnmonitoredAuto-en UnmonitoredManual-upgrade modi ondersteund.
2. Selecteer de upgrade modus die u wilt gebruiken en vul vervolgens het parameter raster in.

    Elke para meter heeft standaard waarden. Voor de optionele para meter *DefaultServiceTypeHealthPolicy* wordt een invoer van een hash-tabel gebruikt. Hier volgt een voor beeld van de invoer indeling voor hash-tabellen voor *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* is een andere optionele para meter waarmee een invoer van hash-tabellen in de volgende indeling wordt gebruikt:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Hier volgt een voor beeld van een Real-Life:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. Als u de UnmonitoredManual-upgrade modus selecteert, moet u hand matig een Power shell-console starten om door te gaan en het upgrade proces te volt ooien. Raadpleeg [service Fabric toepassings upgrade: geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md) voor meer informatie over de werking van hand matige upgrades.

## <a name="upgrade-an-application-by-using-powershell"></a>Een toepassing bijwerken met behulp van Power shell
U kunt Power shell-cmdlets gebruiken om een Service Fabric-toepassing bij te werken. Zie [service Fabric zelf studie](service-fabric-application-upgrade-tutorial.md) voor de upgrade van toepassingen en [Start-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade) voor meer informatie.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Een beleid voor status controle opgeven in het manifest bestand van de toepassing
Elke service in een Service Fabric toepassing kan een eigen status beleids parameter hebben waarmee de standaard waarden worden overschreven. U kunt deze parameter waarden opgeven in het manifest bestand van de toepassing.

In het volgende voor beeld ziet u hoe u een uniek beleid voor status controle toepast voor elke service in het toepassings manifest.

```xml
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Volgende stappen
Zie [een toepassing upgraden met Visual Studio](service-fabric-application-upgrade-tutorial.md)voor meer informatie over het upgraden van een toepassing.
