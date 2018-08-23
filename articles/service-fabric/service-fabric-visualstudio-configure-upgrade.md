---
title: De upgrade van een Service Fabric-toepassing configureren | Microsoft Docs
description: Informatie over het configureren van de instellingen voor het upgraden van een Service Fabric-toepassing met behulp van Microsoft Visual Studio.
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
ms.author: mikkelhegn
ms.openlocfilehash: 79120371ca2a62e5ef9f2bf38476635db12e9fcc
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42060562"
---
# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>De upgrade van een Service Fabric-toepassing in Visual Studio configureren
Visual Studio-hulpprogramma's voor Azure Service Fabric bieden ondersteuning voor de upgrade voor publicatie naar lokale of externe-clusters. Er zijn drie scenario's waarin u upgraden van uw toepassing naar een nieuwere versie in plaats van de toepassing wilt tijdens het testen en foutopsporing vervangen:

* Toepassingsgegevens niet verloren gaan tijdens de upgrade.
* Beschikbaarheid blijft hoge zodat er niet elke service wordt onderbroken tijdens de upgrade als er dat voldoende exemplaren van de service verdeeld over upgrade-domeinen.
* Tests kunnen voor een toepassing worden uitgevoerd terwijl deze wordt bijgewerkt.

## <a name="parameters-needed-to-upgrade"></a>Parameters die nodig zijn om bij te werken
U kunt kiezen uit twee typen implementatie: reguliere of een upgrade. Een normale implementatie wist u alle vorige informatie over de implementatie en gegevens op het cluster, terwijl een upgrade-implementatie blijft behouden in het. Wanneer u een upgrade uitvoert van een Service Fabric-toepassing in Visual Studio, moet u parameters toepassingsupgrade, status en controleer de beleidsregels voor opgeven. Parameters toepassingsupgrade helpen bij de controle van de upgrade, terwijl het statusbeleid selectievakje te bepalen of de upgrade geslaagd is. Zie [Service Fabric-toepassingsupgrade: Upgradeparameters](service-fabric-application-upgrade-parameters.md) voor meer informatie.

Er zijn drie upgrade modi: *bewaakte*, *UnmonitoredAuto*, en *UnmonitoredManual*.

* Een upgrade van een gecontroleerde automatiseert de upgrade en statuscontrole van de toepassing.
* Een upgrade UnmonitoredAuto automatiseert de upgrade, maar de statuscontrole van de toepassing wordt overgeslagen.
* Als u een upgrade UnmonitoredManual doet, moet u handmatig een upgrade van elk upgradedomein.

Elke upgrademodus vereist verschillende sets van parameters. Zie [parameters toepassingsupgrade](service-fabric-application-upgrade-parameters.md) voor meer informatie over de beschikbare opties voor de upgrade.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Upgrade van een Service Fabric-toepassing in Visual Studio
Als u de hulpprogramma's voor Visual Studio Service Fabric gebruikt om te upgraden van een Service Fabric-toepassing, kunt u een proces publiceren als een upgrade in plaats van een normale implementatie door het controleren van de **Upgrade van de toepassing** selectievakje.

### <a name="to-configure-the-upgrade-parameters"></a>Het configureren van de parameters voor het bijwerken
1. Klik op de **instellingen** knop naast het selectievakje in. De **Upgrade Parameters bewerken** in het dialoogvenster wordt weergegeven. De **Upgrade Parameters bewerken** in het dialoogvenster biedt ondersteuning voor de bewaakte UnmonitoredAuto en UnmonitoredManual upgrade modi.
2. Selecteer de upgrademodus die u wilt gebruiken en voer in het raster parameter.

    Elke parameter heeft de standaardwaarden. De optionele parameter *DefaultServiceTypeHealthPolicy* de invoer van een hash-tabel. Hier volgt een voorbeeld van de hash-tabel-invoerindeling voor *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* is een andere optionele parameter waarmee de invoer van een hash-tabel in de volgende indeling:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Hier volgt een voorbeeld van een echte gesprekken:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. Als u de upgrademodus UnmonitoredManual selecteert, moet u handmatig een PowerShell-console als u wilt doorgaan en voltooien van het upgradeproces te starten. Raadpleeg [Service Fabric-toepassingsupgrade: geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md) voor meer informatie over hoe handmatige upgrade werkt.

## <a name="upgrade-an-application-by-using-powershell"></a>Upgrade van een toepassing met behulp van PowerShell
U kunt PowerShell-cmdlets gebruiken om te upgraden van een Service Fabric-toepassing. Zie [Service Fabric application upgrade zelfstudie](service-fabric-application-upgrade-tutorial.md) en [Start ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade) voor gedetailleerde informatie.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Een selectievakje statusbeleid in het manifestbestand van de toepassing opgeven
Elke service in een Service Fabric-toepassing kunt hebben een eigen health beleidsparameters die de standaardwaarden worden overschreven. U kunt de parameterwaarden van deze in het manifestbestand van de toepassing opgeven.

Het volgende voorbeeld ziet hoe u een unieke controle van het statusbeleid voor elke service in het toepassingsmanifest toepast.

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
Zie voor meer informatie over het upgraden van een toepassing [upgraden van een toepassing met Visual Studio](service-fabric-application-upgrade-tutorial.md).
