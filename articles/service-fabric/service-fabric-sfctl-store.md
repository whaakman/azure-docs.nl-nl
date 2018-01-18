---
title: Azure Service Fabric CLI - sfctl store | Microsoft Docs
description: Beschrijving van de Service Fabric CLI sfctl store-opdrachten.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: d4ca3c35c34736c3b4824f956a6a72002c891877
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-store"></a>sfctl store
Basic-bestand niveau bewerkingen uitvoeren op de cluster image store.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
|    verwijderen| Hiermee verwijdert u de bestaande installatiekopie opslaan van inhoud.|
|    root-info| Hiermee haalt u informatie over de inhoud in de hoofdmap van de image store.|
|    Stat  | Hiermee haalt u de image store-inhoudsinformatie.|


## <a name="sfctl-store-delete"></a>sfctl store verwijderen
Hiermee verwijdert u de bestaande installatiekopie opslaan van inhoud.

Hiermee verwijdert u het image store inhoud wordt gevonden binnen de opgegeven installatiekopie van het bestaande opslaan relatief pad. Met deze opdracht kan worden gebruikt geüploade toepassingspakketten verwijderen zodra deze zijn ingericht.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --inhoud-path [vereist]| Relatief pad naar het bestand of map in de image store in de hoofdmap.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-store-stat"></a>sfctl store stat
Hiermee haalt u de image store-inhoudsinformatie.

Retourneert de informatie over de inhoud van de store op het opgegeven contentPath ten opzichte van de hoofdmap van de image store.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --inhoud-path [vereist]| Relatief pad naar het bestand of map in de image store in de hoofdmap.|
| --time-out -t          | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug               | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h             | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o           | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query               | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie over en voorbeelden.|
| --uitgebreide             | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="next-steps"></a>Volgende stappen
- [Setup](service-fabric-cli.md) de Service Fabric CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).