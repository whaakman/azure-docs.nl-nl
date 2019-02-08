---
title: Azure SQL Database-gegevensdetectie en classificatie | Microsoft Docs
description: Azure SQL Database-gegevensdetectie en classificatie
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: 3c5f087ed44c252737e7f45fde12a4b509637499
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892878"
---
# <a name="azure-sql-database-data-discovery--classification"></a>Azure SQL Database-gegevensdetectie en classificatie

Gegevensdetectie en -classificatie (momenteel in preview) biedt geavanceerde mogelijkheden van ingebouwd in Azure SQL Database voor **detecteren**, **classificeren**, **labels**  &  **beveiligen** de gevoelige gegevens in uw databases.
Detecteren en classificeren van uw meest gevoelige gegevens (business, financiële, gezondheidszorg, persoonlijke gegevens (PII) en enz.) kunnen een cruciale rol spelen in uw organisatie informatie protection-status. Deze kan dienen als infrastructuur voor:

- Helpen voldoen aan de privacystandaarden en wettelijke vereisten.
- Verschillende security-scenario's, zoals bewaking (controle) en waarschuwingen voor afwijkende toegang tot gevoelige gegevens.
- Toegang tot beheren en beperken van de beveiliging van databases met zeer gevoelige gegevens.

Gegevensdetectie en classificatie maakt deel uit van de [geavanceerde gegevensbeveiliging](sql-database-advanced-data-security.md) (AD) aanbieding waarmee een uniforme-voor geavanceerde mogelijkheden voor de beveiliging van SQL pakket. gegevensdetectie en classificatie kunnen worden geopend en worden beheerd via de centrale SQL AD-portal.

