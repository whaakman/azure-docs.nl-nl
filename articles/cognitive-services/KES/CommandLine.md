---
title: Opdrachtregelinterface - Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Gebruik de opdrachtregelinterface voor het bouwen van de index en grammatica-bestanden van gestructureerde gegevens, en vervolgens als webservices te implementeren.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/24/2016
ms.author: paulhsu
ms.openlocfilehash: 018552982a8ece3bbbaea2d60e2a6e64f681f822
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860113"
---
# <a name="command-line-interface"></a>Opdrachtregelinterface

De opdrachtregelinterface voor Knowledge Exploration Service (KES) biedt de mogelijkheid om te bouwen index en grammatica-bestanden van gestructureerde gegevens en ze te implementeren als webservices.  Het maakt gebruik van de algemene syntaxis: `kes.exe <command> <required_args> [<optional_args>]`.  U kunt uitvoeren `kes.exe` zonder argumenten om een lijst van opdrachten, weer te geven of `kes.exe <command>` om een lijst met argumenten die beschikbaar zijn voor de opgegeven opdracht weer te geven.  Hieronder volgt een lijst van beschikbare opdrachten:

* build_index
* build_grammar
* host_service
* deploy_service
* describe_index
* describe_grammar

<a name="build_index-command"></a>

## <a name="buildindex-command"></a>build_index Command

De **build_index** opdracht bouwt een binair indexbestand uit een bestand van de definitie van schema en een gegevensbestand van objecten moeten worden geïndexeerd.  Het resulterende indexbestand kan worden gebruikt om gestructureerde queryexpressies te evalueren of voor het genereren van een perfecte ervaring bij van query's in natuurlijke taal in combinatie met een gecompileerde grammaticabestand.

`kes.exe build_index <schemaFile> <dataFile> <indexFile> [options]`

| Parameter      | Description               |
|----------------|---------------------------|
| `<schemaFile>` | Pad van de invoer-schema |
| `<dataFile>`   | Invoergegevens pad   |
| `<indexFile>`  | Uitvoerpad index |
| `--description <description>` | Beschrijving |
| `--remote <vmSize>`           | Grootte van virtuele machine voor externe build |

Deze bestanden kunnen worden opgegeven met lokale paden of URL-paden naar Azure-blobs.  Het schemabestand beschrijft de structuur van de objecten worden geïndexeerd en de bewerkingen worden ondersteund (Zie [Schema-indeling](SchemaFormat.md)).  Het gegevensbestand inventariseert de objecten en kenmerkwaarden worden geïndexeerd (Zie [gegevensindeling](DataFormat.md)).  Als de build is geslaagd, bevat het uitvoerbestand index een gecomprimeerde weergave van de ingevoerde gegevens die ondersteuning biedt voor de gewenste bewerkingen.  

Een beschrijving (optioneel) worden opgegeven voor het identificeren van een binaire index met de **describe_index** opdracht.  

