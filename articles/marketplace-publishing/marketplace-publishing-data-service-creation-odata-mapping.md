---
title: Handleiding voor het maken van een Service van gegevens voor de Marketplace | Microsoft Docs
description: Gedetailleerde instructies voor het maken, certificeren en implementeren van een Service voor gegevens aanschaffen op Azure Marketplace.
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3a632825-db5b-49ec-98bd-887138798bc4
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: a853b4dbd1952ba4ea8ee68ea3ca98f588bb71a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Toewijzing van een bestaande webservice aan OData via CSDL
> [!IMPORTANT]
> **Op dit moment wordt niet langer zijn voorbereiden op alle nieuwe gegevensservice uitgevers. Nieuwe dataservices wordt niet goedgekeurd voor de aanbieding.** Als u een SaaS business-toepassing die u wilt publiceren op AppSource hebt u meer informatie vindt [hier](https://appsource.microsoft.com/partners). Als u beschikt over een IaaS-toepassingen of developer-service die u wilt publiceren op Azure Marketplace u meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

In dit artikel biedt een overzicht over het gebruik van een CSDL een bestaande service toewijzen aan een compatibele OData-service. Hierin wordt uitgelegd hoe de toewijzing-document (CSDL) maken die de invoer-aanvraag van de client via een aanroep van een transformeert en de uitvoer (gegevens) terug naar de client via een compatibel OData-feed. Services voor de eindgebruikers beschrijft van Microsoft Azure Marketplace met behulp van het OData-protocol. Services die beschikbaar worden gesteld door inhoudsproviders (eigenaren) beschikbaar worden gesteld in verschillende vormen, zoals REST, SOAP, enz.

## <a name="what-is-a-csdl-and-its-structure"></a>Wat is een CSDL en de structuur?
Een CSDL (conceptuele Schema Definition Language) is een specificatie bepalen op welke manier om te beschrijven, webservice of database-service in het algemene XML-bewoordingen naar Azure Marketplace.

Eenvoudig overzicht van de **stroom aanvragen:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

De **gegevensstroom** is in de tegengestelde richting:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Afbeelding 1** diagrammen hoe een client zou ophalen via een inhoudsprovider (uw service) door te gaan via de Azure Marketplace.  De CSDL door het onderdeel toewijzing/transformatie wordt gebruikt om de aanvraag te verwerken en gegevens uitwisselen tussen de inhoudsprovider service (s) en de aanvragende client.

*Afbeelding 1: Gedetailleerde stroom van de aanvragende client naar de provider van inhoud via Azure Marketplace*

  ![tekenen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Voor achtergrondinformatie over Atom Atom Pub en het OData-protocol waarop de Azure Marketplace-extensies bouwen, Controleer: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Fragment boven het koppeling: *"het doel van de Open Data protocol (hierna OData) is een protocol op basis van REST CRUD-stijl-bewerkingen (maken, lezen, bijwerken en verwijderen) op basis van bronnen die worden weergegeven als dataservices van bieden. Een service"gegevens" is een eindpunt wanneer blootgesteld aan een of meer 'verzamelingen' elke met nul of meer 'posten', die bestaan uit gegevens hebt ingevoerd met de naam / waarde-paren. OData is gepubliceerd door Microsoft onder OASIS (organisatie voor de vooruitgang van gestructureerde gegevens normen) standaarden zodat iedereen die wil servers, clients of hulpprogramma's zonder royalties of beperkingen kunt samenstellen."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Er zijn drie essentiële gegevens die moeten worden gedefinieerd door de CSDL:
* De **eindpunt** van de Service Provider The Web adres (URI) van de Service
* De **parameters voor** wordt als invoer doorgegeven aan de serviceprovider de definities van de parameters die worden verzonden naar service omlaag naar het gegevenstype van de aanbieder van de inhoud.
* **Schema** van de gegevens worden geretourneerd naar de Service aanvragen van het schema van de gegevens die wordt geleverd door de service van de aanbieder van de inhoud, met inbegrip van Container, verzamelingen/tabellen, variabelen/kolommen en gegevenstypen.

Het volgende diagram toont een overzicht van de stroom van waar de client voert de OData-instructie (aanroep van de inhoud provider-webservice) is aan de resultaatgegevens om terug te zetten.

  ![tekenen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>stappen:
1. Client stuurt via een aanroep van aanvraag voltooid met de invoerparameters in XML-indeling naar Azure Marketplace
2. CSDL wordt gebruikt voor het valideren van de aanroep van de Service.
   * De indeling Service gesprek wordt verzonden naar de inhoud Providers-Service door Azure Marketplace
3. De Webservice wordt uitgevoerd en de actie van de Http-term tekstknooppunten wordt uitgevoerd (dat wil zeggen ophalen) de gegevens worden geretourneerd naar Azure Marketplace de aangevraagde gegevens (indien aanwezig) is zichtbaar gemaakt in XML-indeling naar de Client met behulp van de toewijzing is gedefinieerd in de CSDL.
4. De Client wordt verzonden de gegevens (indien aanwezig) in XML- of JSON-indeling

## <a name="definitions"></a>Definities
### <a name="odata-atom-pub"></a>OData-ATOM pub
Een uitbreiding van het ATOM-pub waarbij elk item staat voor één rij met een resultaat ingesteld. Het inhoud deel van de vermelding is verbeterd zodat de waarden van de rij-als sleutel-waardeparen bevat. Meer informatie is hier vinden: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - Conceptual Schema Definition Language
Hiermee kunt u definiëren functies (SPROCs) en de entiteiten die worden weergegeven via een database. Meer informatie hier vinden: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [!TIP]
> Klik op de **andere versies** vervolgkeuzelijst en selecteer een versie als u het artikel niet ziet.
> 
> 

### <a name="edm---entry-data-model"></a>EDM - gegevensmodel vermelding
* Overzicht: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* Voorbeeld: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* Gegevenstypen: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

De volgende ziet u de gedetailleerde links naar rechts stromen van waar de client voert de OData-instructie (aanroep van de inhoud provider-webservice) voor het ophalen van de resultaatgegevens back:

  ![tekenen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a>CSDL-basisbeginselen
Een CSDL (conceptuele Schema Definition Language) is een specificatie bepalen op welke manier om te beschrijven, webservice of database-service in het algemene XML-bewoordingen naar Azure Marketplace. CSDL beschrijft de kritieke stuks die **het doorgeven van gegevens uit de gegevensbron naar de Azure Marketplace mogelijk maakt.** De belangrijkste onderdelen worden hier beschreven:

* Informatie over de interface met een beschrijving van alle openbaar beschikbare functies (FunctionImport knooppunt)
* Informatie over gegevenstypen voor alle bericht requests(input) en bericht responses(outputs) (EntitySet-EntityContainer/EntityType knooppunten)
* Het binden van informatie over het transportprotocol worden gebruikt (Header knooppunt)
* Adresgegevens voor het zoeken van de opgegeven service (BaseURI kenmerk)

Kortom, vertegenwoordigt de CSDL een platform en taalonafhankelijke overeenkomst tussen de aanvrager service en de serviceprovider. Met behulp van de CSDL een client een webservice service/database vinden en een van de openbaar beschikbare functies aanroepen.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Een CSDL die betrekking hebben op een Database of een verzameling
**De CSDL-specificatie**

CSDL is XML-grammatica voor het beschrijven van een webservice. De specificatie zelf is onderverdeeld in 4 belangrijke elementen: EntitySet, FunctionImport; Naamruimte en EntityType.

Deze abstractie gemakkelijker te begrijpen kunt relatie tussen een CSDL en een tabel.

Houd er rekening mee;

  CSDL vertegenwoordigt een platform en taalonafhankelijke overeenkomst tussen de **service aanvrager** en de **serviceprovider**. CSDL, met een **client** kunnen vinden een **servicedatabase/webservice** en aanroepen van de openbaar **functies.**

Voor een Data-Service kunnen de vier onderdelen van een CSDL in termen van een Database-, tabel-, kolom- of Store Procedure worden beschouwd.

Die betrekking hebben op deze als volgt voor een Service van gegevens:

* EntityContainer ~ = Database
* EntitySet ~ = tabel
* EntityType ~ kolommen =
* FunctionImport ~ opgeslagen Procedure =

**HTTP-termen die zijn toegestaan**

* – Retourneert waarden ophalen uit de database (retourneert een verzameling)
* POST – gebruikt voor het doorgeven van gegevens naar en optionele retourwaarden uit de database (een nieuwe vermelding in de verzameling, return-id/URI maken)
* Hiermee verwijdert u gegevens uit de database (een verzameling verwijdert) – verwijderen
* PUT – gegevens bijwerken in een database (vervangen door een verzameling of maak een)

## <a name="metadatamapping-document"></a>Metagegevens/toewijzing Document
Het document metagegevens/toewijzing wordt gebruikt voor het toewijzen van een inhoudsprovider bestaande web-services zodat deze door het systeem Azure Marketplace kan worden weergegeven als een OData-webservice. Deze is gebaseerd op CSDL en implementeert die toegankelijk is via Azure Marketplace-webservices op basis van een paar uitbreidingen voor CSDL om aan de behoeften van REST te. De extensies zijn gevonden in de [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) naamruimte.

Hier volgt een voorbeeld van de CSDL: (kopiëren en plakken het onderstaande voorbeeld CSDL in een XML-editor en wijzigen die overeenkomen met uw Service.  Plak in CSDL toewijzing DataService tabblad bij het maken van uw service in de [Azure Marketplace Publishing Portal](https://publish.windowsazure.com)).

**Voorwaarden:** termen met betrekking de CSDL naar de [Publishing Portal](https://publish.windowsazure.com) voorwaarden van de gebruikersinterface (PPUI).

* "Title" in de PPUI is gekoppeld aan MyWebOffer bieden
* Mijnbedrijf in de PPUI is gekoppeld aan **Publisher weergavenaam** in de [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) gebruikersinterface
* Uw API is gekoppeld aan een webpagina of Service (een Plan in de PPUI) van de gegevens

**Hiërarchie:** (inhoudsprovider) van een bedrijf eigenaar is van de aanbiedingen die Plan(s) hebben, namelijk service(s), welke uitgelijnd met een API.

### <a name="webservice-csdl-example"></a>WebService CSDL-voorbeeld
Maakt verbinding met een service die een eindpunt van de web-toepassing (zoals een C#-toepassing weergeeft)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID"    d:IsPrimaryKey="True" Type="Int32"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount"    Type="Double"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"    Type="String"    Nullable="false" d:Map="./City"/>
        <Property Name="State"    Type="String"    Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime"    Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [!TIP]
> Meer voorbeelden van de webservice CSDL weergeven in het artikel [voorbeelden voor het toewijzen van een bestaande webservice voor OData via CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)
> 
> 

### <a name="dataservice-csdl-example"></a>DataService CSDL-voorbeeld
Maakt verbinding met een service die weergeeft een databasetabel of weergave als een eindpunt onderstaand voorbeeld ziet u dat twee API's voor base-gegevens op basis van API-CSDL (met kunt weergaven in plaats van tabellen).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Zie ook
* Als u geïnteresseerd bent in het leren van en inzicht in de specifieke knooppunten en de bijbehorende parameters, Lees dit artikel [Service OData toewijzing gegevensknooppunten](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) voor definities en uitleg, voorbeelden en gebruik case-context.
* Als u geïnteresseerd bent in de voorbeelden controleren, Lees dit artikel [gegevens Service OData toewijzing voorbeelden](marketplace-publishing-data-service-creation-odata-mapping-examples.md) voorbeeldcode zien en begrijpen codesyntaxis en in het contextmenu.
* Lees dit artikel om terug te keren naar de voorgeschreven pad voor het publiceren van een Service van gegevens naar Azure Marketplace, [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).

