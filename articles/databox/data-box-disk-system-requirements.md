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
ms.date: 08/28/2018
ms.author: alkohli
ms.openlocfilehash: 2fdd574adf3587f11984bee2a2549d9bcd0c4c0d
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126002"
---
# <a name="azure-data-box-disk-system-requirements-preview"></a>Systeemvereisten voor Azure Data Box-schijf (Preview)

Dit artikel beschrijft de belangrijke systeemvereisten voor uw oplossing Microsoft Azure Data Box-schijf en de clients die verbinding maken met de Data Box-schijf. Het is raadzaam dat u de informatie voordat u uw Data Box-schijf implementeert, en vervolgens terug naar deze zo nodig tijdens de implementatie en het volgende gebruik verwijzen zorgvuldig te controleren.

> [!IMPORTANT]
> Data Box-schijf is in Preview. Raadpleeg de [gebruiksvoorwaarden voor de Preview-versie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert. 

De systeemvereisten zijn de ondersteunde platforms voor clients verbinding maken met schijven, ondersteunde opslagaccounts en opslagtypen.


## <a name="supported-operating-systems-for-clients"></a>Ondersteunde besturingssystemen voor clients

Hier volgt een lijst van de ondersteunde besturingssystemen voor het ontgrendelen van de schijf en de kopieerbewerking gegevens via de clients die zijn verbonden met de Data Box-schijf.

| **Besturingssysteem/platform** | **Versies** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows |7, 8, 10 |
| Windows Powershell |4.0 |
| .NET Framework |4.5.1 |
| Windows Management Framework |4.0|

> [!NOTE] 
> BitLocker moet zijn ingeschakeld op de clients die worden uitgevoerd van de schijf hulpprogramma ontgrendelen en worden gebruikt om de gegevens te kopiëren.


## <a name="supported-storage-accounts"></a>Ondersteunde opslagaccounts

Hier volgt een lijst van de typen ondersteunde opslag voor de Data Box-schijf.

| **Opslagaccount** | **Opmerkingen** |
| --- | --- |
| Klassiek | Standard |
| Algemeen doel  |Standaard; zowel V1 als V2 worden ondersteund. Warme en koude lagen worden ondersteund. |


## <a name="supported-storage-types"></a>Ondersteunde opslagtypen

Hier volgt een lijst van de typen ondersteunde opslag voor de Data Box-schijf.

| **Bestandsindeling** | **Opmerkingen** |
| --- | --- |
| Azure blok-blob | |
| Azure-pagina-blobs  | |


## <a name="next-step"></a>Volgende stap

* [Implementeren van uw Azure Data Box-schijf](data-box-disk-deploy-ordered.md)

