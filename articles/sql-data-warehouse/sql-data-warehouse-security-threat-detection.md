---
title: Detectie van bedreigingen - Azure SQL Data Warehouse | Microsoft Docs
description: Configureer detectie van bedreigingen en verdachte gebeurtenissen in Azure SQL Data Warehouse verkennen.
services: sql-data-warehouse
author: kavithaj
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 7ff23235e9681301984e13e346b23f277662bb5c
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43300931"
---
# <a name="threat-detection-in-azure-sql-data-warehouse"></a>Detectie van bedreigingen in Azure SQL Data Warehouse
Configureer detectie van bedreigingen en verdachte gebeurtenissen in Azure SQL Data Warehouse verkennen.

## <a name="what-is-threat-detection"></a>Wat is de detectie van bedreigingen
Threat Detection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database. 

Bedreigingsdetectie biedt een nieuwe beveiligingslaag, waarmee klanten potentiële bedreigingen kunnen detecteren en erop reageren zodra ze zich voordoen, dankzij beveiligingswaarschuwingen over afwijkende activiteiten. Gebruikers kunnen de verdachte gebeurtenissen met onderzoeken [Azure SQL Data Warehouse Auditing](sql-data-warehouse-auditing-overview.md) om te bepalen of die het gevolg van een poging tot toegang tot, of misbruik te maken gegevens in het datawarehouse.
Detectie van bedreigingen is het eenvoudig om potentiële bedreigingen voor het datawarehouse zonder de noodzaak om te worden van een beveiligingsexpert of het beheren van geavanceerde bewakingssystemen.

Zo kunt u bijvoorbeeld bepaalde afwijkende databaseactiviteiten detecteren die wijzen op mogelijke pogingen van SQL-injectie. SQL-injectie is een van de veelvoorkomende beveiligingsproblemen voor webtoepassingen op internet, en wordt gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers richten zich op de zwakke plekken van de toepassing om schadelijke SQL-instructies te injecteren in invoervelden van de toepassing om zo gegevens in de database te raadplegen of te wijzigen.

## <a name="set-up-threat-detection-for-your-database"></a>Detectie van bedreigingen voor uw database instellen
1. Starten van de Azure Portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de configuratieblade van de SQL Data Warehouse die u wilt bewaken. Selecteer **Controle en detectie van bedreigingen** op de blade Instellingen.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png)
3. In de **controle en detectie van bedreigingen** zet **ON** controle, worden die instellingen voor Bedreigingsdetectie weergegeven.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png)
4. Schakel **ON** detectie van bedreigingen.
5. Configureer de lijst met e-mailberichten die beveiligingswaarschuwingen tijdens de detectie van afwijkend datawarehouse-activiteiten ontvangt.
6. Klik op **opslaan** in de **controle en detectie van bedreigingen** configuratieblade op te slaan de nieuwe of bijgewerkte controle en beleid voor detectie van bedreigingen.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png)

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Afwijkende datawarehouse activiteiten tijdens de detectie van een verdachte activiteit verkennen
1. U ontvangt een e-mailmelding bij detectie van afwijkende activiteiten. <br/>
   De e-mail bevat informatie over de verdachte beveiligingsgebeurtenis, inclusief de aard van de afwijkende activiteiten, de databasenaam, de servernaam en het tijdstip van de gebeurtenis. Daarnaast bevat de e-mail informatie over mogelijke oorzaken en aanbevolen acties om de mogelijke bedreiging van de database te onderzoeken en weg te nemen.<br/>
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/4_td_email.png)
2. Klik in het e-mailbericht, op de **Azure SQL-controle logboek** koppeling, die wordt starten van de Azure portal en de relevante controle records rond de tijd van de verdachte activiteit weer te geven.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png)
3. Klik op de controlerecords voor meer informatie over de verdachte databaseactiviteiten, zoals SQL-instructie, fout reden en client-IP.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png)
4. Klik op de blade Controlerecords op **Openen in Excel** om een vooraf geconfigureerde Excel sjabloon te openen, zodat u een diepgaande analyse van het controlelogboek rond het tijdstip van de verdachte activiteit kunt importeren en uitvoeren.<br/>
   **Opmerking:** In Excel 2010 of hoger zijn Power Query en de **snel combineren** instelling is vereist
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png)
5. Als u de instelling **Snel combineren** wilt configureren, gaat u naar het tabblad **POWER QUERY** op het lint en selecteert u **Opties** om het gelijknamige dialoogvenster weer te geven. Selecteer het onderdeel Privacy en kies de tweede optie, De privacyniveaus en mogelijk verbeterde prestaties negeren:
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png)
6. Als u SQL-controlelogboeken wilt laden, zorgt u ervoor dat de parameters op het tabblad Instellingen juist zijn ingesteld en selecteert u vervolgens de knop Alles vernieuwen op het tabblad Gegevens van het lint.
   
    ![Navigatievenster](media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png)
7. De resultaten worden weergegeven in een **afzonderlijk werkblad**, waarmee u een meer gedetailleerde analyse kunt uitvoeren van de afwijkende activiteiten die zijn gedetecteerd om zo de gevolgen van de beveiligingsgebeurtenis in uw toepassing te beperken.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over beveiliging [beveiligen van een datawarehouse](sql-data-warehouse-overview-manage-security.md).
