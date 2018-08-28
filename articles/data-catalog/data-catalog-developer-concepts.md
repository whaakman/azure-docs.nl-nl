---
title: Azure Data Catalog-concepten voor ontwikkelaars
description: Inleiding tot de belangrijkste concepten in Azure Data Catalog conceptuele model als weergegeven met de REST-API-catalogus.
services: data-catalog
author: spelluru
ms.author: spelluru
ms.assetid: 89de9137-a0a4-40d1-9f8d-625acad31619
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 753b4660c8ca47f12aace87a254b93a88db8aaa7
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053776"
---
# <a name="azure-data-catalog-developer-concepts"></a>Azure Data Catalog-concepten voor ontwikkelaars
Microsoft **Azure Data Catalog** is een volledig beheerde cloudservice die mogelijkheden voor detectie van gegevensbronnen en crowdsourcing metagegevens van de gegevensbron biedt. Ontwikkelaars kunnen de service via de REST-API's gebruiken. Informatie over de concepten die zijn geïmplementeerd in de service is het belangrijk is voor ontwikkelaars om te integreren in is **Azure Data Catalog**.

## <a name="key-concepts"></a>Belangrijkste concepten
De **Azure Data Catalog** conceptuele model is gebaseerd op vier belangrijke concepten: de **catalogus**, **gebruikers**, **activa**, en  **Aantekeningen**.

![Concept][1]

*Afbeelding 1: Azure Data Catalog vereenvoudigd conceptuele model*

### <a name="catalog"></a>Catalogus
Een **catalogus** is de container op het hoogste niveau voor alle metagegevens van een organisatie worden opgeslagen. Er is een **catalogus** toegestaan per Azure-Account. Catalogi zijn gekoppeld aan een Azure-abonnement, maar slechts één **catalogus** kunnen worden gemaakt voor een bepaald Azure-account, ook al een account kan meerdere abonnementen hebt.

Een catalogus bevat **gebruikers** en **activa**.

### <a name="users"></a>Gebruikers
Gebruikers zijn beveiligings-principals die gemachtigd zijn voor het uitvoeren van acties (zoeken in de catalogus, toevoegen, bewerken of verwijderen van items, enzovoort) in de catalogus.

Er zijn diverse verschillende rollen die een gebruiker kan hebben. Voor informatie over rollen, Zie de sectie functies en autorisatie.

Afzonderlijke gebruikers en beveiligingsgroepen worden toegevoegd.

Azure Data Catalog maakt gebruik van Azure Active Directory voor identiteits-en toegang. Elke gebruiker catalogus moet een lid van de Active Directory voor het account.

### <a name="assets"></a>Assets
Een **catalogus** gegevensassets bevat. **Activa** de eenheid van de granulariteit die worden beheerd door de catalogus.

De granulatie van een asset is afhankelijk van de gegevensbron. Een asset kan voor SQL Server of Oracle Database zijn een tabel of weergave. Een asset kan voor SQL Server Analysis Services zijn een meting, een dimensie of een Key Performance Indicator (KPI). Een actief is voor SQL Server Reporting Services, een rapport.

Een **Asset** het dat u toevoegt of verwijdert uit een catalogus. Dit is de maateenheid resultaat je weer toegang vanaf krijgen **zoeken**.

Een **Asset** wordt samengesteld uit de naam, de locatie, en het type en de aantekeningen die verder wordt beschreven.

### <a name="annotations"></a>aantekeningen
Aantekeningen zijn items die staan voor metagegevens over de activa.

Voorbeelden van aantekeningen zijn beschrijving, labels, schema, documentatie, enzovoort. Een volledige lijst van de asset typen en aantekeningentypen zijn in de sectie Asset Object model.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Crowdsourcing aantekeningen en het gebruikersperspectief (multipliciteit van advies)
Een belangrijk aspect van Azure Data Catalog is hoe het bijdraagt aan de crowdsourcing van metagegevens in het systeem. In plaats van naar een wiki-aanpak: waar er is slechts één advies en de laatste schrijver WINS-server: de Azure Data Catalog-model kunt meerdere advies aan naast elkaar bevinden zich in het systeem.

Deze aanpak geeft de echte wereld van gegevens op ondernemingsniveau waarin verschillende gebruikers verschillende perspectieven voor een bepaalde asset hebben:

* Een databasebeheerder kan voorzien in informatie over serviceovereenkomsten, of de beschikbare segmenteringsmodel bulksgewijs ETL-bewerkingen
* Een steward gegevens kan bieden informatie over de bedrijfsprocessen waarop de activa van toepassing is, of de classificaties die het bedrijf heeft toegepast op het
* Een analist Financiën mogen bevatten informatie over hoe de gegevens wordt gebruikt tijdens het einde van de periode rapportagetaken

