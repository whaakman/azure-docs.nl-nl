---
title: Met behulp van SQL-databases in Azure Stack | Microsoft Docs
description: Meer informatie over hoe u SQL-databases kunt implementeren als een service op Azure Stack en de snelle stappen voor het implementeren van de SQL Server-resource provider-adapter.
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
ms.openlocfilehash: 3d608843ef31a1ed665fcb1fd90b822f34f77fdd
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086349"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>SQL-databases gebruiken op Microsoft Azure Stack

Gebruik van de adapter van SQL Server resource provider voor SQL-databases als een service van [Azure Stack](azure-stack-poc.md). Nadat u de resourceprovider installeren en met een of meer exemplaren van SQL Server verbinden, kunnen u en uw gebruikers maken:

- Databases voor cloud-eigen apps.
- Websites die gebruikmaken van SQL.
- Workloads die gebruikmaken van SQL.

De resourceprovider alle van de database geen biedt mogelijkheden voor het beheer van [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Bijvoorbeeld: elastische pools die automatisch worden toegewezen resources worden niet ondersteund. Echter, de vergelijkbare resource provider ondersteunt maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op een SQL Server-database. 

## <a name="sql-resource-provider-adapter-architecture"></a>Architectuur van SQL-resource provider-adapter

De resourceprovider bestaat uit de volgende onderdelen:

- **De SQL-resource provider adapter virtuele machine (VM)**, dit is een Windows Server-VM die het provider-services worden uitgevoerd.
- **De resourceprovider**, die aanvragen verwerkt en toegang tot een database-resources.
- **Servers die als host van SQL Server**, Hier vindt u de capaciteit voor databases met de naam hostingservers mogelijk.

U moet ten minste één exemplaar van SQL Server maakt of toegang bieden tot de externe SQL Server-exemplaren.

> [!NOTE]
> Die als host fungeert voor servers die zijn geïnstalleerd in Azure Stack moeten geïntegreerde systemen worden gemaakt op basis van een tenantabonnement. Ze kunnen niet worden gemaakt van de provider standaardabonnement. Ze moeten worden gemaakt van de tenantportal of met behulp van PowerShell met de juiste aanmelding. Alle hostingservers factureerbare virtuele machines zijn en moeten licenties hebben. De service-beheerder kan de eigenaar van de tenantabonnement zijn.

## <a name="next-steps"></a>Volgende stappen

[De SQL Server-resourceprovider implementeren](azure-stack-sql-resource-provider-deploy.md)
