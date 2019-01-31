---
title: Azure Batch-taak startgebeurtenis | Microsoft Docs
description: Naslaginformatie voor Batch-taak gebeurtenis starten.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: d50a0a7082e409084fd966370934a638ca9bb013
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474425"
---
# <a name="task-start-event"></a>Gebeurtenis taak starten

 Deze gebeurtenis wordt verzonden zodra een taak is gepland om te starten op een rekenknooppunt door de scheduler. Houd er rekening mee dat als de taak wordt opnieuw geprobeerd of ingepland deze gebeurtenis opnieuw voor dezelfde taak, maar het aantal nieuwe pogingen verzonden wordt en Taakversie van het besturingssysteem wordt dienovereenkomstig bijgewerkt.


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

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|jobId|String|De id van de taak met de taak.|
|id|String|De id van de taak.|
|taskType|String|Het type van de taak. Dit kan worden 'Taakbeheer heeft' die wijzen op dat een jobbeheertaak is of 'User' is niet een jobbeheertaak aangeeft.|
|systemTaskVersion|Int32|Dit is de interne aantal nieuwe pogingen voor een taak. Intern kan de Batch-service opnieuw proberen een taak voor het account voor tijdelijke problemen. Deze problemen kunnen interne planning fouten bevatten of pogingen om te herstellen van de rekenknooppunten in een slechte status.|
|[nodeInfo](#nodeInfo)|Complex Type|Bevat informatie over de compute-knooppunt waarop de taak is uitgevoerd.|
|[multiInstanceSettings](#multiInstanceSettings)|Complex Type|Hiermee geeft u op dat de taak een taak met meerdere instanties waarvoor meerdere knooppunten vereist is.  Zie [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) voor meer informatie.|
|[constraints](#constraints)|Complex Type|De uitvoering van beperkingen die betrekking hebben op deze taak.|
|[executionInfo](#executionInfo)|Complex Type|Bevat informatie over het uitvoeren van de taak.|

###  <a name="nodeInfo"></a> nodeInfo

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|poolId|String|De id van de pool waarop de taak is uitgevoerd.|
|nodeId|String|De id van het knooppunt waarop de taak is uitgevoerd.|

###  <a name="multiInstanceSettings"></a> multiInstanceSettings

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|numberOfInstances|Int|Het aantal rekenknooppunten dat is vereist voor de taak.|

###  <a name="constraints"></a> Beperkingen

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Het maximum aantal keren dat de taak kan opnieuw worden uitgevoerd. De Batch-service probeert een taak opnieuw als de afsluitcode aan nul is.<br /><br /> Houd er rekening mee dat deze waarde specifiek het aantal nieuwe pogingen bepaalt. De Batch-service probeert de taak één keer, en vervolgens wordt opnieuw uitgevoerd tot aan deze limiet. Bijvoorbeeld, als het maximale aantal pogingen maximaal 3, Batch probeert een taak is 4 een time-out (één initiële probeer en 3 nieuwe pogingen).<br /><br /> Als het maximale aantal pogingen 0 is, probeert taken niet in de Batch-service opnieuw.<br /><br /> Als het maximale aantal pogingen -1 is, probeert taken zonder beperking in de Batch-service opnieuw.<br /><br /> De standaardwaarde is 0 (geen nieuwe pogingen).|

###  <a name="executionInfo"></a> executionInfo

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|retryCount|Int32|Het aantal keren dat die de taak door de Batch-service opnieuw is geprobeerd. De taak wordt opnieuw uitgevoerd als deze wordt afgesloten met een andere afsluitcode, tot aan de opgegeven MaxTaskRetryCount|
