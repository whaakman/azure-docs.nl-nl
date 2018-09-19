---
title: Beveiligen van Azure SQL-service en -gegevens in Azure Security Center | Microsoft Docs
description: Dit document adressen aanbevelingen in Azure Security Center waarmee u Bescherm uw gegevens en Azure SQL-service en blijven in overeenstemming met beveiligingsbeleid.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 45f5dc840f015793912e314ab3d47e54a409708e
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126663"
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Beveiligen van Azure SQL-service en -gegevens in Azure Security Center
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen die u bij het proces begeleiden van het configureren van de benodigde besturingselementen.  Aanbevelingen zijn van toepassing op Azure-resource-typen: virtuele machines (VM's), netwerk-, SQL en gegevens en toepassingen.

In dit artikel biedt aanbevelingen die betrekking hebben op Azure SQL-service en de gegevens. Aanbevelingen voor centreren rond het inschakelen van controle voor Azure SQL-servers en databases, het inschakelen van versleuteling voor SQL-databases en inschakelen van versleuteling van uw Azure storage-account.  Gebruik de onderstaande tabel als referentie om te begrijpen van de beschikbare SQL-service en aanbevelingen en wat elke doet als u deze toepast.
### <a name="monitor-data-security"></a>Gegevensbeveiliging bewaken

Wanneer u klikt op **Gegevensbeveiliging** in de sectie **Preventie**, wordt **Gegevensbronnen** geopend met aanbevelingen voor SQL en opslag. Ook bevat de blade [aanbevelingen](security-center-sql-service-recommendations.md) voor de algemene integriteitsstatus van de database. Lees voor meer informatie over de versleuteling van opslag [Versleuteling inschakelen voor een Azure-opslagaccount in Azure Security Center](security-center-enable-encryption-for-storage-account.md).

![Gegevensbronnen](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

U kunt onder **SQL-aanbevelingen** op elke aanbeveling klikken voor meer informatie over verdere acties die u kunt ondernemen om een probleem te verhelpen. In het volgende voorbeeld is de aanbeveling **Databasecontrole en detectie van bedreigingen in SQL-databases** uitgevouwen.

![Details over een SQL-aanbeveling](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

**Controle en detectie van bedreigingen in SQL-databases inschakelen** bevat de volgende informatie:

* Een lijst met SQL-databases
* De server waarop deze zich bevinden
* Informatie over of deze instelling is overgenomen van de server of dat deze uniek is in deze database
* De huidige status
* De ernst van het probleem

Als u op de database klikt om deze aanbeveling op te volgen, wordt **Controle en detectie van bedreigingen** geopend, zoals wordt weergegeven in het volgende scherm.

![Controle en detectie van bedreigingen](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Als u controle wilt inschakelen, hoeft u alleen maar **AAN** te selecteren onder de optie **Controle**.

## <a name="available-sql-service-and-data-recommendations"></a>Beschikbare SQL-service en aanbevelingen
| Aanbeveling | Beschrijving |
| --- | --- |
| [Controle en detectie van bedreigingen op SQL-servers inschakelen](security-center-enable-auditing-on-sql-servers.md) |Hiermee wordt aanbevolen dat u controle en detectie van bedreigingen voor Azure SQL-servers (Azure SQL-service alleen; bevat geen SQL die worden uitgevoerd op uw virtuele machines) inschakelen. |
| [Controle en detectie van bedreigingen op SQL-databases inschakelen](security-center-enable-auditing-on-sql-databases.md) |Hiermee wordt aanbevolen dat u controle en detectie van bedreigingen voor Azure SQL-databases (Azure SQL-service alleen; bevat geen SQL die worden uitgevoerd op uw virtuele machines) inschakelen. |
| [Transparent Data Encryption inschakelen voor SQL-databases](security-center-enable-transparent-data-encryption.md) |Hiermee wordt aanbevolen dat u versleuteling voor SQL-databases (alleen voor Azure SQL-service inschakelen). |

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over aanbevelingen die betrekking hebben op andere typen Azure-resources:

* [Protecting your virtual machines in Azure Security Center](security-center-virtual-machine-recommendations.md) (Uw virtuele machines beveiligen in Azure Security Center)
* [Protecting your applications in Azure Security Center](security-center-application-recommendations.md) (Uw toepassingen beveiligen in Azure Security Center)
* [Protecting your network in Azure Security Center](security-center-network-recommendations.md) (Uw netwerk beveiligen in Azure Security Center)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
