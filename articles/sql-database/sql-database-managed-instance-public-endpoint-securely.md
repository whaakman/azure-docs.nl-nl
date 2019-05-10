---
title: Beheerd exemplaar van openbare eindpunten - Azure SQL-Database beveiligen beheerd exemplaar | Microsoft Docs
description: Veilig gebruik van openbare eindpunten in Azure met een beheerd exemplaar
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 05/08/2019
ms.openlocfilehash: f06677b66c8f6586fec8cc5dfe97b1515b741e9c
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65470281"
---
# <a name="use-an-azure-sql-database-managed-instance-securely-with-public-endpoints"></a>Veilig gebruik van een beheerd exemplaar voor Azure SQL Database met openbare eindpunten

Azure SQL Database beheerde exemplaren u een gebruiker verbinding via krijgt [openbare eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md). In dit artikel wordt uitgelegd hoe u deze configuratie beter te beveiligen.

## <a name="scenarios"></a>Scenario's

Een beheerd exemplaar van SQL Database biedt een persoonlijke eindpunt om toe te staan de connectiviteit van binnen het virtuele netwerk. Er is de standaardoptie om maximale isolatie te bieden. Er zijn echter scenario's waar u nodig hebt voor een openbaar eindpunt verbinding:

- Het beheerde exemplaar moet integreren met meerdere-tenant alleen-platform-as-a-service (PaaS)-aanbiedingen.
- U moet een hogere doorvoer van het uitwisselen van gegevens dan mogelijk is wanneer u een VPN-verbinding.
- Bedrijfsbeleid verbieden PaaS binnen bedrijfsnetwerken.

## <a name="deploy-a-managed-instance-for-public-endpoint-access"></a>Een beheerd exemplaar voor openbaar eindpunt toegang implementeren

Hoewel niet verplicht is, wordt het algemene implementatiemodel voor een beheerd exemplaar met een openbaar eindpunt toegang is het maken van het exemplaar in een speciale geïsoleerd virtual network. In deze configuratie, wordt het virtuele netwerk alleen gebruikt voor isolatie virtueel cluster. Het maakt niet uit als het beheerde exemplaar IP-adresruimte met een bedrijfsnetwerk IP-adresruimte overlapt.

## <a name="secure-data-in-motion"></a>Bescherm uw gegevens in beweging

Beheerd exemplaar gegevensverkeer worden altijd versleuteld als versleuteling biedt ondersteuning voor het clientstuurprogramma. Gegevens die worden verzonden tussen het beheerde exemplaar en andere virtuele machines van Azure of Azure-services blijven nooit van Azure-backbone. Als er een verbinding tussen het beheerde exemplaar en een on-premises netwerk is, raden wij dat u Azure ExpressRoute gebruiken met Microsoft-peering. ExpressRoute kunt u voorkomen verplaatsen van gegevens via het openbare internet. Voor beheerd exemplaar particuliere verbindingen, kan alleen persoonlijke peering worden gebruikt.

## <a name="lock-down-inbound-and-outbound-connectivity"></a>Binnenkomende en uitgaande connectiviteit vergrendelen

Het volgende diagram toont de aanbevolen beveiligingsconfiguraties:

![Beveiligingsconfiguraties voor binnenkomende en uitgaande connectiviteit te vergrendelen](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

Een beheerd exemplaar heeft een [specifiek adres openbaar eindpunt](sql-database-managed-instance-find-management-endpoint-ip-address.md). In de uitgaande client-side-firewall en in de regels voor netwerkbeveiligingsgroepen, stelt u dit openbare eindpunt-IP-adres aan de uitgaande connectiviteit beperken.

Om te controleren of verkeer naar het beheerde exemplaar is afkomstig van betrouwbare bronnen, wordt u aangeraden verbinding te maken van bronnen met bekende IP-adressen. Een netwerkbeveiligingsgroep gebruiken om te beperken van toegang tot het beheerde exemplaar openbare eindpunt op poort 3342.

Wanneer u clients wilt initiëren een verbinding van een on-premises netwerk, zorg er dan voor dat het oorspronkelijke adres wordt omgezet naar een bekende set IP-adressen. Als u geen (bijvoorbeeld mobiele werknemers wordt een typisch scenario), raden wij aan u [punt-naar-site VPN-verbindingen en een persoonlijke eindpunt](sql-database-managed-instance-configure-p2s.md).

Als verbindingen van Azure worden gestart, raden wij aan dat verkeer afkomstig zijn van een bekende toegewezen [virtuele IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md) (bijvoorbeeld een virtuele machine). Als u wilt beheren van virtuele IP-adres (VIP)-adressen gemakkelijker, wilt u mogelijk gebruiken [openbare IP-adresvoorvoegsels](../virtual-network/public-ip-address-prefix.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het configureren van openbare eindpunt voor de exemplaren beheren: [Openbare eindpunt configureren](sql-database-managed-instance-public-endpoint-configure.md)
