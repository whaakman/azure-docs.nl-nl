---
title: Concepten voor ontwikkelaars van Data Catalog | Microsoft Docs
description: Inleiding tot de belangrijkste concepten in Azure Data Catalog conceptuele model als weergegeven met de REST-API-catalogus.
services: data-catalog
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
tags: 
ms.assetid: 89de9137-a0a4-40d1-9f8d-625acad31619
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: spelluru
ms.openlocfilehash: 48d4a33f7667786f2eb8851ed69dedc206e777ae
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-data-catalog-developer-concepts"></a>Azure Data Catalog-concepten voor ontwikkelaars
Microsoft **Azure Data Catalog** is een volledig beheerde cloudservice waarmee mogelijkheden voor detectie van gegevensbronnen en crowdsourcing metagegevens van de gegevensbron biedt. Ontwikkelaars kunnen de service via de REST-API's gebruiken. Informatie over de concepten die zijn geïmplementeerd in de service is belangrijk voor ontwikkelaars is geïntegreerd met **Azure Data Catalog**.

## <a name="key-concepts"></a>Belangrijkste concepten
De **Azure Data Catalog** conceptuele model is gebaseerd op vier belangrijke concepten: de **catalogus**, **gebruikers**, **activa**, en **aantekeningen**.

![Concept][1]

*Afbeelding 1: Azure Data Catalog vereenvoudigd conceptuele model*

### <a name="catalog"></a>Catalogus
Een **catalogus** is een container voor alle metagegevens van een organisatie worden opgeslagen op het hoogste niveau. Er is een **catalogus** toegestaan per Azure-Account. Catalogussen zijn gekoppeld aan een Azure-abonnement, maar slechts één **catalogus** kunnen worden gemaakt voor een bepaald Azure-account, zelfs als een account kan meerdere abonnementen hebt.

Een catalogus bevat **gebruikers** en **activa**.

### <a name="users"></a>Gebruikers
Gebruikers kunnen beveiligings-principals die gemachtigd zijn voor het uitvoeren van acties (zoeken in de catalogus, toevoegen, bewerken of verwijderen van items, enzovoort) in de catalogus.

Er zijn diverse verschillende rollen die een gebruiker kan hebben. Voor informatie over de functies, Zie de sectie rollen en autorisatie.

Afzonderlijke gebruikers en beveiligingsgroepen worden toegevoegd.

Azure Data Catalog gebruikt Azure Active Directory voor identiteits-en toegang. Elke gebruiker catalogus moet lid zijn van de Active Directory voor het account.

### <a name="assets"></a>Assets
Een **catalogus** gegevensassets bevat. **Activa** de eenheid van granulatie beheerd door de catalogus.

De granulatie van een asset hangt af van de gegevensbron. Voor SQL Server- of Oracle-Database mag een asset een tabel of weergave. Een asset kan voor SQL Server Analysis Services zijn een meting een dimensie of een Key Performance Indicator (KPI). Een actief is voor SQL Server Reporting Services, een rapport.

Een **Asset** het wat u toevoegen of verwijderen uit een catalogus is. Dit is de maateenheid resultaat u van terughalen **Search**.

Een **Asset** wordt samengesteld uit de naam, locatie, en type en aantekeningen verdere beschrijving van het.

### <a name="annotations"></a>Aantekeningen
Aantekeningen zijn items die metagegevens over activa vertegenwoordigen.

Voorbeelden van aantekeningen zijn beschrijving, tags, schema, documentatie, enzovoort. Een volledige lijst van de typen asset en aantekeningentypen zijn in de sectie Asset Object model.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Crowdsourcing aantekeningen en het gebruikersperspectief (multipliciteit van advies)
Een belangrijk aspect van Azure Data Catalog is ondersteuning voor de crowdsourcing van metagegevens in het systeem. In plaats van naar een aanpak wiki: waar er is slechts één advies en de laatste schrijver wins – het model van Azure Data Catalog kan meerdere advies aan elkaar bevinden zich in het systeem.

Deze aanpak geeft de praktijk van ondernemingsgegevens waarin verschillende gebruikers verschillende perspectieven op een opgegeven activum hebben:

* Een databasebeheerder kan voorzien in informatie over serviceovereenkomsten of het venster beschikbaar verwerking bulkbewerkingen voor ETL
* Een wereldburgers gegevens kan bevatten informatie over de bedrijfsprocessen waarop de activa van toepassing is, of de classificaties die het bedrijf is toegepast
* Een analist Financiën mogen bevatten informatie over hoe de gegevens wordt gebruikt tijdens het einde van de periode rapportagetaken

Ter ondersteuning van dit voorbeeld, kunt elke gebruiker, de DBA, de wereldburgers gegevens en de analist, een beschrijving toevoegen aan één tabel die is geregistreerd in de catalogus. Alle beschrijvingen worden bijgehouden in het systeem en in de portal voor Azure Data Catalog alle beschrijvingen worden weergegeven.

