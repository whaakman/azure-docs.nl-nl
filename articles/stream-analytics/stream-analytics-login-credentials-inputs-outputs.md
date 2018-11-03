---
title: Draaien aanmeldingsreferenties in Azure Stream Analytics-taken
description: In dit artikel wordt beschreven hoe u om bij te werken van de referenties van de invoer en uitvoer sinks in Azure Stream Analytics-taken.
services: stream-analytics
author: jasonwhowell
ms.author: mamccrea
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/11/2018
ms.openlocfilehash: 362fdca3b9a54ea0a8785ae37b32b88cbe0f67ba
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978763"
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-of-a-stream-analytics-job"></a>Draaien aanmeldingsreferenties voor invoer en uitvoer van een Stream Analytics-taak

Wanneer u referenties voor een invoer of uitvoer van een Stream Analytics-taak opnieuw wilt genereren, moet u de taak bijwerken met nieuwe referenties. U moet de taak stoppen voordat u de referenties bijwerkt, u kunt de referenties niet vervangen, terwijl de taak wordt uitgevoerd. Om te beperken de vertraging tussen het stoppen en opnieuw starten van de taak, Stream Analytics biedt ondersteuning voor hervatten van een project vanaf de laatste uitvoer. In dit onderwerp beschrijft het proces van de aanmeldingsreferenties draaien en de taak met de nieuwe referenties opnieuw te starten.

## <a name="regenerate-new-credentials-and-update-your-job-with-the-new-credentials"></a>Nieuwe referenties opnieuw wilt genereren en uw taak bijwerken met de nieuwe referenties 

In deze sectie begeleidt we u stapsgewijs door opnieuw gegenereerd referenties voor Blob-opslag, Event Hubs, SQL-Database en Table Storage. 

### <a name="blob-storagetable-storage"></a>BLOB-opslag/Table storage
1. Meld u aan bij Azure portal > Bladeren van het opslagaccount dat u als invoer/uitvoer voor de Stream Analytics-taak gebruikt.    
2. Open in de sectie instellingen **toegangssleutels**. Tussen de twee standaard-sleutels (key1, key2), die niet wordt gebruikt door uw werk kiezen en opnieuw genereren:  
   ![Sleutels voor opslagaccount opnieuw genereren](media/stream-analytics-login-credentials-inputs-outputs/image1.png)
3. Kopieer het zojuist gegenereerde sleutel.    
4. Vanuit de Azure-portal, blader uw Stream Analytics-taak > Selecteer **stoppen** en wacht totdat de taak te stoppen.    
5. Ga naar de Blob/tabel opslag i/o-waarvoor u wilt bijwerken van referenties.    
6. Zoek de **Opslagaccountsleutel** veld en plak uw zojuist gegenereerde sleutel > klikt u op **opslaan**.    
7. Een test van databaseverbinding wordt automatisch gestart wanneer u uw wijzigingen opslaat, kunt u deze op het tabblad meldingen bekijken. Er zijn twee meldingen-een komt overeen met het opslaan van de update en andere komt overeen met het testen van de verbinding:  
   ![Meldingen na het bewerken van de sleutel](media/stream-analytics-login-credentials-inputs-outputs/image4.png)
