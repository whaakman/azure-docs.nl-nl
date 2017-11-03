---
title: Azure Batch taakgebeurtenis | Microsoft Docs
description: Verwijzing voor de gebeurtenis complete Batch-taak.
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 015adf7dbc47c29a78df4e4889b2ee1ddcccdd8e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="task-complete-event"></a>Taakgebeurtenis

 Deze gebeurtenis wordt verzonden zodra een taak is voltooid, ongeacht de afsluitcode. Deze gebeurtenis kan worden gebruikt voor de duur van een taak wanneer de taak is uitgevoerd en of deze opnieuw is uitgevoerd.


 Het volgende voorbeeld ziet de hoofdtekst van een taak voltooid gebeurtenis.

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "startTime": "2016-09-08T16:32:23.799Z",
        "endTime": "2016-09-08T16:34:00.666Z",
        "exitCode": 0,
        "retryCount": 0,
        "requeueCount": 0
    }
}
```

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|JobId|Tekenreeks|De id van de taak met de taak.|
|id|Tekenreeks|De id van de taak.|
|taskType|Tekenreeks|Het type van de taak. Dit kan worden 'Taakbeheer heeft, die aangeeft dat het een jobbeheertaak of gebruiker is niet een jobbeheertaak aangeeft. Deze gebeurtenis is niet verzonden voor jobvoorbereidingstaken, jobvrijgevingstaken of Begintaken.|
|systemTaskVersion|Int32|Dit is de interne nieuwe pogingen voor een taak. Intern kan de Batch-service opnieuw proberen een taak voor het account voor tijdelijke problemen. Deze problemen kunnen interne planning fouten of pogingen tot het herstellen van de rekenknooppunten in een verkeerde status.|
|[nodeInfo](#nodeInfo)|Complex Type|Bevat informatie over het rekenknooppunt waarop de taak is uitgevoerd.|
|[multiInstanceSettings](#multiInstanceSettings)|Complex Type|Geeft aan dat de taak een taak met meerdere instanties vereisen van meerdere rekenknooppunten.  Zie [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) voor meer informatie.|
|[beperkingen](#constraints)|Complex Type|De uitvoering van beperkingen voor deze taak.|
|[executionInfo](#executionInfo)|Complex Type|Bevat informatie over de uitvoering van de taak.|

###  <a name="nodeInfo"></a>nodeInfo

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|poolId|Tekenreeks|De id van de groep waarop de taak is uitgevoerd.|
|nodeId|Tekenreeks|De id van het knooppunt waarop de taak is uitgevoerd.|

###  <a name="multiInstanceSettings"></a>multiInstanceSettings

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|numberOfInstances|Int32|Het aantal rekenknooppunten dat is vereist voor de taak.|

###  <a name="constraints"></a>beperkingen

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Het maximum aantal keren dat de taak kan opnieuw worden uitgevoerd. De Batch-service probeert een taak opnieuw als de afsluitcode gelijk aan nul is.<br /><br /> Houd er rekening mee dat deze waarde specifiek het aantal nieuwe pogingen bepaalt. De Batch-service probeert de taak één keer en probeer vervolgens tot deze limiet. Bijvoorbeeld, als het maximale aantal pogingen maximaal 3, Batch probeert een taak is 4 een time-out (een initiële probeer en 3 nieuwe pogingen).<br /><br /> Als het maximale aantal pogingen 0 is, probeert taken niet in de Batch-service opnieuw.<br /><br /> Als het maximale aantal pogingen -1 is, probeert de Batch-service opnieuw taken zonder limiet.<br /><br /> De standaardwaarde is 0 (geen herhaalde pogingen).|

###  <a name="executionInfo"></a>executionInfo

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|startTime|Datum/tijd|De tijd waarop de taak is gestart. 'Actief' komt overeen met de **met** staat, zodat als de taak wordt opgegeven bronbestanden of toepassingspakketten, klikt u vervolgens de begintijd weerspiegelt de tijd waarop de taak gestart downloaden of implementeren van deze.  Als de taak is opnieuw gestart of opnieuw uitgevoerd, is dit de meest recente tijd waarop de taak is gestart.|
|endTime|Datum/tijd|De tijd waarop de taak is voltooid.|
|exitCode|Int32|De afsluitcode van de taak.|
|retryCount|Int32|Het aantal keren dat de taak door de Batch-service opnieuw is geprobeerd. De taak opnieuw wordt gestart als deze wordt afgesloten met een andere waarde dan nul afsluitcode tot maximaal de opgegeven MaxTaskRetryCount.|
|requeueCount|Int32|Het aantal keren dat de taak is door de Batch-service is opnieuw als gevolg van een gebruikersaanvraag.<br /><br /> Wanneer de knooppunten van de gebruiker wordt verwijderd uit een pool (door vergroten of verkleinen of verkleinen van de pool) of wanneer de taak wordt uitgeschakeld, de gebruiker kan opgeven dat wordt uitgevoerd op de knooppunten taken worden opnieuw uitgevoerd. Dit aantal houdt het aantal keren dat de taak is opnieuw om deze redenen.|
