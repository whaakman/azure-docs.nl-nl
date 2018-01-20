---
title: Verbinding maken met gegevensbronnen | Microsoft Docs
description: Hoe kan ik artikel verbinding maken met gegevensbronnen die worden gedetecteerd met Azure Data Catalog is gemarkeerd.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 4e6b27a5-cf75-4012-b88c-333c1fe638e8
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: b5bed534d9fa1a64b0e90c268407281724185ce8
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-connect-to-data-sources"></a>Verbinding maken met gegevensbronnen
## <a name="introduction"></a>Inleiding
**Microsoft Azure Data Catalog** is een volledig beheerde cloudservice die als een systeem van registratie en systeem van detectie voor zakelijke gegevensbronnen fungeert. Met andere woorden, **Azure Data Catalog** alles draait om medewerkers detecteren, begrijpen en gebruiken van gegevensbronnen helpen en organisaties Haal meer uit hun bestaande gegevens te helpen. Een belangrijk aspect van dit scenario wordt met behulp van de gegevens: wanneer een gebruiker een gegevensbron detecteert en het doel ervan begrijpt, de volgende stap is verbinding maken met de gegevensbron om de gegevens beter benutten.

## <a name="data-source-locations"></a>Gegevensbronlocaties
Tijdens de registratie van gegevensbron, **Azure Data Catalog** ontvangt de metagegevens over de gegevensbron. Deze metagegevens omvatten de details van de locatie van de gegevensbron. De details van de locatie van de gegevensbron met gegevensbron verschilt, maar bevat altijd de benodigde informatie om de verbinding. De locatie voor een SQL Server-tabel bevat er bijvoorbeeld voor dat de servernaam, databasenaam, de naam van schema en tabelnaam, terwijl de locatie voor een SQL Server Reporting Services-rapport de servernaam en het pad naar het rapport bevat. Andere typen gegevensbronnen hebt locaties die overeenkomen met de structuur en mogelijkheden van het bronsysteem.

## <a name="integrated-client-tools"></a>Geïntegreerde clienthulpprogramma 's
De eenvoudigste manier om verbinding maken met een gegevensbron is met de "openen in... ' menu van de **Azure Data Catalog** portal. Dit menu geeft een lijst met opties voor het verbinden met de geselecteerde gegevensasset.
Wanneer u de standaardweergave voor de tegel, is dit menu is beschikbaar op de elke tegel.

 ![Bij het openen van een SQL Server-tabel in Excel van de gegevens asset-tegel](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Wanneer u de lijstweergave, is het menu beschikbaar in de zoekbalk aan de bovenkant van het venster van de portal.

 ![Openen van een SQL Server Reporting Services-rapport in Report Manager van de zoekbalk](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Ondersteunde Client-toepassingen
Wanneer u de "openen in... ' menu voor gegevensbronnen in de Azure Data Catalog-portal, de juiste clienttoepassing moet worden geïnstalleerd op de clientcomputer.

| Open in de toepassing | Extensie / protocol | Versies van de ondersteunde toepassing |
| --- | --- | --- |
| Excel |.odc |Excel 2010 of hoger |
| Excel (Top 1000) |.odc |Excel 2010 of hoger |
| Power Query |.xlsx |Excel 2016 of Excel 2010 of Excel 2013 met de Power Query voor Excel-invoegtoepassing geïnstalleerd |
| Power BI Desktop |.pbix |Power BI Desktop juli 2016 of hoger |
| SQL Server Data Tools |vsweb:// |Visual Studio 2013 Update 4 of hoger met SQL Server tooling geïnstalleerd |
| Rapportbeheer |http:// |Zie [Browservereisten voor het SQL Server Reporting Services](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Uw gegevens, de hulpprogramma 's
De beschikbare opties in het menu is afhankelijk van het type van de geselecteerde gegevensasset. Natuurlijk niet alle mogelijke hulpprogramma's worden opgenomen in de "openen in... ' menu, maar is nog steeds eenvoudig verbinding maken met de gegevensbron met behulp van een clienthulpprogramma. Wanneer een gegevensasset is geselecteerd in de **Azure Data Catalog** portal, de volledige locatie wordt weergegeven in het deelvenster properties.

 ![Verbindingsgegevens voor een SQL Server-databasetabel](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

De verbindingsgegevens van de informatie wordt afwijken van het gegevensbrontype met gegevensbrontype, maar de informatie die is opgenomen in de portal, krijgt u alles wat die u moet verbinding maken met de gegevensbron in een clienthulpprogramma. Gebruikers kunnen de details van de verbinding voor de gegevensbronnen die ze hebben gedetecteerd via kopiëren **Azure Data Catalog**, zodat ze werken met de gegevens in een hulpprogramma naar keuze.

## <a name="connecting-and-data-source-permissions"></a>Verbinding maken en gegevens bron machtigingen
Hoewel **Azure Data Catalog** maakt gegevensbronnen kunnen worden gedetecteerd, toegang tot de gegevens zelf blijft onder het beheer van de data source-eigenaar of beheerder. Detectie van een gegevensbron in **Azure Data Catalog** geeft geen een gebruiker machtigingen voor toegang tot de gegevensbron zelf.

Als u wilt maken het gemakkelijker voor gebruikers die een gegevensbron detecteren, maar hebben geen toestemming voor toegang tot de gegevens, kunnen gebruikers informatie in de eigenschap toegang aanvragen opgeven wanneer aantekeningen maken van een gegevensbron. Hier wordt beschreven, waaronder koppelingen naar het proces of contactpunt voor toegang tot de gegevensbron – informatie wordt weergegeven naast de locatie van de gegevensbroninformatie in de portal.

 ![Verbindingsgegevens met aanvraag toegang instructies](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

## <a name="summary"></a>Samenvatting
Registreren van een gegevensbron met **Azure Data Catalog** maakt die gegevens kunnen worden gedetecteerd door te kopiëren structurele en beschrijvende metagegevens uit de gegevensbron in de catalogus-service. Als een gegevensbron is geregistreerd en gedetecteerd, kunnen gebruikers verbinding maken met de gegevensbron van de **Azure Data Catalog** portal "openen in..." " menu of met hun data tools van keuze.

## <a name="see-also"></a>Zie ook
* [Aan de slag met Azure Data Catalog](data-catalog-get-started.md) zelfstudie voor meer informatie over verbinding maken met gegevensbronnen.