Dit patroon wordt toegepast op het merendeel van de items in het objectmodel, zodat de objecttypen die in de JSON-nettolading zijn vaak matrices voor eigenschappen waarbij u een singleton verwacht.

Onder de asset bijvoorbeeld is hoofdmap een matrix met objecten beschrijving. De naam van de matrixeigenschap is 'beschrijvingen'. Een object beschrijving heeft één eigenschap - beschrijving. Het patroon is dat elke gebruiker die de typen beschrijving haalt een beschrijving-object voor de waarde die is geleverd door de gebruiker gemaakt.

De UX kunt vervolgens het weergeven van de combinatie kiezen. Er zijn drie verschillende patronen voor weergave.

* De eenvoudigste patroon is 'Alles weergeven'. In dit patroon worden alle objecten weergegeven in een lijst weergegeven. De portal voor Azure Data Catalog UX wordt dit patroon gebruikt voor beschrijving.
* Een andere notatie is 'Samenvoegen'. In dit patroon worden de waarden van de verschillende gebruikers samengevoegd, met een dubbel verwijderd. Voorbeelden van dit patroon in de portal voor Azure Data Catalog UX zijn de eigenschappen tags en experts.
* Een derde patroon is 'laatste schrijver wins'. In dit patroon worden alleen de meest recente waarde hebt getypt in weergegeven. friendlyName is een voorbeeld van dit patroon.

## <a name="asset-object-model"></a>Asset-objectmodel
Zoals geïntroduceerd in de sectie hoofdconcepten van de **Azure Data Catalog** objectmodel bevat items, die activa of aantekeningen worden kunnen. Objecten hebben eigenschappen die optioneel of vereist worden kunnen. Sommige eigenschappen zijn van toepassing op alle items. Sommige eigenschappen zijn van toepassing op alle activa. Sommige eigenschappen gelden alleen voor specifieke asset typen.

### <a name="system-properties"></a>Systeemeigenschappen
<table><tr><td><b>De naam van eigenschap</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr><tr><td>tijdstempel</td><td>Datum en tijd</td><td>De laatste keer dat het item is gewijzigd. Dit veld wordt gegenereerd door de server als een item wordt toegevoegd en telkens wanneer een item wordt bijgewerkt. De waarde van deze eigenschap wordt genegeerd op invoer van bewerkingen publiceren.</td></tr><tr><td>id</td><td>URI</td><td>Absolute url van het item (alleen-lezen). De unieke adresseerbare URI voor het item is.  De waarde van deze eigenschap wordt genegeerd op invoer van bewerkingen publiceren.</td></tr><tr><td>type</td><td>Tekenreeks</td><td>Het type van de asset (alleen-lezen).</td></tr><tr><td>ETag</td><td>Tekenreeks</td><td>Een tekenreeks die overeenkomt met de versie van het item dat kan worden gebruikt voor Optimistisch gelijktijdigheidbeheer bij het uitvoeren van bewerkingen die items in de catalogus bijwerken. ' * ' kunnen worden gebruikt om een willekeurige waarde.</td></tr></table>

### <a name="common-properties"></a>Algemene eigenschappen
Deze eigenschappen zijn van toepassing op alle root asset typen en alle aantekeningentypen.

<table>
<tr><td><b>De naam van eigenschap</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr>
<tr><td>fromSourceSystem</td><td>Boole-waarde</td><td>Hiermee wordt aangegeven of de gegevens van artikel is afgeleid van een bronsysteem (zoals Sql Server-Database, Oracle-Database) of door een gebruiker geschreven.</td></tr>
</table>

### <a name="common-root-properties"></a>Algemene eigenschappen voor de hoofdmap
<p>
Deze eigenschappen zijn van toepassing op alle root asset typen.

<table><tr><td><b>De naam van eigenschap</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr><tr><td>naam</td><td>Tekenreeks</td><td>Een naam die is afgeleid van de locatie van de gegevensbroninformatie</td></tr><tr><td>dsl</td><td>DataSourceLocation</td><td>Een unieke beschrijving van de gegevensbron en is een van de id's voor de asset. (Zie de sectie met dubbele identiteit).  De structuur van de dsl hangt af van het type protocol en de bron.</td></tr><tr><td>Gegevensbron</td><td>DataSourceInfo</td><td>Meer informatie over het type van activa.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>Beschrijving van de gebruiker die dit activum meest recent geregistreerd.  Bevat de unieke id voor de gebruiker (upn) en de weergavenaam (lastName en firstName).</td></tr><tr><td>containerId</td><td>Tekenreeks</td><td>Id van de asset container voor de gegevensbron. Deze eigenschap wordt niet ondersteund voor het type van de Container.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Algemene eigenschappen voor niet-singleton aantekening
Deze eigenschappen van toepassing op alle niet-singleton aantekeningentypen (aantekeningen die kunnen worden meerdere per asset).

