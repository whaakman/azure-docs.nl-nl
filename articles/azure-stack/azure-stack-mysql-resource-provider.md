---
title: MySQL-databases als PaaS op Azure-Stack gebruiken | Microsoft Docs
description: Ontdek hoe u implementeert de Bronprovider van MySQL en MySQL-databases opgeven als een service op Azure-Stack.
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
ms.date: 06/21/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 24ba595413cde07c420a94de234d7926e0eb0e7f
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309839"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>MySQL-database gebruiken op Microsoft Azure-Stack

U kunt de MySQL-resourceprovider API's voor het gebruik van MySQL-databases geïmplementeerd in Azure-Stack implementeren. Zie voor meer informatie over de resourceprovider API [Windows Azure Pack MySQL Resource Provider REST API-verwijzing](https://msdn.microsoft.com/library/dn528442.aspx).

MySQL-databases zijn algemene op websites en veel website platforms ondersteunen. U kunt bijvoorbeeld WordPress-websites met de Web-Apps-platform als een service (PaaS)-invoegtoepassing maken.

Nadat u de resourceprovider implementeert, kunt u:

* MySQL-servers en databases die gebruikmaken van Azure Resource Manager deployment sjablonen maken.
* MySQL-databases opgeven als een service.  

## <a name="mysql-resource-provider-adapter-architecture"></a>Architectuur van MySQL resource provider-adapter

De resourceprovider heeft de volgende onderdelen:

* **MySQL resource provider adapter virtuele machine (VM)**, dit is een Windows Server-VM met de provider-services.
* **De resourceprovider**, dat verzoeken verwerkt en toegang tot bronnen van de database.
* **Servers die als host MySQL-Server fungeren**, die een capaciteit voor databases die worden genoemd die als host fungeert voor servers. U kunt maken van MySQL-exemplaren of bieden toegang tot externe MySQL-exemplaren. De [Azure Stack Snelstartgalerie](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) is een van de voorbeeldsjabloon die u kunt gebruiken voor:

  * Maak een MySQL-server voor u.
  * Downloaden en implementeren vanuit Azure Marketplace een MySQL-Server.

> [!NOTE]
> Hosting-servers die zijn geïnstalleerd op Azure-Stack moeten geïntegreerde systemen worden gemaakt van een tenantabonnement. Ze kunnen niet worden gemaakt van het standaard provider-abonnement. Ze moeten worden gemaakt vanuit de tenantportal of vanuit een PowerShell-sessie met een juiste aanmelden. Alle hosting-servers factureerbare VM's zijn en moeten licenties hebben. De servicebeheerder kan de eigenaar van de tenant-abonnement zijn.

### <a name="required-privileges"></a>Vereiste bevoegdheden

Het systeem-account moet hebben de volgende bevoegdheden:

* **Database:** maken, verwijderen
* **Aanmelding:** maken, instellen, verwijderen, toekennen, intrekken  

## <a name="next-steps"></a>Volgende stappen

[De resourceprovider MySQL implementeren](azure-stack-mysql-resource-provider-deploy.md)
