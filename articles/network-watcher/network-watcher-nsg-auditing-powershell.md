---
title: NSG controle met Azure-Watcher netwerkbeveiliging groepsweergave automatiseren | Microsoft Docs
description: Deze pagina vindt u instructies voor het configureren van de controle van een Netwerkbeveiligingsgroep
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 78a01bcf-74fe-402a-9812-285f3501f877
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 57f2200e541eeb629f72d60ffa0acb2d8233c018
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a>Automatiseren NSG controle met Azure-Watcher netwerkbeveiliging groep weergeven

Klanten zijn vaak moeten de beveiligingsstatus van hun infrastructuur te controleren. Deze uitdaging gaat niet anders zijn voor hun virtuele machines in Azure. Het is belangrijk om een soortgelijke beveiligingsprofiel op basis van de Netwerkbeveiligingsgroep (NSG) regels toegepast. De weergave van de groep beveiliging gebruikt, krijgt u nu de lijst met regels voor een virtuele machine binnen een NSG wordt toegepast. U kunt definiëren van een gouden NSG-beveiligingsprofiel en beveiliging groepsweergave een wekelijkse uitgebracht initiëren en vergelijken van de uitvoer naar het gouden profiel en een rapport maken. Op deze manier kunt u identificeren met gemak alle virtuele machines die niet met het voorgeschreven beveiligingsprofiel overeen komen.

Als u niet bekend met Netwerkbeveiligingsgroepen bent, gaat u naar [beveiligingsoverzicht van het netwerk](../virtual-network/virtual-networks-nsg.md)

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario vergelijken u met een bekende goede basis voor de beveiliging groep weergaveresultaten geretourneerd voor een virtuele machine.

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher. Het scenario wordt ervan uitgegaan dat er een resourcegroep met een geldige virtuele machine bestaat om te worden gebruikt.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel wordt de weergave van de groep beveiliging voor een virtuele machine.

U wordt in dit scenario:

- Ophalen van een bekende goede regelset
- Ophalen van een virtuele machine met de Rest-API
- Groepsweergave beveiliging voor virtuele machine ophalen
- Antwoord evalueren

## <a name="retrieve-rule-set"></a>Regelset ophalen

De eerste stap in dit voorbeeld is voor gebruik met een bestaande basislijn. Het volgende voorbeeld is sommige json opgehaald uit een bestaande Netwerkbeveiligingsgroep met behulp van de `Get-AzureRmNetworkSecurityGroup` cmdlet die wordt gebruikt als de basislijn voor dit voorbeeld.

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

## <a name="convert-rule-set-to-powershell-objects"></a>Regelset converteren naar objecten met PowerShell

We zijn een json-bestand dat eerder is gemaakt met de regels die naar verwachting wordt op de Netwerkbeveiligingsgroep voor dit voorbeeld lezen in deze stap.

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a>Ophalen van netwerk-Watcher

De volgende stap is het exemplaar van de netwerk-Watcher ophalen. De `$networkWatcher` variabele wordt doorgegeven aan de `AzureRmNetworkWatcherSecurityGroupView` cmdlet.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>Een virtuele machine ophalen

Een virtuele machine is vereist om de `Get-AzureRmNetworkWatcherSecurityGroupView` cmdlet tegen. Het volgende voorbeeld wordt een VM-object.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a>Groep beveiligingsweergave ophalen

De volgende stap is het resultaat van beveiliging groep weergave ophalen. Dit resultaat wordt vergeleken met de 'basislijn' json die eerder is aangegeven.

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-the-results"></a>De resultaten analyseren

Het antwoord worden gegroepeerd per netwerkinterfaces. De verschillende typen regels geretourneerd geldig zijn en de standaardinstellingen van de beveiligingsregels voor verbindingen. Het resultaat is verder uitgesplitst hoe deze wordt toegepast, op een subnet of een virtuele NIC.

De volgende PowerShell-script worden de resultaten van de weergave van de groep beveiliging aan een bestaande uitvoer van een NSG vergeleken. Het volgende voorbeeld wordt een eenvoudig voorbeeld van hoe de resultaten kunnen worden vergeleken met `Compare-Object` cmdlet.

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

Als de instellingen zijn gewijzigd, Zie [Netwerkbeveiligingsgroepen beheren](../virtual-network/virtual-network-manage-nsg-arm-portal.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die betrokken zijn.