<table>
<tr><td><b>De naam van eigenschap</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr>
<tr><td>sleutel</td><td>Tekenreeks</td><td>Een gebruiker opgegeven sleutel, die een unieke identificatie van de aantekening in de huidige verzameling. Lengte van de sleutel mag maximaal 256 tekens.</td></tr>
</table>

### <a name="root-asset-types"></a>Hoofdmap asset typen
Hoofdmap asset typen zijn deze typen die staan voor de verschillende typen van gegevensassets die kunnen worden geregistreerd in de catalogus. Er is een weergave, die beschrijft asset en aantekeningen opgenomen in de weergave voor elke hoofdtype. De naam moet worden gebruikt in het bijbehorende {view_name} url-segment bij het publiceren van een activum met REST-API.

<table><tr><td><b>Activatype (weergavenaam)</b></td><td><b>Aanvullende eigenschappen</b></td><td><b>Gegevenstype</b></td><td><b>Toegestane aantekeningen</b></td><td><b>Opmerkingen</b></td></tr><tr><td>Tabel ('tabellen")</td><td></td><td></td><td>Beschrijving<p>FriendlyName<p>Label<p>Schema<p>ColumnDescription<p>ColumnTag<p> Expert<p>Preview<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>ColumnDataClassification<p>Documentatie<p></td><td>Een tabel vertegenwoordigt tabellaire gegevens.  Bijvoorbeeld: SQL-tabel, SQL-weergave, in tabelvorm tabel van Analysis Services, Analysis Services multidimensionale dimensie, Oracle-tabel, enzovoort.   </td></tr><tr><td>Meting ('metingen')</td><td></td><td></td><td>Beschrijving<p>FriendlyName<p>Label<p>Expert<p>AccessInstruction<p>Documentatie<p></td><td>Dit type vertegenwoordigt een Analysis Services-meting.</td></tr><tr><td></td><td>meting</td><td>Kolom</td><td></td><td>Metagegevens met een beschrijving van de meting</td></tr><tr><td></td><td>isCalculated </td><td>Boole-waarde</td><td></td><td>Geeft aan of de meting is berekend of niet.</td></tr><tr><td></td><td>MeasureGroup</td><td>Tekenreeks</td><td></td><td>Fysieke container voor de meting</td></tr><td>KPI ("kpis")</td><td></td><td></td><td>Beschrijving<p>FriendlyName<p>Label<p>Expert<p>AccessInstruction<p>Documentatie</td><td></td></tr><tr><td></td><td>MeasureGroup</td><td>Tekenreeks</td><td></td><td>Fysieke container voor de meting</td></tr><tr><td></td><td>goalExpression</td><td>Tekenreeks</td><td></td><td>Een numerieke MDX-expressie of een berekening die als resultaat de doelwaarde van de KPI geeft.</td></tr><tr><td></td><td>valueExpression</td><td>Tekenreeks</td><td></td><td>Een numerieke MDX-expressie die als resultaat de werkelijke waarde van de KPI geeft.</td></tr><tr><td></td><td>statusExpression</td><td>Tekenreeks</td><td></td><td>Een MDX-expressie die de status van de KPI op een bepaald punt in tijd vertegenwoordigt.</td></tr><tr><td></td><td>trendExpression</td><td>Tekenreeks</td><td></td><td>Een MDX-expressie die wordt geëvalueerd als de waarde van de KPI gedurende een bepaalde periode. Het trendanalyse mag elk criterium op basis van tijd, dat is handig in een specifieke zakelijke context.</td>
<tr><td>Rapport ('rapporten')</td><td></td><td></td><td>Beschrijving<p>FriendlyName<p>Label<p>Expert<p>AccessInstruction<p>Documentatie<p></td><td>Dit type vertegenwoordigt een SQL Server Reporting Services-rapport </td></tr><tr><td></td><td>assetCreatedDate</td><td>Tekenreeks</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>Tekenreeks</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>Tekenreeks</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>Tekenreeks</td><td></td><td></td></tr><tr><td>Container ('containers')</td><td></td><td></td><td>Beschrijving<p>FriendlyName<p>Label<p>Expert<p>AccessInstruction<p>Documentatie<p></td><td>Dit type vertegenwoordigt een container van andere activa zoals een SQL-database, een container Azure Blobs of een Analysis Services-model.</td></tr></table>

### <a name="annotation-types"></a>Annotatie-typen
Aantekeningentypen vertegenwoordigen typen metagegevens die kunnen worden toegewezen aan andere typen in de catalogus.

<table>
<tr><td><b>Type aantekening (Nested weergavenaam)</b></td><td><b>Aanvullende eigenschappen</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr>

<tr><td>Beschrijving ('beschrijvingen')</td><td></td><td></td><td>Deze eigenschap bevat een beschrijving op voor een asset. Elke gebruiker van het systeem kan hun eigen beschrijving toevoegen.  Alleen die gebruiker kan het object beschrijving bewerken.  (Beheerders en Asset eigenaars kunnen de beschrijving object verwijderen, maar deze niet bewerken). Het systeem onderhoudt beschrijvingen van de gebruikers afzonderlijk.  Het is dus een matrix met beschrijvingen van elke asset (één voor elke gebruiker die is bijgedragen hun kennis over het activum, naast het mogelijk een bestand met gegevens die zijn afgeleid van de gegevensbron).</td></tr>
<tr><td></td><td>description</td><td>tekenreeks</td><td>Een korte beschrijving (2-3 regels) van de activa</td></tr>

<tr><td>Tag ('tags')</td><td></td><td></td><td>Deze eigenschap definieert een label voor een asset. Elke gebruiker van het systeem kan meerdere labels voor een asset toevoegen.  Alleen de gebruiker die Tag-objecten gemaakt, kan ze bewerken.  Beheerders en Asset eigenaars kunnen verwijderen van de Tag-object maar deze niet bewerken. Het systeem onderhoudt labels gebruikers afzonderlijk.  Het is dus een matrix met objecten van de Tag op elke asset.</td></tr>
<tr><td></td><td>tag</td><td>tekenreeks</td><td>Een label met een beschrijving van de asset.</td></tr>

<tr><td>FriendlyName ('friendlyName')</td><td></td><td></td><td>Deze eigenschap bevat een beschrijvende naam voor een asset. FriendlyName is een singleton-annotatie - slechts één FriendlyName kan worden toegevoegd aan een asset.  Alleen de gebruiker die FriendlyName-object is gemaakt kunt bewerken. (Beheerders en Asset eigenaren kunnen de FriendlyName object verwijderen, maar deze niet bewerken). Het systeem onderhoudt beschrijvende namen van de gebruikers afzonderlijk.</td></tr>
<tr><td></td><td>friendlyName</td><td>tekenreeks</td><td>Een beschrijvende naam van de activa.</td></tr>

<tr><td>Schema ('schema')</td><td></td><td></td><td>De structuur van de gegevens in het Schema wordt beschreven.  Het hier worden de kenmerknamen (kolom, kenmerk, veld, enzovoort), alsook andere metagegevens van het type.  Deze informatie wordt afgeleid uit de gegevensbron.  Het schema is een singleton-annotatie - slechts één Schema kan worden toegevoegd voor een asset.</td></tr>
<tr><td></td><td>Kolommen</td><td>Kolom]</td><td>Een matrix met objecten van de kolom. Ze beschrijven de kolom met gegevens die zijn afgeleid van de gegevensbron.</td></tr>

