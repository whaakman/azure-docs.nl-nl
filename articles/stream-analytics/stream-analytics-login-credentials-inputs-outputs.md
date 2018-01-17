---
title: 'Stream Analytics: Draaien aanmeldingsreferenties voor in- en uitgangen | Microsoft Docs'
description: Informatie over het bijwerken van de referenties voor Stream Analytics-invoer en uitvoer.
keywords: aanmeldingsreferenties
services: stream-analytics
documentationcenter: 
author: SnehaGunda
manager: kfile
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/11/2018
ms.author: sngun
ms.openlocfilehash: c1aded8fefc7b56acd2e9ff36bb2c9641665db76
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-of-a-stream-analytics-job"></a>Draaien aanmeldingsreferenties voor de invoer en uitvoer van een Stream Analytics-taak

Wanneer u referenties voor een invoer of uitvoer van een Stream Analytics-taak genereren, moet u de taak met de nieuwe referenties bijwerken. U moet de taak stoppen voordat u de referenties bijwerkt, u kunt de referenties niet vervangen, terwijl de taak wordt uitgevoerd. Als u de vertraging tussen het stoppen en opnieuw starten van de taak, Stream Analytics ondersteunt een taak in de laatste uitvoer wordt hervat. Dit onderwerp beschrijft het proces van het roteren van de aanmeldingsreferenties en opnieuw starten van de taak met de nieuwe referenties.

## <a name="regenerate-new-credentials-and-update-your-job-with-the-new-credentials"></a>Opnieuw genereren van nieuwe referenties en het bijwerken van de taak met de nieuwe referenties 

In deze sectie begeleidt we u stapsgewijs door referenties opnieuw genereren voor Blob Storage, Event Hubs, SQL-Database en Table Storage. 

### <a name="blob-storagetable-storage"></a>BLOB storage-/ tabelnaam-opslag
1. Meld u aan bij de Azure portal > Bladeren van het opslagaccount dat u als invoer/uitvoer voor de Stream Analytics-taak gebruikt.    
2. Open in de sectie instellingen **toegangssleutels**. Tussen de twee standaard-sleutels (key1, key2), die niet wordt gebruikt door uw werk kiezen en opnieuw genereren:  
   ![Opnieuw genereren van sleutels voor opslagaccount](media/stream-analytics-login-credentials-inputs-outputs/image1.png)
3. Kopieer het zojuist gegenereerde sleutel.    
4. Bladeren vanuit de Azure-portal uw Stream Analytics-taak > Selecteer **stoppen** en wacht totdat de taak te stoppen.    
5. Zoek de Blobtabel input/output op opslag waarvan u wilt referenties bijwerken.    
6. Zoek de **Opslagaccountsleutel** veld en plak uw zojuist gegenereerde sleutel > klikt u op **opslaan**.    
7. Een verbindingstest wordt automatisch gestart wanneer u uw wijzigingen hebt opgeslagen, kunt u deze bekijken op het tabblad meldingen. Er zijn twee meldingen-een komt overeen met het opslaan van de update en andere komt overeen met de verbinding testen:  
   ![Meldingen na het bewerken van de sleutel](media/stream-analytics-login-credentials-inputs-outputs/image4.png)
