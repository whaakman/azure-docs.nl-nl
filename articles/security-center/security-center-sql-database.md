---
title: Azure Security Center-en Azure SQL Database-Service | Microsoft Docs
description: In dit artikel wordt beschreven hoe Security Center u kan helpen bij het beveiligen van uw data bases in Azure SQL Database.
services: sql-database
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f109adfd-daed-4257-9692-2042a1399480
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: rkarlin
ms.openlocfilehash: 0a889de79b6a5921007614dac8d610c1be0222d2
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "60704557"
---
# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure Security Center-en Azure SQL Database-Service
[Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) helpt u bedreigingen te voorkomen, te detecteren en erop te reageren. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

In dit artikel wordt beschreven hoe Security Center u kan helpen bij het beveiligen van uw data bases in Azure SQL Database.

## <a name="why-use-security-center"></a>Waarom Security Center gebruiken?
Security Center helpt u bij het beveiligen van gegevens in SQL Database door inzicht te krijgen in de beveiliging van al uw servers en data bases. Met Security Center kunt u het volgende doen:

* Beleids regels definiëren voor SQL Database versleuteling en controle.
* Bewaak de beveiliging van SQL Database resources in al uw abonnementen.
* U kunt beveiligings problemen snel identificeren en oplossen.
* Integreer waarschuwingen van [Azure SQL database-detectie van bedreigingen](../sql-database/sql-database-threat-detection.md).

Naast het helpen beveiligen van uw SQL Database-resources, biedt Security Center ook beveiligings bewaking en-beheer voor virtuele machines van Azure, Cloud Services, App Services, virtuele netwerken en meer. Meer informatie over Security Center [hier](security-center-intro.md).

