---
title: Systeemvereisten voor Microsoft Azure Data Box-schijf | Microsoft Docs
description: Meer informatie over de software en netwerkvereisten voor uw Azure Data Box-schijf
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/06/2018
ms.author: alkohli
ms.openlocfilehash: aaa4e4bb24ca42adb9d283e6286dbef879bcb1ea
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299845"
---
# <a name="azure-data-box-disk-system-requirements-preview"></a>Systeemvereisten voor Azure Data Box-schijf (Preview)

Dit artikel beschrijft de belangrijke systeemvereisten voor uw oplossing Microsoft Azure Data Box-schijf en de clients die verbinding maken met de Data Box-schijf. Het is raadzaam dat u de informatie voordat u uw Data Box-schijf implementeert, en vervolgens terug naar deze zo nodig tijdens de implementatie en het volgende gebruik verwijzen zorgvuldig te controleren.

> [!IMPORTANT]
> Data Box-schijf is in Preview. Raadpleeg de [gebruiksvoorwaarden voor de Preview-versie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert. 

De systeemvereisten zijn de ondersteunde platforms voor clients verbinding maken met schijven, ondersteunde opslagaccounts en opslagtypen.


## <a name="supported-operating-systems-for-clients"></a>Ondersteunde besturingssystemen voor clients

Hier volgt een lijst van de ondersteunde besturingssystemen voor het ontgrendelen van de schijf en de kopieerbewerking gegevens via de clients die zijn verbonden met de Data Box-schijf.

| **Besturingssysteem** | **Geteste versies** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows |7, 8, 10 |
|Linux <br> <li> Ubuntu </li><li> Debian </li><li> Red Hat Enterprise Linux (RHEL) </li><li> CentOS| <br>14.04, 16.04, 18.04 <br> 8.11, 9 <br> 7.0 <br> 6.5, 6,9, 7.0, 7.5 |  

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

## <a name="supported-storage-accounts"></a>Ondersteunde opslagaccounts

Hier volgt een lijst van de typen ondersteunde opslag voor de Data Box-schijf.

| **Opslagaccount** | **Opmerkingen** |
| --- | --- |
| Klassiek | Standaard |
| Algemeen gebruik  |Standaard; zowel V1 als V2 worden ondersteund. Warme en koude lagen worden ondersteund. |


## <a name="supported-storage-types"></a>Ondersteunde opslagtypen

Hier volgt een lijst van de typen ondersteunde opslag voor de Data Box-schijf.

| **Bestandsindeling** | **Opmerkingen** |
| --- | --- |
| Azure blok-blob | |
| Azure-pagina-blobs  | |


## <a name="next-step"></a>Volgende stap

* [Implementeren van uw Azure Data Box-schijf](data-box-disk-deploy-ordered.md)

