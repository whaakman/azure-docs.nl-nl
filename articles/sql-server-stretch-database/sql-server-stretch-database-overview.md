---
title: Overzicht Stretch Database | Microsoft Docs
description: Informatie over hoe Stretch Database uw koude gegevens transparant en veilig naar de Microsoft Azure-cloud migreert.
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: c360dc10-a02b-446f-91a0-278358f7a297
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2016
ms.author: douglasl
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f63900b0a4f32e1bc5a88c068091916bd96aafb8


---
# <a name="stretch-database-overview"></a>Overzicht Stretch Database
Stretch Database migreert uw koude gegevens transparant en veilig naar de Microsoft Azure-cloud.

Als u meteen met Stretch Database aan de slag wilt, bekijk dan [Get started by running the Enable Database for Stretch Wizard](sql-server-stretch-database-wizard.md).

## <a name="what-are-the-benefits-of-stretch-database"></a>Wat zijn de voordelen van Stretch Database?
Stretch Database biedt de volgende voordelen:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Biedt kosten\-effectieve beschikbaarheid voor koude gegevens
Dynamische verspreiding van warme en koude transactionele gegevens van SQL Server naar Microsoft Azure met SQL Server Database. In tegenstelling tot traditionele opslag van koude gegevens zijn uw gegevens altijd online en beschikbaar om op te vragen. U kunt gegevens langer bewaren zonder dat u door grote tabellen, zoals de ordergeschiedenis van klanten, uw budget overschrijdt. Profiteer van de lage kosten van Azure in plaats van dure opslag on\--premises op te schalen. U kiest in Azure Portal de prijscategorie en configureert de instellingen om de kosten onder controle te houden. Schaal naar behoefte omhoog of omlaag. Ga naar de pagina [Prijzen van SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) voor meer informatie.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Geen wijzigingen in query's of toepassingen vereist
U hebt naadloos toegang tot uw SQL Server-gegevens ongeacht of deze \-lokaal zijn opgeslagen of zijn verspreid in de cloud.  U stelt u het beleid vast dat bepaalt waar de gegevens worden opgeslagen, en SQL Server verplaatst op de achtergrond de gegevens. De volledige tabel is altijd online en opvraagbaar. Bovendien vereist Stretch Database geen wijzigingen in bestaande query's of toepassingen – de locatie van de gegevens is volledig transparant voor de toepassing.

### <a name="streamlines-on-premises-data-maintenance"></a>Stroomlijnt onderhoud van \-lokale gegevens
Beperk opslag en onderhoud van \-lokale gegevens. Backups voor uw on\--premises gegevens worden sneller uitgevoerd en tijdens de onderhoudssessie voltooid. Backups voor uw gegevens in de cloud worden automatisch uitgevoerd. Uw on\--premises opslagbehoeften worden aanzienlijk beperkt. Azure-opslag is 80% goedkoper dan een on\--premises SSD toevoegen.

### <a name="keeps-your-data-secure-even-during-migration"></a>Beveiligt uw gegevens, zelfs tijdens migratie
U kunt uw belangrijkste toepassingen met een gerust hart veilig in de cloud plaatsen. Always Encrypted van SQL-Server versleutelt uw gegevens tijdens de migratie. Beveiliging op rijniveau (Row Level Security - RLS) en andere geavanceerde functies van de SQL Server-beveiliging werken ook met Stretch Database samen om uw gegevens te beveiligen.

## <a name="what-does-stretch-database-do"></a>Wat doet Stretch Database?
Nadat u Stretch Database voor een SQL Server-exemplaar hebt ingeschakeld, begint een database, en ten minste één tabel, Stretch Database onopvallend op de achtergrond uw koude gegevens naar Azure te migreren.

* Als u koude gegevens in een afzonderlijke tabel hebt opgeslagen, kunt u de gehele tabel migreren.
* Als uw tabel zowel actuele als koude gegevens bevat, kunt u een filterfunctie opgeven om de rijen te selecteren die u wilt migreren.

