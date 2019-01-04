---
title: Informatie over prijzen voor Azure Data Factory met behulp van voorbeelden | Microsoft Docs
description: In dit artikel wordt beschreven en ziet u het prijsmodel met gedetailleerde voorbeelden van Azure Data Factory
documentationcenter: ''
author: shlo
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: shlo
ms.openlocfilehash: ec5ebfa29af13b5a34617be7f423fd7bc220636b
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017749"
---
# <a name="understanding-data-factory-pricing-through-examples"></a>Informatie over prijzen van Data Factory met behulp van voorbeelden

In dit artikel wordt uitgelegd en ziet u het prijsmodel met gedetailleerde voorbeelden van Azure Data Factory.

## <a name="copy-data-from-aws-s3-to-azure-blob-storage-hourly"></a>Gegevens kopiëren van AWS S3 naar Azure Blob-opslag per uur

In dit scenario wilt u gegevens kopiëren van AWS S3 naar Azure Blob-opslag op een planning per uur.

Om uit te voeren van het scenario, moet u een pijplijn maken met de volgende items:

1. Een kopieeractiviteit met een invoergegevensset voor de gegevens moeten worden gekopieerd van AWS S3.

2. Een uitvoergegevensset voor de gegevens in Azure Storage.

3. Een schematrigger het uitvoeren van de pijplijn elk uur.

 ![Scenario1](media/pricing-concepts/scenario1.png)

| **Bewerkingen** | **Typen en eenheden** |
| --- | --- |
| Gekoppelde Service maken | 2 voor lezen/schrijven-entiteit  |
| Gegevenssets maken | 4 lezen/schrijven-entiteiten (2 voor het maken van de gegevensset, 2 voor de gekoppelde service verwijst naar) |
| Pijplijn maken | 3 voor lezen/schrijven-entiteiten (1 voor het maken van de pijplijn, 2 voor gegevensset verwijzingen) |
| Pijplijn ophalen | 1 lezen/schrijven-entiteit |
| Pijplijn uitvoeren | 2 uitvoeringen van activiteit (1 voor de trigger uitvoeren, 1 voor uitvoeringen van activiteit) |
| Kopieer gegevens veronderstelling: uitvoeringstijd = 10 minuten | 10 \* 4 azure Integratieruntime (DIU standaardinstelling = 4) Zie voor meer informatie over integratie gegevenseenheden en optimaliseren kopieerprestaties [in dit artikel](copy-activity-performance.md) |
| Monitor pijplijn veronderstelling: Alleen 1 uitvoeren opgetreden | 2 bewaking uitvoeren records opnieuw geprobeerd (1 voor pijplijn-run, 1 voor de activiteit die wordt uitgevoerd) |

**Totaal aantal Scenario prijzen: $0.16811**

- Data Factory Operations = **$0,0001**
  - Lezen/schrijven = 10\*00001 $0,0001 = [1 R/W = $ 0,50/50000 0,00001 =]
  - Bewaking = 2\*000005 $0,00001 = [1 bewaking = $ 0,25/50000 = 0.000005]
- Pipeline-Orchestration &amp; uitvoering = **$0.168**
  - Uitvoeringen van activiteit = 001\*2 = 0,002 [1 uitvoeren = $1/1000 0,001 =]
  - Activiteiten voor gegevensverplaatsing = $0.166 (tijdens 10 minuten van de uitvoeringstijd. $0,25 per uur op Azure Integration Runtime)

## <a name="copy-data-and-transform-with-azure-databricks-hourly"></a>Gegevens kopiëren en transformeren met Azure Databricks per uur

In dit scenario, die u wilt kopiëren van gegevens van AWS S3 naar Azure Blob-opslag en de gegevens transformeren met Azure Databricks op een planning per uur.

Om uit te voeren van het scenario, moet u een pijplijn maken met de volgende items:

1. Een kopieeractiviteit met een invoergegevensset voor de gegevens moeten worden gekopieerd van AWS S3 en een uitvoergegevensset voor de gegevens in Azure storage.
2. Een Azure Databricks-activiteit voor de gegevenstransformatie.
3. Het uitvoeren van de pijplijn elk uur schematrigger een.

![Scenario2](media/pricing-concepts/scenario2.png)

| **Bewerkingen** | **Typen en eenheden** |
| --- | --- |
| Gekoppelde Service maken | 3 voor lezen/schrijven-entiteit  |
| Gegevenssets maken | 4 lezen/schrijven-entiteiten (2 voor het maken van de gegevensset, 2 voor de gekoppelde service verwijst naar) |
| Pijplijn maken | 3 voor lezen/schrijven-entiteiten (1 voor het maken van de pijplijn, 2 voor gegevensset verwijzingen) |
| Pijplijn ophalen | 1 lezen/schrijven-entiteit |
| Pijplijn uitvoeren | 3 uitvoeringen van activiteit (1 voor de trigger uitvoeren, 2 voor uitvoeringen van activiteit) |
| Kopieer gegevens veronderstelling: uitvoeringstijd = 10 minuten | 10 \* 4 azure Integratieruntime (DIU standaardinstelling = 4) Zie voor meer informatie over integratie gegevenseenheden en optimaliseren kopieerprestaties [in dit artikel](copy-activity-performance.md) |
| Monitor pijplijn veronderstelling: Alleen 1 uitvoeren opgetreden | 3 bewaking uitvoeren records opnieuw geprobeerd (1 voor pijplijn-run, 2 voor de activiteit die wordt uitgevoerd) |
| Databricks-activiteit veronderstelling uitvoeren: uitvoeringstijd = 10 minuten | Externe Pijplijnuitvoering activiteit 10 minuten |