8. [Uw taak starten vanaf de laatste keer dat gestopt] doorgaan (#start-your-job-from-the-last-stopped-time) sectie.

### <a name="event-hubs"></a>Event Hubs

1. Meld u aan bij de Azure portal > Bladeren de Event Hub die u als invoer/uitvoer voor de Stream Analytics-taak gebruikt.    
2. Open in de sectie instellingen **gedeeld toegangsbeleid** en selecteer het beleid vereiste toegang. Tussen de **primaire sleutel** en **secundaire sleutel**, kiest u die niet wordt gebruikt door uw werk en opnieuw genereren:  
   ![Opnieuw genereren van sleutels voor Event Hub](media/stream-analytics-login-credentials-inputs-outputs/image2.png)
3. Kopieer het zojuist gegenereerde sleutel.    
4. Bladeren vanuit de Azure-portal uw Stream Analytics-taak > Selecteer **stoppen** en wacht totdat de taak te stoppen.    
5. Ga naar de Event hubs input/output op waarvoor u wilt bijwerken van referenties.    
6. Zoek de **Event Hub beleidssleutel** veld en plak uw zojuist gegenereerde sleutel > klikt u op **opslaan**.    
7. Een verbindingstest wordt automatisch gestart wanneer u de wijzigingen opslaan, zorg ervoor dat met succes is verstreken.    
8. Ga door naar [uw taak starten vanaf de laatste keer dat gestopt](#start-your-job-from-the-last-stopped-time) sectie.

### <a name="sql-database"></a>SQL Database

U moet verbinding maken met de SQL-database bijwerken van de aanmeldingsreferenties van een bestaande gebruiker. U kunt referenties bijwerken met behulp van Azure-portal of een client-side '-hulpprogramma zoals SQL Server Management Studio. Deze sectie wordt gedemonstreerd dat het proces voor het bijwerken van referenties met behulp van Azure portal.

1. Meld u aan bij de Azure portal > Bladeren van de SQL-database die u als uitvoer voor de Stream Analytics-taak gebruikt.    
2. Van **Gegevensverkenner**, aanmelding/verbinding maken met uw database > Selecteer autorisatie type als **SQL server-verificatie** > typt u in uw **aanmelding** en  **Wachtwoord** details > Selecteer **Ok**.  
   ![Referenties voor SQL-database opnieuw genereren](media/stream-analytics-login-credentials-inputs-outputs/image3.png)

3. In het tabblad query wijzigt u het wachtwoord voor een van de gebruiker door de volgende query uit te voeren (Vervang `<user_name>` met uw gebruikersnaam en `<new_password>` met uw nieuwe wachtwoord):  

   ```SQL
   Alter user `<user_name>` WITH PASSWORD = '<new_password>'
   Alter role db_owner Add member `<user_name>`
   ```

4. Noteer het nieuwe wachtwoord.    
5. Bladeren vanuit de Azure-portal uw Stream Analytics-taak > Selecteer **stoppen** en wacht totdat de taak te stoppen.    
6. Zoek de uitvoer van de SQL-database waarvoor u wilt draaien referenties. Werk het wachtwoord en de wijzigingen opslaan.    
7. Een verbindingstest wordt automatisch gestart wanneer u de wijzigingen opslaan, zorg ervoor dat met succes is verstreken.    
8. Ga door naar [uw taak starten vanaf de laatste keer dat gestopt](#start-your-job-from-the-last-stopped-time) sectie.

### <a name="power-bi"></a>Power BI
1. Meld u aan bij de Azure portal > uw Stream Analytics-taak Bladeren > Selecteer **stoppen** en wacht totdat de taak te stoppen.    
2. Zoek de waarvoor u wilt vernieuwen referenties Power BI-uitvoer > Klik op de **vernieuwen autorisatie** (u ziet een bericht) > **opslaan** de wijzigingen.    
3. Een verbindingstest wordt automatisch gestart wanneer u uw wijzigingen hebt opgeslagen, zorg ervoor dat het heeft doorstaan.    
4. Ga door naar [uw taak starten vanaf de laatste keer dat gestopt](#start-your-job-from-the-last-stopped-time) sectie.

## <a name="start-your-job-from-the-last-stopped-time"></a>De taak starten vanaf de laatste keer dat gestopt

1. Navigeer naar de taak **overzicht** deelvenster > Selecteer **Start** om de taak te starten.    
2. Selecteer **als laatste gestopt** > klikt u op **Start**. Let op de optie 'als laatste is gestopt' wordt alleen weergegeven als u eerder de taak is uitgevoerd en een uitvoer heeft gegenereerd. De taak opnieuw wordt gestart op basis van de laatste uitvoerwaarde tijd.
   ![Start de taak](media/stream-analytics-login-credentials-inputs-outputs/image5.png)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
