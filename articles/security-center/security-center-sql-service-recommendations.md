---
title: Beveiligen van Azure SQL-service en -gegevens in Azure Security Center | Microsoft Docs
description: Dit document gaat in Azure Security Center aanbevelingen die u helpen beveiligen van uw gegevens en de Azure SQL-service en blijven in overeenstemming met het beveiligingsbeleid.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 0c3a11e9a86767641533b16de1b96b4c59bfdf51
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Beveiligen van Azure SQL-service en -gegevens in Azure Security Center
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer het Beveiligingscentrum identificeert mogelijke beveiligingsproblemen, maakt deze aanbevelingen die u bij het proces begeleiden van het configureren van benodigde besturingselementen.  Aanbevelingen hebben betrekking op Azure brontypen: virtuele machines (VM's), netwerken, SQL en gegevens en toepassingen.

In dit artikel biedt de aanbevelingen die betrekking hebben op Azure SQL-service en -gegevens. Aanbevelingen center rond het inschakelen van controle voor Azure SQL-servers en databases, het inschakelen van versleuteling voor SQL-databases en inschakelen van de versleuteling van uw Azure storage-account.  Gebruik de onderstaande tabel als referentie om te begrijpen van de beschikbare SQL-service en gegevens aanbevelingen en elkaar biedt als u deze toe te passen.

## <a name="available-sql-service-and-data-recommendations"></a>Beschikbare SQL-service en -gegevens aanbevelingen
| Aanbeveling | Beschrijving |
| --- | --- |
| [Controle en detectie van bedreigingen op SQL-servers inschakelen](security-center-enable-auditing-on-sql-servers.md) |Raadt aan dat u de controle en detectie van bedreigingen voor Azure SQL-servers (Azure SQL-service alleen; niet opnemen SQL uitgevoerd op uw virtuele machines) inschakelen. |
| [Controle en detectie van bedreigingen op SQL-databases inschakelen](security-center-enable-auditing-on-sql-databases.md) |Raadt aan dat u de controle en detectie van bedreigingen voor Azure SQL-databases (Azure SQL-service alleen; niet opnemen SQL uitgevoerd op uw virtuele machines) inschakelen. |
| [Transparante gegevensversleuteling inschakelt op de SQL-databases](security-center-enable-transparent-data-encryption.md) |Raadt aan om versleuteling voor SQL-databases (alleen voor Azure SQL-service) in te schakelen. |

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over de aanbevelingen die betrekking hebben op andere Azure-resource-typen:

* [Beveiligen van uw virtuele machines in Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Beveiligen van uw toepassingen in Azure Security Center](security-center-application-recommendations.md)
* [Beveiligen van uw netwerk in Azure Security Center](security-center-network-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