**Totaal aantal Scenario prijzen: $0.16916**

- Data Factory Operations = **$0.00012**
  - Lezen/schrijven = 11\*00001 $0.00011 = [1 R/W = $ 0,50/50000 0,00001 =]
  - Bewaking = 3\*000005 $0,00001 = [1 bewaking = $ 0,25/50000 = 0.000005]
- Pipeline-Orchestration &amp; uitvoering = **$0.16904**
  - Uitvoeringen van activiteit = 001\*3 = 0,003 [1 uitvoeren = $1/1000 0,001 =]
  - Activiteiten voor gegevensverplaatsing = $0.166 (tijdens 10 minuten van de uitvoeringstijd. $0,25 per uur op Azure Integration Runtime)
  - Externe Pipeline-activiteit = $0.000041 (tijdens 10 minuten van de uitvoeringstijd. $0.00025/uur op Azure Integration Runtime)

## <a name="copy-data-and-transform-with-dynamic-parameters-hourly"></a>Gegevens kopiëren en transformeren met dynamische parameters per uur

In dit scenario wilt u gegevens van AWS S3 kopiëren naar Azure Blob-opslag en transformeren met Azure Databricks (met dynamische parameters in het script) op een planning per uur.

Om uit te voeren van het scenario, moet u een pijplijn maken met de volgende items:

1. Een kopieeractiviteit met een invoergegevensset voor de gegevens van AWS S3, een uitvoergegevensset voor de gegevens in Azure storage worden gekopieerd.
2. Een Lookup-activiteit voor het doorgeven van parameters dynamisch aan de transformatiescript.
3. Een Azure Databricks-activiteit voor de gegevenstransformatie.
4. Het uitvoeren van de pijplijn elk uur schematrigger een.

![Scenario3](media/pricing-concepts/scenario3.png)

| **Bewerkingen** | **Typen en eenheden** |
| --- | --- |
| Gekoppelde Service maken | 3 voor lezen/schrijven-entiteit  |
| Gegevenssets maken | 4 lezen/schrijven-entiteiten (2 voor het maken van de gegevensset, 2 voor de gekoppelde service verwijst naar) |
| Pijplijn maken | 3 voor lezen/schrijven-entiteiten (1 voor het maken van de pijplijn, 2 voor gegevensset verwijzingen) |
| Pijplijn ophalen | 1 lezen/schrijven-entiteit |
| Pijplijn uitvoeren | 4 uitvoeringen van activiteit (1 voor de trigger uitvoeren, 3 voor uitvoeringen van activiteit) |
| Kopieer gegevens veronderstelling: uitvoeringstijd = 10 minuten | 10 \* 4 azure Integratieruntime (DIU standaardinstelling = 4) Zie voor meer informatie over integratie gegevenseenheden en optimaliseren kopieerprestaties [in dit artikel](copy-activity-performance.md) |
| Monitor pijplijn veronderstelling: Alleen 1 uitvoeren opgetreden | 4 bewaking uitvoeren records opnieuw geprobeerd (1 voor pijplijn-run, 3 voor de activiteit die wordt uitgevoerd) |
| Activiteit Lookup veronderstelling uitvoeren: uitvoeringstijd = 1 minuut | 1 minuut Pipeline-activiteit uitvoeren |
| Databricks-activiteit veronderstelling uitvoeren: uitvoeringstijd = 10 minuten | 10 minuten tot de uitvoering van externe Pipeline-activiteit |

**Totaal aantal Scenario prijzen: $0.17020**

- Data Factory Operations = **$0.00013**
  - Lezen/schrijven = 11\*00001 $0.00011 = [1 R/W = $ 0,50/50000 0,00001 =]
  - Controle 4 =\*000005 $0.00002 = [1 bewaking = $ 0,25/50000 0.000005 =]
- Pipeline-Orchestration &amp; uitvoering = **$0.17007**
  - Uitvoeringen van activiteit = 001\*4 = 0.004 [1 uitvoeren = $1/1000 0,001 =]
  - Activiteiten voor gegevensverplaatsing = $0.166 (tijdens 10 minuten van de uitvoeringstijd. $0,25 per uur op Azure Integration Runtime)
  - Pipeline-activiteit = $0,00003 (tijdens voor 1 minuut van de uitvoeringstijd. $ 0,002/uur op Azure Integration Runtime)
  - Externe Pipeline-activiteit = $0.000041 (tijdens 10 minuten van de uitvoeringstijd. $0.00025/uur op Azure Integration Runtime)

## <a name="next-steps"></a>Volgende stappen

Nu dat u begrijpt dat de prijzen voor Azure Data Factory, u kunt aan de slag!

- [Een data factory maken met behulp van de Azure Data Factory-UI](quickstart-create-data-factory-portal.md)

- [Inleiding tot Azure Data Factory](introduction.md)

- [Visueel ontwerpen in Azure Data Factory](author-visually.md)