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
ms.openlocfilehash: d5bb2f2f4b79c4b03e631fc844a712f76fc69109
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258164"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Een aangepaste DNS configureren voor Azure SQL Database Managed Instance

Een Azure SQL Database Managed Instance (preview) moet worden geïmplementeerd in een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md). Er zijn enkele scenario's, gekoppelde servers naar andere SQL-exemplaren in uw cloud of hybride omgeving, die persoonlijke hostnamen worden omgezet in het beheerde exemplaar vereisen. In dit geval moet u een aangepaste DNS-server in Azure configureren. Omdat het beheerde exemplaar van de dezelfde DNS-server voor de interne werking gebruikt, moet de DNS-configuratie van het virtuele netwerk compatibel zijn met Managed Instance. 

Een aangepaste DNS-configuratie om compatibel te maken met Managed Instance, moet u de volgende stappen uit: 
- Configureren van aangepaste DNS voor het doorsturen van aanvragen naar Azure DNS 
- De aangepaste DNS als primaire en de Azure DNS als secundaire instellen voor het VNet 
- Meld u aan de aangepaste DNS als primaire en de Azure DNS als secundaire

## <a name="configure-custom-dns-to-forward-requests-to-azure-dns"></a>Configureren van aangepaste DNS voor het doorsturen van aanvragen naar Azure DNS 

Gebruik deze stappen voor het doorsturen van de DNS configureren op Windows Server 2016: 

1. In **Serverbeheer**, klikt u op **extra**, en klik vervolgens op **DNS**. 

   ![DNS](./media/sql-database-managed-instance-custom-dns/dns.png) 

2. Dubbelklik op **doorstuurservers**.

   ![Doorstuurservers](./media/sql-database-managed-instance-custom-dns/forwarders.png) 

3. Klik op **Bewerken**. 

   ![Lijst met doorstuurservers](./media/sql-database-managed-instance-custom-dns/forwarders-list.png) 

4. Voer Azure recursieve resolvers IP-adres, zoals 168.63.129.16.

   ![Recursieve resolvers ip-adres](./media/sql-database-managed-instance-custom-dns/recursive-resolvers-ip-address.png) 
 
## <a name="set-up-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Aangepaste DNS als primaire en Azure DNS als secundaire instellen 
 
DNS-configuratie op een Azure-VNet vereist dat u IP-adressen, dus configureren van de Azure-virtuele machine die als host fungeert voor de DNS-server met een statisch IP-adres met behulp van de volgende stappen uitvoeren: 

1. Open in de Azure-portal, de aangepaste DNS-VM-netwerkinterface.

   ![de netwerkinterface](./media/sql-database-managed-instance-custom-dns/network-interface.png) 

2. In de sectie IP-configuraties. Selecteer IP-configuratie 

   ![IP-configuratie](./media/sql-database-managed-instance-custom-dns/ip-configuration.png) 


3. Privé IP-adres instellen als statisch. Noteer het IP-adres (10.0.1.5 op deze schermafbeelding) 

   ![statisch](./media/sql-database-managed-instance-custom-dns/static.png) 


## <a name="register-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Aangepaste DNS als primaire en Azure DNS registreren als secundaire 

1. Zoeken in de Azure-portal, aangepaste DNS-optie voor uw VNet.

   ![aangepaste DNS-optie](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Schakel over naar aangepaste en voer uw aangepaste DNS-server IP-adres, evenals Azure recursieve resolvers IP-adres, zoals 168.63.129.16. 

   ![aangepaste DNS-optie](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Lijst met niet instellen van Azure recursieve naamomzetting in DNS zorgt ervoor dat het beheerde exemplaar in te voeren van slechte staat. Herstellen vanaf dat staat mogelijk moet u nieuwe instantie in een VNet aan het nalevingsbeleid voor netwerken, serviceniveau-Instantiegegevens maken en herstellen van uw databases. Zie [VNet-configuratie](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar](sql-database-managed-instance.md)
- Zie voor een zelfstudie wordt uitgelegd hoe u een nieuwe Managed Instance maakt u [het maken van een beheerd exemplaar](sql-database-managed-instance-create-tutorial-portal.md).
- Zie voor meer informatie over het configureren van een VNet voor een beheerd exemplaar [VNet-configuratie voor beheerde exemplaren](sql-database-managed-instance-vnet-configuration.md)
