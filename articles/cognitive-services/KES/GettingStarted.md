---
title: Aan de slag met de Service van de exploratie kennis | Microsoft Docs
description: Gebruik kennis exploratie Service (KES) te maken van een engine voor een interactieve zoekfunctie over academic publicaties in cognitieve Microsoft-Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 02dc9368eef02d6fa507335ef3171e923412acca
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344784"
---
<a name="getting-started"></a>
# <a name="get-started-with-the-knowledge-exploration-service"></a>Aan de slag met de kennis exploratie-Service
In dit scenario kunt u de kennis exploratie Service (KES) gebruiken voor het maken van de engine voor een interactieve zoekfunctie voor academic publicaties. U kunt het opdrachtregelprogramma installeren [ `kes.exe` ](CommandLine.md), en alle voorbeeldbestanden van een van de [kennis exploratie Service SDK](https://www.microsoft.com/en-us/download/details.aspx?id=51488).

Het voorbeeld academic publicaties bevat een voorbeeld van 1000 academic papers gepubliceerd door Microsoft-onderzoekers.  Elk artikel is gekoppeld aan een titel, jaar van de publicatie, auteurs en sleutelwoorden. De auteur van elk wordt voorgesteld door een ID, de naam en het lidmaatschap van de op het moment van publicatie. Elk trefwoord kan worden gekoppeld aan een reeks synoniemen (bijvoorbeeld het sleutelwoord 'ondersteuning vectormachine' kan worden gekoppeld aan het synoniem 'svm').

<a name="defining-schema"></a>
## <a name="define-the-schema"></a>Het schema definiëren
De kenmerk-structuur van de objecten in het domein wordt beschreven in het schema. Dit geeft het type naam en het gegevenstype voor elk kenmerk in een JSON-bestandsindeling. Het volgende voorbeeld wordt de inhoud van het bestand *Academic.schema*.

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

Hier kunt u definiëren *titel*, *jaar*, en *sleutelwoord* als een tekenreeks, geheel getal en de tekenreeks kenmerk, respectievelijk. Omdat auteurs worden vertegenwoordigd door de ID, naam en relatie, definieert u *auteur* als een samengesteld kenmerk met drie onderliggende kenmerken: *Author.Id*, *naam.auteur*, en *Author.Affiliation*.

Standaard ondersteunt kenmerken alle bewerkingen die beschikbaar zijn voor het gegevenstype, inclusief *gelijk is aan*, *starts_with*, en *is_between*. Omdat Auteur-ID alleen intern als een id gebruikt wordt, de standaardwaarde onderdrukken en geef *gelijk is aan* geïndexeerd als de enige bewerking.

Voor de *sleutelwoord* kenmerk, toestaan synoniemen overeenkomen met het sleutelwoord canonieke waarden door te geven van het bestand synoniem *Keyword.syn* in de kenmerkdefinitie van het. Dit bestand bevat een lijst met canonieke- en waardeparen synoniem:

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

<a name="generating-data"></a>
## <a name="generate-data"></a>Gegevens moeten genereren
Het gegevensbestand beschrijving van de lijst van de publicaties worden geïndexeerd, met elke regel opgeven van de kenmerkwaarden van een artikel in [JSON-indeling](http://json.org/).  Het volgende voorbeeld wordt een enkele regel uit het gegevensbestand *Academic.data*, geformatteerde voor leesbaarheid:

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

In deze-codefragment maakt u de *titel* en *jaar* kenmerk van het papier als een JSON-tekenreeks en een nummer, respectievelijk. Meerdere waarden worden weergegeven met behulp van de JSON-matrices. Omdat *auteur* is een samengesteld kenmerk, elke waarde wordt vertegenwoordigd door een JSON-object dat bestaat uit de onderliggende kenmerken. Kenmerken met ontbrekende waarden, zoals *sleutelwoord* in dit geval kunnen worden uitgesloten van de JSON-weergave.

Om te onderscheiden van de kans op verschillende documenten, geeft u de kans relatieve logboek met behulp van de ingebouwde *logprob* kenmerk. Een waarschijnlijkheid gegeven *p* tussen 0 en 1 u de kans logboek als logboek berekenen (*p*), waarbij log() de functie natuurlijke log.

Zie voor meer informatie [gegevensindeling](DataFormat.md).

<a name="building-index"></a>
## <a name="build-a-compressed-binary-index"></a>Een gecomprimeerde binaire index bouwen
Nadat u een schemabestand en het gegevensbestand hebt, kunt u een gecomprimeerde binaire index van de gegevensbron objecten maken met behulp van [ `kes.exe build_index` ](CommandLine.md#build_index-command). In dit voorbeeld bouwt u de indexbestand *Academic.index* uit het bestand voor invoer schema *Academic.schema* en gegevensbestand *Academic.data*. Gebruik de volgende opdracht:

`kes.exe build_index Academic.schema Academic.data Academic.index`

Voor snelle maken van een prototype buiten Azure, [ `kes.exe build_index` ](CommandLine.md#build_index-command) lokaal kleine indexen van gegevensbestanden met maximaal 10.000 objecten kunt maken. Voor grotere gegevensbestanden, kunt u ofwel de opdracht uitvoeren vanuit een [Windows virtuele machine in Azure](../../../articles/virtual-machines/windows/quick-create-portal.md), of een externe build uitvoeren in Azure. Zie voor meer informatie [omhoog schalen](#scaling-up).

<a name="authoring-grammar"></a>
## <a name="use-an-xml-grammar-specification"></a>Een XML-grammatica-specificatie gebruiken
De grammatica bevat de set van natuurlijke taal query's die de service kan worden geïnterpreteerd, en hoe deze query's van natuurlijke taal worden vertaald in semantische query-expressies. In dit voorbeeld gebruikt u de grammatica die is opgegeven in *academic.xml*:

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

Zie voor meer informatie over de syntaxis van de specificatie grammatica [grammatica-indeling](GrammarFormat.md).

<a name="compiling-grammar"></a>
## <a name="compile-the-grammar"></a>De grammatica compileren
Nadat u een XML-grammatica-specificatie hebt, kunt u deze in een binaire grammatica compileren met behulp van [ `kes.exe build_grammar` ](CommandLine.md#build_grammar-command). Houd er rekening mee dat als de grammatica een schema importeert, het schemabestand moet zich bevinden in hetzelfde pad hebben als het XML-grammatica. In dit voorbeeld bouwt u de binaire grammaticabestand *Academic.grammar* uit het invoerbestand XML-grammatica *Academic.xml*. Gebruik de volgende opdracht:

`kes.exe build_grammar Academic.xml Academic.grammar`

<a name="hosting-index"></a>
## <a name="host-the-grammar-and-index-in-a-web-service"></a>De grammatica en wordt de index in een webservice host
U kunt de grammatica en de index in een webservice op de lokale computer voor snelle maken van een prototype, hosten met behulp van [ `kes.exe host_service` ](CommandLine.md#host_service-command). U kunt vervolgens toegang tot de service via [web-API's](WebAPI.md) voor het valideren van het ontwerp van de gegevens correct en grammatica. In dit voorbeeld u het grammaticabestand host *Academic.grammar* en indexbestand *Academic.index* op http://localhost:8000/. Gebruik de volgende opdracht:

`kes.exe host_service Academic.grammar Academic.index --port 8000`

Hiermee initieert u een lokaal exemplaar van de webservice. U kunt de service interactief testen door bezoeken `http::localhost:<port>` vanuit een browser. Zie voor meer informatie [service testen](#testing-service).

Kunt u ook rechtstreeks aanroepen verschillende [web-API's](WebAPI.md) interpretatie van natuurlijke taal, query is voltooid, structured query-evaluatie en histogram berekening testen. Invoeren voor het stoppen van de service, 'afsluiten' in de `kes.exe host_service` opdrachtprompt of druk op Ctrl + C. Hier volgen enkele voorbeelden:

* [http://localhost:8000/interpret?query=papers door susan t dumais](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20dumais)
* [http://localhost:8000/interpret?query=papers door susan t & d- h aanvullen = 1](http://localhost:8000/interpret?query=papers%20by%20susan%20t%20d&complete=1)
* [http://localhost:8000/evaluate?expr=Composite(Author.Name=='susan t dumais') & attributes=Title,Year,Author.Name,Author.Id & count = 2](http://localhost:8000/evaluate?expr=Composite%28Author.Name==%27susan%20t%20dumais%27%29&attributes=Title,Year,Author.Name,Author.Id&count=2)
* [http://localhost:8000/calchistogram?expr=And(Composite(Author.Name=='susan t dumais'), jaar > = 2013) & kenmerken = jaar, sleutelwoord & count = 4](http://localhost:8000/calchistogram?expr=And%28Composite%28Author.Name=='susan%20t%20dumais'%29,Year>=2013%29&attributes=Year,Keyword&count=4)

Buiten Azure, [ `kes.exe host_service` ](CommandLine.md#host_service-command) is beperkt tot indexen van maximaal 10.000 objecten. Andere beperkingen zijn een percentage API 10 aanvragen per seconde en een totaal van 1000 aanvragen voordat het proces wordt automatisch beëindigd. Als u wilt deze beperkingen omzeilen, voert u de opdracht vanuit een [Windows virtuele machine in Azure](../../../articles/virtual-machines/windows/quick-create-portal.md), of met een Azure-cloud-service implementeren met behulp van de [ `kes.exe deploy_service` ](CommandLine.md#deploy_service-command) opdracht. Zie voor meer informatie [implementatie service](#deploying-service).

<a name="scaling-up"></a>
## <a name="scale-up-to-host-larger-indices"></a>Opschalen naar host groter indexen
Wanneer u uitvoert `kes.exe` buiten Azure, de index is beperkt tot 10.000 objecten. U kunt bouwen en grotere indexen host met behulp van Azure. Aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). Als u zich op Visual Studio of MSDN abonneert, u kunt ook [voordelen-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Deze bieden sommige Azure-tegoed per maand.

Om toe te staan `kes.exe` toegang tot een Azure-account [download het Azure Publish Settings-bestand](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) vanuit de Azure-portal. Als u wordt gevraagd, meld u aan bij het gewenste Azure-account. Sla het bestand als *AzurePublishSettings.xml* in de werkmap waar `kes.exe` wordt uitgevoerd.

Er zijn twee manieren om te bouwen en hosten van grote indexen. De eerste is voor het voorbereiden van de bestanden van het schema en de gegevens in een Windows-VM in Azure. Voer [ `kes.exe build_index` ](#building-index) index te maken lokaal op de virtuele machine, zonder beperkingen. De resulterende index kan worden gehost lokaal op de virtuele machine met behulp van [ `kes.exe host_service` ](#hosting-service) voor snelle maken van een prototype, opnieuw zonder beperkingen. Zie voor gedetailleerde stappen de [Azure VM-zelfstudie](../../../articles/virtual-machines/windows/quick-create-portal.md).

De tweede methode is het uitvoeren van een externe Azure bouwen met behulp van [ `kes.exe build_index` ](CommandLine.md#build_index-command) met de `--remote` parameter. Hiermee geeft u een Azure VM-grootte. Wanneer de `--remote` parameter is opgegeven, wordt de opdracht maakt u een tijdelijke virtuele machine van Azure die grootte. Vervolgens maakt u de index op de virtuele machine, wordt de index geüpload naar de doel-blob-opslag en wordt verwijderd van de virtuele machine na voltooiing. Uw Azure-abonnement is kosten in rekening gebracht voor de kosten van de virtuele machine terwijl de index wordt opgebouwd.

Deze mogelijkheid van de externe Azure build [ `kes.exe build_index` ](CommandLine.md#build_index-command) kunnen worden uitgevoerd in een omgeving. Wanneer u een externe build uitvoert, de invoerargumenten schema en de gegevens zijn lokale paden of [Azure blob-opslag](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) URL's. Het indexargument uitvoer moet de URL van een blob-opslag. Voor het maken van een Azure storage-account, Zie [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md). Bestanden efficiënt te kopiëren en gebruiken van blob-opslag, de [AzCopy](../../storage/common/storage-use-azcopy.md) hulpprogramma.

In dit voorbeeld kunt u ervan uitgaan dat de volgende blob storage-container al is gemaakt: http://&lt;*account*&gt;.blob.core.windows.net/&lt;*container* &gt;/. Deze bevat het schema *Academic.schema*, het bestand waarnaar wordt verwezen synoniem *Keywords.syn*, en het volledige bestand *Academic.full.data*. U kunt de volledige index op afstand bouwen met behulp van de volgende opdracht:

`kes.exe build_index http://<account>.blob.core.windows.net/<container>/Academic.schema http://<account>.blob.core.windows.net/<container>/Academic.full.data http://<account>.blob.core.windows.net/<container>/Academic.full.index --remote <vm_size>`

Houd er rekening mee dat het duurt 5-10 minuten voor het inrichten van een temporay VM index te maken. Voor snelle maken van een prototype, kunt u het volgende doen:
- Ontwikkelen met een kleinere set gegevens lokaal op elke computer.
- Handmatig [maken van een Azure VM](../../../articles/virtual-machines/windows/quick-create-portal.md), [verbinding maken met het](../../../articles/virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) installeren via Extern bureaublad de [kennis exploratie Service SDK](https://www.microsoft.com/en-us/download/details.aspx?id=51488), en voer [ `kes.exe` ](CommandLine.md) uit vanuit de virtuele machine.

Paginering vertraagt de procedure voor het maken. Om te voorkomen paginering, gebruiken een virtuele machine met drie keer de hoeveelheid RAM-geheugen als de bestandsgrootte van de invoergegevens voor het bouwen van de index. Gebruik een virtuele machine met 1 GB meer dan de indexgrootte voor het hosten van RAM. Zie voor een lijst met beschikbare grootten voor virtuele machine, [grootten voor virtuele machines](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

<a name="deploying-service"></a>
## <a name="deploy-the-service"></a>De service implementeren
Nadat u een grammatica en -index hebt, bent u klaar voor het implementeren van de service met een Azure-cloud-service. Zie het maken van een nieuwe Azure-cloudservice [maken en implementeren van een cloudservice](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md). Geef een implementatiepakket op dit moment.  

Als u de cloudservice hebt gemaakt, kunt u [ `kes.exe deploy_service` ](CommandLine.md#deploy_service-command) om de service te implementeren. Een Azure-cloud-service heeft twee implementatiesites: productie en in fasering. Voor een service die live gebruikersverkeer ontvangt, moet u in eerste instantie op de faseringssleuf implementeren. Wacht totdat de service om te starten en te initialiseren. Vervolgens kunt u enkele aanvragen voor het valideren van de implementatie en controleer of dat deze basic doorstaat verzenden.

[Wisselen](../../../articles/cloud-services/cloud-services-nodejs-stage-application.md) de inhoud van de staging-sleuf met de productiesite zodat de geïmplementeerde service nu live verkeer wordt omgeleid. U kunt dit proces herhalen bij het implementeren van een bijgewerkte versie van de service met nieuwe gegevens. Zoals met alle andere Azure-cloud-services, u kunt eventueel de Azure-portal voor het configureren van [automatisch schalen](../../../articles/cloud-services/cloud-services-how-to-scale-portal.md).

In dit voorbeeld implementeert u de *Academic* index van de faseringssleuf van een bestaande cloudservice met *< vm_size >* virtuele machines. Gebruik de volgende opdracht:

`kes.exe deploy_service http://<account>.blob.core.windows.net/<container>/Academic.grammar http://<account>.blob.core.windows.net/<container>/Academic.index <serviceName> <vm_size> --slot Staging`

Zie voor een lijst met beschikbare grootten voor virtuele machine, [grootten voor virtuele machines](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

Nadat u de service implementeert, kunt u de verschillende aanroepen [web-API's](WebAPI.md) interpretatie van natuurlijke taal, query is voltooid, structured query-evaluatie en histogram berekening testen.  

<a name="testing-service"></a>
## <a name="test-the-service"></a>De service testen
Als u wilt opsporen in een service voor live, blader naar de hostmachine vanuit een webbrowser. Voor een lokale service geïmplementeerd [host_service](#hosting-service), gaat u naar `http://localhost:<port>/`.  Voor een Azure cloud service geïmplementeerd [deploy_service](#deploying-service), gaat u naar `http://<serviceName>.cloudapp.net/`.

Deze pagina bevat een koppeling naar informatie over basic statistieken voor API-aanroep, evenals de grammatica en de index op deze service wordt gehost. Deze pagina bevat ook een interactieve zoekinterface over het gebruik van de web-API's. Voer de query's in het zoekvak om te zien van de resultaten van de [interpreteren](interpretMethod.md), [evalueren](evaluateMethod.md), en [calchistogram](calchistogramMethod.md) API-aanroepen. De onderliggende HTML-bron van deze pagina fungeert ook als een voorbeeld van het integreren van de web-API's in een app, voor het maken van een geavanceerde, interactieve zoekfunctie.


