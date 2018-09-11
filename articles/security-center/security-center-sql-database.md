---
title: Azure Security Center en Azure SQL Database-service | Microsoft Docs
description: Dit artikel wordt beschreven hoe Security Center kan u helpen beveiligen van uw databases in Azure SQL Database.
services: sql-database
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: f109adfd-daed-4257-9692-2042a1399480
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: d8be831469659993da4f9f392e23b9408eb283e3
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302214"
---
# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure Security Center en Azure SQL Database-service
[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) helpt u bedreigingen te voorkomen, te detecteren en erop te reageren. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Dit artikel wordt beschreven hoe Security Center kan u helpen beveiligen van uw databases in Azure SQL Database.

## <a name="why-use-security-center"></a>Waarom Security Center gebruiken?
Security Center kunt u gegevens in SQL-Database beveiligen door inzicht in de beveiliging van uw servers en databases te bieden. Met Security Center, kunt u het volgende doen:

* Hiermee definieert u beleid voor controle en versleuteling van de SQL-Database.
* Bewaak de beveiliging van SQL Database-resources in al uw abonnementen.
* Snel identificeren en oplossen van beveiligingsproblemen met zich mee.
* Integreer waarschuwingen van [Azure SQL Database threat detection](../sql-database/sql-database-threat-detection.md).

Naast het beveiligen van uw SQL Database-resources, biedt Security Center ook beveiligingscontrole en -beheer voor virtuele Azure-machines, Cloud Services, App Services, virtuele netwerken en meer. Meer informatie over het Beveiligingscentrum [hier](security-center-intro.md).

