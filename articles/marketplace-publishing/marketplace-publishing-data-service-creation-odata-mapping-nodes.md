---
title: Handleiding voor het maken van een Service van gegevens voor de Marketplace | Microsoft Docs
description: Gedetailleerde instructies voor het maken, certificeren en implementeren van een Service voor gegevens aanschaffen op Azure Marketplace.
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 107baab2-5828-4158-abdf-59a580c80d37
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 8ff76ea21ba684ae2a2afcb74d66b4912d7be053
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Wat is de knooppunten schema voor het toewijzen van een bestaande webservice aan OData via CSDL?
> [!IMPORTANT]
> **Op dit moment wordt niet langer zijn voorbereiden op alle nieuwe gegevensservice uitgevers. Nieuwe dataservices wordt niet goedgekeurd voor de aanbieding.** Als u een SaaS business-toepassing die u wilt publiceren op AppSource hebt u meer informatie vindt [hier](https://appsource.microsoft.com/partners). Als u beschikt over een IaaS-toepassingen of developer-service die u wilt publiceren op Azure Marketplace u meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).
>
>

Dit document helpt de structuur van het knooppunt voor het toewijzen van een OData-protocol aan CSDL verduidelijken. Het is belangrijk te weten dat de structuur knooppunt goed is gevormd XML. Basis-, bovenliggende en onderliggende schema kan worden gebruikt bij het ontwerpen van uw OData-toewijzing.

## <a name="ignored-elements"></a>Genegeerd elementen
Hier volgen de hoog niveau CSDL-elementen (XML-knooppunten) die u niet tijdens het importeren van metagegevens van de webservice worden gebruikt door de back-end voor Azure Marketplace. Ze kunnen worden gebruikt, maar worden genegeerd.

| Element | Bereik |
| --- | --- |
| Met behulp van Element |Het knooppunt, subknooppunten en alle kenmerken |
| Documentation-Element |Het knooppunt, subknooppunten en alle kenmerken |
| ComplexType |Het knooppunt, subknooppunten en alle kenmerken |
| Koppelingelement |Het knooppunt, subknooppunten en alle kenmerken |
| Uitgebreide eigenschap |Het knooppunt, subknooppunten en alle kenmerken |
| EntityContainer |Alleen de volgende kenmerken worden genegeerd: *breidt* en *AssociationSet* |
| Schema |Alleen de volgende kenmerken worden genegeerd: *Namespace* |
| FunctionImport |Alleen de volgende kenmerken worden genegeerd: *modus* (de standaardwaarde van RG wordt gebruikt) |
| EntityType |De volgende subknooppunten worden genegeerd: *sleutel* en *PropertyRef* |

Het volgende is, worden de wijzigingen (toegevoegd en elementen genegeerd) voor de verschillende CSDL XML-knooppunten in detail beschreven.

## <a name="functionimport-node"></a>FunctionImport knooppunt
Een FunctionImport-knooppunt vertegenwoordigt een URL (toegangspunt) waarmee een service aan de eindgebruiker. Het knooppunt kan met een beschrijving van hoe de URL is gericht, welke parameters zijn beschikbaar voor de eindgebruiker en hoe deze parameters worden geleverd.

Meer informatie over dit knooppunt worden gevonden op [hier][MSDNFunctionImportLink](https://msdn.microsoft.com/library/cc716710.aspx)

Hieronder vindt u de extra kenmerken (of toevoegingen aan kenmerken) die door het knooppunt FunctionImport beschikbaar worden gesteld:

**d:BaseUri** -URI van de sjabloon voor de REST-resource die wordt blootgesteld aan de Marketplace. De sjabloon Marketplace gebruikt om een query uitgevoerd op de REST-webservice samen te stellen. De URI-sjabloon bevat de tijdelijke aanduidingen voor de parameters in de vorm van {parameterName}, waarbij parameterName is de naam van de parameter. Bijvoorbeeld apiVersion = {apiVersion}.
Parameters mogen worden weergegeven als de URI-parameters of als onderdeel van de URI-pad. In het geval van de weergave in het pad zijn ze altijd verplicht (kan niet worden gemarkeerd als null-waarden). *Voorbeeld:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Naam** -de naam van de geïmporteerde functie.  Kan niet hetzelfde is als andere gedefinieerde namen in de CSDL.  Bijvoorbeeld Naam = 'GetModelUsageFile'

**EntitySet** *(optioneel)* - als de functie een verzameling van Entiteitstypen, de waarde van retourneert de **EntitySet** moet worden de entiteit ingesteld op waartoe de verzameling behoort. Anders wordt de **EntitySet** kenmerk mag niet worden gebruikt. *Voorbeeld:*`EntitySet="GetUsageStatisticsEntitySet"`

**ReturnType** *(optioneel)* -Hiermee wordt het type van elementen die zijn geretourneerd door de URI.  Gebruik dit kenmerk niet als de functie geen waarde retourneert. Hier volgen de ondersteunde typen:

* **Verzameling (<Entity type name>)**: Hiermee geeft u een verzameling van gedefinieerde Entiteitstypen. De naam is aanwezig in het naamkenmerk van het EntityType-knooppunt. Een voorbeeld is een verzameling (WXC. HourlyResult).
* **Onbewerkte (<mime type>)**: Hiermee geeft u een onbewerkte document/blob die wordt geretourneerd naar de gebruiker. Een voorbeeld is Raw(image/jpeg) andere voorbeelden:

  * ReturnType="Raw(text/plain)'
  * ReturnType = "verzameling (sage. DeleteAllUsageFilesEntity) ' *

**d:paging** -geeft aan hoe paginering wordt verwerkt door de REST-resource. De parameterwaarden die worden gebruikt binnen de accolades braches bijvoorbeeld pagina = {$page} & itemsperpage = {$size} de beschikbare opties zijn:

* **Geen:** geen paginering is beschikbaar
* **Overslaan:** paginering wordt uitgedrukt door middel van een logische 'overslaan' en 'uitvoeren' (boven). Overslaan vroom M-elementen en nemen retourneert vervolgens de volgende N-elementen. Parameterwaarde: $skip
* **Los het:** nemen retourneert de volgende N-elementen. Parameterwaarde: $take
* **PageSize:** paginering wordt gerealiseerd via een logische pagina en de grootte (items per pagina). Pagina vertegenwoordigt de huidige pagina die wordt geretourneerd. Parameterwaarde: $page
* **Grootte:** grootte geeft het aantal items geretourneerd voor elke pagina. Parameterwaarde: $size

**d:AllowedHttpMethods** *(optioneel)* -geeft aan welke term is verwerkt door de REST-resource. Ook beperkt geaccepteerde term met de opgegeven waarde.  Standaardinstelling = POST.  *Voorbeeld:* `d:AllowedHttpMethods="GET"` de beschikbare opties zijn:

* **GET:** meestal gebruikt om gegevens te retourneren
* **POST:** meestal gebruikt voor het invoegen van nieuwe gegevens
* **PUT:** meestal gebruikt om gegevens te werken
* **VERWIJDEREN:** gebruikt om gegevens te verwijderen

Extra onderliggende knooppunten (die niet wordt gedekt door de CSDL-documentatie) in de FunctionImport-knooppunt zijn:

**d:RequestBody** *(optioneel)* -de hoofdtekst van de aanvraag wordt gebruikt om aan te geven dat de aanvraag verwacht met een instantie worden verzonden. Parameters kunnen worden opgegeven in de aanvraagtekst. Ze worden uitgedrukt in de accolades, bijvoorbeeld {parameterName}. Deze parameters worden toegewezen vanuit de invoer van de gebruiker in de hoofdtekst die wordt overgebracht naar de inhoud provider-service. Het element requestBody heeft een kenmerk met naam httpMethod. Het kenmerk kunt u twee waarden:

* **POST:** gebruikt als de aanvraag een HTTP POST is
* **GET:** gebruikt als de aanvraag een HTTP GET is

    Voorbeeld:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** en **d:Namespace** -dit knooppunt wordt de naamruimten die zijn gedefinieerd in de XML-bestand dat wordt geretourneerd door de functie-import (URI eindpunt) beschreven. De XML-bestand dat wordt geretourneerd door de back-endservice bevatten mogelijk een willekeurig aantal naamruimten te onderscheiden van de inhoud die wordt geretourneerd. **Alle deze naamruimten als d:Map of d:Match XPath-query's gebruikt, moeten worden weergegeven.** Het knooppunt d:Namespaces bevat een setlijst van d:Namespace knooppunten. Elk van deze bevat een naamruimte die wordt gebruikt in het antwoord van de back-end-service. Het kenmerk van het knooppunt d:Namespace zijn:

* **d:prefix:** het voorvoegsel voor de naamruimte, zoals in de XML-resultaten geretourneerd door de service, zoals f:FirstName, f:LastName, f waar het voorvoegsel is.
* **d:URI:** de volledige URI van de naamruimte die wordt gebruikt in het document resultaat. Hiermee geeft u de waarde die het voorvoegsel wordt omgezet naar tijdens runtime.

**d:ErrorHandling** *(optioneel)* -dit knooppunt bevat de voorwaarden voor foutafhandeling. Elk van de voorwaarden is gevalideerd met het resultaat dat wordt geretourneerd door de service van de aanbieder van de inhoud. Als een voorwaarde overeenkomt met de voorgestelde HTTP-foutcode wordt een foutbericht wordt geretourneerd naar de eindgebruiker.

**d:ErrorHandling** *(optioneel)* en **d:Condition** *(optioneel)* -een voorwaarde-knooppunt bevat een voorwaarde die het resultaat geretourneerd door de service van de aanbieder van de inhoud is ingecheckt. Hieronder vindt u de **vereist** kenmerken:

* **d:match:** een XPath-expressie die wordt gevalideerd of een bepaalde knooppuntwaarde aanwezig in de inhoudsprovider is XML-uitvoer. De XPath wordt uitgevoerd voor de uitvoer en waar als de voorwaarde een overeenkomst of ONWAAR anders is moet retourneren.
* **d:HttpStatusCode:** de HTTP-statuscode die moet worden geretourneerd door Marketplace in het geval de voorwaarde voldoet aan. Marketplace signalizes fouten aan de gebruiker via HTTP-statuscodes. Een lijst met HTTP-statuscodes zijn beschikbaar op http://en.wikipedia.org/wiki/HTTP_status_code
* **d:errorMessage:** het foutbericht dat wordt geretourneerd: met de HTTP-statuscode: aan de eindgebruiker. Dit moet een beschrijvende foutbericht die geen geen geheimen bevat zijn.

**d:Title** *(optioneel)* -kunt u met een beschrijving van de titel van de functie. De waarde voor de titel afkomstig is van

* Het optionele map-kenmerk (een xpath) dat wordt opgegeven waar zich de titel in het antwoord geretourneerd van de serviceaanvraag.
* - Of - de naam is opgegeven als de waarde van het knooppunt.

**d:Rights** *(optioneel)* -(bijvoorbeeld auteursrecht) van de rechten die zijn gekoppeld aan de functie. De waarde voor de rechten afkomstig is van:

* Het optionele map-kenmerk (een xpath) dat wordt opgegeven waar zich de rechten in het antwoord geretourneerd van de serviceaanvraag.
* - Of - de rechten die is opgegeven als de waarde van het knooppunt.

**d:Description** *(optioneel)* : een korte beschrijving voor de functie. De waarde voor de beschrijving is afkomstig van

* Het optionele map-kenmerk (een xpath) dat wordt opgegeven waar zich de beschrijving in het antwoord geretourneerd van de serviceaanvraag.
* - Of – de beschrijving is opgegeven als de waarde van het knooppunt.

**d:EmitSelfLink** - *bovenstaande voorbeeld 'FunctionImport 'Gewisseld' via geretourneerde gegevens'*

**d:EncodeParameterValue** -optionele uitbreiding voor OData

**d:QueryResourceCost** -optionele uitbreiding voor OData

**d:map** -optionele uitbreiding voor OData

**d:headers** -optionele uitbreiding voor OData

**d:headers** -optionele uitbreiding voor OData

**d:Value** -optionele uitbreiding voor OData

**d:HttpStatusCode** -optionele uitbreiding voor OData

**d:errorMessage** -optionele uitbreiding voor OData

## <a name="parameter-node"></a>De parameterknooppunt
Dit knooppunt vertegenwoordigt een parameter die wordt weergegeven als onderdeel van de URI-sjabloon / aanvraagtekst die is opgegeven in het knooppunt van de FunctionImport.

Een documentpagina zeer nuttige informatie over het ' Parameter ' elementknooppunt bevindt zich in [hier](http://msdn.microsoft.com/library/ee473431.aspx) (Gebruik de **andere versie** dropdown selecteren van een andere versie indien nodig zijn om weer te geven van de documentatie). *Voorbeeld:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| De Parameterkenmerk | Is vereist | Waarde |
| --- | --- | --- |
| Naam |Ja |De naam van de parameter. Hoofdlettergevoelig!  BaseUri hoofdletters. **Voorbeeld:**`<Property Name="IsDormant" Type="Byte" />` |
| Type |Ja |Het parametertype. De waarde moet een **EDMSimpleType** of een complex type die zich binnen het bereik van het model. Zie '6 ondersteunde Parameter of de eigenschap typen' voor meer informatie.  (Hoofdlettergevoelig! Eerste teken is een hoofdletter, rest kleine letters.)  Zie ook, [conceptuele Model typen (CSDL)][MSDNParameterLink](http://msdn.microsoft.com/library/bb399548.aspx). **Voorbeeld:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Modus |Nee |**In**, Out of InOut afhankelijk van of de parameter is een invoer, uitvoer of input/output-parameter. (Alleen 'IN' is beschikbaar in Azure Marketplace.) **Voorbeeld:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| maxLength |Nee |De maximaal toegestane lengte van de parameter. **Voorbeeld:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| precisie |Nee |De precisie van de parameter. **Voorbeeld:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Schalen |Nee |De schaal van de parameter. **Voorbeeld:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

Hier volgen de kenmerken die zijn toegevoegd aan de CSDL-specificatie:

| De Parameterkenmerk | Beschrijving |
| --- | --- |
| **d:Regex** *(optioneel)* |Een regex-instructie voor het valideren van de invoerwaarde voor de parameter. De waarde wordt geweigerd als de ingevoerde waarde komt niet overeen met de instructie. Hiermee geeft u ook een set van waarden in, bijvoorbeeld ^ [0-9] +? $ toe te staan dat cijfers. **Voorbeeld:** ' < parameternaam = "name" modus = "In" Type = 'Tekenreeks' d: null-waarden bevatten = 'false' d:Regex = ' ^ [a-zA-Z] * $' d:Description = "een naam die kan geen spaties of niet-alfanumerieke niet-Engelse tekens bevatten' d:SampleValues ="George |
| **d:Enum** *(optioneel)* |Een pipe gescheiden lijst met waarden die geldig zijn voor de parameter. Het type van de waarden moet overeenkomen met het gedefinieerde type van de parameter. Voorbeeld: ' Engels |
| **d: null-waarden bevatten** *(optioneel)* |Hiermee kunt u definiëren of een parameter kan niet null zijn. De standaardwaarde is: true. Parameters die beschikbaar worden gesteld als onderdeel van het pad in de URI-sjabloon kunnen echter niet null. Wanneer het kenmerk is ingesteld op false voor deze parameters: de invoer van de gebruiker wordt overschreven. **Voorbeeld:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(optioneel)* |Een voorbeeldwaarde een opmerking voor de Client in de gebruikersinterface wordt weergegeven.  Het is mogelijk meerdere waarden toevoegen via een pipe gescheiden lijst, dat wil zeggen, een |

## <a name="entitytype-node"></a>EntityType knooppunt
Dit knooppunt vertegenwoordigt een van de typen die worden geretourneerd van Marketplace voor de eindgebruiker. Het bevat ook de toewijzing van de uitvoer die wordt geretourneerd door de inhoud provider-service op de waarden die worden geretourneerd aan de eindgebruiker.

Meer informatie over dit knooppunt worden gevonden op [hier](http://msdn.microsoft.com/library/bb399206.aspx) (Gebruik de **andere versie** dropdown selecteren van een andere versie indien nodig om de documentatie weer te geven.)

| Naam van kenmerk | Is vereist | Waarde |
| --- | --- | --- |
| Naam |Ja |De naam van het entiteitstype. **Voorbeeld:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType |Nee |De naam van een andere entiteitstype dat is het basistype van het entiteitstype die wordt gedefinieerd. **Voorbeeld:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Hier volgen de kenmerken die zijn toegevoegd aan de CSDL-specificatie:

**d:map** -uitgevoerd met de service-uitvoer van een XPath-expressie. De veronderstelling Hier wordt de uitvoer van de service bevat een set-elementen die herhalen, zoals een ATOM-feed waar er is een set van vermelding knooppunten die worden herhaald. Elk van deze herhalende knooppunten bevat één record. De XPath is vervolgens verwijzen de afzonderlijke herhalende knooppunten in de inhoudsprovider service resultaat waarin de waarden voor een afzonderlijke record opgegeven. Voorbeeld van uitvoer van de service:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

De XPath-expressie zou worden /foo/balk omdat elk van de balk knooppunt is het herhalende knooppunt in de uitvoer en bevat de werkelijke inhoud die wordt geretourneerd naar de eindgebruiker.

**Sleutel** -dit kenmerk wordt genegeerd door de Marketplace. REST op basis van webservices, niet beschikbaar in het algemeen een primaire sleutel.

## <a name="property-node"></a>Eigenschapsknooppunt
Dit knooppunt bevat een eigenschap van de record.

Meer informatie over dit knooppunt worden gevonden op [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (Gebruik de **andere versie** dropdown selecteren van een andere versie indien nodig om de documentatie weer te geven.) *Voorbeeld:*`<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"     Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Vereist | Waarde |
| --- | --- | --- |
| Naam |Ja |De naam van de eigenschap. |
| Type |Ja |Het type van de waarde van eigenschap. Het type moet een **EDMSimpleType** of een complex type (aangeduid door een volledig gekwalificeerde naam) die zich binnen het bereik van het model. Zie conceptuele Model-typen (CSDL) voor meer informatie. |
| Null-waarden bevatten |Nee |**De waarde True** (de standaardwaarde) of **False** afhankelijk van of de eigenschap een null-waarde kan hebben. Opmerking: In de versie van CSDL aangegeven door de [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) naamruimte, moet een complex type-eigenschap hebben die null-waarden bevatten = 'False'. |
| Standaardwaarde |Nee |De standaardwaarde van de eigenschap. |
| maxLength |Nee |De maximale lengte van de eigenschapswaarde. |
| FixedLength |Nee |**De waarde True** of **False** afhankelijk van of de waarde van eigenschap met een tekenreekslengte fiexed wordt opgeslagen. |
| precisie |Nee |Verwijst naar het maximum aantal cijfers in de numerieke waarde die moet worden bewaard. |
| Schalen |Nee |Maximum aantal decimalen in de numerieke waarde die moet worden bewaard. |
| Unicode |Nee |**De waarde True** of **False** afhankelijk van of de waarde van eigenschap zijn opgeslagen als een Unicode-tekenreeks. |
| Sortering |Nee |Een tekenreeks waarin de sorteervolgorde moet worden gebruikt in de gegevensbron. |
| ConcurrencyMode |Nee |**Geen** (de standaardwaarde) of **vast**. Als de waarde is ingesteld op **vast**, waarde van de eigenschap wordt gebruikt in optimistische gelijktijdigheid controles. |

Hier volgen de extra kenmerken die zijn toegevoegd aan de CSDL-specificatie:

**d:map** -XPath-expressie uitgevoerd met de service de uitvoer en extraheert een eigenschap van de uitvoer. Het opgegeven XPath is ten opzichte van het herhalende knooppunt dat is geselecteerd in het EntityType-knooppunt XPath. Het is ook mogelijk om op te geven van een absolute XPath om toe te staan, met inbegrip van een statische resource in elk van de uitvoerknooppunten, zoals bijvoorbeeld copyrightinformatie die slechts één keer is gevonden in de oorspronkelijke service uitvoer maar moet aanwezig zijn in elk van de rijen in de OData-uitvoer. Voorbeeld van de service:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

De XPath-expressie zijn ./bar/baz0 het knooppunt baz0 ophalen uit de inhoud provider-service.

**d:CharMaxLength** -voor het type string, kunt u de maximale lengte. Zie DataService CSDL-voorbeeld

**d:IsPrimaryKey** -geeft aan of de kolom de primaire sleutel in de tabel of weergave is. Zie DataService CSDL-voorbeeld.

**d:isExposed** -bepaalt als het tabelschema is beschikbaar gemaakt (meestal true). Zie DataService CSDL-voorbeeld

**d:IsView** *(optioneel)* : true als dit is gebaseerd op een weergave in plaats van een tabel.  Zie DataService CSDL-voorbeeld

**d:Tableschema** -Zie DataService CSDL-voorbeeld

**d:columnName** -de naam van de kolom in de tabel of weergave.  Zie DataService CSDL-voorbeeld

**d:IsReturned** -Is de Booleaanse waarde die bepaalt of de Service deze waarde naar de client wordt.  Zie DataService CSDL-voorbeeld

**d:IsQueryable** -de Booleaanse waarde die bepaalt of de kolom kan worden gebruikt in een databasequery Is.   Zie DataService CSDL-voorbeeld

**d:OrdinalPosition** -positie van de kolom numerieke van uiterlijk, x, in de tabel of de weergave, waarbij x van 1 aan het aantal kolommen in de tabel staat Is.  Zie DataService CSDL-voorbeeld

**d:DatabaseDataType** -Is het gegevenstype van de kolom in de database, dat wil zeggen SQL-gegevenstype. Zie DataService CSDL-voorbeeld

## <a name="supported-parametersproperty-types"></a>Ondersteunde Parameters of de eigenschap typen
Hier volgen de ondersteunde typen voor parameters en eigenschappen. (Hoofdlettergevoelig)

| Primitieve typen | Beschrijving |
| --- | --- |
| Null |Vertegenwoordigt het ontbreken van een waarde |
| Booleaanse waarde |Hiermee geeft u het wiskundige concept van logic binaire waarden |
| Byte |Niet-ondertekende 8-bits geheel getal |
| Datum/tijd |Datum en tijd vertegenwoordigt met waarden die variëren van 12:00:00 middernacht, 1 januari 1753 A.D. tot en met 11:59:59 uur, December 9999 A.D. |
| Decimale |Numerieke waarden met vaste precision en scale vertegenwoordigt. Dit type wordt beschreven en een numerieke waarde variërend van het negatieve 10 ^ 255 + 1 tot en met 10 positief ^ 255 -1 |
| dubbele |Hiermee geeft u een getal met 15 cijfers precisie waarbij waarden met een benadering bereik van + 2.23E-308 via + 1.79E drijvende komma +308. **Gebruik decimale vanwege een probleem van Exel exporteren** |
| Één |Hiermee geeft u een getal met 7 cijfers precisie waarbij waarden met een benadering bereik van + 1.18E-38 3.40E + tot en met drijvende komma +38 |
| GUID |Hiermee geeft u een waarde van de unieke id van 16 bytes (128-bits) |
| Int16 |Hiermee geeft u een ondertekende 16-bits geheel getal |
| Int32 |Hiermee geeft u een ondertekende 32-bits geheel getal |
| Int64 |Hiermee geeft u een ondertekende 64-bits geheel getal |
| Tekenreeks |Hiermee geeft u of variabele-tekengegevens met vaste lengte |

## <a name="see-also"></a>Zie ook
* Als u geïnteresseerd bent in het algehele proces voor OData-toewijzing en het doel te begrijpen, Lees dit artikel [gegevens Service OData-toewijzing](marketplace-publishing-data-service-creation-odata-mapping.md) bekijken definities, structuren en instructies.
* Als u geïnteresseerd bent in de voorbeelden controleren, Lees dit artikel [gegevens Service OData toewijzing voorbeelden](marketplace-publishing-data-service-creation-odata-mapping-examples.md) voorbeeldcode zien en begrijpen codesyntaxis en in het contextmenu.
* Lees dit artikel om terug te keren naar de voorgeschreven pad voor het publiceren van een Service van gegevens naar Azure Marketplace, [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).
