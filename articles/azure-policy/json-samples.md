---
title: Voorbeelden van beleidssjablonen
description: JSON-voorbeelden voor Azure Policy
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/17/2018
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 4b9096c1fb0d9ee74849e259a6e0af2486c5d29b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195121"
---
# <a name="templates-for-azure-policy"></a>Sjablonen voor Azure Policy

De volgende tabel bevat links naar voorbeeldsjablonen voor Azure Policy. Deze voorbeelden staan in [de opslagplaats met voorbeelden voor Azure Policy](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**Compute**||
| [Goedgekeurde VM-installatiekopieën](scripts/allowed-custom-images.md) | Zorgt ervoor dat alleen goedgekeurde aangepaste installatiekopieën kunnen worden geïmplementeerd in uw omgeving. U geeft een matrix van goedgekeurde installatiekopie-id’s op. |
| [Controle of VM geen beheerde schijf gebruikt](scripts/create-vm-managed-disk.md) | Controleert of een virtuele machine wordt gemaakt die geen beheerde schijven gebruikt.|
| [Controleert of uitbreiding bestaat](scripts/audit-ext-not-exist.md) | Controleert of er niet een uitbreiding met een virtuele machine wordt geïmplementeerd. U geeft de uitgever van de uitbreiding op en het type om te controleren of deze zijn geïmplementeerd. |
| [Aangepaste VM-installatiekopie van een resourcegroep toestaan](scripts/allow-custom-vm-image.md) |  Zorgt ervoor dat aangepaste installatiekopieën alleen afkomstig kunnen zijn uit een goedgekeurde resourcegroep. U geeft de naam op van de goedgekeurde resourcegroep. |
| [Azure Hybrid Use Benefit weigeren](scripts/deny-hybrid-use.md) | Verbiedt gebruik van Azure Hybrid Use Benefit (AHUB). Gebruik dit beleid wanneer u gebruik van on-premises licenties niet wilt toestaan. |
| [Niet-toegestane VM-extensies](scripts/not-allowed-vm-ext.md) | Verbiedt het gebruik van de opgegeven extensies. U geeft een matrix op met de verboden extensietypen. |
| [Alleen een bepaalde VM-platforminstallatiekopie toestaan](scripts/allow-certain-vm-image.md) | Vereist dat virtuele machines een specifieke versie van UbuntuServer uitvoeren. |
| [Virtuele machine met beheerde schijf maken](scripts/use-managed-disk-vm.md) | Vereist dat virtuele machines beheerde schijven gebruiken.|
|**Controle**||
| [Audit diagnostic setting](scripts/audit-diag-setting.md) (Diagnostische instelling voor controleren) | Controleert of er geen diagnostische instellingen zijn ingeschakeld voor de opgegeven resourcetypen. U geeft een matrix van resourcetypen op waarvoor u wilt controleren of er diagnostische instellingen zijn ingeschakeld. |
|**Naam- en tekstconventies**||
| [Allow multiple name patterns](scripts/allow-multiple-name-patterns.md) (Meerdere naamgevingspatronen toestaan) | Toestaan dat verschillende naamgevingspatronen mogen worden gebruikt voor resources. |
| [Require like pattern](scripts/enforce-like-pattern.md) (Like-patroon vereisen) | Resourcenamen moeten voldoen aan de voorwaarde like voor een patroon. |
| [Require match pattern](scripts/enforce-match-pattern.md) (Match-patroon vereisen) | Resourcenamen moeten overeenkomen met het opgegeven naamgevingspatroon. |
| [Require tag match pattern](scripts/enforce-tag-match-pattern.md) (Tag-match-patroon vereisen) | Een tagwaarde moet overeenkomen met een tekstpatroon. |
|**Netwerk**||
| [Toegestane Application Gateway-SKU’s](scripts/allowed-app-gate-sku.md) | Zorgt ervoor dat toepassingsgateways een goedgekeurde SKU gebruiken. U geeft een matrix van goedgekeurde SKU’s op. |
| [Controleren of Network Watcher is ingeschakeld voor de regio](scripts/net-watch-not-enabled.md) | Controleert of Network Watcher is ingeschakeld voor een bepaalde regio. U geeft de naam op van de regio waarin u wilt controleren of Network Watcher is ingeschakeld. |
| [NSG X on every NIC](scripts/nsg-on-nic.md) (NSG X op elke NIC) | Vereist dat een specifieke netwerkbeveiligingsgroep wordt gebruikt met elke interface van een virtueel netwerk. U geeft de id op van de netwerkbeveiligingsgroep die u wilt gebruiken. |
| [NSG X on every subnet](scripts/nsg-on-subnet.md) (NSG X in elk subnet) | Vereist dat een specifieke netwerkbeveiligingsgroep wordt gebruikt met elk virtueel subnet. U geeft de id op van de netwerkbeveiligingsgroep die u wilt gebruiken. |
| [Toegestane Express Route-bandbreedten](scripts/allowed-er-band.md) | Zorgt ervoor dat Express Routes een goedgekeurde set bandbreedten gebruikt. U geeft een matrix op van bandbreedten die kunnen worden opgegeven voor Express Route. |
| [Toegestane peeringlocatie voor Express Route](scripts/allowed-peering-er.md) | Zorgt ervoor dat Express Routes opgegeven peeringlocaties gebruikt. U geeft een matrix op van toegestane peeringlocaties. |
| [Toegestane Express Route-SKU's](scripts/allowed-er-skus.md) | Zorgt ervoor dat Express Routes een goedgekeurde SKU gebruikt. U geeft een matrix op van toegestane SKU's. |
| [Toegestane load balancer-SKU's](scripts/allowed-lb-skus.md) | Zorgt ervoor dat load balancers een goedgekeurde SKU gebruiken. U geeft een matrix op van toegestane SKU's. |
| [Geen netwerkpeering naar ER-netwerk](scripts/no-peering-er-net.md) | Voorkomt de koppeling van een netwerkpeering aan een netwerk in een opgegeven brongroep. Gebruiken om de verbinding met centraal beheerde netwerkinfrastructuur te voorkomen. U geeft de naam op van de brongroep die niet gekoppeld mag worden. |
| [Geen door de gebruiker gedefinieerde routetabellen](scripts/no-user-def-route-table.md)  |Voorkomt dat virtuele netwerken worden geïmplementeerd met een door de gebruiker gedefinieerde routetabel. |
| [Toegestane gateway-SKU’s van virtueel netwerk](scripts/allowed-vn-gate-sku.md) | Zorgt ervoor dat gateways van virtuele netwerken een goedgekeurde SKU en gateway-type gebruiken. U geeft een matrix op van goedgekeurde SKU's en een matrix van goedgekeurde gateway-typen. |
| [Use approved subnet for VM network interfaces](scripts/use-approved-subnet-vm-nics.md) (Goedgekeurd subnet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd subnet gebruiken. U geeft de id op van het goedgekeurde subnet. |
| [Use approved vNet for VM network interfaces](scripts/use-approved-vnet-vm-nics.md) (Goedgekeurd vNet gebruiken voor VM-netwerkinterfaces) | Vereist dat netwerkinterfaces een goedgekeurd virtueel netwerk gebruiken. U geeft de id op van het goedgekeurde virtuele netwerk. |
|**Tags**||
| [Billing Tags Policy Initiative](scripts/billing-tags-policy-init.md) (Beleidsinitiatief voor factureringstags) | Vereist opgegeven tagwaarden voor kostenplaats en productnaam. Er wordt gebruikgemaakt van ingebouwde beleidsregels om vereiste tags toe te passen en af te dwingen. U geeft de vereiste waarden voor de tags op.  |
| [Enforce tag and its value on resource groups](scripts/enforce-tag-rg.md) (Tag met de bijbehorende waarde afdwingen voor resourcegroepen) | Vereist een tag en waarde voor een resourcegroep. U geeft de vereiste naam en waarde voor de tag op.  |
|**SQL**||
| [Auditinstellingsniveau SQL DB controleren](scripts/audit-sql-db-audit-setting.md) | Controleert of het auditinstellingsniveau van de SQL-database overeenkomt met een opgegeven waarde. U geeft een waarde op die aangeeft of de auditinstellingen moeten worden ingeschakeld of uitgeschakeld.  |
| [Controlestatus van transparante gegevensversleuteling](scripts/audit-trans-data-enc-status.md) | Controleert of transparante gegevensversleuteling voor SQL-database is ingeschakeld.  |
| [Instelling dreigingsdetectieniveau DB controleren](scripts/audit-db-threat-det-setting.md) | Controleert of het beveiligingsmeldingenbeleid van SQL-databases is ingesteld op de specifieke status. U geeft een waarde op die aangeeft of de detectie van dreigingen is ingeschakeld of uitgeschakeld.  |
| [Auditinstellingsniveau SQL Server controleren](scripts/audit-sql-ser-leve-audit-setting.md) | Controleert of het auditinstellingsniveau van de SQL Server overeenkomt met een opgegeven waarde. U geeft een waarde op die aangeeft of de auditinstellingen moeten worden ingeschakeld of uitgeschakeld. |
| [Instelling dreigingsdetectieniveau server controleren](scripts/audit-sql-ser-threat-det-setting.md) | Controleert of het beveiligingsmeldingenbeleid van SQL-databases is ingesteld op de specifieke status. U geeft een waarde op die aangeeft of de detectie van dreigingen is ingeschakeld of uitgeschakeld.  |
| [Op een Azure Active Directory-beheerder controleren](scripts/audit-no-aad-admin.md) | Controleert of er een Azure Active Directory-beheerder is toegewezen aan de SQL Server. |
| [Toegestane SQL DB-SKU's](scripts/allowed-sql-db-skus.md) | Zorgt ervoor dat SQL-databases een goedgekeurde SKU gebruiken. U geeft een matrix op van toegestane SKU-id's of een matrix van toegestane SKU-namen. |
|**Storage**||
| [Toegestane SKU's voor opslagaccounts en virtuele machines](scripts/allowed-skus-storage.md) | Zorgt ervoor dat opslagaccounts en virtuele machines gebruikmaken van goedgekeurde SKU's. Gebruik ingebouwde beleidsregels om verzekerd te zijn van goedgekeurde SKU's. U geeft een reeks goedgekeurde SKU's voor virtuele machines op en een reeks goedgekeurde SKU's voor opslagaccounts. |
| [Https-verkeer voor opslagaccounts vereisen](scripts/ensure-https-stor-acct.md) | Zorgt ervoor dat opslagaccounts HTTPS-verkeer gebruiken.  |
| [Statische toegangslaag voor opslagaccounts weigeren](scripts/deny-cool-access-tiering.md) | Voorkomt het gebruik van statische toegangslagen voor blob-opslagaccounts.  |
| [Opslagbestandsversleuteling vereisen](scripts/ensure-store-file-enc.md) | Zorgt ervoor dat bestandsversleuteling is ingeschakeld voor opslagaccounts.  |
|**Ingebouwd beleid**||
| [Allowed locations](scripts/allowed-locs.md) (Toegestane locaties) | Vereist dat alle resources worden geïmplementeerd naar de goedgekeurde locaties. U geeft een matrix van goedgekeurde locaties op.  |
| [Allowed resource types](scripts/allowed-res-types.md) (Toegestane resourcetypen) | Zorgt ervoor dat alleen goedgekeurde resourcetypen worden geïmplementeerd. U geeft een matrix met resourcetypen op die zijn toegestaan.  |
| [Toegestane opslagaccount-SKU's](scripts/allowed-stor-acct-skus.md) | Zorgt ervoor dat opslagaccounts een goedgekeurde SKU gebruiken. U geeft een matrix van goedgekeurde SKU’s op. |
| [Apply tag and its default value](scripts/apply-tag-def-val.md) (Tag met standaardwaarde toepassen) | Hiermee voegt u de opgegeven naam en waarde voor een tag toe, als die tag niet is opgegeven. U geeft de tagnaam en -waarde op die moet worden toegepast.  |
| [Versleuteling van SQL-database controleren](scripts/sql-database-encryption-audit.md) | Controleert of transparante gegevensversleuteling voor SQL-database is ingeschakeld. |
| [Auditinstellingen SQL Server controleren](scripts/sql-server-audit.md) | Controleert SQL Server op basis van of de auditinstellingen zijn ingeschakeld. |
| [Data Lake Store-versleuteling vereisen](scripts/enforce-datalakestore-encryption.md) | Weigert alle Data Lake Store-accounts waarvoor geen versleuteling is ingeschakeld. |
| [Enforce tag and its value](scripts/enforce-tag-val.md) (Tag en waarde afdwingen) | Vereist een opgegeven naam en waarde voor de tag. U geeft de tagnaam en -waarde op die moet worden afgedwongen.  |
| [Not allowed resource types](scripts/not-allowed-res-type.md) (Niet-toegestane resourcetypen) | Voorkomt de implementatie van de opgegeven resourcetypen. U geeft een matrix met brontypen op die niet zijn toegestaan.  |
| [SQL Server-versie 12.0 vereisen](scripts/req-sql-12.md) | Vereist dat SQL Server-versie 12.0 wordt gebruikt.  |
| [Opslagaccountversleuteling vereisen](scripts/req-store-acct-enc.md) | Zorgt ervoor dat het opslagaccount blob-versleuteling gebruikt.  |
