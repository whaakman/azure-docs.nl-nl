---
title: Kennis exploratie Service opdrachtregelinterface | Microsoft Docs
description: De opdrachtregelinterface KES gebruiken om bestanden index en grammatica van gestructureerde gegevens samen te stellen en vervolgens als webservices in cognitieve Microsoft-Services te implementeren.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/24/2016
ms.author: paulhsu
ms.openlocfilehash: ffa42ac73b42a8271004d2d45d7a80f3307ef059
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344547"
---
# <a name="command-line-interface"></a>Opdrachtregelinterface
De KES opdrachtregelinterface biedt de mogelijkheid om te bouwen index en grammatica-bestanden van gestructureerde gegevens en deze als webservices te implementeren.  De algemene syntaxis wordt gebruikt: `kes.exe <command> <required_args> [<optional_args>]`.  U kunt uitvoeren `kes.exe` zonder argumenten om een lijst met opdrachten weer te geven of `kes.exe <command>` om een lijst met argumenten die beschikbaar zijn voor de opgegeven opdracht weer te geven.  Hieronder volgt een lijst met beschikbare opdrachten:
* build_index
* build_grammar
* host_service
* deploy_service
* describe_index
* describe_grammar

<a name="build_index-command"></a>
## <a name="buildindex-command"></a>build_index opdracht
De **build_index** opdracht maakt u een bestand binaire index van een schemadefinitiebestand en een gegevensbestand van objecten moeten worden geïndexeerd.  Het resulterende indexbestand kan worden gebruikt om gestructureerde queryexpressies te evalueren of interpretatie van natuurlijke taal query's in combinatie met een gecompileerde grammaticabestand genereren.

`kes.exe build_index <schemaFile> <dataFile> <indexFile> [options]`

| Parameter      | Beschrijving               |
|----------------|---------------------------|
| `<schemaFile>` | Invoer schema pad |
| `<dataFile>`   | Pad van de invoergegevens   |
| `<indexFile>`  | Uitvoerpad index |
| `--description <description>` | Beschrijving |
| `--remote <vmSize>`           | Grootte van virtuele machine voor externe build |

Deze bestanden kunnen worden opgegeven door lokale paden of URL-paden naar Azure blobs.  Het schemabestand beschrijft de structuur van de objecten die worden geïndexeerd en de bewerkingen worden ondersteund (Zie [Schema-indeling](SchemaFormat.md)).  Het gegevensbestand inventariseert de objecten en kenmerkwaarden worden geïndexeerd (Zie [gegevensindeling](DataFormat.md)).  Wanneer de opbouwbewerking is voltooid, bevat het uitvoerbestand index een gecomprimeerde weergave van de invoergegevens die ondersteuning biedt voor de gewenste bewerkingen.  

Een beschrijving (optioneel) worden opgegeven voor het identificeren van een binaire index met de **describe_index** opdracht.  

Standaard worden de index is gebaseerd op de lokale computer.  Lokale builds zijn buiten de Azure-omgeving, beperkt tot gegevensbestanden die maximaal 10.000 objecten bevat.  Wanneer de--externe vlag is opgegeven, wordt de index wordt gebouwd op een tijdelijk gemaakte Azure virtuele machine van de opgegeven grootte.  Hiermee kunt grote indexen kunnen worden gebouwd efficiënt gebruik maakt van Azure VM's met meer geheugen.  Om te voorkomen paginering die de procedure voor het maken vertraagt, wordt u aangeraden een virtuele machine met 3 keer de hoeveelheid RAM-geheugen als de bestandsgrootte invoergegevens.  Zie voor een lijst met beschikbare grootten voor virtuele machine, [grootten voor virtuele machines](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

> [!TIP] 
> Post voor snellere builds voor de objecten in het gegevensbestand door de kans te verlagen.

<a name="build_grammar-command"></a>
## <a name="buildgrammar-command"></a>build_grammar opdracht
De **build_grammar** opdracht wordt een grammatica die is opgegeven in XML naar een binaire grammaticabestand gecompileerd.  Het resulterende grammaticabestand kan worden gebruikt in combinatie met een indexbestand interpretatie van natuurlijke taal query's te genereren.

`kes.exe build_grammar <xmlFile> <grammarFile>`

| Parameter       | Beschrijving               |
|-----------------|---------------------------|
| `<xmlFile>`     | Invoer-XML-grammatica specificatie pad |
| `<grammarFile>` | Uitvoerpad gecompileerde grammatica         |

