---
title: Gebruik van Azure Resource Manager-sjablonen in Azure-Stack | Microsoft Docs
description: Informatie over het gebruik van Azure Resource Manager-sjablonen in Azure-Stack voor resources inrichten.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 9c4d538f77ae056163fd17aa547162a4ad3eff63
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure Resource Manager-sjablonen in Azure-Stack gebruiken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt Azure Resource Manager-sjablonen gebruiken om te implementeren en inrichten van alle resources voor uw toepassing in een enkele, gecoördineerde bewerking. U kunt ook opnieuw implementeren sjablonen om de resources in een resourcegroep te wijzigen.

Deze sjablonen kunnen worden geïmplementeerd met de Stack van Microsoft Azure-portal, PowerShell, de opdrachtregel en Visual Studio.

De volgende Quick Start-sjablonen zijn beschikbaar op [GitHub](http://aka.ms/azurestackgithub).

## <a name="deploy-sharepoint-server-non-high-availability-deployment"></a>SharePoint-Server (hoge beschikbaarheid-implementatie) implementeren

Gebruik de PowerShell DSC-uitbreiding voor het maken van een SharePoint Server 2013-farm met de volgende bronnen:

* Een virtueel netwerk
* Drie storage-accounts
* Twee externe load balancers
* Een virtuele machine (VM) geconfigureerd als een domeincontroller voor een nieuw forest met één domein
* Een virtuele machine die zijn geconfigureerd als een zelfstandige server van SQL Server 2014
* Een virtuele machine die zijn geconfigureerd als een SharePoint Server 2013-farm met één machine

## <a name="deploy-ad-non-high-availability-deployment"></a>Implementeren van AD (niet-hoge-beschikbaarheid-implementatie)

Gebruik de PowerShell DSC-uitbreiding voor het maken van een AD-domeincontrollerserver met de volgende bronnen:

* Een virtueel netwerk
* Een opslagaccount
* Een externe load balancer
* Een virtuele machine (VM) geconfigureerd als een domeincontroller voor een nieuw forest met één domein

## <a name="deploy-adsql-non-high-availability-deployment"></a>Implementeren van AD/SQL (niet-hoge-beschikbaarheid-implementatie)

Gebruik de PowerShell DSC-uitbreiding voor het maken van een zelfstandige server van SQL Server 2014 met de volgende bronnen:

* Een virtueel netwerk
* Twee storage-accounts
* Een externe load balancer
* Een virtuele machine (VM) geconfigureerd als een domeincontroller voor een nieuw forest met één domein
* Een virtuele machine die zijn geconfigureerd als een zelfstandige server van SQL Server 2014

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Gebruik de PowerShell DSC-uitbreiding voor het configureren van een bestaande virtuele machine lokale Configuration Manager (LCM) en registreert u dit naar een Azure Automation-Account DSC-Pull-Server.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Een virtuele machine maken van een gebruikersinstallatiekopie

Een virtuele machine uit een aangepaste installatiekopie maken. Deze sjabloon implementeert ook een virtueel netwerk (met DNS), openbare IP-adres en een netwerkinterface.

## <a name="basic-virtual-machine"></a>Basic virtuele machine

Implementeer een Windows-VM met een virtueel netwerk (met DNS), openbare IP-adres en een netwerkinterface.

## <a name="cancel-a-running-template-deployment"></a>Annuleren van een actieve sjabloonimplementatie

Als u wilt annuleren een actieve sjabloonimplementatie, gebruiken de `Stop-AzureRmResourceGroupDeployment` PowerShell-cmdlet.

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen implementeren met de portal](azure-stack-deploy-template-portal.md)
* [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
