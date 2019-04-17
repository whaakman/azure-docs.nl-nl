---
title: Inzicht in de uitvoer van Azure Stream Analytics
description: Dit artikel beschrijft de opties voor uitvoer gegevens beschikbaar zijn in Azure Stream Analytics, met inbegrip van Power BI voor analyseresultaten.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 3/25/2019
ms.custom: seodec18
ms.openlocfilehash: 03871c3f3627e85cc2af2f05a5fba38bd8069a15
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59609486"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Inzicht in de uitvoer van Azure Stream Analytics
Dit artikel wordt beschreven welke typen beschikbaar zijn voor een Azure Stream Analytics-taak uitvoer. Uitvoer kunnen u opslaan en sla de resultaten van de Stream Analytics-taak. U kunt doen met behulp van de uitvoergegevens, meer business analytics en datawarehousing van uw gegevens.

Bij het ontwerpen van uw Stream Analytics-query, verwijzen naar de naam van de uitvoer met behulp van de [component INTO](https://msdn.microsoft.com/azure/stream-analytics/reference/into-azure-stream-analytics). U kunt een enkele uitvoer per taak of meerdere uitvoer per streaming-taak (als u deze nodig hebt) door te geven meerdere INTO-componenten in de query.

Maken, bewerken en testen van Stream Analytics-taak weergeeft, kunt u de [Azure-portal](stream-analytics-quick-create-portal.md#configure-job-output), [Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.NET API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [REST-API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output), en [Visual Studio](stream-analytics-quick-create-vs.md).

Sommige typen ondersteuning voor uitvoer [partitioneren](#partitioning). [Uitvoer van de batch-grootten](#output-batch-size) variëren voor het optimaliseren van doorvoer.


## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics ondersteunt [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store is een bedrijfsbrede, grootschalige opslagplaats voor big data-analysewerklasten. U kunt Data Lake Store gebruiken voor het opslaan van gegevens van elke grootte, het type en de opname-snelheid voor operationele en verkennende analyse. Stream Analytics moet zijn gemachtigd voor toegang tot Data Lake Store.

Azure Data Lake Store-uitvoer van Stream Analytics is momenteel niet beschikbaar in de Azure China (21Vianet) en regio's van Azure Duitsland (T-Systems International).

### <a name="authorize-an-azure-data-lake-store-account"></a>Een Azure Data Lake Store-account toestaan

1. Wanneer u Data Lake Store als uitvoer in Azure portal selecteert, wordt u gevraagd om een verbinding met een bestaand Data Lake Store-exemplaar te autoriseren.

   ![Een verbinding met Data Lake Store toestaan](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)

2. Als u al toegang tot Data Lake Store hebt, selecteert u **autoriseren nu**. Een pagina wordt weergegeven en geeft aan dat **omleiden naar autorisatie**. Nadat de autorisatie is gelukt, krijgt u de pagina die u kunt de uitvoer van de Data Lake Store configureren.

3. Nadat u de Data Lake Store-account dat is geverifieerd hebt, kunt u de eigenschappen configureren voor uw Data Lake Store-uitvoer.

   ![Het definiëren van Data Lake Store als uitvoer van de Stream Analytics](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)

De volgende tabel bevat de namen van eigenschappen en de bijbehorende beschrijvingen de uitvoer van uw Data Lake Store configureren.   

| Naam van eigenschap | Description |
| --- | --- |
| Uitvoeralias | Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar Data Lake Store te regelen. |
| Accountnaam | De naam van het Data Lake Store-account waar u de uitvoer verzendt. U ziet een vervolgkeuzelijst met Data Lake Store-accounts die beschikbaar in uw abonnement zijn. |
| Voorvoegsel van het padpatroon | Het bestandspad dat wordt gebruikt om uw bestanden in de opgegeven Data Lake Store-account te schrijven. U kunt opgeven van een of meer exemplaren van de {date} en {time} variabelen:<br /><ul><li>Voorbeeld 1: Map1/logboeken / {date} / {time}</li><li>Voorbeeld 2: Map1/logboeken / {date}</li></ul><br />De tijdstempel van de gemaakte mapstructuur volgt UTC en niet de lokale tijd.<br /><br />Als het pad naar bestandspatroon geen afsluitende schuine streep (/) bevat, wordt het laatste patroon in het bestandspad wordt behandeld als het voorvoegsel van een bestand. <br /><br />Nieuwe bestanden worden gemaakt in de volgende situaties:<ul><li>In de uitvoerschema wijzigen</li><li>Externe of interne opnieuw opstarten van een taak</li></ul> |
| Datumnotatie | Optioneel. Als de datumtoken van de in het pad van het voorvoegsel wordt gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld. Voorbeeld: JJJJ/MM/DD |
|Tijdnotatie | Optioneel. Als de token van de tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de indeling waarin de bestanden zijn ingedeeld. De enige ondersteunde waarde is momenteel HH. |
| Serialisatie-indeling voor gebeurtenissen | De serialisatie-indeling voor de uitvoergegevens. JSON-, CSV- en Avro worden ondersteund.|
| Encoding | Als u van CSV of JSON-indeling gebruikmaakt, moet een codering worden opgegeven. Alleen de coderingsindeling UTF-8 wordt momenteel ondersteund.|
| Scheidingsteken | Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn met door komma's, door puntkomma's, spatie, tab en verticale balk.|
| Indeling | Alleen van toepassing op JSON-serialisatie. **Regel gescheiden** geeft aan dat de uitvoer is geformatteerd met elk JSON-object dat is gescheiden door een nieuwe regel. **Matrix** geeft aan dat de uitvoer wordt opgemaakt als een matrix met JSON-objecten. Deze matrix wordt gesloten wanneer de taak stopt of Stream Analytics is verplaatst op naar de volgende periode. Het is in het algemeen beter regel gescheiden JSON gebruiken omdat er is geen speciale verwerking geen vereist terwijl is nog steeds naar het uitvoerbestand worden geschreven.|

### <a name="renew-data-lake-store-authorization"></a>Vernieuw de autorisatie van Data Lake Store
U moet uw Data Lake Store-account te verifiëren als het wachtwoord is gewijzigd sinds de taak is gemaakt of laatst geverifieerd. Als u niet opnieuw worden geverifieerd, wordt de taak produceert geen uitvoer resultaten en ziet u een foutbericht weergegeven dat de noodzaak van opnieuw te autoriseren in de logboeken voor bewerkingen. 

Op dit moment moet het verificatietoken handmatig worden vernieuwd om de 90 dagen voor alle taken met Data Lake Store-uitvoer. U kunt strijden tegen deze beperking door [verifiëren via beheerde identiteiten (preview)](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-managed-identities-adls).

Autorisatie vernieuwen:

1. Selecteer **stoppen** om uw taak te stoppen.
1. Ga naar uw Data Lake Store-uitvoer en selecteer de **autorisatie vernieuwen** koppeling.

   Voor een korte tijd een pop-pagina geeft aan dat **omleiden naar autorisatie**. De pagina wordt aangegeven als autorisatie geslaagd is, **autorisatie is vernieuwd** en vervolgens automatisch wordt gesloten. 
   
1. Selecteer **opslaan** aan de onderkant van de pagina. U kunt vervolgens opnieuw starten de taak van de **gestopt laatst** om gegevensverlies te voorkomen.

![Vernieuw de autorisatie van de Data Lake Store in de uitvoer](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)

## <a name="sql-database"></a>SQL Database
U kunt [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) als uitvoer voor gegevens die relationele of voor toepassingen die afhankelijk zijn van de inhoud wordt gehost in een relationele database. Stream Analytics-taken schrijven naar een bestaande tabel in SQL-Database. Het tabelschema moet exact overeenkomen met de velden en hun typen in de uitvoer van uw taak. U kunt ook opgeven [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) uitvoer optie als uitvoer via de SQL-Database. Zie voor meer informatie over manieren om te schrijven-doorvoer te verbeteren, de [Stream Analytics met Azure SQL Database als uitvoer](stream-analytics-sql-output-perf.md) artikel. 

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van de uitvoer van een SQL-Database.

| Naam van eigenschap | Description |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer met deze database te regelen. |
| Database | De naam van de database waar u de uitvoer verzendt. |
| Servernaam | De naam van de SQL-Database-server. |
| Gebruikersnaam | De gebruikersnaam die schrijftoegang tot de database heeft. Stream Analytics ondersteunt alleen SQL-verificatie. |
| Wachtwoord | Het wachtwoord voor verbinding met de database. |
| Tabel | De naam van de tabel waar de uitvoer wordt geschreven. Naam van de tabel is hoofdlettergevoelig. Het schema van deze tabel moet exact overeenkomen met het aantal velden en hun typen die de taakuitvoer wordt gegenereerd. |
|Partitieschema overnemen| Een optie voor het overnemen van het partitieschema van de vorige querystap om in te schakelen volledig parallelle-topologie met meerdere makers in de tabel. Zie voor meer informatie, [Azure Stream Analytics-uitvoer naar Azure SQL Database](stream-analytics-sql-output-perf.md).|
|Batch-aantal overeenkomsten| De aanbevolen limiet voor het aantal records dat is verzonden met elke bulksgewijs invoegen transactie.|

> [!NOTE]
> De aanbieding Azure SQL Database wordt momenteel ondersteund voor de taakuitvoer van een in Stream Analytics. Een Azure-VM met SQL Server met een database die is gekoppeld, wordt niet ondersteund. Dit is mogelijk gewijzigd in toekomstige releases.
>

## <a name="blob-storage"></a>Blob Storage
Azure Blob-opslag biedt een voordelige en schaalbare oplossing voor het opslaan van grote hoeveelheden ongestructureerde gegevens in de cloud. Zie voor een inleiding op Blob-opslag en het gebruik ervan [over het gebruik van blobs](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

De volgende tabel bevat de namen van eigenschappen en de bijbehorende beschrijvingen voor het maken van een blob-uitvoer.

| Naam van eigenschap       | Description                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| Uitvoeralias        | Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze blobopslag te regelen. |
| Storage-account     | De naam van het opslagaccount waar u de uitvoer verzendt.               |
| Opslagaccountsleutel | De geheime sleutel die is gekoppeld aan de storage-account.                              |
| Opslagcontainer   | Een logische groepering van blobs die zijn opgeslagen in de Azure Blob-service. Wanneer u een blob geüpload naar de Blob-service, moet u een container voor die blob opgeven. |
| Padpatroon | Optioneel. Het pad-patroon voor het bestand dat wordt gebruikt voor het schrijven van uw blobs in de opgegeven container. <br /><br /> In het padpatroon, kunt u een of meer exemplaren van de datum en tijd variabelen gebruiken om op te geven van de frequentie waarmee blobs worden geschreven: <br /> {date}, {time} <br /><br />U kunt aangepaste blob partitioneren gebruiken een aangepaste {veld}-naam van uw gebeurtenisgegevens voor blobs partitie opgeven. Naam van het veld is alfanumerieke en kan spaties, afbreekstreepjes en onderstrepingstekens bevatten. De volgende beperkingen met betrekking tot aangepaste velden zijn beschikbaar: <ul><li>Veldnamen zijn niet hoofdlettergevoelig. Bijvoorbeeld kan geen de service onderscheid tussen de kolom 'ID' en kolom "id."</li><li>Geneste velden zijn niet toegestaan. Gebruik in plaats daarvan een alias in de taakquery het veld ' afvlakken '.</li><li>Expressies kunnen niet worden gebruikt als de naam van een veld.</li></ul> <br />Deze functie kunt het gebruik van aangepaste datum/tijd-indeling aanduiding configuraties in het pad. Aangepaste datum en tijd indelingen moeten worden opgegeven op een tijdstip, ingesloten door de {datum/tijd:\<aanduiding >} trefwoord. Toegestane invoer voor \<aanduiding > jjjj, MM, M, dd, d, HH, H, mm, m, ss of s zijn. De {datum/tijd:\<aanduiding >} sleutelwoord kan meerdere keren worden gebruikt in het pad om te vormen van aangepaste datum/tijd-configuraties. <br /><br />Voorbeelden: <ul><li>Voorbeeld 1: cluster1/logboeken / {date} / {time}</li><li>Voorbeeld 2: cluster1/logboeken / {date}</li><li>Voorbeeld 3: cluster1 / {client_id} / {date} / {time}</li><li>Voorbeeld 4: cluster1 / {datetime:ss} / {myField} wanneer de query is: Selecteer data.myField als myField van invoer;</li><li>Voorbeeld 5: cluster1/jaar = {datetime:yyyy} / maand = {datetime:MM} / dag = {datetime:dd}</ul><br />De tijdstempel van de gemaakte mapstructuur volgt UTC en niet de lokale tijd.<br /><br />Naamgeving van bestanden maakt gebruik van de volgende gebruikt: <br /><br />{Pad voorvoegsel Pattern}/schemaHashcode_Guid_Number.extension<br /><br />Voorbeeld van de uitvoerbestanden:<ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li>  <li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul> <br />Zie voor meer informatie over deze functie [blob partitioneren van uitvoer-Azure Stream Analytics aangepaste](stream-analytics-custom-path-patterns-blob-storage-output.md). |
| Datumnotatie | Optioneel. Als de datumtoken van de in het pad van het voorvoegsel wordt gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld. Voorbeeld: JJJJ/MM/DD |
| Tijdnotatie | Optioneel. Als de token van de tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de indeling waarin de bestanden zijn ingedeeld. De enige ondersteunde waarde is momenteel HH. |
| Serialisatie-indeling voor gebeurtenissen | Serialisatie-indeling voor de uitvoergegevens. JSON-, CSV- en Avro worden ondersteund. |
| Encoding    | Als u van CSV of JSON-indeling gebruikmaakt, moet een codering worden opgegeven. Alleen de coderingsindeling UTF-8 wordt momenteel ondersteund. |
| Scheidingsteken   | Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn met door komma's, door puntkomma's, spatie, tab en verticale balk. |
| Indeling      | Alleen van toepassing op JSON-serialisatie. **Regel gescheiden** geeft aan dat de uitvoer is geformatteerd met elk JSON-object dat is gescheiden door een nieuwe regel. **Matrix** geeft aan dat de uitvoer wordt opgemaakt als een matrix met JSON-objecten. Deze matrix wordt gesloten wanneer de taak stopt of Stream Analytics is verplaatst op naar de volgende periode. Het is in het algemeen beter regel gescheiden JSON gebruiken omdat er is geen speciale verwerking geen vereist terwijl is nog steeds naar het uitvoerbestand worden geschreven. |

Wanneer u Blob-opslag als uitvoer gebruikt, wordt een nieuw bestand wordt gemaakt in de blob in de volgende gevallen:

* Als het bestand is groter dan het maximum aantal toegestane blokken (momenteel 50.000). U mogelijk het maximale aantal toegestane blokken bereikt zonder dat de voor maximale toegestane blobgrootte is bereikt. Als de uitvoerpakketten hoog is, ziet u meer bytes per blok en het bestand is groter. Als de uitvoerpakketten laag is, elk blok is minder gegevens en de grootte van het kleiner is.
* Als er een schemawijziging in de uitvoer, en de uitvoerindeling vaste schema (CSV- en Avro) is vereist.
* Als een taak opnieuw wordt opgestart, extern door een gebruiker het stoppen en starten of intern voor onderhoud of fout bij het herstel van het systeem.
* Als de query gepartitioneerd is volledig en een wordt nieuw bestand gemaakt voor elke partitie uitvoer.
* Als de gebruiker Hiermee verwijdert u een bestand of een container van het opslagaccount.
* Als de uitvoer is het tijd is gepartitioneerd met behulp van het patroon voor het voorvoegsel van pad en een nieuwe blob wordt gebruikt wanneer de query naar het volgende uur verplaatst.
* Als de uitvoer is gepartitioneerd op basis van een aangepast veld, en wordt er een nieuwe blob per partitie sleutel als gemaakt bestaat deze niet.
* Als de uitvoer is gepartitioneerd op basis van een aangepast veld waarin de partitie kardinaliteit sleutel overschrijdt 8.000 en een nieuwe blob per partitiesleutel is gemaakt.

## <a name="event-hubs"></a>Event Hubs
De [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) -service is een zeer schaalbare publish-subscribe ingestor voor het event. Het kan miljoenen gebeurtenissen per seconde verzamelen. Een gebruik van een event hub als uitvoer is als de uitvoer van een Stream Analytics-taak de invoer van een ander streaming-taak wordt.

U moet een aantal parameters voor het configureren van data-stromen van eventhubs als uitvoer.

| Naam van eigenschap | Description |
| --- | --- |
| Uitvoeralias | Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze event hub te regelen. |
| Event hub-naamruimte |Een container voor een set berichtentiteiten. Wanneer u een nieuwe event hub hebt gemaakt, is ook een event hub-naamruimte gemaakt. |
| Naam van Event Hub | De naam van de event hub-uitvoer. |
| De naam van een Event hub-beleid | Het beleid voor gedeelde toegang, die u in de event hub maken kunt **configureren** tabblad. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| Beleidssleutel voor Event hub | De gedeelde toegangssleutel die wordt gebruikt voor het verifiëren van toegang tot de event hub-naamruimte. |
| Partitiesleutelkolom | Optioneel. Een kolom met de partitiesleutel voor event hub-uitvoer. |
| Serialisatie-indeling voor gebeurtenissen | De serialisatie-indeling voor de uitvoergegevens. JSON-, CSV- en Avro worden ondersteund. |
| Encoding | Voor CSV en JSON is UTF-8 de enige ondersteunde coderingsindeling op dit moment. |
| Scheidingsteken | Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn met door komma's, door puntkomma's, spatie, tab en verticale balk. |
| Indeling | Alleen van toepassing op JSON-serialisatie. **Regel gescheiden** geeft aan dat de uitvoer is geformatteerd met elk JSON-object dat is gescheiden door een nieuwe regel. **Matrix** geeft aan dat de uitvoer wordt opgemaakt als een matrix met JSON-objecten. Deze matrix wordt gesloten wanneer de taak stopt of Stream Analytics is verplaatst op naar de volgende periode. Het is in het algemeen beter regel gescheiden JSON gebruiken omdat er is geen speciale verwerking geen vereist terwijl is nog steeds naar het uitvoerbestand worden geschreven. |
| Eigenschappenkolommen | Optioneel. Door komma's gescheiden kolommen die moeten worden gekoppeld als gebruikerseigenschappen van het uitgaande bericht in plaats van de nettolading van de. Meer informatie over deze functie is in de sectie [eigenschappen van aangepaste metagegevens voor uitvoer](#custom-metadata-properties-for-output). |

## <a name="power-bi"></a>Power BI
U kunt [Power BI](https://powerbi.microsoft.com/) als uitvoer voor een Stream Analytics-taak te voorzien in een krachtige visualisatie-ervaring van de resultaten van de analyse. Voor operationele dashboards, rapporten genereren en de metriek gebaseerde reporting kunt u deze mogelijkheid.

Power BI-uitvoer van Stream Analytics is momenteel niet beschikbaar in de Azure China (21Vianet) en regio's van Azure Duitsland (T-Systems International).

### <a name="authorize-a-power-bi-account"></a>Power BI-account toestaan
1. Wanneer Power BI is geselecteerd als uitvoer in Azure portal, u wordt gevraagd om een bestaande Power BI-gebruiker of een nieuw Power BI-account maken.
   
   ![Autorisatie van een Power BI-gebruiker uitvoer configureren](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)

2. Maak een nieuw account als u niet nog een hebben en selecteer vervolgens **autoriseren nu**. De volgende pagina wordt weergegeven:
   
   ![Verifiëren bij Power BI uit Azure-account](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)

3. Geef het account voor werk of school voor het verlenen van de uitvoer van de Power BI. Als u zich nog niet is geregistreerd voor Power BI, selecteert u **Meld u nu**. Het account voor werk- of schoolaccount waarmee u voor Power BI kan afwijken van het account voor Azure-abonnement dat u nu bent aangemeld.

### <a name="configure-the-power-bi-output-properties"></a>De eigenschappen van de Power BI-uitvoer configureren
Nadat u de Power BI-account dat is geverifieerd hebt, kunt u de eigenschappen configureren voor uw Power BI-uitvoer. De volgende tabel bevat de namen van eigenschappen en de bijbehorende beschrijvingen om te configureren van de uitvoer van uw Power BI.

| Naam van eigenschap | Description |
| --- | --- |
| Uitvoeralias |Geef een beschrijvende naam die wordt gebruikt in query's om te leiden van de query-uitvoer naar de uitvoer van deze Power BI. |
| Groepswerkruimte |Als u wilt delen van gegevens met andere Power BI-gebruikers hebt ingeschakeld, kunt u groepen in uw Power BI-account selecteren of kiezen **mijn werkruimte** als u niet wilt schrijven naar een groep. Bijwerken van een bestaande groep is vereist voor het vernieuwen van de Power BI-verificatie. |
| Naam van de gegevensset |Geef de gegevenssetnaam van een die u wilt dat de uitvoer van de Power BI te gebruiken. |
| Tabelnaam |Geef een tabelnaam onder de gegevensset van de Power BI-uitvoer. Power BI-uitvoer van Stream Analytics-taken kan op dit moment slechts één tabel hebt in een gegevensset. |

Zie voor een overzicht van het configureren van een Power BI-uitvoer en -dashboard, de [Azure Stream Analytics en Power BI](stream-analytics-power-bi-dashboard.md) artikel.

> [!NOTE]
> Niet expliciet de gegevensset en een tabel maken in de Power BI-dashboard. De gegevensset en de tabel worden automatisch ingevuld wanneer de taak wordt gestart en de taak wordt gestart reageert uitvoer naar Power BI. Als de taakquery geen resultaten genereren heeft, worden de gegevensset en de tabel niet gemaakt. Als Power BI al een gegevensset en een tabel met dezelfde naam als die van deze Stream Analytics-taak is opgegeven, wordt de bestaande gegevens overschreven.
>

### <a name="create-a-schema"></a>Maak een schema
Azure Stream Analytics maakt een Power BI-gegevensset en tabelschema voor de gebruiker als deze nog niet bestaan. In alle andere gevallen moet wordt de tabel bijgewerkt met nieuwe waarden. Op dit moment kan slechts één tabel bestaan binnen een dataset. 

Power BI maakt gebruik van het eerste in, First out (FIFO) bewaarbeleid voor. Gegevens worden verzameld in een tabel, totdat het aantal treffers 200.000 rijen.

### <a name="convert-a-data-type-from-stream-analytics-to-power-bi"></a>Stream Analytics een gegevenstype converteren naar Power BI
Azure Stream Analytics bijgewerkt het gegevensmodel in runtime dynamisch als het uitvoerschema wordt gewijzigd. Alle worden wijzigingen in de kolom naam, de wijzigingen van het type kolom, en het toevoegen of verwijderen van kolommen bijgehouden.

Deze tabel bevat informatie over de conversie van het gegevenstype van [Stream Analytics-gegevenstypen](https://msdn.microsoft.com/library/azure/dn835065.aspx) naar Power BI [Entity Data Model (EDP) typen](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/entity-data-model), als een Power BI-gegevensset en een tabel niet bestaat.

Van Stream Analytics | Met Power BI
-----|-----
bigint | Int64
nvarchar(max) | Reeks
datum/tijd | Datum en tijd
float | Double-waarde
Matrix van de record | Type, constante waarde "Irecords" of "IArray" tekenreeks

### <a name="update-the-schema"></a>Het schema bijwerken
Stream Analytics bepaalt welk data model-schema op basis van de eerste set met gebeurtenissen in de uitvoer. Later, indien nodig, wordt het modelschema gegevens bijgewerkt om te voldoen aan inkomende gebeurtenissen die niet zijn voor in de oorspronkelijke schema geschikt mogelijk.

Voorkomen dat de `SELECT *` query om te voorkomen dat dynamische schema-update in rijen. Naast de mogelijke gevolgen voor de prestaties, kan dit de onzekerheid of de gebruikte tijd voor de resultaten leiden. Selecteer de exacte velden die moeten worden weergegeven op de Power BI-dashboard. Bovendien moeten de gegevenswaarden compatibel zijn met het gekozen type zijn.


Vorige/huidige | Int64 | Reeks | Datum en tijd | Double-waarde
-----------------|-------|--------|----------|-------
Int64 | Int64 | Reeks | Reeks | Double-waarde
Double-waarde | Double-waarde | Reeks | Reeks | Double-waarde
Reeks | Reeks | Reeks | Reeks | Reeks 
Datum en tijd | Reeks | Reeks |  Datum en tijd | Reeks


### <a name="renew-power-bi-authorization"></a>Vernieuw de autorisatie van Power BI
Als het wachtwoord van uw Power BI-account wordt gewijzigd nadat uw Stream Analytics-taak is gemaakt of laatst geverifieerd, moet u verifiëren van Stream Analytics. Als u Azure multi-factor Authentication is geconfigureerd op uw tenant Azure Active Directory (Azure AD), moet u ook om te vernieuwen van Power BI-autorisatie elke twee weken. Een symptoom zijn van dit probleem is geen taakuitvoer en een "verifiëren gebruiker error" in de logboeken voor bewerkingen:

  ![Power BI gebruikersfout verifiëren](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)

De actieve taak stoppen om op te lossen dit probleem, en Ga naar de uitvoer van uw Power BI. Selecteer de **autorisatie vernieuwen** koppelen en start de taak van de **gestopt laatst** om gegevensverlies te voorkomen.

  ![Vernieuw de autorisatie van Power BI voor uitvoer](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)

## <a name="table-storage"></a>Table Storage
[Azure-tabelopslag](../storage/common/storage-introduction.md) biedt een zeer beschikbare en zeer schaalbare opslag, zodat een toepassing kan automatisch worden geschaald om te voldoen aan de behoeften van de gebruiker. Tabelopslag is van Microsoft NoSQL-sleutel-/ kenmerkopslag, die u voor gestructureerde gegevens met minder beperkingen met betrekking tot het schema gebruiken kunt. Azure Table storage kan worden gebruikt voor het opslaan van gegevens voor de persistentie en efficiënte ophalen.

De volgende tabel bevat de namen van eigenschappen en de bijbehorende beschrijvingen voor het maken van een van de tabeluitvoer.

| Naam van eigenschap | Description |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze tabelopslag te regelen. |
| Storage-account |De naam van het opslagaccount waar u de uitvoer verzendt. |
| Opslagaccountsleutel |De toegangssleutel die is gekoppeld aan de storage-account. |
| Tabelnaam |De naam van de tabel. De tabel wordt gemaakt als deze niet bestaat. |
| Partitiesleutel |De naam van de uitvoerkolom die de partitiesleutel bevat. De partitiesleutel is een unieke id voor de partitie in een tabel die het eerste deel van de primaire sleutel van een entiteit vormen. Het is een tekenreekswaarde die maximaal 1 KB groot kan zijn. |
| Rijsleutel |De naam van de uitvoerkolom die de rijsleutel bevat. De rijsleutel is een unieke id voor een entiteit binnen een partitie. Deze bestaat uit het tweede gedeelte van de primaire sleutel van een entiteit. De rijsleutel is een tekenreekswaarde die maximaal 1 KB groot kan zijn. |
| Batchgrootte |Het aantal records voor een batchbewerking. De standaardwaarde (100) is voldoende voor de meeste taken. Zie de [tabel batchbewerking spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) voor meer informatie over het aanpassen van deze instelling. |

## <a name="service-bus-queues"></a>Service Bus-wachtrijen
[Service Bus-wachtrijen](https://msdn.microsoft.com/library/azure/hh367516.aspx) bieden een FIFO-berichtbezorging naar een of meer concurrerende consumenten. Normaal gesproken worden berichten ontvangen en verwerkt door de ontvangers in de tijdsvolgorde waarin ze zijn toegevoegd aan de wachtrij. Elk bericht wordt ontvangen en verwerkt door slechts één berichtconsument.

De volgende tabel bevat de namen van eigenschappen en de bijbehorende beschrijvingen voor het maken van een wachtrij van de uitvoer.

| Naam van eigenschap | Description |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om te leiden van de query-uitvoer naar deze Service Bus-wachtrij. |
| Service Bus-naamruimte |Een container voor een set berichtentiteiten. |
| Wachtrijnaam |De naam van de Service Bus-wachtrij. |
| Naam van het wachtrijbeleid |Wanneer u een wachtrij maakt, kunt u ook beleid voor gedeelde toegang maken op van de wachtrij **configureren** tabblad. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| Sleutel voor het wachtrijbeleid |De gedeelde toegangssleutel die wordt gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte. |
| Serialisatie-indeling voor gebeurtenissen |De serialisatie-indeling voor de uitvoergegevens. JSON-, CSV- en Avro worden ondersteund. |
| Encoding |Voor CSV en JSON is UTF-8 de enige ondersteunde coderingsindeling op dit moment. |
| Scheidingsteken |Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn met door komma's, door puntkomma's, spatie, tab en verticale balk. |
| Indeling |Alleen van toepassing op JSON-type. **Regel gescheiden** geeft aan dat de uitvoer is geformatteerd met elk JSON-object dat is gescheiden door een nieuwe regel. **Matrix** geeft aan dat de uitvoer wordt opgemaakt als een matrix met JSON-objecten. |
| Eigenschappenkolommen | Optioneel. Door komma's gescheiden kolommen die moeten worden gekoppeld als gebruikerseigenschappen van het uitgaande bericht in plaats van de nettolading van de. Meer informatie over deze functie is in de sectie [eigenschappen van aangepaste metagegevens voor uitvoer](#custom-metadata-properties-for-output). |

Het aantal partities is [op basis van de Service Bus-SKU en grootte](../service-bus-messaging/service-bus-partitioning.md). Partitiesleutel is een unieke integer-waarde voor elke partitie.

## <a name="service-bus-topics"></a>Service Bus-onderwerpen
Service Bus-wachtrijen bieden een-op-een communicatiemethode van afzender naar ontvanger. [Service Bus-onderwerpen](https://msdn.microsoft.com/library/azure/hh367516.aspx) voorzien in de vorm van een een-op-veel communicatie.

De volgende tabel bevat de namen van eigenschappen en de bijbehorende beschrijvingen voor het maken van een onderwerp van de uitvoer.

| Naam van eigenschap | Description |
| --- | --- |
| Uitvoeralias |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze Service Bus-onderwerp te regelen. |
| Service Bus-naamruimte |Een container voor een set berichtentiteiten. Wanneer u een nieuwe event hub hebt gemaakt, is ook een Service Bus-naamruimte gemaakt. |
| Onderwerpnaam |Onderwerpen zijn berichtentiteiten, vergelijkbaar met eventhubs en wachtrijen. Ze zijn ontworpen om gebeurtenisstromen te verzamelen van apparaten en services. Als u een onderwerp maakt, heeft deze ook een specifieke naam gegeven. De berichten die worden verzonden naar een onderwerp niet beschikbaar zijn, tenzij er een abonnement is gemaakt, dus zorg ervoor dat er is een of meer abonnementen onder het onderwerp. |
| Naam van het onderwerpbeleid |Wanneer u een onderwerp maakt, kunt u ook beleid voor gedeelde toegang maken op van het onderwerp **configureren** tabblad. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| Sleutel voor het onderwerpbeleid |De gedeelde toegangssleutel die wordt gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte. |
| Serialisatie-indeling voor gebeurtenissen |De serialisatie-indeling voor de uitvoergegevens. JSON-, CSV- en Avro worden ondersteund. |
| Encoding |Als u van CSV of JSON-indeling gebruikmaakt, moet een codering worden opgegeven. Alleen de coderingsindeling UTF-8 wordt momenteel ondersteund. |
| Scheidingsteken |Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn met door komma's, door puntkomma's, spatie, tab en verticale balk. |
| Eigenschappenkolommen | Optioneel. Door komma's gescheiden kolommen die moeten worden gekoppeld als gebruikerseigenschappen van het uitgaande bericht in plaats van de nettolading van de. Meer informatie over deze functie is in de sectie [eigenschappen van aangepaste metagegevens voor uitvoer](#custom-metadata-properties-for-output). |

Het aantal partities is [op basis van de Service Bus-SKU en grootte](../service-bus-messaging/service-bus-partitioning.md). De partitiesleutel is een unieke integer-waarde voor elke partitie.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) is een wereldwijd gedistribueerde databaseservice biedt onbeperkt flexibel schalen over de hele wereld, uitgebreide query en automatische indexering via schema-agnostische gegevensmodellen. Zie voor meer informatie over opties voor het verzamelen van Azure Cosmos DB voor Stream Analytics, de [Stream Analytics met Azure Cosmos DB als uitvoer](stream-analytics-documentdb-output.md) artikel.

Azure Cosmos DB-uitvoer van Stream Analytics is momenteel niet beschikbaar in de Azure China (21Vianet) en regio's van Azure Duitsland (T-Systems International).

> [!Note]
> Op dit moment ondersteunt Azure Stream Analytics alleen verbinding met Azure Cosmos DB met behulp van de SQL-API.
> Andere Azure Cosmos DB-API's zijn nog niet ondersteund. Als u punt Azure Stream Analytics met de Azure Cosmos DB-accounts die zijn gemaakt met andere API's, zijn de gegevens mogelijk niet juist opgeslagen.

De volgende tabel beschrijft de eigenschappen voor het maken van een Azure Cosmos DB-uitvoer.

| Naam van eigenschap | Description |
| --- | --- |
| Uitvoeralias | Een alias om te verwijzen deze uitvoer in uw Stream Analytics-query. |
| Sink | Azure Cosmos DB. |
| Importoptie | Kies een **Cosmos DB selecteren uit uw abonnement** of **bieden Cosmos DB-instellingen handmatig**.
| Account-id | De naam of URI van de Azure Cosmos DB-account van het eindpunt. |
| Accountcode | De gedeelde toegangssleutel voor het Azure Cosmos DB-account. |
| Database | De naam van de Azure Cosmos DB-database. |
| Patroon voor de collectienaam | Naam van de verzameling of het patroon voor de verzamelingen op die moet worden gebruikt. <br />U kunt de indeling van de verzameling maken met behulp van de optionele {partition}-token, waarbij partities beginnen bij 0. Twee voorbeelden:  <br /><ul><li> _MyCollection_: Een verzameling met de naam 'MyCollection' moet bestaan.</li>  <li> _MyCollection{partition}_: Op basis van de partitioneringskolom.</li></ul> De partitionering kolom verzamelingen gelden: "MyCollection0", "MyCollection1,", "MyCollection2", enzovoort. |
| Partitiesleutel | Optioneel. U moet dit alleen als u van een token {partition} in het patroon voor de verzameling gebruikmaakt.<br /> De partitiesleutel is de naam van het veld in uitvoergebeurtenissen dat wordt gebruikt om op te geven van de sleutel voor het partitioneren van uitvoer in collecties.<br /> Voor één verzameling uitvoer, kunt u elke willekeurige uitvoerkolom. Een voorbeeld is PartitionId. |
| Document-id |Optioneel. De naam van het veld in uitvoergebeurtenissen dat wordt gebruikt voor de primaire sleutel opgeven op welke insert- of updatebewerkingen zijn gebaseerd.

## <a name="azure-functions"></a>Azure Functions
Azure Functions is een serverloze rekenservice waarmee u code op aanvraag uitvoeren zonder expliciet inrichten of beheren van infrastructuur kunt gebruiken. Hiermee kunt u code te implementeren die wordt geactiveerd door gebeurtenissen in Azure of partner services. Deze mogelijkheid van Azure Functions om op triggers te reageren, is het een natuurlijke uitvoer voor Azure Stream Analytics. Deze uitvoeradapter kan gebruikers Stream Analytics verbinden met Azure Functions en een script of een stukje code uitvoeren in reactie op een aantal gebeurtenissen.

Azure Functions-uitvoer van Stream Analytics is momenteel niet beschikbaar in de Azure China (21Vianet) en regio's van Azure Duitsland (T-Systems International).

Azure Stream Analytics activeert Azure Functions via HTTP-triggers. De Azure Functions-uitvoeradapter is beschikbaar met de volgende eigenschappen kunnen worden geconfigureerd:

| Naam van eigenschap | Description |
| --- | --- |
| Function App |De naam van uw Azure Functions-app. |
| Function |De naam van de functie in uw Azure Functions-app. |
| Sleutel |Als u een Azure-functie van een ander abonnement gebruiken wilt, kunt u dit doen door op te geven van de sleutel voor toegang tot uw functie. |
| Maximale batchgrootte |Een eigenschap waarmee u instellen de maximale grootte voor elke uitvoerbatch die wordt verzonden naar uw Azure-functie. De invoer eenheid wordt uitgedrukt in bytes. Deze waarde is standaard 262.144 bytes (256 KB). |
| Maximum aantal batches  |Een eigenschap waarmee u het maximum aantal gebeurtenissen in elke batch die wordt verzonden naar Azure Functions opgeven. De standaardwaarde is 100. |

Wanneer Azure Stream Analytics een 413 ('http Request aanvraagentiteit te groot') ontvangt uitzondering uit een Azure-functie, vermindert de grootte van de batches die naar Azure Functions worden verzonden. In de code van uw Azure-functie, gebruikt u deze uitzondering om ervoor te zorgen dat Azure Stream Analytics geen te grote batches niet verzenden. Zorg er ook voor zorgen dat de maximale batch-aantal en grootte waarden zijn gebruikt in de functie consistent zijn met de waarden hebt ingevoerd in de Stream Analytics-portal.

Ook in een situatie waarbij er geen gebeurtenis terechtkomen in een bepaalde periode, wordt geen uitvoer gegenereerd. Als gevolg hiervan de **computeResult** functie is niet aangeroepen. Dit gedrag is consistent met de ingebouwde statistische functies in vensters.

## <a name="custom-metadata-properties-for-output"></a>Eigenschappen van aangepaste metagegevens voor uitvoer 

U kunt query kolommen als de eigenschappen van de gebruiker koppelen aan uw uitgaande berichten. Deze kolommen gaat niet u in de nettolading. De eigenschappen zijn aanwezig in de vorm van een woordenlijst in de uitvoerbericht. *Sleutel* is de naam van de kolom en *waarde* is de waarde in de kolom in de woordenlijst met eigenschappen. Alle Stream Analytics-gegevenstypen worden ondersteund, behalve de Record en matrix.  

Ondersteunde uitvoer: 
* Service Bus-wachtrij 
* Service Bus-onderwerp 
* Event Hub 

In het volgende voorbeeld wordt we de twee velden toevoegen `DeviceId` en `DeviceStatus` aan de metagegevens. 
* Query: `select *, DeviceId, DeviceStatus from iotHubInput`
* Configuratie van de uitvoer: `DeviceId,DeviceStatus`

![Eigenschappenkolommen](./media/stream-analytics-define-outputs/10-stream-analytics-property-columns.png)

De volgende schermafbeelding ziet u uitvoer berichteigenschappen gecontroleerd in EventHub via [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).

![Aangepaste eigenschappen van gebeurtenis](./media/stream-analytics-define-outputs/09-stream-analytics-custom-properties.png)

## <a name="partitioning"></a>Partitionering

De volgende tabel geeft een overzicht van de partitie ondersteuning en het aantal schrijvers van de uitvoer voor elk uitvoertype:

| Uitvoertype | Ondersteuning voor partitioneren | Partitiesleutel  | Aantal uitvoer schrijvers |
| --- | --- | --- | --- |
| Azure Data Lake Store | Ja | Gebruik {date} en {time} tokens in het patroon voor het voorvoegsel van pad. Kies de notatie voor datum, zoals jjjj/MM/DD, jjjj/MM/jjjj of MM-DD-JJJJ. HH wordt gebruikt voor de tijdnotatie. | De invoer voor het partitioneren van volgt [volledig worden opgestart query's](stream-analytics-scale-jobs.md). |
| Azure SQL Database | Ja | Op basis van de component PARTITION BY in de query. | De invoer voor het partitioneren van volgt [volledig worden opgestart query's](stream-analytics-scale-jobs.md). Zie voor meer informatie over het bereiken van betere prestaties van de netwerkdoorvoer schrijven als u het laden van gegevens in Azure SQL Database [Azure Stream Analytics-uitvoer naar Azure SQL Database](stream-analytics-sql-output-perf.md). |
| Azure Blob Storage | Ja | Gebruik {date} en {time} tokens van de gebeurtenis velden in het padpatroon. Kies de notatie voor datum, zoals jjjj/MM/DD, jjjj/MM/jjjj of MM-DD-JJJJ. HH wordt gebruikt voor de tijdnotatie. BLOB-uitvoer kan worden gepartitioneerd op een kenmerk met één aangepaste gebeurtenis {fieldname} of {datum/tijd:\<aanduiding >}. | De invoer voor het partitioneren van volgt [volledig worden opgestart query's](stream-analytics-scale-jobs.md). |
| Azure Event Hubs | Ja | Ja | Is afhankelijk van de uitlijning van de partitie.<br /> Als de partitiesleutel voor event hub-uitvoer is even uitgelijnd met de upstream querystap (vorige), wordt het aantal schrijvers is hetzelfde als het aantal partities in event hub-uitvoer. Maakt gebruik van elke schrijver de [EventHubSender klasse](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) voor het verzenden van gebeurtenissen naar de specifieke partitie. <br /> Als de partitiesleutel voor event hub-uitvoer is niet uitgelijnd met de upstream querystap (vorige), wordt het aantal schrijvers is hetzelfde als het aantal partities in die de vorige stap. Maakt gebruik van elke schrijver de [SendBatchAsync klasse](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) in **EventHubClient** gebeurtenissen verzenden naar alle partities van de uitvoer. |
| Power BI | Nee | Geen | Niet van toepassing. |
| Azure Table Storage | Ja | Elke uitvoerkolom.  | De invoer voor het partitioneren van volgt [volledig geparallelliseerd query's](stream-analytics-scale-jobs.md). |
| Azure Service Bus-onderwerp | Ja | Automatisch gekozen. Het aantal partities is gebaseerd op de [Service Bus-SKU's en grootte](../service-bus-messaging/service-bus-partitioning.md). De partitiesleutel is een unieke integer-waarde voor elke partitie.| Hetzelfde als het aantal partities in het onderwerp van de uitvoer.  |
| Azure Service Bus-wachtrij | Ja | Automatisch gekozen. Het aantal partities is gebaseerd op de [Service Bus-SKU's en grootte](../service-bus-messaging/service-bus-partitioning.md). De partitiesleutel is een unieke integer-waarde voor elke partitie.| Hetzelfde als het aantal partities in de uitvoerwachtrij. |
| Azure Cosmos DB | Ja | Het token {partition} in het patroon voor de Collectienaam gebruiken. De waarde {partition} is gebaseerd op de component PARTITION BY in de query. | De invoer voor het partitioneren van volgt [volledig geparallelliseerd query's](stream-analytics-scale-jobs.md). |
| Azure Functions | Nee | Geen | Niet van toepassing. |

Als de uitvoeradapter niet is gepartitioneerd, gebrek aan gegevens in één invoer partitie, zorgt ervoor dat een vertraging van maximaal het latere aankomst van tijd. In dergelijke gevallen wordt de uitvoer naar een enkele writer, wat leiden knelpunten in de pijplijn tot kan samengevoegd. Zie voor meer informatie over beleid voor latere aankomst [overwegingen bij de volgorde van de Azure Stream Analytics gebeurtenis](stream-analytics-out-of-order-and-late-events.md).

## <a name="output-batch-size"></a>Batchgrootte voor uitvoer
Azure Stream Analytics maakt gebruik van variabele grootte batches gebeurtenissen verwerken en naar uitvoer schrijven. Normaal gesproken de Stream Analytics-engine niet schrijven van één bericht op een tijdstip en batches gebruikt voor efficiëntie. Wanneer de snelheid van de inkomende en uitgaande gebeurtenissen hoog is, wordt in Stream Analytics maakt gebruik van grotere batches. Wanneer de snelheid van uitgangsgebeurtenissen laag is, gebruikt het kleinere batches latentie laag te houden.

De volgende tabel worden enkele van de overwegingen voor uitvoer batchverwerking uitgelegd:

| Uitvoertype | Maximale berichtlengte | Optimalisatie van de grootte van batch |
| :--- | :--- | :--- |
| Azure Data Lake Store | Zie [Data Lake-opslag beperkt](../azure-subscription-service-limits.md#data-lake-store-limits). | Gebruik maximaal 4 MB per schrijfbewerking. |
| Azure SQL Database | 10.000 maximum aantal rijen per één bulksgewijs invoegen.<br />minimaal 100 rijen per één bulksgewijs invoegen. <br />Zie [limieten van Azure SQL](../sql-database/sql-database-resource-limits.md). |  Elke batch is in eerste instantie bulksgewijs invoegen met maximale batchgrootte. U kunt de batch in de helft (totdat u de minimale batchgrootte) op basis van herstelbare fouten uit SQL splitsen. |
| Azure Blob Storage | Zie [Azure Storage beperkt](../azure-subscription-service-limits.md#storage-limits). | De grootte van het maximum aantal blob-blok is 4 MB.<br />Het maximum aantal blob bock aantal is 50.000. |
| Azure Event Hubs  | 256 KB per bericht. <br />Zie [Event Hubs beperkt](../event-hubs/event-hubs-quotas.md). |  Als het partitioneren van invoer/uitvoer is niet uitgelijnd, elke gebeurtenis afzonderlijk in is verpakt **EventData** en verzonden in een batch van maximaal de maximale berichtgrootte (1 MB voor de Premium-SKU). <br /><br />  Als het partitioneren van invoer/uitvoer wordt uitgelijnd, meerdere gebeurtenissen zijn verpakt in een enkele **EventData** exemplaar, tot de maximale berichtgrootte en verzonden.  |
| Power BI | Zie [Power BI Rest-API-limieten](https://msdn.microsoft.com/library/dn950053.aspx). |
| Azure Table Storage | Zie [Azure Storage beperkt](../azure-subscription-service-limits.md#storage-limits). | De standaardwaarde is 100 entiteiten per één transactie. U kunt deze configureren in een kleinere waarde indien nodig. |
| Azure Service Bus-wachtrij   | 256 KB per bericht.<br /> Zie [Service Bus beperkt](../service-bus-messaging/service-bus-quotas.md). | Gebruik één gebeurtenis per bericht. |
| Azure Service Bus-onderwerp | 256 KB per bericht.<br /> Zie [Service Bus beperkt](../service-bus-messaging/service-bus-quotas.md). | Gebruik één gebeurtenis per bericht. |
| Azure Cosmos DB   | Zie [limieten van Azure Cosmos DB](../azure-subscription-service-limits.md#azure-cosmos-db-limits). | Batch-grootte en frequentie worden dynamisch aangepast schrijven op basis van Azure Cosmos DB-antwoorden. <br /> Er zijn geen vooraf bepaalde beperkingen van Stream Analytics. |
| Azure Functions   | | De standaardgrootte van de batch is 262.144 bytes (256 KB). <br /> Het standaardaantal gebeurtenis per batch is 100. <br /> De batchgrootte is configureerbaar en kan worden vergroot of verkleind in de Stream Analytics [uitvoeropties](#azure-functions).

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> 
> [Snelstart: Een Stream Analytics-taak maken met behulp van Azure portal](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