Ter ondersteuning van dit voorbeeld, kunt elke gebruiker, de DBA, de data steward en de analist, een beschrijving toevoegen aan een enkele tabel die is geregistreerd in de catalogus. Alle beschrijvingen worden bijgehouden in het systeem en in de Azure Data Catalog-portal alle beschrijvingen worden weergegeven.

Dit patroon wordt toegepast op de meeste van de items in het objectmodel objecttypen in de JSON-nettolading zijn daarom vaak matrices voor eigenschappen waarbij u een singleton had verwacht.

Onder de asset is hoofdmap bijvoorbeeld een matrix met objecten van de beschrijving. De matrixeigenschap is met de naam 'beschrijvingen'. Een object voor een beschrijving heeft één eigenschap - beschrijving. Het patroon is dat elke gebruiker die de typen beschrijving een object voor een beschrijving wordt voor de waarde die is opgegeven door de gebruiker gemaakt.

De UX kan vervolgens kiezen hoe u de combinatie van weergeven. Er zijn drie verschillende patronen om weer te geven.

* De eenvoudigste patroon is 'Alles weergeven'. In dit patroon, worden alle objecten weergegeven in een lijst weergegeven. De Azure Data Catalog-portal UX maakt gebruik van dit patroon voor beschrijving.
* Een andere patroon is 'Samenvoegen'. In dit patroon, worden alle waarden van de verschillende gebruikers samen, samengevoegd met dubbele verwijderd. Voorbeelden van dit patroon in de portal voor Azure Data Catalog UX zijn de eigenschappen van tags en experts.
* Een derde patroon is de 'laatste schrijver wins'. In dit patroon alleen de meest recente waarde hebt getypt in weergegeven. friendlyName is een voorbeeld van dit patroon.

## <a name="asset-object-model"></a>Asset-objectmodel
Zoals geïntroduceerd in de sectie van de belangrijkste concepten, de **Azure Data Catalog** objectmodel-items, die activa of aantekeningen worden kunnen bevat. Objecten hebben eigenschappen die optioneel of vereist worden kunnen. Sommige eigenschappen zijn van toepassing op alle items. Sommige eigenschappen zijn van toepassing op alle activa. Sommige eigenschappen zijn van toepassing alleen op specifieke asset typen.

### <a name="system-properties"></a>Systeemeigenschappen
<table><tr><td><b>De naam van eigenschap</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr><tr><td>tijdstempel</td><td>DateTime</td><td>De laatste keer dat het item is gewijzigd. Dit veld wordt gegenereerd door de server als een item wordt toegevoegd en telkens wanneer een item wordt bijgewerkt. De waarde van deze eigenschap wordt genegeerd op invoer van de bewerkingen te publiceren.</td></tr><tr><td>id</td><td>URI</td><td>De absolute url van het item (alleen-lezen). Dit is de unieke adresseerbare URI voor het item.  De waarde van deze eigenschap wordt genegeerd op invoer van de bewerkingen te publiceren.</td></tr><tr><td>type</td><td>Reeks</td><td>Het type van de asset (alleen-lezen).</td></tr><tr><td>ETag</td><td>Reeks</td><td>Een tekenreeks die overeenkomt met de versie van het item dat kan worden gebruikt voor Optimistisch gelijktijdigheidbeheer bij het uitvoeren van bewerkingen die items in de catalogus bijwerken. ' * ' kan worden gebruikt om een willekeurige waarde.</td></tr></table>

### <a name="common-properties"></a>Algemene eigenschappen
Deze eigenschappen zijn van toepassing op alle root asset typen en alle aantekeningentypen.

<table>
<tr><td><b>De naam van eigenschap</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr>
<tr><td>fromSourceSystem</td><td>Booleaans</td><td>Geeft aan of de gegevens van het item is afgeleid van een bronsysteem (zoals Sql Server-Database, Oracle-Database) of door een gebruiker geschreven.</td></tr>
</table>

### <a name="common-root-properties"></a>Algemene eigenschappen van hoofdmap
<p>
Deze eigenschappen zijn van toepassing op alle root asset-typen.

<table><tr><td><b>De naam van eigenschap</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr><tr><td>naam</td><td>Reeks</td><td>Een naam die is afgeleid van de gegevens van de bron-locatie</td></tr><tr><td>DSL</td><td>DataSourceLocation</td><td>Een unieke beschrijving van de gegevensbron en is een van de id's voor de asset. (Zie sectie dubbele identiteit).  De structuur van de dsl is afhankelijk van het type protocol en de bron.</td></tr><tr><td>Gegevensbron</td><td>DataSourceInfo</td><td>Meer informatie over het type van de asset.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>Beschrijving van de gebruiker die deze asset meest recent is geregistreerd.  Bevat de unieke id voor de gebruiker (upn) en een weergavenaam (lastName en firstName).</td></tr><tr><td>containerId</td><td>Reeks</td><td>Id van de container-asset voor de gegevensbron. Deze eigenschap wordt niet ondersteund voor het type van de Container.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Algemene eigenschappen voor niet-singleton-aantekening
Deze eigenschappen zijn van toepassing op alle niet-singleton aantekeningentypen (aantekeningen, die kunnen worden meerdere per asset).