De index is standaard gebouwd op de lokale computer.  Lokale builds zijn buiten de Azure-omgeving, beperkt tot gegevensbestanden die maximaal 10.000 objecten bevatten.  Wanneer de--externe vlag is opgegeven, wordt de index op een tijdelijk gemaakte Azure-VM van de opgegeven grootte worden samengesteld.  Hiermee kunt grote indexen kunnen worden gebouwd efficiënt gebruik van virtuele Azure-machines met meer geheugen.  Om te voorkomen wisselbestand die het bouwproces vertraagt, wordt u aangeraden een virtuele machine met 3 keer de hoeveelheid RAM-geheugen als de bestandsgrootte van invoergegevens.  Kijk voor een lijst met beschikbare VM-grootten op [Grootten voor virtuele machines](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

> [!TIP] 
> Post voor snellere builds voor de objecten in het bestand met door de kans te verkleinen.

<a name="build_grammar-command"></a>

## <a name="buildgrammar-command"></a>build_grammar opdracht

De **build_grammar** opdracht wordt een grammatica is opgegeven in XML-bestand naar een binaire grammaticabestand gecompileerd.  Het resulterende grammaticabestand kan worden gebruikt in combinatie met een indexbestand voor het genereren van een perfecte ervaring bij van query's in natuurlijke taal.

`kes.exe build_grammar <xmlFile> <grammarFile>`

| Parameter       | Description               |
|-----------------|---------------------------|
| `<xmlFile>`     | XML-grammatica-specificatie invoerpad |
| `<grammarFile>` | Uitvoerpad gecompileerde grammatica         |

Deze bestanden kunnen worden opgegeven met lokale paden of URL-paden naar Azure-blobs.  De specificatie grammatica wordt beschreven welke gewogen natuurlijke taal expressies en hun functionaliteit voor semantische interpretaties (Zie [grammatica-indeling](GrammarFormat.md)).  Wanneer de build is gelukt, bevat het uitvoerbestand grammatica een binaire weergave van de grammatica-specificatie om in te schakelen snelle decoderen.

<a name="host_service-command"/>

## <a name="hostservice-command"></a>host_service Command

De **host_service** opdracht als host fungeert voor een exemplaar van de service KES op de lokale computer.

`kes.exe host_service <grammarFile> <indexFile> [options]`

| Parameter       | Description                |
|-----------------|----------------------------|
| `<grammarFile>` | Invoer binaire grammatica-pad         |
| `<indexFile>`   | Pad van invoer binaire index           |
| `--port <port>` | Lokale poortnummer.  Standaard: 8000 |

Deze bestanden kunnen worden opgegeven met lokale paden of URL-paden naar Azure-blobs.  Een webservice wordt gehost op http://localhost:&lt; poort&gt;/.  Zie [Web-API's](WebAPI.md) voor een lijst met ondersteunde bewerkingen.

Buiten de Azure-omgeving lokaal gehoste services beperkt zijn tot index maximaal 1 MB in grootte, 10 aanvragen per seconde en totaal aantal aanroepen van 1000 bestanden.  Uitvoeren om te strijden tegen deze beperkingen, **host_service** binnen een Azure-VM of implementeren op een Azure-cloud service met behulp van **deploy_service**.

<a name="deploy_service-command"/>

## <a name="deployservice-command"></a>deploy_service Command

De **deploy_service** opdracht wordt een exemplaar van de service KES geïmplementeerd in een Azure cloudservice.

`kes.exe deploy_service <grammarFile> <indexFile> <serviceName> <vmSize>[options]`

| Parameter       | Description                  |
|-----------------|------------------------------|
| `<grammarFile>` | Invoer binaire grammatica-pad           |
| `<indexFile>`   | Pad van invoer binaire index             |
| `<serviceName>` | Naam van de doelcloudservice |
| `<vmSize>`      | Grootte van cloudservice-VM     |
| `--slot <slot>` | Cloudservicesite: "staging" (standaard), 'productie' |

Deze bestanden kunnen worden opgegeven met lokale paden of URL-paden naar Azure-blobs.  De naam van service geeft aan dat een vooraf geconfigureerde Azure-cloud-service (Zie [maken en implementeren van een Cloudservice](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md)).  De opdracht wordt de service KES automatisch geïmplementeerd naar de opgegeven Azure-cloud-service, met behulp van virtuele machines van de opgegeven grootte.  Om te voorkomen wisselbestand die de prestaties aanzienlijk verlaagd, wordt u aangeraden een virtuele machine met 1 GB meer RAM-geheugen dan de grootte van het invoer-index.  Zie voor een lijst van beschikbare VM-grootten, [groottes voor Cloud Services](../../../articles/cloud-services/cloud-services-sizes-specs.md).

Standaard de service wordt geïmplementeerd in de faseringsomgeving (optioneel) via de sleuf parameter--overschreven.  Zie [Web-API's](WebAPI.md) voor een lijst met ondersteunde bewerkingen.

<a name="describe_index-command"/>

## <a name="describeindex-command"></a>describe_index opdracht

De **describe_index** opdracht voert informatie over een indexbestand, met inbegrip van het schema en de beschrijving.

`kes.exe describe_index <indexFile>`

| Parameter     | Description      |
|---------------|------------------|
| `<indexFile>` | Pad van de invoer-index |

Dit bestand kan worden opgegeven met een lokaal bestandspad of een URL-pad naar een Azure-blob.  De beschrijving van de uitvoer kan worden opgegeven met behulp van de--beschrijvingsparameter van de **build_index** opdracht.

<a name="describe_grammar-command"/>

## <a name="describegrammar-command"></a>describe_grammar opdracht

De **describe_grammar** opdracht levert de oorspronkelijke grammatica-specificatie gebruikt voor het bouwen van de binaire grammatica.

`kes.exe describe_grammar <grammarFile>`

| Parameter       | Description      |
|-----------------|------------------|
| `<grammarFile>` | Pad van de invoer-grammatica |

Dit bestand kan worden opgegeven met een lokaal bestandspad of een URL-pad naar een Azure-blob.