<tr><td>ColumnDescription ('columnDescriptions')</td><td></td><td></td><td>Deze eigenschap bevat een beschrijving op voor een kolom.  Elke gebruiker van het systeem kan hun eigen beschrijvingen voor meerdere kolommen (maximaal één per kolom) toevoegen. Alleen de gebruiker die ColumnDescription objecten heeft gemaakt, kan ze bewerken.  (Beheerders en Asset eigenaren kunnen de ColumnDescription object verwijderen, maar deze niet bewerken). Het systeem onderhoudt afzonderlijk kolombeschrijvingen van deze gebruiker.  Het is dus een matrix van ColumnDescription objecten op elke asset (één per kolom voor elke gebruiker die is bijgedragen hun kennis over de kolom naast mogelijk een bestand met gegevens die zijn afgeleid van de gegevensbron).  De ColumnDescription los afhankelijk van het Schema zodat niet gesynchroniseerd krijgt. De ColumnDescription beschrijft een kolom die niet langer in het schema bestaat.  Het is tot de writer beschrijving en het schema om synchroon te houden.  De gegevensbron mogelijk ook gegevens voor beschrijving van kolommen en zijn extra ColumnDescription-objecten die moeten worden gemaakt wanneer u het hulpprogramma uitvoert.</td></tr>
<tr><td></td><td>columnName</td><td>Tekenreeks</td><td>De naam van de kolom die deze beschrijving naar verwijst.</td></tr>
<tr><td></td><td>description</td><td>Tekenreeks</td><td>een korte beschrijving (2-3 regels) van de kolom.</td></tr>

