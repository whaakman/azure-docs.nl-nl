---
title: Voorbeelden van beleid-sjabloon | Microsoft Docs
description: JSON-voorbeelden voor Azure-beleid
services: azure-policy
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
tags: 
ms.assetid: 
ms.service: azure-policy
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 11/13/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: 875c8c87f62ecf0e7459dfcec22089854ad95594
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="templates-for-azure-policy"></a>Sjablonen voor Azure-beleid

De volgende tabel bevat koppelingen naar json-sjablonen voor Azure-beleid. Deze voorbeelden vindt u in de [Azure beleid voorbeelden opslagplaats](https://github.com/Azure/azure-policy).

| | |
|---|---|
|**Compute**||
| [Goedgekeurde VM-installatiekopieën](scripts/allowed-custom-images.md) | Vereist dat alleen goedgekeurde aangepaste installatiekopieën in uw omgeving zijn geïmplementeerd. U opgeven een matrix van goedgekeurde afbeelding id's. |
| [Audit wanneer VM niet beheerd schijf](scripts/create-vm-managed-disk.md) | Audits wanneer een virtuele machine die wordt gemaakt, gebruikt geen beheerde schijven.|
| [Audit als uitbreiding niet bestaat](scripts/audit-ext-not-exist.md) | Audits als een uitbreiding niet met een virtuele machine wordt geïmplementeerd. Geeft u de uitgever van de extensie en het type om te controleren of deze is geïmplementeerd. |
| [Aangepaste VM-installatiekopie van een resourcegroep toestaan](scripts/allow-custom-vm-image.md) |  Vereist dat aangepaste installatiekopieën afkomstig van een goedgekeurde resourcegroep zijn. U opgeven de naam van de goedgekeurde resourcegroep. |
| [Hybride gebruik voordeel weigeren](scripts/deny-hybrid-use.md) | Verbiedt gebruik van Azure hybride gebruiken voordeel (AHUB). Gebruik deze optie wanneer u niet wilt toestaan van gebruik van on-premises-licenties. |
| [VM-extensies toegestaan niet](scripts/not-allowed-vm-ext.md) | Het gebruik van de opgegeven extensies verbiedt. U opgeven een matrix met de extensietypen niet-toegestane. |
| [Alleen een bepaalde VM platforminstallatiekopie toestaan](scripts/allow-certain-vm-image.md) | Moeten virtuele machines met een specifieke versie van UbuntuServer. |
| [Virtuele machine maken met beheerde-schijf](scripts/use-managed-disk-vm.md) | Vereist dat virtuele machines gebruikmaken van beheerde schijven.|
|**Bewaking**||
| [De diagnostische instelling audit](scripts/audit-diag-setting.md) | Audits als diagnostische instellingen niet ingeschakeld voor brontypen die is opgegeven. U een matrix van resourcetypen om te controleren of de diagnostische instellingen zijn ingeschakeld. |
|**Naam en de tekst conventies**||
| [Meerdere bestandsnaampatronen toestaan](scripts/allow-multiple-name-patterns.md) | Toestaan dat een groot aantal bestandsnaampatronen moet worden gebruikt voor resources. |
| [Vereisen zoals patroon](scripts/enforce-like-pattern.md) | Zorg ervoor dat resourcenamen voldoen aan de voorwaarde like voor een patroon. |
| [Vereisen-matchpatroon](scripts/enforce-match-pattern.md) | Zorg ervoor dat de resourcenamen overeenkomen met het naamgevingspatroon. |
| [Vereist label-matchpatroon](scripts/enforce-tag-match-pattern.md) | Zorg ervoor dat een tagwaarde overeenkomt met een tekstpatroon. |
|**Netwerk**||
| [Application Gateway-SKU's toegestaan](scripts/allowed-app-gate-sku.md) | Moet Toepassingsgateways gebruiken een goedgekeurde SKU. U opgeven een matrix van goedgekeurde SKU's. |
| [Audit als netwerk-Watcher niet is ingeschakeld voor de regio](scripts/net-watch-not-enabled.md) | Audits als netwerk-watcher niet is ingeschakeld voor een opgegeven regio. U de naam van de regio waarin u wilt controleren of de netwerk-watcher is ingeschakeld. |
| [NSG X op elke NIC](scripts/nsg-on-nic.md) | Vereist dat een specifieke netwerkbeveiligingsgroep wordt gebruikt voor elke virtuele netwerkinterface. U de ID van de netwerkbeveiligingsgroep te gebruiken. |
| [NSG X in elk subnet](scripts/nsg-on-subnet.md) | Vereist dat een specifieke netwerkbeveiligingsgroep voor elk virtueel subnet wordt gebruikt. U de ID van de netwerkbeveiligingsgroep te gebruiken. |
| [Toegestane Express Route-bandbreedte](scripts/allowed-er-band.md) | Moeten snelle routes met een opgegeven set met bandbreedten. U opgeven een matrix van SKU's die kunnen worden opgegeven voor Express Route. |
| [Peeringlocatie voor Express Route toegestaan](scripts/allowed-peering-er.md) | Alleen een Express Routes gebruik peeringlocaties opgegeven. U opgeven een matrix van toegestane peeringlocaties. |
| [Toegestane Express Route-SKU 's](scripts/allowed-er-skus.md) | Moet Routes Express gebruiken een goedgekeurde SKU. U opgeven een matrix van toegestane SKU's. |
| [SKU's van Load Balancer toegestaan](scripts/allowed-lb-skus.md) | Moet netwerktaakverdelers gebruiken een goedgekeurde SKU. U opgeven een matrix van toegestane SKU's. |
| [Geen netwerk met ER netwerk-peering](scripts/no-peering-er-net.md) | Voorkomt dat een netwerk-peering van wordt gekoppeld aan een netwerk in een opgegeven resourcegroep. Gebruiken om te voorkomen dat de verbinding met de centraal beheerde netwerkinfrastructuur. U opgeven de naam van de resourcegroep om te voorkomen dat de koppeling. |
| [Er is geen gebruiker gedefinieerde Route-tabel](scripts/no-user-def-route-table.md)  |Verhindert dat virtuele netwerken worden geïmplementeerd met een door de gebruiker gedefinieerde routetabel. |
| [Virtual Network Gateway-SKU's toegestaan](scripts/no-user-def-route-table.md) | Moet virtuele netwerkgateways gebruiken een goedgekeurde SKU en Gatewaytype-type. Geef een matrix van goedgekeurde SKU's en een matrix van goedgekeurde gatewaytypen. |
| [Gebruik goedgekeurde subnet voor VM-netwerkinterfaces](scripts/use-approved-subnet-vm-nics.md) | Moet netwerkinterfaces gebruiken een goedgekeurde subnet. U opgeven de ID van de goedgekeurde subnet. |
| [Goedgekeurde vNet gebruiken voor VM-netwerkinterfaces](scripts/use-approved-vnet-vm-nics.md) | Vereist dat netwerkinterfaces een goedgekeurde virtueel netwerk gebruiken. U opgeven de ID van de goedgekeurde virtueel netwerk. |
|**Tags**||
| [Facturering labels beleid initiatief](scripts/billing-tags-policy-init.md) | Opgegeven labelwaarden vereist voor de naam van de center en het product van de kosten. Maakt gebruik van ingebouwde beleid wilt toepassen en afdwingen van de vereiste labels. Geeft u de vereiste waarden voor de labels.  |
| [Afdwingen van code en de waarde ervan over resourcegroepen](scripts/enforce-tag-rg.md) | Vereist een label en de waarde van een resourcegroep. U opgeven de naam van het vereiste label en waarde.  |
|**SQL**||
| [Controle van SQL DB niveau instelling controleren](scripts/audit-sql-db-audit-setting.md) | Hiermee kunt u controle-instellingen voor SQL-database als deze instellingen komen niet overeen met een opgegeven waarde. U een waarde die aangeeft of controle-instellingen moeten worden ingeschakeld of uitgeschakeld.  |
| [Transparent data encryption status controleren](scripts/audit-trans-data-enc-status.md) | Audits transparent data encryption voor SQL-database als deze niet is ingeschakeld.  |
| [DB niveau threat detectie instelling controleren](scripts/audit-db-threat-det-setting.md) | Audits waarschuwing beleidsregels voor de beveiliging van SQL database als deze beleidsregels zijn niet ingesteld op de opgegeven status. U een waarde die aangeeft of de detectie van dreigingen is ingeschakeld of uitgeschakeld.  |
| [Niveau van de SQL Server audit instelling controleren](scripts/audit-sql-ser-leve-audit-setting.md) | Hiermee kunt u SQL server audit-instellingen als deze instellingen komen niet overeen met een opgegeven waarde. U een waarde die aangeeft of controle-instellingen moeten worden ingeschakeld of uitgeschakeld. |
| [Detectie-serverinstelling niveau threat controleren](scripts/audit-sql-ser-threat-det-setting.md) | Audits waarschuwing beleidsregels voor de beveiliging van SQL database als deze beleidsregels zijn niet ingesteld op de opgegeven status. U een waarde die aangeeft of de detectie van dreigingen is ingeschakeld of uitgeschakeld.  |
| [SQL DB-SKU's toegestaan](scripts/allowed-sql-db-skus.md) | Vereist SQL-databases gebruiken een goedgekeurde SKU. U opgeven een matrix van toegestane SKU-id's of een matrix van toegestane SKU-namen. |
|**Storage**||
| [Toegestane SKU's voor virtuele Machines en Opslagaccounts](scripts/allowed-skus-storage.md) | Vereist dat virtuele machines en opslagaccounts goedgekeurde SKU's gebruiken. Maakt gebruik van ingebouwde beleid om ervoor te zorgen goedgekeurd SKU's. U geeft een matrix van goedgekeurde virtuele machines SKU's en een matrix van goedgekeurde opslagaccount SKU's. |
| [Zorg ervoor dat https-verkeer alleen voor storage-account](scripts/ensure-https-stor-acct.md) | Storage-accounts gebruiken HTTPS-verkeer vereist.  |
| [Cool toegang lagen voor opslagaccounts weigeren](scripts/deny-cool-access-tiering.md) | Verbiedt het gebruik van cool toegang lagen voor blob storage-accounts.  |
| [Zorg ervoor dat de bestandscodering voor opslag](scripts/ensure-store-file-enc.md) | Vereist dat versleuteling is ingeschakeld voor de storage-accounts.  |
|**Ingebouwde beleid**||
| [Toegestane locaties](scripts/allowed-locs.md) | Vereist dat alle resources voor de goedgekeurde locaties worden geïmplementeerd. U opgeven een matrix van goedgekeurde locaties.  |
| [Toegestane brontypen](scripts/allowed-res-types.md) | Zorgt ervoor dat alleen goedgekeurde brontypen die zijn geïmplementeerd. U een matrix met brontypen die zijn toegestaan.  |
| [Storage-account SKU's toegestaan](scripts/allowed-stor-acct-skus.md) | Moet een goedgekeurde SKU storage-accounts gebruiken. U opgeven een matrix van goedgekeurde SKU's. |
| [Label en de standaardwaarde toepassen](scripts/apply-tag-def-val.md) | Voegt een bepaalde tagnaam en waarde, als dat label is niet opgegeven. U de tagnaam en waarde toe te passen.  |
| [Afdwingen van code en de bijbehorende waarde](scripts/enforce-tag-val.md) | Vereist een bepaalde tagnaam en waarde. U de tagnaam en waarde af te dwingen.  |
| [Brontypen die toegestaan niet](scripts/not-allowed-res-type.md) | Hierdoor worden de implementatie van de opgegeven brontypen. U een matrix van de volgende resourcetypen moeten worden geblokkeerd.  |
| [SQL Server versie 12.0 vereisen](scripts/req-sql-12.md) | SQL-servers te gebruiken versie 12.0 vereist.  |
| [Storage-accountversleuteling vereisen](scripts/req-store-acct-enc.md) | Storage-account gebruiken blob versleuteling vereist.  |