**U hoeft bestaande query's en klantenapps niet te wijzigen.** Zelfs tijdens de gegevensmigratie heeft u naadloos toegang tot lokale en externe gegevens. Bij externe query's treedt er een korte latentietijd op, maar deze latentie merkt u alleen wanneer u koude gegevens opvraagt.

**Stretch Database zorgt ervoor dat er geen gegevens verloren gaan** als er een fout optreedt tijdens de migratie. Het bevat ook pogingslogica om verbindingsproblemen op te lossen die tijdens de migratie kunnen optreden. In een dynamische beheerweergave wordt de status van de migratie weergegeven

**U kunt de gegevensmigratie onderbreken** om problemen op de lokale server op te lossen of om de beschikbare netwerkbandbreedte te maximaliseren.

![Overzicht Stretch Database][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Is Stretch Database iets voor u?
Als u het volgende kunt beamen, kan Stretch Database wellicht aan uw wensen voldoen en uw problemen oplossen.

| Als u een zakelijke besluitvormer bent | Als u een DBA bent |
| --- | --- |
| Ik moet transactionele gegevens lange tijd bewaren. |Mijn tabellen worden te groot. |
| Soms moet ik query’s op de koude gegevens uitvoeren. |Mijn gebruikers zeggen dat ze toegang tot koude gegevens willen, maar ze gebruiken deze zelden. |
| Ik heb apps, waaronder oudere apps, die ik niet wil bijwerken. |Ik moet steeds meer extra opslag kopen. |
| Ik wil geld besparen op opslag. |Ik kan in de SLA geen back-up maken van dergelijke grote tabellen of ze herstellen. |

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Welke databases en tabellen zijn geschikt voor Stretch Database?
Stretch Database is bedoeld voor transactionele databases met een grote hoeveelheid koude gegevens, die doorgaans in een klein aantal tabellen zijn opgeslagen. Deze tabellen kunnen meer dan een miljard rijen bevatten.

Als u de tijdelijke-tabelfunctie van SQL Server 2016 gebruikt, gebruik dan Stretch Database om de gehele bijbehorende geschiedenistabel of een deel daarvan naar de kosten\-efficiënte opslag in Azure te migreren. Zie voor meer informatie [Manage Retention of Historical Data in System-Versioned Temporal Tables](https://msdn.microsoft.com/library/mt637341.aspx).

Gebruik Stretch Database Advisor, een functie van SQL Server 2016 Upgrade Advisor, om databases en tabellen voor Stretch Database te identificeren. Zie voor meer informatie [Identify databases and tables for Stretch Database](sql-server-stretch-database-identify-databases.md). Zie [Limitations for Stretch Database](sql-server-stretch-database-limitations.md) (Beperkingen voor Stretch Database) voor meer informatie over potentiële blokkeringsproblemen.

## <a name="test-drive-stretch-database"></a>Testen Stretch Database
**Test Stretch Database met de AdventureWorks voorbeelddatabase.** Om de AdventureWorks voorbeelddatabase te verkrijgen downloadt u [hier](https://www.microsoft.com/download/details.aspx?id=49502) ten minste het databasebestand en het bestand met voorbeelden en scripts. Decomprimeer, nadat u de database naar een exemplaar van SQL Server 2016 heeft hersteld, het voorbeeldbestand en open het Stretch DB voorbeeldbestand uit de map Stretch DB. Voer de scripts in dit bestand uit om de ruimte te controleren die door uw gegevens wordt gebruikt voor- en nadat u Stretch Database heeft ingeschakeld, om de voortgang van de gegevensmigratie bij te houden en om te bevestigen dat u zowel tijdens als na de gegevensmigratie bestaande gegevens kunt blijven opvragen en nieuwe gegevens kunt invoegen.

## <a name="next-step"></a>Volgende stap
**Identificeer de databases en tabellen die geschikt zijn voor Stretch Database.** Download SQL Server 2016 Upgrade Advisor en voer de Stretch Database Advisor uit om databases en tabellen te identificeren die geschikt zijn voor Stretch Database. Stretch Database Advisor identificeert ook problemen met blokkeringen. Zie voor meer informatie [Identify databases and tables for Stretch Database](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png



<!--HONumber=Dec16_HO1-->


