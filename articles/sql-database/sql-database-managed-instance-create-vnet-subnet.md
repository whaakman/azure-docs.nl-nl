---
title: Een virtueel netwerk maken voor Azure SQL Database Managed Instance | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een virtueel netwerk waarin u Azure SQL Database Managed Instance kunt implementeren.
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
ms.date: 01/15/2019
ms.openlocfilehash: df8581f572ded3268b0dc1b9243476b76fb5c688
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55568512"
---
# <a name="create-a-virtual-network-for-azure-sql-database-managed-instance"></a>Een virtueel netwerk maken voor Azure SQL Database Managed Instance

In dit artikel wordt uitgelegd hoe u een geldige virtuele netwerk en subnet waarin u Azure SQL Database Managed Instance kunt implementeren.

Azure SQL Database Managed Instance moet worden geïmplementeerd in een Azure [virtueel netwerk](../virtual-network/virtual-networks-overview.md). Deze implementatie kunt de volgende scenario's:

- Privé IP-adres beveiligen
- Verbinding maken met een beheerd exemplaar rechtstreeks vanuit een on-premises netwerk
- Verbinding te maken van een beheerd exemplaar met gekoppelde server of een andere on-premises gegevensarchief
- Een beheerd exemplaar verbinden met Azure-resources  

> [!Note]
> U moet [bepaalt de grootte van het subnet voor beheerd exemplaar](sql-database-managed-instance-determine-size-vnet-subnet.md) voordat u het eerste exemplaar implementeert. U kunt de grootte van het subnet kan niet wijzigen nadat u de resources binnen geplaatst.
>
> Als u van plan bent een bestaand virtueel netwerk gebruiken, moet u die netwerkconfiguratie om te voldoen aan uw beheerde exemplaar wijzigen. Zie voor meer informatie, [wijzigen van een bestaand virtueel netwerk voor beheerd exemplaar](sql-database-managed-instance-configure-vnet-subnet.md).

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

De eenvoudigste manier om te maken en configureren van een virtueel netwerk is met een sjabloon voor de implementatie Azure Resource Manager.

1. Meld u aan bij Azure Portal.

2. Selecteer de **implementeren in Azure** knop:

   <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   Deze knop opent een formulier dat u gebruiken kunt om de netwerkomgeving te configureren waarin u Managed Instance kunt implementeren.

   > [!Note]
   > Deze Azure Resource Manager-sjabloon implementeert een virtueel netwerk met twee subnetten. Een subnet, met de naam **ManagedInstances**, is gereserveerd voor Managed Instance en heeft een vooraf geconfigureerde routetabel. Het andere subnet, met de naam **standaard**, wordt gebruikt voor andere resources die toegang moeten hebben tot Managed Instance (bijvoorbeeld Azure virtuele Machines).

3. Configureer de netwerkomgeving. U kunt parameters van uw netwerkomgeving configureren op de volgende notatie:

   ![Resource Manager-sjabloon voor het configureren van het Azure-netwerk](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

   U mogelijk de namen van het virtuele netwerk en subnetten wijzigen en aanpassen van de IP-adresbereiken die zijn gekoppeld aan uw netwerkresources. Nadat u hebt geselecteerd de **aankoop** knop, dit formulier wordt maken en configureren uw omgeving. Als u niet twee subnetten nodig hebt, kunt u de standaardwaarde verwijderen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar?](sql-database-managed-instance.md).
- Meer informatie over [connectiviteitsarchitectuur in het beheerde exemplaar](sql-database-managed-instance-connectivity-architecture.md).
- Meer informatie over het [wijzigen van een bestaand virtueel netwerk voor beheerd exemplaar](sql-database-managed-instance-configure-vnet-subnet.md).
- Zie voor een zelfstudie die laat zien hoe u een virtueel netwerk maken en een database herstellen vanuit een databaseback-up maken van een beheerd exemplaar, [maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Zie voor DNS-problemen, [configureren van een aangepaste DNS](sql-database-managed-instance-custom-dns.md).
