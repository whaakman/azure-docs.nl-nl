---
title: U ziet Gast OS-familie 1 buiten gebruik stellen | Microsoft Docs
description: Bevat informatie over wanneer het Azure Gast OS-familie 1 buiten gebruik stellen is er gebeurd en als u last bepalen
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 37b422e9-0713-4a81-a942-f553ef478064
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 5/21/2017
ms.author: raiye
ms.openlocfilehash: 3178a09dab1cb972a3460d54dc9908fb95cce68b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="guest-os-family-1-retirement-notice"></a>U ziet het buiten gebruik stellen van Gast OS-familie 1
Het buiten gebruik stellen van OS-familie 1 werd eerst aangekondigd op 1 juni 2013.

**2 september 2014** The Azure gastbesturingssysteem (Gast OS) familie 1.x, die is gebaseerd op het besturingssysteem Windows Server 2008, is officieel buiten gebruik gesteld. Alle pogingen tot het implementeren van nieuwe services of upgraden van bestaande services met behulp van familie 1 mislukt met een foutbericht weergegeven waarin wordt gemeld dat de Gast OS-familie 1 heeft teruggetrokken.

**3 november 2014** uitgebreide ondersteuning voor gast OS-familie 1 beëindigd en het is volledig buiten gebruik gesteld. Alle services nog steeds op familie 1 zullen worden beïnvloed. We kunnen deze services op elk gewenst moment stoppen. Er is geen garantie dat uw services wordt nog uitgevoerd tenzij u handmatig ze zelf bijwerken.

Als u aanvullende vragen hebt, gaat u naar de [Cloud Services-Forums](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) of [Neem contact op met de ondersteuning van Azure](https://azure.microsoft.com/support/options/).

## <a name="are-you-affected"></a>Worden beïnvloed?
Uw Cloud-Services worden beïnvloed als een van de volgende van toepassing is:

1. U hebt een waarde van ' osFamily = "1" expliciet worden opgegeven in het bestand ServiceConfiguration.cscfg voor de Cloudservice.
2. U beschikt niet over een waarde voor osFamily expliciet worden opgegeven in het bestand ServiceConfiguration.cscfg voor de Cloudservice. Op dit moment wordt het systeem gebruikt de standaardwaarde '1' in dit geval.
3. De Azure-portal geeft de waarde van de familie gastbesturingssysteem als 'WindowsServer 2008'.

Om te zoeken die van uw cloud-services welke OS-familie worden uitgevoerd, kunt u het volgende script in Azure PowerShell uitvoeren als u moet [instellen van Azure PowerShell](/powershell/azureps-cmdlets-docs) eerste. Zie voor meer informatie over het script [Azure Gast OS-familie 1 einde van de levenscyclus: juni 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Uw cloudservices zullen worden beïnvloed door de OS-familie 1 buiten gebruik stellen als de kolom osFamily in de uitvoer van het script leeg is of bevat een '1'.

## <a name="recommendations-if-you-are-affected"></a>Aanbevelingen als u last
U wordt aangeraden dat u uw Cloud Service-rollen migreren naar een van de ondersteunde Gast typen besturingssystemen:

**Gast OS-familie 4.x** -Windows Server 2012 R2 *(aanbevolen)*

1. Zorg ervoor dat uw toepassing van SDK 2.1 of hoger met .NET framework 4.0, 4.5 en 4.5.1 gebruikmaakt.
2. Stel het osFamily-kenmerk op '4' in het bestand ServiceConfiguration.cscfg en implementeer uw cloudservice opnieuw.

**Gast OS-familie 3.x** -Windows Server 2012

1. Zorg ervoor dat uw toepassing van SDK 1.8 of hoger met .NET framework 4.0 of 4.5 gebruikmaakt.
2. Stel het osFamily-kenmerk op "3" in het bestand ServiceConfiguration.cscfg en implementeer uw cloudservice opnieuw.

**Gast OS-familie 2.x** -Windows Server 2008 R2

1. Zorg ervoor dat uw toepassing van SDK 1.3 gebruikmaakt en hoger met .NET framework 3.5 of 4.0.
2. Stel het osFamily-kenmerk op '2' in het bestand ServiceConfiguration.cscfg en implementeer uw cloudservice opnieuw.

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Uitgebreide ondersteuning voor de Gast OS-familie 1 afgelopen 3 november 2014
Cloudservices op de Gast OS-familie 1 worden niet langer ondersteund. Migratie uitgeschakeld familie 1 zo snel mogelijk om te voorkomen dat de service wordt onderbroken.  

## <a name="next-steps"></a>Volgende stappen
Bekijk de meest recente [Gast OS releases](cloud-services-guestos-update-matrix.md).
