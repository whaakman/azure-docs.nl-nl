---
title: 'Stream Analytics: Aanmelden referenties voor de invoer en uitvoer draaien | Microsoft Docs'
description: Informatie over het bijwerken van de referenties voor Stream Analytics-invoer en uitvoer.
keywords: aanmeldingsreferenties
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: a1a927fa9c34b38e54fdb22782e80fd13bf430c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Draaien aanmeldingsreferenties voor in- en uitgangen in Stream Analytics-taken
## <a name="abstract"></a>Abstracte
Azure Stream Analytics toegestaan vandaag nog niet in de referenties op een i/o vervangen terwijl de taak wordt uitgevoerd.

Terwijl Azure Stream Analytics biedt ondersteuning voor het hervatten van een taak van laatste uitvoer, die we wilden delen van het gehele proces voor het minimaliseren van de vertraging tussen het stoppen en starten van de taak en roteren van de aanmeldingsreferenties.

## <a name="part-1---prepare-the-new-set-of-credentials"></a>Deel 1: voorbereiden van de nieuwe set referenties:
Dit onderdeel is van toepassing op de volgende invoer/uitvoer:

* Blob Storage
* Event Hubs
* SQL Database
* Table Storage

Voor andere invoer/uitvoer, gaat u verder met deel 2.

### <a name="blob-storagetable-storage"></a>BLOB storage-/ tabelnaam-opslag
1. Ga naar de extensie van de opslag op de Azure Management portal:  
   ![graphic1][graphic1]
2. Zoek naar de opslag die wordt gebruikt door de taak en gaat u in de App:  
   ![graphic2][graphic2]
3. Klik op de opdracht toegangssleutels beheren:  
   ![graphic3][graphic3]
4. Tussen de primaire toegangssleutel en de secundaire toegangssleutel **kiest u het niet wordt gebruikt door uw taak**.
5. Klik op opnieuw genereren:  
   ![graphic4][graphic4]
6. Kopieer het zojuist gegenereerde sleutel:  
   ![graphic5][graphic5]
7. Deel 2 blijven.

### <a name="event-hubs"></a>Event hubs
1. Ga naar de Service Bus-uitbreiding op de Azure Management portal:  
   ![graphic6][graphic6]
2. Zoek naar de Service Bus-Namespace gebruikt door de taak en gaat u in de App:  
   ![graphic7][graphic7]
3. Als uw taak gebruikmaakt van een gedeeld toegangsbeleid op de Service Bus-Namespace, Ga naar stap 6  
4. Ga naar het tabblad Event Hubs:  
   ![graphic8][graphic8]
5. Zoek naar de Event Hub gebruikt door de taak en gaat u in de App:  
   ![graphic9][graphic9]
6. Ga naar het tabblad configureren:  
   ![graphic10][graphic10]
7. Zoek het beleid voor gedeelde toegang gebruikt door de taak op de vervolgkeuzelijst de naam van beleid:  
   ![graphic11][graphic11]
8. Tussen de primaire sleutel en de secundaire sleutel **kiest u het niet wordt gebruikt door uw taak**.  
9. Klik op opnieuw genereren:  
   ![graphic12][graphic12]
10. Kopieer het zojuist gegenereerde sleutel:  
   ![graphic13][graphic13]
11. Deel 2 blijven.  

### <a name="sql-database"></a>SQL Database
> [!NOTE]
> Opmerking: u moet verbinding maken met de SQL Database-Service. We gaan laten zien hoe u dit doen met behulp van de beheerervaring op de Azure Management portal, maar u kunt sommige clientzijde hulpprogramma ook zoals SQL Server Management Studio te gebruiken.
>
> 

1. Ga naar de extensie van de SQL-Databases op de Azure Management portal:  
   ![graphic14][graphic14]
2. Zoek de SQL-Database die wordt gebruikt door uw werk en **Klik op de server** koppeling op dezelfde regel:  
   ![graphic15][graphic15]
3. Klik op de opdracht beheren:  
   ![graphic16][graphic16]
4. Type Database Master:  
   ![graphic17][graphic17]
5. Typ uw gebruikersnaam, wachtwoord, en klik op logboek op:  
   ![graphic18][graphic18]
6. Klik op nieuwe Query:  
   ![graphic19][graphic19]
7. Type in de volgende query waarbij < login_name > vervangt door uw gebruikersnaam en vervangen <enterStrongPasswordHere> met uw nieuwe wachtwoord:  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. Klik op uitvoeren:  
   ![graphic20][graphic20]
9. Ga terug naar stap 2 en deze keer klikt u op de database:  
   ![graphic21][graphic21]
10. Klik op de opdracht beheren:  
   ![graphic22][graphic22]
11. Typ uw gebruikersnaam, wachtwoord, en klik op aanmelden:  
   ![graphic23][graphic23]
12. Klik op nieuwe Query:  
   ![graphic24][graphic24]
