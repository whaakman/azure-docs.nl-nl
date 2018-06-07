---
title: Vereisten voor Azure Import/Export-service | Microsoft Docs
description: Inzicht in de hardware- en softwarevereisten voor de Azure Import/Export-service.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 06/06/2018
ms.author: alkohli
ms.openlocfilehash: 4c6e22f50f4550cb4a6e25960bcc13a4d92e9819
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825064"
---
# <a name="azure-importexport-system-requirements"></a>Systeemvereisten voor Azure Import/Export

Dit artikel worden de belangrijke vereisten voor uw Azure Import/Export-service. Het is raadzaam dat u de informatie voordat u het gebruik van de service voor importeren/exporteren en vervolgens naar deze zo nodig tijdens de bewerking terugverwijzen zorgvuldig te controleren.

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

Voorbereiden van de harde schijven met het hulpprogramma WAImportExport, de volgende **64-bits besturingssysteem die ondersteuning bieden voor BitLocker-stationsversleuteling** worden ondersteund.


|Platform |Versie |
|---------|---------|
|Windows     | Windows 7 Enterprise, Windows 7 Ultimate <br> Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> WindowsServer 2012, Windows Server 2012 R2         |



## <a name="supported-storage-accounts"></a>Ondersteunde storage-accounts

Azure Import/Export-service ondersteunt de volgende Azure storage-accounts.
- Klassiek
- Blob Storage-accounts
- Algemene doel v1-opslagaccounts. 

Elke taak kan worden gebruikt voor gegevensoverdracht naar of van slechts één opslagaccount. Een enkel voor importeren/exporteren-taak kan niet met andere woorden, meerdere opslagaccounts overbruggen. Zie voor meer informatie over het maken van een nieuw opslagaccount [het maken van een Opslagaccount](storage-create-storage-account.md#create-a-storage-account).

> [!IMPORTANT] 
> Storage-accounts biedt geen ondersteuning voor de service Azure Import exporteren waar de [Virtual Network Service-eindpunten](../../virtual-network/virtual-network-service-endpoints-overview.md) functie is ingeschakeld. 

## <a name="supported-storage-types"></a>Ondersteunde opslagtypen

De volgende lijst met opslagtypen wordt ondersteund met Azure Import/Export-service.


|Job  |Storage  |Ondersteund  |Niet ondersteund  |
|---------|---------|---------|---------|
|Importeren     |  Azure Blob-opslag. <br>Blok-Blobs, pagina-blobs ondersteund. <br> Azure-bestanden ondersteund.       |         |
|Exporteren     |   Azure Blob-opslag. <br>Blok-blobs, pagina-blobs en toevoeg-blobs ondersteund.       | Azure Files wordt niet ondersteund.        |


## <a name="supported-hardware"></a>Ondersteunde hardware 

Voor de service Azure Import/Export ondersteunde schijven nodig en ondersteunde SATA-connectors om gegevens te kopiëren.

### <a name="supported-disks"></a>Ondersteunde schijven

De volgende lijst van schijven wordt ondersteund voor gebruik met de Import/Export-service.


|Schijftype  |Grootte  |Ondersteund |Niet ondersteund  |
|---------|---------|---------|---------|
|SSD    |   2,5-inch      |         |         |
|HDD     |  2,5-inch<br>3.5"       |SATA II, SATA III         |Externe harde schijf met ingebouwde USB-adapter <br> Schijf binnen het hoofdlettergebruik van een externe harde schijf         |


Een taak één importeren/exporteren kan hebben:
- Maximaal 10 HDD SSD's.
- Een combinatie van harde schijven per SSD van elke grootte.

Groot aantal schijven kan worden verdeeld over meerdere taken en er is geen beperkingen op het aantal taken dat kan worden gemaakt. 

Voor taken van gegevensimport wordt alleen het eerste gegevensvolume op het station verwerkt. Het gegevensvolume moet zijn geformatteerd met NTFS.

### <a name="supported-external-usb-adaptors"></a>Externe USB-adapters ondersteund

Wanneer het voorbereiden van de harde schijven en kopiëren van de gegevens met het hulpprogramma WAImportExport, kunt u externe USB-adapters (buiten-de-shelp) te volgen: 
- Anker 68UPSATAA 02 door
- Anker 68UPSHHDS-door
- Startech SATADOCK22UE
- Orico 6628SUS3-C-BK (6628-serie)
- Thermaltake BlacX Hot Swap SATA externe harde schijf dockingstation (USB 2.0 & eSATA)


## <a name="next-steps"></a>Volgende stappen

* [Het hulpprogramma WAImportExport instellen](storage-import-export-tool-how-to.md)
* [Gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md)
* [Azure Import exporteren REST-API-voorbeeld](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