<tr><td>ColumnTag ('columnTags')</td><td></td><td></td><td>Deze eigenschap bevat een code voor een kolom. Elke gebruiker van het systeem kunt meerdere labels voor een bepaalde kolom toevoegen en labels voor meerdere kolommen kunt toevoegen. Alleen de gebruiker die ColumnTag objecten heeft gemaakt, kan ze bewerken. (Beheerders en Asset eigenaren kunnen de ColumnTag object verwijderen, maar deze niet bewerken). Het systeem onderhoudt kolom labels van deze gebruikers afzonderlijk.  Het is dus een matrix van ColumnTag objecten op elke asset.  De ColumnTag los afhankelijk van het schema zodat niet gesynchroniseerd krijgt. De ColumnTag beschrijft een kolom die niet langer in het schema bestaat.  Het is tot de writer kolom label en het schema om synchroon te houden.</td></tr>
<tr><td></td><td>columnName</td><td>Tekenreeks</td><td>De naam van de kolom die deze code naar verwijst.</td></tr>
<tr><td></td><td>tag</td><td>Tekenreeks</td><td>Een label met een beschrijving van de kolom.</td></tr>

<tr><td>Expert ('experts')</td><td></td><td></td><td>Deze eigenschap bevat een gebruiker die wordt beschouwd als een expert in de gegevensset. De deskundigen opinions(descriptions) belgrootte boven aan de UX wanneer beschrijvingen. Elke gebruiker kan hun eigen deskundigen opgeven. Alleen die gebruiker kunt het experts-object bewerken. (Beheerders en Asset eigenaren kunnen de Expert objecten verwijderen maar niet te bewerken).</td></tr>
<tr><td></td><td>expert</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Preview ('previews')</td><td></td><td></td><td>De Preview-versie bevat een momentopname van de top 20 rijen van de gegevens voor de asset. Preview alleen zinvol zijn voor bepaalde typen van assets (het zinvol voor de tabel, maar niet voor de meting).</td></tr>
<tr><td></td><td>preview</td><td>object[]</td><td>Matrix van objecten die een kolom vertegenwoordigen.  Elk object heeft geen eigenschapstoewijzing aan een kolom met een waarde voor de kolom voor de rij.</td></tr>

<tr><td>AccessInstruction ('accessInstructions')</td><td></td><td></td><td></td></tr>
<tr><td></td><td>mimeType</td><td>tekenreeks</td><td>Het mime-type van de inhoud.</td></tr>
<tr><td></td><td>Inhoud</td><td>tekenreeks</td><td>De instructies voor het toegang krijgen tot deze gegevens activa. De inhoud is mogelijk een URL, een e-mailadres of een set instructies.</td></tr>

<tr><td>TableDataProfile ('tableDataProfiles')</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>int</td><td>Het aantal rijen in de gegevensset</td></tr>
<tr><td></td><td>grootte</td><td>lang</td><td>De grootte in bytes van de gegevensset.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>tekenreeks</td><td>De laatste keer dat het schema is gewijzigd.</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>tekenreeks</td><td>De laatste keer dat de gegevensset is gewijzigd (gegevens werd toegevoegd, gewijzigd, of verwijderen)</td></tr>

<tr><td>ColumnsDataProfile ('columnsDataProfiles')</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Kolommen</td></td><td>ColumnDataProfile[]</td><td>Een matrix van profielen voor kolom-gegevens.</td></tr>

<tr><td>ColumnDataClassification ('columnDataClassifications')</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName</td><td>Tekenreeks</td><td>De naam van de kolom die deze categorie heeft betrekking op.</td></tr>
<tr><td></td><td>Classificatie</td><td>Tekenreeks</td><td>De classificatie van de gegevens in deze kolom.</td></tr>

<tr><td>Documentatie ("documentatie")</td><td></td><td></td><td>Een opgegeven activum kan slechts één documentatie gekoppeld hebben.</td></tr>
<tr><td></td><td>mimeType</td><td>tekenreeks</td><td>Het mime-type van de inhoud.</td></tr>
<tr><td></td><td>Inhoud</td><td>tekenreeks</td><td>De inhoud van de documentatie.</td></tr>

</table>

### <a name="common-types"></a>Algemene typen
Algemene typen kunnen worden gebruikt als de typen voor eigenschappen, maar er zijn geen Items.

