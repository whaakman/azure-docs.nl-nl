---
title: 'Stream Analytics-uitvoer: opties voor opslag, analyse | Microsoft Docs'
description: Meer informatie over Stream Analytics uitvoer Gegevensopties met inbegrip van Power BI voor analyseresultaten als doel.
keywords: gegevenstransformatie, analyseresultaten, opties voor opslag van gegevens
services: stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: ba6697ac-e90f-4be3-bafd-5cfcf4bd8f1f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/18/2017
ms.author: sngun
ms.openlocfilehash: ae8c4daf6b4beff3b1baf7ef5a5f3b84d706401a
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Stream Analytics-uitvoer: opties voor opslag, analyse
Houd rekening met hoe de geretourneerde gegevens wordt gebruikt bij het ontwerpen van een Stream Analytics-taak. Hoe kunt u de resultaten van de Stream Analytics-taak weergeven en waarin kunt u deze opslaan?

Azure Stream Analytics heeft om in te schakelen tal van toepassing patronen, verschillende opties voor het opslaan van de uitvoer en weergeven van resultaten van de analyse. Dit maakt het eenvoudig taakuitvoer weergeven en geeft u de extra flexibiliteit in het verbruik en de opslag van de taakuitvoer van de voor gegevensopslag en andere doeleinden. Geen uitvoer geconfigureerd in de taak moet bestaan voordat de taak is gestart en gebeurtenissen start stromende. Bijvoorbeeld, als u een Blob-opslag als uitvoer gebruikt, de taak niet een opslagaccount maken automatisch. Een opslagaccount maken voordat de Stream Analytics-taak wordt gestart.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics ondersteunt [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Deze opslag kunt u voor het opslaan van gegevens van elke grootte, type en opnamesnelheid snelheid voor operationele en experimentele analyses. Bovendien moet de Stream Analytics worden geautoriseerd voor toegang tot de Data Lake Store. Meer informatie over de autorisatie en hoe u zich registreert voor de Data Lake Store (indien nodig) worden besproken in de [Data Lake uitvoer artikel](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Een Azure Data Lake Store toestaan
Wanneer Data Lake Storage als uitvoer in de Azure portal is geselecteerd, wordt u gevraagd om een verbinding met een bestaande Data Lake Store te autoriseren.  

![Data Lake Store toestaan](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Vul vervolgens de eigenschappen voor de Data Lake Store-uitvoer zoals hieronder wordt weergegeven:

![Data Lake Store toestaan](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

De volgende tabel bevat de namen van eigenschappen en hun beschrijving op die nodig zijn voor het maken van een Data Lake Store-uitvoer.

<table>
<tbody>
<tr>
<td><B>DE NAAM VAN EIGENSCHAP</B></td>
<td><B>BESCHRIJVING</B></td>
</tr>
<tr>
<td>Uitvoeraliassen</td>
<td>Een beschrijvende naam die wordt gebruikt in query's om de queryuitvoer naar Data Lake Store.</td>
</tr>
<tr>
<td>Accountnaam</td>
<td>De naam van het Data Lake Storage-account waarin u de uitvoer wilt verzenden. Krijgt u een vervolgkeuzelijst met Data Lake Store-accounts waartoe de gebruiker is aangemeld bij de portal toegang tot heeft.</td>
</tr>
<tr>
<td>Pad voorvoegselpatroon</td>
<td>Naamgeving van bestanden, volgt de volgende conventies: <BR>{Path Prefix Pattern}/schemaHashcode_Guid_Number.extension <BR> <BR>Voorbeeld van de uitvoerbestanden:<BR>Myoutput/20170901/00/45434_gguid_1.csv <BR>Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR>Hier volgen bovendien de situaties waarbij een nieuw bestand is gemaakt:<BR>1. Wijzig in het uitvoerschema <BR>2. Externe of interne opnieuw opstarten van een taak<BR><BR>Bovendien, als het bestandspatroon pad geen een afsluitende bevat '/', de laatste patroon in het bestandspad wordt behandeld als een voorvoegsel van de bestandsnaam.<BR><BR>Voorbeeld:<BR>Voor het patroon van pad: Logboeken-Map1/uu, het gegenereerde bestand eruit als: folder1/logs/02_134343_gguid_1.csv</td>
</tr>
<tr>
<td>Datum notatie [<I>optionele</I>]</td>
<td>Als de datum-token in het pad van het voorvoegsel wordt gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld. Voorbeeld: Jjjj/MM/DD</td>
</tr>
<tr>
<td>Tijdnotatie [<I>optionele</I>]</td>
<td>Als het token tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de tijdnotatie waarin de bestanden zijn ingedeeld. De enige ondersteunde waarde is momenteel HH.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Serialisatie-indeling voor uitvoergegevens. JSON, CSV en Avro worden ondersteund.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Als CSV- of JSON-indeling wordt gebruikt, moet een codering worden opgegeven. Alleen de coderingsindeling UTF-8 wordt momenteel ondersteund.</td>
</tr>
<tr>
<td>Scheidingsteken</td>
<td>Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn komma, puntkomma's, spatie, tab en verticale balk.</td>
</tr>
<tr>
<td>Indeling</td>
<td>Alleen van toepassing op JSON-serialisatie. Lijn gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt ingedeeld als een matrix met JSON-objecten. Deze matrix is gesloten, alleen wanneer de taak wordt gestopt of Stream Analytics is verplaatst op naar de volgende periode. In het algemeen is het beter regels gebruiken gescheiden JSON, omdat het niet nodig is geen speciale verwerking terwijl is nog steeds naar het uitvoerbestand worden geschreven.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Vernieuwen van Data Lake Store-autorisatie
U moet uw Data Lake Store-account te verifiëren als het wachtwoord is gewijzigd sinds de taak is gemaakt of laatst geverifieerd.

![Data Lake Store toestaan](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) kan worden gebruikt als uitvoer voor gegevens die relationele gegevens of voor toepassingen die afhankelijk zijn van de inhoud wordt gehost in een relationele database. Stream Analytics-taken schrijven naar een bestaande tabel in een Azure SQL Database.  Het tabelschema moet exact overeenkomen met de velden en hun typen wordt de uitvoer van de taak. Een [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) kan ook worden opgegeven als uitvoer via de SQL-Database output-optie ook (dit is een preview-functie). De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van de uitvoer van een SQL-Database.

| De naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeraliassen |Een beschrijvende naam die wordt gebruikt in query's om de uitvoer van de query met deze database. |
| Database |De naam van de database waar u de uitvoer wilt verzenden |
| Servernaam |De naam van de SQL-Database-server |
| Gebruikersnaam |De gebruikersnaam die toegang heeft tot het schrijven naar de database |
| Wachtwoord |Het wachtwoord voor verbinding met de database |
| Tabel |De naam van de tabel waar de uitvoer wordt geschreven. Naam van de tabel is hoofdlettergevoelig en het schema van deze tabel moet exact overeenkomen met het aantal velden en hun typen die door de taakuitvoer wordt gegenereerd. |

> [!NOTE]
> De aanbieding voor Azure SQL Database wordt momenteel ondersteund voor de taakuitvoer van een in de Stream Analytics. Een virtuele Machine van Azure met SQL Server met een database die is gekoppeld, wordt echter niet ondersteund. Dit kan worden gewijzigd in toekomstige releases.
> 
> 

## <a name="blob-storage"></a>Blob Storage
BLOB storage vormt een rendabele en schaalbare oplossing voor het opslaan van grote hoeveelheden ongestructureerde gegevens in de cloud.  Voor een inleiding op Azure Blob-opslag en het gebruik ervan, Zie de documentatie bij [het gebruik van Blobs](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van een blob-uitvoer.

<table>
<tbody>
<tr>
<td>DE NAAM VAN EIGENSCHAP</td>
<td>BESCHRIJVING</td>
</tr>
<tr>
<td>Uitvoeraliassen</td>
<td>Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze blob-opslag.</td>
</tr>
<tr>
<td>Opslagaccount</td>
<td>De naam van het opslagaccount waar u de uitvoer wilt verzenden.</td>
</tr>
<tr>
<td>Opslagaccountsleutel</td>
<td>De geheime sleutel die is gekoppeld aan het opslagaccount.</td>
</tr>
<tr>
<td>Storage-Container</td>
<td>Containers bieden een logische groepering van blobs die zijn opgeslagen in de Microsoft Azure Blob-service. Wanneer u een blob naar de Blob-service uploaden, moet u een blob-container opgeven.</td>
</tr>
<tr>
<td>Pad voorvoegsel patroon [optioneel]</td>
<td>Het pad naar bestandspatroon gebruikt voor het schrijven van uw BLOB's binnen de opgegeven container. <BR> U kunt kiezen in het patroon pad naar een of meer exemplaren van de volgende 2 variabelen gebruiken om op te geven van de frequentie waarmee blobs worden geschreven: <BR> {date}, {time} <BR> Voorbeeld 1: cluster1/logs / {date} / {time} <BR> Voorbeeld 2: cluster1/logs / {date} <BR> <BR> Naamgeving van bestanden, volgt de volgende conventies: <BR> {Path Prefix Pattern}/schemaHashcode_Guid_Number.extension <BR> <BR> Voorbeeld van de uitvoerbestanden: <BR> Myoutput/20170901/00/45434_gguid_1.csv <BR> Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR> Hier volgen bovendien de situaties waarbij een nieuw bestand is gemaakt: <BR> 1. Huidige bestand overschrijdt de maximum toegestane aantal blokken (momenteel 50.000) <BR> 2. Wijzig in het uitvoerschema <BR> 3. Externe of interne opnieuw opstarten van een taak  </td>
</tr>
<tr>
<td>[Optioneel] datumnotatie</td>
<td>Als de datum-token in het pad van het voorvoegsel wordt gebruikt, kunt u de datumnotatie waarin de bestanden zijn ingedeeld. Voorbeeld: Jjjj/MM/DD</td>
</tr>
<tr>
<td>[Optioneel] tijdnotatie</td>
<td>Als het token tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de tijdnotatie waarin de bestanden zijn ingedeeld. De enige ondersteunde waarde is momenteel HH.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Serialisatie-indeling voor uitvoergegevens.  JSON, CSV en Avro worden ondersteund.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Als CSV- of JSON-indeling wordt gebruikt, moet een codering worden opgegeven. Alleen de coderingsindeling UTF-8 wordt momenteel ondersteund.</td>
</tr>
<tr>
<td>Scheidingsteken</td>
<td>Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn komma, puntkomma, schijfruimte, tabblad en de verticale balk.</td>
</tr>
<tr>
<td>Indeling</td>
<td>Alleen van toepassing op JSON-serialisatie. Lijn gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt ingedeeld als een matrix met JSON-objecten. Deze matrix is gesloten, alleen wanneer de taak wordt gestopt of Stream Analytics is verplaatst op naar de volgende periode. In het algemeen is het beter regels gebruiken gescheiden JSON, omdat het niet nodig is geen speciale verwerking terwijl is nog steeds naar het uitvoerbestand worden geschreven.</td>
</tr>
</tbody>
</table>

Wanneer u blobopslag gebruikt als uitvoer, wordt een nieuw bestand gemaakt in de blob in de volgende gevallen:

* Als het bestand het maximum aantal toegestane blokken (Let erop overschrijdt dat de maximaal toegestane aantal blokken kan worden bereikt zonder de maximum toegestane blobgrootte is bereikt. Als de uitvoerpakketten hoog is, ziet u meer bytes per blok en de bestandsgrootte is groter. Als de uitvoerpakketten laag is, wordt elk blok is minder gegevens en worden de bestandsgrootte kleiner is.)  
* Als u een schema is gewijzigd in de uitvoer en indeling van de uitvoer moet vaste schema (CSV en Avro).  
* Als een taak opnieuw wordt opgestart een extern of interne opnieuw opstarten van een taak.  
* Als de query is volledig gepartitioneerd, wordt voor elke partitie uitvoer nieuw bestand gemaakt.  
* Als een bestand of een container van het opslagaccount is verwijderd door de gebruiker.  
* Als de uitvoer van de tijd die is gepartitioneerd met behulp van het pad voorvoegselpatroon is, wordt een nieuwe blob wordt gebruikt wanneer de query wordt verplaatst naar het volgende uur.

## <a name="event-hub"></a>Event Hub
[Event Hubs](https://azure.microsoft.com/services/event-hubs/) is een uiterst schaalbare voor publiceren / abonneren event ingestor. Het kan miljoenen gebeurtenissen per seconde verzamelen. Een gebruik van een Event Hub als uitvoer is wanneer de uitvoer van een Stream Analytics-taak de invoer van een ander streaming-taak wordt.

Er zijn een aantal parameters die nodig zijn voor het configureren van de Event Hub-gegevensstromen als uitvoer.

| De naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeraliassen |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze Event Hub. |
| Service Bus-naamruimte |Een Service Bus-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe Event Hub hebt gemaakt, hebt u ook een Service Bus-naamruimte gemaakt |
| Event Hub |De naam van uw Event Hub-uitvoer |
| Naam Event Hub-beleid |Het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad Event Hub configureren. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels |
| Event Hub beleidssleutel |De toegang tot gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte |
| Partitie-sleutelkolom [optioneel] |Deze kolom bevat de partitiesleutel voor uitvoer van de Event Hub. |
| Gebeurtenis serialisatie-indeling |Serialisatie-indeling voor uitvoergegevens.  JSON, CSV en Avro worden ondersteund. |
| Encoding |Voor de CSV en JSON is UTF-8 de enige ondersteunde coderingsindeling op dit moment |
| Scheidingsteken |Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn komma, puntkomma, schijfruimte, tabblad en de verticale balk. |
| Indeling |Alleen van toepassing op JSON-serialisatie. Lijn gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt ingedeeld als een matrix met JSON-objecten. Deze matrix is gesloten, alleen wanneer de taak wordt gestopt of Stream Analytics is verplaatst op naar de volgende periode. In het algemeen is het beter regels gebruiken gescheiden JSON, omdat het niet nodig is geen speciale verwerking terwijl is nog steeds naar het uitvoerbestand worden geschreven. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) kan worden gebruikt als uitvoer voor een Stream Analytics-taak te voorzien in een uitgebreide visualisatie-ervaring van de resultaten van de analyse. Deze mogelijkheid kan worden gebruikt voor de operationele dashboards, rapporten te genereren en de metriek aangestuurd reporting.

### <a name="authorize-a-power-bi-account"></a>Een Power BI-account toestaan
1. Als Power BI als uitvoer in de Azure portal is geselecteerd, wordt u gevraagd voor het autoriseren van een bestaande Power BI-gebruiker of een nieuwe Power BI-account maken.  
   
   ![Power BI gebruiker autoriseren](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  
2. Maak een nieuw account als u niet nog een klik op nu autoriseren.  Een scherm zoals hieronder wordt weergegeven.  
   
   ![Azure Account Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  
3. Geef het werk of school-account voor het machtigen van de Power BI-uitvoer in deze stap. Als u weet al niet aangemeld voor Power BI, kiest u nu Sign up. Het werk of school-account die u voor Power BI gebruikt kan afwijken van het account in Azure-abonnement, dat u momenteel bent aangemeld.

### <a name="configure-the-power-bi-output-properties"></a>Configureer de eigenschappen van Power BI-uitvoer
Zodra u de geverifieerde Power BI-account hebt, kunt u de eigenschappen configureren voor uw Power BI-uitvoer. De onderstaande tabel wordt de lijst met namen van eigenschappen en hun beschrijving voor het configureren van uw Power BI-uitvoer.

| De naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeraliassen |Een beschrijvende naam die wordt gebruikt in query's om de queryuitvoer naar deze Power BI-uitvoer. |
| Groep werkruimte |U kunt groepen binnen uw Power BI-account selecteren of kies 'Mijn werkruimte' als u niet wilt schrijven naar een groep zodat delen van gegevens met andere Power BI-gebruikers.  Bijwerken van een bestaande groep is vereist voor het vernieuwen van de Power BI-verificatie. |
| De naam van de gegevensset |Geef een gegevenssetnaam dat het nodig is voor de uitvoer van de Power BI te gebruiken |
| Tabelnaam |Geef een tabelnaam onder de gegevensset van de Power BI-uitvoer. Op dit moment kan Power BI-uitvoer van de Stream Analytics-taken alleen hebben één tabel in een gegevensset |

Zie voor een demonstratie van de configuratie van een Power BI-uitvoer en het dashboard, het [Azure Stream Analytics & Power BI](stream-analytics-power-bi-dashboard.md) artikel.

> [!NOTE]
> Expliciet maken geen de gegevensset en de tabel in de Power BI-dashboard. De gegevensset en de tabel wordt automatisch gevuld wanneer de taak is gestart en de taak begint gemalen uitvoer naar Power BI. Houd er rekening mee dat als de taak query geen resultaten, de gegevensset niet genereren en de tabel is niet gemaakt. Houd er rekening mee dat als Power BI al een gegevensset en een tabel met dezelfde naam als een opgegeven in deze Stream Analytics-taak, de bestaande gegevens worden overschreven.
> 
> 

### <a name="schema-creation"></a>Schema maken
Azure Stream Analytics maakt een Power BI gegevensset en tabel namens de gebruiker als deze niet al bestaat. In andere gevallen is wordt de tabel bijgewerkt met nieuwe waarden. Er is een beperking die slechts één tabel binnen een dataset kan bestaan.

### <a name="data-type-conversion-from-stream-analytics-to-power-bi"></a>Conversie van Stream Analytics naar Power BI-gegevenstype
Azure Stream Analytics werkt het gegevensmodel dynamisch tijdens runtime als het uitvoerschema wordt gewijzigd. Alle worden wijzigingen in de kolom naam, wijzigingen van het type kolom, en het toevoegen of verwijderen van kolommen bijgehouden.

Deze tabel bevat informatie over de conversie van het gegevenstype van [Stream Analytics-gegevenstypen](https://msdn.microsoft.com/library/azure/dn835065.aspx) naar Power BIs [typen met Entity Data Model (EDP)](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) als een gegevensset met POWER BI en de tabel niet bestaat.


Vanuit Stream Analytics | Met Power BI
-----|-----|------------
bigint | Int64
nvarchar(max) | Tekenreeks
datum/tijd | Datum en tijd
Float | Double
Record matrix | String-type, constante waarde 'Irecords' of 'IArray'

### <a name="schema-update"></a>Schema-Update
Stream Analytics infereert het modelschema van gegevens op basis van de eerste reeks gebeurtenissen in de uitvoer. Later, indien nodig, wordt het modelschema gegevens bijgewerkt zodat binnenkomende gebeurtenissen die mogelijk niet in het oorspronkelijke schema passen.

De `SELECT *` query om te voorkomen dat dynamische schema-update in rijen moet worden vermeden. Naast de mogelijke gevolgen voor prestaties, kan dit ook leiden tot onzekerheid van de tijd die voor de resultaten. De exacte velden die moeten worden weergegeven in Power BI-dashboard moeten worden geselecteerd. Daarnaast moet de gegevenswaarden compatibel zijn met het gekozen gegevenstype.


Vorige/huidige | Int64 | Tekenreeks | Datum en tijd | Double
-----------------|-------|--------|----------|-------
Int64 | Int64 | Tekenreeks | Tekenreeks | Double
Double | Double | Tekenreeks | Tekenreeks | Double
Tekenreeks | Tekenreeks | Tekenreeks | Tekenreeks |  | Tekenreeks | 
Datum en tijd | Tekenreeks | Tekenreeks |  Datum en tijd | Tekenreeks


### <a name="renew-power-bi-authorization"></a>Vernieuwen van Power BI-autorisatie
U moet uw Power BI-account te verifiëren als het wachtwoord is gewijzigd sinds de taak is gemaakt of laatst geverifieerd. Als multi-factor Authentication (MFA) is geconfigureerd op de tenant van uw Azure Active Directory (AAD) moet u ook voor het vernieuwen van Power BI-autorisatie elke twee weken. Een symptoom zijn van dit probleem is geen taakuitvoer en een "verifiëren gebruiker error" in de logboeken voor bewerking:

  ![Power BI vernieuwen fout](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

U lost dit probleem, stop de actieve taak en gaat u naar uw Power BI-uitvoer.  Klik op de koppeling 'Vernieuwen autorisatie' en start u de taak van de laatste keer dat kan worden gestopt om gegevensverlies te voorkomen.

  ![Power BI wordt verlengd autorisatie](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Table Storage
[Azure Table storage](../storage/common/storage-introduction.md) biedt maximaal beschikbare, sterk schaalbare opslag, zodat een toepassing automatisch schalen kan om te voldoen aan de behoeften van de gebruiker. Tabelopslag is van Microsoft NoSQL sleutel-/ kenmerkopslag, waarvoor een van voor gestructureerde gegevens met minder beperkingen van het schema gebruikmaken kan. Azure Table storage kan worden gebruikt voor het opslaan van gegevens voor de persistentie en efficiënte ophalen.

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van de tabeluitvoer van een.

| De naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeraliassen |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar de tabelopslag van deze. |
| Opslagaccount |De naam van het opslagaccount waar u de uitvoer wilt verzenden. |
| Opslagaccountsleutel |De toegangssleutel die is gekoppeld aan het opslagaccount. |
| Tabelnaam |De naam van de tabel. De tabel wordt gemaakt als deze niet bestaat. |
| Partitiesleutel |De naam van de uitvoerkolom die de partitiesleutel bevat. De partitiesleutel is een unieke id voor de partitie in een bepaalde tabel die dient als het eerste deel van de primaire sleutel van een entiteit. Het is een tekenreekswaarde die maximaal 1 KB groot zijn. |
| Rijsleutel |De naam van de uitvoerkolom die de rijsleutel bevat. De rijsleutel is een unieke id voor een entiteit binnen een bepaalde partitie. Het tweede gedeelte van een entiteit primaire sleutel vormt. De rijsleutel is een tekenreekswaarde die maximaal 1 KB groot zijn. |
| Batchgrootte |Het aantal records voor een batchbewerking. De standaardwaarde is meestal voldoende voor de meeste taken verwijzen naar de [tabel batchbewerking spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) voor meer informatie over het aanpassen van deze instelling. |
 
## <a name="service-bus-queues"></a>Service Bus-wachtrijen
[Service Bus-wachtrijen](https://msdn.microsoft.com/library/azure/hh367516.aspx) bieden een First In, eerste Out (FIFO) berichtbezorging naar een of meer concurrerende consumenten. Normaal gesproken berichten moeten worden ontvangen en verwerkt door de ontvangers in de tijdsvolgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht wordt ontvangen en verwerkt door slechts één berichtconsument.

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van de uitvoer van een wachtrij.

| De naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeraliassen |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze Service Bus-wachtrij. |
| Service Bus-naamruimte |Een Service Bus-naamruimte is een container voor een set berichtentiteiten. |
| De wachtrijnaam van de |De naam van de Service Bus-wachtrij. |
| Wachtrij-beleidsnaam |Wanneer u een wachtrij maakt, kunt u ook gedeeld toegangsbeleid maken op het tabblad wachtrij configureren. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| Wachtrij beleidssleutel |De toegang tot gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte |
| Gebeurtenis serialisatie-indeling |Serialisatie-indeling voor uitvoergegevens.  JSON, CSV en Avro worden ondersteund. |
| Encoding |Voor de CSV en JSON is UTF-8 de enige ondersteunde coderingsindeling op dit moment |
| Scheidingsteken |Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn komma, puntkomma, schijfruimte, tabblad en de verticale balk. |
| Indeling |Alleen van toepassing op JSON-type. Lijn gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk JSON-object dat is gescheiden door een nieuwe regel. Matrix geeft aan dat de uitvoer wordt ingedeeld als een matrix met JSON-objecten. |

## <a name="service-bus-topics"></a>Service Bus-onderwerpen
Terwijl Service Bus-wachtrijen een communicatiemethode één van afzender naar ontvanger bieden [Service Bus-onderwerpen](https://msdn.microsoft.com/library/azure/hh367516.aspx) een een-op-veel-vorm van communicatie bieden.

De volgende tabel bevat de namen van eigenschappen en hun beschrijving voor het maken van de tabeluitvoer van een.

| De naam van eigenschap | Beschrijving |
| --- | --- |
| Uitvoeraliassen |Een beschrijvende naam die wordt gebruikt in query's om de query-uitvoer naar deze Service Bus-onderwerp. |
| Service Bus-naamruimte |Een Service Bus-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe Event Hub hebt gemaakt, hebt u ook een Service Bus-naamruimte gemaakt |
| Onderwerpnaam |Onderwerpen zijn berichtentiteiten, vergelijkbaar met event hubs en wachtrijen. Ze zijn ontworpen voor het verzamelen van gebeurtenisstromen uit een aantal verschillende apparaten en services. Wanneer u een onderwerp maakt, is het ook een specifieke naam gegeven. De berichten die worden verzonden naar een onderwerp is alleen beschikbaar als een abonnement is gemaakt, dus zorg ervoor er zijn een of meer abonnementen in het onderwerp |
| Onderwerp-beleidsnaam |Wanneer u een onderwerp maakt, kunt u ook gedeeld toegangsbeleid maken op het tabblad onderwerp configureren. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels |
| Onderwerp beleidssleutel |De toegang tot gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte |
| Gebeurtenis serialisatie-indeling |Serialisatie-indeling voor uitvoergegevens.  JSON, CSV en Avro worden ondersteund. |
 | Encoding |Als CSV- of JSON-indeling wordt gebruikt, moet een codering worden opgegeven. UTF-8 is de enige ondersteunde coderingsindeling op dit moment |
| Scheidingsteken |Alleen van toepassing voor CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens om gegevens te serialiseren in csv-indeling. Ondersteunde waarden zijn komma, puntkomma, schijfruimte, tabblad en de verticale balk. |

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) is van een database globaal gedistribueerd en modellen service dat aanbiedingen oneindig elastisch schalen rond de hele wereld, uitgebreide query en automatische indexeren via schema networkdirect gegevensmodellen, lage latentie gegarandeerd en toonaangevende uitgebreide Sla's.

De details hieronder een lijst met namen van de eigenschappen en hun beschrijving voor het maken van een Azure DB die Cosmos-uitvoer.

* **Uitvoer Alias** – een alias voor deze uitvoer in de Stream Analytics query verwijzen  
* **De accountnaam** : de naam of de URI van de Cosmos-DB-account voor het eindpunt.  
* **Account van de sleutel** – de gedeelde toegangssleutel voor het account van de Cosmos-DB.  
* **Database** – naam van de Cosmos-DB-database.  
* **Verzameling naampatroon** – naam van de verzameling of het patroon voor de verzamelingen moet worden gebruikt. Indeling van de Collectienaam kan worden samengesteld met behulp van de optionele {partition}-token, waarbij partities beginnen bij 0. Hieronder vindt u voorbeeld geldige invoer:  
  1\) MyCollection: een verzameling met de naam 'MyCollection' moet aanwezig zijn.  
  2\) MyCollection {partition} – deze verzamelingen moeten bestaan – 'MyCollection0', 'MyCollection1', 'MyCollection2', enzovoort.  
* **Partitie-sleutel** : optioneel. Dit is alleen nodig als u een partitie-token in het patroon van de naam van verzameling. De naam van het veld in uitvoergebeurtenissen dat wordt gebruikt om de sleutel op te geven voor het partitioneren van uitvoer in collecties. Voor één verzameling uitvoer, kan elke willekeurige uitvoerkolom bijvoorbeeld PartitionId worden gebruikt.  
* **ID-document** : optioneel. De naam van het veld in uitvoergebeurtenissen dat wordt gebruikt voor de primaire sleutel opgeven in welke invoegen of updatebewerkingen zijn gebaseerd.  

## <a name="azure-functions-in-preview"></a>Azure Functions (In Preview)
Azure Functions is een serverloze compute-service waarmee u code op aanvraag kunt uitvoeren zonder expliciet een infrastructuur in te richten of te beheren. Hiermee kunt u code die wordt geactiveerd door gebeurtenissen in Azure of services van derden implementeren.  Deze mogelijkheid van Azure Functions om te reageren op triggers maakt het een natuurlijke uitvoer voor een Azure Stream Analytics. Deze uitvoeradapter kan gebruikers verbinding van Stream Analytics met Azure Functions, en voer een script of een stuk code in reactie op tal van gebeurtenissen.

Azure Stream Analytics roept Azure Functions via HTTP-triggers. De nieuwe uitvoer van de functie Azure-adapter is beschikbaar met de volgende eigenschappen kunnen worden geconfigureerd:

| De naam van eigenschap | Beschrijving |
| --- | --- |
| Functie-app |Naam van uw App in Azure Functions |
| Functie |Naam van de functie in uw App in Azure-functies |
| Maximale batchgrootte |Deze eigenschap kan worden gebruikt om de maximale grootte voor elke uitvoer batch die wordt verzonden naar uw Azure-functie. Deze waarde is standaard 256 KB |
| Maximale Batch-aantal  |Als de naam aangeeft, wordt deze eigenschap kunt u het maximum aantal gebeurtenissen in elke batch die wordt verzonden naar Azure Functions opgeven. De standaardwaarde voor de maximale batch-aantal is 100 |
| Sleutel |Als u een Azure-functie van een ander abonnement gebruiken wilt, kunt u doen door op te geven van de sleutel voor toegang tot uw functie |

Houd er rekening mee dat wanneer Azure Stream Analytics 413 (http-aanvragen entiteit te groot) uitzondering van de functie Azure ontvangt, het vermindert de grootte van de batches naar Azure Functions verzendt. Gebruik in uw Azure functiecode deze uitzondering om ervoor te zorgen dat Azure Stream Analytics te grote batches niet verzenden. Zorg ervoor dat de maximale batch-aantal en grootte waarden gebruikt in de functie consistent met de waarden die zijn opgegeven in de Stream Analytics-portal zijn. 

Ook wordt in een situatie waarin er geen gebeurtenis in een tijdvenster aanvoer is, wordt geen uitvoer gegenereerd. Als gevolg hiervan computeResult functie niet wordt aangeroepen. Dit gedrag is consistent met de ingebouwde statistische functies in vensters.


## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
U hebt kennisgemaakt met Stream Analytics, een beheerde service voor streaminganalyse van gegevens afkomstig van het Internet of Things. Raadpleeg de volgende onderwerpen voor meer informatie over deze service:

* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
