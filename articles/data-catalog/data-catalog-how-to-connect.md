---
title: Verbinding maken met gegevensbronnen in Azure Data Catalog
description: Verbinding maken met gegevensbronnen die zijn gedetecteerd met Azure Data Catalog markeren artikel met instructies.
services: data-catalog
author: steelanddata
ms.author: maroche
ms.assetid: 4e6b27a5-cf75-4012-b88c-333c1fe638e8
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 759758c9f3d0f1dadf2048e8ef15eeab8a77ef07
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053723"
---
# <a name="how-to-connect-to-data-sources"></a>Verbinding maken met gegevensbronnen
## <a name="introduction"></a>Inleiding
**Microsoft Azure Data Catalog** is een volledig beheerde cloudservice die als registratiesysteem en detectiesysteem voor zakelijke gegevensbronnen fungeert. Met andere woorden, **Azure Data Catalog** draait helpt mensen detecteren, begrijpen en gebruiken van gegevensbronnen en waarmee organisaties meer waarde halen uit hun bestaande gegevens. Een belangrijk aspect van dit scenario wordt met behulp van de gegevens, wanneer een gebruiker een gegevensbron detecteert en het doel begrijpt, de volgende stap is het verbinding maken met de gegevensbron om de gegevens te gebruiken.

## <a name="data-source-locations"></a>Gegevensbronnen
Tijdens de registratie van gegevensbron, **Azure Data Catalog** ontvangt metagegevens over de gegevensbron. Deze metagegevens bevat de details van de locatie van de gegevensbron. De details van de locatie van de gegevensbron met gegevensbron verschilt, maar bevat altijd de informatie die nodig is om verbinding te maken. De locatie voor een SQL Server-tabel bevat er bijvoorbeeld voor dat de servernaam, databasenaam, naam van het schema en de tabelnaam van de, terwijl de locatie voor een SQL Server Reporting Services-rapport de naam van de server en het pad naar het rapport bevat. Andere typen gegevensbronnen heeft locaties die overeenkomen met de structuur en de mogelijkheden van het bronsysteem.

## <a name="integrated-client-tools"></a>Geïntegreerde clienthulpprogramma 's
De eenvoudigste manier om verbinding maken met een gegevensbron is met de ' Open in... " in het menu in de **Azure Data Catalog** portal. Dit menu geeft een lijst van opties voor het verbinden met de geselecteerde gegevensasset.
Wanneer u de standaardweergave van de tegel, is dit menu is beschikbaar op de elke tegel.

 ![Een SQL Server-tabel in Excel te openen vanaf de tegel van de asset gegevens](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Wanneer u de lijstweergave gebruikt, wordt het menu in de zoekbalk boven aan het portalvenster beschikbaar is.

 ![Een SQL Server Reporting Services-rapport openen in Report Manager in de zoekbalk](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Ondersteunde Client-toepassingen
Bij het gebruik van de ' Open in... " menu voor gegevensbronnen in de Azure Data Catalog-portal, de juiste clienttoepassing moet worden geïnstalleerd op de clientcomputer.

| Open in de toepassing | Bestandsextensie / -protocol | Ondersteunde toepassingsversies |
| --- | --- | --- |
| Excel |ODC |Excel 2010 of hoger |
| Excel (Top 1000) |ODC |Excel 2010 of hoger |
| Power Query |.xlsx |Excel 2016 of Excel 2010 of Excel 2013 met de Power Query voor Excel-invoegtoepassing geïnstalleerd |
| Power BI Desktop |.pbix |Power BI Desktop juli 2016 of hoger |
| SQL Server Data Tools |vsweb:// |Visual Studio 2013 Update 4 of hoger met SQL Server-hulpprogramma's geïnstalleerd |
| Rapportbeheer |http:// |Zie [Browservereisten voor het SQL Server Reporting Services](https://technet.microsoft.com/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Uw gegevens, uw hulpprogramma 's
De beschikbare opties in het menu is afhankelijk van het type van de geselecteerde gegevensasset. Natuurlijk niet alle mogelijke hulpprogramma's worden opgenomen in de "openen in..." menu, maar is nog wel eenvoudig verbinding maken met de gegevensbron met behulp van een clienthulpprogramma. Wanneer een gegevensasset is geselecteerd in de **Azure Data Catalog** portal, de volledige locatie wordt weergegeven in het deelvenster met eigenschappen.

 ![Verbindingsgegevens voor een SQL Server-tabel](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

De details van de verbinding informatie wijkt af van het gegevensbrontype voor gegevensbrontype, maar de informatie die is opgenomen in de portal krijgt u alles wat die u moet verbinding maken met de gegevensbron in een clienthulpprogramma. Gebruikers kunnen de gegevens van de verbinding voor de gegevensbronnen die ze hebben gedetecteerd met behulp van kopiëren **Azure Data Catalog**, waardoor ze werken met de gegevens in een hulpprogramma naar keuze.

## <a name="connecting-and-data-source-permissions"></a>Machtigingen voor de bron verbinding te maken en gegevens
Hoewel **Azure Data Catalog** maakt gegevensbronnen kunnen worden gedetecteerd, toegang tot de gegevens zelf blijft onder het beheer van de eigenaar van de bron van gegevens of de beheerder. Detectie van een gegevensbron in **Azure Data Catalog** geeft geen een gebruiker geen machtigingen voor toegang tot de gegevensbron zelf.

Als u wilt maken het gemakkelijker voor gebruikers die een gegevensbron detecteren, maar u bent niet gemachtigd voor toegang tot de gegevens, kunnen gebruikers gegevens in de eigenschap verzoeken tot toegang opgeven wanneer aantekeningen maken bij een gegevensbron. Informatie die hier beschikbaar zijn, inclusief koppelingen naar het proces of contactpunt voor het verkrijgen van toegang tot de gegevensbron: wordt weergegeven naast de locatie van de gegevensbroninformatie in de portal.

 ![Gegevens van de verbinding met de aanvraag voor toegang tot instructies](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

## <a name="summary"></a>Samenvatting
Het registreren van een gegevensbron met **Azure Data Catalog** maakt die gegevens kunnen worden gedetecteerd door te kopiëren van de structurele en beschrijvende metagegevens uit de gegevensbron in de Catalog-service. Zodra een gegevensbron is geregistreerd en gedetecteerd, gebruikers verbinding kunnen maken met de gegevensbron van de **Azure Data Catalog** portal ' Open in... "" menu of met behulp van hun hulpprogramma's voor gegevens van keuze.

## <a name="see-also"></a>Zie ook
* [Aan de slag met Azure Data Catalog](data-catalog-get-started.md) zelfstudies voor stapsgewijze informatie over hoe u verbinding maakt met gegevensbronnen.