8. Gaat u verder met [uw taak starten vanaf de laatste keer dat gestopt](#start-your-job-from-the-last-stopped-time) sectie.

### <a name="event-hubs"></a>Event Hubs

1. Meld u aan bij Azure portal > Bladeren van de Event Hub die u als invoer/uitvoer voor de Stream Analytics-taak gebruikt.    
2. Open in de sectie instellingen **beleid voor gedeelde toegang** en selecteer de vereiste toegang-beleid. Tussen de **primaire sleutel** en **secundaire sleutel**, kiest u de map die niet wordt gebruikt door de taak en opnieuw genereren:  
   ![Sleutels opnieuw genereren voor Event Hub](media/stream-analytics-login-credentials-inputs-outputs/image2.png)
3. Kopieer het zojuist gegenereerde sleutel.    
4. Vanuit de Azure-portal, blader uw Stream Analytics-taak > Selecteer **stoppen** en wacht totdat de taak te stoppen.    
5. Ga naar de Event hubs invoer/uitvoer waarvoor u wilt bijwerken van referenties.    
6. Zoek de **beleidssleutel voor Event Hub** veld en plak uw zojuist gegenereerde sleutel > klikt u op **opslaan**.    
7. Een test van databaseverbinding wordt automatisch gestart wanneer u uw wijzigingen opslaat Zorg ervoor dat met succes is verstreken.    
8. Gaat u verder met [uw taak starten vanaf de laatste keer dat gestopt](#start-your-job-from-the-last-stopped-time) sectie.

### <a name="sql-database"></a>SQL Database

U moet verbinding maken met de SQL-database voor het bijwerken van de inloggegevens van een bestaande gebruiker. U kunt referenties bijwerken met behulp van Azure portal of een client-side-hulpprogramma zoals SQL Server Management Studio. In deze sectie ziet u het proces voor het bijwerken van referenties met behulp van Azure portal.

1. Meld u aan bij Azure portal > Bladeren van de SQL-database die u als uitvoer voor de Stream Analytics-taak gebruikt.    
2. Van **Data explorer**, aanmelding/verbinding maken met uw database > Selecteer autorisatietype als **SQL server-verificatie** > typt u in uw **aanmelding** en  **Wachtwoord** details > Selecteer **Ok**.  
   ![Referenties voor SQL-database opnieuw wilt genereren](media/stream-analytics-login-credentials-inputs-outputs/image3.png)

3. In de query-tabblad, wijzigt u het wachtwoord voor een van de gebruiker door de volgende query uit te voeren (Vervang `<user_name>` met uw gebruikersnaam en `<new_password>` met uw nieuwe wachtwoord):  

   ```SQL
   Alter user `<user_name>` WITH PASSWORD = '<new_password>'
   Alter role db_owner Add member `<user_name>`
   ```

4. Noteer het nieuwe wachtwoord.    
5. Vanuit de Azure-portal, blader uw Stream Analytics-taak > Selecteer **stoppen** en wacht totdat de taak te stoppen.    
6. Ga naar de uitvoer van SQL-database waarvoor u wenst te roteren. Het wachtwoord bijwerken en opslaan van wijzigingen.    
7. Een test van databaseverbinding wordt automatisch gestart wanneer u uw wijzigingen opslaat Zorg ervoor dat met succes is verstreken.    
8. Gaat u verder met [uw taak starten vanaf de laatste keer dat gestopt](#start-your-job-from-the-last-stopped-time) sectie.

### <a name="power-bi"></a>Power BI
1. Meld u aan bij Azure portal > uw Stream Analytics-taak Bladeren > Selecteer **stoppen** en wacht totdat de taak te stoppen.    
2. Ga naar de Power BI-uitvoer die u wilt vernieuwen referenties > Klik op de **autorisatie vernieuwen** (u ziet een bericht weergegeven) > **opslaan** de wijzigingen.    
3. Een test van databaseverbinding wordt automatisch gestart wanneer u uw wijzigingen opslaat, zorg ervoor dat deze is verstreken.    
4. Gaat u verder met [uw taak starten vanaf de laatste keer dat gestopt](#start-your-job-from-the-last-stopped-time) sectie.

## <a name="start-your-job-from-the-last-stopped-time"></a>Uw taak starten van de laatste keer is gestopt

1. Navigeer naar een van de taak **overzicht** deelvenster > Selecteer **Start** om de taak te starten.    
2. Selecteer **laatste tijd geëindigd** > klikt u op **Start**. Houd er rekening mee dat de optie 'als laatste is gestopt' wordt alleen weergegeven als u eerder de taak is uitgevoerd en sommige uitvoer gegenereerd. De taak opnieuw wordt gestart op basis van de laatste uitvoerwaarde tijd.
   ![De taak starten](media/stream-analytics-login-credentials-inputs-outputs/image5.png)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
