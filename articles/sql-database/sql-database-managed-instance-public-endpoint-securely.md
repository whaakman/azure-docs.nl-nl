---
title: Beheerd exemplaar van openbare eindpunten - Azure SQL-Database beveiligen beheerd exemplaar | Microsoft Docs
description: Veilig gebruik van openbare eindpunten in Azure met beheerd exemplaar
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 04/16/2019
ms.openlocfilehash: 9d5a3d18e8a7d3c5a6cb08e16e74dd4fbda9ca31
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013623"
---
# <a name="using-azure-sql-database-managed-instance-securely-with-public-endpoint"></a>Met behulp van Azure SQL-Database beheerd exemplaar veilig met een openbaar eindpunt

Azure SQL-Database beheerd exemplaar kan worden ingeschakeld voor gebruiker connectiviteit via [openbaar eindpunt](../virtual-network/virtual-network-service-endpoints-overview.md). Dit artikel bevat richtlijnen over hoe u deze configuratie veiliger.

## <a name="scenarios"></a>Scenario's

Beheerd exemplaar biedt persoonlijke eindpunt voor de connectiviteit van binnen het virtuele netwerk. Er is de standaardoptie om maximale isolatie te bieden. Er zijn echter scenario's waarbij een openbaar eindpunt verbinding nodig is:

- Integratie met meerdere tenants alleen PaaS-aanbiedingen.
- Hogere doorvoer van het uitwisselen van gegevens dan het gebruik van VPN.
- Bedrijfsbeleid verbieden PaaS binnen bedrijfsnetwerken.

## <a name="deploying-managed-instance-for-public-endpoint-access"></a>Beheerd exemplaar voor openbare eindpunt toegang implementeren

Hoewel niet verplicht is, wordt het algemene implementatiemodel voor een beheerd exemplaar met een openbaar eindpunt toegang is het maken van het exemplaar in een speciale geïsoleerd virtual network. In deze configuratie wordt het virtuele netwerk alleen voor virtuele cluster isolatie gebruikt. Het is niet relevant zijn als het beheerde exemplaar IP-adresruimte met een bedrijfsnetwerk IP-adresruimte overlapt.

## <a name="securing-data-in-motion"></a>Het beveiligen van gegevens in beweging

Beheerd exemplaar gegevensverkeer worden altijd versleuteld als versleuteling biedt ondersteuning voor het clientstuurprogramma. Gegevens tussen het beheerde exemplaar en andere virtuele Machines van Azure of Azure-services blijven nooit Azure backbone-netwerk binnenkomt. Als er een beheerd exemplaar van een on-premises netwerkverbinding, is het raadzaam het gebruik van Express Route met het Microsoft-peering. Express Route helpt te voorkomen dat verplaatsen van gegevens via het openbare Internet (voor beheerd exemplaar particuliere verbindingen, alleen persoonlijke peering kan worden gebruikt).

## <a name="locking-down-inbound-and-outbound-connectivity"></a>Vergrendelen van binnenkomende en uitgaande verbindingen

Het volgende diagram toont aanbevolen beveiligingsconfiguraties.

![managed-instance-vnet.png](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

Beheerd exemplaar heeft een [specifiek adres openbaar eindpunt](sql-database-managed-instance-find-management-endpoint-ip-address.md). Dit IP-adres moet worden ingesteld in de client-side uitgaande firewall en Network Security Group regels op de uitgaande connectiviteit beperken.

Het is raadzaam om te controleren of verkeer naar het beheerde exemplaar is afkomstig van betrouwbare bronnen, verbinding maken tussen bronnen met bekende IP-adressen. De toegang tot het beheerde exemplaar openbare eindpunt op poort 3342 beperken met behulp van een Netwerkbeveiligingsgroep.

Wanneer u clients wilt initiëren een verbinding van een on-premises netwerk, zorg er dan voor dat het oorspronkelijke adres wordt omgezet naar een bekende set IP-adressen. Als dat niet kan worden bereikt (bijvoorbeeld mobiele medewerkers wordt een typisch scenario), het is raadzaam om te gebruiken [punt-naar-site VPN-verbindingen en een persoonlijke eindpunt](sql-database-managed-instance-configure-p2s.md).

Als de verbindingen van Azure worden gestart, wordt aangeraden dat verkeer afkomstig zijn van bekende toegewezen [VIP](../virtual-network/virtual-networks-reserved-public-ip.md) (bijvoorbeeld virtuele Machines). Gemakkelijker te maken van het VIP-adressen beheren met behulp van klanten overwegen [openbare IP-adresvoorvoegsel](../virtual-network/public-ip-address-prefix.md).