---
title: Aan de slag met SQL Data Warehouse met detectie van dreigingen
description: Hoe u aan de slag met detectie van dreigingen
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: c9073dd9-6c62-4735-8457-dfb9f859c900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 7f5dab6936e8cac10ac7a4a7dc4c3be116de5ad5
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="get-started-with-threat-detection"></a>Aan de slag met detectie van dreigingen
> [!div class="op_single_selector"]
> * [Controle](sql-data-warehouse-auditing-overview.md)
> * [Detectie van bedreigingen](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a>Overzicht
Detectie van dreigingen detecteert afwijkende databaseactiviteiten die wijzen op beveiligingsdreigingen voor de database. Detectie van dreigingen is Preview-versie en wordt ondersteund voor SQL Data Warehouse.

Detectie van dreigingen biedt een nieuwe laag van beveiliging, waarmee klanten om te detecteren en op mogelijke bedreigingen reageert wanneer deze zich voordoen doordat beveiligingswaarschuwingen op vreemde activiteiten worden gedetecteerd. Gebruikers kunnen de verdachte gebeurtenissen met verkennen [Azure SQL Data Warehouse Auditing](sql-data-warehouse-auditing-overview.md) om te bepalen of ze het gevolg zijn van een poging om te openen, inbreuk of misbruik van gegevens in het datawarehouse.
Detectie van dreigingen kunt u eenvoudig op mogelijke bedreigingen adres naar het datawarehouse hoeft te worden van een deskundige beveiliging of systemen bewaking van de geavanceerde beveiliging te beheren.

Detectie van dreigingen detecteert bijvoorbeeld bepaalde afwijkende databaseactiviteiten potentiële SQL-injectie pogingen die aangeeft. SQL-injectie is een van de algemene Web application beveiligingsproblemen op het Internet worden gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers te profiteren van de toepassing zwakke plekken in het injecteren schadelijke SQL-instructies in de invoervelden toepassing voor schendingen veroorzaken of wijzigen van gegevens in de database.

## <a name="set-up-threat-detection-for-your-database"></a>Detectie van dreigingen voor uw database instellen
1. Starten van de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Navigeer naar de blade van de configuratie van de SQL Data Warehouse die u wilt bewaken. Selecteer in de blade instellingen **controle en detectie van dreigingen**.
   
    ![Navigatiedeelvenster][1]
3. In de **controle en detectie van dreigingen** configuratie blade Schakel **ON** controle, wordt die de Threat detectie-instellingen weergegeven.
   
    ![Navigatiedeelvenster][2]
4. Schakel **ON** Bedreigingendetectie.
5. Configureer de lijst met e-mailberichten die beveiligingswaarschuwingen na detectie van afwijkende activiteiten datawarehouse ontvangt.
6. Klik op **opslaan** in de **controle en detectie van bedreigingen** blade van de configuratie op te slaan de nieuwe of bijgewerkte controle dreiging van beleid.
   
    ![Navigatiedeelvenster][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Verken afwijkende datawarehouse activiteiten na detectie van een verdachte activiteit
1. U ontvangt een e-mailmelding na detectie van afwijkende databaseactiviteiten. <br/>
   Het e-mailadres bevatten informatie over de verdachte-gebeurtenis met inbegrip van de aard van de afwijkende activiteiten, databasenaam, de servernaam van de en de tijd van de gebeurtenis. Bovendien bevatten informatie over mogelijke oorzaken en aanbevolen acties te onderzoeken en de mogelijke bedreiging voor de database te verminderen.<br/>
   
    ![Navigatiedeelvenster][4]
2. Klik in de e-mail op de **Azure SQL-controle logboek** koppelt, wordt die de Azure-portal te starten en de relevante records controle rond de tijd van de verdachte gebeurtenis wilt weergeven.
   
    ![Navigatiedeelvenster][5]
3. Klik op de controlerecords naar meer details weergeven over de op verdachte databaseactiviteiten zoals SQL-instructie is mislukt reden en client-IP.
   
    ![Navigatiedeelvenster][6]
4. Klik op de blade controle Records **openen in Excel** openen van een vooraf geconfigureerde excel sjabloon importeren en uitvoeren van de diepgaande analyse van het controlelogboek rond de tijd van de verdachte activiteit.<br/>
   **Opmerking:** In Excel 2010 of hoger, Power Query en de **snel combineren** instelling is vereist
   
    ![Navigatiedeelvenster][7]
5. Voor het configureren van de **snel combineren** instellen - In de **POWER QUERY** linttabblad, selecteer **opties** om het dialoogvenster opties weer te geven. Selecteer de sectie Privacy en kiest u de tweede optie - 'Negeren van de privacyniveaus en mogelijk verbeterde prestaties':
   
    ![Navigatiedeelvenster][8]
6. Als u wilt laden controlelogboeken SQL, zorg ervoor dat de parameters in de instellingen juist zijn ingesteld en selecteer vervolgens het lint 'Data' en klik op de knop Alles vernieuwen tabblad.
   
    ![Navigatiedeelvenster][9]
7. De resultaten worden weergegeven de **SQL controlelogboeken** blad waarmee u meer gedetailleerde analyse van de afwijkende activiteiten die zijn gedetecteerd uitvoeren, en het effect van de beveiligingsgebeurtenis in uw toepassing.

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
