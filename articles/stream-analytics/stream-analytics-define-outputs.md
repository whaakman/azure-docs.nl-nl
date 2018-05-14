---
title: Uitvoer van de Azure Stream Analytics begrijpen
description: Dit artikel wordt beschreven Uitvoeropties gegevens beschikbaar zijn in Azure Stream Analytics, met inbegrip van Power BI voor analyseresultaten.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/11/2018
ms.openlocfilehash: 030af72951e226d3484706e627bc8b74d5469670
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Uitvoer van de Azure Stream Analytics begrijpen
In dit artikel beschrijft de verschillende soorten uitvoer beschikbaar voor een Azure Stream Analytics-taak. Uitvoer kunnen u opslaan en sla de resultaten van de Stream Analytics-taak. Met de uitvoergegevens, kunt u doen verdere business analytics en datawarehousing van uw gegevens. 

Bij het ontwerpen van uw Stream Analytics query verwijzen naar de naam van het gebruik van de uitvoer de [component INTO](https://msdn.microsoft.com/azure/stream-analytics/reference/into-azure-stream-analytics). U kunt één uitvoer per taak of meerdere uitgangen per streaming-taak als u aan de hand van meerdere INTO-componenten in de query moet gebruiken.

Maken, bewerken en testen van de Stream Analytics-taak levert, kunt u de [Azure-portal](stream-analytics-quick-create-portal.md#configure-output-to-the-job), [Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.Net API](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [REST-API](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-output), en [Visual Studio](stream-analytics-tools-for-visual-studio.md).

Sommige typen uitvoer ondersteuning [partitioneren](#partitioning), en [uitvoer batch grootten](#output-batch-size) variëren voor het optimaliseren van doorvoer.


## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics ondersteunt [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store is een ondernemingsbrede opslagplaats op hyperschaal voor analytische workloads van big data. Data Lake Store kunt u voor het opslaan van gegevens van elke grootte, type en opnamesnelheid snelheid voor operationele en experimentele analyses. Er is een stream Analytics worden geautoriseerd voor toegang tot de Data Lake Store.

### <a name="authorize-an-azure-data-lake-store-account"></a>Een Azure Data Lake Store-account toestaan

1. Wanneer Data Lake Storage als uitvoer in de Azure portal is geselecteerd, wordt u gevraagd om een verbinding met een bestaande Data Lake Store te autoriseren.  

   ![Data Lake Store toestaan](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

2. Als u al toegang naar Data Lake Store hebt, selecteert u **autoriseren nu** en er verschijnt een pagina die aangeeft **omleiden naar autorisatie**. Nadat de autorisatie is geslaagd, krijgt u de pagina die u kunt de uitvoer van de Data Lake Store configureren.

3. Zodra u het Data Lake Store-account dat is geverifieerd hebt, kunt u de eigenschappen configureren voor de uitvoer van uw Data Lake Store. De onderstaande tabel wordt de lijst met namen van eigenschappen en hun beschrijving voor het configureren van de uitvoer van uw Data Lake Store.

   ![Data Lake Store toestaan](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

| Naam van eigenschap | Beschrijving | 
| --- | --- |
| Uitvoeralias | Een beschrijvende naam die wordt gebruikt in query's om de queryuitvoer naar Data Lake Store. | 
| Accountnaam | De naam van het Data Lake Storage-account waarin u de uitvoer wilt verzenden. Krijgt u een vervolgkeuzelijst met Data Lake Store-accounts die beschikbaar in uw abonnement zijn. |
| Voorvoegsel van het padpatroon | Het pad dat wordt gebruikt om uw bestanden binnen de opgegeven Data Lake Store-Account te schrijven. U kunt opgeven van een of meer exemplaren van de {date} en {time} variabelen.</br><ul><li>Voorbeeld 1: Map1/logs / {date} / {time}</li><li>Voorbeeld 2: Map1/logs / {date}</li></ul>Als het bestandspatroon pad geen een afsluitende bevat '/', de laatste patroon in het bestandspad wordt behandeld als een voorvoegsel van de bestandsnaam. </br></br>Nieuwe bestanden worden gemaakt in deze omstandigheden:<ul><li>Wijzig in het uitvoerschema</li><li>Externe of interne opnieuw opstarten van een taak.</li></ul> |
| Datumnotatie | Optioneel. Als de datum-token in het pad van het voorvoegsel wordt gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld. Voorbeeld: Jjjj/MM/DD |
|Tijdnotatie | Optioneel. Als het token tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de tijdnotatie waarin de bestanden zijn ingedeeld. De enige ondersteunde waarde is momenteel HH. |
| Serialisatie-indeling voor gebeurtenissen | Serialisatie-indeling voor uitvoergegevens. JSON, CSV en Avro worden ondersteund.| 
| Encoding | Als CSV- of JSON-indeling wordt gebruikt, moet een codering worden opgegeven. Alleen de coderingsindeling UTF-8 wordt momenteel ondersteund.|
| Scheidingsteken | Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn komma, puntkomma's, spatie, tab en verticale balk.|
| Indeling | Alleen van toepassing op JSON-serialisatie. Lijn gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt ingedeeld als een matrix met JSON-objecten. Deze matrix is gesloten, alleen wanneer de taak wordt gestopt of Stream Analytics is verplaatst op naar de volgende periode. In het algemeen is het beter regels gebruiken gescheiden JSON, omdat het niet nodig is geen speciale verwerking terwijl is nog steeds naar het uitvoerbestand worden geschreven.|

### <a name="renew-data-lake-store-authorization"></a>Vernieuwen van Data Lake Store-autorisatie
U moet uw Data Lake Store-account te verifiëren als het wachtwoord is gewijzigd sinds de taak is gemaakt of laatst geverifieerd. Als u niet verifiëren, wordt de taak levert geen resultaten van de uitvoer en ziet u een foutbericht retourneren over de noodzaak van opnieuw autoriseren in de logboeken van de bewerking. Er is momenteel een beperking waar het verificatietoken moet worden handmatig vernieuwd om de 90 dagen voor alle taken met Data Lake Store-uitvoer. 

Vernieuwen van de autorisatie, **stoppen** uw taak > Ga naar de uitvoer van uw Data Lake Store > klikt u op de **vernieuwen autorisatie** koppelen en gedurende een korte periode een pagina wordt weergegeven dat aangeeft **omleiden naar autorisatie...** . Automatisch wordt gesloten en als dit lukt, geeft u aan de pagina **autorisatie is vernieuwd**. Vervolgens moet u op **opslaan** aan de onderkant van de pagina en verder kunt gaan door het opnieuw starten van de taak in de **laatste tijd geëindigd** om gegevensverlies te voorkomen.

![Data Lake Store toestaan](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) kan worden gebruikt als uitvoer voor gegevens die relationele gegevens of voor toepassingen die afhankelijk zijn van de inhoud wordt gehost in een relationele database. Stream Analytics-taken schrijven naar een bestaande tabel in een Azure SQL Database.  Het tabelschema moet exact overeenkomen met de velden en hun typen wordt de uitvoer van de taak. Een [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) kan ook worden opgegeven als uitvoer via de evenals SQL-Database output-optie. De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van de uitvoer van een SQL-Database.

| Naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de uitvoer van de query met deze database. |
| Database | De naam van de database waar u de uitvoer wilt verzenden. |
| Servernaam | De naam van de SQL-Database. |
| Gebruikersnaam | De gebruikersnaam die toegang heeft tot het schrijven naar de databas. |
| Wachtwoord | Het wachtwoord voor het verbinding maken met de databas.e |
| Tabel | De naam van de tabel waar de uitvoer wordt geschreven. Naam van de tabel is hoofdlettergevoelig en het schema van deze tabel moet exact overeenkomen met het aantal velden en hun typen die door de taakuitvoer wordt gegenereerd. |

> [!NOTE]
> De aanbieding voor Azure SQL Database wordt momenteel ondersteund voor de taakuitvoer van een in de Stream Analytics. Een virtuele Machine van Azure met SQL Server met een database die is gekoppeld, wordt echter niet ondersteund. Dit kan worden gewijzigd in toekomstige releases.
> 

## <a name="blob-storage"></a>Blob Storage
BLOB storage vormt een rendabele en schaalbare oplossing voor het opslaan van grote hoeveelheden ongestructureerde gegevens in de cloud.  Voor een inleiding op Azure Blob-opslag en het gebruik ervan, Zie de documentatie bij [het gebruik van Blobs](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van een blob-uitvoer.

| Naam van eigenschap | Beschrijving | 
| --- | --- |
| Uitvoeraliassen | Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze blob-opslag. |
| Opslagaccount | De naam van het opslagaccount waar u de uitvoer wilt verzenden. |
| Opslagaccountsleutel | De geheime sleutel die is gekoppeld aan het opslagaccount. |
| Storage-Container | Containers bieden een logische groepering van blobs die zijn opgeslagen in de Microsoft Azure Blob-service. Wanneer u een blob naar de Blob-service uploaden, moet u een blob-container opgeven. |
| Padpatroon | Optioneel. Het pad naar bestandspatroon gebruikt voor het schrijven van uw BLOB's binnen de opgegeven container. </br></br> U kunt kiezen in het patroon pad naar een of meer exemplaren van de datum-tijdvariabelen gebruiken om op te geven van de frequentie waarmee blobs worden geschreven: </br> {date} {time} </br> </br>U kunt ook een aangepaste {veld} naam opgeven van de gegevens van gebeurtenissen voor het partitioneren van blobs, waarbij de naam van het alfanumerieke en kan spaties, afbreekstreepjes en onderstrepingstekens bevatten. Beperkingen op aangepaste velden omvatten het volgende: <ul><li>Case ongevoeligheid (kan verschillen tussen de kolom 'ID' en kolom 'id' niet)</li><li>Geneste velden zijn niet toegestaan (in plaats daarvan een alias gebruiken in de query voor de taak voor het veld 'plat')</li><li>Expressies kunnen niet als een veldnaam worden gebruikt</li></ul>Voorbeelden: <ul><li>Voorbeeld 1: cluster1/logs / {date} / {time}</li><li>Voorbeeld 2: cluster1/logs / {date}</li><li>Voorbeeld 3: cluster1 / {client_id} / {date} / {time}</li><li>Voorbeeld 4: cluster1 / {myField} waarbij de query is: Selecteer data.myField als myField van invoer;</li></ul><BR> Naamgeving van bestanden, volgt de volgende conventies: </br> {Pad voorvoegsel Pattern}/schemaHashcode_Guid_Number.extension </br></br> Voorbeeld van de uitvoerbestanden: </br><ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li><li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul><br/>
| Datumnotatie | Optioneel. Als de datum-token in het pad van het voorvoegsel wordt gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld. Voorbeeld: Jjjj/MM/DD |
| Tijdnotatie | Optioneel. Als het token tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de tijdnotatie waarin de bestanden zijn ingedeeld. De enige ondersteunde waarde is momenteel HH. |
| Serialisatie-indeling voor gebeurtenissen | Serialisatie-indeling voor uitvoergegevens.  JSON, CSV en Avro worden ondersteund.
| Encoding | Als CSV- of JSON-indeling wordt gebruikt, moet een codering worden opgegeven. Alleen de coderingsindeling UTF-8 wordt momenteel ondersteund. |
| Scheidingsteken | Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn komma, puntkomma, schijfruimte, tabblad en de verticale balk. |
| Indeling | Alleen van toepassing op JSON-serialisatie. Lijn gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt ingedeeld als een matrix met JSON-objecten. Deze matrix is gesloten, alleen wanneer de taak wordt gestopt of Stream Analytics is verplaatst op naar de volgende periode. In het algemeen is het beter regels gebruiken gescheiden JSON, omdat het niet nodig is geen speciale verwerking terwijl is nog steeds naar het uitvoerbestand worden geschreven. |

Wanneer u blobopslag gebruikt als uitvoer, wordt een nieuw bestand gemaakt in de blob in de volgende gevallen:

* Als het bestand overschrijdt het maximum aantal toegestane blokken (momenteel 50.000). Het maximale aantal toegestane blokken kan worden bereikt zonder de maximum toegestane blobgrootte is bereikt. Als de uitvoerpakketten hoog is, ziet u meer bytes per blok en de bestandsgrootte is groter. Als de uitvoerpakketten laag is, elk blok is minder gegevens en de bestandsgrootte kleiner is.
* Als u een schema is gewijzigd in de uitvoer en indeling van de uitvoer moet vaste schema (CSV en Avro).  
* Als een taak opnieuw wordt opgestart, extern door een gebruiker gestopt en worden gestart of intern voor onderhoud of fout bij het herstel.  
* Als de query is volledig gepartitioneerd, wordt voor elke partitie uitvoer nieuw bestand gemaakt.  
* Als een bestand of een container van het opslagaccount is verwijderd door de gebruiker.  
* Als de uitvoer van de tijd die is gepartitioneerd met behulp van het pad voorvoegselpatroon is, wordt een nieuwe blob wordt gebruikt wanneer de query wordt verplaatst naar het volgende uur.
* Als de uitvoer is gepartitioneerd door een aangepast veld, wordt een nieuwe blob per partitiesleutel gemaakt als deze niet bestaat.
*   Als de uitvoer is gepartitioneerd door een aangepast veld waarin de kardinaliteit van de partitie 8000 overschrijdt, kan een nieuwe blob per partitiesleutel worden gemaakt.

## <a name="event-hub"></a>Event Hub
De [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) -service is een uiterst schaalbare voor publiceren / abonneren event ingestor. Het kan miljoenen gebeurtenissen per seconde verzamelen. Een gebruik van een Event Hub als uitvoer is wanneer de uitvoer van een Stream Analytics-taak de invoer van een ander streaming-taak wordt.

Er zijn een aantal parameters die nodig zijn voor het configureren van de Event Hub-gegevensstromen als uitvoer.

| Naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeralias | Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze Event Hub. |
| Event Hub-naamruimte |Een Event Hub-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe Event Hub hebt gemaakt, hebt u ook een Event Hub-naamruimte gemaakt. |
| Event Hub-naam | De naam van de uitvoer van uw Event Hub. |
| Naam van het Event Hub-beleid | Het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad Event Hub configureren. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| Sleutel voor het Event Hub-beleid | De toegang tot de gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Event Hub-naamruimte. |
| Partitiesleutelkolom [optioneel] | Deze kolom bevat de partitiesleutel voor uitvoer van de Event Hub. |
| Serialisatie-indeling voor gebeurtenissen | Serialisatie-indeling voor uitvoergegevens.  JSON, CSV en Avro worden ondersteund. |
| Encoding | Voor de CSV en JSON is UTF-8-de enige ondersteunde coderingsindeling op dit moment. |
| Scheidingsteken | Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn komma, puntkomma, schijfruimte, tabblad en de verticale balk. |
| Indeling | Alleen van toepassing op JSON-serialisatie. Lijn gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt ingedeeld als een matrix met JSON-objecten. Deze matrix is gesloten, alleen wanneer de taak wordt gestopt of Stream Analytics is verplaatst op naar de volgende periode. In het algemeen is het beter regels gebruiken gescheiden JSON, omdat het niet nodig is geen speciale verwerking terwijl is nog steeds naar het uitvoerbestand worden geschreven. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) kan worden gebruikt als uitvoer voor een Stream Analytics-taak te voorzien in een uitgebreide visualisatie-ervaring van de resultaten van de analyse. Deze mogelijkheid kan worden gebruikt voor de operationele dashboards, rapporten te genereren en de metriek aangestuurd reporting.

### <a name="authorize-a-power-bi-account"></a>Een Power BI-account toestaan
1. Als Power BI als uitvoer in de Azure portal is geselecteerd, wordt u gevraagd voor het autoriseren van een bestaande Power BI-gebruiker of een nieuwe Power BI-account maken.  
   
   ![Power BI gebruiker autoriseren](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2. Maak een nieuw account als u niet nog een klik op nu autoriseren.  De volgende pagina wordt weergegeven:
   
   ![Azure-Account Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3. Geef het werk of school-account voor het machtigen van de Power BI-uitvoer in deze stap. Als u weet al niet aangemeld voor Power BI, kiest u nu Sign up. Het werk of school-account die u voor Power BI gebruikt kan afwijken van het account in Azure-abonnement, dat u momenteel bent aangemeld.

### <a name="configure-the-power-bi-output-properties"></a>Configureer de eigenschappen van Power BI-uitvoer
Zodra u de geverifieerde Power BI-account hebt, kunt u de eigenschappen configureren voor uw Power BI-uitvoer. De onderstaande tabel wordt de lijst met namen van eigenschappen en hun beschrijving voor het configureren van uw Power BI-uitvoer.

| Naam van eigenschap | description |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de queryuitvoer naar deze Power BI-uitvoer. |
| Groepswerkruimte |U kunt groepen binnen uw Power BI-account selecteren of kies 'Mijn werkruimte' als u niet wilt schrijven naar een groep zodat delen van gegevens met andere Power BI-gebruikers.  Bijwerken van een bestaande groep is vereist voor het vernieuwen van de Power BI-verificatie. |
| Naam van de gegevensset |Geef een gegevenssetnaam dat het nodig is voor de uitvoer van de Power BI te gebruiken |
| Tabelnaam |Geef een tabelnaam onder de gegevensset van de Power BI-uitvoer. Op dit moment kan Power BI-uitvoer van de Stream Analytics-taken alleen hebben één tabel in een gegevensset |

Zie voor een demonstratie van de configuratie van een Power BI-uitvoer en het dashboard, het [Azure Stream Analytics & Power BI](stream-analytics-power-bi-dashboard.md) artikel.

> [!NOTE]
> Expliciet maken geen de gegevensset en de tabel in de Power BI-dashboard. De gegevensset en de tabel wordt automatisch gevuld wanneer de taak is gestart en de taak begint gemalen uitvoer naar Power BI. Houd er rekening mee dat als de taak query geen resultaten, de gegevensset niet genereren en de tabel is niet gemaakt. Houd er rekening mee dat als Power BI al een gegevensset en een tabel met dezelfde naam als een opgegeven in deze Stream Analytics-taak, de bestaande gegevens worden overschreven.
> 

### <a name="schema-creation"></a>Schema maken
Azure Stream Analytics maakt een Power BI gegevensset en tabel namens de gebruiker als deze niet al bestaat. In andere gevallen is wordt de tabel bijgewerkt met nieuwe waarden. Er is een beperking die slechts één tabel binnen een dataset kan bestaan.

### <a name="data-type-conversion-from-stream-analytics-to-power-bi"></a>Conversie van Stream Analytics naar Power BI-gegevenstype
Azure Stream Analytics werkt het gegevensmodel dynamisch tijdens runtime als het uitvoerschema wordt gewijzigd. Alle worden wijzigingen in de kolom naam, wijzigingen van het type kolom, en het toevoegen of verwijderen van kolommen bijgehouden.

Deze tabel bevat informatie over de conversie van het gegevenstype van [Stream Analytics-gegevenstypen](https://msdn.microsoft.com/library/azure/dn835065.aspx) naar Power BIs [typen met Entity Data Model (EDP)](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) als een gegevensset met POWER BI en de tabel niet bestaat.

Vanuit Stream Analytics | Met Power BI
-----|-----|------------
bigint | Int64
nvarchar(max) | Reeks
datum/tijd | Datum en tijd
drijvend | dubbele
Record matrix | String-type, constante waarde 'Irecords' of 'IArray'

### <a name="schema-update"></a>Schema-Update
Stream Analytics infereert het modelschema van gegevens op basis van de eerste reeks gebeurtenissen in de uitvoer. Later, indien nodig, wordt het modelschema gegevens bijgewerkt zodat binnenkomende gebeurtenissen die mogelijk niet in het oorspronkelijke schema passen.

De `SELECT *` query om te voorkomen dat dynamische schema-update in rijen moet worden vermeden. Naast de mogelijke gevolgen voor prestaties, kan dit ook leiden tot onzekerheid van de tijd die voor de resultaten. De exacte velden die moeten worden weergegeven in Power BI-dashboard moeten worden geselecteerd. Daarnaast moet de gegevenswaarden compatibel zijn met het gekozen gegevenstype.


Vorige/huidige | Int64 | Reeks | Datum en tijd | dubbele
-----------------|-------|--------|----------|-------
Int64 | Int64 | Reeks | Reeks | dubbele
dubbele | dubbele | Reeks | Reeks | dubbele
Reeks | Reeks | Reeks | Reeks |  | Reeks | 
Datum en tijd | Reeks | Reeks |  Datum en tijd | Reeks


### <a name="renew-power-bi-authorization"></a>Vernieuwen van Power BI-autorisatie
Als het wachtwoord voor uw Power BI-account verandert nadat uw Stream Analytics-taak is gemaakt of laatst geverifieerd, moet u de Stream Analytics verifiëren. Als multi-factor Authentication (MFA) is geconfigureerd op de tenant van uw Azure Active Directory (AAD), moet u ook voor het vernieuwen van Power BI-autorisatie elke twee weken. Een symptoom zijn van dit probleem is geen taakuitvoer en een "verifiëren gebruiker error" in de logboeken voor bewerking:

  ![Power BI vernieuwen fout](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

U lost dit probleem, stop de actieve taak en gaat u naar uw Power BI-uitvoer.  Selecteer de **vernieuwen autorisatie** koppelen, en start u de taak van de **laatste tijd geëindigd** om gegevensverlies te voorkomen.

  ![Power BI wordt verlengd autorisatie](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Table Storage
[Azure Table storage](../storage/common/storage-introduction.md) biedt maximaal beschikbare, sterk schaalbare opslag, zodat een toepassing automatisch schalen kan om te voldoen aan de behoeften van de gebruiker. Tabelopslag is van Microsoft NoSQL sleutel-/ kenmerkopslag, waarvoor een van voor gestructureerde gegevens met minder beperkingen van het schema gebruikmaken kan. Azure Table storage kan worden gebruikt voor het opslaan van gegevens voor de persistentie en efficiënte ophalen.

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van de tabeluitvoer van een.

| De naam van eigenschap | description |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar de tabelopslag van deze. |
| Storage-account |De naam van het opslagaccount waar u de uitvoer wilt verzenden. |
| Opslagaccountsleutel |De toegangssleutel die is gekoppeld aan het opslagaccount. |
| Tabelnaam |De naam van de tabel. De tabel wordt gemaakt als deze niet bestaat. |
| Partitiesleutel |De naam van de uitvoerkolom die de partitiesleutel bevat. De partitiesleutel is een unieke id voor de partitie in een bepaalde tabel die dient als het eerste deel van de primaire sleutel van een entiteit. Het is een tekenreekswaarde die maximaal 1 KB groot zijn. |
| Rijsleutel |De naam van de uitvoerkolom die de rijsleutel bevat. De rijsleutel is een unieke id voor een entiteit binnen een bepaalde partitie. Het tweede gedeelte van een entiteit primaire sleutel vormt. De rijsleutel is een tekenreekswaarde die maximaal 1 KB groot zijn. |
| Batchgrootte |Het aantal records voor een batchbewerking. De standaardwaarde (100) is voldoende voor de meeste taken. Raadpleeg de [tabel batchbewerking spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) voor meer informatie over het aanpassen van deze instelling. |
 
## <a name="service-bus-queues"></a>Service Bus-wachtrijen
[Service Bus-wachtrijen](https://msdn.microsoft.com/library/azure/hh367516.aspx) bieden een First In, eerste Out (FIFO) berichtbezorging naar een of meer concurrerende consumenten. Normaal gesproken berichten moeten worden ontvangen en verwerkt door de ontvangers in de tijdsvolgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht wordt ontvangen en verwerkt door slechts één berichtconsument.

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van de uitvoer van een wachtrij.

| Naam van eigenschap | description |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze Service Bus-wachtrij. |
| Service Bus-naamruimte |Een Service Bus-naamruimte is een container voor een set berichtentiteiten. |
| Wachtrijnaam |De naam van de Service Bus-wachtrij. |
| Naam van het wachtrijbeleid |Wanneer u een wachtrij maakt, kunt u ook gedeeld toegangsbeleid maken op het tabblad wachtrij configureren. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| Sleutel voor het wachtrijbeleid |De toegang tot gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte |
| Serialisatie-indeling voor gebeurtenissen |Serialisatie-indeling voor uitvoergegevens.  JSON, CSV en Avro worden ondersteund. |
| Encoding |Voor de CSV en JSON is UTF-8 de enige ondersteunde coderingsindeling op dit moment |
| Scheidingsteken |Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn komma, puntkomma, schijfruimte, tabblad en de verticale balk. |
| Indeling |Alleen van toepassing op JSON-type. Lijn gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt ingedeeld als een matrix met JSON-objecten. |

Het aantal partities is [op basis van de Service Bus-SKU en grootte](../service-bus-messaging/service-bus-partitioning.md). De partitiesleutel is een unieke integer-waarde voor elke partitie.

## <a name="service-bus-topics"></a>Service Bus-onderwerpen
Terwijl Service Bus-wachtrijen een communicatiemethode één van afzender naar ontvanger bieden [Service Bus-onderwerpen](https://msdn.microsoft.com/library/azure/hh367516.aspx) een een-op-veel-vorm van communicatie bieden.

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van de tabeluitvoer van een.

| Naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze Service Bus-onderwerp. |
| Service Bus-naamruimte |Een Service Bus-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe Event Hub hebt gemaakt, hebt u ook een Service Bus-naamruimte gemaakt |
| Onderwerpnaam |Onderwerpen zijn berichtentiteiten, vergelijkbaar met event hubs en wachtrijen. Ze zijn ontworpen voor het verzamelen van gebeurtenisstromen uit een aantal verschillende apparaten en services. Wanneer u een onderwerp maakt, is het ook een specifieke naam gegeven. De berichten die worden verzonden naar een onderwerp is alleen beschikbaar als een abonnement is gemaakt, dus zorg ervoor er zijn een of meer abonnementen in het onderwerp |
| Naam van het onderwerpbeleid |Wanneer u een onderwerp maakt, kunt u ook gedeeld toegangsbeleid maken op het tabblad onderwerp configureren. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels |
| Sleutel voor het onderwerpbeleid |De toegang tot gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte |
| Serialisatie-indeling voor gebeurtenissen |Serialisatie-indeling voor uitvoergegevens.  JSON, CSV en Avro worden ondersteund. |
| Encoding |Als CSV- of JSON-indeling wordt gebruikt, moet een codering worden opgegeven. UTF-8 is de enige ondersteunde coderingsindeling op dit moment |
| Scheidingsteken |Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn komma, puntkomma, schijfruimte, tabblad en de verticale balk. |

Het aantal partities is [op basis van de Service Bus-SKU en grootte](../service-bus-messaging/service-bus-partitioning.md). De partitiesleutel is een unieke integer-waarde voor elke partitie.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) is van een database globaal gedistribueerd en modellen service dat aanbiedingen oneindig elastisch schalen rond de hele wereld, uitgebreide query en automatische indexeren via schema networkdirect gegevensmodellen, lage latentie gegarandeerd en toonaangevende uitgebreide Sla's. Raadpleeg voor meer informatie over opties voor het verzamelen van Cosmos DB voor Stream Analytics, de [Stream Analytics met Cosmos DB als uitvoer](stream-analytics-documentdb-output.md) artikel.

> [!Note]
> Op dit moment Azure Stream Analytics biedt alleen ondersteuning voor verbinding met behulp van CosmosDB **SQL-API**.
> Andere Azure Cosmos DB-API's zijn nog niet ondersteund. Als u punt Azure Stream Analytics aan de Azure DB die Cosmos-accounts die worden gemaakt met andere API's, zijn de gegevens mogelijk niet juist opgeslagen. 

De volgende tabel beschrijft de eigenschappen voor het maken van een Azure DB die Cosmos-uitvoer.
| Naam van eigenschap | description |
| --- | --- |
| Uitvoeralias | Een alias aan dit uitvoer in de Stream Analytics query verwijzen. |
| Sink | Cosmos DB |
| Importoptie | Kies een 'Cosmos-DB uit uw abonnement selecteren', of ' Geef Cosmos DB-instellingen handmatig '.
| Account-id | De naam of de URI van de Cosmos-DB-account voor het eindpunt. |
| Accountcode | De gedeelde toegangssleutel voor het account van de Cosmos-DB. |
| Database | De naam van de Cosmos-DB-database. |
| Patroon voor de collectienaam | Naam van de verzameling of het patroon voor de verzamelingen moet worden gebruikt. <br/>Indeling van de Collectienaam kan worden samengesteld met behulp van de optionele {partition}-token, waarbij partities beginnen bij 0. Twee voorbeelden:  <br/>1. _MyCollection_ : een verzameling met de naam 'MyCollection' moet aanwezig zijn.  <br/>2. _MyCollection {partition}_ – op basis van de partitioneringskolom. <br/>De partitionerende kolom verzamelingen moeten aanwezig zijn: 'MyCollection0', 'MyCollection1', 'MyCollection2', enzovoort. |
| Partitiesleutel | Optioneel. Dit is alleen nodig als u een token {partition} in het patroon van de naam van verzameling.<br/> De partitiesleutel is de naam van het veld in uitvoergebeurtenissen dat wordt gebruikt voor het opgeven van de sleutel voor het partitioneren van uitvoer in collecties.<br/> Voor één verzameling uitvoer, kan een willekeurige uitvoer van de kolommen worden gebruikt. Bijvoorbeeld: PartitionId. |
| Document-id |Optioneel. De naam van het veld in uitvoergebeurtenissen dat wordt gebruikt voor de primaire sleutel opgeven in welke invoegen of updatebewerkingen zijn gebaseerd.  

## <a name="azure-functions"></a>Azure Functions
Azure Functions is een serverloze compute-service waarmee u code op aanvraag kunt uitvoeren zonder expliciet een infrastructuur in te richten of te beheren. Hiermee kunt u code die wordt geactiveerd door gebeurtenissen in Azure of services van derden implementeren.  Deze mogelijkheid van Azure Functions om te reageren op triggers maakt het een natuurlijke uitvoer voor een Azure Stream Analytics. Deze uitvoeradapter kan gebruikers verbinding van Stream Analytics met Azure Functions, en voer een script of een stuk code in reactie op tal van gebeurtenissen.

Azure Stream Analytics roept Azure Functions via HTTP-triggers. De nieuwe uitvoer van de functie Azure-adapter is beschikbaar met de volgende eigenschappen kunnen worden geconfigureerd:

| Naam van eigenschap | description |
| --- | --- |
| Functie-app |Naam van uw App in Azure Functions |
| Functie |Naam van de functie in uw App in Azure-functies |
| Sleutel |Als u een Azure-functie van een ander abonnement gebruiken wilt, kunt u doen door op te geven van de sleutel voor toegang tot uw functie |
| Maximale batchgrootte |Deze eigenschap kan worden gebruikt om de maximale grootte voor elke uitvoer batch die wordt verzonden naar uw Azure-functie. Deze waarde is standaard 256 KB |
| Maximum aantal batches  |Als de naam aangeeft, wordt deze eigenschap kunt u het maximum aantal gebeurtenissen in elke batch die wordt verzonden naar Azure Functions opgeven. De standaardwaarde voor de maximale batch-aantal is 100 |

Wanneer Azure Stream Analytics 413 (http-aanvragen entiteit te groot) uitzondering van de functie Azure ontvangt, vermindert het de grootte van de batches naar Azure Functions verzendt. Gebruik in uw Azure functiecode deze uitzondering om ervoor te zorgen dat Azure Stream Analytics te grote batches niet verzenden. Zorg ervoor dat de maximale batch-aantal en grootte waarden gebruikt in de functie consistent met de waarden die zijn opgegeven in de Stream Analytics-portal zijn. 

Ook wordt in een situatie waarin er geen gebeurtenis in een tijdvenster aanvoer is, wordt geen uitvoer gegenereerd. Als gevolg hiervan computeResult functie niet wordt aangeroepen. Dit gedrag is consistent met de ingebouwde statistische functies in vensters.

## <a name="partitioning"></a>Partitionering

De volgende tabel geeft een overzicht van de partitie ondersteuning en het aantal uitvoer schrijvers voor elk uitvoertype:

| Uitvoertype | Ondersteuning voor partitioneren | Partitiesleutel  | Aantal uitvoer schrijvers | 
| --- | --- | --- | --- |
| Azure Data Lake Store | Ja | Gebruik {date} en {time} tokens in het pad voorvoegsel-patroon. Kies de notatie van datum, zoals jjjj/MM/DD, MM-DD-JJJJ, MM-DD-JJJJ. HH wordt gebruikt voor de tijdnotatie. | De invoer partitionering voor volgt [volledig worden query's](stream-analytics-scale-jobs.md). | 
| Azure SQL Database | Nee | Geen | Niet van toepassing. | 
| Azure Blob Storage | Ja | Gebruik {date} en {time} tokens of een enkel {fieldname} van de gebeurtenis velden in het pad-patroon. Kies de notatie van datum, zoals jjjj/MM/DD, MM-DD-JJJJ, MM-DD-JJJJ. HH wordt gebruikt voor de tijdnotatie. | De invoer partitionering voor volgt [volledig worden query's](stream-analytics-scale-jobs.md). | 
| Azure Event Hub | Ja | Ja | Varieert afhankelijk van de uitlijning van de partitie.</br> Bij de uitvoer van de Event Hub-partitiesleutel gelijkmatig wordt uitgelijnd met upstream (vorige) query stap is het aantal schrijvers hetzelfde aantal het aantal is partities van de Event Hub uitvoer. Elke writer maakt gebruik van de EventHub [EventHubSender klasse](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) gebeurtenissen verzenden naar de specifieke partitie. </br> Wanneer de uitvoer van de Event Hub-partitiesleutel is niet uitgelijnd met upstream (vorige) query stap, het aantal schrijvers is hetzelfde als het aantal partities in die de vorige stap. Elke writer gebruikt EventHubClient [SendBatchAsync klasse](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) gebeurtenissen verzenden naar alle partities van de uitvoer. |
| Power BI | Nee | Geen | Niet van toepassing. | 
| Azure Table Storage | Ja | De uitvoer kolom.  | De invoer partitionering voor volgt [volledig geparallelliseerde query's](stream-analytics-scale-jobs.md). | 
| Azure Service Bus-onderwerp | Ja | Automatisch gekozen. Het aantal partities is gebaseerd op de [Service Bus-SKU's en de grootte van](../service-bus-messaging/service-bus-partitioning.md). De partitiesleutel is een unieke integer-waarde voor elke partitie.| Hetzelfde als het aantal partities in het onderwerp uitvoer.  |
| Azure Service Bus-wachtrij | Ja | Automatisch gekozen. Het aantal partities is gebaseerd op de [Service Bus-SKU's en de grootte van](../service-bus-messaging/service-bus-partitioning.md). De partitiesleutel is een unieke integer-waarde voor elke partitie.| Hetzelfde als het aantal partities in de wachtrij voor de uitvoer. |
| Azure Cosmos DB | Ja | Token {partition} in de verzameling naampatroon gebruiken. de waarde {partition} is gebaseerd op de component PARTITION BY in de query. | De invoer partitionering voor volgt [volledig geparallelliseerde query's](stream-analytics-scale-jobs.md). |
| Azure Functions | Nee | Geen | Niet van toepassing. | 

## <a name="output-batch-size"></a>Batchgrootte voor uitvoer
Azure Stream Analytics maakt gebruik van variabele grootte batches voor het verwerken van gebeurtenissen en schrijven naar uitvoer. Doorgaans de Stream Analytics-engine schrijft een bericht niet op een tijdstip en maakt gebruik van batches voor efficiëntie. Wanneer zowel het binnenkomende en de uitgaande snelheid van gebeurtenissen hoog is, gebruikt deze grotere batches. Wanneer de snelheid van de uitgaande laag is, gebruikt deze kleinere batches latentie laag te houden. 

De volgende tabel worden enkele van de overwegingen voor uitvoer van batchverwerking:

| Uitvoertype | Maximale berichtlengte | Optimalisatie van de batch-grootte |
| :--- | :--- | :--- | 
| Azure Data Lake Store | Zie [Data Lake Storage beperkt](../azure-subscription-service-limits.md#data-lake-store-limits) | Maximaal 4 MB per schrijfbewerking |
| Azure SQL Database | 10.000 maximumaantal rijen per één bulksgewijs invoegen</br>Min 100 rijen per één bulksgewijs invoegen </br>Zie ook [Azure SQL beperkt](../sql-database/sql-database-resource-limits.md) |  Elke batch is in eerste instantie bulksgewijs invoegen met maximale batchgrootte en batch kan worden gesplitst in helft (tot batchgrootte Min) op basis van herstelbare fouten van SQL. |
| Azure Blob Storage | Zie [beperkt van Azure Storage](../azure-subscription-service-limits.md#storage-limits) | Maximumgrootte voor Blob-blok is 4 MB</br>Maximum aantal van de Blob bock is 50000 |
| Azure Event Hub   | 256 KB per bericht </br>Zie ook [beperkt van Event Hubs](../event-hubs/event-hubs-quotas.md) |    Bij het partitioneren van uitvoer van de invoer niet afstemmen, wordt elke gebeurtenis afzonderlijk in een EventData worden verpakt en verzonden in een batch tot de maximale berichtgrootte (1 MB voor Premium-SKU). </br></br>  Bij het partitioneren van Input-Output wordt uitgelijnd, worden meerdere gebeurtenissen in een enkel EventData tot de maximale berichtgrootte verpakt en verzonden.    |
| Power BI | Zie [beperkt van Power BI Rest-API](https://msdn.microsoft.com/library/dn950053.aspx) |
| Azure Table Storage | Zie [beperkt van Azure Storage](../azure-subscription-service-limits.md#storage-limits) | Standaard is 100 entiteiten per één transactie en kan worden geconfigureerd voor een kleinere waarde indien nodig. |
| Azure Service Bus-wachtrij   | 256 KB per bericht</br> Zie ook [Service Bus beperkt](../service-bus-messaging/service-bus-quotas.md) | Enkelvoudige gebeurtenis per bericht |
| Azure Service Bus-onderwerp | 256 KB per bericht</br> Zie ook [Service Bus beperkt](../service-bus-messaging/service-bus-quotas.md) | Enkelvoudige gebeurtenis per bericht |
| Azure Cosmos DB   | Zie [Azure Cosmos DB beperkt](../azure-subscription-service-limits.md#azure-cosmos-db-limits) | Batch de grootte en frequentie is aangepast dynamisch gebaseerde CosmosDB antwoorden schrijven. </br> Er is geen vooraf bepaalde beperkingen van Stream Analytics. |
| Azure Functions   | | Standaard-batchgrootte is 246 KB. </br> Aantal van de standaard gebeurtenissen per batch is 100. </br> De batchgrootte is configureerbaar en kan worden verhoogd of verlaagd in de Stream Analytics [uitvoeropties](#azure-functions). 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Snelstartgids: Een Stream Analytics-taak maken met behulp van de Azure-portal](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
