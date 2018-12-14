---
title: Azure SQL Database Managed Instance aangepaste DNS | Microsoft Docs
description: Dit onderwerp beschrijft de configuratie-opties voor een aangepaste DNS-server met een Azure SQL Database Managed Instance.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/23/2018
ms.openlocfilehash: 7ad3b81b792b37d2c3667dd554d41319a5727045
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53336406"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Een aangepaste DNS configureren voor Azure SQL Database Managed Instance

Een Azure SQL Database Managed Instance moet worden geïmplementeerd in een Azure [virtueel netwerk (VNet)](../virtual-network/virtual-networks-overview.md). Er zijn enkele scenario's (bijvoorbeeld db e-mail, gekoppelde servers naar andere SQL-exemplaren in uw omgeving cloud of hybride) waarvoor persoonlijke hostnamen worden omgezet in het beheerde exemplaar. In dit geval moet u een aangepaste DNS-server in Azure configureren. Omdat het beheerde exemplaar van de dezelfde DNS-server voor de interne werking gebruikt, moet de DNS-configuratie van het virtuele netwerk compatibel zijn met Managed Instance. 

   > [!IMPORTANT]
   > Gebruik de volledig gekwalificeerde domeinnamen (FQDN) voor de e-mailservers, SQL-Servers en andere services altijd, zelfs als ze binnen uw privé-DNS-zone. Gebruik bijvoorbeeld `smtp.contoso.com` voor e-mailserver omdat eenvoudige `smtp` worden niet correct opgelost.

Als u wilt maken van een aangepaste DNS-configuratie is compatibel met het beheerde exemplaar, moet u: 
- Aangepaste DNS-server configureren zodat deze kunnen openbare domeinnamen omzetten 
- Azure recursieve naamomzetting DNS IP-adres 168.63.129.16 aan het einde van het virtuele netwerk DNS-lijst plaatsen 
 
## <a name="setting-up-custom-dns-servers-configuration"></a>Instellen van aangepaste DNS-servers configureren

1. Zoeken in de Azure-portal, aangepaste DNS-optie voor uw VNet.

   ![aangepaste DNS-optie](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Schakel over naar aangepaste en voer uw aangepaste DNS-server IP-adres, evenals Azure recursieve resolvers IP-adres 168.63.129.16. 

   ![aangepaste DNS-optie](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Instelling van Azure recursieve naamomzetting in de lijst met DNS kan leiden tot het beheerde exemplaar in te voeren een beschadigde status waarbij de aangepaste DNS-servers om een bepaalde reden niet beschikbaar zijn. Herstellen vanaf dat staat mogelijk moet u nieuwe instantie in een VNet aan het nalevingsbeleid voor netwerken, serviceniveau-Instantiegegevens maken en herstellen van uw databases. Instelling van de Azure recursieve naamomzetting omdat de laatste vermelding in de lijst met DNS-zorgt ervoor dat, zelfs als alle aangepaste DNS-servers mislukt, kunnen openbare namen nog steeds worden opgelost.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar](sql-database-managed-instance.md)
- Zie voor een zelfstudie wordt uitgelegd hoe u een nieuwe Managed Instance maakt u [het maken van een beheerd exemplaar](sql-database-managed-instance-get-started.md).
- Zie voor meer informatie over het configureren van een VNet voor een beheerd exemplaar [VNet-configuratie voor beheerde exemplaren](sql-database-managed-instance-connectivity-architecture.md)
