---
title: Azure SQL-Database beheerd exemplaar aangepaste DNS | Microsoft Docs
description: Dit onderwerp beschrijft de configuratie-opties voor een aangepaste DNS-server met een Azure SQL Database beheerd exemplaar.
services: sql-database
author: srdjan-bozovic
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 993ff518a1987581ad7de4797611f104080c2c2f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Exemplaar van een aangepaste DNS configureren voor Azure SQL-Database beheerd

Een Azure SQL Database beheerd-instantie (preview) moet worden geïmplementeerd in een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md). Er zijn enkele scenario's, gekoppelde servers naar andere SQL-exemplaren in uw omgeving cloud of hybride, waarvoor persoonlijke hostnamen worden omgezet in het exemplaar worden beheerd. In dit geval moet u een aangepaste DNS-server in Azure te configureren. Omdat de dezelfde DNS-server beheerd exemplaar voor de interne werking gebruikt, moet de DNS-configuratie van virtueel netwerk compatibel zijn met beheerde-exemplaar. 

Een aangepaste DNS-configuratie om compatibel te maken met beheerde exemplaar, moet u de volgende stappen uit: 
- Aangepaste DNS voor het doorsturen van aanvragen naar Azure DNS configureren 
- Het aangepaste DNS als primaire en de Azure DNS als secundaire ingesteld voor het VNet 
- De aangepaste DNS als primaire en de Azure DNS registreren als secundaire

## <a name="configure-custom-dns-to-forward-requests-to-azure-dns"></a>Aangepaste DNS voor het doorsturen van aanvragen naar Azure DNS configureren 

Voor het configureren van DNS-doorsturen op Windows Server 2016, volg deze stappen: 

1. In **Serverbeheer**, klikt u op **extra**, en klik vervolgens op **DNS**. 

   ![DNS](./media/sql-database-managed-instance-custom-dns/dns.png) 

2. Dubbelklik op **doorstuurservers**.

   ![Doorstuurservers](./media/sql-database-managed-instance-custom-dns/forwarders.png) 

3. Klik op **Bewerken**. 

   ![Lijst met doorstuurservers](./media/sql-database-managed-instance-custom-dns/forwarders-list.png) 

4. Voer in Azure recursieve resolvers IP-adres, zoals 168.63.129.16.

   ![Recursieve resolvers IP-adres](./media/sql-database-managed-instance-custom-dns/recursive-resolvers-ip-address.png) 
 
## <a name="set-up-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Aangepaste DNS als primaire en Azure DNS als secundaire instellen 
 
DNS-configuratie op een Azure-VNet vereist dat u IP-adressen opgeeft, configureren zodat de Azure-virtuele machine die als host fungeert voor de DNS-server met een statisch IP-adres met behulp van de volgende stappen uitvoeren: 

1. Open in de Azure-portal, de aangepaste DNS VM-netwerkinterface.

   ![network-interface](./media/sql-database-managed-instance-custom-dns/network-interface.png) 

2. In de sectie IP-configuraties. Selecteer IP-configuratie 

   ![IP-configuratie](./media/sql-database-managed-instance-custom-dns/ip-configuration.png) 


3. Privé IP-adres instellen als Static. Noteer de IP-adres (10.0.1.5 op deze schermafbeelding) 

   ![statisch](./media/sql-database-managed-instance-custom-dns/static.png) 


## <a name="register-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Aangepaste DNS als primaire en Azure DNS als secundaire registreren 

1. Zoek in de Azure-portal aangepaste DNS-optie voor uw VNet.

   ![aangepaste DNS-optie](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Overschakelen naar aangepaste en voer uw aangepaste IP-adres van de DNS-server, evenals Azure recursieve resolvers IP-adres, zoals 168.63.129.16. 

   ![aangepaste DNS-optie](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Lijst van Azure recursieve resolver niet instellen in DNS zorgt ervoor dat het exemplaar beheerd slechte staat invoeren. Herstellen van uit dat de status moet u mogelijk maken van nieuwe instantie in een VNet aan het nalevingsbeleid voor netwerken, serviceniveau-Instantiegegevens maken en herstellen van uw databases. Zie [VNet-configuratie](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is er een exemplaar beheerd](sql-database-managed-instance.md)
- Zie voor een voorbeeld van het maken van een VNet en maak vervolgens een exemplaar beheerd [het maken van een exemplaar beheerd](sql-database-managed-instance-tutorial-portal.md).
- Zie voor het configureren van het VNet [VNet-configuratie voor beheerde exemplaren](sql-database-managed-instance-vnet-configuration.md)