13. Typ in de volgende query waarbij < gebruikersnaam > vervangt door een naam waarmee u deze aanmelding in de context van deze database wilt (u kunt u < login_name > bijvoorbeeld heeft dezelfde waarde opgeven) te identificeren en vervangen < login_name > met uw nieuwe gebruikersnaam:  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. Klik op uitvoeren:  
   ![graphic25][graphic25]
15. Bieden nu de nieuwe gebruiker met dezelfde functies en de oorspronkelijke gebruiker heeft bevoegdheden.
16. Deel 2 blijven.

## <a name="part-2-stopping-the-stream-analytics-job"></a>Deel 2: De Stream Analytics-taak stoppen
1. Ga naar de Stream Analytics-uitbreiding op de Azure Management portal:  
   ![graphic26][graphic26]
2. Zoek uw taak en gaat u in de App:  
   ![graphic27][graphic27]
3. Ga naar het tabblad invoer of de uitvoer op basis van of het roteren van de referenties op invoer of uitvoer.  
   ![graphic28][graphic28]
4. Klik op de opdracht Stop en Bevestig dat de taak is gestopt:  
   ![graphic29][graphic29] wacht u totdat de taak te stoppen.
5. Zoek de invoer/uitvoer die u wilt draaien referenties op en gaat u in de App:  
   ![graphic30][graphic30]
6. Ga naar deel 3.

## <a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Deel 3: De referenties op de Stream Analytics-taak bewerken
### <a name="blob-storagetable-storage"></a>BLOB storage-/ tabelnaam-opslag
1. Zoek het sleutelveld van Storage-Account en uw zojuist gegenereerde sleutel plakken:  
   ![graphic31][graphic31]
2. Klik op de opdracht opslaan en Bevestig de wijzigingen worden opgeslagen:  
   ![graphic32][graphic32]
3. Een verbindingstest wordt automatisch gestart wanneer u de wijzigingen opslaan, zorg ervoor dat geslaagd is.
4. Ga naar deel 4.

### <a name="event-hubs"></a>Event hubs
1. Zoek het sleutelveld Event Hub-beleid en uw zojuist gegenereerde sleutel plakken:  
   ![graphic33][graphic33]
2. Klik op de opdracht opslaan en Bevestig de wijzigingen worden opgeslagen:  
   ![graphic34][graphic34]
3. Een verbindingstest wordt automatisch gestart wanneer u de wijzigingen opslaan, zorg ervoor dat met succes is verstreken.
4. Ga naar deel 4.

### <a name="power-bi"></a>Power BI
1. Klik op de autorisatie vernieuwen:  

   ![graphic35][graphic35]
2. U krijgt de bevestiging van het volgende:  

   ![graphic36][graphic36]
3. Klik op de opdracht opslaan en Bevestig de wijzigingen worden opgeslagen:  
   ![graphic37][graphic37]
4. Een verbindingstest wordt automatisch gestart wanneer u uw wijzigingen hebt opgeslagen, zorg ervoor dat het heeft doorstaan.
5. Ga naar deel 4.

### <a name="sql-database"></a>SQL Database
1. De velden gebruikersnaam en wachtwoord te vinden en plak de zojuist gemaakte set referenties in deze:  
   ![graphic38][graphic38]
2. Klik op de opdracht opslaan en Bevestig de wijzigingen worden opgeslagen:  
   ![graphic39][graphic39]
3. Een verbindingstest wordt automatisch gestart wanneer u de wijzigingen opslaan, zorg ervoor dat met succes is verstreken.  
4. Ga naar deel 4.

## <a name="part-4-starting-your-job-from-last-stopped-time"></a>Deel 4: De taak starten vanuit de tijd van laatste gestopt
1. De invoer/uitvoer verlaat:  
   ![graphic40][graphic40]
2. Klik op de opdracht Start:  
   ![graphic41][graphic41]
3. Kies het laatst is gestopt, en klik op OK:  
   ![graphic42][graphic42]
4. Ga naar deel 5.  

## <a name="part-5-removing-the-old-set-of-credentials"></a>Deel 5: De oude set referenties verwijderen
Dit onderdeel is van toepassing op de volgende invoer/uitvoer:

* Blob Storage
* Event Hubs
* SQL Database
* Table Storage

### <a name="blob-storagetable-storage"></a>BLOB storage-/ tabelnaam-opslag
Herhaal Part 1 voor de toegangssleutel die eerder werd gebruikt door de taak voor het vernieuwen van de toegangssleutel nu niet worden gebruikt.

### <a name="event-hubs"></a>Event hubs
Herhaal Part 1 voor de sleutel die eerder werd gebruikt door de taak voor het vernieuwen van de sleutel nu niet worden gebruikt.

### <a name="sql-database"></a>SQL Database
1. Ga terug naar het queryvenster van deel 1 stap 7 en typt u de volgende query waarbij < previous_login_name > vervangt door de gebruikersnaam die eerder werd gebruikt door de taak:  
   `DROP LOGIN <previous_login_name>`  
2. Klik op uitvoeren:  
   ![graphic43][graphic43]  

U moet de volgende bevestiging krijgen: 

    Command(s) completed successfully.

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png

