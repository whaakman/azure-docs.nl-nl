---
title: Azure SQL Database-gegevens detectie & classificatie | Microsoft Docs
description: Azure SQL Database-gegevens detectie & classificatie
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: giladm
ms.openlocfilehash: 673286c8dc9ec688199fe80cf5a763f249192de5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646776"
---
# <a name="azure-sql-database-data-discovery-and-classification"></a>Azure SQL Database-gegevens voor het opsporen en classificatie
Detectie van de gegevens & classificatie (momenteel in preview) biedt geavanceerde mogelijkheden die zijn ingebouwd in Azure SQL Database voor **detecteren**, **classificeren**, **labels**  &  **beveiligen** de gevoelige gegevens in uw databases.
Detecteren en classificeren van uw meest gevoelige gegevens (business, financiële, gezondheidszorg, PII, enz.) kan een belangrijke rol spelen in uw organisatiegegevens protection-status. Deze kan dienen als infrastructuur voor:
* Helpt voldoen aan gegevens privacystandaarden en regelgeving.
* Verschillende security-scenario's, zoals controle (controleren) en waarschuwingen voor afwijkende toegang tot gevoelige gegevens.
* Toegang tot beheren en beperken van de beveiliging van databases met uiterst gevoelige gegevens.

Detectie van de gegevens & classificatie maakt deel uit van de [SQL geavanceerde Threat Protection](sql-advanced-threat-protection.md) (ATP)-oplossing, dit een uniform pakket voor geavanceerde mogelijkheden voor SQL-beveiliging is. Detectie van de gegevens & classificatie kan worden geopend en worden beheerd via de centrale SQL ATP-portal.