<table>
<tr><td><b>Algemeen Type</b></td><td><b>Eigenschappen</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr>
<tr><td>DataSourceInfo</td><td></td><td></td><td></td></tr>
<tr><td></td><td>SourceType</td><td>tekenreeks</td><td>Beschrijving van het type van de gegevensbron.  Bijvoorbeeld: SQL Server, Oracle-Database, enzovoort.  </td></tr>
<tr><td></td><td>objectType</td><td>tekenreeks</td><td>Beschrijving van het type object in de gegevensbron. Bijvoorbeeld: tabel, weergave voor SQL Server.</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>protocol</td><td>tekenreeks</td><td>Vereist. Beschrijft een protocol dat wordt gebruikt om te communiceren met de gegevensbron. Bijvoorbeeld: "tds' voor SQl Server, 'oracle' voor Oracle, enzovoort. Raadpleeg [gegevensbron verwijzing opgegeven - DSL structuur](data-catalog-dsr.md) voor de lijst met ondersteunde protocollen.</td></tr>
<tr><td></td><td>Adres</td><td>Woordenlijst<string, object></td><td>Vereist. Adres is een set gegevens die specifiek zijn voor het protocol dat wordt gebruikt voor het identificeren van de gegevensbron waarnaar wordt verwezen. De gegevens voor het adres binnen het bereik van een bepaald protocol, wat betekent dat deze is nutteloos zonder te weten het protocol.</td></tr>
<tr><td></td><td>verificatie</td><td>tekenreeks</td><td>Optioneel. Het verificatieschema dat wordt gebruikt voor communicatie met de gegevensbron. Bijvoorbeeld: windows, oauth, enzovoort.</td></tr>
<tr><td></td><td>connectionProperties</td><td>Woordenlijst<string, object></td><td>Optioneel. Extra informatie over hoe u verbinding maakt met een gegevensbron.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>De back-end voert een validatie van de opgegeven eigenschappen met AAD tijdens de publicatie.</td></tr>
<tr><td></td><td>UPN</td><td>tekenreeks</td><td>Uniek e-mailadres van de gebruiker. Moet worden opgegeven als object-id is niet opgegeven of in de context van de eigenschap 'lastRegisteredBy', anders is optioneel.</td></tr>
<tr><td></td><td>objectId</td><td>GUID</td><td>Gebruiker of beveiligingsgroep groep AAD identiteit. Optioneel. Moet worden opgegeven als upn niet, anders is optioneel opgegeven is.</td></tr>
<tr><td></td><td>firstName</td><td>tekenreeks</td><td>De voornaam van de gebruiker (ter informatie weergegeven). Optioneel. Alleen geldig in de context van de eigenschap 'lastRegisteredBy'. Kan niet worden opgegeven bij het opgeven van beveiligings-principal voor 'functies', 'machtigingen' en 'deskundigen'.</td></tr>
<tr><td></td><td>lastName</td><td>tekenreeks</td><td>De achternaam van de gebruiker (ter informatie weergegeven). Optioneel. Alleen geldig in de context van de eigenschap 'lastRegisteredBy'. Kan niet worden opgegeven bij het opgeven van beveiligings-principal voor 'functies', 'machtigingen' en 'deskundigen'.</td></tr>

<tr><td>Kolom</td><td></td><td></td><td></td></tr>
<tr><td></td><td>naam</td><td>tekenreeks</td><td>Naam van de kolom of een kenmerk.</td></tr>
<tr><td></td><td>type</td><td>tekenreeks</td><td>het gegevenstype van de kolom of een kenmerk. De toegestane typen zijn afhankelijk van sourceType gegevens van de activa.  Alleen een subset van de typen wordt ondersteund.</td></tr>
<tr><td></td><td>maxLength</td><td>int</td><td>De maximaal toegestane lengte voor de kolom of een kenmerk. Afgeleid van de gegevensbron. Alleen van toepassing op bepaalde brontypen.</td></tr>
<tr><td></td><td>precisie</td><td>byte</td><td>De precisie voor de kolom of een kenmerk. Afgeleid van de gegevensbron. Alleen van toepassing op bepaalde brontypen.</td></tr>
<tr><td></td><td>isNullable</td><td>Boole-waarde</td><td>Of de kolom aan de waarde null hebben of niet is toegestaan. Afgeleid van de gegevensbron. Alleen van toepassing op bepaalde brontypen.</td></tr>
<tr><td></td><td>expressie</td><td>tekenreeks</td><td>Als de waarde een berekende kolom is, bevat dit veld in de expressie waarmee de waarde wordt uitgedrukt. Afgeleid van de gegevensbron. Alleen van toepassing op bepaalde brontypen.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName </td><td>tekenreeks</td><td>De naam van de kolom</td></tr>
<tr><td></td><td>type </td><td>tekenreeks</td><td>Het type van de kolom</td></tr>
<tr><td></td><td>min. </td><td>tekenreeks</td><td>De minimale waarde in de gegevensset</td></tr>
<tr><td></td><td>max </td><td>tekenreeks</td><td>De maximale waarde in de gegevensset</td></tr>
<tr><td></td><td>gem. </td><td>dubbel</td><td>De gemiddelde waarde in de gegevensset</td></tr>
<tr><td></td><td>stdev </td><td>dubbel</td><td>De standaardafwijking voor de gegevensset</td></tr>
<tr><td></td><td>nullCount </td><td>int</td><td>Het aantal null-waarden in de gegevensset</td></tr>
<tr><td></td><td>distinctCount  </td><td>int</td><td>Het aantal afzonderlijke waarden in de gegevensset</td></tr>


</table>

