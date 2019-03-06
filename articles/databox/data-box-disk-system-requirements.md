---
title: Systeemvereisten voor Microsoft Azure Data Box-schijf | Microsoft Docs
description: Meer informatie over de software en netwerkvereisten voor uw Azure Data Box-schijf
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 02/22/2019
ms.author: alkohli
ms.openlocfilehash: 067bb6b806ddd9b83d4ea755876a980ca45d76e9
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57407622"
---
# <a name="azure-data-box-disk-system-requirements"></a>Azure Data Box-schijf-systeemvereisten

Dit artikel beschrijft de belangrijke systeemvereisten voor uw oplossing Microsoft Azure Data Box-schijf en de clients die verbinding maken met de Data Box-schijf. Het is raadzaam dat u de informatie voordat u uw Data Box-schijf implementeert, en vervolgens terug naar deze zo nodig tijdens de implementatie en het volgende gebruik verwijzen zorgvuldig te controleren.

De systeemvereisten zijn de ondersteunde platforms voor clients verbinding maken met schijven, ondersteunde opslagaccounts en opslagtypen.


## <a name="supported-operating-systems-for-clients"></a>Ondersteunde besturingssystemen voor clients

Hier volgt een lijst van de ondersteunde besturingssystemen voor het ontgrendelen van de schijf en de kopieerbewerking gegevens via de clients die zijn verbonden met de Data Box-schijf.

| **Besturingssysteem** | **Geteste versies** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows |7, 8, 10 |
|Linux <br> <li> Ubuntu </li><li> Debian </li><li> Red Hat Enterprise Linux (RHEL) </li><li> CentOS| <br>14.04, 16.04, 18.04 <br> 8.11, 9 <br> 7.0 <br> 6.5, 6.9, 7.0, 7.5 |  

## <a name="other-required-software-for-windows-clients"></a>Andere vereiste software voor Windows-clients

Volgende moeten ook worden geïnstalleerd voor Windows-client.

| **Software**| **Versie** |
| --- | --- |
| Windows Powershell |5.0 |
| .NET Framework |4.5.1 |
| Windows Management Framework |5.0|
| BitLocker| - |

## <a name="other-required-software-for-linux-clients"></a>Andere vereiste software voor Linux-clients

Voor Linux-client installeert de Data Box-schijf-toolset van de volgende vereiste software:

- dislocker
- OpenSSL

## <a name="supported-connection"></a>Ondersteunde verbinding

De clientcomputer met de gegevens moet hebben een USB-3.0 of hoger poort. De schijven verbinding worden gemaakt met deze met behulp van de opgegeven kabel.

## <a name="supported-storage-accounts"></a>Ondersteunde opslagaccounts

Hier volgt een lijst van de typen ondersteunde opslag voor de Data Box-schijf.

| **Opslagaccount** | **Opmerkingen** |
| --- | --- |
| Klassiek | Standard |
| Algemeen doel  |Standaard; zowel V1 als V2 worden ondersteund. Warme en koude lagen worden ondersteund. |
| BLOB storage-account | |

>[!NOTE]
> Azure Data Lake Storage Gen 2-accounts worden niet ondersteund.


## <a name="supported-storage-types-for-upload"></a>Ondersteunde opslagtypen voor uploaden

Hier volgt een lijst van de opslag die worden ondersteund voor geüpload naar Azure met behulp van Data Box-schijf.

| **Bestandsindeling** | **Opmerkingen** |
| --- | --- |
| Azure blok-blob | |
| Azure-pagina-blobs  | |
| Azure Files  | |
| Beheerde schijven | |


## <a name="next-step"></a>Volgende stap

* [Implementeren van uw Azure Data Box-schijf](data-box-disk-deploy-ordered.md)