## <a name="prerequisites"></a>Vereiste onderdelen
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. De gratis laag van Security Center is ingeschakeld bij uw abonnement. Zie voor meer informatie over Security Center de lagen gratis en standaard [prijzen van Beveiligingscentrum](https://azure.microsoft.com/pricing/details/security-center/).

Security Center biedt ondersteuning voor toegang op basis van rollen. Zie voor meer informatie over op rollen gebaseerd toegangsbeheer (RBAC) in Azure, [toegangsbeheer op basis van Azure Active Directory-rol](../role-based-access-control/role-assignments-portal.md). De veelgestelde vragen over Security Center bevat informatie over het [hoe machtigingen worden verwerkt in Security Center](security-center-faq.md#permissions).

## <a name="access-security-center"></a>Security Center openen
Security Center is toegankelijk via [Azure Portal](https://azure.microsoft.com/features/azure-portal/). [Aanmelden bij de portal](https://portal.azure.com/) en selecteer de **Security Center-optie**.

![Security Center-optie][1]

De **Security Center** blade wordt geopend.
![Blade Security Center][2]

## <a name="set-security-policy"></a>Beveiligingsbeleid instellen
Een beveiligingsbeleid bepaalt welke set besturingselementen die worden aanbevolen voor resources binnen het opgegeven abonnement of resourcegroep. In Security Center definieert u beleid voor uw abonnementen of resourcegroepen overeenkomstig de behoeften van uw bedrijf en het type toepassingen of de gevoeligheid van de gegevens in elk abonnement.

U kunt een beleid om weer te geven van de aanbevelingen voor SQL-controle en transparent data encryption (TDE) voor SQL instellen.

* Wanneer u inschakelt **SQL-controle en detectie van bedreigingen**, Security Center raadt aan dat de controle van toegang tot Azure-Database wordt ingeschakeld voor naleving, geavanceerde detectie en onderzoek.
* Wanneer u inschakelt **transparent data encryption voor SQL**, Security Center wordt aanbevolen dat versleuteling-at-rest wordt ingeschakeld voor uw Azure SQL Database, gekoppelde back-ups en transactielogboekbestanden.

Als u wilt een beveiligingsbeleid instellen, selecteert u de **beleid** tegel op de blade Security Center. Op de **beveiligingsbeleid** blade, selecteer het abonnement dat u wilt inschakelen het beveiligingsbeleid. Selecteer **beleid ter preventie van** en schakel **op** de aanbevelingen voor beveiliging die u wilt gebruiken voor dit abonnement.
![Beveiligingsbeleid][3]

Zie voor meer informatie, [beveiligingsbeleid instellen](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Beveiligingsaanbeveling beheren
De beveiligingsstatus van uw Azure-bronnen worden regelmatig door Security Center gecontroleerd. Wanneer met Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen gedaan. Deze aanbevelingen begeleiden u bij het configureren van de benodigde besturingselementen.

Nadat u een beveiligingsbeleid hebt ingesteld, analyseert Security Center de beveiligingsstatus van uw resources om mogelijke beveiligingsproblemen te identificeren. De aanbevelingen worden weergegeven in de vorm van een tabel waarin elke regel één bepaalde aanbeveling vertegenwoordigt. Gebruik de volgende tabel als referentie om te begrijpen van de beschikbare aanbevelingen voor Azure SQL Database, en elke aanbeveling heeft als u deze toepast. Wanneer u een aanbeveling selecteert, wordt u gaat naar een artikel waarin wordt uitgelegd hoe u de aanbeveling kunt implementeren in Security Center.

| Aanbeveling | Beschrijving |
| --- | --- |
| [Controle en detectie van bedreigingen op SQL-servers inschakelen](security-center-enable-auditing-on-sql-servers.md) |Hiermee wordt aanbevolen dat u controle en detectie van bedreigingen voor SQL Database-servers inschakelen. (Alleen SQL Database-service. Bevat geen Microsoft SQL Server op uw virtuele machines wordt uitgevoerd.) |
| [Controle en detectie van bedreigingen op SQL-databases inschakelen](security-center-enable-auditing-on-sql-databases.md) |Hiermee wordt aanbevolen dat u controle en detectie van bedreigingen voor SQL Database-databases inschakelen. (Alleen SQL Database-service. Bevat geen Microsoft SQL Server op uw virtuele machines wordt uitgevoerd.) |
| [Transparante gegevensversleuteling inschakelen](security-center-enable-transparent-data-encryption.md) |Hiermee wordt aanbevolen dat u versleuteling voor SQL-databases inschakelen. (Alleen SQL Database-service). |

Aanbevelingen voor uw Azure-resources, selecteer de **aanbevelingen** tegel op de blade Security Center. Op de **aanbevelingen** blade, selecteer een aanbeveling voor details. In dit voorbeeld gaan we selecteren **inschakelen controle en detectie van bedreigingen op SQL-servers**.

![Aanbevelingen][4]

Zoals hieronder wordt weergegeven, ziet u in Security Center de SQL-servers waar controle en detectie van bedreigingen niet zijn ingeschakeld. Nadat u controle inschakelt, kunt u instellingen voor detectie van bedreigingen en e-mailinstellingen voor het ontvangen van beveiligingswaarschuwingen configureren. Detectie van bedreigingen wordt u gewaarschuwd wanneer er afwijkende activiteiten die duiden op potentiële beveiligingsrisico's met de database worden gedetecteerd. De waarschuwingen worden weergegeven in het dashboard van Security Center.
![Controle en bedreigingen detecteren][5]

Volg de stappen in [detectie van bedreigingen op SQL-Database in Azure portal](../sql-database/sql-database-threat-detection-portal.md) inschakelen en configureren van detectie van bedreigingen en het configureren van de lijst met e-mailberichten die beveiligingswaarschuwingen tijdens de detectie van afwijkende activiteiten ontvangt.

Zie voor meer informatie over aanbevelingen, [aanbevelingen voor beveiliging beheren](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Beveiligingsstatus controleren
Nadat u een [beveiligingsbeleid](security-center-policies.md) voor de resources van een abonnement hebt ingeschakeld, analyseert Security Center de beveiliging van uw resources om mogelijke beveiligingsproblemen op te sporen.  U vindt de beveiligingsstatus van uw resources in de **resourcebeveiligingsstatus** tegel. Wanneer u klikt op **gegevens** in de **resourcebeveiligingsstatus** tegel, het **gegevensbronnen** blade geopend met de SQL-aanbevelingen voor problemen zoals controle en transparent data versleuteling is niet ingeschakeld. Ook bevat de blade aanbevelingen voor de algemene integriteitsstatus van de database.
![Beveiligingsstatus van de resource][6]

Zie voor meer informatie, [Security health monitoring](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Beveiligingswaarschuwingen beheren en erop reageren
Security Center automatisch verzamelt, analyseert en integreert logboekgegevens van [Azure SQL-Bedreigingsdetectie](../sql-database/sql-database-threat-detection.md), en over andere Azure-resources, om echte bedreigingen te detecteren en fout-positieven te verminderen. In Security Center wordt een lijst met beveiligingswaarschuwingen met prioriteiten weergegeven samen met de informatie die u nodig hebt om snel onderzoek te doen naar het probleem en aanbevelingen voor het herstellen van een aanval.

Om waarschuwingen te bekijken, selecteert u de **beveiligingswaarschuwingen** tegel op de blade Security Center. Op de **beveiligingswaarschuwingen** blade, selecteer een waarschuwing voor meer informatie over de gebeurtenissen die de waarschuwing en welke geactiveerd als elk kunt u de stappen moet uitvoeren om te herstellen van een aanval. In dit voorbeeld gaan we selecteren **mogelijke SQL-injectie**.
![Beveiligingswaarschuwingen][7]

Zoals hieronder wordt weergegeven, Security Center biedt extra informatie die inzicht bieden in wat de waarschuwing, de doelresource, indien van toepassing is geactiveerd de bron-IP-adres en aanbevelingen voor het herstellen.
![Mogelijke SQL-injectie][8]

Zie voor meer informatie, [beheren en erop reageren op beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Volgende stappen
* [Security Center FAQ](security-center-faq.md) : Raadpleeg Veelgestelde vragen over het gebruik van de service.
* [De plannings- en bedieningsgids voor Security Center](security-center-planning-and-operations-guide.md) : Ga als volgt een reeks stappen en taken voor het optimaliseren van uw gebruik van Security Center op basis van de beveiligingsvereisten en het cloudbeheermodel van uw organisatie.
* [Beveiliging van Security Center gegevens](security-center-data-security.md) : informatie over hoe Security Center verzamelt en verwerkt gegevens over uw Azure-resources, met inbegrip van configuratie-informatie, metagegevens, gebeurtenislogboeken, crashdumpbestanden en meer.
* [Beveiligingsincidenten afhandelen](security-center-incident.md) -informatie over het gebruik van de beveiligingswaarschuwingen in Security Center helpt u bij het afhandelen van beveiligingsincidenten.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
