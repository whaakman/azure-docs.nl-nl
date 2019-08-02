---
title: Aangepaste DNS voor Azure SQL Database Managed instance | Microsoft Docs
description: In dit onderwerp worden configuratie opties beschreven voor een aangepaste DNS met een Azure SQL Database beheerd exemplaar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 07/17/2019
ms.openlocfilehash: 588fac1fc48396584188eec44f21a7005dc8ed96
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567557"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Een aangepaste DNS configureren voor Azure SQL Database Managed instance

Een Azure SQL Database beheerd exemplaar moet worden geïmplementeerd in een [virtueel Azure-netwerk (VNet)](../virtual-network/virtual-networks-overview.md). Er zijn enkele scenario's (bijvoorbeeld DB mail, gekoppelde servers naar andere SQL-exemplaren in uw Cloud of hybride omgeving) waarvoor de namen van particuliere hosts moeten worden omgezet van het beheerde exemplaar. In dit geval moet u een aangepaste DNS-server in azure configureren. 

Omdat voor het beheerde exemplaar dezelfde DNS wordt gebruikt voor de interne werk, moet u de aangepaste DNS-server zo configureren dat open bare domein namen kunnen worden omgezet.

   > [!IMPORTANT]
   > Gebruik altijd volledig gekwalificeerde domein namen (FQDN) voor de e-mail servers, SQL-servers en andere services, zelfs als ze zich in uw privé-DNS-zone bevinden. U kunt bijvoorbeeld `smtp.contoso.com` gebruiken voor de e- `smtp` mail server, omdat eenvoudig niet goed kan worden opgelost.

   > [!IMPORTANT]
   > Het bijwerken van de DNS-servers van het virtuele netwerk heeft geen invloed op het beheerde exemplaar direct. De DNS-configuratie van het beheerde exemplaar wordt bijgewerkt nadat de DHCP-lease verloopt of nadat het platform upgarade is, wat het eerst komt. **Gebruikers wordt aangeraden hun DNS-configuratie voor het virtuele netwerk in te stellen voordat ze hun eerste beheerde exemplaar maken.**

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is een beheerd exemplaar](sql-database-managed-instance.md) ? voor een overzicht.
- Zie [een beheerd exemplaar maken](sql-database-managed-instance-get-started.md)voor een zelf studie waarin wordt getoond hoe u een nieuw beheerd exemplaar maakt.
- Zie [vnet-configuratie voor beheerde instanties](sql-database-managed-instance-connectivity-architecture.md) voor meer informatie over het configureren van een vnet voor een beheerd exemplaar