<table>
<tr><td><b>De naam van eigenschap</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr>
<tr><td>sleutel</td><td>Reeks</td><td>Een gebruiker opgegeven sleutel, die de aantekening in de huidige verzameling wordt aangeduid. Lengte van de sleutel kan niet groter zijn dan 256 tekens.</td></tr>
</table>

### <a name="root-asset-types"></a>Hoofdmap asset typen
Hoofdmap asset typen zijn de typen die staan voor de verschillende typen gegevensassets dat kunnen worden geregistreerd in de catalogus. Er is een weergave, waarin wordt beschreven asset en aantekeningen opgenomen in de weergave voor elk type hoofdmap. Weergavenaam moet worden gebruikt in het bijbehorende {view_name} url-segment bij het publiceren van een asset met behulp van REST-API.

<table><tr><td><b>Assettype (weergavenaam)</b></td><td><b>Aanvullende eigenschappen</b></td><td><b>Gegevenstype</b></td><td><b>Toegestane aantekeningen</b></td><td><b>Opmerkingen</b></td></tr><tr><td>Tabel ('tabellen')</td><td></td><td></td><td>Beschrijving<p>FriendlyName<p>Label<p>Schema<p>ColumnDescription<p>ColumnTag<p> Expert<p>Preview<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>ColumnDataClassification<p>Documentatie<p></td><td>Een tabel vertegenwoordigt geen tabellaire gegevens.  Bijvoorbeeld: SQL-tabel, SQL-weergave, Tabellaire Analysis Services-tabel, Analysis Services Multidimensional dimensie, Oracle-tabel, enzovoort.   </td></tr><tr><td>Meting ("metingen")</td><td></td><td></td><td>Beschrijving<p>FriendlyName<p>Label<p>Expert<p>AccessInstruction<p>Documentatie<p></td><td>Dit type vertegenwoordigt een Analysis Services-meting.</td></tr><tr><td></td><td>meting</td><td>Kolom</td><td></td><td>Metagegevens met een beschrijving van de meting</td></tr><tr><td></td><td>isCalculated </td><td>Booleaans</td><td></td><td>Hiermee geeft u als de meting is berekend of niet.</td></tr><tr><td></td><td>measureGroup</td><td>Reeks</td><td></td><td>Fysieke container voor de meting</td></tr><td>KPI ("kpis")</td><td></td><td></td><td>Beschrijving<p>FriendlyName<p>Label<p>Expert<p>AccessInstruction<p>Documentatie</td><td></td></tr><tr><td></td><td>measureGroup</td><td>Reeks</td><td></td><td>Fysieke container voor de meting</td></tr><tr><td></td><td>goalExpression</td><td>Reeks</td><td></td><td>Een numerieke MDX-expressie of een berekening waarmee de doelwaarde van de KPI wordt geretourneerd.</td></tr><tr><td></td><td>valueExpression</td><td>Reeks</td><td></td><td>Een numerieke MDX-expressie die als resultaat de werkelijke waarde van de KPI geeft.</td></tr><tr><td></td><td>statusExpression</td><td>Reeks</td><td></td><td>Een MDX-expressie die de status van de KPI op een gegeven moment in-time vertegenwoordigt.</td></tr><tr><td></td><td>trendExpression</td><td>Reeks</td><td></td><td>Een MDX-expressie die wordt geëvalueerd als de waarde van de KPI na verloop van tijd. De trend mag elk criterium op basis van tijd, dat is handig in een specifieke zakelijke context.</td>
<tr><td>Rapport ('rapporten")</td><td></td><td></td><td>Beschrijving<p>FriendlyName<p>Label<p>Expert<p>AccessInstruction<p>Documentatie<p></td><td>Dit type vertegenwoordigt een SQL Server Reporting Services-rapport </td></tr><tr><td></td><td>assetCreatedDate</td><td>Reeks</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>Reeks</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>Reeks</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>Reeks</td><td></td><td></td></tr><tr><td>Container ("containers")</td><td></td><td></td><td>Beschrijving<p>FriendlyName<p>Label<p>Expert<p>AccessInstruction<p>Documentatie<p></td><td>Dit type vertegenwoordigt een container van andere items, zoals een SQL-database, een Azure-Blobs-container of een Analysis Services-model.</td></tr></table>

### <a name="annotation-types"></a>Aantekeningentypen
Aantekeningentypen vertegenwoordigen typen metagegevens die kunnen worden toegewezen aan andere typen in de catalogus.

<table>
<tr><td><b>Type aantekening (geneste weergavenaam)</b></td><td><b>Aanvullende eigenschappen</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr>

<tr><td>Beschrijving ("beschrijvingen")</td><td></td><td></td><td>Deze eigenschap bevat een beschrijving op voor een asset. Elke gebruiker van het systeem kan hun eigen beschrijving toevoegen.  Alleen voor die gebruiker kan het object beschrijving bewerken.  (Beheerders en Asset eigenaren kunnen verwijderen van het object voor een beschrijving, maar niet bewerken). Het systeem onderhoudt beschrijvingen van de gebruikers afzonderlijk.  Er is dus een matrix met beschrijvingen voor elke asset (één voor elke gebruiker die heeft bijgedragen hun kennis op over de asset, naast het mogelijk een bestand met gegevens die zijn afgeleid van de gegevensbron).</td></tr>
<tr><td></td><td>description</td><td>tekenreeks</td><td>Een korte beschrijving (2-3 regels) van de asset</td></tr>

<tr><td>Tag ('tags")</td><td></td><td></td><td>Deze eigenschap definieert een code voor een asset. Elke gebruiker van het systeem kan meerdere labels voor een asset toevoegen.  Alleen de gebruiker die heeft gemaakt van de Tag-objecten kunt bewerken.  (Beheerders en Asset eigenaren kunnen verwijderen van het object Tag, maar niet bewerken). Het systeem onderhoudt afzonderlijk gebruikers labels.  Er is dus een matrix met objecten van de Tag op elke asset.</td></tr>
<tr><td></td><td>tag</td><td>tekenreeks</td><td>Een label met een beschrijving van de asset.</td></tr>

<tr><td>FriendlyName ("friendlyName")</td><td></td><td></td><td>Deze eigenschap bevat een beschrijvende naam voor een asset. FriendlyName is een singleton-aantekening - slechts één FriendlyName kan worden toegevoegd aan een asset.  Alleen de gebruiker die FriendlyName object is gemaakt, kunt deze bewerken. (Beheerders en Asset eigenaren kunnen de FriendlyName-object verwijderen, maar niet bewerken). Het systeem onderhoudt beschrijvende namen van de gebruikers afzonderlijk.</td></tr>
<tr><td></td><td>friendlyName</td><td>tekenreeks</td><td>Een beschrijvende naam van de asset.</td></tr>

<tr><td>Schema ('schema')</td><td></td><td></td><td>Het Schema wordt de structuur van de gegevens beschreven.  Het bevat de kenmerknamen (kolom, kenmerk, veld, enzovoort), alsook andere metagegevens van het type.  Deze informatie wordt afgeleid uit de gegevensbron.  Het schema is een singleton-aantekening: slechts één Schema kan worden toegevoegd voor een asset.</td></tr>
<tr><td></td><td>Kolommen</td><td>Kolom]</td><td>Een matrix met objecten van de kolom. Ze beschrijven de kolom met gegevens die zijn afgeleid van de gegevensbron.</td></tr>

<tr><td>ColumnDescription ("columnDescriptions")</td><td></td><td></td><td>Deze eigenschap bevat een beschrijving op voor een kolom.  Elke gebruiker van het systeem kan hun eigen beschrijvingen voor meerdere kolommen (maximaal één per kolom) toevoegen. Alleen de gebruiker die ColumnDescription objecten heeft gemaakt, kan ze bewerken.  (Beheerders en Asset eigenaren kunnen de ColumnDescription-object verwijderen, maar niet bewerken). Het systeem onderhoudt afzonderlijk kolombeschrijvingen van deze gebruiker.  Er is dus een matrix met objecten ColumnDescription voor elke asset (één per kolom voor elke gebruiker die heeft bijgedragen hun kennis op over de kolom naast mogelijk een bestand met gegevens die zijn afgeleid van de gegevensbron).  De ColumnDescription is los gebonden aan het Schema, zodat deze niet gesynchroniseerd. De ColumnDescription beschrijft een kolom die niet meer in het schema bestaat.  Het is aan de writer beschrijving en het schema in om gesynchroniseerd te houden.  De gegevensbron mogelijk ook kolommen beschrijving informatie en ze zijn aanvullende ColumnDescription-objecten die moeten worden gemaakt wanneer het programma wordt uitgevoerd.</td></tr>
<tr><td></td><td>Kolomnaam</td><td>Reeks</td><td>De naam van de kolom die deze beschrijving naar het verwijst.</td></tr>
<tr><td></td><td>description</td><td>Reeks</td><td>een korte beschrijving (2-3 regels) van de kolom.</td></tr>

<tr><td>ColumnTag ("columnTags")</td><td></td><td></td><td>Deze eigenschap bevat een label voor een kolom. Elke gebruiker van het systeem meerdere labels voor een bepaalde kolom kunt toevoegen en labels voor meerdere kolommen kunt toevoegen. Alleen de gebruiker die ColumnTag objecten heeft gemaakt, kan ze bewerken. (Beheerders en Asset eigenaren kunnen de ColumnTag-object verwijderen, maar niet bewerken). Het systeem houdt deze gebruikers kolom labels afzonderlijk.  Er is dus een matrix met objecten ColumnTag voor elke asset.  De ColumnTag is los gebonden aan het schema, zodat deze niet gesynchroniseerd. De ColumnTag beschrijft een kolom die niet meer in het schema bestaat.  Het is aan de writer kolom label en het schema in om gesynchroniseerd te houden.</td></tr>
<tr><td></td><td>Kolomnaam</td><td>Reeks</td><td>De naam van de kolom die dit label naar verwijst.</td></tr>
<tr><td></td><td>tag</td><td>Reeks</td><td>Een label met een beschrijving van de kolom.</td></tr>

<tr><td>Deskundige ("experts")</td><td></td><td></td><td>Deze eigenschap bevat een gebruiker die wordt beschouwd als een expert in de gegevensset. De deskundigen opinions(descriptions) Bel aan het begin van de UX wanneer beschrijvingen weergegeven. Elke gebruiker kan een eigen experts opgeven. Alleen voor die gebruiker kan de experts-object niet bewerken. (Beheerders en Asset eigenaren kunnen de deskundige objecten verwijderen, maar niet bewerken).</td></tr>
<tr><td></td><td>expert</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Preview-versie ("previews")</td><td></td><td></td><td>De Preview-versie bevat een momentopname van de bovenste 20 rijen met gegevens voor de asset. Preview-versie alleen zinvol zijn voor bepaalde typen activa (het zinvol voor de tabel, maar niet voor de meting).</td></tr>
<tr><td></td><td>preview</td><td>object]</td><td>Matrix met objecten die staan voor een kolom.  Elk object moet een eigenschap toewijzen aan een kolom met een waarde voor die kolom voor de rij.</td></tr>

