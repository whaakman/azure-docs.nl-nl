---
title: 'Voorbeeld: Aan de slag - Knowledge Exploration Service-API'
titlesuffix: Azure Cognitive Services
description: Gebruik de Knowledge Exploration Service (KES) API om een engine te maken voor een interactieve zoekopdracht in academische publicaties.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: sample
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 815147abba444f0a55a8455c0a818aa048271b92
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56309638"
---
# <a name="get-started-with-the-knowledge-exploration-service"></a>Aan de slag met de Knowledge Exploration Service

In dit voorbeeld gebruikt u de Knowledge Exploration Service (KES) om een engine te maken voor een interactieve zoekopdracht in academische publicaties. U kunt het opdrachtregelprogramma installeren [ `kes.exe` ](CommandLine.md), en alle voorbeeldbestanden van de [Knowledge Exploration Service SDK](https://www.microsoft.com/en-us/download/details.aspx?id=51488).

Het voorbeeld academische publicaties bevat een set van 1000 academische publicaties gepubliceerd door Microsoft-onderzoekers.  Elk document is gekoppeld aan een titel, publicatie jaar auteurs en sleutelwoorden. Elke auteur wordt voorgesteld door een ID, de naam en de relatie op het moment van publicatie. Elk sleutelwoord kan worden gekoppeld aan een set van synoniemen (bijvoorbeeld het sleutelwoord “support vector machine” kan worden gekoppeld aan het synoniem 'svm').

## <a name="define-the-schema"></a>Het schema definiëren

Het schema beschrijft de kenmerk-structuur van de objecten in het domein. Het geeft de naam en het gegevenstype voor elk kenmerk in een JSON-bestand-indeling. Het volgende voorbeeld is de inhoud van het bestand *Academic.schema*.

```json
{
  "attributes":[
    {"name":"Title", "type":"String"},
    {"name":"Year", "type":"Int32"},
    {"name":"Author", "type":"Composite"},
    {"name":"Author.Id", "type":"Int64", "operations":["equals"]},
    {"name":"Author.Name", "type":"String"},
    {"name":"Author.Affiliation", "type":"String"},
    {"name":"Keyword", "type":"String", "synonyms":"Keyword.syn"}
  ]
}
```

Hier kunt u *titel*, *jaar*, en *sleutelwoord* definiëren als een tekenreeks, geheel getal en een tekenreeks van het kenmerk, respectievelijk. Omdat auteurs worden vertegenwoordigd door id, naam en relatie, definieert u *Auteur* als een samengesteld kenmerk met drie onderliggende kenmerken: *Author.Id*, *Author.Name* en *Author.Affiliation*.

Standaard ondersteunen kenmerken alle bewerkingen voor het gegevenstype, met inbegrip van *is gelijk aan*, *starts_with*, en *is_between*. Omdat Auteur-ID alleen intern als id gebruikt wordt, de standaardwaarde overschrijven en geef *is gelijk aan* als de enige geïndexeerde bewerking.

Voor het *sleutelwoord*-kenmerk staat u synoniemen die overeenkomen met canonieke sleutelwoordwaarden zoals gespecificeerd in het synoniemenbestand *Keyword.syn* in de kenmerkdefinitie. Dit bestand bevat een lijst van canonieke en synonieme waarde-paren:

```json
...
["support vector machine","support vector machines"]
["support vector machine","support vector networks"]
["support vector machine","support vector regression"]
["support vector machine","support vector"]
["support vector machine","svm machine learning"]
["support vector machine","svm"]
["support vector machine","svms"]
["support vector machine","vector machine"]
...
```

Zie voor meer informatie over de schemadefinitie [Schema-indeling](SchemaFormat.md).

## <a name="generate-data"></a>Gegevens genereren

Het gegevensbestand beschrijft de lijst van de publicaties om te indexeren, met op elke regel de kenmerkwaarden van een publicatie in [JSON-indeling](http://json.org/).  Het volgende voorbeeld is een enkele regel uit het gegevensbestand *Academic.data*opgemaakt voor de leesbaarheid:

```
...
{
    "logprob": -16.714,
    "Title": "the world wide telescope",
    "Year": 2001,
    "Author": [
        {
            "Id": 717694024,
            "Name": "alexander s szalay",
            "Affiliation": "microsoft"
        },
        {
            "Id": 2131537204,
            "Name": "jim gray",
            "Affiliation": "microsoft"
        }
    ]
}
...
```

In dit fragment, geeft u het *titel* en *jaar*-kenmerk van de publicatie als een JSON-tekenreeks en een nummer, respectievelijk. Meerdere waarden worden weergegeven met behulp van JSON-matrices. Omdat *Auteur* een samengestelde kenmerk is, wordt elke waarde weergegeven met behulp van een JSON-object dat bestaat uit de onderliggende kenmerken. Kenmerken met ontbrekende waarden, zoals *sleutelwoord* in dit geval kunnen worden uitgesloten van de JSON-weergave.

Om de kans op verschillende publicaties te onderscheiden, geeft u de relatieve log-kans op met behulp van het ingebouwde *Logprob* kenmerk. Met een kans *p* tussen 0 en 1, berekent u de logkans als log(*p*), waarbij log() staat voor de natuurlijke log-functie.

Ga voor meer informatie naar [Gegevensindeling](DataFormat.md).

## <a name="build-a-compressed-binary-index"></a>Bouw een gecomprimeerde binaire-index

Nadat u een schemabestand en het gegevensbestand hebt, kunt u een gecomprimeerde binaire index van de gegevensobjecten maken met behulp van [ `kes.exe build_index` ](CommandLine.md#build_index-command). In dit voorbeeld bouwt u het indexbestand *Academic.index* uit het bestand invoerschema *Academic.schema* en gegevensbestand *Academic.data*. Gebruik de volgende opdracht:

`kes.exe build_index Academic.schema Academic.data Academic.index`

Voor snelle ontwikkeling van prototypen buiten Azure, kan [ `kes.exe build_index` ](CommandLine.md#build_index-command) kleine indexen lokaal bouwen van gegevensbestanden met maximaal 10.000 objecten. Voor grotere bestanden, kunt u ofwel de opdracht uitvoeren vanuit een [Windows VM in Azure](../../../articles/virtual-machines/windows/quick-create-portal.md) of een externe build uitvoeren in Azure. Ga voor meer informatie naar Omhoog schalen.

## <a name="use-an-xml-grammar-specification"></a>Gebruik een XML-grammatica-specificatie

De grammatica geeft de set van query's in natuurlijke taal die de service interpreteren kan en hoe deze query's in natuurlijke taal worden vertaald in functionaliteit voor semantische query-expressies. In dit voorbeeld gebruikt u de grammatica die is opgegeven in *academic.xml*:

```xml
<grammar root="GetPapers">

  <!-- Import academic data schema-->
  <import schema="Academic.schema" name="academic"/>

  <!-- Define root rule-->
  <rule id="GetPapers">
    <example>papers about machine learning by michael jordan</example>

    papers
    <tag>
      yearOnce = false;
      isBeyondEndOfQuery = false;
      query = All();
    </tag>

    <item repeat="1-" repeat-logprob="-10">
      <!-- Do not complete additional attributes beyond end of query -->
      <tag>AssertEquals(isBeyondEndOfQuery, false);</tag>

      <one-of>
        <!-- about <keyword> -->
        <item logprob="-0.5">
          about <attrref uri="academic#Keyword" name="keyword"/>
          <tag>query = And(query, keyword);</tag>
        </item>

        <!-- by <authorName> [while at <authorAffiliation>] -->
        <item logprob="-1">
          by <attrref uri="academic#Author.Name" name="authorName"/>
          <tag>authorQuery = authorName;</tag>
          <item repeat="0-1" repeat-logprob="-1.5">
            while at <attrref uri="academic#Author.Affiliation" name="authorAffiliation"/>
            <tag>authorQuery = And(authorQuery, authorAffiliation);</tag>
          </item>
          <tag>
            authorQuery = Composite(authorQuery);
            query = And(query, authorQuery);
          </tag>
        </item>

        <!-- written (in|before|after) <year> -->
        <item logprob="-1.5">
          <!-- Allow this grammar path to be traversed only once -->
          <tag>
            AssertEquals(yearOnce, false);
            yearOnce = true;
          </tag>
          <ruleref uri="#GetPaperYear" name="year"/>
          <tag>query = And(query, year);</tag>
        </item>
      </one-of>

      <!-- Determine if current parse position is beyond end of query -->
      <tag>isBeyondEndOfQuery = GetVariable("IsBeyondEndOfQuery", "system");</tag>
    </item>
    <tag>out = query;</tag>
  </rule>

  <rule id="GetPaperYear">
    <tag>year = All();</tag>
    written
    <one-of>
      <item>
        in <attrref uri="academic#Year" name="year"/>
      </item>
      <item>
        before
        <one-of>
          <item>[year]</item>
          <item>
            <attrref uri="academic#Year" op="lt" name="year"/>
          </item>
        </one-of>
      </item>
      <item>
        after
        <one-of>
          <item>[year]</item>
          <item>
            <attrref uri="academic#Year" op="gt" name="year"/>
          </item>
        </one-of>
      </item>
    </one-of>
    <tag>out = year;</tag>
  </rule>
</grammar>
```

Ga voor meer informatie over de syntaxis van de grammatica-specificatie naar [Grammatica-indeling](GrammarFormat.md).

## <a name="compile-the-grammar"></a>Compileer de grammatica

Nadat u een XML-grammatica-specificatie hebt, kunt u deze in een binaire grammatica compileren met behulp van [ `kes.exe build_grammar` ](CommandLine.md#build_grammar-command). Houd er rekening mee dat als de grammatica als een schema is geïmporteerd, het schema-bestand zich moet bevinden in hetzelfde pad als de grammatica-XML. In dit voorbeeld bouwt u het binaire grammaticabestand *Academic.grammar* van het XML-grammatica invoerbestand *Academic.xml*. Gebruik de volgende opdracht:

`kes.exe build_grammar Academic.xml Academic.grammar`

## <a name="host-the-grammar-and-index-in-a-web-service"></a>Host grammatica en de index in een webservice

Voor snelle ontwikkeling van prototypen kunt u de grammatica en de index in een webservice op de lokale computer hosten met behulp van [ `kes.exe host_service` ](CommandLine.md#host_service-command). U hebt vervolgens toegang tot de service via [web-API's](WebAPI.md) voor het valideren van de gegevens en het grammatica-ontwerp. In dit voorbeeld gaat u het grammaticabestand *Academic.grammar* en het indexbestand *Academic.index* hosten op http://localhost:8000/. Gebruik de volgende opdracht:

`kes.exe host_service Academic.grammar Academic.index --port 8000`

Hiermee initieert u een lokaal exemplaar van de webservice. U kunt de service interactief testen door naar de pagina `http::localhost:<port>` te gaan vanuit een browser. Ga voor meer informatie naar Service testen.

U kunt ook direct verschillende [web-API's](WebAPI.md) aanroepen om de interpretatie van natuurlijke taal, de query-voltooiing, de gestructureerde query-evaluatie en de histogramberekening te testen. Als u wilt stoppen met de service, voer dan 'afsluiten' in bij de `kes.exe host_service` opdrachtprompt of druk op Ctrl+C. Hier volgen enkele voorbeelden:

* [http://localhost:8000/interpret?query=papers door susan t dumais](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20dumais)
* [http://localhost:8000/interpret?query=papers door susan t d&voltooid=1](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20d&complete=1)
* [http://localhost:8000/evaluate?expr=Composite(Author.Name=='susan t dumais')& kenmerken=Titel,Jaar,Author.Name,Author.Id&count=2](http://localhost:8000/evaluate?expr=Composite%28Author.Name==%27susan%20t%20dumais%27%29&attributes=Title,Year,Author.Name,Author.Id&count=2)
* [http://localhost:8000/calchistogram?expr=And(Composite(Author.Name=='susan t dumais'),jaar>= 2013)&kenmerken=Jaar,Sleutelwoord&count=4](http://localhost:8000/calchistogram?expr=And%28Composite%28Author.Name=='susan%20t%20dumais'%29,Year>=2013%29&attributes=Year,Keyword&count=4)

Buiten Azure is [ `kes.exe host_service` ](CommandLine.md#host_service-command) beperkt tot indexen van maximaal 10.000 objecten. Andere beperkingen zijn een API-frequentie van 10 aanvragen per seconde en een totaal van 1000 aanvragen voordat het proces wordt automatisch beëindigd. Als u deze beperkingen wilt omzeilen, kunt u de opdracht uitvoeren vanuit een [Windows VM in Azure](../../../articles/virtual-machines/windows/quick-create-portal.md), of de Azure cloudservice implementeren met behulp van de [ `kes.exe deploy_service` ](CommandLine.md#deploy_service-command) opdracht. Ga voor meer informatie naar Service implementeren.

## <a name="scale-up-to-host-larger-indices"></a>Opschalen om grotere indexen te hosten

Wanneer u `kes.exe` uitvoert buiten Azure is de index beperkt tot 10.000 objecten. U kunt grotere indexen bouwen en hosten met behulp van Azure. Meld u aan voor een gratis [proefversie](https://azure.microsoft.com/pricing/free-trial/). Als u bent geabonneerd op Visual Studio of MSDN, kunt u ook de [Abonneevoordelen activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Dit biedt sommige maandelijkse Azure-credits.

Om `kes.exe` toegang te geven tot een Azure-account moet u het [Azure Publish Settings-bestand](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) downloaden van de Azure Portal. Meld u aan met uw gewenste Azure-account als u daarom wordt gevraagd. Sla het bestand op als *AzurePublishSettings.xml* in de werkmap waaruit `kes.exe` wordt uitgevoerd.

Er zijn twee manieren voor het bouwen en hosten van grote indexen. De eerste is het voorbereiden van de gegevensbestanden en het schema in een Windows-VM in Azure. Voer daarna `kes.exe build_index` uit voor het bouwen van de lokale index op de virtuele machine, zonder groottebeperkingen. De resulterende index kan lokaal worden gehost op de virtuele machine met behulp van `kes.exe host_service` voor snelle ontwikkeling van prototypen, opnieuw zonder beperkingen. Ga voor gedetailleerde stappen naar [Zelfstudie voor Azure-VM](../../../articles/virtual-machines/windows/quick-create-portal.md).

De tweede methode is het uitvoeren van een externe Azure-build met behulp van [ `kes.exe build_index` ](CommandLine.md#build_index-command) met de `--remote` parameter. Hiermee geeft u een Azure VM-grootte aan. Wanneer de parameter `--remote` is opgegeven, maakt de opdracht een tijdelijke Azure-VM van die grootte. Vervolgens wordt index op de virtuele machine gebouwd, wordt de index geüpload naar de Blob-opslag en wordt na voltooiing de VM weer verwijderd. Uw Azure-abonnement wordt belast voor de kosten van de virtuele machine terwijl de index wordt opgebouwd.

Met de mogelijkheid voor een externe Azure-build kan [ `kes.exe build_index` ](CommandLine.md#build_index-command) worden uitgevoerd in elke omgeving. Wanneer u een externe build uitvoert, mogen de invoerargumenten voor schema en gegevens lokale paden of [Azure blob-opslag](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) URL's zijn. Het argument van de indexuitvoer moet de URL van een blob-opslag zijn. Ga voor informatie over het aanmaken van een Azure-opslagaccount naar [Over Azure-opslagaccounts](../../storage/common/storage-create-storage-account.md). Om bestanden efficiënt van en naar een Blob-opslag te kopiëren, gebruikt u het [AzCopy](../../storage/common/storage-use-azcopy.md) hulpprogramma.

In dit voorbeeld kunt u ervan uitgaan dat de volgende Blob-opslagcontainer al is gemaakt: http://&lt;*account*&gt;.blob.core.windows.net/&lt;*container* &gt;/. Het bevat het schema *Academic.schema*, het synoniemenbestand waarnaar wordt verwezen *Keywords.syn* en het volledige gegevensbestand *Academic.full.data*. U kunt de volledige index op afstand bouwen met behulp van de volgende opdracht:

`kes.exe build_index http://<account>.blob.core.windows.net/<container>/Academic.schema http://<account>.blob.core.windows.net/<container>/Academic.full.data http://<account>.blob.core.windows.net/<container>/Academic.full.index --remote <vm_size>`

Houd er rekening mee dat het 5-10 minuten duurt voor het inrichten van een tijdelijke VM om de index te maken. Voor snelle ontwikkeling van prototypen, kunt u het volgende doen:
- Ontwikkel lokaal met een kleinere set gegevens op een willekeurige machine.
- Handmatig [maken van een Azure-VM](../../../articles/virtual-machines/windows/quick-create-portal.md), [verbinding maken ](../../../articles/virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) via Extern bureaublad, installeer de [Knowledge Exploration Service SDK](https://www.microsoft.com/en-us/download/details.aspx?id=51488) en uitvoeren [ `kes.exe` ](CommandLine.md) vanuit de VM.

Wisselbestand vertraagt het bouwproces. Om wisselbestanden te voorkomen gebruikt u een VM met drie keer de hoeveelheid RAM-geheugen als de bestandsgrootte van de invoergegevens voor het bouwen van de index. Gebruik een VM met 1 GB meer RAM dan de indexgrootte voor het hosten. Kijk voor een lijst met beschikbare VM-grootten op [Grootten voor virtuele machines](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

## <a name="deploy-the-service"></a>Implementeer de service

Nadat u een grammatica en een index hebt, bent u gereed voor de implementatie van de service in een Azure-cloudservice. Ga voor informatie over een nieuwe Azure-cloudservice naar[Hoe kunt u een cloudservice maken en implementeren](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md). Geef op dit moment nog geen implementatiepakket aan.  

Als u de cloudservice hebt gemaakt, kunt u [ `kes.exe deploy_service` ](CommandLine.md#deploy_service-command) gebruiken om de service te implementeren. Een Azure-cloudservice heeft twee implementatiesites: productie en fasering. Voor een service die live gebruikersverkeer ontvangt, moet u in eerste instantie op de staging-site implementeren. Wacht tot de service start en initialiseert. Vervolgens kunt u een paar aanvragen sturen validatie van de implementatie en een basistest uitvoeren.

[Wissel](../../../articles/cloud-services/cloud-services-nodejs-stage-application.md) de inhoud van de staging-site met de productiesite zodat live-verkeer nu wordt omgeleid naar de zojuist geïmplementeerde service. U kunt dit proces herhalen bij het implementeren van een bijgewerkte versie van de service met de nieuwe gegevens. Net als bij alle overige Azure-cloudservices kunt u de Azure Portal (optioneel) gebruiken om [automatisch schalen](../../../articles/cloud-services/cloud-services-how-to-scale-portal.md) te configureren.

In dit voorbeeld implementeert u de *Academische* index naar een staging-site van een bestaande cloudservice met *< vm_size >* VM's. Gebruik de volgende opdracht:

`kes.exe deploy_service http://<account>.blob.core.windows.net/<container>/Academic.grammar http://<account>.blob.core.windows.net/<container>/Academic.index <serviceName> <vm_size> --slot Staging`

Kijk voor een lijst met beschikbare VM-grootten op [Grootten voor virtuele machines](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

Nadat u de service implementeert, kunt u de verschillende [web-API's](WebAPI.md) aanroepen om de interpretatie van natuurlijke taal, de query-voltooiing, de gestructureerde query-evaluatie en de histogramberekening te testen.  

## <a name="test-the-service"></a>Test de service

Voor foutopsporing bij een live-service, blader naar de hostcomputer via een webbrowser. Ga naar `http://localhost:<port>/` voor een lokale service geïmplementeerd via host_service.  Ga naar `http://<serviceName>.cloudapp.net/` voor een lokale service geïmplementeerd via deploy_service.

Deze pagina bevat een koppeling naar informatie over de elementaire statistieken voor API-aanroepen, evenals de grammatica en de index die wordt gehost op deze service. Deze pagina bevat ook een interactieve zoekopdracht-interface die het gebruik van de web-API's demonstreert. Voer query's in het zoekvak in om de resultaten te zien van de [interpretatie](interpretMethod.md), [evaluatie](evaluateMethod.md), en [calchistogram](calchistogramMethod.md) API-aanroepen. De onderliggende HTML-bron van deze pagina fungeert ook als een voorbeeld van hoe u de web-API's kan integreren in een app, om een uitgebreide en interactieve zoekervaring te creëren.


