---
title: Bedreigingendetectie - Azure SQL Data Warehouse | Microsoft Docs
description: Configureer detectie van dreigingen en verkennen verdachte gebeurtenissen in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 8dc1ef0432536c6bfd4fe069406cd057ca069ea2
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
ms.locfileid: "31523894"
---
# <a name="threat-detection-in-azure-sql-data-warehouse"></a>Detectie van dreigingen in Azure SQL Data Warehouse
Configureer detectie van dreigingen en verkennen verdachte gebeurtenissen in Azure SQL Data Warehouse.

## <a name="what-is-threat-detection"></a>Wat is de detectie van dreigingen
Met detectie van bedreigingen worden afwijkende databaseactiviteiten gedetecteerd die kunnen duiden op beveiligingsdreigingen voor de database. 

Bedreigingsdetectie biedt een nieuwe beveiligingslaag, waarmee klanten potentiële bedreigingen kunnen detecteren en erop reageren zodra ze zich voordoen, dankzij beveiligingswaarschuwingen over afwijkende activiteiten. Gebruikers kunnen de verdachte gebeurtenissen met verkennen [Azure SQL Data Warehouse Auditing](sql-data-warehouse-auditing-overview.md) om te bepalen of ze het gevolg zijn van een poging om te openen, inbreuk of misbruik van gegevens in het datawarehouse.
Detectie van dreigingen kunt u eenvoudig op mogelijke bedreigingen adres naar het datawarehouse hoeft te worden van een deskundige beveiliging of systemen bewaking van de geavanceerde beveiliging te beheren.

Zo kunt u bijvoorbeeld bepaalde afwijkende databaseactiviteiten detecteren die wijzen op mogelijke pogingen van SQL-injectie. SQL-injectie is een van de veelvoorkomende beveiligingsproblemen voor webtoepassingen op internet, en wordt gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers richten zich op de zwakke plekken van de toepassing om schadelijke SQL-instructies te injecteren in invoervelden van de toepassing om zo gegevens in de database te raadplegen of te wijzigen.

## <a name="set-up-threat-detection-for-your-database"></a>Detectie van dreigingen voor uw database instellen
1. Starten van de Azure Portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de blade van de configuratie van de SQL Data Warehouse die u wilt bewaken. Selecteer **Controle en detectie van bedreigingen** op de blade Instellingen.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png)
3. In de **controle en detectie van dreigingen** configuratie blade Schakel **ON** controle, wordt die de Threat detectie-instellingen weergegeven.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png)
4. Schakel **ON** Bedreigingendetectie.
5. Configureer de lijst met e-mailberichten die beveiligingswaarschuwingen na detectie van afwijkende activiteiten datawarehouse ontvangt.
6. Klik op **opslaan** in de **controle en detectie van bedreigingen** blade van de configuratie op te slaan de nieuwe of bijgewerkte controle dreiging van beleid.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png)

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Verken afwijkende datawarehouse activiteiten na detectie van een verdachte activiteit
1. U ontvangt een e-mailmelding na detectie van afwijkende databaseactiviteiten. <br/>
   De e-mail bevat informatie over de verdachte beveiligingsgebeurtenis, inclusief de aard van de afwijkende activiteiten, de databasenaam, de servernaam en het tijdstip van de gebeurtenis. Daarnaast bevat de e-mail informatie over mogelijke oorzaken en aanbevolen acties om de mogelijke bedreiging van de database te onderzoeken en weg te nemen.<br/>
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/4_td_email.png)
2. Klik in de e-mail op de **Azure SQL-controle logboek** koppelt, wordt die de Azure-portal te starten en de relevante records controle rond de tijd van de verdachte gebeurtenis wilt weergeven.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png)
3. Klik op de controlerecords naar meer details weergeven over de op verdachte databaseactiviteiten zoals SQL-instructie is mislukt reden en client-IP.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png)
4. Klik op de blade Controlerecords op **Openen in Excel** om een vooraf geconfigureerde Excel sjabloon te openen, zodat u een diepgaande analyse van het controlelogboek rond het tijdstip van de verdachte activiteit kunt importeren en uitvoeren.<br/>
   **Opmerking:** In Excel 2010 of hoger, Power Query en de **snel combineren** instelling is vereist
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png)
5. Als u de instelling **Snel combineren** wilt configureren, gaat u naar het tabblad **POWER QUERY** op het lint en selecteert u **Opties** om het gelijknamige dialoogvenster weer te geven. Selecteer het onderdeel Privacy en kies de tweede optie, De privacyniveaus en mogelijk verbeterde prestaties negeren:
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png)
6. Als u SQL-controlelogboeken wilt laden, zorgt u ervoor dat de parameters op het tabblad Instellingen juist zijn ingesteld en selecteert u vervolgens de knop Alles vernieuwen op het tabblad Gegevens van het lint.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png)
7. De resultaten worden weergegeven in een **afzonderlijk werkblad**, waarmee u een meer gedetailleerde analyse kunt uitvoeren van de afwijkende activiteiten die zijn gedetecteerd om zo de gevolgen van de beveiligingsgebeurtenis in uw toepassing te beperken.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over beveiliging [beveiligen van een datawarehouse](sql-data-warehouse-overview-manage-security.md).
