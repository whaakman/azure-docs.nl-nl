---
title: Azure betaling verwerking blauwdruk - overzicht
description: Azure betaling verwerking blauwdruk - klant verantwoordelijkheid matrix (overzicht)
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 23cf68d8-bebd-4ac4-a194-39e052281c0e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 49acce706f09fe08b257ce8a8554de5da20060a1
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="pci-dss-requirements---high-level-overview"></a>Vereisten voor PCI DSS - overzicht

De Payment Card Industry Data Security Standard (PCI DSS) is ontwikkeld om te stimuleren en kaarthouder gegevensbeveiliging verbeteren en de brede acceptatie van consistente gegevens veiligheidsmaatregelen globaal vergemakkelijken. PCI DSS biedt een basislijn van technische en operationele vereisten die zijn ontworpen om accountgegevens te beveiligen. PCI DSS geldt voor alle entiteiten die betrokken zijn bij het verwerken van betaling-kaart, inclusief verkopers, processors, acquirers, uitgevers van certificaten en serviceproviders. PCI DSS geldt ook voor alle andere entiteiten opslaan, verwerkt of verzonden gegevens kaarthouder (CHD) en/of gevoelige verificatiegegevens (SAD). Hieronder vindt u een overzicht van de 12 PCI DSS-vereisten.

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

|   |   |
|---|---|
| **Maken en onderhouden van een veilige<br/>netwerk- en systemen** | 1. [Installeren en onderhouden van een firewallconfiguratie om kaarthouder gegevens te beveiligen](pci-dss-requirement-1-firewall.md)<br/><br/> 2. [Gebruik geen leverancier standaardwaarden voor Systeemwachtwoorden en andere beveiligingsparameters](pci-dss-requirement-2-password.md) |  
| **Kaarthouder gegevens beveiligen** | 3. [Opgeslagen kaarthouder gegevens beveiligen](pci-dss-requirement-3-chd.md)<br/><br/> 4. [Overdracht van gegevens van de kaarthouder coderen via open, op openbare netwerken](pci-dss-requirement-4-encryption.md) |
| **Een beveiligingsprobleem onderhouden<br/>Management-programma** | 5. [Beveiliging van alle computers tegen schadelijke software en programma's installeren of de antivirussoftware regelmatig bij te werken](pci-dss-requirement-5-malware.md)<br/><br/> 6. [Ontwikkelen en te onderhouden van veilige systemen en toepassingen](pci-dss-requirement-6-secure-system.md) |
| **Sterke toegang implementeren<br/>metingen beheren** | 7. [Toegang tot gegevens van de kaarthouder beperken door bedrijven dat nodig is](pci-dss-requirement-7-access.md)<br/><br/> 8. [Identificeren en verifiëren van toegang tot de onderdelen van het systeem](pci-dss-requirement-8-identity.md) <br/><br/> 9. [Beperk de fysieke toegang tot gegevens van de kaarthouder](pci-dss-requirement-9-physical-access.md) |
| **Regelmatig controleren en<br/>netwerken testen** | 10. [Traceren en alle toegang tot netwerkbronnen en kaarthouder gegevens controleren](pci-dss-requirement-10-monitoring.md) <br/><br/> 11. [Regelmatig testen beveiligingssystemen en processen](pci-dss-requirement-11-testing.md) |
| **Een informatie onderhouden<br/>beveiligingsbeleid** | 12. [Een beleid die zijn gericht op beveiliging van informatie voor alle medewerkers onderhouden](pci-dss-requirement-12-policy.md) |

