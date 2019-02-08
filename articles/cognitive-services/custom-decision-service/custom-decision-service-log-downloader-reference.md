---
title: LogDownloader - Custom Decision Service
titlesuffix: Azure Cognitive Services
description: Download de logboekbestanden die worden geproduceerd door Azure Custom Decision Service.
services: cognitive-services
author: marco-rossi29
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: marossi
ms.openlocfilehash: c03278c0d0faced20f4e02fcc1f61531c88ae141
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55869667"
---
# <a name="logdownloader"></a>LogDownloader

Downloaden van de logboekbestanden die worden geproduceerd door Azure Custom Decision Service en genereert de *.gz* bestanden die worden gebruikt door experimenten.

## <a name="prerequisites"></a>Vereisten

- Python 3: Geïnstalleerd en op het pad. U wordt aangeraden de 64-bits-versie voor het afhandelen van grote bestanden.
- De *mwt-Microsoft-ds* opslagplaats: [Kloon de opslagplaats](https://github.com/Microsoft/mwt-ds).
- De *azure-storage-blob* pakket: Voor installatie-informatie, gaat u naar [Microsoft Azure Storage-clientbibliotheek voor Python](https://github.com/Azure/azure-storage-python#option-1-via-pypi).
- Voer uw Azure storage-verbindingsreeks in *mwt-ds/DataScience/ds.config*: Ga als volgt de *my_app_id: my_connectionString* sjabloon. Kunt u meerdere `app_id`. Bij het uitvoeren van `LogDownloader.py`, als de invoer `app_id` is niet gevonden in `ds.config`, `LogDownloader.py` maakt gebruik van de `$Default` verbindingsreeks.

## <a name="usage"></a>Gebruik

Ga naar `mwt-ds/DataScience` en uit te voeren `LogDownloader.py` met de relevante argumenten, zoals beschreven in de volgende code:

```cmd
python LogDownloader.py [-h] -a APP_ID -l LOG_DIR [-s START_DATE]
                        [-e END_DATE] [-o OVERWRITE_MODE] [--dry_run]
                        [--create_gzip] [--delta_mod_t DELTA_MOD_T]
                        [--verbose] [-v VERSION]
```

### <a name="parameters"></a>Parameters

| Invoer | Description | Standaard |
| --- | --- | --- |
| `-h`, `--help` | Ziet u de help-bericht en afsluiten. | |
| `-a APP_ID`, `--app_id APP_ID` | De app-ID (dat wil zeggen, de Azure Storage blob-containernaam). | Vereist |
| `-l LOG_DIR`, `--log_dir LOG_DIR` | De basismap voor het downloaden van gegevens (een submap wordt gemaakt).  | Vereist |
| `-s START_DATE`, `--start_date START_DATE` | Het downloaden van de begindatum (opgenomen), in *jjjj-MM-DD* indeling. | `None` |
| `-e END_DATE`, `--end_date END_DATE` | De downloaden einddatum (opgenomen), in *jjjj-MM-DD* indeling. | `None` |
| `-o OVERWRITE_MODE`, `--overwrite_mode OVERWRITE_MODE` | De modus voor overschrijven om te gebruiken. | |
| | `0`: Nooit overschrijven; Vraag de gebruiker of-blobs die momenteel worden gebruikt. | Standaard | |
| | `1`: Vraag de gebruiker voor de vervolgacties wanneer de bestanden verschillende grootte hebben of wanneer de blobs zijn momenteel wordt gebruikt. | |
| | `2`: Altijd overschreven; Download blobs die momenteel worden gebruikt. | |
| | `3`: Nooit overschrijven en toevoeg-als de grootte groter is, zonder te vragen; Download blobs die momenteel worden gebruikt. | |
| | `4`: Nooit overschrijven en toevoeg-als de grootte groter is, zonder te vragen; blobs overslaan die momenteel worden gebruikt. | |
| `--dry_run` | Afdrukken welke blobs zou zijn gedownload, zonder te downloaden. | `False` |
| `--create_gzip` | Maak een *gzip* -bestand voor Vowpal Wabbit. | `False` |
| `--delta_mod_t DELTA_MOD_T` | De tijdsduur in seconden, voor het detecteren van of een bestand gebruikt wordt. | `3600` per seconde (`1` uur) |
| `--verbose` | Meer details afdrukken. | `False` |
| `-v VERSION`, `--version VERSION` | De versie van de downloader logboek moet worden gebruikt. | |
| | `1`: Voor ook Logboeken (alleen voor compatibiliteit met eerdere versies). | Afgeschaft |
| | `2`: Voor gekookte Logboeken. | Standaard |

### <a name="examples"></a>Voorbeelden

Voor het droge uitvoeren van het downloaden van de gegevens in uw Azure Storage blob-container, gebruik de volgende code:
```cmd
python LogDownloader.py -a your_app_id -l d:\data --dry_run
```

Alleen logboeken die zijn gemaakt sinds 1 januari 2018 met downloaden `overwrite_mode=4`, gebruik de volgende code:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4
```

Maakt een *gzip* bestand samenvoegen van de gedownloade bestanden, gebruik de volgende code:
```cmd
python LogDownloader.py -a your_app_id -l d:\data -s 2018-1-1 -o 4 --create_gzip
```
