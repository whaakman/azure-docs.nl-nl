---
title: Azure Security Center en Azure SQL Database-service | Microsoft Docs
description: Dit artikel laat zien hoe Security Center kan u helpen uw database in Azure SQL Database beveiligen.
services: sql-database
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f109adfd-daed-4257-9692-2042a1399480
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 46dd298a5664d914e55d45c5b7599d5983287476
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure Security Center en Azure SQL Database-service
[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) helpt u bedreigingen te voorkomen, te detecteren en erop te reageren. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Dit artikel laat zien hoe Security Center kan u helpen uw database in Azure SQL Database beveiligen.

## <a name="why-use-security-center"></a>Waarom Security Center gebruiken?
Security Center helpt u gegevens in SQL-Database beveiligt door te geven inzicht in de beveiliging van uw servers en databases. U kunt met Security Center:

* Beleid voor versleuteling van de SQL-Database en controle definiëren.
* Het bewaken van de beveiliging van SQL Database-resources voor al uw abonnementen.
* Snel identificeren en te verhelpen beveiligingsproblemen.
* Waarschuwingen van integreren [Azure SQL Database met detectie van dreigingen](../sql-database/sql-database-threat-detection.md).

Naast het beveiligen van uw SQL-Database-resources, biedt Security Center ook beveiligingsbewaking en beheer voor virtuele machines in Azure, Cloud Services, App-Services, virtuele netwerken en meer. Meer informatie over Security Center [hier](security-center-intro.md).