> [!NOTE]
> Dit document heeft alleen betrekking op Azure SQL Database. Zie voor SQL Server (on-premises) [SQL gegevens te zoeken en classificatie](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Wat is gegevens te zoeken en classificatie?
Detectie van de gegevens & classificatie introduceert een aantal geavanceerde services en nieuwe SQL-mogelijkheden, vormen een nieuwe SQL-gegevensbeveiliging paradigma die gericht is op het beveiligen van de gegevens, niet alleen de database:
* **Detectie van & aanbevelingen** – de classificatie-engine doorzoekt uw database en identificeert de kolommen die potentieel gevoelige gegevens bevat. Vervolgens wordt u een eenvoudige manier om te bekijken en toepassen van de juiste classificatie aanbevelingen via de Azure-portal.
* **Labels** – classificatielabels gevoeligheid permanent worden gelabeld voor kolommen met nieuwe classificatie metagegevenskenmerken die zijn geïntroduceerd in de SQL-Engine. Deze metagegevens kan vervolgens worden gebruikt voor geavanceerde gevoeligheid gebaseerde controle en beveiliging scenario's.
* **Gevoeligheid queryresultaatset** : de gevoeligheid van de queryresultatenset in realtime voor controledoeleinden wordt berekend.
* **Zichtbaarheid** -status van de database-indeling kan worden weergegeven in een gedetailleerde dashboard in de portal. Bovendien kunt u een rapport (in Excel-indeling) downloaden om te worden gebruikt voor naleving & controledoeleinden, evenals andere behoeften.

## <a id="subheading-2"></a>Detecteren, classificeren en labelen gevoelige kolommen
De volgende sectie worden de stappen beschreven voor het detecteren, classificeren en labels van kolommen met gevoelige gegevens in uw database, evenals de huidige status van de classificatie van uw database weergeven en exporteren van rapporten.

De classificatie bevat twee metagegevenskenmerken:
* Labels – de kenmerken hoofdcategorie gebruikt voor het definiëren van het gevoeligheidsniveau van de gegevens die zijn opgeslagen in de kolom.  
* Gegevenstypen – bieden aanvullende granulatie in het type van de gegevens die zijn opgeslagen in de kolom.

## <a name="classify-your-sql-database"></a>Classificeren van de SQL-Database

1. Ga naar de [Azure Portal](https://portal.azure.com).

2. Navigeer naar **Advanced Threat Protection** onder de kop van de beveiliging in uw Azure SQL Database-deelvenster. Klik om te schakelen Advanced Threat Protection en klik vervolgens op de **gegevens detectie & classificatie (preview)** kaart.

   ![Scannen van een database](./media/sql-data-discovery-and-classification/data_classification.png) 

3. De **overzicht** tabblad bevat een overzicht van de huidige status van de classificatie van de database, inclusief een gedetailleerde lijst met alle geclassificeerde kolommen die u ook filteren kunt om alleen specifieke schema delen, gegevenstypen en labels weer te geven. Als u dit nog niet hebt nog geen kolommen geclassificeerd [verder met stap 5](#step-5).

   ![Overzicht van de huidige status van classificatie](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png) 

4. Voor het downloaden van een rapport in Excel-indeling, klikt u op de **exporteren** optie in het bovenste menu van het venster.

   ![Exporteren naar Excel](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png) 

5.  <a id="step-5"></a>Om te beginnen met uw gegevens classificeren, klikt u op de **tabblad classificatie** aan de bovenkant van het venster.

    ![U gegevens classificeren](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png) 

6. De classificatie-engine doorzoekt uw database voor kolommen die potentieel gevoelige gegevens en biedt een lijst met **aanbevolen kolom classificaties**. Bekijken en toepassen van aanbevelingen voor classificatie:

    * De lijst van classificaties aanbevolen kolom wilt weergeven, klikt u op in het deelvenster aanbevelingen aan de onderkant van het venster:
    
      ![Uw gegevens classificeren](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png) 

    * Bekijk de lijst met aanbevelingen: voor het accepteren van een aanbeveling voor een specifieke kolom, schakel het selectievakje in de linkerkolom van de desbetreffende rij. U kunt ook markeren *alle aanbevelingen* zoals geaccepteerd door het selectievakje in de tabelkop aanbevelingen.

       ![Bekijk aanbeveling lijst](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png) 

    * Als u wilt toepassen op de geselecteerde aanbevelingen, klikt u op de blauwe **geselecteerde aanbevelingen accepteren** knop.

      ![Toepassen van aanbevelingen](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png) 

7. U kunt ook **handmatig classificeren** kolommen als een alternatief of bovendien met de classificatie op basis van een aanbeveling:

    * Klik op **classificatie toevoegen** in het menu van het venster.
  
      ![Handmatig toevoegen van classificatie](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png) 

    * In de context-venster dat wordt geopend, selecteert u het schema > Tabel > kolom die u wilt classificeren en het label van type en de gevoeligheid van informatie. Klik vervolgens op de blauwe **classificatie toevoegen** knop onderaan in het venster context.

      ![Selecteer de kolom te classificeren](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png) 

8. Voor het voltooien van de classificatie en permanent label kolommen (tag) de database met de nieuwe classificatie-metagegevens, klikt u op **opslaan** in het menu van het venster.

   ![Opslaan](./media/sql-data-discovery-and-classification/10_data_classification_save.png) 

## <a id="subheading-3"></a>Toegang tot gevoelige gegevens controleren

Een belangrijk aspect van het information protection paradigma is de mogelijkheid voor het bewaken van toegang tot gevoelige gegevens. [Azure SQL Database Auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) is uitgebreid met een nieuw veld in het controlelogboek aangeroepen *data_sensitivity_information*, die de classificaties gevoeligheid (labels) van de werkelijke hoeveelheid gegevens die is geretourneerd door Logboeken de query.

![Auditlogboek](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png) 

## <a id="subheading-4"></a>Volgende stappen

- Meer informatie over [SQL Advanced Threat Protection](sql-advanced-threat-protection.md).
- Houd rekening met het configureren van [Azure SQL Database Auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) voor bewaking en controle van toegang tot uw geclassificeerde gevoelige gegevens.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Next Steps]: #subheading-4