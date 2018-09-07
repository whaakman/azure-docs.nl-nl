---
title: Over de resources van Batch AI - Azure | Microsoft Docs
description: Overzicht van werkruimten, clusters, bestandsservers, experimenten en taken in de Batch AI-service in Microsoft Azure.
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/15/2018
ms.author: danlep
ms.openlocfilehash: 4a9e3529f9d68ecdc614ea69cffc6897891f4548
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057112"
---
# <a name="overview-of-resources-in-batch-ai"></a>Overzicht van bronnen in de Batch AI

Wanneer u met behulp van de Batch AI-service eerst, moet u informatie over de beschikbare resources van Batch AI. Als u met andere Azure-services in een of meer Azure Batch AI-resources maakt *resourcegroepen*. Maken van een of meer Batch AI *werkruimten* in een resourcegroep. Elke werkruimte bevat een combinatie van Batch AI *clusters*, *bestandsservers*, en *experimenten*. Een Batch AI-experiment ingekapseld in een groep *taken*.

De volgende afbeelding toont een voorbeeld van de resource-hiërarchie voor Batch AI. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

De volgende secties gaat u meer informatie over de resources van Batch AI.

## <a name="workspace"></a>Werkruimte

Een werkruimte in de Batch AI is een verzameling op het hoogste niveau van de rest van de Batch AI-resources. Werkruimten te scheiden van werk die behoren tot verschillende groepen of projecten. U kunt bijvoorbeeld een ontwikkelings- en een test-werkruimte maken.

## <a name="cluster"></a>Cluster

Een Batch AI-cluster bevat de rekenresources voor het uitvoeren van taken. Alle knooppunten in een cluster hebben dezelfde VM-grootte en de installatiekopie van het besturingssysteem. Batch AI biedt veel opties voor het maken van clusters die zijn aangepast aan verschillende behoeften. Normaal gesproken instellen van een ander cluster voor elke categorie van de verwerkingscapaciteit die nodig zijn om een project te voltooien. De Batch AI-clusters omhoog en omlaag schalen op basis van vraag en budget. Zie voor meer informatie, [werken met Batch AI-clusters](clusters.md).

## <a name="file-server"></a>Bestandsserver

U kunt desgewenst een bestandsserver maken in Batch AI voor het opslaan van gegevens, scripts, training en logboeken worden gemaakt. Een Batch AI-bestandsserver is een beheerde NFS één knooppunt, die automatisch kan worden gekoppeld op clusterknooppunten voor een eenvoudige en centraal toegankelijk opslaglocatie voor taken. Server slechts één bestand in een werkruimte is nodig voor de meeste gevallen en kunt u gegevens voor uw trainingstaken scheiden in verschillende mappen. Als NFS niet geschikt is voor uw workloads, Batch AI biedt ondersteuning voor andere opslagopties, waaronder [Azure Storage](use-azure-storage.md) of aangepaste oplossingen, zoals een bestandssysteem Gluster of Lustre.

## <a name="experiment"></a>Experiment

Een experiment gegroepeerd op een verzameling van gerelateerde taken die u query's uitvoeren en samen kunnen worden beheerd. Elke werkruimte mogelijk meerdere experimenten, waarbij elke experiment probeert om een specifiek probleem te verhelpen.

## <a name="job"></a>Taak

Een taak wordt een enkele taak of een script dat worden uitgevoerd moet, bijvoorbeeld om een deep learning-model te trainen. Elke taak wordt een specifiek script uitgevoerd op een cluster in de werkruimte. (Het script kan worden opgeslagen op een Batch AI-bestandsserver of andere opslagoplossing voor.) Elke Batch AI-taak heeft een framework-type dat is gekoppeld: TensorFlow, Horovod, CNTK, Caffe, Caffe2, pyTorch, Chainer, aangepaste MPI of aangepast. Voor elk framework, de Batch AI-service stelt u de vereiste infrastructuur en de taak processen beheren. Elke experiment kan meerdere taken die vergelijkbaar, afgezien van een aantal wijzigingen aan verschillende parameters zijn hebben.

## <a name="next-steps"></a>Volgende stappen

* Voer uw eerste [Batch AI-trainingstaak](quickstart-tensorflow-training-cli.md).

* Bekijk voorbeelden van [trainingsrecepten](https://github.com/Azure/BatchAI/tree/master/recipes) voor verschillende frameworks.