## <a name="prerequisites"></a>Vereisten
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. De gratis categorie van het Beveiligingscentrum is met uw abonnement ingeschakeld. Zie voor meer informatie over de vrije en de standaard lagen van Security Center [Security Center prijzen](https://azure.microsoft.com/pricing/details/security-center/).

Security Center biedt ondersteuning voor toegang op basis van rollen. Zie voor meer informatie over op rollen gebaseerde toegangsbeheer (RBAC) in Azure, [toegangsbeheer op basis van een functie van Azure Active Directory](../active-directory/role-based-access-control-configure.md). De Security Center FAQ biedt informatie over [hoe machtigingen worden verwerkt in Security Center](security-center-faq.md#permissions).

## <a name="access-security-center"></a>Security Center openen
Security Center is toegankelijk via [Azure Portal](https://azure.microsoft.com/features/azure-portal/). [Aanmelden bij de portal](https://portal.azure.com/) en selecteer de **Security Center-optie**.

![Security Center-optie][1]

De **Security Center** blade wordt geopend.
![Blade Security Center][2]

## <a name="set-security-policy"></a>Beveiligingsbeleid instellen
Een beveiligingsbeleid bepaalt welke set besturingselementen die worden aanbevolen voor resources binnen het opgegeven abonnement of resourcegroep. In Security Center definieert u beleid voor uw abonnementen of de resourcegroepen op basis van de beveiligingsvereisten van uw bedrijf en het type toepassingen of de vertrouwelijkheid van de gegevens in elk abonnement.

U kunt een beleid voor het weergeven van aanbevelingen voor SQL-controle en transparent data encryption voor SQL (TDE) instellen.

* Wanneer u inschakelt **SQL controle en detectie van bedreigingen**, Security Center raadt aan dat de controle van toegang tot Azure-Database wordt ingeschakeld voor naleving, geavanceerde detectie en onderzoek doeleinden.
* Wanneer u inschakelt **transparent data encryption voor SQL**, Security Center raadt aan dat versleuteling in rust zijn ingeschakeld voor uw Azure SQL Database, gekoppelde back-ups en transactielogboekbestanden.

Als u een beveiligingsbeleid, selecteert u de **beleid** tegel op de blade Security Center. Op de **beveiligingsbeleid** blade, selecteer het abonnement dat u wilt inschakelen het beveiligingsbeleid. Selecteer **preventiebeleid** en schakel **op** de aanbevelingen voor beveiliging die u wilt gebruiken voor dit abonnement.
![Beveiligingsbeleid][3]

Zie voor meer informatie, [instellen van beveiligingsbeleid](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Beveiligingsaanbeveling beheren
De beveiligingsstatus van uw Azure-bronnen worden regelmatig door Security Center gecontroleerd. Wanneer met Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen gedaan. Deze aanbevelingen begeleiden u bij het configureren van de benodigde besturingselementen.

Nadat u een beveiligingsbeleid instellen, analyseert Security Center de beveiligingsstatus van uw resources voor het identificeren van mogelijke beveiligingsproblemen. De aanbevelingen worden weergegeven in een tabel waarin elke regel staat voor een bepaalde aanbeveling. Gebruik de volgende tabel als uitgangspunt om te begrijpen van de beschikbare aanbevelingen voor Azure SQL Database en elke aanbeveling heeft als u deze toe te passen. Als u een aanbeveling gaat u naar een artikel met uitleg over het implementeren van de aanbeveling in Security Center.

| Aanbeveling | Beschrijving |
| --- | --- |
| [Inschakelen van controle en detectie van bedreigingen op SQL-servers](security-center-enable-auditing-on-sql-servers.md) |Raadt aan dat u de controle en detectie van bedreigingen voor SQL Database-servers inschakelen. (Alleen voor SQL Database-service. Bevat geen Microsoft SQL Server op uw virtuele machines.) |
| [Inschakelen van controle en detectie van bedreigingen op SQL-databases](security-center-enable-auditing-on-sql-databases.md) |Raadt aan dat u de controle en detectie van bedreigingen voor databases in SQL-Database inschakelen. (Alleen voor SQL Database-service. Bevat geen Microsoft SQL Server op uw virtuele machines.) |
| [Transparante gegevensversleuteling inschakelen](security-center-enable-transparent-data-encryption.md) |Raadt aan om versleuteling voor SQL-databases in te schakelen. (Alleen SQL-Database-service). |

Ga voor aanbevelingen voor uw Azure-resources, selecteer de **aanbevelingen** tegel op de blade Security Center. Op de **aanbevelingen** blade, selecteer details van een aanbeveling om te zien. In dit voorbeeld gaan we selecteren **controle inschakelen en detectie van bedreigingen op SQL-servers**.

![Aanbevelingen][4]

Zoals hieronder wordt weergegeven, ziet u in Security Center de SQL-servers waarop de controle en detectie van bedreigingen niet zijn ingeschakeld. Nadat u controle, kunt u de instellingen van de detectie van dreigingen en e-mailinstellingen voor het ontvangen van beveiligingsberichten kunt configureren. Detectie van dreigingen waarschuwt u wanneer deze worden afwijkende databaseactiviteiten die duiden op beveiligingsdreigingen voor de database gedetecteerd. De waarschuwingen worden weergegeven in de Security Center-dashboard.
![Controle en detectie van bedreigingen][5]

Volg de stappen in [bedreigingsdetectie SQL-Database in de Azure portal](../sql-database/sql-database-threat-detection-portal.md) inschakelen en configureren van detectie van dreigingen en configureren van de lijst met e-mailberichten die beveiligingswaarschuwingen bij vreemde activiteiten worden ontvangen.

Zie voor meer informatie over aanbevelingen, [aanbevelingen voor beveiliging beheren](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Beveiligingsstatus controleren
Nadat u een [beveiligingsbeleid](security-center-policies.md) voor de resources van een abonnement hebt ingeschakeld, analyseert Security Center de beveiliging van uw resources om mogelijke beveiligingsproblemen op te sporen.  U vindt de beveiligingsstatus van uw resources in de **resourcebeveiligingsstatus** tegel. Wanneer u klikt op **gegevens** in de **resourcebeveiligingsstatus** tegel, de **gegevensbronnen** er wordt een blade geopend met de SQL-aanbevelingen voor problemen zoals controle en transparante gegevensversleuteling niet is ingeschakeld. Ook bevat de blade aanbevelingen voor de algemene integriteitsstatus van de database.
![Beveiligingsstatus van de resource][6]

Zie voor meer informatie, [Security health monitoring](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Beveiligingswaarschuwingen beheren en erop reageren
Security Center automatisch verzamelt, analyseert en integreert logboekgegevens van [Azure SQL-Bedreigingsdetectie](../sql-database/sql-database-threat-detection.md), plus andere Azure-resources om werkelijke dreigingen te detecteren en fout-positieven te verminderen. In Security Center wordt een lijst met beveiligingswaarschuwingen met prioriteiten weergegeven samen met de informatie die u nodig hebt om snel onderzoek te doen naar het probleem en aanbevelingen voor het herstellen van een aanval.

Om waarschuwingen te bekijken, selecteert u de **beveiligingswaarschuwingen** tegel op de blade Security Center. Op de **beveiligingswaarschuwingen** blade, selecteer een waarschuwing voor meer informatie over de gebeurtenissen die de waarschuwing en welke geactiveerd als eventuele stappen u moet uitvoeren om te herstellen van een aanval. In dit voorbeeld gaan we selecteren **potentiële SQL-injectie**.
![Beveiligingswaarschuwingen][7]

Zoals hieronder wordt weergegeven, Security Center biedt aanvullende informatie inzicht biedt in wat de waarschuwing, de doelresource, indien van toepassing is geactiveerd de IP-bronadres en aanbevelingen voor het herstellen.
![Mogelijke SQL-injectie][8]

Zie voor meer informatie, [beheren en erop reageren beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Volgende stappen
* [Security Center FAQ](security-center-faq.md) : Raadpleeg Veelgestelde vragen over het gebruik van de service.
* [Security Center plannen en de operations guide](security-center-planning-and-operations-guide.md) : een reeks stappen en taken voor het optimaliseren van uw gebruik van het Beveiligingscentrum op basis van de beveiligingsvereisten en het cloudbeheermodel van uw organisatie.
* [Security Center-gegevensbeveiliging](security-center-data-security.md) : meer informatie over hoe Security Center verzamelt en gegevens over uw Azure-resources, met inbegrip van configuratie-informatie, metagegevens, gebeurtenislogboeken, crashdumpbestanden en meer verwerkt.
* [Afhandeling van beveiligingsincidenten](security-center-incident.md) -informatie over het gebruik van de waarschuwing mogelijkheid beveiliging in Security Center helpt u bij het afhandelen van beveiligingsincidenten.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
