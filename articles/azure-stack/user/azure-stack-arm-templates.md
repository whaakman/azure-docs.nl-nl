---
title: Azure Resource Manager-sjablonen maken in Azure Stack | Microsoft Docs
description: Informatie over het gebruik van Azure Resource Manager-sjablonen in Azure Stack voor inrichting van resources.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: a50f91d5cbbc0eac7080437c96144014dad651ee
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161762"
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure Resource Manager-sjablonen maken in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt Azure Resource Manager-sjablonen gebruiken om te implementeren en inrichten van alle resources voor uw toepassing in een enkele, gecoördineerde bewerking. U kunt ook opnieuw implementeren sjablonen om de resources in een resourcegroep te wijzigen.

Deze sjablonen kunnen worden geïmplementeerd met de Microsoft Azure Stack-portal, PowerShell, de opdrachtregel en Visual Studio.

De volgende Quick Start-sjablonen zijn beschikbaar op [GitHub](http://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-server-non-high-availability-deployment"></a>Implementatie van SharePoint-Server (implementatie met hoge beschikbaarheid)

Gebruik de PowerShell DSC-extensie voor [maken van een SharePoint Server 2013-farm](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/AzureStackTechnicalPreview1/sharepoint-2013-non-ha) die omvat de volgende resources:

* Een virtueel netwerk
* Drie opslagaccounts
* Twee externe load balancers
* Een virtuele machine (VM) geconfigureerd als een domeincontroller voor een nieuw forest met één domein
* Een virtuele machine die zijn geconfigureerd als een zelfstandige server van SQL Server 2014
* Een virtuele machine die zijn geconfigureerd als een SharePoint Server 2013-farm met één machine

## <a name="deploy-ad-non-high-availability-deployment"></a>Implementeren van AD (niet-high-beschikbaarheid-implementatie)

Gebruik de PowerShell DSC-extensie voor [maken van een AD-domeincontrollerserver](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/AzureStackTechnicalPreview1/ad-non-ha) die omvat de volgende resources:

* Een virtueel netwerk
* Een storage-account
* Een externe load balancer
* Een virtuele machine (VM) geconfigureerd als een domeincontroller voor een nieuw forest met één domein

## <a name="deploy-adsql-non-high-availability-deployment"></a>Implementeren van AD/SQL (niet-high-beschikbaarheid-implementatie)

Gebruik de PowerShell DSC-extensie voor [maken van een zelfstandige server van SQL Server 2014](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/AzureStackTechnicalPreview1/sql-2014-non-ha) die omvat de volgende resources:

* Een virtueel netwerk
* Twee opslagaccounts
* Een externe load balancer
* Een virtuele machine (VM) geconfigureerd als een domeincontroller voor een nieuw forest met één domein
* Een virtuele machine die zijn geconfigureerd als een zelfstandige server van SQL Server 2014

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Gebruik de PowerShell DSC-extensie aan een bestaande virtuele machine lokale Configuration Manager (LCM) configureren en deze naar een Azure Automation-Account DSC Pull-Server te registreren.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Een virtuele machine maken van een gebruikersinstallatiekopie

[Een virtuele machine maken van een aangepaste gebruikersinstallatiekopie](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/AzureStackTechnicalPreview1/101-vm-from-user-image). Deze sjabloon implementeert ook een virtueel netwerk (met DNS), openbare IP-adres en een netwerkinterface.

## <a name="basic-virtual-machine"></a>Eenvoudige virtuele machine

[Een Windows-VM implementeren](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/AzureStackTechnicalPreview1/101-simple-windows-vm) die bestaat uit een virtueel netwerk (met DNS), openbare IP-adres en een netwerkinterface.

## <a name="cancel-a-running-template-deployment"></a>Annuleren van een actieve sjabloonimplementatie

Als u wilt annuleren van een actieve sjabloonimplementatie, gebruiken de [Stop-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/stop-azurermresourcegroupdeployment) PowerShell-cmdlet.

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen implementeren met de portal](azure-stack-deploy-template-portal.md)
* [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
