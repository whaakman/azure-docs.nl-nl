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
ms.date: 09/19/2017
ms.author: renash
ms.openlocfilehash: 0a87f8572af2620420faa0e3c2e575aa8add42ab
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="use-azure-files-with-linux"></a>Azure Files gebruiken met Linux
[Azure Files ](storage-files-introduction.md) is het eenvoudig te gebruiken cloudbestandssysteem van Microsoft. Azure-bestandsshares kunnen worden gekoppeld in Linux-distributies met behulp van de [CIFS kernelclient](https://wiki.samba.org/index.php/LinuxCIFS). In dit artikel ziet u twee manieren om een Azure-bestandsshare te koppelen: op aanvraag met de `mount` opdracht en op opstarten door het maken van een vermelding in `/etc/fstab`.

> [!NOTE]  
> Om te koppelen van een Azure-bestandsshare buiten de Azure moet regio die deze wordt gehost in, bijvoorbeeld on-premises of in een andere Azure regio, het besturingssysteem ondersteuning voor de versleutelingsfunctionaliteit van SMB 3.0. Coderingsfunctie voor SMB 3.0 voor Linux is geïntroduceerd in 4.11 kernel. Deze functie kunt koppelen van de bestandsshare in Azure vanaf on-premises of andere Azure-regio. Op het moment van publicatie is deze functionaliteit backported Ubuntu van 16.04 en hoger.

## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Prerequisities voor het koppelen van een Azure-bestand delen met Linux en het pakket cifs-utils
* **Kies een Linux-distributie waarvoor het cifs-utils-pakket geïnstalleerd**: Microsoft raadt u aan de volgende Linux-distributies in de afbeelding voor Azure-galerie:

    * Ubuntu Server 14.04 +
    * RHEL 7 +
    * CentOS 7 +
    * Debian 8
    * openSUSE 13.2 +
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**Het pakket cifs-utils is geïnstalleerd**: de cifs-utils kan worden geïnstalleerd met behulp van de package manager op de Linux-distributie van uw keuze. 

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

* **Bepalen van de map/bestand machtigingen van de gekoppelde share**: In de volgende voorbeelden gebruiken we 0777, om te geven lezen, schrijven en uitvoeren van machtigingen voor alle gebruikers. U kunt het vervangen door andere [type chmod machtigingen](https://en.wikipedia.org/wiki/Chmod) naar wens. 

* **Naam van het opslagaccount**: voor het koppelen van een Azure-bestandsshare hebt u de naam van het opslagaccount nodig.

* **Sleutel van het opslagaccount**: voor het koppelen van een Azure-bestandsshare hebt u de primaire (of secundaire) opslagsleutel nodig. SAS-sleutels worden momenteel niet ondersteund voor koppelen.

* **Zorg ervoor dat poort 445 is geopend**: SMB communiceert via TCP-poort 445 - controleren om te zien als uw firewall niet wordt geblokkeerd door TCP-poorten 445 van client-computer.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>De Azure File share op aanvraag met koppelen`mount`
1. **[Installeer het pakket cifs-utils voor uw Linux-distributie](#install-cifs-utils)**.

2. **Maak een map voor het koppelpunt**: u kunt hiervoor een willekeurige plaats in het bestandssysteem.

    ```
    mkdir mymountpoint
    ```

3. **Gebruik de koppelopdracht om de Azure-bestandsshare koppelen**: Vervang `<storage-account-name>`, `<share-name>`, en `<storage-account-key>` met de juiste informatie.

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Wanneer u klaar bent met de Azure-bestandsshare, mag u `sudo umount ./mymountpoint` ontkoppelen van de share.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Maken van een permanente koppelpunt voor de Azure-bestandsshare met`/etc/fstab`
1. **[Installeer het pakket cifs-utils voor uw Linux-distributie](#install-cifs-utils)**.

2. **Maak een map voor het koppelpunt**: dit is mogelijk een willekeurige plaats in het bestandssysteem, maar u moet Noteer het absolute pad van de map. Het volgende voorbeeld maakt een map onder de hoofdmap.

    ```
    sudo mkdir /mymountpoint
    ```

3. **Gebruik de volgende opdracht toe te voegen van de volgende regel om `/etc/fstab`** : Vervang `<storage-account-name>`, `<share-name>`, en `<storage-account-key>` met de juiste informatie.

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs nofail,vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> Zorg ervoor dat u toevoegt de `nofail` optie in de `/etc/fstab` item, anders uw VM tijdens het opstarten in geval van een onjuiste configuratie of een andere fout loopt tijdens het koppelen van de Azure-bestandsshare.

> [!Note]  
> U kunt `sudo mount -a` koppelen van de Azure-bestandsshare na het bewerken van `/etc/fstab` in plaats van opnieuw opstarten.

## <a name="feedback"></a>Feedback
Linux-gebruikers, die we willen graag van u!

De Azure-bestanden voor Linux gebruikersgroep biedt een forum voor u feedback kunt delen terwijl u evalueren en een bestandsopslag op Linux vast. E-mailadres [Azure bestanden Linux-gebruikers](mailto:azurefileslinuxusers@microsoft.com) lid worden van de gebruikersgroep.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende koppelingen voor meer informatie over Azure Files.
* [Naslaginformatie over REST API voor bestandsservices](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [AzCopy gebruiken met Microsoft Azure storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [De Azure CLI gebruiken met Azure storage](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [Veelgestelde vragen](../storage-files-faq.md)
* [Problemen oplossen](storage-troubleshoot-linux-file-connection-problems.md)
