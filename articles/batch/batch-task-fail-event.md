---
title: Azure Batch-taak mislukt gebeurtenis | Microsoft Docs
description: Naslaginformatie voor Batch-taak mislukt gebeurtenis.
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
ms.openlocfilehash: f37769ceb761b8c8bc4834568813bb1b7af7f66a
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55458989"
---
# <a name="task-fail-event"></a>Gebeurtenis taak mislukt

 Deze gebeurtenis wordt verzonden wanneer een taak is voltooid met een fout. Op dit moment fouten worden beschouwd als alle andere afsluitcodes. Deze gebeurtenis wordt verzonden *naast* een taak voltooien van de gebeurtenis en kan worden gebruikt om te detecteren wanneer een taak is mislukt.


 Het volgende voorbeeld ziet de instantie van een taak mislukt gebeurtenis.

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
        "exitCode": 1,
        "retryCount": 2,
        "requeueCount": 0
    }
}
```

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|jobId|String|De id van de taak met de taak.|
|id|String|De id van de taak.|
|taskType|String|Het type van de taak. Dit kan worden 'Taakbeheer heeft' die wijzen op dat een jobbeheertaak is of 'User' is niet een jobbeheertaak aangeeft. Deze gebeurtenis is niet verzonden voor jobvoorbereidingstaken, jobvrijgevingstaken of Begintaken.|
|systemTaskVersion|Int32|Dit is de interne aantal nieuwe pogingen voor een taak. Intern kan de Batch-service opnieuw proberen een taak voor het account voor tijdelijke problemen. Deze problemen kunnen interne planning fouten bevatten of pogingen om te herstellen van de rekenknooppunten in een slechte status.|
|[nodeInfo](#nodeInfo)|Complex Type|Bevat informatie over de compute-knooppunt waarop de taak is uitgevoerd.|
|[multiInstanceSettings](#multiInstanceSettings)|Complex Type|Geeft aan dat de taak een taak met meerdere instanties waarvoor meerdere knooppunten vereist.  Zie [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) voor meer informatie.|
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
|numberOfInstances|Int32|Het aantal rekenknooppunten dat is vereist voor de taak.|

###  <a name="constraints"></a> Beperkingen

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Het maximum aantal keren dat de taak kan opnieuw worden uitgevoerd. De Batch-service probeert een taak opnieuw als de afsluitcode aan nul is.<br /><br /> Houd er rekening mee dat deze waarde specifiek het aantal nieuwe pogingen bepaalt. De Batch-service probeert de taak één keer, en vervolgens wordt opnieuw uitgevoerd tot aan deze limiet. Bijvoorbeeld, als het maximale aantal pogingen maximaal 3, Batch probeert een taak is 4 een time-out (één initiële probeer en 3 nieuwe pogingen).<br /><br /> Als het maximale aantal pogingen 0 is, probeert taken niet in de Batch-service opnieuw.<br /><br /> Als het maximale aantal pogingen -1 is, probeert taken zonder beperking in de Batch-service opnieuw.<br /><br /> De standaardwaarde is 0 (geen nieuwe pogingen).|


###  <a name="executionInfo"></a> executionInfo

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|startTime|DateTime|De tijd waarbinnen de taak is gestart. 'Actief' komt overeen met de **met** staat, dus als de taak wordt opgegeven bestanden of toepassingspakketten, klikt u vervolgens de begintijd weerspiegelt de tijd waarop de taak downloadt of implementeren van deze gestart.  Als de taak is gestart of opnieuw uitgevoerd, is dit de meest recente tijdstip waarop de taak is gestart.|
|endTime|DateTime|De tijd waarbinnen de taak is voltooid.|
|exitCode|Int32|De afsluitcode van de taak.|
|retryCount|Int32|Het aantal keren dat die de taak door de Batch-service opnieuw is geprobeerd. De taak wordt opnieuw uitgevoerd als deze wordt afgesloten met een andere afsluitcode, tot aan de opgegeven MaxTaskRetryCount.|
|requeueCount|Int32|Het aantal keren dat die de taak is door de Batch-service is ingepland als gevolg van een gebruikersaanvraag.<br /><br /> Wanneer de knooppunten van de gebruiker wordt verwijderd uit een groep (op basis van het formaat of verkleinen van de pool) of wanneer de taak wordt uitgeschakeld, de gebruiker kan opgeven dat wordt uitgevoerd op de knooppunten taken worden ingepland voor uitvoering. Dit aantal worden gevolgd hoe vaak de taak heeft zijn ingepland voor deze redenen.|
