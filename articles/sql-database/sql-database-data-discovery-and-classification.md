---
title: Azure SQL Database-gegevens detectie & classificatie | Microsoft Docs
description: Azure SQL Database-gegevens detectie & classificatie
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 01/29/2018
ms.author: giladm
ms.openlocfilehash: 375142b0e55c741e6ab914e969751833f989d2fb
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="azure-sql-database-data-discovery-and-classification"></a>Azure SQL Database-gegevens voor het opsporen en classificatie
Detectie van de gegevens & classificatie (momenteel in preview) biedt geavanceerde mogelijkheden die zijn ingebouwd in Azure SQL Database voor **detecteren**, **classificeren**, **labels**  &  **beveiligen** de gevoelige gegevens in uw databases.
Detecteren en classificeren van uw meest gevoelige gegevens (business, financiële, gezondheidszorg, PII, enz.) kan een belangrijke rol spelen in uw organisatiegegevens protection-status. Deze kan dienen als infrastructuur voor:
* Helpt voldoen aan vereisten van de naleving van regelgeving, zoals GDPR en gegevens privacystandaarden.
* Verschillende security-scenario's, zoals controle (controleren) en waarschuwingen voor afwijkende toegang tot gevoelige gegevens.
* Toegang tot beheren en beperken van de beveiliging van databases met uiterst gevoelige gegevens.

> [!NOTE]
> Dit document heeft alleen betrekking op Azure SQL Database. Zie voor SQL Server (on-premises) [SQL gegevens te zoeken en classificatie](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Overzicht
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

<br>
**Voor het classificeren van de SQL-Database:**

1. Ga naar de [Azure Portal](https://portal.azure.com).

2. Navigeer naar de **gegevens detectie & classificatie (preview)** instellen in de SQL-Database.

    ![Navigatievenster][1]

3. De **overzicht** tabblad bevat een overzicht van de huidige status van de classificatie van de database, inclusief een gedetailleerde lijst met alle geclassificeerde kolommen die u ook filteren kunt om alleen specifieke schema delen, gegevenstypen en labels weer te geven. Als u dit nog niet hebt nog geen kolommen geclassificeerd [verder met stap 5](#step-5).

    ![Navigatievenster][2]

4. Voor het downloaden van een rapport in Excel-indeling, klikt u op de **exporteren** optie in het bovenste menu van het venster.

    ![Navigatievenster][3]

5.  <a id="step-5"></a>Om te beginnen met uw gegevens classificeren, klikt u op de **tabblad classificatie** aan de bovenkant van het venster.

    ![Navigatievenster][4]

6. De classificatie-engine doorzoekt uw database voor kolommen die potentieel gevoelige gegevens en biedt een lijst met **aanbevolen kolom classificaties**. Bekijken en toepassen van aanbevelingen voor classificatie:

    * De lijst van classificaties aanbevolen kolom wilt weergeven, klikt u op in het deelvenster aanbevelingen aan de onderkant van het venster:

        ![Navigatievenster][5]

    * Bekijk de lijst met aanbevelingen: voor het accepteren van een aanbeveling voor een specifieke kolom, schakel het selectievakje in de linkerkolom van de desbetreffende rij. U kunt ook markeren *alle aanbevelingen* zoals geaccepteerd door het selectievakje in de tabelkop aanbevelingen.

        ![Navigatievenster][6]

    * Als u wilt toepassen op de geselecteerde aanbevelingen, klikt u op de blauwe **geselecteerde aanbevelingen accepteren** knop.

        ![Navigatievenster][7]

7. U kunt ook **handmatig classificeren** kolommen als een alternatief of bovendien met de classificatie op basis van een aanbeveling:

    * Klik op **classificatie toevoegen** in het menu van het venster.

        ![Navigatievenster][8]

    * In de context-venster dat wordt geopend, selecteert u het schema > Tabel > kolom die u wilt classificeren en het label van type en de gevoeligheid van informatie. Klik vervolgens op de blauwe **classificatie toevoegen** knop onderaan in het venster context.

        ![Navigatievenster][9]

8. Voor het voltooien van de classificatie en permanent label kolommen (tag) de database met de nieuwe classificatie-metagegevens, klikt u op **opslaan** in het menu van het venster.

    ![Navigatievenster][10]

## <a id="subheading-3"></a>Toegang tot gevoelige gegevens controleren

Een belangrijk aspect van het information protection paradigma is de mogelijkheid voor het bewaken van toegang tot gevoelige gegevens.

[Azure SQL Database Auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) is uitgebreid met een nieuw veld in het controlelogboek aangeroepen *data_sensitivity_information*, die de classificaties gevoeligheid (labels) van de werkelijke hoeveelheid gegevens die is geretourneerd door Logboeken de query.

![Navigatievenster][11]

## <a id="subheading-4"></a>Volgende stappen
Houd rekening met het configureren van [Azure SQL Database Auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) voor bewaking en controle van toegang tot uw geclassificeerde gevoelige gegevens.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Next Steps]: #subheading-4

<!--Image references-->
[1]: ./media/sql-data-discovery-and-classification/1_data_classification_settings_menu.png
[2]: ./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png
[3]: ./media/sql-data-discovery-and-classification/3_data_classification_export_report.png
[4]: ./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png
[5]: ./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png
[6]: ./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png
[7]: ./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png
[8]: ./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png
[9]: ./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png
[10]: ./media/sql-data-discovery-and-classification/10_data_classification_save.png
[11]: ./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png
