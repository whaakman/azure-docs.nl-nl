---
title: Azure SQL Database Managed Instance aangepaste DNS | Microsoft Docs
description: Dit onderwerp beschrijft de configuratie-opties voor een aangepaste DNS-server met een Azure SQL Database Managed Instance.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 09/23/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 2d1bb7e8522da32dd33933261ea41b578f8afac1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949482"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Een aangepaste DNS configureren voor Azure SQL Database Managed Instance

Een Azure SQL Database Managed Instance moet worden geïmplementeerd in een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md). Er zijn enkele scenario's (dat wil zeggen gekoppelde servers naar andere SQL-exemplaren in uw omgeving cloud of hybride) waarvoor persoonlijke hostnamen worden omgezet in het beheerde exemplaar. In dit geval moet u een aangepaste DNS-server in Azure configureren. Omdat het beheerde exemplaar van de dezelfde DNS-server voor de interne werking gebruikt, moet de DNS-configuratie van het virtuele netwerk compatibel zijn met Managed Instance. 

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
- Zie voor een zelfstudie wordt uitgelegd hoe u een nieuwe Managed Instance maakt u [het maken van een beheerd exemplaar](sql-database-managed-instance-get-started.md).
- Zie voor meer informatie over het configureren van een VNet voor een beheerd exemplaar [VNet-configuratie voor beheerde exemplaren](sql-database-managed-instance-vnet-configuration.md)
