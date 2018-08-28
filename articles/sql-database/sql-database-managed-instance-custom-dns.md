---
title: Azure SQL Database Managed Instance aangepaste DNS | Microsoft Docs
description: Dit onderwerp beschrijft de configuratie-opties voor een aangepaste DNS-server met een Azure SQL Database Managed Instance.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: af9afcbf97df5f3d7fa82f6ea0163c714fa4f582
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051738"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Een aangepaste DNS configureren voor Azure SQL Database Managed Instance

Een Azure SQL Database Managed Instance (preview) moet worden geïmplementeerd in een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md). Er zijn enkele scenario's (dat wil zeggen gekoppelde servers naar andere SQL-exemplaren in uw omgeving cloud of hybride) waarvoor persoonlijke hostnamen worden omgezet in het beheerde exemplaar. In dit geval moet u een aangepaste DNS-server in Azure configureren. Omdat het beheerde exemplaar van de dezelfde DNS-server voor de interne werking gebruikt, moet de DNS-configuratie van het virtuele netwerk compatibel zijn met Managed Instance. 

Als u wilt maken van een aangepaste DNS-configuratie is compatibel met het beheerde exemplaar, moet u: 
- Aangepaste DNS-server configureren zodat deze kunnen openbare domeinnamen omzetten 
- Azure recursieve naamomzetting DNS IP-adres 168.63.129.16 aan het einde van het virtuele netwerk DNS-lijst plaatsen 
 
## <a name="setting-up-custom-dns-servers-configuration"></a>Instellen van aangepaste DNS-servers configureren

1. Zoeken in de Azure-portal, aangepaste DNS-optie voor uw VNet.

   ![aangepaste DNS-optie](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Schakel over naar aangepaste en voer uw aangepaste DNS-server IP-adres, evenals Azure recursieve resolvers IP-adres 168.63.129.16. 

   ![aangepaste DNS-optie](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Lijst met niet instellen van Azure recursieve naamomzetting in DNS zorgt ervoor dat het beheerde exemplaar in te voeren van slechte staat. Herstellen vanaf dat staat mogelijk moet u nieuwe instantie in een VNet aan het nalevingsbeleid voor netwerken, serviceniveau-Instantiegegevens maken en herstellen van uw databases. Zie [VNet-configuratie](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar](sql-database-managed-instance.md)
- Zie voor een zelfstudie wordt uitgelegd hoe u een nieuwe Managed Instance maakt u [het maken van een beheerd exemplaar](sql-database-managed-instance-create-tutorial-portal.md).
- Zie voor meer informatie over het configureren van een VNet voor een beheerd exemplaar [VNet-configuratie voor beheerde exemplaren](sql-database-managed-instance-vnet-configuration.md)
