---
title: Beheerd exemplaar voor Azure SQL Database VNet maken | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u een virtueel netwerk (VNet), waarin u een Azure SQL Database Managed Instance kunt implementeren.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: a8000fb26ce5496a9c62ba475b862f8f80adf6b7
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041761"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Een VNet configureren voor het beheerde exemplaar van Azure SQL Database

In dit onderwerp wordt uitgelegd hoe u een geldige VNet en subnet waarin u Azure SQL Database beheerde instanties kunt implementeren.

Azure SQL Database Managed Instance moet worden geïmplementeerd in een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md). Deze implementatie kunt de volgende scenario's:

- Beveilig privé IP-adres.
- Verbinding maken met een beheerd exemplaar rechtstreeks vanuit een on-premises netwerk
- Verbinding te maken van een beheerd exemplaar met gekoppelde server of een andere on-premises gegevensarchief
- Een beheerd exemplaar verbinden met Azure-resources  

  > [!Note]
  > U moet [bepaalt de grootte van het subnet voor Managed Instance](sql-database-managed-instance-determine-size-vnet-subnet.md) voordat u implementeert in eerste instantie omdat de sunet kan niet worden gewijzigd nadat u plaatst resources van het in.
  > Als u van plan bent een bestaand virtueel netwerk gebruiken, moet u die netwerkconfiguratie om te voldoen aan uw beheerde exemplaar wijzigen. Zie voor meer informatie, [wijzigen bestaand virtueel netwerk voor beheerd exemplaar](sql-database-managed-instance-configure-vnet-subnet.md).

## <a name="create-a-new-virtual-network"></a>Nieuw virtueel netwerk maken

De eenvoudigste manier om te maken en configureren van virtueel netwerk is met Azure Resource Manager-implementatiesjabloon.

1. Meld u aan bij Azure Portal.

2. Gebruik **implementeren in Azure** knop om virtuele netwerk in Azure-cloud te implementeren:

   <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   Deze knop opent u een formulier dat u gebruiken kunt om netwerkomgeving te configureren waarin u Managed Instance kunt implementeren.

   > [!Note]
   > Deze Azure Resource Manager-sjabloon implementeert een virtueel netwerk met twee subnetten. Een subnet met de naam **ManagedInstances** is gereserveerd voor beheerde instanties en heeft routetabel, vooraf geconfigureerd terwijl het andere subnet met de naam **standaard** wordt gebruikt voor andere resources die toegang moeten hebben tot de beheerde Het exemplaar (bijvoorbeeld Azure virtuele Machines). U kunt verwijderen **standaard** subnet als u deze niet nodig.

3. Netwerkomgeving configureren. U kunt parameters van uw netwerkomgeving configureren op de volgende notatie:

![Azure-netwerk configureren](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

U mogelijk de namen wijzigen van het VNet en subnetten en IP-adresbereiken die zijn gekoppeld aan uw netwerkresources aanpassen. Zodra u op "Aankoop" knop drukt, wordt dit formulier maken en configureren van uw omgeving. Als u niet twee subnetten nodig hebt, kunt u de standaardwaarde verwijderen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar](sql-database-managed-instance.md).
- Meer informatie over [connectiviteitsarchitectuur in het beheerde exemplaar](sql-database-managed-instance-connectivity-architecture.md).
- Wissen hoe [bestaand virtueel netwerk voor beheerd exemplaar wijzigen](sql-database-managed-instance-configure-vnet-subnet.md)
- Zie voor een zelfstudie waarin wordt getoond hoe een VNet maken en een database herstellen vanuit een databaseback-up maken van een beheerd exemplaar, [het maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Zie voor DNS-problemen, [een aangepaste DNS-server configureren](sql-database-managed-instance-custom-dns.md)
