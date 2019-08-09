---
title: AzCopy configureren, optimaliseren en problemen oplossen met Azure Storage | Microsoft Docs
description: AzCopy configureren, optimaliseren en problemen oplossen.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: 8a96c5b2d39967c8ee82f48e880bac9270a58c36
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68844796"
---
# <a name="configure-optimize-and-troubleshoot-azcopy"></a>AzCopy configureren, optimaliseren en problemen oplossen

AzCopy is een opdracht regel programma dat u kunt gebruiken voor het kopiëren van blobs of bestanden naar of van een opslag account. Dit artikel helpt u bij het uitvoeren van geavanceerde configuratie taken en helpt u bij het oplossen van problemen die kunnen optreden tijdens het gebruik van AzCopy.

> [!NOTE]
> Als u op zoek bent naar inhoud die u helpt aan de slag te gaan met AzCopy, raadpleegt u een van de volgende artikelen:
> - [Aan de slag met AzCopy](storage-use-azcopy-v10.md)
> - [Gegevens overdragen met AzCopy en Blob Storage](storage-use-azcopy-blobs.md)
> - [Gegevens overdragen met AzCopy en File Storage](storage-use-azcopy-files.md)
> - [Gegevens overdragen met AzCopy en Amazon S3-buckets](storage-use-azcopy-s3.md)

## <a name="configure-proxy-settings"></a>Proxy-instellingen configureren

Stel de `https_proxy` omgevings variabele in om de proxy-instellingen voor AzCopy te configureren. Als u AzCopy uitvoert in Windows, detecteert AzCopy automatisch proxy-instellingen. u hoeft deze instelling niet te gebruiken in Windows. Als u kiest voor het gebruik van deze instelling in Windows, wordt automatische detectie vervangen.

| Besturingssysteem | Opdracht  |
|--------|-----------|
| **Windows** | In een opdracht prompt gebruikt u:`set https_proxy=<proxy IP>:<proxy port>`<br> In Power shell gebruikt u:`$env:https_proxy="<proxy IP>:<proxy port>"`|
| **Linux** | `export https_proxy=<proxy IP>:<proxy port>` |
| **MacOS** | `export https_proxy=<proxy IP>:<proxy port>` |

Momenteel biedt AzCopy geen ondersteuning voor proxy's waarvoor authenticatie met NTLM of Kerberos is vereist.

## <a name="optimize-throughput"></a>Door Voer optimaliseren

U kunt de `cap-mbps` vlag gebruiken om een plafond te plaatsen op basis van het gegevens aantal door voer. Met de volgende opdracht wordt bijvoorbeeld een hoofd letter `10` door voer naar megabits (MB) per seconde.

```azcopy
azcopy cap-mbps 10
```

De door Voer kan afnemen bij het overbrengen van kleine bestanden. U kunt de door Voer verhogen door de `AZCOPY_CONCURRENCY_VALUE` omgevings variabele in te stellen. Met deze variabele geeft u het aantal gelijktijdige aanvragen op dat kan worden uitgevoerd.  Als uw computer minder dan 5 Cpu's heeft, wordt de waarde van deze variabele ingesteld op `32`. Anders is de standaard waarde gelijk aan 16 vermenigvuldigd met het aantal Cpu's. De maximale standaard waarde van deze variabele is `300`, maar u kunt deze waarde ook op een hoger of lager niveau instellen.

| Besturingssysteem | Opdracht  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Linux** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **MacOS** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

Gebruik de `azcopy env` om de huidige waarde van deze variabele te controleren.  Als de waarde leeg is, wordt de `AZCOPY_CONCURRENCY_VALUE` variabele ingesteld op de standaard waarde van. `300`

## <a name="change-the-location-of-the-log-files"></a>De locatie van de logboek bestanden wijzigen

Logboek bestanden bevinden zich standaard in de `%USERPROFILE\\.azcopy` map in Windows of in de `$HOME\\.azcopy` map op Mac en Linux. U kunt deze locatie wijzigen als u deze nodig hebt met behulp van deze opdrachten.

| Besturingssysteem | Opdracht  |
|--------|-----------|
| **Windows** | `set AZCOPY_LOG_LOCATION=<value>` |
| **Linux** | `export AZCOPY_LOG_LOCATION=<value>` |
| **MacOS** | `export AZCOPY_LOG_LOCATION=<value>` |

Gebruik de `azcopy env` om de huidige waarde van deze variabele te controleren. Als de waarde leeg is, worden logboeken naar de standaard locatie geschreven.

## <a name="change-the-default-log-level"></a>Het standaard logboek niveau wijzigen

AzCopy-logboek niveau is standaard ingesteld op `INFO`. Als u de uitgebreidheid van het logboek wilt beperken om schijf ruimte te besparen, kunt u deze instelling overschrijven ``--log-level`` met behulp van de optie. 

Beschik bare logboek niveaus `DEBUG`zijn `INFO`: `WARNING`, `ERROR`, `PANIC`,, `FATAL`en.

## <a name="troubleshoot-issues"></a>Problemen oplossen

AzCopy maakt logboek-en plan bestanden voor elke taak. U kunt de Logboeken gebruiken om potentiële problemen te onderzoeken en op te lossen. 

De logboeken bevatten de status van fout (`UPLOADFAILED`, `COPYFAILED`, en `DOWNLOADFAILED`), het volledige pad en de reden van de fout.

De logboek-en plan bestanden bevinden zich standaard in de `%USERPROFILE\\.azcopy` map in Windows of `$HOME\\.azcopy` in de map op Mac en Linux.

> [!IMPORTANT]
> Wanneer u een aanvraag indient om Microsoft Ondersteuning (of het probleem met een derde partij op te lossen), deelt u de geredigeerde versie van de opdracht die u wilt uitvoeren. Dit zorgt ervoor dat de SAS niet per ongeluk met iedereen wordt gedeeld. U kunt de geredigeerde versie vinden aan het begin van het logboek bestand.

### <a name="review-the-logs-for-errors"></a>De logboeken controleren op fouten

Met `UPLOADFAILED` de volgende opdracht worden alle fouten uit het `04dc9ca9-158f-7945-5933-564021086c79` logboek opgehaald:

**Windows (Power shell)**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**Linux**

```
grep UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

### <a name="view-and-resume-jobs"></a>Taken weer geven en hervatten

Bij elke overdrachts bewerking wordt een AzCopy-taak gemaakt. Gebruik de volgende opdracht om de geschiedenis van taken weer te geven:

```
azcopy jobs list
```

Als u de taak statistieken wilt weer geven, gebruikt u de volgende opdracht:

```
azcopy jobs show <job-id>
```

Als u de overdrachten wilt filteren op status, gebruikt u de volgende opdracht:

```
azcopy jobs show <job-id> --with-status=Failed
```

Gebruik de volgende opdracht om een mislukte/geannuleerde taak te hervatten. Met deze opdracht wordt de id samen met het SAS-token gebruikt, omdat dit om veiligheids redenen niet permanent is:

```
azcopy jobs resume <job-id> --source-sas="<sas-token>"
azcopy jobs resume <job-id> --destination-sas="<sas-token>"
```

Wanneer u een taak hervat, zoekt AzCopy naar het job plan-bestand. In het plan bestand worden alle bestanden vermeld die zijn geïdentificeerd voor verwerking toen de taak voor het eerst werd gemaakt. Wanneer u een taak hervat, probeert AzCopy alle bestanden over te dragen die worden vermeld in het plan bestand dat niet al is overgedragen.