> [!NOTE]
> Dit document heeft alleen betrekking op Azure SQL Database. Zie voor SQL Server (on-premises) [SQL-gegevensdetectie en classificatie](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Wat is er gegevensdetectie en classificatie

Gegevensdetectie en classificatie introduceert een aantal geavanceerde services en de nieuwe SQL-functies, vormen een nieuwe SQL Information Protection paradigma is gericht op het beschermen van gegevens, niet alleen de database:

- **Detectie en aanbevelingen**

  De classificatie-engine van uw database scant en identificeert de kolommen die potentieel gevoelige gegevens bevatten. Vervolgens wordt u een eenvoudige manier om te bekijken en toepassen van de juiste classificatie aanbevelingen voor afstemming via Azure portal.

- **Labeling**

  Gevoeligheid classificatielabels kunnen persistent worden gelabeld voor kolommen met nieuwe classificatie metagegevens kenmerken die zijn geïntroduceerd in de SQL-Engine. Deze metagegevens kan vervolgens worden gebruikt voor geavanceerde op basis van gevoeligheid controle en bescherming van scenario's.

- **Query resultaat set gevoeligheid**

  De gevoeligheid van de queryresultaatset wordt berekend in realtime voor controledoeleinden.

- **Zichtbaarheid**

  De status van de database-indeling kan worden weergegeven in een gedetailleerd dashboard in de portal. Bovendien kunt u een rapport (in de Excel-indeling) downloaden om te worden gebruikt voor naleving & controledoeleinden, evenals andere behoeften.

## <a id="subheading-2"></a>Detecteren, classificeren en labelen gevoelige kolommen

Het volgende gedeelte bevat de stappen voor het detecteren, classificeren en labelen kolommen met gevoelige gegevens in uw database, evenals de huidige status van de classificatie van uw database bekijken en rapporten exporteert.

De classificatie bevat twee metagegevens kenmerken:

- Labels: de kenmerken van de belangrijkste classificatie, gebruikt voor het definiëren van het vertrouwelijkheidsniveau van de gegevens die zijn opgeslagen in de kolom.  
- Informatietypen: bieden meer granulatie in het type van de gegevens die zijn opgeslagen in de kolom.

## <a name="define-and-customize-your-classification-taxonomy"></a>Definiëren en aanpassen van uw taxonomie classificatie

SQL-gegevensdetectie en classificatie wordt geleverd met een ingebouwde set gevoeligheidslabels en een ingebouwde verzameling informatietypen en logica voor detectie. U hebt nu de mogelijkheid deze taxonomie aanpassen en het definiëren van een set en de rangorde van classification constructies specifiek voor uw omgeving.

Definitie- en aanpassen van uw taxonomie classificatie wordt uitgevoerd in één centrale locatie voor uw hele Azure-tenant. Dat locatie bevindt zich in [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), als onderdeel van uw beveiligingsbeleid. Alleen gebruikers met beheerdersrechten op de Tenant root management-groep kunt met deze taak uitvoeren.

Als onderdeel van de Information Protection-beleidsbeheer, kunt u aangepaste etiketten zijn gedefinieerd, ze rangschikken en deze koppelen aan een geselecteerde set informatietypen. U kunt ook uw eigen aangepaste gegevens toevoegen en configureren met de tekenreeks patronen die zijn toegevoegd aan de detectie-logica voor het identificeren van dit type gegevens in uw databases.
Meer informatie over het aanpassen en beheren van uw beleid in de [Information Protection-beleid gebruiksaanwijzing](https://go.microsoft.com/fwlink/?linkid=2009845&clcid=0x409).

Zodra de tenant-brede, door het beleid is gedefinieerd, kunt u doorgaan met de classificatie van afzonderlijke databases met behulp van uw aangepaste beleid.

## <a name="classify-your-sql-database"></a>Classificeren van uw SQL-Database

1. Ga naar de [Azure Portal](https://portal.azure.com).

2. Navigeer naar **gegevensbeveiliging geavanceerde** onder de kop van de beveiliging in uw Azure SQL Database-deelvenster. Klik om te schakelen geavanceerde gegevensbeveiliging en klik vervolgens op de **gegevensdetectie en -classificatie (preview)** kaart.

   ![Een database controleren](./media/sql-data-discovery-and-classification/data_classification.png)

3. De **overzicht** tabblad bevat een overzicht van de huidige status van de classificatie van de database, inclusief een gedetailleerde lijst met alle geclassificeerde kolommen die u ook filteren kunt om alleen specifieke schema delen, gegevenstypen en labels weer te geven. Als u dit nog niet hebt nog geen kolommen geclassificeerd [gaat u verder met stap 5](#step-5).

   ![Overzicht van de huidige status van classificatie](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Als u wilt een rapport in de Excel-indeling downloaden, klikt u op de **exporteren** optie in het bovenste menu van het venster.

   ![Exporteren naar Excel](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png)

5. <a id="step-5"></a>Om te beginnen met uw gegevens classificeren, klikt u op de **tabblad classificatie** aan de bovenkant van het venster.

    ![U gegevens classificeren](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png)

6. De classificatie-engine van uw database voor kolommen met potentieel gevoelige gegevens scant en geeft een lijst van **kolomclassificaties aanbevolen**. Bekijken en toepassen van aanbevelingen voor classificatie:

   - De lijst van classificaties van de aanbevolen kolom wilt weergeven, klikt u op de aanbevelingen in het venster aan de onderkant van het venster:

      ![Uw gegevens classificeren](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png)

   - Bekijk de lijst met aanbevelingen – een aanbeveling voor een specifieke kolom accepteren, schakel het selectievakje in de linkerkolom van de desbetreffende rij. U kunt ook markeren *alle aanbevelingen* geaccepteerd door het controleren van het selectievakje in de kop van de aanbevelingen.

       ![Beoordeling wordt de lijst met](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

   - Als u wilt toepassen op de geselecteerde aanbevelingen, klikt u op de blauwe **geselecteerde aanbevelingen accepteren** knop.

      ![Toepassen van aanbevelingen](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png)

7. U kunt ook **handmatig classificeren** kolommen als alternatief of verder naar de classificatie op basis van een aanbeveling:

   - Klik op **classificatie toevoegen** in het bovenste menu van het venster.

      ![Classificatie handmatig toevoegen](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png)

   - In de context-upvenster dat wordt geopend, selecteert u het schema > Tabel > kolom die u wilt classificeren en het label van type en de gevoeligheid van informatie. Klik vervolgens op de blauwe **classificatie toevoegen** knop aan de onderkant van het venster context.

      ![Kolom selecteren om te classificeren](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Voor het voltooien van de classificatie en permanent label kolommen met de nieuwe classificatie-metagegevens (tag) de database, klikt u op **opslaan** in het bovenste menu van het venster.

   ![Opslaan](./media/sql-data-discovery-and-classification/10_data_classification_save.png)

## <a id="subheading-3"></a>Toegang tot gevoelige gegevens controleren

Een belangrijk aspect van het information protection paradigma is de mogelijkheid voor het bewaken van toegang tot gevoelige gegevens. [Azure SQL Database Auditing](sql-database-auditing.md) heeft uitgebreid met een nieuw veld in het auditlogboek met de naam *data_sensitivity_information*, die de classificaties gevoeligheid (labels) van de werkelijke hoeveelheid gegevens die is geretourneerd door Logboeken de query.

![Auditlogboek](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a id="subheading-4"></a>Geautomatiseerde/Programmatic classificatie

T-SQL kunt u kolomclassificaties toevoegen/verwijderen, evenals alle classificaties voor de gehele database ophalen.

> [!NOTE]
> Bij het gebruik van T-SQL voor het beheren van labels, is er geen validatie die labels toegevoegd aan een kolom in de organisatie information protection-beleid (de set van labels die worden weergegeven in de portal aanbevelingen) bestaan. Het is daarom aan u kunt dit controleren.

- De indeling van een of meer kolommen toevoegen/bijwerken: [GEVOELIGHEIDSCLASSIFICATIE TOEVOEGEN](https://docs.microsoft.com/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
- De classificatie uit een of meer kolommen verwijderen: [GEVOELIGHEIDSCLASSIFICATIE VERWIJDEREN](https://docs.microsoft.com/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
- Alle classificaties weergeven op de database: [sys.sensitivity_classifications](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

U kunt ook REST-API's gebruiken voor het programmatisch beheren van classificaties. De gepubliceerde REST-API's ondersteunen de volgende bewerkingen:

- [Maken of bijwerken](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate) - Hiermee maken of bijwerken van het gevoeligheidslabel van de van een bepaalde kolom
- [Verwijder](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete) -Hiermee verwijdert u de gevoeligheid, label van een bepaalde kolom
- [Ophalen](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get) -haalt de gevoeligheid, label van een bepaalde kolom
- [Lijst met huidige door Database](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listcurrentbydatabase) -haalt de huidige gevoeligheidslabels van een bepaalde database
- [Lijst met aanbevolen door Database](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listrecommendedbydatabase) -haalt de huidige gevoeligheidslabels van een bepaalde database

## <a id="subheading-5"></a>Volgende stappen

- Meer informatie over [geavanceerde beveiliging van gegevens](sql-database-advanced-data-security.md).
- Overweeg de configuratie van [Azure SQL Database Auditing](sql-database-auditing.md) voor bewaking en controle van toegang tot uw geclassificeerde gevoelige gegevens.

<!--Anchors-->
[SQL data discovery & classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Automated/Programmatic classification]: #subheading-4
[Next Steps]: #subheading-5
