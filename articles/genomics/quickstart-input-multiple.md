---
title: 'Snelstartgids: een werkstroom verzenden met behulp van meerdere invoergegevens | Microsoft Docs'
titleSuffix: Azure
description: "In deze snelstartgids wordt ervan uitgegaan dat u de msgen-client hebt geïnstalleerd en de voorbeeldgegevens via de service hebt uitgevoerd."
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: quickstart
ms.date: 12/07/2017
ms.openlocfilehash: d410516f807b7914e15bed1fb93ee58d3e340d1e
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="submit-a-workflow-using-multiple-inputs-from-the-same-sample"></a>Een werkstroom verzenden met behulp van meerdere invoergegevens uit hetzelfde voorbeeld

In deze snelstartgids ziet u hoe u een werkstroom kunt verzenden naar de Microsoft Genomics-service wanneer uw invoerbestand bestaat uit meerdere FASTQ- of BAM-bestanden **die afkomstig zijn uit hetzelfde voorbeeld**. Houd er echter rekening mee dat u FASTQ- en BAM-bestanden **niet** kunt combineren in dezelfde verzending.

In dit onderwerp wordt ervan uitgegaan dat u de `msgen`-client al hebt geïnstalleerd en uitgevoerd, en dat u bekend bent met het gebruik van Azure Storage. Als u met behulp van de opgegeven voorbeeldgegevens een werkstroom hebt verzonden, bent u klaar om verder te gaan met deze snelstartgids. 


## <a name="multiple-bam-files"></a>Meerdere BAM-bestanden

### <a name="upload-your-input-files-to-azure-storage"></a>Uw invoerbestanden uploaden naar Azure Storage
Stel, u hebt meerdere BAM-bestanden als invoer, *reads.bam*, *additional_reads.bam* en *yet_more_reads.bam*, en u hebt deze geüpload naar uw opslagaccount *myaccount* in Azure. U hebt de URL van de API en uw toegangssleutel. U wilt de uitvoer in **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>** hebben.


### <a name="submit-your-job-to-the-msgen-client"></a>De taak naar de `msgen`-client verzenden 

U kunt meerdere BAM-bestanden verzenden door alle bijbehorende namen door te geven aan het argument --input-blob-name-1. Let op: alle bestanden moeten afkomstig zijn uit hetzelfde voorbeeld, maar de volgorde van de bestanden is niet van belang. Hieronder ziet u voorbeelden van verzendingen vanaf een opdrachtregel in Windows, in Unix en met behulp van een configuratiebestand. Voor de duidelijkheid zijn regeleinden toegevoegd:


Voor Windows:

```
msgen submit ^
  --api-url-base <Genomics API URL> ^
  --access-key <Genomics access key> ^
  --process-args R=b37m1 ^
  --input-storage-account-name myaccount ^
  --input-storage-account-key <storage access key to "myaccount"> ^
  --input-storage-account-container inputs ^
  --input-blob-name-1 reads.bam additional_reads.bam yet_more_reads.bam ^
  --output-storage-account-name myaccount ^
  --output-storage-account-key <storage access key to "myaccount"> ^
  --output-storage-account-container outputs
```


Voor Unix:

```
msgen submit \
  --api-url-base <Genomics API URL> \
  --access-key <Genomics access key> \
  --process-args R=b37m1 \
  --input-storage-account-name myaccount \
  --input-storage-account-key <storage access key to "myaccount"> \
  --input-storage-account-container inputs \
  --input-blob-name-1 reads.bam additional_reads.bam yet_more_reads.bam \
  --output-storage-account-name myaccount \
  --output-storage-account-key <storage access key to "myaccount"> \
  --output-storage-account-container outputs
```


Als u liever een configuratiebestand gebruikt, is dit wat het zou moeten bevatten:

