---
title: Maken, wijzigen of verwijderen van de beveiligingsgroep van een Azure-netwerk | Microsoft Docs
description: Informatie over het maken, wijzigen of verwijderen van een netwerkbeveiligingsgroep.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/05/2018
ms.author: jdial
ms.openlocfilehash: 22cf62f201b21f3035687b7f0f2ff07dc94f1a29
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658669"
---
# <a name="create-change-or-delete-a-network-security-group"></a>Maken, wijzigen of verwijderen van een netwerkbeveiligingsgroep

Beveiligingsregels voor verbindingen in netwerkbeveiligingsgroepen kunnen u voor het filteren van het type netwerkverkeer waarop bevinden in een virtueel netwerksubnetten en netwerkinterfaces kunnen stromen. Als u niet bekend met netwerkbeveiligingsgroepen bent, Zie [netwerk groep beveiligingsoverzicht](security-overview.md) voor meer informatie over deze en volledige de [filteren van netwerkverkeer](tutorial-filter-network-traffic.md) zelfstudie om toegang te krijgen tot enige ervaring met het netwerk beveiligingsgroepen.

## <a name="before-you-begin"></a>Voordat u begint

De volgende taken uitvoeren voordat u stappen uitvoert in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u de PowerShell-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Deze zelfstudie vereist de Azure PowerShell moduleversie 5.4.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor het uitvoeren van taken in dit artikel, ofwel de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI vanaf uw computer. Deze zelfstudie vereist de Azure CLI versie 2.0.28 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook uitvoeren `az login` geen verbinding maken met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure met moet worden toegewezen aan de [netwerk Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of naar een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="work-with-network-security-groups"></a>Met netwerkbeveiligingsgroepen werken

U kunt maken, [weergeven van alle](#view-all-network-security-groups), [details weergeven van](#view-details-of-a-network-security-group), [wijzigen](#change-a-network-security-group), en [verwijderen](#delete-a-network-security-group) een netwerkbeveiligingsgroep. U kunt ook [koppelen of ontkoppelen](#associate-or-dissociate-a-network-security-group-to-or-from-a-resource) een netwerkbeveiligingsgroep van een netwerkinterface of een subnet.

### <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Er is een limiet voor het aantal netwerkbeveiligingsgroepen die per Azure-locatie en abonnement kunt u. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

1. Selecteer in de linkerbovenhoek van de portal **+ maken van een resource**.
2. Selecteer **Networking**, selecteer daarna **netwerkbeveiligingsgroep**.
3. Voer een **naam** voor de netwerkbeveiligingsgroep, selecteert u uw **abonnement**, maak een nieuwe **resourcegroep**, of Selecteer een bestaande resourcegroep, selecteert u een **Locatie**, en selecteer vervolgens **maken**.

**Opdrachten**

- Azure CLI: [az netwerk nsg maken](/cli/azure/network/nsg#az-network-nsg-create)
- PowerShell: [nieuwe AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup)

### <a name="view-all-network-security-groups"></a>Alle netwerkbeveiligingsgroepen weergeven

Voer in het zoekvak boven aan de portal *netwerkbeveiligingsgroepen*. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze. De netwerkbeveiligingsgroepen die zijn opgenomen in uw abonnement worden weergegeven.

**Opdrachten**

- Azure CLI: [az netwerk nsg lijst](/cli/azure/network/nsg#az-network-nsg-list)
- PowerShell: [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)

### <a name="view-details-of-a-network-security-group"></a>Details weergeven van een netwerkbeveiligingsgroep

1. Voer in het zoekvak boven aan de portal *netwerkbeveiligingsgroepen*. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de netwerkbeveiligingsgroep in de lijst die u wilt bekijken voor. Onder **instellingen** vindt u de **inkomende beveiligingsregels** en **uitgaande beveiligingsregels**, wordt de **netwerkinterfaces** en  **Subnetten** de netwerkbeveiligingsgroep is gekoppeld. U kunt ook in- of uitschakelen **diagnostische logboeken** en bekijk **effectieve beveiligingsregels**. Zie voor meer informatie, [diagnostische logboeken](virtual-network-nsg-manage-log.md) en [effectieve beveiligingsregels weergeven](diagnose-network-traffic-filter-problem.md).
3. Zie voor meer informatie over de algemene instellingen voor Azure vermeld, de volgende artikelen:
    *   [Activiteitenlogboek](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
    *   [Toegangsbeheer (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
    *   [Tags](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Hiermee vergrendelt u](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Automatiseringsscript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Opdrachten**

- Azure CLI: [az netwerk nsg weergeven](/cli/azure/network/nsg#az-network-nsg-show)
- PowerShell: [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)

### <a name="change-a-network-security-group"></a>Een netwerkbeveiligingsgroep wijzigen

1. Voer in het zoekvak boven aan de portal *netwerkbeveiligingsgroepen* in het zoekvak. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de netwerkbeveiligingsgroep die u wilt wijzigen. De meest voorkomende wijzigingen zijn [toe te voegen](#create-a-security-rule) of [verwijderen](#delete-a-security-rule) beveiligingsregels en [Associating of een netwerkbeveiligingsgroep naar of van een subnet of netwerk interface dissociating](#associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface).

**Opdrachten**

- Azure CLI: [az netwerk nsg update](/cli/azure/network/nsg#az-network-nsg-update)
- PowerShell: [Set AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/set-azurermnetworksecuritygroup)

### <a name="associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface"></a>Koppelen of ontkoppelen van een netwerkbeveiligingsgroep naar of van een subnet of netwerk-interface

Als u een netwerkbeveiligingsgroep te koppelen of ontkoppelen van een netwerkbeveiligingsgroep van een netwerkinterface, Zie [een netwerkbeveiligingsgroep te koppelen of ontkoppelen van een netwerkbeveiligingsgroep van een netwerkinterface](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group). Als u een netwerkbeveiligingsgroep te koppelen of ontkoppelen van een netwerkbeveiligingsgroep van een subnet, Zie [subnetinstellingen wijzigen](virtual-network-manage-subnet.md#change-subnet-settings).

### <a name="delete-a-network-security-group"></a>Een netwerkbeveiligingsgroep verwijderen

Als een netwerkbeveiligingsgroep gekoppeld aan subnetten of netwerkinterfaces is, kan niet worden verwijderd. [Ontkoppelen](#associate-or-dissociate-a-network-security-group-to-or-from-a-resource) een netwerkbeveiligingsgroep van alle subnetten en netwerkinterfaces voordat u probeert te verwijderen.

1. Voer in het zoekvak boven aan de portal *netwerkbeveiligingsgroepen* in het zoekvak. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de netwerkbeveiligingsgroep die u wilt verwijderen uit de lijst.
3. Selecteer **verwijderen**, en selecteer vervolgens **Ja**.

**Opdrachten**

- Azure CLI: [az netwerk nsg verwijderen](/cli/azure/network/nsg#az-network-nsg-delete)
- PowerShell: [verwijderen AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/remove-azurermnetworksecuritygroupp) 

## <a name="work-with-security-rules"></a>Werken met beveiligingsregels voor verbindingen

Een netwerkbeveiligingsgroep bevat nul of meer beveiligingsregels voor verbindingen. U kunt maken, [weergeven van alle](#view-all-security-rules), [details weergeven van](#view-details-of-a-security-rule), [wijzigen](#change-a-security-rule), en [verwijderen](#delete-a-security-rule) een beveiligingsregel.

### <a name="create-a-security-rule"></a>Een beveiligingsregel maken

Er is een limiet voor het aantal regels per netwerkbeveiligingsgroep per Azure-locatie en abonnement kunnen maken. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

1. Voer in het zoekvak boven aan de portal *netwerkbeveiligingsgroepen* in het zoekvak. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de netwerkbeveiligingsgroep in de lijst die u wilt een beveiligingsregel aan toevoegen.
3. Selecteer **inkomende beveiligingsregels** onder **instellingen**. Enkele bestaande regels worden weergegeven. Sommige van de regels die u mogelijk niet toegevoegd. Wanneer een netwerkbeveiligingsgroep is gemaakt, worden enkele standaardregels beveiliging gemaakt in het. Zie voor meer informatie, [beveiligingsregels standaard](security-overview.md#default-security-rules).  Standaardregels voor de beveiliging kan niet worden verwijderd, maar u kunt deze overschrijft met regels die een hogere prioriteit hebben.
4. <a name = "security-rule-settings"></a>Selecteer **+ toevoegen**.  Selecteer of waarden toevoegen voor de volgende instellingen en selecteer vervolgens **OK**:
    
    |Instelling  |Waarde  |Details  |
    |---------|---------|---------|
    |Bron     | Selecteer **eventuele**, **IP-adressen**, of **Service Tag**.        | Als u selecteert **IP-adressen**, moet u vervolgens opgeven **bron-IP-adressen/CIDR-bereiken**. U kunt een enkele waarde of een door komma's gescheiden lijst met meerdere waarden opgeven. Een voorbeeld van meerdere waarden is 10.0.0.0/16, 192.188.1.1. Er gelden beperkingen voor het aantal waarden die u kunt opgeven. Zie [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie. Als u selecteert **Service Tag**, moet u één service-label selecteren. Een servicetag is een vooraf gedefinieerde id voor een categorie IP-adressen. Zie voor meer informatie over de beschikbare service-tags en wat elke tag vertegenwoordigt, [Service labels](security-overview.md#service-tags)        |
    |Poortbereiken van bron     | Geef een losse poort, bijvoorbeeld 80, een poortbereik, bijvoorbeeld 1024-65535 of een door komma's gescheiden lijst van één poorten en/of poortbereiken zoals 80, 1024-65535. Hiermee geeft u een sterretje voor verkeer op een willekeurige poort. | De poorten en adresbereiken opgeven welke poorten-verkeer wordt toegestaan of geweigerd door de regel. Er gelden beperkingen voor het aantal poorten die u kunt opgeven. Zie [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.  |
    |Doel     | Selecteer **eventuele**, **IP-adressen**, of **virtueel netwerk**.        | Als u selecteert **IP-adressen**, moet u vervolgens opgeven **doel-IP-adressen/CIDR-bereiken**. Net als bij **bron** en **bron-IP-adressen/CIDR-bereiken**, kunt u één of meerdere adressen of adresbereiken en er gelden beperkingen aan het getal dat u kunt opgeven. Selecteren **virtueel netwerk**, die een servicetag is, betekent dat verkeer is toegestaan voor alle IP-adressen in de adresruimte van het virtuele netwerk.        |
    |Poortbereiken van doel     | Geef een enkele waarde of een door komma's gescheiden lijst met waarden. | Net als bij **bron poortbereiken**, kunt u één of meerdere poorten en adresbereiken opgeven en er gelden beperkingen aan het getal dat u kunt opgeven. |
    |Protocol     | Selecteer **eventuele**, **TCP**, of **UDP**.        |         |
    |Bewerking     | Selecteer **toestaan** of **weigeren**.        |         |
    |Prioriteit     | Voer een waarde tussen 100-4096 die uniek is voor alle beveiligingsregels binnen de netwerkbeveiligingsgroep. |Regels worden verwerkt in volgorde van prioriteit. Hoe lager het getal, hoe hoger de prioriteit. Het raadzaam dat u een lege ruimte tussen prioriteitsnummers laten bij het maken van regels, zoals 100, 200, 300. Openingen maakt het eenvoudiger om toe te voegen in de toekomst regels die u mogelijk wilt aanbrengen hoger of lager is dan de bestaande regels.         |
    |Naam     | Een unieke naam voor de regel in de netwerkbeveiligingsgroep.        |  De naam mag maximaal 80 tekens lang zijn. De naam moet beginnen met een letter of cijfer eindigen met een letter, cijfer of onderstrepingsteken en mag alleen letters, cijfers, onderstrepingstekens, punten of afbreekstreepjes bevatten.       |
    |Beschrijving     | Een optionele beschrijving.        |         |

    U kunt geen opgeven een [toepassing beveiligingsgroep](#work-with-application-security-groups) voor de **bron** of **bestemming** instellingen met behulp van de portal. U kunt echter met de Azure CLI of PowerShell. De instellingen voor **uitgaande beveiligingsregels** zijn vergelijkbaar, zodat ze niet afzonderlijk worden behandeld.

**Opdrachten**

- Azure CLI: [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#az-network-nsg-rule-create)
- PowerShell: [nieuwe AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig)

### <a name="view-all-security-rules"></a>Alle regels weergeven

Een netwerkbeveiligingsgroep bevat geen of meerdere regels. Zie voor meer informatie over de gegevens die worden vermeld bij het weergeven van regels, [netwerk groep beveiligingsoverzicht](security-overview.md).

1. Voer in het zoekvak boven aan de portal *netwerkbeveiligingsgroepen*. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de netwerkbeveiligingsgroep in de lijst die u wilt weergeven van regels voor.
3. Selecteer **inkomende beveiligingsregels** of **uitgaande beveiligingsregels** onder **instellingen**.

De lijst bevat alle regels die u hebt gemaakt en de netwerkbeveiligingsgroep [beveiligingsregels standaard](security-overview.md#default-security-rules).

**Opdrachten**

- Azure CLI: [az netwerk nsg Regellijst](/cli/azure/network/nsg/rule#az-network-nsg-rule-list)
- PowerShell: [Get-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/get-azurermnetworksecurityruleconfig)

### <a name="view-details-of-a-security-rule"></a>Details van een beveiligingsregel weergeven

1. Voer in het zoekvak boven aan de portal *netwerkbeveiligingsgroepen*. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Selecteer de gewenste details wilt weergeven van een beveiligingsregel voor netwerk-beveiligingsgroep.
3. Selecteer **inkomende beveiligingsregels** of **uitgaande beveiligingsregels** onder **instellingen**.
4. Selecteer de regel die u wilt bekijken voor. Zie voor een gedetailleerde uitleg van alle instellingen [regel beveiligingsinstellingen](#security-rule-settings).

**Opdrachten**

- Azure CLI: [az netwerk nsg regel weergeven](/cli/azure/network/nsg/rule#az-network-nsg-rule-show)
- PowerShell: [Get-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/get-azurermnetworksecurityruleconfig)

### <a name="change-a-security-rule"></a>Een beveiligingsregel wijzigen

1. Voer de stappen in [details weergeven van een beveiligingsregel](#view-details-of-a-security-rule).
2. De instellingen desgewenst wijzigen en selecteer vervolgens **opslaan**. Zie voor een gedetailleerde uitleg van alle instellingen [regel beveiligingsinstellingen](#security-rule-settings).

**Opdrachten**

- Azure CLI: [az netwerk nsg regel update](/cli/azure/network/nsg/rule#az-network-nsg-rule-update)
- PowerShell: [Set AzureRmSecurityRuleConfig](/powershell/module/azurerm.network/set-azurermnetworksecurityruleconfig)

### <a name="delete-a-security-rule"></a>Een beveiligingsregel verwijderen

1. Voer de stappen in [details weergeven van een beveiligingsregel](#view-details-of-a-security-rule).
2. Selecteer **verwijderen**, en selecteer vervolgens **Ja**.

**Opdrachten**

- Azure CLI: [az netwerk het nsg regel verwijderen](/cli/azure/network/nsg/rule#az-network-nsg-rule-delete)
- PowerShell: [verwijderen AzureRmSecurityRuleConfig](/powershell/module/azurerm.network/remove-azurermnetworksecurityruleconfig)


## <a name="work-with-application-security-groups"></a>Werken met beveiligingsgroepen van toepassing

De beveiligingsgroep van een toepassing bevat nul of meer netwerkinterfaces. Zie voor meer informatie, [toepassing beveiligingsgroepen](security-overview.md#application-security-groups). U kunt niet werken met beveiligingsgroepen toepassing in de portal, maar u kunt PowerShell of Azure CLI gebruiken. Alle netwerkinterfaces in de beveiligingsgroep van een toepassing moeten zich in hetzelfde virtuele netwerk. De eerste netwerkinterface toegevoegd aan de beveiligingsgroep van een toepassing bepaalt welke virtuele netwerk dat alle daaropvolgende netwerkinterfaces moeten liggen. Zie voor informatie over het toevoegen van een netwerkinterface aan de beveiligingsgroep van een toepassing, [een netwerkinterface toevoegen aan de beveiligingsgroep van een toepassing](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups).

### <a name="create-an-application-security-group"></a>De beveiligingsgroep van een toepassing maken

- Azure CLI: [az netwerk asg maken](/cli/azure/network/asg#az-network-asg-create)
- PowerShell: [nieuwe AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/new-azurermapplicationsecuritygroup)

### <a name="view-all-application-security-groups"></a>Alle beveiligingsgroepen van de toepassing weergeven

- Azure CLI: [az netwerk asg lijst](/cli/azure/network/asg#az-network-asg-list)
- PowerShell: [Get-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/get-azurermapplicationsecuritygroup)

### <a name="view-details-of-a-specific-application-security-group"></a>Details van de beveiligingsgroep van een specifieke toepassing weergeven

- Azure CLI: [az netwerk asg weergeven](/cli/azure/network/asg#az-network-asg-show)
- PowerShell: [Get-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/get-azurermapplicationsecuritygroup)

### <a name="change-an-application-security-group"></a>Wijzigen van de beveiligingsgroep van een toepassing

Terwijl u bepaalde instellingen zoals tags en machtigingen voor een bestaande beveiligingsgroep voor de toepassing wijzigen kunt, kunt u de naam of locatie niet wijzigen.

- Azure CLI: [az netwerk asg update](/cli/azure/network/asg#az-network-asg-update)
- PowerShell: Er is geen PowerShell-cmdlet.

### <a name="delete-an-application-security-group"></a>De beveiligingsgroep van een toepassing verwijderen

U kunt de beveiligingsgroep van een toepassing niet verwijderen als er netwerkinterfaces in deze. Alle netwerkinterfaces verwijderen uit de beveiligingsgroep van de toepassing door de netwerkinterface-instellingen wijzigen of verwijderen van de netwerkinterfaces. Zie voor meer informatie [toevoegen aan of verwijder een netwerkinterface van de beveiligingsgroepen toepassing](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups) of [verwijderen van een netwerkinterface](virtual-network-network-interface.md#delete-a-network-interface).

**Opdrachten**

- Azure CLI: [az netwerk asg verwijderen](/cli/azure/network/asg#az-network-asg-delete)
- PowerShell: [verwijderen AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/remove-azurermapplicationsecuritygroup)

## <a name="permissions"></a>Machtigingen

Om taken voor netwerkbeveiligingsgroepen beveiligingsregels en beveiligingsgroepen toepassing uitvoert, moet uw account worden toegewezen aan de [netwerk Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of naar een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de juiste machtigingen die worden vermeld in de volgende tabellen:

### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

| Bewerking                                                        |   Naam                                                                |
|-------------------------------------------------------------- |   -------------------------------------------                         |
| Microsoft.Network/ruleTables/read                             |   Netwerkbeveiligingsgroep ophalen                                          |
| Microsoft.Network/ruleTables/write                            |   Maken of bijwerken van netwerkbeveiligingsgroep                             |
| Microsoft.Network/ruleTables/delete                           |   Netwerkbeveiligingsgroep verwijderen                                       |
| Microsoft.Network/ruleTables/join/action                      |   Koppelen van een netwerkbeveiligingsgroep aan een subnet of netwerk-interface |
| Microsoft.Network/ruleTables/rules/read                       |   Regel voor ophalen                                                            |
| Microsoft.Network/ruleTables/rules/write                      |   Regel maken of bijwerken                                               |
| Microsoft.Network/ruleTables/rules/delete                     |   Regel verwijderen                                                         |
| Microsoft.Network/networkInterfaces/effectiveruleTable/action |   Aan netwerk Interface effectieve netwerkbeveiligingsgroep ophalen              |
| Microsoft.Network/networkWatchers/nextHop/action              |   De volgende hop van een virtuele machine opgehaald                                         |

### <a name="application-security-groups"></a>Toepassingsbeveiligingsgroepen

| Bewerking                                                                     | Naam                                                     |
| --------------------------------------------------------------             | -------------------------------------------              |
| Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action     | Een IP-configuratie toevoegen aan de beveiligingsgroep van een toepassing|
| Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Een beveiligingsregel toevoegen aan de beveiligingsgroep van een toepassing    |
| Microsoft.Network/applicationSecurityGroups/read                           | Een beveiligingsgroep voor de toepassing ophalen                        |
| Microsoft.Network/applicationSecurityGroups/write                          | Maken of bijwerken van de beveiligingsgroep van een toepassing           |
| Microsoft.Network/applicationSecurityGroups/delete                         | De beveiligingsgroep van een toepassing verwijderen                     |

## <a name="next-steps"></a>Volgende stappen

- Maken van een netwerk of de groep wordt gebruikt door toepassing beveiliging [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) steekproef scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)
- Maken en toepassen van [Azure beleid](policy-samples.md) voor virtuele netwerken