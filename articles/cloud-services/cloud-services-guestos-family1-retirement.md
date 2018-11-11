---
title: U ziet Guest OS family 1 buiten gebruik stellen | Microsoft Docs
description: Bevat informatie over wanneer de Azure Guest OS Family 1 buiten gebruik stellen is er gebeurd en hoe u bepaalt als u ondervindt
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: ''
ms.assetid: 37b422e9-0713-4a81-a942-f553ef478064
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 5/21/2017
ms.author: raiye
ms.openlocfilehash: d6429766b6aac547fd99279659acb1067298e77c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244675"
---
# <a name="guest-os-family-1-retirement-notice"></a>Kennisgeving van buitengebruikstelling van serie 1 van gastbesturingssysteem
De buitengebruikstelling van serie 1 werd eerst aangekondigd op 1 juni 2013.

**2 september 2014** (Gastbesturingssysteem) voor de Azure-Gast-besturingssysteem familie 1.x, die is gebaseerd op het besturingssysteem Windows Server 2008, is officieel buiten gebruik gesteld. Alle pogingen voor de nieuwe services uitrollen of upgrade bestaande services met behulp van serie 1 mislukt met een foutbericht weergegeven waarin wordt gemeld dat de Gast OS Family 1 is verouderd.

**3 november 2014** uitgebreide ondersteuning voor Guest OS Family 1 is beëindigd en het is volledig buiten gebruik gesteld. Alle services nog steeds op serie 1 wordt beïnvloed. We kunnen deze services op elk gewenst moment stoppen. Er is geen garantie dat uw services uitgevoerd worden, tenzij u handmatig een upgrade van ze zelf.

Als u nog vragen hebt, gaat u naar de [Cloud Services-Forums](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) of [Neem contact op met ondersteuning van Azure](https://azure.microsoft.com/support/options/).

## <a name="are-you-affected"></a>Worden beïnvloed?
Uw Cloud-Services worden beïnvloed als een van de volgende van toepassing is:

1. U hebt een waarde van "osFamily ="1"expliciet worden opgegeven in het bestand ServiceConfiguration.cscfg voor je Cloud Service.
2. U beschikt niet over een waarde voor osFamily expliciet worden opgegeven in het bestand ServiceConfiguration.cscfg voor je Cloud Service. Op dit moment het systeem maakt gebruik van de standaardwaarde van '1' in dit geval.
3. De Azure-portal geeft een lijst van de waarde van de Gast-besturingssysteem familie als 'WindowsServer 2008'.

Als u wilt zoeken die van uw cloud-services welke type besturingssysteem worden uitgevoerd, kunt u het volgende script in Azure PowerShell uitvoeren als u moet [instellen van Azure PowerShell](/powershell/azureps-cmdlets-docs) eerste. Zie voor meer informatie over het script [Azure Gast OS Family 1 einde van de levenscyclus: juni 2014](https://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Uw cloudservices worden beïnvloed door de buitengebruikstelling van serie 1 als de osFamily-kolom in de uitvoer van het script leeg is of bevat een '1'.

## <a name="recommendations-if-you-are-affected"></a>Aanbevelingen als u ondervindt
U wordt aangeraden dat u uw Cloud Service-rollen migreren naar een van de ondersteunde Gast OS Families:

**Guest OS family 4.x** -Windows Server 2012 R2 *(aanbevolen)*

1. Zorg ervoor dat uw toepassing met .NET framework 4.0, 4.5 en 4.5.1 SDK 2.1 of hoger gebruikt.
2. De osFamily-kenmerk voor het "4" in het bestand ServiceConfiguration.cscfg ingesteld en implementeer uw cloudservice opnieuw.

**Guest OS family 3.x** -Windows Server 2012

1. Zorg ervoor dat uw toepassing met .NET framework 4.0 of 4.5 SDK 1.8 of later gebruikt.
2. De osFamily-kenmerk op '3' in het bestand ServiceConfiguration.cscfg ingesteld en implementeer uw cloudservice opnieuw.

**Guest OS family 2.x** -Windows Server 2008 R2

1. Zorg ervoor dat uw toepassing met behulp van SDK 1.3 en hoger met .NET framework 3.5 of 4.0.
2. De osFamily-kenmerk op '2' in het bestand ServiceConfiguration.cscfg ingesteld en implementeer uw cloudservice opnieuw.

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Uitgebreide ondersteuning voor Guest OS Family 1 afgelopen 3 november 2014
Cloudservices op Guest OS family 1 worden niet meer ondersteund. Migratie uitgeschakeld family 1 zo snel mogelijk om te voorkomen dat de service wordt onderbroken.  

## <a name="next-steps"></a>Volgende stappen
Bekijk de meest recente [releases van Gastbesturingssystemen](cloud-services-guestos-update-matrix.md).