## <a name="asset-identity"></a>Asset-identiteit
Azure Data Catalog maakt gebruik van eigenschappen "protocol" en de identiteit van de eigenschappenverzameling 'adres' van de DataSourceLocation 'dsl'-eigenschap voor het genereren van de identiteit van de asset die wordt gebruikt voor het oplossen van de activa in de catalogus.
Bijvoorbeeld, heeft het protocol 'tds' identiteit eigenschappen 'server', 'database', 'schema' en 'object'. De combinaties van het protocol en de identiteitseigenschappen worden gebruikt voor het genereren van de identiteit van de SQL Server-tabel Asset.
Azure Data Catalog bevat verschillende ingebouwde gegevensbron-protocollen die worden vermeld op [gegevensbron verwijzing opgegeven - DSL structuur](data-catalog-dsr.md).
Kan de set met ondersteunde protocollen programmatisch worden uitgebreid (Zie Naslaginformatie over REST API van Data Catalog). Beheerders van de catalogus kunnen aangepaste gegevensbron protocollen registreren. De volgende tabel beschrijft de eigenschappen die nodig is om een aangepaste protocol registreren.

### <a name="custom-data-source-protocol-specification"></a>Aangepaste gegevensbron protocol specification
<table>
<tr><td><b>Type</b></td><td><b>Eigenschappen</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>naamruimte</td><td>tekenreeks</td><td>De naamruimte van het protocol. Namespace moet tussen 1 en 255 tekens lang zijn, een of meer niet-lege onderdelen gescheiden door een punt (.) bevatten. Elk deel moet liggen tussen 1 en 255 tekens lang, met een letter beginnen en alleen letters en cijfers bevatten.</td></tr>
<tr><td></td><td>naam</td><td>tekenreeks</td><td>De naam van het protocol. Naam moet liggen tussen 1 en 255 tekens lang, met een letter beginnen en mag alleen letters, cijfers en het streepje (-).</td></tr>
<tr><td></td><td>identityProperties</td><td>DataSourceProtocolIdentityProperty[]</td><td>Lijst van identiteitseigenschappen, moet ten minste één echter meer dan 20 eigenschappen bevatten. Bijvoorbeeld: 'server', 'database', 'schema', 'object' identiteitseigenschappen van het protocol 'tds' zijn.</td></tr>
<tr><td></td><td>identitySets</td><td>DataSourceProtocolIdentitySet[]</td><td>Hiermee stelt u de lijst van identiteit. Hiermee definieert u sets identiteitseigenschappen die geldige asset-id voorstelt. Moet ten minste één echter meer dan 20 sets bevatten. Bijvoorbeeld: {'server', 'database', 'schema' en 'object'} is ingesteld voor "tds" protocol, dat Hiermee definieert u de id van Sql Server-tabel actief identiteit.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>naam</td><td>tekenreeks</td><td>De naam van de eigenschap. Naam moet tussen 1 en 100 tekens lang zijn, start met een letter en mag alleen letters en cijfers bevatten.</td></tr>
<tr><td></td><td>type</td><td>tekenreeks</td><td>Het type van de eigenschap. Ondersteunde waarden: 'bool,' boolean ', 'byte', 'guid', 'int', 'integer', 'lang', 'string', "url"</td></tr>
<tr><td></td><td>ignoreCase</td><td>BOOL</td><td>Hiermee wordt aangegeven of geval moet worden genegeerd wanneer u de waarde van eigenschap. Kan alleen worden opgegeven voor eigenschappen met het type 'string'. Standaard is ingesteld op false.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>BOOL]</td><td>Hiermee wordt aangegeven of de aanvraag voor elk segment van de url-pad moet worden genegeerd. Kan alleen worden opgegeven voor eigenschappen met het type "url". Standaardwaarde is [false].</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>naam</td><td>tekenreeks</td><td>De naam van de identiteit is ingesteld.</td></tr>
<tr><td></td><td>properties</td><td>String]</td><td>De lijst met opgenomen in deze identiteit identiteitseigenschappen instellen. Het mag geen duplicaten bevatten. Elke eigenschap identiteitset waarnaar moet worden gedefinieerd in de lijst met 'identityProperties' van het protocol.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Rollen en -autorisatie
Microsoft Azure Data Catalog biedt autorisatiemogelijkheden voor voor CRUD-bewerkingen op activa en aantekeningen.

## <a name="key-concepts"></a>Belangrijkste concepten
De Azure Data Catalog gebruikt twee autorisatiemechanismen:

* Verificatie op basis van rollen
* Machtiging gebaseerde verificatie

### <a name="roles"></a>Rollen
Er zijn drie rollen: **beheerder**, **eigenaar**, en **Inzender**.  Elke functie heeft een bereik en de rechten die worden samengevat in de volgende tabel.

<table><tr><td><b>Rol</b></td><td><b>Bereik</b></td><td><b>Rechten</b></td></tr><tr><td>Beheerder</td><td>Catalogus (alle activa/aantekeningen in de catalogus)</td><td>Lees verwijderen ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Eigenaar</td><td>Elke asset (hoofditem)</td><td>Lees verwijderen ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Inzender</td><td>Elke individuele asset en aantekening</td><td>Lees Update verwijderen ViewRoles Opmerking: alle rechten worden ingetrokken als het lezen van het item rechts van de Inzender is ingetrokken</td></tr></table>

