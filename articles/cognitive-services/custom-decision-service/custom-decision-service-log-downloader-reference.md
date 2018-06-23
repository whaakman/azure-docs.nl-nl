---
title: LogDownloader - cognitieve Azure-Services | Microsoft Docs
description: Download de logboekbestanden die door Azure aangepaste besluit Service worden gemaakt.
services: cognitive-services
author: marco-rossi29
manager: marco-rossi29
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: marossi
ms.openlocfilehash: 783b534b3b3f4bb7f5d9f073f491690759edfea5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345464"
---
# <a name="logdownloader"></a>LogDownloader

Downloaden van de logboekbestanden die worden geproduceerd door Azure aangepaste besluit Service en het genereren van de *.gz* bestanden die worden gebruikt door experimenteren.

## <a name="prerequisites"></a>Vereisten

- Python 3: Geïnstalleerd en op uw pad. U wordt aangeraden de 64-bits versie grote bestanden moeten worden verwerkt.
- De *mwt-Microsoft-ds* opslagplaats: [kloon de opslagplaats](https://github.com/Microsoft/mwt-ds).
- De *azure-opslag-blob* pakket: voor details over de installatie, gaat u naar [bibliotheek van Microsoft Azure-opslag voor Python](https://github.com/Azure/azure-storage-python#option-1-via-pypi).
- Voer uw Azure-opslag-verbindingsreeks in *mwt-ds/DataScience/ds.config*: Volg de *my_app_id: my_connectionString* sjabloon. U kunt meerdere opgeven `app_id`. Bij het uitvoeren van `LogDownloader.py`, als de invoer `app_id` is niet gevonden in `ds.config`, `LogDownloader.py` maakt gebruik van de `$Default` verbindingsreeks.

## <a name="usage"></a>Gebruik

Ga naar `mwt-ds/DataScience` en voer `LogDownloader.py` met de relevante argumenten, zoals beschreven in de volgende code:

```cmd
python LogDownloader.py [-h] -a APP_ID -l LOG_DIR [-s START_DATE]
                        [-e END_DATE] [-o OVERWRITE_MODE] [--dry_run]
                        [--create_gzip] [--delta_mod_t DELTA_MOD_T]
                        [--verbose] [-v VERSION]
```

### <a name="parameters"></a>Parameters

| Invoer | Beschrijving | Normaal |
| --- | --- | --- |
| `-h`, `--help` | Ziet u de help-bericht en afsluiten. | |
| `-a APP_ID`, `--app_id APP_ID` | De app-ID (dat wil zeggen, de Azure Storage-blob-containernaam). | Vereist |
| `-l LOG_DIR`, `--log_dir LOG_DIR` | De basismap voor het downloaden van gegevens (een submap is gemaakt).  | Vereist |
| `-s START_DATE`, `--start_date START_DATE` | Het downloaden begindatum (opgenomen), in *jjjj-MM-DD* indeling. | `None` |
| `-e END_DATE`, `--end_date END_DATE` | De downloaden einddatum (opgenomen), in *jjjj-MM-DD* indeling. | `None` |
| `-o OVERWRITE_MODE`, `--overwrite_mode OVERWRITE_MODE` | De modus overschrijven te gebruiken. | |
| | `0`: Nooit overschrijven; Vraag de gebruiker of BLOB's worden momenteel gebruikt. | Normaal | |
| | `1`: Vraagt de gebruiker hoe verder te gaan wanneer de bestanden verschillende grootten hebben of wanneer de blobs momenteel in gebruik. | |
| | `2`: Altijd boven; blobs downloaden die momenteel worden gebruikt. | |
| | `3`: Nooit overschrijven en ook als de groter is, zonder gebruikersbevestiging; blobs downloaden die momenteel worden gebruikt. | |
| | `4`: Nooit overschrijven en ook als de groter is, zonder gebruikersbevestiging; blobs overslaan momenteel gebruikt. | |
| `--dry_run` | Afdrukken welke blobs zou zijn gedownload, zonder te downloaden. | `False` |
| `--create_gzip` | Maak een *gzip* -bestand voor Vowpal Wabbit. | `False` |
| `--delta_mod_t DELTA_MOD_T` | Het tijdvenster in seconden, voor het detecteren of een bestand momenteel in gebruik is. | `3600` per seconde (`1` uur) |
| `--verbose` | Meer details afdrukken. | `False` |
| `-v VERSION`, `--version VERSION` | De versie van de downloadprogramma voor het logboek moet worden gebruikt. | |
| | `1`: Voor ook Logboeken (alleen voor neerwaartse compatibiliteit). | Afgeschaft |
| | `2`: Voor gekookte Logboeken. | Normaal |

### <a name="examples"></a>Voorbeelden

Gebruik de volgende code voor een vulprocedure van downloaden van de gegevens in uw Azure Storage-blob-container:
```cmd
python LogDownloader.py -a your_app_id -l d:\data --dry_run
```

Voor het downloaden van alleen de logboeken die zijn gemaakt sinds 1 januari 2018 met `overwrite_mode=4`, de volgende code gebruiken:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4
```

Maken van een *gzip* bestand met de gedownloade bestanden samenvoegen, gebruikt u de volgende code:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4 --create_gzip
```