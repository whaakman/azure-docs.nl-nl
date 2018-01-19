---
title: Azure Files gebruiken met Linux | Microsoft Docs
description: Informatie over het koppelen van een Azure-bestandsshare via SMB op Linux.
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: renash
ms.openlocfilehash: cca0d315a815faca5db07099b8e8e451ef55fad5
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="use-azure-files-with-linux"></a>Azure Files gebruiken met Linux
[Azure Files ](storage-files-introduction.md) is het eenvoudig te gebruiken cloudbestandssysteem van Microsoft. Azure-bestandsshares kunnen worden gekoppeld in de Linux-distributies met behulp van de [CIFS kernelclient](https://wiki.samba.org/index.php/LinuxCIFS). In dit artikel ziet u twee manieren om een Azure-bestandsshare te koppelen: op aanvraag met de `mount` opdracht en op opstarten door het maken van een vermelding in `/etc/fstab`.

> [!NOTE]  
> Om te koppelen van een Azure-bestandsshare buiten de Azure-regio die deze wordt gehost in, bijvoorbeeld on-premises of in een andere Azure-regio, moet het besturingssysteem de versleutelingsfunctionaliteit van SMB 3.0 ondersteunen.

## <a name="prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Vereisten voor het koppelen van een Azure-bestandsshare met Linux en het pakket cifs-utils
* **Kies een Linux-distributie waarvoor het cifs utils pakket is geïnstalleerd.**  
    De volgende Linux-distributies zijn beschikbaar voor gebruik in de Azure-galerie:

    * Ubuntu Server 14.04 +
    * RHEL 7 +
    * CentOS 7+
    * Debian 8 +
    * openSUSE 13.2 +
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**Het pakket cifs-utils is geïnstalleerd.**  
    Het pakket cifs utils kan worden geïnstalleerd met behulp van de package manager op de Linux-distributie van uw keuze. 

    Op **Ubuntu** en **op basis van Debian** distributies, gebruiken de `apt-get` Pakketbeheer:

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    Op **RHEL** en **CentOS**, gebruiken de `yum` Pakketbeheer:

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    Op **openSUSE**, gebruiken de `zypper` Pakketbeheer:

    ```
    sudo zypper install samba*
    ```

    Gebruik de juiste package manager op andere distributies of [compileren van bron](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* <a id="smb-client-reqs"></a>**Vereisten voor de SMB-client begrijpen.**  
    Azure Files worden gekoppeld, hetzij via SMB 2.1 als SMB 3.0. Voor verbindingen die afkomstig is van clients on-premises of in andere Azure-regio's, weigert Azure-bestanden SMB 2.1 (of SMB 3.0 zonder versleuteling). Als *vereiste gegevensoverdracht secure* is ingeschakeld voor storage-account Azure-bestanden alleen verbindingen met versleuteling met behulp van SMB 3.0 wordt toestaan.
    
    Ondersteuning voor SMB 3.0-versleuteling is is geïntroduceerd in Linux kernelversie 4.11 en backported tot oudere versies van de kernel voor populaire Linux-distributies. Deze functie op het moment van publicatie van dit document wordt ondersteund door de volgende distributies vanuit de Azure-galerie:

    - Ubuntu Server 16.04 +
    - openSUSE 42,3 +
    - SUSE Linux Enterprise Server 12 SP3+
    
    Als uw Linux-distributie hier niet weergegeven is, kunt u controleren de versie van de Linux-kernel met de volgende opdracht:

    ```
    uname -r
    ```

* **Bepalen van de map/bestand machtigingen van de gekoppelde share**: In de voorbeelden hieronder de machtiging `0777` wordt gebruikt om aan te geven lezen, schrijven en uitvoeren van machtigingen voor alle gebruikers. U kunt het vervangen door andere [type chmod machtigingen](https://en.wikipedia.org/wiki/Chmod) naar wens. 

* **Naam van het opslagaccount**: een Azure-bestandsshare koppelen, moet u de naam van het opslagaccount.

* **Opslagaccountsleutel**: een Azure-bestandsshare koppelen, moet u de primaire (of secundaire)-opslagsleutel. SAS-sleutels worden momenteel niet ondersteund voor koppelen.

* **Zorg ervoor dat poort 445 is geopend**: SMB communiceert via TCP-poort 445 - controleren om te zien als uw firewall niet wordt geblokkeerd door TCP-poorten 445 van client-computer.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>De Azure file share op aanvraag met koppelen`mount`
1. **[Installeer het pakket cifs-utils voor uw Linux-distributie](#install-cifs-utils)**.

2. **Maak een map voor het koppelpunt**: een map voor een koppelpunt overal kan worden gemaakt op het bestandssysteem, maar het is algemene conventie voor het maken van dit onder de `/mnt` map. Bijvoorbeeld:

    ```
    mkdir /mnt/MyAzureFileShare
    ```

3. **Gebruik de koppelopdracht om de Azure-bestandsshare koppelen**: Vervang `<storage-account-name>`, `<share-name>`, `<smb-version>`, `<storage-account-key>`, en `<mount-point>` met de juiste informatie voor uw omgeving. Als uw Linux-distributie SMB 3.0 met versleuteling ondersteunt (Zie [SMB begrijpen clientvereisten](#smb-client-reqs) voor meer informatie), gebruiken `3.0` voor `<smb-version>`. Gebruik voor Linux-distributies die bieden geen ondersteuning voor SMB 3.0 met versleuteling, `2.1` voor `<smb-version>`. Houd er rekening mee dat een Azure-bestandsshare alleen kan worden gemonteerd buiten een Azure-regio (met inbegrip van lokale of in een andere Azure-regio) met SMB 3.0. 

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Wanneer u klaar bent met de Azure-bestandsshare, mag u `sudo umount <mount-point>` ontkoppelen van de share.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Maken van een permanente koppelpunt voor de Azure-bestandsshare met`/etc/fstab`
1. **[Installeer het pakket cifs-utils voor uw Linux-distributie](#install-cifs-utils)**.

2. **Maak een map voor het koppelpunt**: een map voor een koppelpunt overal kan worden gemaakt op het bestandssysteem, maar het is algemene conventie voor het maken van dit onder de `/mnt` map. Let op het absolute pad van de map waar u dit hebt gemaakt. Bijvoorbeeld de volgende opdracht maakt u een nieuwe map onder `/mnt` (het pad is een absoluut pad).

    ```
    sudo mkdir /mnt/MyAzureFileShare
    ```

3. **Gebruik de volgende opdracht toe te voegen van de volgende regel om `/etc/fstab`** : Vervang `<storage-account-name>`, `<share-name>`, `<smb-version>`, `<storage-account-key>`, en `<mount-point>` met de juiste informatie voor uw -omgeving. Als uw Linux-distributie SMB 3.0 met versleuteling ondersteunt (Zie [SMB begrijpen clientvereisten](#smb-client-reqs) voor meer informatie), gebruiken `3.0` voor `<smb-version>`. Gebruik voor Linux-distributies die bieden geen ondersteuning voor SMB 3.0 met versleuteling, `2.1` voor `<smb-version>`. Houd er rekening mee dat een Azure-bestandsshare alleen kan worden gemonteerd buiten een Azure-regio (met inbegrip van lokale of in een andere Azure-regio) met SMB 3.0. 

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> <mount-point> cifs nofail,vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> U kunt `sudo mount -a` koppelen van de Azure-bestandsshare na het bewerken van `/etc/fstab` in plaats van opnieuw opstarten.

## <a name="feedback"></a>Feedback
Linux-gebruikers, die we willen graag van u!

De Azure-bestanden voor Linux gebruikersgroep biedt een forum voor u feedback kunt delen terwijl u evalueren en een bestandsopslag op Linux vast. E-mailadres [Azure bestanden Linux-gebruikers](mailto:azurefileslinuxusers@microsoft.com) lid worden van de gebruikersgroep.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende koppelingen voor meer informatie over Azure Files.
* [Inleiding tot Azure-bestanden](storage-files-introduction.md)
* [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md)
* [Veelgestelde vragen](../storage-files-faq.md)
* [Problemen oplossen](storage-troubleshoot-linux-file-connection-problems.md)