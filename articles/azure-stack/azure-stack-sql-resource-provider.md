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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: b1cc1fad6b0831bcf0bab5ba4f37b753c3cf33ca
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>SQL-databases op Microsoft Azure-Stack gebruiken
Gebruik van de adapter van SQL Server resource provider voor SQL-databases weergeven als een service van [Azure Stack](azure-stack-poc.md). Na het installeren van de resourceprovider en verbinden met een of meer exemplaren van SQL Server, kunnen u en uw gebruikers maken:
- Databases voor cloud-systeemeigen apps.
- Websites op basis van SQL.
- Werklasten die zijn gebaseerd op SQL.
Er geen voor het inrichten van een virtuele machine (VM) die als host fungeert voor SQL Server elke keer.

De resourceprovider biedt geen ondersteuning voor alle database beheermogelijkheden van [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Bijvoorbeeld, pools voor elastische databases en de mogelijkheid om de prestaties van de database automatisch omhoog en omlaag bellen zijn niet beschikbaar. Echter, de resource provider biedt ondersteuning voor vergelijkbare maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen. De API is niet compatibel met SQL-Database.

## <a name="sql-resource-provider-adapter-architecture"></a>Architectuur van SQL resource provider-adapter
De resourceprovider bestaat uit drie onderdelen:

- **De SQL resource provider adapter VM**, dit is een virtuele Windows-computer die de provider-services worden uitgevoerd.
- **De resourceprovider zelf**, die inrichting verzoeken verwerkt en resources van de database zichtbaar gemaakt.
- **Servers waarop SQL Server worden gehost**, hosting-servers die een capaciteit voor databases worden aangeroepen.

U moet een (of meer) exemplaren van SQL Server maken en/of toegang tot de externe SQL Server-exemplaren.

> [!NOTE]
> Hosting-servers die zijn geïnstalleerd op Azure-Stack moeten geïntegreerde systemen worden gemaakt van een tenantabonnement. Ze kunnen niet worden gemaakt van het standaard provider-abonnement. Ze moeten worden gemaakt vanuit de tenantportal of vanuit een PowerShell-sessie met een juiste aanmelden. Alle hosting-servers toerekenbare VM's zijn en moeten de juiste licenties hebt. De servicebeheerder kan de eigenaar van de tenant-abonnement zijn.


## <a name="next-steps"></a>Volgende stappen

[De resource-provider voor SQL Server implementeren](azure-stack-sql-resource-provider-deploy.md)
