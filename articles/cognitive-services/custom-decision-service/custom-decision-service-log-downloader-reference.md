---
title: LogDownloader-Custom Decision Service
titlesuffix: Azure Cognitive Services
description: Down load de logboek bestanden die door Azure Custom Decision Service worden gemaakt.
services: cognitive-services
author: marco-rossi29
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: marossi
ROBOTS: NOINDEX
ms.openlocfilehash: 33cc0d0dcf16ff82ac128507566427e123020236
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707231"
---
# <a name="logdownloader"></a>LogDownloader

Down load de logboek bestanden die door Azure Custom Decision Service worden gemaakt en Genereer de *. gz* -bestanden die worden gebruikt door experimenten.

## <a name="prerequisites"></a>Vereisten

- Python 3: Geïnstalleerd en op uw pad. We raden u aan de 64-bits versie te verwerken om grote bestanden af te handelen.
- De *micro soft/mwt-DS-* opslag plaats: [Kloon de opslag plaats](https://github.com/Microsoft/mwt-ds).
- Het pakket *Azure-Storage-BLOB* : Voor installatie Details gaat u naar [Microsoft Azure Storage-bibliotheek voor python](https://github.com/Azure/azure-storage-python#option-1-via-pypi).
- Voer uw Azure Storage-connection string in *mwt-DS/DataScience/DS. config*in: Volg de *my_app_id: my_connectionString* -sjabloon. U kunt meerdere `app_id`opgeven. Wanneer u uitvoert `LogDownloader.py`en de invoer `app_id` niet wordt gevonden in `ds.config`, `LogDownloader.py` gebruikt de `$Default` Connection String.

## <a name="usage"></a>Gebruik

Ga naar `mwt-ds/DataScience` en voer `LogDownloader.py` uit met de relevante argumenten, zoals wordt beschreven in de volgende code:

```cmd
python LogDownloader.py [-h] -a APP_ID -l LOG_DIR [-s START_DATE]
                        [-e END_DATE] [-o OVERWRITE_MODE] [--dry_run]
                        [--create_gzip] [--delta_mod_t DELTA_MOD_T]
                        [--verbose] [-v VERSION]
```

### <a name="parameters"></a>Parameters

| Invoer | Description | Standaard |
| --- | --- | --- |
| `-h`, `--help` | Het Help-bericht weer geven en afsluiten. | |
| `-a APP_ID`, `--app_id APP_ID` | De App-ID (dat wil zeggen, de Azure Storage BLOB-container naam). | Vereist |
| `-l LOG_DIR`, `--log_dir LOG_DIR` | De basis directory voor het downloaden van gegevens (een submap wordt gemaakt).  | Vereist |
| `-s START_DATE`, `--start_date START_DATE` | De Download begin datum (inbegrepen) in de indeling *jjjj-mm-dd* . | `None` |
| `-e END_DATE`, `--end_date END_DATE` | De eind datum van downloaden (inbegrepen) in de indeling *jjjj-mm-dd* . | `None` |
| `-o OVERWRITE_MODE`, `--overwrite_mode OVERWRITE_MODE` | De Overschrijf modus die moet worden gebruikt. | |
| | `0`: Nooit overschrijven; vraag de gebruiker of blobs momenteel worden gebruikt. | Standaard |
| | `1`: Vraag de gebruiker hoe u kunt door gaan wanneer de bestanden een andere grootte hebben of wanneer de blobs momenteel worden gebruikt. | |
| | `2`: Altijd overschrijven; down load momenteel gebruikte blobs. | |
| | `3`: Nooit overschrijven en toevoegen als de grootte groter is, zonder te vragen; down load momenteel gebruikte blobs. | |
| | `4`: Nooit overschrijven en toevoegen als de grootte groter is, zonder te vragen; huidige gebruikte blobs overs Laan. | |
| `--dry_run` | Afdrukken welke blobs zouden zijn gedownload, zonder te worden gedownload. | `False` |
| `--create_gzip` | Maak een *gzip* -bestand voor Vowpal Wabbit. | `False` |
| `--delta_mod_t DELTA_MOD_T` | Het tijd venster, in seconden, voor het detecteren of een bestand momenteel in gebruik is. | `3600`SEC (`1` uur) |
| `--verbose` | Meer informatie afdrukken. | `False` |
| `-v VERSION`, `--version VERSION` | De log Downloader-versie die moet worden gebruikt. | |
| | `1`: Voor niet-gebakken Logboeken (alleen voor achterwaartse compatibiliteit). | Afgeschaft |
| | `2`: Voor gekookte Logboeken. | Standaard |

### <a name="examples"></a>Voorbeelden

Voor een droge uitvoering van het downloaden van alle gegevens in uw Azure Storage BLOB-container gebruikt u de volgende code:
```cmd
python LogDownloader.py -a your_app_id -l d:\data --dry_run
```

Als u alleen logboeken wilt downloaden die zijn gemaakt sinds `overwrite_mode=4`1 januari 2018 met, gebruikt u de volgende code:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4
```

Als u een *gzip* -bestand wilt maken waarin alle gedownloade bestanden worden samengevoegd, gebruikt u de volgende code:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4 --create_gzip
```