## <a name="prerequisites"></a>Vereisten
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. De gratis laag van Security Center is ingeschakeld voor uw abonnement. Zie [Security Center prijzen](https://azure.microsoft.com/pricing/details/security-center/)voor meer informatie over de gratis en standaard lagen van Security Center.

Security Center ondersteunt op rollen gebaseerde toegang. Zie [Azure Active Directory op rollen gebaseerd Access Control](../role-based-access-control/role-assignments-portal.md)voor meer informatie over op rollen gebaseerd toegangs beheer (RBAC) in Azure. De veelgestelde vragen over Security Center biedt informatie over [hoe machtigingen worden verwerkt in Security Center](security-center-faq.md#permissions).

## <a name="access-security-center"></a>Security Center openen
Security Center is toegankelijk via [Azure Portal](https://azure.microsoft.com/features/azure-portal/). [Meld u aan bij de portal](https://portal.azure.com/) en selecteer de **optie Security Center**.

![Security Center optie][1]

De Blade **Security Center** wordt geopend.
![Security Center Blade][2]

## <a name="set-security-policy"></a>Beveiligingsbeleid instellen
Een beveiligings beleid bepaalt welke set besturings elementen wordt aanbevolen voor resources binnen het opgegeven abonnement of deze resource groep. In Security Center definieert u beleid voor uw abonnementen of resource groepen op basis van de beveiligings behoeften van uw bedrijf en het type toepassingen of de gevoeligheid van de gegevens in elk abonnement.

U kunt een beleid instellen om aanbevelingen voor SQL-controle en SQL transparent Data Encryption (TDE) weer te geven.

* Wanneer u SQL- **controle en detectie van bedreigingen**inschakelt, wordt Security Center aanbevolen om de controle van toegang tot Azure data base in te scha kelen voor naleving, geavanceerde detectie en onderzoek.
* Wanneer u **SQL transparent Data Encryption**inschakelt, wordt Security Center aanbevolen dat versleuteling op rest wordt ingeschakeld voor uw Azure SQL database, gekoppelde back-ups en transactie logboek bestanden.

Als u een beveiligings beleid wilt instellen, selecteert u de tegel **beleid** op de blade Security Center. Selecteer op de Blade **beveiligings beleid** het abonnement waarvoor u het beveiligings beleid wilt inschakelen. Selecteer **preventie beleid** en Schakel de beveiligings aanbevelingen in die u wilt gebruiken voor dit abonnement.
![Beveiligingsbeleid][3]

Zie [beveiligings beleid instellen](tutorial-security-policy.md)voor meer informatie.

## <a name="manage-security-recommendation"></a>Beveiligings aanbeveling beheren
De beveiligingsstatus van uw Azure-bronnen worden regelmatig door Security Center gecontroleerd. Wanneer met Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen gedaan. Deze aanbevelingen begeleiden u bij het configureren van de benodigde besturingselementen.

Nadat u een beveiligings beleid hebt ingesteld, analyseert Security Center de beveiligings status van uw resources om mogelijke beveiligings problemen te identificeren. De aanbevelingen worden weer gegeven in een tabel indeling waarin elke regel een bepaalde aanbeveling vertegenwoordigt. Gebruik de volgende tabel als referentie om inzicht te krijgen in de beschik bare aanbevelingen voor Azure SQL Database en wat elke aanbeveling doet als u deze toepast. Als u een aanbeveling selecteert, gaat u naar een artikel waarin wordt uitgelegd hoe u de aanbeveling in Security Center implementeert.

| Aanbeveling | Description |
| --- | --- |
| [Controle en detectie van bedreigingen inschakelen op SQL-servers](security-center-enable-auditing-on-sql-servers.md) |Hiermee wordt aanbevolen om controle en detectie van bedreigingen in te scha kelen voor SQL Database-servers. (Alleen SQL Database-Service. Bevat geen Microsoft SQL Server die worden uitgevoerd op uw virtuele machines.) |
| [Controle en detectie van bedreigingen inschakelen voor SQL-data bases](security-center-enable-auditing-on-sql-databases.md) |Hiermee wordt aanbevolen om controle en detectie van bedreigingen in te scha kelen voor SQL Database-data bases. (Alleen SQL Database-Service. Bevat geen Microsoft SQL Server die worden uitgevoerd op uw virtuele machines.) |
| [Transparante gegevensversleuteling inschakelen](security-center-enable-transparent-data-encryption.md) |Hiermee wordt aanbevolen om versleuteling voor SQL-data bases in te scha kelen. (Alleen SQL Database-Service.) |

Als u aanbevelingen voor uw Azure-resources wilt bekijken, selecteert u de tegel **aanbevelingen** op de blade Security Center. Selecteer op de Blade **aanbevelingen** een aanbeveling om details weer te geven. In dit voor beeld selecteren we **controle inschakelen & detectie van bedreigingen op SQL-servers**.

![Aanbevelingen][4]

Zoals hieronder wordt weer gegeven, bevat Security Center de SQL-servers waar controle en detectie van bedreigingen niet zijn ingeschakeld. Nadat u controle hebt ingeschakeld, kunt u instellingen voor detectie van bedreigingen en e-mail instellingen configureren om beveiligings waarschuwingen te ontvangen. Met detectie van bedreigingen wordt u gewaarschuwd wanneer er afwijkende database activiteiten worden gedetecteerd die duiden op potentiële beveiligings dreigingen voor de data base. De waarschuwingen worden weer gegeven in het Security Center dash board.
![Controle en bedreigingen detecteren][5]

Volg de stappen in [SQL database detectie van bedreigingen in de Azure Portal](../sql-database/sql-database-threat-detection-portal.md) om detectie van dreigingen in te scha kelen en te configureren en om de lijst met e-mail berichten te configureren die beveiligings waarschuwingen ontvangen bij het detecteren van afwijkende activiteiten.

Zie [beveiligings aanbevelingen beheren](security-center-recommendations.md)voor meer informatie over aanbevelingen.

## <a name="monitor-security-health"></a>Beveiligingsstatus controleren
Nadat u een [beveiligingsbeleid](tutorial-security-policy.md) voor de resources van een abonnement hebt ingeschakeld, analyseert Security Center de beveiliging van uw resources om mogelijke beveiligingsproblemen op te sporen.  U kunt de beveiligings status van uw resources bekijken op de tegel **beveiligings status** van de resource. Wanneer u op **gegevens** in de tegel **beveiligings status** van de resource klikt, wordt de Blade **gegevens bronnen** geopend met SQL-aanbevelingen voor problemen zoals controle en transparante gegevens versleuteling die niet is ingeschakeld. Ook bevat de blade aanbevelingen voor de algemene integriteitsstatus van de database.
![Beveiligings status van de resource][6]

Zie [beveiligings status controleren](security-center-monitoring.md)voor meer informatie.

## <a name="manage-and-respond-to-security-alerts"></a>Beveiligingswaarschuwingen beheren en erop reageren
Security Center verzamelt, analyseert en integreert automatisch logboek gegevens van de [Azure SQL-bedreigings detectie](../sql-database/sql-database-threat-detection.md), evenals andere Azure-resources, om echte bedreigingen te detecteren en fout-positieven te verminderen. In Security Center wordt een lijst met beveiligingswaarschuwingen met prioriteiten weergegeven samen met de informatie die u nodig hebt om snel onderzoek te doen naar het probleem en aanbevelingen voor het herstellen van een aanval.

Als u waarschuwingen wilt weer geven, selecteert u de tegel **beveiligings waarschuwingen** op de blade Security Center. Selecteer op de Blade **beveiligings waarschuwingen** een waarschuwing voor meer informatie over de gebeurtenissen die de waarschuwing hebben geactiveerd en wat, indien van toepassing, stappen die u moet uitvoeren om een aanval te herstellen. In dit voor beeld selecteren we **mogelijke SQL**-injecties.
![Beveiligingswaarschuwingen][7]

Zoals hieronder wordt weer gegeven, bevat Security Center extra informatie over de trigger van de waarschuwing, de doel resource, het bron-IP-adres en aanbevelingen voor het oplossen van het probleem.
![Mogelijke SQL-injectie][8]

Zie [beveiligings waarschuwingen beheren en erop reageren](security-center-managing-and-responding-alerts.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
* [Security Center FAQ](security-center-faq.md) : vind Veelgestelde vragen over het gebruik van de service.
* [Security Center plannings-en bedienings handleiding](security-center-planning-and-operations-guide.md) : Volg een reeks stappen en taken om uw gebruik van Security Center te optimaliseren op basis van de beveiligings vereisten van uw organisatie en het model voor Cloud beheer.
* [Security Center gegevens beveiliging](security-center-data-security.md) : informatie over hoe Security Center gegevens over uw Azure-resources verzamelt en verwerkt, waaronder configuratie-informatie, meta gegevens, gebeurtenis logboeken, crash dump bestanden en meer.
* [Beveiligings incidenten](security-center-incident.md) afhandelen: informatie over het gebruik van de functie voor beveiligings waarschuwingen in Security Center om u te helpen bij het afhandelen van beveiligings incidenten.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
