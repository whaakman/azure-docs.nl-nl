---
title: NSG controle met Azure Network Watcher-beveiligingsgroepweergave automatiseren | Microsoft Docs
description: Deze pagina vindt u instructies over het configureren van de controle van een Netwerkbeveiligingsgroep
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 78a01bcf-74fe-402a-9812-285f3501f877
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: 016d68de90088314250fef1fcfdb57d7f155ef79
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707162"
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a>Automatiseren NSG controle met Azure Network Watcher-beveiligingsgroepweergave

Klanten worden vaak momenteel geconfronteerd met de uitdaging van het controleren van de beveiligingsstatus van de infrastructuur. Deze vraag is niet anders is voor hun virtuele machines in Azure. Het is belangrijk dat u hebt een vergelijkbare beveiligingsprofiel op basis van de Netwerkbeveiligingsgroep (NSG) regels toegepast. Met behulp van de weergave van de beveiligingsgroep, nu krijgt u de lijst met regels die zijn toegepast op een virtuele machine binnen een NSG. U kunt een golden NSG-beveiligingsprofiel definiëren en starten van de weergave van de beveiligingsgroep een wekelijkse uitgebracht en vergelijken van de uitvoer naar het golden profiel en een rapport maken. Op deze manier kunt u identificeren met gemak alle virtuele machines die niet aan het voorgeschreven beveiligingsprofiel voldoen.

Als u niet bekend met Network Security Groups bent, Zie [overzicht van netwerkbeveiliging](../virtual-network/security-overview.md).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario, moet u een bekende goede basislijn op de beveiliging groep weergaveresultaten geretourneerd voor een virtuele machine vergelijken.

In dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een Network Watcher](network-watcher-create.md) te maken van een Network Watcher. Het scenario ook van uitgegaan dat er een resourcegroep met een geldige virtuele machine bestaat om te worden gebruikt.

## <a name="scenario"></a>Scenario

Het scenario in dit artikel besproken Hiermee haalt u de weergave van de beveiligingsgroep voor een virtuele machine.

U wordt in dit scenario:

- Ophalen van een bekende goede regelset
- Ophalen van een virtuele machine met de Rest-API
- Weergave van de beveiligingsgroep voor de virtuele machine ophalen
- Antwoord beoordelen

## <a name="retrieve-rule-set"></a>Regelset ophalen

De eerste stap in dit voorbeeld is om te werken met een bestaande basislijn. Het volgende voorbeeld is enkele json geëxtraheerd uit een bestaande Netwerkbeveiligingsgroep met behulp van de `Get-AzNetworkSecurityGroup` cmdlet die wordt gebruikt als de basislijn voor dit voorbeeld.

```json
[
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "3389",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1000,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "default-allow-rdp",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/default-allow-rdp"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "111",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1010,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "MyRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/MyRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "112",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1020,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "My2ndRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/My2ndRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "5672",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Deny",
        "Priority":  1030,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "ThisRuleNeedsToStay",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/ThisRuleNeedsToStay"
    }
]
```

## <a name="convert-rule-set-to-powershell-objects"></a>Regelset converteren naar PowerShell-objecten

In deze stap zijn we een json-bestand dat eerder is gemaakt met de regels die naar verwachting in de Netwerkbeveiligingsgroep voor dit voorbeeld wordt gelezen.

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a>Retrieve Network Watcher

De volgende stap is om op te halen van de Network Watcher-exemplaar. De `$networkWatcher` variabele wordt doorgegeven aan de `AzNetworkWatcherSecurityGroupView` cmdlet.

```powershell
$nw = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>Een VM ophalen

Een virtuele machine is vereist om uit te voeren de `Get-AzNetworkWatcherSecurityGroupView` cmdlet tegen. Het volgende voorbeeld wordt een VM-object.

```powershell
$VM = Get-AzVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a>Weergave van de beveiligingsgroep ophalen

De volgende stap is om op te halen van het resultaat van beveiliging groep weergeven. Dit resultaat wordt vergeleken met de "baseline" json die eerder is aangegeven.

```powershell
$secgroup = Get-AzNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-the-results"></a>De resultaten analyseren

Het antwoord is gegroepeerd op netwerkinterfaces. De verschillende typen regels geretourneerd gelden en standaard beveiligingsregels. Het resultaat verder wordt onderverdeeld op basis van hoe deze wordt toegepast, op een subnet of een virtuele netwerkadapter.

De volgende PowerShell-script worden de resultaten van de weergave van de beveiligingsgroep op een bestaande uitvoer van een NSG met elkaar vergeleken. Het volgende voorbeeld wordt een eenvoudig voorbeeld van hoe de resultaten kunnen worden vergeleken met `Compare-Object` cmdlet.

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

Het volgende voorbeeld is het resultaat. U ziet twee van de regels die zijn in de eerste regel ingesteld zijn niet aanwezig in de vergelijking.

```
Name                     : My2ndRuleDoNotDelete
Description              : 
Protocol                 : *
SourcePortRange          : *
DestinationPortRange     : 112
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Allow
Priority                 : 1020
Direction                : Inbound
SideIndicator            : <=

Name                     : ThisRuleNeedsToStay
Description              : 
Protocol                 : TCP
SourcePortRange          : *
DestinationPortRange     : 5672
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Deny
Priority                 : 1030
Direction                : Inbound
SideIndicator            : <=
```

## <a name="next-steps"></a>Volgende stappen

Als de instellingen zijn gewijzigd, Zie [Netwerkbeveiligingsgroepen beheren](../virtual-network/manage-network-security-group.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die betrokken zijn.













