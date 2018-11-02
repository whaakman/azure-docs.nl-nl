---
title: Inzicht in de uitvoer van Azure Stream Analytics
description: Dit artikel beschrijft de opties voor uitvoer gegevens beschikbaar zijn in Azure Stream Analytics, met inbegrip van Power BI voor analyseresultaten.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 396b843ecd17c15e9476bacb5e96545f963f0c4e
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50747844"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Inzicht in de uitvoer van Azure Stream Analytics
Dit artikel beschrijft de verschillende typen uitvoer beschikbaar voor een Azure Stream Analytics-taak. Uitvoer kunnen u opslaan en sla de resultaten van de Stream Analytics-taak. De uitvoergegevens die worden gebruikt, kunt u doen verdere bedrijfsanalyses en datawarehousing van uw gegevens. 

Bij het ontwerpen van uw Stream Analytics-query, verwijzen naar de naam van het gebruik van de uitvoer de [component INTO](https://msdn.microsoft.com/azure/stream-analytics/reference/into-azure-stream-analytics). U kunt een enkele uitvoer per taak of meerdere uitvoer per streaming-taak als u nodig hebt door te geven meerdere INTO-componenten in de query.

Maken, bewerken en testen van Stream Analytics-taak weergeeft, kunt u de [Azure-portal](stream-analytics-quick-create-portal.md#configure-output-to-the-job), [Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.Net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [REST-API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output), en [Visual Studio](stream-analytics-quick-create-vs.md).

Sommige typen ondersteuning voor uitvoer [partitioneren](#partitioning), en [uitvoer van de batch-grootten](#output-batch-size) variëren voor het optimaliseren van doorvoer.


## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics ondersteunt [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store is een ondernemingsbrede opslagplaats op hyperschaal voor analytische workloads van big data. Data Lake Store kunt u voor het opslaan van gegevens van elke grootte, soort en opnamesnelheid voor operationele en verkennende analyse. Stream Analytics moet zijn gemachtigd voor toegang tot de Data Lake Store.

Azure Data Lake Store-uitvoer van Stream Analytics is momenteel niet beschikbaar in de Azure China (21Vianet) en regio's van Azure Duitsland (T-Systems International).

### <a name="authorize-an-azure-data-lake-store-account"></a>Een Azure Data Lake Store-account toestaan

1. Wanneer Data Lake-opslag is geselecteerd als uitvoer in Azure portal, wordt u gevraagd om een verbinding met een bestaande Data Lake Store.  

   ![Data Lake Store toestaan](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

2. Als u al toegang tot Data Lake Store hebt, selecteert u **nu autoriseren** en wordt een pagina weergegeven die wijzen op **omleiden naar autorisatie**. Nadat de autorisatie is gelukt, krijgt u de pagina die u kunt de uitvoer van de Data Lake Store configureren.

3. Zodra u de Data Lake Store-account dat is geverifieerd hebt, kunt u de eigenschappen configureren voor uw Data Lake Store-uitvoer. De onderstaande tabel wordt de lijst met namen van eigenschappen en hun beschrijving van de uitvoer van uw Data Lake Store configureren.

   ![Data Lake Store toestaan](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

| Naam van eigenschap | Beschrijving | 
| --- | --- |
| Uitvoeralias | Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer aan dit Data Lake Store te regelen. | 
| Accountnaam | De naam van het Data Lake Storage-account waar u de uitvoer verzendt. Krijgt u een vervolgkeuzelijst met Data Lake Store-accounts die beschikbaar in uw abonnement zijn. |
| Voorvoegsel van het padpatroon | Het pad dat wordt gebruikt om uw bestanden in de opgegeven Data Lake Store-Account te schrijven. U kunt opgeven van een of meer exemplaren van de {date} en {time} variabelen.</br><ul><li>Voorbeeld 1: Map1/logboeken / {date} / {time}</li><li>Voorbeeld 2: Map1/logboeken / {date}</li></ul><br>De tijdstempel van de mapstructuur gemaakt volgt UTC en niet de lokale tijd.</br><br>Als het pad naar bestandspatroon geen een afsluitende bevat '/', de laatste patroon in het bestandspad wordt beschouwd als een voorvoegsel voor de bestandsnaam. </br></br>Nieuwe bestanden worden gemaakt in de volgende situaties:<ul><li>In de uitvoerschema wijzigen</li><li>Externe of interne opnieuw starten van een taak.</li></ul> |
| Datumnotatie | Optioneel. Als de datumtoken van de in het pad van het voorvoegsel wordt gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld. Voorbeeld: Jjjj/MM/DD |
|Tijdnotatie | Optioneel. Als de token van de tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de indeling waarin de bestanden zijn ingedeeld. De enige ondersteunde waarde is momenteel HH. |
| Serialisatie-indeling voor gebeurtenissen | Serialisatie-indeling voor de uitvoergegevens. JSON-, CSV- en Avro worden ondersteund.| 
| Encoding | Als u CSV of JSON-indeling, moet een codering worden opgegeven. Alleen de coderingsindeling UTF-8 wordt momenteel ondersteund.|
| Scheidingsteken | Alleen van toepassing voor de CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn komma, puntkomma, spatie, tab en verticale balk.|
| Indeling | Alleen van toepassing op JSON-serialisatie. Lijn gescheiden geeft aan dat de uitvoer is geformatteerd met elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt opgemaakt als een matrix met JSON-objecten. Deze matrix wordt gesloten wanneer de taak stopt of Stream Analytics is verplaatst op naar de volgende periode. In het algemeen is het beter gebruik van de regel gescheiden JSON, omdat er geen speciale verwerking niet vereist, terwijl het uitvoerbestand is nog steeds worden geschreven naar.|

### <a name="renew-data-lake-store-authorization"></a>Vernieuw de autorisatie van Data Lake Store
U moet uw Data Lake Store-account te verifiëren als het wachtwoord is gewijzigd sinds de taak is gemaakt of laatst geverifieerd. Als u niet opnieuw worden geverifieerd, wordt de taak produceert geen uitvoer resultaten en ziet u een foutbericht met de noodzaak van opnieuw te autoriseren in de logboeken voor bewerkingen. Er is momenteel beperkt tot waar het verificatietoken moet worden handmatig vernieuwd om de 90 dagen voor alle taken met Data Lake Store-uitvoer. 

Autorisatie vernieuwen **stoppen** uw taak > Ga naar de uitvoer van uw Data Lake Store > klikt u op de **autorisatie vernieuwen** koppelen en gedurende een korte periode een pagina weergegeven die wijzen op **omleiden naar autorisatie...** . Automatisch wordt gesloten en als dit lukt, geeft u aan de pagina **autorisatie is vernieuwd**. Vervolgens moet u klikken op **opslaan** aan de onderkant van de pagina, en u kunt doorgaan door de taak opnieuw te starten de **gestopt laatst** om gegevensverlies te voorkomen.

![Data Lake Store toestaan](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) kan worden gebruikt als uitvoer voor gegevens die relationele of voor toepassingen die afhankelijk zijn van de inhoud wordt gehost in een relationele database. Stream Analytics-taken schrijven naar een bestaande tabel in een Azure SQL Database.  Het tabelschema moet exact overeenkomen met de velden en de typen hiervan wordt de uitvoer van uw taak. Een [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) kan ook worden opgegeven als uitvoer via de uitvoeroptie SQL-Database. Raadpleeg voor meer informatie over manieren om te schrijven-doorvoer te verbeteren, de [Stream Analytics met Azure SQL-database als uitvoer](stream-analytics-sql-output-perf.md) artikel. De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van de uitvoer van een SQL-Database.

| Naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer met deze database te regelen. |
| Database | De naam van de database waar u de uitvoer verzendt. |
| Servernaam | De naam van de SQL-Database-server. |
| Gebruikersnaam | De gebruikersnaam die toegang heeft tot het schrijven naar de database. |
| Wachtwoord | Het wachtwoord voor verbinding met de database |
| Tabel | De naam van de tabel waar de uitvoer wordt geschreven. Naam van de tabel is hoofdlettergevoelig en het schema van deze tabel moet exact overeenkomen met het aantal velden en hun type wordt gegenereerd door de taakuitvoer van uw. |

> [!NOTE]
> De aanbieding Azure SQL Database wordt momenteel ondersteund voor de taakuitvoer van een in Stream Analytics. Een virtuele Machine van Azure SQL-Server met een database die is gekoppeld, wordt echter niet ondersteund. Dit is mogelijk gewijzigd in toekomstige releases.
> 

## <a name="blob-storage"></a>Blob Storage
BLOB-opslag biedt een voordelige en schaalbare oplossing voor het opslaan van grote hoeveelheden ongestructureerde gegevens in de cloud.  Voor een inleiding op Azure Blob-opslag en het gebruik ervan, Zie de documentatie bij [over het gebruik van Blobs](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van een blob-uitvoer.

| Naam van eigenschap       | Beschrijving                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| Uitvoeralias        | Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze blobopslag te regelen. |
| Opslagaccount     | De naam van het opslagaccount waar u de uitvoer verzendt.               |
| Opslagaccountsleutel | De geheime sleutel die is gekoppeld aan de storage-account.                              |
| Storage-Container   | Containers bieden een logische groepering van blobs die zijn opgeslagen in de Microsoft Azure Blob-service. Wanneer u een blob geüpload naar de Blob-service, moet u een container voor die blob opgeven. |
| Padpatroon | Optioneel. Het pad naar bestandspatroon gebruikt voor het schrijven van uw blobs in de opgegeven container. <br /><br /> U kunt kiezen in het padpatroon naar een of meer exemplaren van de datum-tijdvariabelen gebruiken om op te geven van de frequentie waarmee blobs worden geschreven: <br /> {date}, {time} <br /><br />Als u dit [Azure portal-koppeling](https://portal.azure.com/?microsoft_azure_streamanalytics_bloboutputpathpartitioning=true&Microsoft_Azure_StreamAnalytics_bloboutputcontainerpartitioning=true) voor toegang tot de aangepaste blob partitioneren Preview-versie moet u één aangepaste {veld}-naam van uw gebeurtenisgegevens tot partitie blobs kunt opgeven. Naam van het veld is alfanumerieke en kan spaties, afbreekstreepjes en onderstrepingstekens bevatten. De volgende beperkingen met betrekking tot aangepaste velden zijn beschikbaar: <ul><li>Case ongevoeligheid (kan geen onderscheid tussen de kolom 'ID' en kolom 'id')</li><li>Geneste velden zijn niet toegestaan (in plaats daarvan een alias gebruiken in de taakquery het veld ' afvlakken ")</li><li>Expressies kunnen niet worden gebruikt als de naam van een veld.</li></ul> <br /><br /> De Preview-versie kunt ook het gebruik van aangepaste datum/tijd-indeling aanduiding configuraties in het pad. Aangepaste datum en tijd indelingen moeten worden opgegeven op een tijdstip, ingesloten door de {datum/tijd:\<aanduiding >} trefwoord. Toegestane invoer \<aanduiding > jjjj, MM, M, dd, d, HH, H, mm, m, ss of s zijn. De {datum/tijd:\<aanduiding >} sleutelwoord kan meerdere keren worden gebruikt in het pad om te vormen van aangepaste datum/tijd-configuraties. <br /><br />Voorbeelden: <ul><li>Voorbeeld 1: cluster1/logboeken / {date} / {time}</li><li>Voorbeeld 2: cluster1/logboeken / {date}</li><li>Voorbeeld 3 (preview): cluster1 / {client_id} / {date} / {time}</li><li>Voorbeeld 4 (preview): cluster1 / {datetime:ss} / {myField} wanneer de query is: Selecteer data.myField als myField van invoer;</li><li>Voorbeeld 5 (preview): cluster1/jaar = {datetime:yyyy} / maand = {datetime:MM} / dag = {datetime:dd}</ul><br /><br />De tijdstempel van de mapstructuur gemaakt volgt UTC en niet de lokale tijd.<br /><br/>Naamgeving van bestanden, volgt de volgende gebruikt: <br /><br />{Pad voorvoegsel Pattern}/schemaHashcode_Guid_Number.extension<br /><br />Voorbeeld van de uitvoerbestanden:<ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li>  <li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul> <br /><br /> Voor meer informatie over deze Preview-versie, gaat u naar [aangepaste datum/tijd-padpatronen voor Azure Stream Analytics blob storage-uitvoer (Preview)](stream-analytics-custom-path-patterns-blob-storage-output.md). |
| Datumnotatie | Optioneel. Als de datumtoken van de in het pad van het voorvoegsel wordt gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld. Voorbeeld: Jjjj/MM/DD |
| Tijdnotatie | Optioneel. Als de token van de tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de indeling waarin de bestanden zijn ingedeeld. De enige ondersteunde waarde is momenteel HH. |
| Serialisatie-indeling voor gebeurtenissen | Serialisatie-indeling voor de uitvoergegevens.  JSON-, CSV- en Avro worden ondersteund. |
| Encoding    | Als u CSV of JSON-indeling, moet een codering worden opgegeven. Alleen de coderingsindeling UTF-8 wordt momenteel ondersteund. |
| Scheidingsteken   | Alleen van toepassing voor de CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn met door komma's, door puntkomma's, spatie, tab en verticale balk. |
| Indeling      | Alleen van toepassing op JSON-serialisatie. Lijn gescheiden geeft aan dat de uitvoer is geformatteerd met elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt opgemaakt als een matrix met JSON-objecten. Deze matrix wordt gesloten wanneer de taak stopt of Stream Analytics is verplaatst op naar de volgende periode. In het algemeen is het beter gebruik van de regel gescheiden JSON, omdat er geen speciale verwerking niet vereist, terwijl het uitvoerbestand is nog steeds worden geschreven naar. |

Wanneer u blob-opslag als uitvoer, wordt een nieuw bestand wordt gemaakt in de blob in de volgende gevallen:

* Als het bestand is groter dan het maximum aantal toegestane blokken (momenteel 50.000). Het maximale aantal toegestane blokken kan worden bereikt zonder dat de voor maximale toegestane blobgrootte is bereikt. Als de uitvoerpakketten hoog is, ziet u meer bytes per blok en het bestand is groter. Als de uitvoerpakketten laag is, elk blok is minder gegevens en de grootte van het kleiner is.
* Als er een schemawijziging in de uitvoer, en de uitvoerindeling vaste schema (CSV- en Avro) is vereist.  
* Als een taak opnieuw wordt opgestart, extern door een gebruiker het stoppen en starten of intern voor onderhoud of fout bij het herstel van het systeem.  
* Als de query gepartitioneerd is volledig, worden nieuw bestand wordt gemaakt voor elke partitie uitvoer.  
* Als een bestand of een container van het opslagaccount is verwijderd door de gebruiker.  
* Als de uitvoer gepartitioneerd is met behulp van het voorvoegsel padpatroon tijd, wordt een nieuwe blob wordt gebruikt wanneer de query naar het volgende uur verplaatst.
* Als de uitvoer van een aangepast veld is gepartitioneerd, wordt een nieuwe blob per partitiesleutel gemaakt als deze niet bestaat.
* Als de uitvoer van een aangepast veld waarin de kardinaliteit van de partitie is groter dan 8000 is gepartitioneerd, kan een nieuwe blob per partitiesleutel worden gemaakt.

## <a name="event-hub"></a>Event Hub
De [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) -service is een zeer schaalbare publish-subscribe ingestor voor het event. Het kan miljoenen gebeurtenissen per seconde verzamelen. Een gebruik van een Event Hub als uitvoer is als de uitvoer van een Stream Analytics-taak de invoer van een ander streaming-taak wordt.

Er zijn een aantal parameters die nodig zijn voor de Event Hub-gegevensstromen configureren als uitvoer.

| Naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeralias | Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze Event Hub te regelen. |
| Event hub-naamruimte |Een Event Hub-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe Event Hub hebt gemaakt, is ook een Event Hub-naamruimte gemaakt. |
| Event Hub-naam | De naam van de Event Hub-uitvoer. |
| Naam van het Event Hub-beleid | Het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad configureren van Event Hub. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| Sleutel voor het Event Hub-beleid | De gedeelde toegangssleutel gebruikt voor het verifiëren van toegang tot de Event Hub-naamruimte. |
| Partitiesleutelkolom (optioneel) | Deze kolom bevat de partitiesleutel voor Event Hub-uitvoer. |
| Serialisatie-indeling voor gebeurtenissen | Serialisatie-indeling voor de uitvoergegevens.  JSON-, CSV- en Avro worden ondersteund. |
| Encoding | Voor CSV en JSON is UTF-8 de enige ondersteunde coderingsindeling op dit moment. |
| Scheidingsteken | Alleen van toepassing voor de CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn met door komma's, door puntkomma's, spatie, tab en verticale balk. |
| Indeling | Alleen van toepassing op JSON-serialisatie. Lijn gescheiden geeft aan dat de uitvoer is geformatteerd met elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt opgemaakt als een matrix met JSON-objecten. Deze matrix wordt gesloten wanneer de taak stopt of Stream Analytics is verplaatst op naar de volgende periode. In het algemeen is het beter gebruik van de regel gescheiden JSON, omdat er geen speciale verwerking niet vereist, terwijl het uitvoerbestand is nog steeds worden geschreven naar. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) kan worden gebruikt als uitvoer voor een Stream Analytics-taak te voorzien in een krachtige visualisatie-ervaring van de resultaten van de analyse. Deze mogelijkheid kan worden gebruikt voor operationele dashboards, rapporten genereren en de metrische gegevens gestuurde reporting.

Power BI-uitvoer van Stream Analytics is momenteel niet beschikbaar in de Azure China (21Vianet) en regio's van Azure Duitsland (T-Systems International).

### <a name="authorize-a-power-bi-account"></a>Power BI-account toestaan
1. Wanneer Power BI is geselecteerd als uitvoer in Azure portal, maakt u wordt gevraagd om een bestaande Power BI-gebruiker of een nieuw Power BI-account maken.  
   
   ![Autorisatie van Power BI-gebruiker](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2. Maak een nieuw account als u niet nog een, en klik vervolgens op nu autoriseren.  De volgende pagina wordt weergegeven:
   
   ![Azure-Account Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3. In deze stap maakt het account voor werk of school te bieden voor het verlenen van de uitvoer van de Power BI. Als u bent nog niet aangemeld voor Power BI, kiest u nu Sign up. Het werk- of schoolaccount waarmee u voor Power BI kan afwijken van het account Azure-abonnement, dat u momenteel bent aangemeld.

### <a name="configure-the-power-bi-output-properties"></a>De eigenschappen van de Power BI-uitvoer configureren
Zodra u de Power BI-account dat is geverifieerd hebt, kunt u de eigenschappen configureren voor uw Power BI-uitvoer. De onderstaande tabel wordt de lijst met namen van eigenschappen en hun beschrijving van het configureren van de uitvoer van uw Power BI.

| Naam van eigenschap | description |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar de uitvoer van deze Power BI te regelen. |
| Groepswerkruimte |U kunt groepen in uw Power BI-account selecteren of 'Mijn werkruimte' kiezen als u niet wilt schrijven naar een groep om in te schakelen delen van gegevens met andere Power BI-gebruikers.  Bijwerken van een bestaande groep is vereist voor het vernieuwen van de Power BI-verificatie. |
| Naam van de gegevensset |Geef een gegevenssetnaam dat het nodig is voor de uitvoer van de Power BI gebruiken |
| Tabelnaam |Geef een tabelnaam onder de gegevensset van de Power BI-uitvoer. Op dit moment kan Power BI-uitvoer van Stream Analytics-taken alleen hebben één tabel in een gegevensset |

Zie voor een overzicht van het configureren van een Power BI-uitvoer en -dashboard, de [Azure Stream Analytics en Power BI](stream-analytics-power-bi-dashboard.md) artikel.

> [!NOTE]
> Expliciet Maak geen de gegevensset en de tabel in de Power BI-dashboard. De gegevensset en de tabel wordt automatisch gevuld wanneer de taak wordt gestart en de taak wordt gestart reageert uitvoer naar Power BI. Merk op dat als de taakquery geen resultaten, de gegevensset niet genereren en de tabel is niet gemaakt. Houd er rekening mee dat als Power BI al een gegevensset en een tabel met dezelfde naam als een van deze Stream Analytics-taak wordt opgegeven, de bestaande gegevens worden overschreven.
> 

### <a name="schema-creation"></a>Schema maken
Azure Stream Analytics maakt u een Power BI-gegevensset en een tabel namens de gebruiker als deze niet al bestaat. In alle andere gevallen moet wordt de tabel bijgewerkt met nieuwe waarden. Er is op dit moment een beperking die slechts één tabel kan bestaan in een gegevensset.

### <a name="data-type-conversion-from-stream-analytics-to-power-bi"></a>Conversie van Stream Analytics naar Power BI-gegevenstype
Azure Stream Analytics bijgewerkt het gegevensmodel in runtime dynamisch als het uitvoerschema wordt gewijzigd. Alle worden wijzigingen in de kolom naam, de wijzigingen van het type kolom, en het toevoegen of verwijderen van kolommen bijgehouden.

Deze tabel bevat informatie over de conversie van het gegevenstype van [Stream Analytics-gegevenstypen](https://msdn.microsoft.com/library/azure/dn835065.aspx) naar BIs Power [Entity Data Model (EDP) typen](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) als een POWER BI-gegevensset en een tabel niet bestaat.

Van Stream Analytics | Met Power BI
-----|-----|------------
bigint | Int64
nvarchar(max) | Reeks
datum/tijd | Datum en tijd
float | Double-waarde
Matrix van de record | Type, constante waarde "Irecords" of "IArray" tekenreeks

### <a name="schema-update"></a>Schema-Update
Stream Analytics bepaalt welk data model-schema op basis van de eerste set met gebeurtenissen in de uitvoer. Later, indien nodig, wordt het modelschema gegevens bijgewerkt om te voldoen aan inkomende gebeurtenissen die niet zijn voor in de oorspronkelijke schema geschikt mogelijk.

De `SELECT *` query om te voorkomen dat dynamische schema-update in rijen moet worden vermeden. Naast de mogelijke gevolgen voor de prestaties, kan dit ook onzekerheid of de gebruikte tijd voor de resultaten leiden. De exacte velden die moeten worden weergegeven in Power BI-dashboard moeten worden geselecteerd. Bovendien moeten de gegevenswaarden compatibel zijn met het gekozen type zijn.


Vorige/huidige | Int64 | Reeks | Datum en tijd | Double-waarde
-----------------|-------|--------|----------|-------
Int64 | Int64 | Reeks | Reeks | Double-waarde
Double-waarde | Double-waarde | Reeks | Reeks | Double-waarde
Reeks | Reeks | Reeks | Reeks |  | Reeks | 
Datum en tijd | Reeks | Reeks |  Datum en tijd | Reeks


### <a name="renew-power-bi-authorization"></a>Vernieuw de autorisatie van Power BI
Als het wachtwoord van uw Power BI-account wordt gewijzigd nadat uw Stream Analytics-taak is gemaakt of laatst geverifieerd, moet u verifiëren van de Stream Analytics. Als multi-factor Authentication (MFA) is geconfigureerd op uw Azure Active Directory (AAD)-tenant, moet u ook Power BI-autorisatie elke twee weken vernieuwen. Een symptoom zijn van dit probleem is geen taakuitvoer en een "verifiëren gebruiker error" in de logboeken voor bewerkingen:

  ![Power BI refresh token fout](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

De actieve taak stoppen om op te lossen dit probleem, en Ga naar de uitvoer van uw Power BI.  Selecteer de **autorisatie vernieuwen** koppelen en start de taak van de **gestopt laatst** om gegevensverlies te voorkomen.

  ![Power BI wordt vernieuwd autorisatie](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Table Storage
[Azure-tabelopslag](../storage/common/storage-introduction.md) biedt een zeer beschikbare en zeer schaalbare opslag, zodat een toepassing kan automatisch worden geschaald om te voldoen aan de behoeften van de gebruiker. Tabelopslag is van Microsoft NoSQL-sleutel-/ kenmerkopslag, welke voor gestructureerde gegevens met minder beperkingen met betrekking tot het schema gebruiken kunt. Azure Table storage kan worden gebruikt voor het opslaan van gegevens voor de persistentie en efficiënte ophalen.

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van een van de tabeluitvoer.

| De naam van eigenschap | description |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze tabelopslag te regelen. |
| Storage-account |De naam van het opslagaccount waar u de uitvoer verzendt. |
| Opslagaccountsleutel |De toegangssleutel die is gekoppeld aan de storage-account. |
| Tabelnaam |De naam van de tabel. De tabel wordt gemaakt als deze niet bestaat. |
| Partitiesleutel |De naam van de uitvoerkolom die de partitiesleutel bevat. De partitiesleutel is een unieke id voor de partitie binnen een bepaalde tabel, die bestaat uit het eerste deel van de primaire sleutel van een entiteit. Het is een tekenreekswaarde die maximaal 1 KB groot kan zijn. |
| Rijsleutel |De naam van de uitvoerkolom die de rijsleutel bevat. De rijsleutel is een unieke id van een entiteit binnen een bepaalde partitie. Deze bestaat uit het tweede gedeelte van de primaire sleutel van een entiteit. De rijsleutel is een tekenreekswaarde die maximaal 1 KB groot kan zijn. |
| Batchgrootte |Het aantal records voor een batchbewerking. De standaardwaarde (100) is voldoende voor de meeste taken. Raadpleeg de [tabel batchbewerking spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) voor meer informatie over het aanpassen van deze instelling. |
 
## <a name="service-bus-queues"></a>Service Bus-wachtrijen
[Service Bus-wachtrijen](https://msdn.microsoft.com/library/azure/hh367516.aspx) bieden een First In, eerste uit FIFO-principe ()-berichtbezorging naar een of meer concurrerende consumenten. Normaal gesproken wordt berichten te ontvangen en verwerkt door de ontvangers in de tijdsvolgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht wordt ontvangen en verwerkt door slechts één berichtconsument.

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van een wachtrij van de uitvoer.

| Naam van eigenschap | description |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om te leiden van de query-uitvoer aan deze Service Bus-wachtrij. |
| Service Bus-naamruimte |Een Service Bus-naamruimte is een container voor een set berichtentiteiten. |
| Wachtrijnaam |De naam van de Service Bus-wachtrij. |
| Naam van het wachtrijbeleid |Wanneer u een wachtrij maakt, kunt u ook beleid voor gedeelde toegang maken op het tabblad configureren van de wachtrij. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| Sleutel voor het wachtrijbeleid |De toegang tot gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte |
| Serialisatie-indeling voor gebeurtenissen |Serialisatie-indeling voor de uitvoergegevens.  JSON-, CSV- en Avro worden ondersteund. |
| Encoding |Voor CSV en JSON is UTF-8 de enige ondersteunde coderingsindeling op dit moment |
| Scheidingsteken |Alleen van toepassing voor de CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn met door komma's, door puntkomma's, spatie, tab en verticale balk. |
| Indeling |Alleen van toepassing voor JSON-type. Lijn gescheiden geeft aan dat de uitvoer is geformatteerd met elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt opgemaakt als een matrix met JSON-objecten. |

Het aantal partities is [op basis van de Service Bus-SKU en grootte](../service-bus-messaging/service-bus-partitioning.md). Partitiesleutel is een unieke integer-waarde voor elke partitie.

## <a name="service-bus-topics"></a>Service Bus-onderwerpen
Hoewel Service Bus-wachtrijen een methode voor communicatie in één van afzender naar ontvanger bieden, [Service Bus-onderwerpen](https://msdn.microsoft.com/library/azure/hh367516.aspx) voorzien in de vorm van een een-op-veel communicatie.

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van een van de tabeluitvoer.

| Naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze Service Bus-onderwerp te regelen. |
| Service Bus-naamruimte |Een Service Bus-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe Event Hub hebt gemaakt, u ook een Service Bus-naamruimte gemaakt |
| Onderwerpnaam |Onderwerpen zijn berichtentiteiten, vergelijkbaar met eventhubs en wachtrijen. Ze zijn ontworpen om gebeurtenisstromen te verzamelen uit een aantal verschillende apparaten en services. Wanneer een onderwerp is gemaakt, krijgt deze ook een specifieke naam. De berichten die worden verzonden naar een onderwerp is niet beschikbaar, tenzij er een abonnement is gemaakt, dus zorg ervoor dat er zijn een of meer abonnementen onder het onderwerp |
| Naam van het onderwerpbeleid |Wanneer u een onderwerp maakt, kunt u ook beleid voor gedeelde toegang maken op het tabblad configureren van het onderwerp. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels |
| Sleutel voor het onderwerpbeleid |De toegang tot gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte |
| Serialisatie-indeling voor gebeurtenissen |Serialisatie-indeling voor de uitvoergegevens.  JSON-, CSV- en Avro worden ondersteund. |
| Encoding |Als u CSV of JSON-indeling, moet een codering worden opgegeven. UTF-8 is de enige ondersteunde coderingsindeling op dit moment |
| Scheidingsteken |Alleen van toepassing voor de CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn met door komma's, door puntkomma's, spatie, tab en verticale balk. |

Het aantal partities is [op basis van de Service Bus-SKU en grootte](../service-bus-messaging/service-bus-partitioning.md). Partitiesleutel is een unieke integer-waarde voor elke partitie.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) is een wereldwijd gedistribueerde, multi-model database service dat biedt onbeperkt flexibel schalen over de hele wereld, uitgebreide query en automatische indexering via schema-agnostische gegevensmodellen, gegarandeerde lage latentie en toonaangevende uitgebreide Sla's. Raadpleeg voor meer informatie over opties voor het verzamelen van Cosmos DB voor Stream Analytics, de [Stream Analytics met Cosmos DB als uitvoer](stream-analytics-documentdb-output.md) artikel.

Azure Cosmos DB-uitvoer van Stream Analytics is momenteel niet beschikbaar in de Azure China (21Vianet) en regio's van Azure Duitsland (T-Systems International).

> [!Note]
> Op dit moment ondersteunt Azure Stream Analytics alleen verbinding met CosmosDB met **SQL API**.
> Andere Azure Cosmos DB-API's zijn nog niet ondersteund. Als u punt Azure Stream Analytics met de Azure Cosmos DB-accounts die zijn gemaakt met andere API's, zijn de gegevens mogelijk niet juist opgeslagen. 

De volgende tabel beschrijft de eigenschappen voor het maken van een Azure Cosmos DB-uitvoer.
| Naam van eigenschap | description |
| --- | --- |
| Uitvoeralias | Een alias om te verwijzen deze uitvoer in uw Stream Analytics-query. |
| Sink | Cosmos DB |
| Importoptie | Kies een 'Cosmosdb selecteren bij uw abonnement', of ' Geef Cosmos DB-instellingen handmatig '.
| Account-id | De naam of URI van het Cosmos DB-account van het eindpunt. |
| Accountcode | De gedeelde toegangssleutel voor het Cosmos DB-account. |
| Database | De naam van de Cosmos DB-database. |
| Patroon voor de collectienaam | Naam van de verzameling of het patroon voor de verzamelingen op die moet worden gebruikt. <br/>De indeling van de verzameling kan worden samengesteld met behulp van de optionele {partition}-token, waarbij partities beginnen bij 0. Twee voorbeelden:  <br/>1. _MyCollection_ : een verzameling met de naam 'MyCollection' moet bestaan.  <br/>2. _MyCollection {partition}_ – op basis van de partitioneringskolom. <br/>De partitionering kolom verzamelingen moeten aanwezig zijn: 'MyCollection0","MyCollection1","MyCollection2", enzovoort. |
| Partitiesleutel | Optioneel. Dit is alleen nodig als u een token {partition} in het patroon voor de verzameling.<br/> De partitiesleutel is de naam van het veld in uitvoergebeurtenissen dat wordt gebruikt om op te geven van de sleutel voor het partitioneren van uitvoer in collecties.<br/> Voor één verzameling uitvoer, kan elke willekeurige uitvoerkolom worden gebruikt. Bijvoorbeeld, PartitionId. |
| Document-id |Optioneel. De naam van het veld in uitvoergebeurtenissen dat wordt gebruikt om op te geven van de primaire sleutel op welke invoegen of bijwerken bewerkingen zijn gebaseerd.  

## <a name="azure-functions"></a>Azure Functions
Azure Functions is een serverloze compute-service waarmee u code op aanvraag kunt uitvoeren zonder expliciet een infrastructuur in te richten of te beheren. Hiermee kunt u code te implementeren die wordt geactiveerd door gebeurtenissen in Azure of services van derden.  Deze mogelijkheid van Azure Functions om op triggers te reageren, is het een natuurlijke uitvoer voor een Azure Stream Analytics. Deze uitvoeradapter kan gebruikers Stream Analytics verbinden met Azure Functions en een script of een stukje code uitvoeren in reactie op een aantal gebeurtenissen.

Azure Functions-uitvoer van Stream Analytics is momenteel niet beschikbaar in de Azure China (21Vianet) en regio's van Azure Duitsland (T-Systems International).

Azure Stream Analytics activeert Azure Functions via HTTP-triggers. De nieuwe Azure-functie uitvoeradapter is beschikbaar met de volgende eigenschappen kunnen worden geconfigureerd:

| Naam van eigenschap | description |
| --- | --- |
| Function App |Naam van uw Azure Functions-App |
| Functie |Naam van de functie in uw Azure Functions-App |
| Sleutel |Als u een Azure-functie van een ander abonnement gebruiken wilt, kunt u doen door op te geven van de sleutel voor toegang tot uw functie |
| Maximale batchgrootte |Deze eigenschap kan worden gebruikt om in te stellen de maximale grootte voor elke uitvoerbatch die wordt verzonden naar uw Azure-functie. Deze waarde is standaard 256 KB |
| Maximum aantal batches  |Als de naam aangeeft, wordt deze eigenschap kunt u het maximum aantal gebeurtenissen in elke batch wordt verzonden naar Azure Functions opgeven. De standaardwaarde voor de maximale batch-aantal is 100 |

Wanneer Azure Stream Analytics 413 (HTTP-aanvragen aanvraagentiteit te groot)-uitzondering van Azure-functie ontvangt, vermindert het de grootte van de batches deze naar Azure Functions verzendt. In de code van uw Azure-functie, gebruikt u deze uitzondering om ervoor te zorgen dat Azure Stream Analytics geen te grote batches niet verzenden. Zorg er ook voor zorgen dat de maximale batch-aantal en grootte waarden zijn gebruikt in de functie consistent zijn met de waarden hebt ingevoerd in de Stream Analytics-portal. 

Ook in een situatie waarbij er geen gebeurtenis terechtkomen in een bepaalde periode, wordt geen uitvoer gegenereerd. Als gevolg hiervan is computeResult-functie niet aangeroepen. Dit gedrag is consistent met de ingebouwde statistische functies in vensters.

## <a name="partitioning"></a>Partitionering

De volgende tabel geeft een overzicht van de partitie ondersteuning en het aantal schrijvers van de uitvoer voor elk uitvoertype:

| Uitvoertype | Ondersteuning voor partitioneren | Partitiesleutel  | Aantal uitvoer schrijvers | 
| --- | --- | --- | --- |
| Azure Data Lake Store | Ja | Gebruik {date} en {time} tokens in het patroon voor het voorvoegsel van pad. Kies de notatie voor datum, zoals jjjj/MM/DD, jjjj/MM/jjjj, MM-DD-JJJJ. HH wordt gebruikt voor de tijdnotatie. | De invoer voor het partitioneren van volgt [volledig worden opgestart query's](stream-analytics-scale-jobs.md). | 
| Azure SQL Database | Ja | Op basis van de component PARTITION BY in de query | De invoer voor het partitioneren van volgt [volledig worden opgestart query's](stream-analytics-scale-jobs.md). | 
| Azure Blob Storage | Ja | Gebruik {date} en {time} tokens van de gebeurtenis velden in het pad-patroon. Kies de notatie voor datum, zoals jjjj/MM/DD, jjjj/MM/jjjj, MM-DD-JJJJ. HH wordt gebruikt voor de tijdnotatie. Als onderdeel van de [preview](https://aka.ms/ASApreview1), blob-uitvoer kan worden gepartitioneerd op een kenmerk met één aangepaste gebeurtenis {fieldname} of {datum/tijd:\<aanduiding >}. | De invoer voor het partitioneren van volgt [volledig worden opgestart query's](stream-analytics-scale-jobs.md). | 
| Azure Event Hub | Ja | Ja | Is afhankelijk van de uitlijning van de partitie.</br> Als uitvoer van de uitvoer Event Hub-partitiesleutel is even uitgelijnd met upstream (vorige) querystap is het aantal schrijvers is hetzelfde van het aantal Event Hub-partities. Elke schrijver maakt gebruik van de EventHub [EventHubSender klasse](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) voor het verzenden van gebeurtenissen naar de specifieke partitie. </br> Wanneer de uitvoer Event Hub-partitiesleutel is niet uitgelijnd met upstream (vorige) querystap is het aantal schrijvers is hetzelfde als het aantal partities in die de vorige stap. EventHubClient maakt gebruik van elke schrijver [SendBatchAsync klasse](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) gebeurtenissen verzenden naar alle partities van de uitvoer. |
| Power BI | Nee | Geen | Niet van toepassing. | 
| Azure Table Storage | Ja | Elke uitvoerkolom.  | De invoer voor het partitioneren van volgt [volledig geparallelliseerd query's](stream-analytics-scale-jobs.md). | 
| Azure Service Bus-onderwerp | Ja | Automatisch gekozen. Het aantal partities is gebaseerd op de [Service Bus-SKU's en grootte](../service-bus-messaging/service-bus-partitioning.md). Partitiesleutel is een unieke integer-waarde voor elke partitie.| Hetzelfde als het aantal partities in het onderwerp van de uitvoer.  |
| Azure Service Bus-wachtrij | Ja | Automatisch gekozen. Het aantal partities is gebaseerd op de [Service Bus-SKU's en grootte](../service-bus-messaging/service-bus-partitioning.md). Partitiesleutel is een unieke integer-waarde voor elke partitie.| Hetzelfde als het aantal partities in de uitvoerwachtrij. |
| Azure Cosmos DB | Ja | Token {partition} in het patroon voor de Collectienaam gebruiken. {partition}-waarde is gebaseerd op de component PARTITION BY in de query. | De invoer voor het partitioneren van volgt [volledig geparallelliseerd query's](stream-analytics-scale-jobs.md). |
| Azure Functions | Nee | Geen | Niet van toepassing. | 

## <a name="output-batch-size"></a>Batchgrootte voor uitvoer
Azure Stream Analytics maakt gebruik van variabele grootte batches gebeurtenissen verwerken en naar uitvoer schrijven. Normaal gesproken de Stream Analytics-engine schrijft een bericht niet op een tijdstip en batches gebruikt voor efficiëntie. Wanneer zowel de inkomende en de uitgaande snelheid van gebeurtenissen hoog is, gebruikt het grotere batches. Wanneer de snelheid van uitgangsgebeurtenissen laag is, gebruikt het kleinere batches latentie laag te houden. 

De volgende tabel worden enkele van de overwegingen voor de uitvoer batchverwerking uitgelegd:

| Uitvoertype | Maximale berichtlengte | Optimalisatie van de grootte van batch |
| :--- | :--- | :--- | 
| Azure Data Lake Store | Zie [Data Lake-opslag wordt beperkt](../azure-subscription-service-limits.md#data-lake-store-limits) | Maximaal 4 MB per schrijfbewerking |
| Azure SQL Database | 10.000 maximum aantal rijen per één bulksgewijs invoegen</br>Min 100 rijen per één bulksgewijs invoegen </br>Zie ook [limieten van Azure SQL](../sql-database/sql-database-resource-limits.md) |  Elke batch is in eerste instantie bulksgewijs invoegen met maximale batchgrootte en batch kan worden gesplitst in helft (tot batchgrootte Min) op basis van herstelbare fouten van SQL. |
| Azure Blob Storage | Zie [limieten van Azure Storage](../azure-subscription-service-limits.md#storage-limits) | Maximale blokgrootte van de Blob is 4 MB</br>Maximum aantal van de blobs bock is 50000 |
| Azure Event Hub   | 256 KB per bericht </br>Zie ook [beperkt van Event Hubs](../event-hubs/event-hubs-quotas.md) |    Bij het partitioneren van uitvoer van de invoer niet uitlijnen, wordt elke gebeurtenis afzonderlijk in een EventData verpakt en verzonden in een batch van maximaal de maximale berichtgrootte (1 MB voor Premium-SKU). </br></br>  Als het partitioneren van Input / Output is uitgelijnd, worden meerdere gebeurtenissen verpakt in een enkele EventData maximaal de maximale berichtgrootte en verzonden.    |
| Power BI | Zie [Power BI Rest-API-limieten](https://msdn.microsoft.com/library/dn950053.aspx) |
| Azure Table Storage | Zie [limieten van Azure Storage](../azure-subscription-service-limits.md#storage-limits) | Standaard is 100 entiteiten per één transactie, en kan worden geconfigureerd voor een kleinere waarde indien nodig. |
| Azure Service Bus-wachtrij   | 256 KB per bericht</br> Zie ook [Service Bus is beperkt](../service-bus-messaging/service-bus-quotas.md) | Één gebeurtenis per bericht |
| Azure Service Bus-onderwerp | 256 KB per bericht</br> Zie ook [Service Bus is beperkt](../service-bus-messaging/service-bus-quotas.md) | Één gebeurtenis per bericht |
| Azure Cosmos DB   | Zie [limieten van Azure Cosmos DB](../azure-subscription-service-limits.md#azure-cosmos-db-limits) | Batch de grootte en frequentie is aangepast dynamisch op basis van cosmos DB-antwoorden schrijven. </br> Er is geen vooraf bepaalde beperkingen van Stream Analytics. |
| Azure Functions   | | Standaardbatchgrootte is 246 KB. </br> Standaardaantal per batch is 100. </br> De batchgrootte is configureerbaar en kan worden vergroot of verkleind in de Stream Analytics [uitvoeropties](#azure-functions). 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Snelstart: Een Stream Analytics-taak maken met behulp van de Azure-portal](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
