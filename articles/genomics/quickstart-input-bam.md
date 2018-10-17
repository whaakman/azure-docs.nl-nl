---
title: 'Snelstartgids: een werkstroom verzenden met behulp van een BAM-bestandsinvoer - Microsoft Genomics'
titleSuffix: Azure
description: In deze snelstartgids wordt ervan uitgegaan dat u de msgen-client hebt geïnstalleerd en de voorbeeldgegevens via de service hebt uitgevoerd.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: quickstart
ms.date: 12/07/2017
ms.openlocfilehash: 91bc13dbf024df20ac23ca0ff83a73ed246dce28
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729501"
---
# <a name="submit-a-workflow-using-a-bam-file-input"></a>Een werkstroom verzenden met behulp van een BAM-bestandsinvoer

In deze snelstartgids wordt getoond hoe u een werkstroom verzendt naar de Microsoft Genomics-service als uw invoerbestand een enkel BAM-bestand is. In dit onderwerp wordt ervan uitgegaan dat u de `msgen`-client al hebt geïnstalleerd en uitgevoerd, en dat u bekend bent met het gebruik van Azure Storage. Als u met behulp van de opgegeven voorbeeldgegevens een werkstroom hebt verzonden, bent u klaar om verder te gaan met deze snelstartgids. 

## <a name="set-up-upload-your-bam-file-to-azure-storage"></a>Instellen: uw BAM-bestand uploaden naar Azure Storage
Stel dat u één BAM bestand, *reads.bam*, hebt en dat u het naar uw opslagaccount *myaccount* in Azure hebt geüpload als **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/inputs/reads<span></span>.bam<span></span>**. U hebt de URL van de API en uw toegangssleutel. U wilt de uitvoer in **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>** hebben.



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
  --input-blob-name-1 reads.bam ^
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
  --input-blob-name-1 reads.bam \
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
input_blob_name_1:                reads.bam
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

Verzend het `config.txt`-bestand met deze aanroep: `msgen submit -f config.txt`

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u een BAM-bestand geüpload naar Azure Storage, en een werkstroom verzonden naar de Microsoft Genomics-service via de `msgen`-client voor Python. Zie onze [veelgestelde vragen](frequently-asked-questions-genomics.md) voor meer informatie over de verzending van werkstromen en andere opdrachten die u kunt gebruiken met de Microsoft Genomics-service. 