Deze bestanden kunnen worden opgegeven door lokale paden of URL-paden naar Azure blobs.  De specificatie grammatica wordt beschreven welke gewogen natuurlijke taal expressies en hun semantische interpretaties (Zie [grammatica-indeling](GrammarFormat.md)).  Wanneer de opbouwbewerking is voltooid, bevat de grammatica-bestand voor uitvoer een binaire voorstelling van de grammatica-specificatie inschakelen snel decoderen.

<a name="host_service-command"/>
## <a name="hostservice-command"></a>host_service opdracht
De **host_service** opdracht als host fungeert voor een exemplaar van de service KES op de lokale computer.

`kes.exe host_service <grammarFile> <indexFile> [options]`

| Parameter       | Beschrijving                |
|-----------------|----------------------------|
| `<grammarFile>` | Invoer binaire grammatica pad         |
| `<indexFile>`   | Invoer binaire index pad           |
| `--port <port>` | Lokale poortnummer.  Standaard: 8000 |

Deze bestanden kunnen worden opgegeven door lokale paden of URL-paden naar Azure blobs.  Een webservice wordt gehost op http://localhost:&lt; poort&gt;/.  Zie [Web-API's](WebAPI.md) voor een lijst met ondersteunde bewerkingen.

Omgeving lokaal gehoste services beperkt zijn tot het indexeren bestanden buiten de Azure maximaal 1 MB in grootte, 10 aanvragen per seconde en het totaal aantal aanroepen 1000.  Uitvoeren als u wilt opheffen van deze beperkingen, **host_service** binnen een virtuele machine van Azure of implementeren naar een Azure-cloud service met behulp van **deploy_service**.

<a name="deploy_service-command"/>
## <a name="deployservice-command"></a>deploy_service opdracht
De **deploy_service** opdracht een exemplaar van de service KES naar een Azure cloudservice wordt geïmplementeerd.

`kes.exe deploy_service <grammarFile> <indexFile> <serviceName> <vmSize>[options]`

| Parameter       | Beschrijving                  |
|-----------------|------------------------------|
| `<grammarFile>` | Invoer binaire grammatica pad           |
| `<indexFile>`   | Invoer binaire index pad             |
| `<serviceName>` | Naam van de cloudservice doel |
| `<vmSize>`      | Grootte van cloudservice-VM     |
| `--slot <slot>` | Cloud service sleuf: "fasering' (standaard), 'productie' |

Deze bestanden kunnen worden opgegeven door lokale paden of URL-paden naar Azure blobs.  De naam van service geeft een vooraf geconfigureerde Azure-cloud-service (Zie [maken en implementeren van een Cloudservice](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md)).  De opdracht wordt automatisch de KES-service implementeren in de opgegeven Azure-cloudservice, met behulp van virtuele machines van de opgegeven grootte.  Om te voorkomen paginering die aanzienlijk duurt, wordt u aangeraden een virtuele machine met 1 GB meer RAM-geheugen dan de bestandsgrootte van de invoer-index.  Zie voor een lijst met beschikbare grootten voor virtuele machine, [grootten voor Cloudservices](../../../articles/cloud-services/cloud-services-sizes-specs.md).

Standaard wordt de service geïmplementeerd bij de faseringsomgeving eventueel overschreven via de--sleuf-parameter.  Zie [Web-API's](WebAPI.md) voor een lijst met ondersteunde bewerkingen.

<a name="describe_index-command"/>
## <a name="describeindex-command"></a>describe_index-opdracht
De **describe_index** opdracht levert informatie over een indexbestand, met inbegrip van het schema en de beschrijving.

`kes.exe describe_index <indexFile>`

| Parameter     | Beschrijving      |
|---------------|------------------|
| `<indexFile>` | Invoer index pad |

Dit bestand kan worden opgegeven met een lokaal pad of een URL-pad naar een Azure-blob.  De beschrijving van de uitvoer kan worden opgegeven met de--parameter beschrijving van de **build_index** opdracht.

<a name="describe_grammar-command"/>
## <a name="describegrammar-command"></a>describe_grammar-opdracht
De **describe_grammar** opdracht levert de oorspronkelijke grammatica-specificatie voor het samenstellen van de binaire grammatica.

`kes.exe describe_grammar <grammarFile>`

| Parameter       | Beschrijving      |
|-----------------|------------------|
| `<grammarFile>` | Invoer grammatica pad |

Dit bestand kan worden opgegeven met een lokaal pad of een URL-pad naar een Azure-blob.