> [!NOTE]
> **Lees**, **Update**, **verwijderen**, **ViewRoles** rechten zijn van toepassing op een item (asset of aantekening) tijdens het **TakeOwnership**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** zijn alleen van toepassing op de hoofdmap asset.
> 
> **Verwijder** recht geldt voor een item en subitems of één item eronder. Bijvoorbeeld, verwijdert een asset ook alle aantekeningen voor de activa.
> 
> 

### <a name="permissions"></a>Machtigingen
De machtiging is als lijst met vermeldingen voor toegangsbeheer. Elk access control entry toewijst set rechten voor een beveiligings-principal. Machtigingen kunnen alleen worden opgegeven voor een asset (dat wil zeggen hoofditem) en van toepassing op de asset en alle subitems.

Tijdens de **Azure Data Catalog** bekijken, alleen **lezen** rechts wordt ondersteund in de lijst met machtigingen om in te schakelen scenario om door te beperken van de zichtbaarheid van een asset.

Alle geverifieerde gebruikers heeft standaard **lezen** tenzij zichtbaarheid beperkt tot de set-principals in de machtigingen is voor elk item in de catalogus naar rechts.

## <a name="rest-api"></a>REST-API
**PLAATSEN** en **POST** item weergeven aanvragen kunnen worden gebruikt voor het beheren van rollen en machtigingen: naast de nettolading van het item, twee eigenschappen kunnen worden opgegeven **rollen** en **machtigingen**.

> [!NOTE]
> **machtigingen** alleen van toepassing op een hoofditem.
> 
> **Eigenaar** functie is alleen van toepassing op een hoofditem.
> 
> Standaard wanneer een item in de catalogus wordt gemaakt de **Inzender** is ingesteld op de huidige geverifieerde gebruiker. Als het item moet worden bijgewerkt door iedereen, **Inzender** moet worden ingesteld op &lt;iedereen&gt; speciale beveiligings-principal in de **rollen** eigenschap wanneer het item eerst gepubliceerd (Zie het volgende voorbeeld). **Inzender** kan niet worden gewijzigd en blijft hetzelfde tijdens de levensduur van een item (zelfs **beheerder** of **eigenaar** geen machtiging om te wijzigen de **Inzender**). De enige waarde die wordt ondersteund voor de expliciete instelling van de **Inzender** is &lt;iedereen&gt;: **Inzender** mag alleen een gebruiker die een item heeft gemaakt of &lt;iedereen&gt;.
> 
> 

### <a name="examples"></a>Voorbeelden
**Inzender ingesteld op &lt;iedereen&gt; bij het publiceren van een item.**
Speciale beveiligings-principal &lt;iedereen&gt; objectId '00000000-0000-0000-0000-000000000201' is.
  **POST** https://api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

> [!NOTE]
> Sommige implementaties van HTTP-client kunnen aanvragen in reactie op een 302 van de server automatisch opnieuw worden uitgegeven, maar doorgaans van strook / autorisatie-header van de aanvraag. Aangezien de autorisatie-header is vereist in Azure Data Catalog aanbrengen aanvragen, moet u ervoor zorgen dat de autorisatie-header wordt nog steeds worden opgegeven als opnieuw uitgeven van een aanvraag naar een omleidingslocatie is opgegeven door Azure Data Catalog. De volgende voorbeeldcode laat zien met behulp van het .NET HttpWebRequest-object.
> 
> 

**Hoofdtekst**

    {
        "roles": [
            {
                "role": "Contributor",
                "members": [
                    {
                        "objectId": "00000000-0000-0000-0000-000000000201"
                    }
                ]
            }
        ]
    }

  **Eigenaren toewijzen en zichtbaarheid voor een bestaande hoofditem beperken**: **plaatsen** https://api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30

    {
        "roles": [
            {
                "role": "Owner",
                "members": [
                    {
                        "objectId": "c4159539-846a-45af-bdfb-58efd3772b43",
                        "upn": "user1@contoso.com"
                    },
                    {
                        "objectId": "fdabd95b-7c56-47d6-a6ba-a7c5f264533f",
                        "upn": "user2@contoso.com"
                    }
                ]
            }
        ],
        "permissions": [
            {
                "principal": {
                    "objectId": "27b9a0eb-bb71-4297-9f1f-c462dab7192a",
                    "upn": "user3@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            },
            {
                "principal": {
                    "objectId": "4c8bc8ce-225c-4fcf-b09a-047030baab31",
                    "upn": "user4@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            }
        ]
    }

> [!NOTE]
> In PUT dit is niet vereist om op te geven van de nettolading van een item in de hoofdtekst: opslag kan worden gebruikt om bij te werken net rollen en/of machtigingen.
> 
> 

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png
