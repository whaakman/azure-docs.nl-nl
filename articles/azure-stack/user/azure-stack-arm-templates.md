---
title: Gebruik van Azure Resource Manager-sjablonen in Azure-Stack | Microsoft Docs
description: Informatie over het gebruik van Azure Resource Manager-sjablonen in Azure-Stack voor resources inrichten.
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 7648855011e8f77c35713d2d2ae50f2e474a08a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure Resource Manager-sjablonen in Azure-Stack gebruiken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure Resource Manager-sjablonen implementeren en inrichten van alle resources voor uw toepassing in een enkele, gecoördineerde bewerking. U kunt ook opnieuw implementeren sjablonen om de resources in de resourcegroep te wijzigen.

Deze sjablonen kunnen worden geïmplementeerd met de Stack van Microsoft Azure-portal, PowerShell, de opdrachtregel en Visual Studio.

De volgende Quick Start-sjablonen zijn beschikbaar op [GitHub](http://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-non-high-availability"></a>Implementeren van SharePoint (niet-hoge beschikbaarheid)
Gebruik de PowerShell DSC-uitbreiding voor het maken van een SharePoint 2013-farm met de volgende bronnen:

* Een virtueel netwerk
* Drie storage-accounts
* Twee externe load balancers
* Een virtuele machine die zijn geconfigureerd als een domeincontroller voor een nieuw forest met één domein
* Een virtuele machine die zijn geconfigureerd als een zelfstandige server van SQL Server 2014
* Een virtuele machine die zijn geconfigureerd als een SharePoint 2013-farm met één machine

## <a name="deploy-ad-non-high-availability"></a>Implementeren van AD (niet-hoge beschikbaarheid)
Gebruik de PowerShell DSC-uitbreiding voor het maken van een AD-domeincontrollerserver met de volgende bronnen:

* Een virtueel netwerk
* Een opslagaccount
* Een externe load balancer
* Een virtuele machine die zijn geconfigureerd als een domeincontroller voor een nieuw forest met één domein

## <a name="deploy-adsql-non-high-availability"></a>Implementeren van AD/SQL (niet-hoge beschikbaarheid)
Gebruik de PowerShell DSC-uitbreiding voor het maken van een zelfstandige server van SQL Server 2014 met de volgende bronnen:

* Een virtueel netwerk
* Twee storage-accounts
* Een externe load balancer
* Een virtuele machine die zijn geconfigureerd als een domeincontroller voor een nieuw forest met één domein
* Een virtuele machine die zijn geconfigureerd als een zelfstandige server van SQL Server 2014

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server
Gebruik de PowerShell DSC-uitbreiding voor het configureren van een bestaande virtuele machine lokale Configuration Manager (LCM) en registreert u dit naar een Azure Automation-Account DSC-Pull-Server.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Een virtuele machine maken van een gebruikersinstallatiekopie
Een virtuele machine uit een aangepaste installatiekopie maken. Deze sjabloon implementeert ook een virtueel netwerk (met DNS), openbare IP-adres en een netwerkinterface.

## <a name="simple-vm"></a>Eenvoudige VM
Implementeer een Windows-VM met een virtueel netwerk (met DNS), openbare IP-adres en een netwerkinterface.

## <a name="cancel-a-running-template-deployment"></a>Annuleren van een actieve sjabloonimplementatie
Als u wilt annuleren een actieve sjabloonimplementatie, gebruiken de `Stop-AzureRmResourceGroupDeployment` PowerShell-cmdlet.

## <a name="next-steps"></a>Volgende stappen
[Sjablonen implementeren met de portal](azure-stack-deploy-template-portal.md)

[Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)

