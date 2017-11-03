---
title: Azure Batch-taak startgebeurtenis | Microsoft Docs
description: Naslaginformatie voor Batch-taak start gebeurtenis.
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
ms.openlocfilehash: c47ab36c99dddd46a14c15018a2a46bf7f873ffa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="task-start-event"></a>Gebeurtenissen voor het starten van taak

 Deze gebeurtenis wordt verzonden wanneer een taak is gepland om te starten op een rekenknooppunt door de planner. Houd er rekening mee dat als de taak wordt later opnieuw of opnieuw wordt deze gebeurtenis weer voor dezelfde taak, maar het aantal nieuwe pogingen worden verzonden en besturingssysteemversie van de taak wordt dienovereenkomstig bijgewerkt.


 Het volgende voorbeeld ziet de instantie van een taak start gebeurtenis.

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
        "retryCount": 0
    }
}
```

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|JobId|Tekenreeks|De id van de taak met de taak.|
|id|Tekenreeks|De id van de taak.|
|taskType|Tekenreeks|Het type van de taak. Dit kan worden 'Taakbeheer heeft, die aangeeft dat het een jobbeheertaak of gebruiker is niet een jobbeheertaak aangeeft.|
|systemTaskVersion|Int32|Dit is de interne nieuwe pogingen voor een taak. Intern kan de Batch-service opnieuw proberen een taak voor het account voor tijdelijke problemen. Deze problemen kunnen interne planning fouten of pogingen tot het herstellen van de rekenknooppunten in een verkeerde status.|
|[nodeInfo](#nodeInfo)|Complex Type|Bevat informatie over het rekenknooppunt waarop de taak is uitgevoerd.|
|[multiInstanceSettings](#multiInstanceSettings)|Complex Type|Geeft aan dat de taak taak met meerdere instanties vereisen van meerdere rekenknooppunten.  Zie [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) voor meer informatie.|
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
|numberOfInstances|int|Het aantal rekenknooppunten dat is vereist voor de taak.|

###  <a name="constraints"></a>beperkingen

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Het maximum aantal keren dat de taak kan opnieuw worden uitgevoerd. De Batch-service probeert een taak opnieuw als de afsluitcode gelijk aan nul is.<br /><br /> Houd er rekening mee dat deze waarde specifiek het aantal nieuwe pogingen bepaalt. De Batch-service probeert de taak één keer en probeer vervolgens tot deze limiet. Bijvoorbeeld, als het maximale aantal pogingen maximaal 3, Batch probeert een taak is 4 een time-out (een initiële probeer en 3 nieuwe pogingen).<br /><br /> Als het maximale aantal pogingen 0 is, probeert taken niet in de Batch-service opnieuw.<br /><br /> Als het maximale aantal pogingen -1 is, probeert de Batch-service opnieuw taken zonder limiet.<br /><br /> De standaardwaarde is 0 (geen herhaalde pogingen).|

###  <a name="executionInfo"></a>executionInfo

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|retryCount|Int32|Het aantal keren dat de taak door de Batch-service opnieuw is geprobeerd. De taak opnieuw wordt gestart als deze wordt afgesloten met een andere waarde dan nul afsluitcode tot maximaal de opgegeven MaxTaskRetryCount|
