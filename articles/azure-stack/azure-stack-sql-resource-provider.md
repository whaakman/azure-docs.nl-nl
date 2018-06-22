---
title: Met behulp van SQL-databases op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u SQL-databases kan implementeren als een service op Azure-Stack en de snelle stappen voor het implementeren van de SQL Server resource provider-adapter.
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
ms.openlocfilehash: 55d0e51606e8768a01c0b5a7766dbafe24d97a0d
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307822"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>SQL-databases op Microsoft Azure-Stack gebruiken

De API van de resource provider-adapter in SQL Server gebruiken voor het SQL-databases weergeven als een service van [Azure Stack](azure-stack-poc.md). Na het installeren van de resourceprovider en verbinden met een of meer exemplaren van SQL Server, kunnen u en uw gebruikers maken:

- Databases voor cloud-systeemeigen apps.
- Websites die gebruikmaken van SQL.
- Werkbelastingen die gebruikmaken van SQL.

De resourceprovider de database geen biedt mogelijkheden voor het beheer van [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Bijvoorbeeld: elastische pools waarmee bronnen worden automatisch toegewezen worden niet ondersteund. Echter resource provider ondersteunt de vergelijkbare maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op een SQL Server-database. Zie voor meer informatie over de resourceprovider API [Windows Azure Pack SQL Server Resource Provider REST API Reference](https://msdn.microsoft.com/library/dn528529.aspx).

>[!NOTE]
De SQL Server-resourceprovider API is niet compatibel met Azure SQL Database.

## <a name="sql-resource-provider-adapter-architecture"></a>Architectuur van SQL resource provider-adapter

De resourceprovider bestaat uit de volgende onderdelen:

- **De SQL resource provider adapter virtuele machine (VM)**, dit is een Windows Server-VM met de provider-services.
- **De resourceprovider**, dat verzoeken verwerkt en toegang tot bronnen van de database.
- **Servers waarop SQL Server worden gehost**, hosting-servers die een capaciteit voor databases worden aangeroepen.

U moet ten minste één exemplaar van SQL Server maken of toegang tot de externe SQL Server-exemplaren.

> [!NOTE]
> Hosting-servers die zijn geïnstalleerd op Azure-Stack moeten geïntegreerde systemen worden gemaakt van een tenantabonnement. Ze kunnen niet worden gemaakt van het standaard provider-abonnement. Ze moeten worden gemaakt van de tenantportal of met behulp van PowerShell met de juiste aanmelden. Alle hosting-servers factureerbare virtuele machines zijn en moeten licenties hebben. De servicebeheerder kan de eigenaar van de tenant-abonnement zijn.

## <a name="next-steps"></a>Volgende stappen

[De resource-provider voor SQL Server implementeren](azure-stack-sql-resource-provider-deploy.md)
