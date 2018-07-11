---
title: Maken, wijzigen of verwijderen van een Azure-netwerkbeveiligingsgroep | Microsoft Docs
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
ms.openlocfilehash: 5ca70b085b5ac9db4d108966fa695f042c20489d
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920508"
---
# <a name="create-change-or-delete-a-network-security-group"></a>Maken, wijzigen of verwijderen van een netwerkbeveiligingsgroep

Beveiligingsregels in netwerkbeveiligingsgroepen kunnen u voor het filteren van het type netwerkverkeer dat naar en uit de virtuele subnetten en netwerkinterfaces stromen kan. Als u niet bekend met netwerkbeveiligingsgroepen bent, Zie [overzicht van netwerkbeveiligingsgroepen](security-overview.md) voor meer informatie over deze en voer de [-netwerkverkeer filteren](tutorial-filter-network-traffic.md) zelfstudie om te krijgen van enige ervaring met het netwerk -beveiligingsgroepen.

## <a name="before-you-begin"></a>Voordat u begint

Voer de volgende taken voordat u de stappen in elke sectie van dit artikel:

- Als u nog een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefaccount](https://azure.microsoft.com/free).
- Als u de portal gebruikt, opent u https://portal.azure.com, en meld u aan met uw Azure-account.
- Als u PowerShell-opdrachten gebruikt om taken in dit artikel te voltooien, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. In deze zelfstudie vereist de Azure PowerShell-moduleversie 5.4.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.
- Als u Azure-opdrachtregelinterface (CLI)-opdrachten voor taken in dit artikel uit te voeren, hetzij de opdrachten uitvoert in de [Azure Cloud Shell](https://shell.azure.com/bash), of door het uitvoeren van de CLI van de computer. In deze zelfstudie gebruikmaken van Azure CLI versie 2.0.28 of hoger. Voer `az --version` uit om te kijken welke versie is geïnstalleerd. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u de Azure CLI lokaal uitvoert, moet u ook om uit te voeren `az login` voor het maken van een verbinding met Azure.

Het account dat u zich aanmelden bij of verbinding maken met Azure met moet worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de nodige acties die worden vermeld in [machtigingen ](#permissions).

## <a name="work-with-network-security-groups"></a>Met netwerkbeveiligingsgroepen werken

U kunt maken, [weergeven van alle](#view-all-network-security-groups), [details bekijken van](#view-details-of-a-network-security-group), [wijzigen](#change-a-network-security-group), en [verwijderen](#delete-a-network-security-group) een netwerkbeveiligingsgroep. U kunt ook [koppelen of ontkoppelen](#associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface) een netwerkbeveiligingsgroep van een netwerkinterface of subnet.

### <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Er is een limiet aan het aantal netwerkbeveiligingsgroepen die kunt u per Azure-locatie en abonnement maken. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

1. Selecteer in de linkerbovenhoek van de portal **+ een resource maken**.
2. Selecteer **netwerken**en selecteer vervolgens **netwerkbeveiligingsgroep**.
3. Voer een **naam** voor de netwerkbeveiligingsgroep, selecteert u uw **abonnement**, maakt u een nieuw **resourcegroep**, of Selecteer een bestaande resourcegroep, selecteert u een **Locatie**, en selecteer vervolgens **maken**.

**Opdrachten**

- Azure CLI: [az network nsg maken](/cli/azure/network/nsg#az-network-nsg-create)
- PowerShell: [nieuwe-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup)

### <a name="view-all-network-security-groups"></a>Alle netwerkbeveiligingsgroepen weergeven

Voer in het zoekvak boven aan de portal, *netwerkbeveiligingsgroepen*. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze. De netwerkbeveiligingsgroepen die zijn opgenomen in uw abonnement worden weergegeven.

**Opdrachten**

- Azure CLI: [az network nsg list](/cli/azure/network/nsg#az-network-nsg-list)
- PowerShell: [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)

### <a name="view-details-of-a-network-security-group"></a>Details van een netwerkbeveiligingsgroep weergeven

1. Voer in het zoekvak boven aan de portal, *netwerkbeveiligingsgroepen*. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkbeveiligingsgroep in de lijst die u wilt weergeven van details voor. Onder **instellingen** vindt u de **inkomende beveiligingsregels** en **uitgaande beveiligingsregels**, wordt de **netwerkinterfaces** en  **Subnetten** de netwerkbeveiligingsgroep is gekoppeld aan. U kunt ook inschakelen of uitschakelen **diagnostische logboeken** en **effectieve beveiligingsregels**. Zie voor meer informatie, [diagnostische logboeken](virtual-network-nsg-manage-log.md) en [effectieve beveiligingsregels bekijken](diagnose-network-traffic-filter-problem.md).
3. Zie voor meer informatie over de algemene instellingen voor Azure die worden vermeld, de volgende artikelen:
    *   [Activiteitenlogboek](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
    *   [Toegangsbeheer (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
    *   [Tags](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Hiermee vergrendelt u](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Automatiseringsscript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Opdrachten**

- Azure CLI: [az network nsg show](/cli/azure/network/nsg#az-network-nsg-show)
- PowerShell: [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)

### <a name="change-a-network-security-group"></a>Een netwerkbeveiligingsgroep wijzigen

1. Voer in het zoekvak boven aan de portal, *netwerkbeveiligingsgroepen* in het zoekvak in. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkbeveiligingsgroep die u wilt wijzigen. De meest voorkomende wijzigingen zijn [toe te voegen](#create-a-security-rule) of [verwijderen](#delete-a-security-rule) beveiligingsregels en [Associating of een netwerkbeveiligingsgroep naar of van een subnet of netwerkinterface ontkoppelen](#associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface).

**Opdrachten**

- Azure CLI: [az network nsg update](/cli/azure/network/nsg#az-network-nsg-update)
- PowerShell: [Set-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/set-azurermnetworksecuritygroup)

### <a name="associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface"></a>Koppelen of ontkoppelen van een netwerkbeveiligingsgroep naar of van een subnet of netwerkinterface

Als u een netwerkbeveiligingsgroep te koppelen of ontkoppelen van een netwerkbeveiligingsgroep naar een netwerkinterface, Zie [een netwerkbeveiligingsgroep te koppelen of ontkoppelen van een netwerkbeveiligingsgroep naar een netwerkinterface](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group). Als u een netwerkbeveiligingsgroep te koppelen of ontkoppelen van een netwerkbeveiligingsgroep van een subnet, Zie [-subnet-instellingen wijzigen](virtual-network-manage-subnet.md#change-subnet-settings).

### <a name="delete-a-network-security-group"></a>Een netwerkbeveiligingsgroep verwijderen

Als een netwerkbeveiligingsgroep gekoppeld aan subnetten of netwerkinterfaces is, kan niet worden verwijderd. [Ontkoppelen](#associate-or-dissociate-a-network-security-group-to-or-from-a-resource) een netwerkbeveiligingsgroep van alle subnetten en netwerkinterfaces voordat u probeert te verwijderen.

1. Voer in het zoekvak boven aan de portal, *netwerkbeveiligingsgroepen* in het zoekvak in. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkbeveiligingsgroep die u wilt verwijderen uit de lijst.
3. Selecteer **verwijderen**, en selecteer vervolgens **Ja**.

**Opdrachten**

- Azure CLI: [az network nsg verwijderen](/cli/azure/network/nsg#az-network-nsg-delete)
- PowerShell: [Remove-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/remove-azurermnetworksecuritygroupp) 

## <a name="work-with-security-rules"></a>Werken met beveiligingsregels

Een netwerkbeveiligingsgroep bevat nul of meer beveiligingsregels. U kunt maken, [weergeven van alle](#view-all-security-rules), [details bekijken van](#view-details-of-a-security-rule), [wijzigen](#change-a-security-rule), en [verwijderen](#delete-a-security-rule) een beveiligingsregel.

### <a name="create-a-security-rule"></a>Maak een beveiligingsregel

Er is een limiet aan het aantal regels per netwerkbeveiligingsgroep per Azure-locatie en abonnement kunnen maken. Zie [Netwerkenlimieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.

1. Voer in het zoekvak boven aan de portal, *netwerkbeveiligingsgroepen* in het zoekvak in. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. De netwerkbeveiligingsgroep in de lijst die u wilt toevoegen van een beveiligingsregel te selecteren.
3. Selecteer **inkomende beveiligingsregels** onder **instellingen**. Meerdere bestaande regels worden weergegeven. Sommige van de regels die u niet hebt toegevoegd. Wanneer een netwerkbeveiligingsgroep wordt gemaakt, worden in deze verschillende standaardbeveiligingsregels gemaakt. Zie voor meer informatie, [standaard beveiligingsregels](security-overview.md#default-security-rules).  Standaardregels voor beveiliging kan niet worden verwijderd, maar u kunt ze wel negeren met regels die een hogere prioriteit hebben.
4. <a name = "security-rule-settings"></a>Selecteer **+ toevoegen**.  Selecteer of voeg waarden toe voor de volgende instellingen en selecteer vervolgens **OK**:
    
    |Instelling  |Waarde  |Details  |
    |---------|---------|---------|
    |Bron     | Selecteer **eventuele**, **toepassingsbeveiligingsgroep**, **IP-adressen**, of **servicetag** voor inkomende beveiligingsregels. Als u een beveiligingsregel voor uitgaand maakt, de opties zijn hetzelfde als de opties die worden weergegeven voor **bestemming**.       | Als u selecteert **toepassingsbeveiligingsgroep**, selecteert u vervolgens een of meer bestaande toepassing beveiligingsgroepen die aanwezig zijn in dezelfde regio als de netwerkinterface. Meer informatie over het [Maak een toepassingsbeveiligingsgroep](#create-an-application-security-group). Als u selecteert **toepassingsbeveiligingsgroep** voor zowel de **bron** en **bestemming**, de netwerkinterfaces in beide toepassingsbeveiligingsgroepen moeten zich in dezelfde virtueel netwerk. Als u selecteert **IP-adressen**, geeft u **bron-IP-adressen/CIDR-bereiken**. U kunt een enkele waarde of een door komma's gescheiden lijst met meerdere waarden opgeven. Een voorbeeld van meerdere waarden is 10.0.0.0/16, 192.188.1.1. Er gelden beperkingen voor het aantal waarden die u kunt opgeven. Zie [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie. Als u selecteert **servicetag**, selecteer vervolgens een servicetag. Een servicetag is een vooraf gedefinieerde id voor een bepaalde categorie IP-adressen. Zie voor meer informatie over de beschikbare service-tags en wat elke tag vertegenwoordigt, [servicetags](security-overview.md#service-tags). Als het IP-adres dat u opgeeft is toegewezen aan een virtuele machine van Azure, zorg ervoor dat u de privé IP-adres, niet het openbare IP-adres toegewezen aan de virtuele machine opgeven. Beveiligingsregels worden verwerkt nadat Azure vertaalt het openbare IP-adres op een privé IP-adres voor inkomende beveiligingsregels en voordat Azure zet een privé IP-adres een openbaar IP-adres voor regels voor uitgaand verkeer. Zie voor meer informatie over openbare en persoonlijke IP-adressen in Azure, [IP-adrestypen](virtual-network-ip-addresses-overview-arm.md).        |
    |Poortbereiken van bron     | Geef één poort zijn, zoals 80, een poortbereik, bijvoorbeeld 1024-65535, of een door komma's gescheiden lijst met losse poorten en/of poortbereiken, zoals 80, 1024-65535. Voer een sterretje voor verkeer op een willekeurige poort. | De poorten en adresbereiken opgeven welke poorten verkeer wordt toegestaan of geweigerd door de regel. Er gelden beperkingen voor het aantal poorten die u kunt opgeven. Zie [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie.  |
    |Doel     | Selecteer **eventuele**, **toepassingsbeveiligingsgroep**, **IP-adressen**, of **Virtueelnetwerk** voor inkomende beveiligingsregels. Als u een beveiligingsregel voor uitgaand maakt, de opties zijn hetzelfde als de opties die worden weergegeven voor **bron**.        | Als u selecteert **toepassingsbeveiligingsgroep** Selecteer u vervolgens een of meer bestaande toepassingsbeveiligingsgroepen die aanwezig zijn in dezelfde regio als de netwerkinterface. Meer informatie over het [Maak een toepassingsbeveiligingsgroep](#create-an-application-security-group). Als u selecteert **toepassingsbeveiligingsgroep**, selecteert u een bestaande toepassingsbeveiligingsgroep dat zich in dezelfde regio als de netwerkinterface. Als u selecteert **IP-adressen**, geeft u **doel-IP-adressen/CIDR-bereiken**. Vergelijkbaar met **bron** en **bron-IP-adressen/CIDR-bereiken**, kunt u één of meerdere adressen of bereiken en er gelden beperkingen aan het getal dat u kunt opgeven. Selecteren **virtueel netwerk**, welke een servicetag is, betekent dat het verkeer is toegestaan voor alle IP-adressen in de adresruimte van het virtuele netwerk. Als het IP-adres dat u opgeeft is toegewezen aan een virtuele machine van Azure, zorg ervoor dat u de privé IP-adres, niet het openbare IP-adres toegewezen aan de virtuele machine opgeven. Beveiligingsregels worden verwerkt nadat Azure vertaalt het openbare IP-adres op een privé IP-adres voor inkomende beveiligingsregels en voordat Azure zet een privé IP-adres een openbaar IP-adres voor regels voor uitgaand verkeer. Zie voor meer informatie over openbare en persoonlijke IP-adressen in Azure, [IP-adrestypen](virtual-network-ip-addresses-overview-arm.md).        |
    |Poortbereiken van doel     | Geef een enkele waarde of een door komma's gescheiden lijst met waarden. | Vergelijkbaar met **poortbereiken van bron**, kunt u één of meerdere poorten en -bereiken en er gelden beperkingen aan het getal dat u kunt opgeven. |
    |Protocol     | Selecteer **eventuele**, **TCP**, of **UDP**.        |         |
    |Bewerking     | Selecteer **toestaan** of **weigeren**.        |         |
    |Prioriteit     | Voer een waarde tussen 100-4096 die uniek is voor alle beveiligingsregels binnen de netwerkbeveiligingsgroep. |Regels worden verwerkt in volgorde van prioriteit. Des te lager het nummer, hoe hoger de prioriteit. Het raadzaam dat u een lege ruimte tussen de getallen prioriteit laten bij het maken van regels, zoals 100, 200, 300. Openingen maakt het eenvoudiger om toe te voegen in de toekomst regels die u mogelijk wilt aanbrengen hoger of lager is dan de bestaande regels.         |
    |Naam     | Een unieke naam voor de regel in de netwerkbeveiligingsgroep.        |  De naam mag maximaal 80 tekens bevatten. Moet beginnen met een letter of cijfer, eindigen met een letter, cijfer of onderstrepingsteken en mag alleen letters, cijfers, onderstrepingstekens, punten of afbreekstreepjes bevatten.       |
    |Beschrijving     | Een optionele beschrijving.        |         |

**Opdrachten**

- Azure CLI: [az network nsg-regel maken](/cli/azure/network/nsg/rule#az-network-nsg-rule-create)
- PowerShell: [nieuwe-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig)

### <a name="view-all-security-rules"></a>Alle regels weergeven

Een netwerkbeveiligingsgroep bevat nul of meerdere regels. Zie voor meer informatie over de gegevens die worden vermeld bij het weergeven van regels, [overzicht van netwerkbeveiligingsgroepen](security-overview.md).

1. Voer in het zoekvak boven aan de portal, *netwerkbeveiligingsgroepen*. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkbeveiligingsgroep in de lijst die u wilt weergeven van regels voor.
3. Selecteer **inkomende beveiligingsregels** of **uitgaande beveiligingsregels** onder **instellingen**.

De lijst bevat alle regels die u hebt gemaakt en de netwerkbeveiligingsgroep [standaard beveiligingsregels](security-overview.md#default-security-rules).

**Opdrachten**

- Azure CLI: [lijst az network nsg-regel](/cli/azure/network/nsg/rule#az-network-nsg-rule-list)
- PowerShell: [Get-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/get-azurermnetworksecurityruleconfig)

### <a name="view-details-of-a-security-rule"></a>Details van een beveiligingsregel weergeven

1. Voer in het zoekvak boven aan de portal, *netwerkbeveiligingsgroepen*. Wanneer **netwerkbeveiligingsgroepen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
2. Selecteer de netwerkbeveiligingsgroep die u wilt weergeven van details van een beveiligingsregel voor.
3. Selecteer **inkomende beveiligingsregels** of **uitgaande beveiligingsregels** onder **instellingen**.
4. Selecteer de regel die u wilt weergeven van details voor. Zie voor een gedetailleerde beschrijving van alle instellingen [beveiligingsinstellingen van de regel](#security-rule-settings).

**Opdrachten**

- Azure CLI: [az network nsg-regel weergeven](/cli/azure/network/nsg/rule#az-network-nsg-rule-show)
- PowerShell: [Get-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/get-azurermnetworksecurityruleconfig)

### <a name="change-a-security-rule"></a>Wijzigen van een beveiligingsregel

1. Voer de stappen in [details bekijken van een beveiligingsregel](#view-details-of-a-security-rule).
2. De instellingen naar wens wijzigen en selecteer vervolgens **opslaan**. Zie voor een gedetailleerde beschrijving van alle instellingen [beveiligingsinstellingen van de regel](#security-rule-settings).

**Opdrachten**

- Azure CLI: [az network nsg-regel update](/cli/azure/network/nsg/rule#az-network-nsg-rule-update)
- PowerShell: [Set AzureRmSecurityRuleConfig](/powershell/module/azurerm.network/set-azurermnetworksecurityruleconfig)

### <a name="delete-a-security-rule"></a>Een beveiligingsregel verwijderen

1. Voer de stappen in [details bekijken van een beveiligingsregel](#view-details-of-a-security-rule).
2. Selecteer **verwijderen**, en selecteer vervolgens **Ja**.

**Opdrachten**

- Azure CLI: [az network nsg-regel verwijderen](/cli/azure/network/nsg/rule#az-network-nsg-rule-delete)
- PowerShell: [Remove-AzureRmSecurityRuleConfig](/powershell/module/azurerm.network/remove-azurermnetworksecurityruleconfig)

## <a name="work-with-application-security-groups"></a>Werken met beveiligingsgroepen voor toepassing

Een toepassingsbeveiligingsgroep bevat nul of meer netwerkinterfaces. Zie voor meer informatie, [toepassingsbeveiligingsgroepen](security-overview.md#application-security-groups). Alle netwerkinterfaces in een toepassingsbeveiligingsgroep moeten zich in hetzelfde virtuele netwerk. Zie voor meer informatie over een netwerkinterface toevoegen aan een toepassingsbeveiligingsgroep, [een netwerkinterface toevoegen aan een toepassingsbeveiligingsgroep](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups).

### <a name="create-an-application-security-group"></a>Maak een toepassingsbeveiligingsgroep

1. Selecteer **+ Een resource maken** in de linkerbovenhoek van Azure Portal.
2. In de **Marketplace doorzoeken** Voer *toepassingsbeveiligingsgroep*. Wanneer **toepassingsbeveiligingsgroep** wordt weergegeven in de zoekresultaten, selecteert u deze optie **toepassingsbeveiligingsgroep** opnieuw onder **Alles**, en selecteer vervolgens **Maken**.
3. Voer de volgende gegevens in of selecteer deze, en selecteer **Maken**:

    | Instelling        | Waarde                                                   |
    | ---            | ---                                                     |
    | Naam           | De naam moet uniek zijn binnen een resourcegroep.        |
    | Abonnement   | Selecteer uw abonnement.                               |
    | Resourcegroep | Selecteer een bestaande resourcegroep of maak een nieuwe. |
    | Locatie       | Een locatie selecteren                                       |

**Opdrachten**

- Azure CLI: [az netwerk asg maken](/cli/azure/network/asg#az-network-asg-create)
- PowerShell: [New-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/new-azurermapplicationsecuritygroup)

### <a name="view-all-application-security-groups"></a>Alle beveiligingsgroepen voor toepassing weergeven

1. Selecteer **alle services** links in de linkerbovenhoek van Azure portal.
2. Voer *toepassingsbeveiligingsgroepen* in de **alle services Filter** vak en selecteer vervolgens **toepassingsbeveiligingsgroepen** wanneer deze wordt weergegeven in de lijst met zoekresultaten.

**Opdrachten**

- Azure CLI: [az netwerk asg lijst](/cli/azure/network/asg#az-network-asg-list)
- PowerShell: [Get-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/get-azurermapplicationsecuritygroup)

### <a name="view-details-of-a-specific-application-security-group"></a>Details van de beveiligingsgroep van een specifieke toepassing weergeven

1. Selecteer **alle services** links in de linkerbovenhoek van Azure portal.
2. Voer *toepassingsbeveiligingsgroepen* in de **alle services Filter** vak en selecteer vervolgens **toepassingsbeveiligingsgroepen** wanneer deze wordt weergegeven in de lijst met zoekresultaten.
3. Selecteer de toepassing van een beveiligingsgroep die u wilt weergeven van de details van.

**Opdrachten**

- Azure CLI: [az netwerk asg weergeven](/cli/azure/network/asg#az-network-asg-show)
- PowerShell: [Get-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/get-azurermapplicationsecuritygroup)

### <a name="change-an-application-security-group"></a>Een toepassingsbeveiligingsgroep wijzigen

1. Selecteer **alle services** links in de linkerbovenhoek van Azure portal.
2. Voer *toepassingsbeveiligingsgroepen* in de **alle services Filter** vak en selecteer vervolgens **toepassingsbeveiligingsgroepen** wanneer deze wordt weergegeven in de lijst met zoekresultaten.
3. Selecteer de toepassing van een beveiligingsgroep die u wilt wijzigen van instellingen voor. U kunt toevoegen of verwijderen van tags, of toewijzen of verwijderen van machtigingen aan de toepassingsbeveiligingsgroep.

- Azure CLI: [az netwerk asg update](/cli/azure/network/asg#az-network-asg-update)
- PowerShell: Er is geen PowerShell-cmdlet.

### <a name="delete-an-application-security-group"></a>Een toepassingsbeveiligingsgroep verwijderen

U kunt een toepassingsbeveiligingsgroep niet verwijderen als er netwerkinterfaces in het. Verwijder alle netwerkinterfaces van de toepassingsbeveiligingsgroep door netwerkinterface-instellingen wijzigen of verwijderen van de netwerkinterfaces. Zie voor meer informatie, [toevoegen aan of verwijderen van een netwerkinterface van toepassingsbeveiligingsgroepen](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups) of [verwijderen van een netwerkinterface](virtual-network-network-interface.md#delete-a-network-interface).

1. Selecteer **alle services** links in de linkerbovenhoek van Azure portal.
2. Voer *toepassingsbeveiligingsgroepen* in de **alle services Filter** vak en selecteer vervolgens **toepassingsbeveiligingsgroepen** wanneer deze wordt weergegeven in de lijst met zoekresultaten.
3. Selecteer de toepassing van een beveiligingsgroep die u wilt verwijderen.
4. Selecteer **verwijderen**, en selecteer vervolgens **Ja** verwijderen van de toepassingsbeveiligingsgroep.

**Opdrachten**

- Azure CLI: [az netwerk asg verwijderen](/cli/azure/network/asg#az-network-asg-delete)
- PowerShell: [Remove-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/remove-azurermapplicationsecuritygroup)

## <a name="permissions"></a>Machtigingen

Om uit te voeren taken van netwerkbeveiligingsgroepen, beveiligingsregels en beveiligingsgroepen voor toepassing, moet uw account worden toegewezen aan de [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rol of een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) die is toegewezen de juiste machtigingen die worden vermeld in de volgende tabellen:

### <a name="network-security-group"></a>Netwerkbeveiligingsgroep

| Bewerking                                                        |   Naam                                                                |
|-------------------------------------------------------------- |   -------------------------------------------                         |
| Microsoft.Network/networkSecurityGroups/read                  |   Netwerkbeveiligingsgroep ophalen                                          |
| Microsoft.Network/networkSecurityGroups/write                 |   Maken of bijwerken van netwerkbeveiligingsgroep                             |
| Microsoft.Network/networkSecurityGroups/delete                |   Netwerkbeveiligingsgroep verwijderen                                       |
| Microsoft.Network/networkSecurityGroups/join/action           |   Een netwerkbeveiligingsgroep aan een subnet of netwerkinterface koppelen 


### <a name="network-security-group-rule"></a>Regel voor netwerkbeveiligingsgroep

| Bewerking                                                        |   Naam                                                                |
|-------------------------------------------------------------- |   -------------------------------------------                         |
| Microsoft.Network/networkSecurityGroups/rules/read            |   Regel ophalen                                                            |
| Microsoft.Network/networkSecurityGroups/rules/write           |   Maken of bijwerken van regel                                               |
| Microsoft.Network/networkSecurityGroups/rules/delete          |   Regel verwijderen                                                         |

### <a name="application-security-group"></a>Toepassingsbeveiligingsgroep

| Bewerking                                                                     | Naam                                                     |
| --------------------------------------------------------------             | -------------------------------------------              |
| Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action     | Een IP-configuratie toevoegen aan een toepassingsbeveiligingsgroep|
| Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Een beveiligingsregel toevoegen aan een toepassingsbeveiligingsgroep    |
| Microsoft.Network/applicationSecurityGroups/read                           | Een toepassingsbeveiligingsgroep ophalen                        |
| Microsoft.Network/applicationSecurityGroups/write                          | Maken of bijwerken van een toepassingsbeveiligingsgroep           |
| Microsoft.Network/applicationSecurityGroups/delete                         | Een toepassingsbeveiligingsgroep verwijderen                     |

## <a name="next-steps"></a>Volgende stappen

- Maken van een netwerk of een toepassing via [PowerShell](powershell-samples.md) of [Azure CLI](cli-samples.md) voorbeeld scripts of met behulp van Azure [Resource Manager-sjablonen](template-samples.md)
- Maken en toepassen [Azure policy](policy-samples.md) voor virtuele netwerken
