---
title: Configureren van de upgrade van een Service Fabric-toepassing | Microsoft Docs
description: Informatie over het configureren van instellingen voor het upgraden van een Service Fabric-toepassing met behulp van Microsoft Visual Studio.
services: service-fabric
documentationcenter: na
author: mikkelhegn
manager: mfussell
editor: tglee
ms.assetid: 1757ba85-0b7b-4f16-8a23-2ddaa61c86c6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/29/2017
ms.author: mikkelhegn
ms.openlocfilehash: 314b29a56e4651222822f40a116af97a7372ff2c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>De upgrade van een Service Fabric-toepassing in Visual Studio configureren
Visual Studio tools voor Azure Service Fabric bieden upgrade ondersteuning voor publicatie naar de lokale of externe clusters. Er zijn drie scenario's waarin u upgraden van uw toepassing naar een nieuwere versie in plaats van de toepassing wilt tijdens het testen en foutopsporing vervangen:

* Toepassingsgegevens niet verloren gaan tijdens de upgrade.
* Beschikbaarheid blijft hoge zodat er niet serviceonderbreking tijdens de upgrade als er dat voldoende service-exemplaren verspreid over upgradedomeinen.
* Tests kunnen worden uitgevoerd op basis van een toepassing, terwijl deze wordt bijgewerkt.

## <a name="parameters-needed-to-upgrade"></a>Parameters zijn vereist om bij te werken
U kunt kiezen uit twee soorten implementatie: reguliere of de upgrade. Een gewone implementatie worden gewist alle vorige implementatie-informatie en gegevens op het cluster, terwijl een upgrade-implementatie behouden deze blijft. Wanneer u een Service Fabric-toepassing in Visual Studio bijwerkt, moet u de status en -parameters voor het bijwerken van toepassing controleren beleidsregels bieden. Parameters voor het bijwerken van toepassing helpen bij het beheren van de upgrade, terwijl controle statusbeleid bepalen of de upgrade geslaagd is. Zie [upgrade van de Service Fabric-toepassing: upgrade parameters](service-fabric-application-upgrade-parameters.md) voor meer informatie.

Er zijn drie upgrade modi: *bewaakte*, *UnmonitoredAuto*, en *UnmonitoredManual*.

* De upgrade van een bewaakte automatiseert de upgrade en statuscontrole van de toepassing.
* Een upgrade UnmonitoredAuto automatiseert de upgrade, maar de statuscontrole van de toepassing wordt overgeslagen.
* Wanneer u een upgrade UnmonitoredManual doet, moet u elk upgradedomein handmatig bijwerken.

Elke upgrademodus vereist verschillende sets van parameters. Zie [parameters voor het bijwerken van toepassing](service-fabric-application-upgrade-parameters.md) voor meer informatie over de beschikbare opties voor de upgrade.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Upgrade van een Service Fabric-toepassing in Visual Studio
Als u de Visual Studio Service Fabric-hulpprogramma's voor het upgraden van een Service Fabric-toepassing gebruikt, kunt u een proces publiceren als een upgrade in plaats van een normale implementatie door het controleren van de **Upgrade van de toepassing** selectievakje.

### <a name="to-configure-the-upgrade-parameters"></a>Voor het configureren van de parameters voor het bijwerken
1. Klik op de **instellingen** knop naast het selectievakje in. De **Upgrade Parameters bewerken** dialoogvenster wordt weergegeven. De **Upgrade Parameters bewerken** in het dialoogvenster ondersteunt de bewaakte UnmonitoredAuto en UnmonitoredManual upgrade modi.
2. Selecteer de upgrademodus die u wilt gebruiken en geef vervolgens het raster parameter.

    Elke parameter heeft standaardwaarden. De optionele parameter *DefaultServiceTypeHealthPolicy* een hash-tabel invoer. Hier volgt een voorbeeld van de hash-tabel-invoerindeling voor *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *Servicetypehealthpolicymap; deze* is een andere optionele parameter waarmee de invoer van een hash-tabel in de volgende indeling:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Hier volgt een voorbeeld van de praktijk:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. Als u de upgrademodus UnmonitoredManual selecteert, moet u handmatig een PowerShell-console om te gaan en klaar bent met het upgradeproces te starten. Raadpleeg [upgrade van de Service Fabric-toepassing: geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md) voor meer informatie over hoe handmatige upgrade werkt.

## <a name="upgrade-an-application-by-using-powershell"></a>Upgrade van een toepassing met behulp van PowerShell
PowerShell-cmdlets kunt u een Service Fabric-toepassing upgraden. Zie [upgrade zelfstudie over Service Fabric](service-fabric-application-upgrade-tutorial.md) en [Start ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) voor gedetailleerde informatie.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Een selectievakje statusbeleid in het manifestbestand van de toepassing opgeven
Elke service in een Service Fabric-toepassing kan een eigen parameters voor het beleid van health dat voorrang de standaardwaarden boven hebben. U kunt deze parameterwaarden in het manifestbestand van de toepassing opgeven.

Het volgende voorbeeld laat zien hoe een unieke controle van het statusbeleid voor elke service in het toepassingsmanifest toepassen.

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
Zie voor meer informatie over het implementeren van een toepassing [implementeren van een bestaande toepassing in Azure Service Fabric](service-fabric-deploy-existing-app.md).