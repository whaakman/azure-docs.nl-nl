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
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: 456f27b97ee644aef34f9bb9e2c0525bd61c1c84
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056880"
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure Resource Manager-sjablonen maken in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt Azure Resource Manager-sjablonen gebruiken om te implementeren en inrichten van alle resources voor uw toepassing in een enkele, gecoördineerde bewerking. U kunt ook opnieuw implementeren sjablonen om de resources in een resourcegroep te wijzigen.

Deze sjablonen kunnen worden geïmplementeerd met de Microsoft Azure Stack-portal, PowerShell, de opdrachtregel en Visual Studio.

De volgende Quick Start-sjablonen zijn beschikbaar op [GitHub](http://aka.ms/azurestackgithub).

## <a name="deploy-sharepoint-server-non-high-availability-deployment"></a>Implementatie van SharePoint-Server (implementatie met hoge beschikbaarheid)

Gebruik de PowerShell DSC-extensie om te maken van een SharePoint Server 2013-farm met de volgende bronnen:

* Een virtueel netwerk
* Drie opslagaccounts
* Twee externe load balancers
* Een virtuele machine (VM) geconfigureerd als een domeincontroller voor een nieuw forest met één domein
* Een virtuele machine die zijn geconfigureerd als een zelfstandige server van SQL Server 2014
* Een virtuele machine die zijn geconfigureerd als een SharePoint Server 2013-farm met één machine

## <a name="deploy-ad-non-high-availability-deployment"></a>Implementeren van AD (niet-high-beschikbaarheid-implementatie)

Gebruik de PowerShell DSC-extensie om te maken van een AD domain controller-server met de volgende bronnen:

* Een virtueel netwerk
* Een storage-account
* Een externe load balancer
* Een virtuele machine (VM) geconfigureerd als een domeincontroller voor een nieuw forest met één domein

## <a name="deploy-adsql-non-high-availability-deployment"></a>Implementeren van AD/SQL (niet-high-beschikbaarheid-implementatie)

Gebruik de PowerShell DSC-extensie om te maken van een zelfstandige server van SQL Server 2014 met de volgende bronnen:

* Een virtueel netwerk
* Twee opslagaccounts
* Een externe load balancer
* Een virtuele machine (VM) geconfigureerd als een domeincontroller voor een nieuw forest met één domein
* Een virtuele machine die zijn geconfigureerd als een zelfstandige server van SQL Server 2014

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Gebruik de PowerShell DSC-extensie aan een bestaande virtuele machine lokale Configuration Manager (LCM) configureren en deze naar een Azure Automation-Account DSC Pull-Server te registreren.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Een virtuele machine maken van een gebruikersinstallatiekopie

Een virtuele machine maken vanaf een aangepaste installatiekopie. Deze sjabloon implementeert ook een virtueel netwerk (met DNS), openbare IP-adres en een netwerkinterface.

## <a name="basic-virtual-machine"></a>Eenvoudige virtuele machine

Implementeer een virtuele Windows-machine met een virtueel netwerk (met DNS), openbare IP-adres en een netwerkinterface.

## <a name="cancel-a-running-template-deployment"></a>Annuleren van een actieve sjabloonimplementatie

Als u wilt annuleren van een actieve sjabloonimplementatie, gebruiken de `Stop-AzureRmResourceGroupDeployment` PowerShell-cmdlet.

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen implementeren met de portal](azure-stack-deploy-template-portal.md)
* [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