<tr><td>AccessInstruction ("accessInstructions")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>mimeType</td><td>tekenreeks</td><td>Het mime-type van de inhoud.</td></tr>
<tr><td></td><td>content</td><td>tekenreeks</td><td>De instructies voor het toegang krijgen tot deze gegevensasset. De inhoud wordt mogelijk een URL, een e-mailadres of een set met instructies.</td></tr>

<tr><td>TableDataProfile ("tableDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Aantal_rijen</td></td><td>int</td><td>Het aantal rijen in de gegevensset</td></tr>
<tr><td></td><td>grootte</td><td>lengte</td><td>De grootte in bytes van de gegevensset.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>tekenreeks</td><td>De laatste keer dat het schema is gewijzigd</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>tekenreeks</td><td>De laatste keer dat de gegevensset is gewijzigd (gegevens is toegevoegd, gewijzigd, of verwijderen)</td></tr>

<tr><td>ColumnsDataProfile ("columnsDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Kolommen</td></td><td>ColumnDataProfile]</td><td>Een matrix van profielen voor kolom-gegevens.</td></tr>

<tr><td>ColumnDataClassification ("columnDataClassifications")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Kolomnaam</td><td>Reeks</td><td>De naam van de kolom die deze classificatie naar verwijst.</td></tr>
<tr><td></td><td>classificatie</td><td>Reeks</td><td>De indeling van de gegevens in deze kolom.</td></tr>

<tr><td>Documentatie ("documentatie")</td><td></td><td></td><td>Een bepaalde asset kan slechts één documentatie die zijn gekoppeld aan deze hebben.</td></tr>
<tr><td></td><td>mimeType</td><td>tekenreeks</td><td>Het mime-type van de inhoud.</td></tr>
<tr><td></td><td>content</td><td>tekenreeks</td><td>De inhoud van de documentatie.</td></tr>

</table>

### <a name="common-types"></a>Algemene typen
Algemene typen kunnen worden gebruikt als de typen voor eigenschappen, maar er zijn geen Items.

<table>
<tr><td><b>Gemeenschappelijk Type</b></td><td><b>Eigenschappen</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr>
<tr><td>DataSourceInfo</td><td></td><td></td><td></td></tr>
<tr><td></td><td>sourceType</td><td>tekenreeks</td><td>Beschrijving van het type van de gegevensbron.  Bijvoorbeeld: SQL Server, Oracle Database, enzovoort.  </td></tr>
<tr><td></td><td>objectType</td><td>tekenreeks</td><td>Beschrijving van het type object in de gegevensbron. Bijvoorbeeld: tabel, bekijken voor SQL Server.</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>protocol</td><td>tekenreeks</td><td>Vereist. Beschrijving van een protocol dat wordt gebruikt om te communiceren met de gegevensbron. Bijvoorbeeld: "tds" voor SQl Server, "oracle' voor Oracle, enzovoort. Raadpleeg [verwijzing opgegeven - DSL-structuur van gegevensbron](data-catalog-dsr.md) voor de lijst met ondersteunde protocollen.</td></tr>
<tr><td></td><td>Adres</td><td>Woordenlijst<string, object></td><td>Vereist. Adres is een set gegevens die specifiek zijn voor het protocol dat wordt gebruikt voor het identificeren van de gegevensbron waarnaar wordt verwezen. De gegevens voor adres binnen het bereik van een bepaald protocol, wat betekent dat deze zonder te weten het protocol is geen betekenis heeft.</td></tr>
<tr><td></td><td>verificatie</td><td>tekenreeks</td><td>Optioneel. Het verificatieschema gebruikt om te communiceren met de gegevensbron. Bijvoorbeeld: windows, oauth, enzovoort.</td></tr>
<tr><td></td><td>connectionProperties</td><td>Woordenlijst<string, object></td><td>Optioneel. Meer informatie over hoe u verbinding maakt met een gegevensbron.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>De back-end wordt een validatie van de opgegeven eigenschappen op basis van AAD tijdens de publicatie niet uitvoeren.</td></tr>
<tr><td></td><td>UPN</td><td>tekenreeks</td><td>De unieke e-mailadres van gebruiker. Moet worden opgegeven als object-id is niet opgegeven of in de context van de eigenschap 'lastRegisteredBy', anders is optioneel.</td></tr>
<tr><td></td><td>object-id</td><td>GUID</td><td>Gebruiker of beveiligingsgroep groep AAD-identiteit. Optioneel. Moet worden opgegeven als upn niet is opgegeven, anders is optioneel.</td></tr>
<tr><td></td><td>Voornaam</td><td>tekenreeks</td><td>De voornaam van de gebruiker (voor weergavedoeleinden). Optioneel. Alleen geldig in de context van de eigenschap 'lastRegisteredBy'. Kan niet worden opgegeven bij het opgeven van beveiligings-principal voor 'functies', 'machtigingen' en "experts".</td></tr>
<tr><td></td><td>lastName</td><td>tekenreeks</td><td>De achternaam van de gebruiker (voor weergavedoeleinden). Optioneel. Alleen geldig in de context van de eigenschap 'lastRegisteredBy'. Kan niet worden opgegeven bij het opgeven van beveiligings-principal voor 'functies', 'machtigingen' en "experts".</td></tr>

<tr><td>Kolom</td><td></td><td></td><td></td></tr>
<tr><td></td><td>naam</td><td>tekenreeks</td><td>De naam van de kolom of het kenmerk.</td></tr>
<tr><td></td><td>type</td><td>tekenreeks</td><td>het gegevenstype van de kolom of het kenmerk. De toegestane typen, is afhankelijk van de sourceType gegevens van de asset.  Alleen een subset van de typen wordt ondersteund.</td></tr>
<tr><td></td><td>maxLength</td><td>int</td><td>De maximale lengte van de kolom of het kenmerk. Afgeleid van gegevensbron. Alleen van toepassing op bepaalde typen gegevensbronnen.</td></tr>
<tr><td></td><td>precisie</td><td>byte</td><td>De precisie voor de kolom of het kenmerk. Afgeleid van gegevensbron. Alleen van toepassing op bepaalde typen gegevensbronnen.</td></tr>
<tr><td></td><td>isNullable</td><td>Booleaans</td><td>Of de kolom aan de waarde null hebben of niet is toegestaan. Afgeleid van gegevensbron. Alleen van toepassing op bepaalde typen gegevensbronnen.</td></tr>
<tr><td></td><td>expressie</td><td>tekenreeks</td><td>Als de waarde een berekende kolom is, bevat dit veld in de expressie waarmee de waarde wordt uitgedrukt. Afgeleid van gegevensbron. Alleen van toepassing op bepaalde typen gegevensbronnen.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Kolomnaam </td><td>tekenreeks</td><td>De naam van de kolom</td></tr>
<tr><td></td><td>type </td><td>tekenreeks</td><td>Het type van de kolom</td></tr>
<tr><td></td><td>min. </td><td>tekenreeks</td><td>De minimale waarde in de gegevensset</td></tr>
<tr><td></td><td>max </td><td>tekenreeks</td><td>De maximale waarde in de gegevensset</td></tr>
<tr><td></td><td>gem. </td><td>double</td><td>De gemiddelde waarde in de gegevensset</td></tr>
<tr><td></td><td>stdev </td><td>double</td><td>De standaardafwijking voor de gegevensset</td></tr>
<tr><td></td><td>nullCount </td><td>int</td><td>Het aantal null-waarden in de gegevensset</td></tr>
<tr><td></td><td>distinctCount  </td><td>int</td><td>Het aantal distinctieve waarden in de gegevensset</td></tr>


</table>

## <a name="asset-identity"></a>Asset-id
Azure Data Catalog maakt gebruik van 'protocol' en de identiteit eigenschappen uit de eigenschappenverzameling "adres" van de DataSourceLocation 'dsl'-eigenschap voor het genereren van de identiteit van de asset die wordt gebruikt voor het adres van de activa in de catalogus.
Het protocol "tds" heeft bijvoorbeeld identiteit eigenschappen 'server', 'database', 'schema' en 'object'. De combinaties van het protocol en de id-eigenschappen worden gebruikt voor het genereren van de identiteit van de SQL Server-tabel Asset.
Azure Data Catalog biedt diverse ingebouwde bron-protocollen die worden vermeld op [verwijzing opgegeven - DSL-structuur van gegevensbron](data-catalog-dsr.md).
De set met ondersteunde protocollen kan programmatisch worden uitgebreid (Zie Naslaginformatie over Data Catalog REST API). Beheerders van de catalogus kunnen zich registreren voor aangepaste gegevensbronprotocollen. De volgende tabel beschrijft de eigenschappen die nodig zijn voor het registreren van een aangepaste protocol.

### <a name="custom-data-source-protocol-specification"></a>Aangepaste protocolspecificatie van gegevensbron
<table>
<tr><td><b>Type</b></td><td><b>Eigenschappen</b></td><td><b>Gegevenstype</b></td><td><b>Opmerkingen</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>naamruimte</td><td>tekenreeks</td><td>De naamruimte van het protocol. Namespace moet 1 tot 255 tekens lang zijn en een of meer niet-lege onderdelen gescheiden door een punt (.) bevatten. Elk onderdeel moet afkomstig zijn van 1 tot 255 tekens lang zijn, beginnen met een letter en mag alleen letters en cijfers.</td></tr>
<tr><td></td><td>naam</td><td>tekenreeks</td><td>De naam van het protocol. Naam moet tussen 1 tot 255 tekens lang zijn, beginnen met een letter en mag alleen letters, cijfers en streepjes (-) bevatten.</td></tr>
<tr><td></td><td>identityProperties</td><td>DataSourceProtocolIdentityProperty[]</td><td>Lijst met identiteitseigenschappen voor, moet ten minste één echter meer dan 20 eigenschappen bevatten. Bijvoorbeeld: 'server', 'database', 'schema', 'object' identiteitseigenschappen van het protocol 'tds' zijn.</td></tr>
<tr><td></td><td>identitySets</td><td>DataSourceProtocolIdentitySet[]</td><td>Lijst met identiteit wordt ingesteld. Hiermee definieert u sets identiteitseigenschappen die geldige asset identiteit vertegenwoordigen. Moet ten minste één echter meer dan 20 sets bevatten. Bijvoorbeeld: {'server', 'database', 'schema' en 'object'} is een identiteit die is ingesteld voor "tds"-protocol, dat Hiermee definieert u de id van de Sql Server-tabel asset.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>naam</td><td>tekenreeks</td><td>De naam van de eigenschap. De naam moet tussen 1 en 100 tekens lang zijn, beginnen met een letter en mag alleen letters en cijfers bevatten.</td></tr>
<tr><td></td><td>type</td><td>tekenreeks</td><td>Het type van de eigenschap. Ondersteunde waarden: "bool", Booleaanse waarde ","byte","guid", 'int', 'integer',"long","string","url"</td></tr>
<tr><td></td><td>ignoreCase</td><td>BOOL</td><td>Geeft aan of de aanvraag moet worden genegeerd bij het gebruik van de waarde van eigenschap. Kan alleen worden opgegeven voor eigenschappen met het type 'tekenreeks'. Standaardwaarde is false.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>BOOL]</td><td>Geeft aan of de aanvraag moet worden genegeerd voor elk segment van de url-pad genoteerd. Kan alleen worden opgegeven voor 'url'-type-eigenschappen. Standaardwaarde is [false].</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>naam</td><td>tekenreeks</td><td>De naam van de identiteit is ingesteld.</td></tr>
<tr><td></td><td>properties</td><td>String]</td><td>De lijst met identiteitseigenschappen die zijn opgenomen in deze identiteit is ingesteld. Het mag geen dubbele waarden bevatten. Elke eigenschap waarnaar wordt verwezen door de identiteitsset moet worden gedefinieerd in de lijst 'identityProperties' van het protocol.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Rollen en -autorisatie
Microsoft Azure Data Catalog biedt autorisatiemogelijkheden voor voor CRUD-bewerkingen op activa en aantekeningen.

