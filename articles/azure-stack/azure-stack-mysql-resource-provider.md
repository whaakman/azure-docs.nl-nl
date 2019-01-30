---
title: MySQL-databases gebruiken als PaaS op Azure Stack | Microsoft Docs
description: Leer hoe u kunt de MySQL-Resourceprovider implementeren en MySQL-databases opgeven als een service in Azure Stack.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 03e429e1a40f9b23a2a3bedef5ef488d81392d9c
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236901"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>MySQL-databases gebruiken op Microsoft Azure Stack

MySQL-databases worden vaak op websites en veel website platforms ondersteunen. Bijvoorbeeld, kunt u WordPress-website met behulp van de Web-Apps-platform als een invoegtoepassing voor service (PaaS).

Nadat u de resourceprovider hebt geïmplementeerd, kunt u het volgende doen:

* MySQL-servers en databases met behulp van sjablonen voor Azure Resource Manager-implementatie maken.
* MySQL-databases opgeven als een service.  

## <a name="mysql-resource-provider-adapter-architecture"></a>Architectuur van MySQL resource provider-adapter

De resourceprovider heeft de volgende onderdelen:

* **De MySQL-resource provider adapter virtuele machine (VM)**, dit is een Windows Server-VM waarop de services provider wordt uitgevoerd.
* **De resourceprovider**, die aanvragen verwerkt en toegang tot een database-resources.
* **Servers die als host van de MySQL-Server**, Hier vindt u de capaciteit voor databases die worden genoemd die als host fungeert voor servers. U kunt MySQL exemplaren zelf maken of toegang bieden tot externe MySQL-exemplaren. De [Azure Stack-Snelstartgalerie](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) is een van de voorbeeldsjabloon die u kunt gebruiken voor:

  * Een MySQL-server voor u gemaakt.
  * Downloaden en implementeren van een MySQL-Server op Azure Marketplace.

> [!NOTE]
> Die als host fungeert voor servers die zijn geïnstalleerd in Azure Stack moeten geïntegreerde systemen worden gemaakt op basis van een tenantabonnement. Ze kunnen niet worden gemaakt van de provider standaardabonnement. Ze moeten worden gemaakt van de tenantportal of vanuit een PowerShell-sessie met een juiste aanmelding. Alle hosting-servers zijn factureerbaar VM's en licenties moeten zijn. De service-beheerder kan de eigenaar van de tenantabonnement zijn.

### <a name="required-privileges"></a>Vereiste bevoegdheden

Het systeem-account moet de volgende bevoegdheden hebben:

* **Database:** maken, verwijderen
* **Aanmelding:** maken, instellen, verwijderen, verlenen of intrekken  

## <a name="next-steps"></a>Volgende stappen

[De MySQL-resourceprovider implementeren](azure-stack-mysql-resource-provider-deploy.md)
