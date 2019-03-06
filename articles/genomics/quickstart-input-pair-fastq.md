---
title: Een werkstroom met behulp van FASTQ-bestandsinvoeren - Microsoft Genomics verzenden
titleSuffix: Azure
description: In dit artikel wordt ervan uitgegaan dat u hebt de msgen-client is geïnstalleerd en de voorbeeldgegevens via de service hebt uitgevoerd.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: conceptual
ms.date: 12/07/2017
ms.openlocfilehash: 2662a8f52c58a39916e5789fa9ed7fadd91216c0
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57438802"
---
# <a name="submit-a-workflow-using-fastq-file-inputs-in-microsoft-genomics"></a>Een werkstroom verzenden met behulp van FASTQ-bestandsinvoer in Microsoft Genomics

In dit artikel ziet u hoe u een werkstroom verzendt naar de service Microsoft Genomics als uw invoerbestanden een tweetal FASTQ-bestanden. In dit onderwerp wordt ervan uitgegaan dat u de `msgen`-client al hebt geïnstalleerd en uitgevoerd, en dat u bekend bent met het gebruik van Azure Storage. Als u een werkstroom met de opgegeven voorbeeldgegevens is verzonden, bent u klaar om door te gaan met dit artikel. 

## <a name="set-up-upload-your-fastq-files-to-azure-storage"></a>Instellen: Uw FASTQ-bestanden uploaden naar Azure storage
Stel dat u twee bestanden hebt, *reads_1.fq.gz* en *reads_2.fq.gz*, en u ze hebt geüpload naar uw opslagaccount *myaccount* in Azure als **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/inputs/reads_1<span></span>.fq<span></span>.gz<span></span>** en **https://<span></span>myaccount.blob.core.<span></span>windows<span></span>.net/<span></span>inputs/<span></span>reads_2.fq<span></span>.gz<span></span>**. U hebt de URL van de API en uw toegangssleutel. U wilt de uitvoer in **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>** hebben.


## <a name="submit-your-job-to-the-msgen-client"></a>De taak naar de `msgen`-client verzenden 

Dit is de minimale set argumenten die u dient op te geven voor de `msgen`-client; voor de duidelijkheid zijn regeleinden toegevoegd:

Voor Windows:

```
msgen submit ^
  --api-url-base <Genomics API URL> ^
  --access-key <Genomics access key> ^
  --process-args R=b37m1 ^
  --input-storage-account-name myaccount ^
  --input-storage-account-key <storage access key to "myaccount"> ^
  --input-storage-account-container inputs ^
  --input-blob-name-1 reads_1.fq.gz ^
  --input-blob-name-2 reads_2.fq.gz ^
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
  --input-blob-name-1 reads_1.fq.gz \
  --input-blob-name-2 reads_2.fq.gz \
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
input_blob_name_1:                reads_1.fq.gz
input_blob_name_2:                reads_2.fq.gz
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

Verzend het `config.txt`-bestand met deze aanroep: `msgen submit -f config.txt`

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u twee FASTQ-bestanden geüpload naar Azure Storage, en een werkstroom verzonden naar de Microsoft Genomics-service via de `msgen`-client voor Python. Zie voor meer informatie over de verzending van werkstromen en andere opdrachten die u met de service Microsoft Genomics gebruiken kunt, onze [Veelgestelde vragen over](frequently-asked-questions-genomics.md). 