## <a name="key-concepts"></a>Belangrijkste concepten
De Azure Data Catalog maakt gebruik van twee autorisatiemechanismen voor:

* Autorisatie op basis van rollen
* Op basis van claims machtiging

### <a name="roles"></a>Rollen
Er zijn drie rollen: **beheerder**, **eigenaar**, en **Inzender**.  Elke rol heeft een bereik en de rechten die worden samengevat in de volgende tabel.

<table><tr><td><b>Rol</b></td><td><b>Bereik</b></td><td><b>Rechten</b></td></tr><tr><td>Beheerder</td><td>Catalogus (alle activa/aantekeningen in de catalogus)</td><td>Lezen verwijderen ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Eigenaar</td><td>Elke asset (hoofditem)</td><td>Lezen verwijderen ViewRoles

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Inzender</td><td>Elke individuele asset en aantekening</td><td>Lezen Update verwijderen ViewRoles Opmerking: alle rechten worden ingetrokken als de lezen rechts op het item is ingetrokken voor de Inzender</td></tr></table>

> [!NOTE]
> **Lezen**, **Update**, **verwijderen**, **ViewRoles** rechten zijn van toepassing op een willekeurig item (actief of aantekening) tijdens het **TakeOwnership**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** zijn alleen van toepassing op de basis-asset.
> 
> **Verwijder** rechts is van toepassing op een item en subitems of één item eronder. Bijvoorbeeld: een asset ook verwijdert alle aantekeningen voor de activa.
> 
> 

