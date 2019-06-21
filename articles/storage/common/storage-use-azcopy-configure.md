---
title: Configureer, optimaliseren en oplossen van AzCopy met Azure Storage | Microsoft Docs
description: Configureer, optimaliseren en oplossen van AzCopy.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 1a67846889b43d582a7a7d477a33f0e2168fd760
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147855"
---
# <a name="configure-optimize-and-troubleshoot-azcopy"></a>Configureer, optimaliseren en oplossen van AzCopy

AzCopy is een opdrachtregelprogramma waarmee u kunt blobs of bestanden kopiëren naar of van een storage-account. In dit artikel helpt u bij het uitvoeren van geavanceerde configuratietaken en helpt u bij het oplossen van problemen die optreden kunnen als u AzCopy gebruiken.

> [!NOTE]
> Als u inhoud kunt u aan de slag met AzCopy zoekt, ziet u een van de volgende artikelen:
> - [Aan de slag met AzCopy](storage-use-azcopy-v10.md)
> - [Gegevens overdragen met AzCopy en blob-opslag](storage-use-azcopy-blobs.md)
> - [Gegevens overdragen met AzCopy en bestandsopslag](storage-use-azcopy-files.md)
> - [Gegevens overdragen met AzCopy en Amazon S3 buckets](storage-use-azcopy-s3.md)

## <a name="configure-proxy-settings"></a>Proxy-instellingen configureren

Voor het configureren van de proxy-instellingen voor AzCopy, stel de `https_proxy` omgevingsvariabele.

| Besturingssysteem | Opdracht  |
|--------|-----------|
| **Windows** | In het gebruik van een opdrachtprompt: `set https_proxy=<proxy IP>:<proxy port>`<br> In het gebruik van PowerShell: `$env:https_proxy="<proxy IP>:<proxy port>"`|
| **Linux** | `export https_proxy=<proxy IP>:<proxy port>` |
| **MacOS** | `export https_proxy=<proxy IP>:<proxy port>` |

AzCopy ondersteunt op dit moment geen proxy's waarvoor verificatie met NTLM of Kerberos.

## <a name="optimize-throughput"></a>Optimaliseren van doorvoer

Stel de `AZCOPY_CONCURRENCY_VALUE` omgevingsvariabele te configureren van het aantal gelijktijdige aanvragen, en voor het beheren van het verbruik van prestatie- en doorvoer. Als uw computer minder dan 5 CPU's is, wordt de waarde van deze variabele is ingesteld op `32`. De standaardwaarde is anders, gelijk zijn aan 16 vermenigvuldigd met het aantal CPU's. De maximale standaardwaarde van deze variabele is `300`, maar u kunt deze waarde hoger of lager handmatig instellen.

| Besturingssysteem | Opdracht  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Linux** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **MacOS** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

Gebruik de `azcopy env` om te controleren of de huidige waarde van deze variabele.  Als de waarde leeg is, dan zal de `AZCOPY_CONCURRENCY_VALUE` variabele is ingesteld op de standaardwaarde van `300`.

## <a name="change-the-location-of-the-log-files"></a>Wijzig de locatie van de logboekbestanden

Standaard logboekbestanden bevinden zich in de `%USERPROFILE\\.azcopy` Active directory op Windows of in de `$HOME\\.azcopy` Active directory op Mac en Linux. U kunt deze locatie kunt wijzigen als u wilt met behulp van deze opdrachten.

| Besturingssysteem | Opdracht  |
|--------|-----------|
| **Windows** | `set AZCOPY_LOG_LOCATION=<value>` |
| **Linux** | `export AZCOPY_LOG_LOCATION=<value>` |
| **MacOS** | `export AZCOPY_LOG_LOCATION=<value>` |

Gebruik de `azcopy env` om te controleren of de huidige waarde van deze variabele. Als de waarde leeg is, worden de logboeken geschreven naar de standaardlocatie.

## <a name="change-the-default-log-level"></a>Het standaardniveau wijzigen

AzCopy logboek-niveau is standaard ingesteld op `INFO`. Als u wilt beperken, het detailniveau van logboekbestanden om schijfruimte te besparen, deze instelling overschrijven met behulp van de ``--log-level`` optie. 

Beschikbare logboekniveaus zijn: `DEBUG`, `INFO`, `WARNING`, `ERROR`, `PANIC`, en `FATAL`.

## <a name="troubleshoot-issues"></a>Problemen oplossen

AzCopy logboek- en plan bestanden voor elke taak gemaakt. U kunt de logboeken kunt gebruiken om te onderzoeken en oplossen van mogelijke problemen. 

De logboeken bevat de status van de fout (`UPLOADFAILED`, `COPYFAILED`, en `DOWNLOADFAILED`), het volledige pad en de reden van de fout.

Standaard het logboek- en plan-bestanden bevinden zich in de `%USERPROFILE\\.azcopy` map op Windows of `$HOME\\.azcopy` Active directory op Mac en Linux.

> [!IMPORTANT]
> Bij het indienen van een aanvraag voor het Microsoft Support (of het probleem met betrekking tot een derde partij op te lossen), delen de geredigeerde versie van de opdracht die u wilt uitvoeren. Dit zorgt ervoor dat de SAS niet per ongeluk worden gedeeld met iedereen. U vindt de geredigeerde versie aan het begin van het logboekbestand.

### <a name="review-the-logs-for-errors"></a>Bekijk de logboeken voor fouten

De volgende opdracht krijgt alle fouten met `UPLOADFAILED` status van de `04dc9ca9-158f-7945-5933-564021086c79` logboek:

**Windows**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**Linux**

```
grep UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

### <a name="view-and-resume-jobs"></a>Weergeven en taken hervatten

Elke overdrachtbewerking wordt een AzCopy-taak maken. Gebruik de volgende opdracht om de geschiedenis van taken weer te geven:

```
azcopy jobs list
```

Als u wilt weergeven van statistieken voor de taak, gebruik de volgende opdracht:

```
azcopy jobs show <job-id>
```

Als u wilt filteren de overdrachten op status, gebruik de volgende opdracht:

```
azcopy jobs show <job-id> --with-status=Failed
```

Gebruik de volgende opdracht uit om te hervatten van een taak is mislukt/geannuleerd. Met deze opdracht maakt gebruik van de id samen met het SAS-token niet permanent uit veiligheidsoverwegingen is:

```
azcopy jobs resume <job-id> --source-sas="<sas-token>"
azcopy jobs resume <job-id> --destination-sas="<sas-token>"
```

Wanneer u een taak hervatten, AzCopy het plan taakbestand gekeken. Het plan-bestand geeft een lijst van alle bestanden die zijn geïdentificeerd voor de verwerking van wanneer de taak voor het eerst is gemaakt. Als u een taak hervatten, probeert AzCopy om over te dragen van alle bestanden die worden vermeld in het plan-bestand dat al was niet overgedragen.