``` config.txt
api_url_base:                     <Genomics API URL>
access_key:                       <Genomics access key>
process_args:                     R=b37m1
input_storage_account_name:       myaccount
input_storage_account_key:        <storage access key to "myaccount">
input_storage_account_container:  inputs
input_blob_name_1:                reads.bam additional_reads.bam yet_more_reads.bam
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

Verzend het `config.txt`-bestand met deze aanroep: `msgen submit -f config.txt`


## <a name="multiple-paired-fastq-files"></a>Meerdere gekoppelde FASTQ-bestanden

### <a name="upload-your-input-files-to-azure-storage"></a>Uw invoerbestanden uploaden naar Azure Storage
Stel, u hebt meerdere gekoppelde FASTQ-bestanden als invoer, *reads_1.fq.gz* en *reads_2.fq.gz*, *additional_reads_1.fq.gz* en *additional_ reads_2.fq.GZ*, en *yet_more_reads_1.fq.gz* en *yet_more_reads_2.fq.gz*. U hebt deze geüpload naar uw opslagaccount *myaccount* in Azure, en u hebt de URL van de API en uw toegangssleutel. U wilt de uitvoer in **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>** hebben.


### <a name="submit-your-job-to-the-msgen-client"></a>De taak naar de `msgen`-client verzenden 

Gekoppelde FASTQ-bestanden moeten niet alleen afkomstig zijn uit hetzelfde voorbeeld, maar ze moeten ook samen worden verwerkt.  De volgorde van de bestandsnamen is van belang wanneer ze als argumenten worden doorgegeven naar --input-blob-name-1 en --input-blob-name-2. 

Hieronder ziet u voorbeelden van verzendingen vanaf een opdrachtregel in Windows, in Unix en met behulp van een configuratiebestand. Voor de duidelijkheid zijn regeleinden toegevoegd:


Voor Windows:

```
msgen submit ^
  --api-url-base <Genomics API URL> ^
  --access-key <Genomics access key> ^
  --process-args R=b37m1 ^
  --input-storage-account-name myaccount ^
  --input-storage-account-key <storage access key to "myaccount"> ^
  --input-storage-account-container inputs ^
  --input-blob-name-1 reads_1.fastq.gz additional_reads_1.fastq.gz yet_more_reads_1.fastq.gz ^
  --input-blob-name-2 reads_2.fastq.gz additional_reads_2.fastq.gz yet_more_reads_2.fastq.gz ^
  --output-storage-account-name myaccount ^
  --output-storage-account-key <storage access key to "myaccount"> ^
  --output-storage-account-container outputs
```


Voor Unix:

```
msgen submit \
  --api-url-base <Genomics API URL> \
  --access-key <Genomics access key> \
  --process-args R=b37m1 \
  --input-storage-account-name myaccount \
  --input-storage-account-key <storage access key to "myaccount"> \
  --input-storage-account-container inputs \
  --input-blob-name-1 reads_1.fastq.gz additional_reads_1.fastq.gz yet_more_reads_1.fastq.gz \
  --input-blob-name-2 reads_2.fastq.gz additional_reads_2.fastq.gz yet_more_reads_2.fastq.gz \
  --output-storage-account-name myaccount \
  --output-storage-account-key <storage access key to "myaccount"> \
  --output-storage-account-container outputs
```


Als u liever een configuratiebestand gebruikt, is dit wat het zou moeten bevatten:

```
api_url_base:                     <Genomics API URL>
access_key:                       <Genomics access key>
process_args:                     R=b37m1
input_storage_account_name:       myaccount
input_storage_account_key:        <storage access key to "myaccount">
input_storage_account_container:  inputs
input_blob_name_1:                reads_1.fq.gz additional_reads_1.fastq.gz yet_more_reads_1.fastq.gz
input_blob_name_2:                reads_2.fq.gz additional_reads_2.fastq.gz yet_more_reads_2.fastq.gz
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

Verzend het `config.txt`-bestand met deze aanroep: `msgen submit -f config.txt`

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u meerdere BAM-bestanden of gekoppelde FASTQ-bestanden geüpload naar Azure Storage, en een werkstroom verzonden naar de Microsoft Genomics-service via de `msgen`-client voor Python. Zie onze [veelgestelde vragen](frequently-asked-questions-genomics.md) voor meer informatie over de verzending van werkstromen en andere opdrachten die u kunt gebruiken met de Microsoft Genomics-service. 