### <a name="permissions"></a>Machtigingen
Er is een machtiging als lijst met vermeldingen voor toegangsbeheer. Elk access control entry toegewezen set met rechten voor een beveiligings-principal. Machtigingen kunnen alleen worden opgegeven voor een asset (dat wil zeggen, hoofditem) en toegepast op de asset en eventuele subitems.

Tijdens de **Azure Data Catalog** preview, alleen **lezen** rechts wordt ondersteund in de lijst met machtigingen om in te schakelen scenario om te beperken van de zichtbaarheid van een asset.

Elke geverifieerde gebruiker heeft standaard **lezen** tenzij zichtbaarheid beperkt tot het instellen van beveiligings-principals in de machtigingen is voor een item in de catalogus naar rechts.

## <a name="rest-api"></a>REST-API
**Plaats** en **POST** weergave-item-aanvragen kan worden gebruikt voor het beheren van rollen en machtigingen: naast de nettolading van het item, twee eigenschappen kunnen worden opgegeven **rollen** en  **machtigingen**.

> [!NOTE]
> **machtigingen** alleen van toepassing op een item met root.
> 
> **Eigenaar** rol alleen van toepassing op een item met root.
> 
> Standaard wanneer een item wordt gemaakt in de catalogus zijn **Inzender** is ingesteld op de huidige geverifieerde gebruiker. Als het item moet worden bijgewerkt door iedereen, **Inzender** moet worden ingesteld op &lt;iedereen&gt; speciale beveiligings-principal in de **rollen** eigenschap wanneer het item eerst gepubliceerd (Zie ook in het volgende voorbeeld). **Inzender** kan niet worden gewijzigd en blijft hetzelfde tijdens de levensduur van een item (zelfs **beheerder** of **eigenaar** niet het recht hebben om te wijzigen de **Inzender**). De enige waarde die wordt ondersteund voor de expliciete instelling van de **Inzender** is &lt;iedereen&gt;: **Inzender** mag alleen bestaan uit een gebruiker die een item heeft gemaakt of &lt;iedereen &gt;.
> 
> 

### <a name="examples"></a>Voorbeelden
**Inzender ingesteld op &lt;iedereen&gt; bij het publiceren van een item.**
Speciale beveiligings-principal &lt;iedereen&gt; "00000000-0000-0000-0000-000000000201" object-id heeft.
  **VERZENDEN** https://api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

> [!NOTE]
> Sommige implementaties van HTTP-client kunnen automatisch opnieuw aanvragen in reactie op een 302 van de server, maar meestal van strook / autorisatie-header van de aanvraag. Omdat de autorisatie-header is vereist om aan te vragen tot Azure Data Catalog, moet u ervoor zorgen dat de autorisatie-header wordt nog steeds vermeld als opnieuw uitgeven van een aanvraag naar de omleidingslocatie van een door Azure Data Catalog hebt opgegeven. De volgende voorbeeldcode laat zien met behulp van de .NET-HttpWebRequest-object.
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

  **Eigenaren toewijzen en de zichtbaarheid voor een bestaand item met root beperken**: **plaatsen** https://api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30

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
> In de PUT het geen vereiste is om op te geven van de nettolading van een item in de hoofdtekst: opslag kan worden gebruikt om bij te werken alleen functies en/of machtigingen.
> 
> 

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png
