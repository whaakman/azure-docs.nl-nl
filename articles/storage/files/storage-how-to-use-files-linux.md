---
title: Azure Files gebruiken met Linux | Microsoft Docs
description: Leer hoe u een Azure-bestandsshare koppelen via SMB op Linux.
services: storage
author: RenaShahMSFT
ms.service: storage
ms.topic: article
ms.date: 03/29/2018
ms.author: renash
ms.component: files
ms.openlocfilehash: 89343f3e4ec91dd32b24cdea6632cecd855cc6f8
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39523220"
---
# <a name="use-azure-files-with-linux"></a>Azure Files gebruiken met Linux
[Azure Files ](storage-files-introduction.md) is het eenvoudig te gebruiken cloudbestandssysteem van Microsoft. Azure-bestandsshares kunnen worden gekoppeld in Linux-distributies, met de [SMB-kernel-client](https://wiki.samba.org/index.php/LinuxCIFS). In dit artikel ziet u een Azure-bestandsshare koppelen op twee manieren: op aanvraag met de `mount` opdracht en op opstarten met het maken van een vermelding in `/etc/fstab`.

> [!NOTE]  
> Als u wilt koppelen van een Azure-bestandsshare buiten de Azure-regio waarin die deze wordt gehost in, bijvoorbeeld on-premises of in een andere Azure-regio moet het besturingssysteem ondersteuning bieden voor de versleutelingsfunctionaliteit van SMB 3.0.

## <a name="prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Vereisten voor het koppelen van een Azure-bestandsshare met Linux en de cifs-utils-pakket
<a id="smb-client-reqs"></a>
* **Kies een Linux-distributie op basis van de behoeften van uw koppelen.**  
      Azure Files kunnen worden gekoppeld via het SMB 2.1 als SMB 3.0. Voor verbindingen die afkomstig zijn van clients on-premises of in andere Azure-regio's, weigert Azure-bestanden SMB 2.1 (of SMB 3.0 zonder versleuteling). Als *veilige overdracht vereist* is ingeschakeld voor een opslagaccount, Azure Files mag alleen worden verbindingen met behulp van SMB 3.0 met-codering.
    
    Ondersteuning voor SMB 3.0-versleuteling is geïntroduceerd in Linux kernelversie 4.11 en is backported tot oudere kernelversies voor populaire Linux-distributies. Op het moment van publicatie van dit document ondersteunen de volgende distributies vanuit de Azure-galerie koppelen optie opgegeven in de headers-tabel. 

* **Minimaal aanbevolen versies met de bijbehorende mount-mogelijkheden (SMB-versie 2.1 vs SMB-versie 3.0)**    
    
    |   | SMB 2.1 <br>(Koppelingen op VM's binnen dezelfde Azure-regio) | SMB 3.0 <br>(Koppelingen van locatie en regio-overschrijdende) |
    | --- | :---: | :---: |
    | Ubuntu Server | 14.04 + | 16.04 + |
    | RHEL | 7 + | 7.5 + |
    | CentOS | 7 + |  7.5 + |
    | Debian | 8 + |   |
    | openSUSE | 13.2 + | 42,3 + |
    | SUSE Linux Enterprise Server | 12 | 12 SP3 + |
    
    Als uw Linux-distributie hier niet wordt vermeld, kunt u controleren de versie van de Linux-kernel met de volgende opdracht:    

   ```bash
   uname -r
   ```    

* <a id="install-cifs-utils"></a>**De cifs-utils-pakket is geïnstalleerd.**  
    De cifs-utils-pakket kan worden geïnstalleerd met behulp van de package manager op de Linux-distributie van uw keuze. 

    Op **Ubuntu** en **Debian gebaseerde** distributies, gebruiken de `apt-get` Pakketbeheer:

    ```bash
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    Op **RHEL** en **CentOS**, gebruikt u de `yum` Pakketbeheer:

    ```bash
    sudo yum install cifs-utils
    ```

    Op **openSUSE**, gebruikt u de `zypper` Pakketbeheer:

    ```bash
    sudo zypper install cifs-utils
    ```

    Gebruik de juiste package manager in een andere distributie of [compileren van bron](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download)
    
* **Besluit op de map/bestand machtigingen van de gekoppelde share**: In de voorbeelden hieronder de machtiging `0777` wordt gebruikt om te voorzien van lezen, schrijven en uitvoeren van machtigingen voor alle gebruikers. U kunt deze vervangen door andere [chmod machtigingen](https://en.wikipedia.org/wiki/Chmod) naar wens. 

* **De naam van opslagaccount**: voor het koppelen van een Azure-bestandsshare, moet u de naam van het opslagaccount.

* **Opslagaccountsleutel**: voor het koppelen van een Azure-bestandsshare, moet u de primaire (of secundaire) opslagsleutel. SAS-sleutels worden momenteel niet ondersteund voor koppelen.

* **Zorg ervoor dat poort 445 is geopend**: SMB communiceert via TCP-poort 445 - Controleer poorten om te zien als de firewall niet wordt geblokkeerd door TCP 445 van de clientcomputer.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>De Azure file share op aanvraag met koppelen `mount`
1. **[Installeer de cifs-utils-pakket voor uw Linux-distributie](#install-cifs-utils)**.

2. **Maak een map voor het koppelpunt**: een map voor een koppelpunt overal kan worden gemaakt op het bestandssysteem, maar wel algemene naamgeving om aan te maken onder de `/mnt` map. Bijvoorbeeld:

    ```bash
    mkdir /mnt/MyAzureFileShare
    ```

3. **Gebruik de koppelopdracht om de Azure-bestandsshare koppelen**: Vervang `<storage-account-name>`, `<share-name>`, `<smb-version>`, `<storage-account-key>`, en `<mount-point>` met de juiste informatie voor uw omgeving. Als uw Linux-distributie biedt ondersteuning voor SMB 3.0 met-codering (Zie [clientvereisten voor informatie over SMB](#smb-client-reqs) voor meer informatie), gebruikt u `3.0` voor `<smb-version>`. Voor Linux-distributies die bieden geen ondersteuning voor SMB 3.0 met versleuteling, gebruikt u `2.1` voor `<smb-version>`. Houd er rekening mee dat een Azure-bestandsshare alleen buiten een Azure-regio kan worden gekoppeld (met inbegrip van on-premises of in een andere Azure-regio) met SMB 3.0. 

    ```bash
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Wanneer u klaar bent met behulp van de Azure-bestandsshare, mag u `sudo umount <mount-point>` ontkoppelen van de share.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Maken van een permanente koppelpunt voor de Azure-bestandsshare met `/etc/fstab`
1. **[Installeer de cifs-utils-pakket voor uw Linux-distributie](#install-cifs-utils)**.

2. **Maak een map voor het koppelpunt**: een map voor een koppelpunt overal kan worden gemaakt op het bestandssysteem, maar wel algemene naamgeving om aan te maken onder de `/mnt` map. Houd er rekening mee het absolute pad van de map waar u dit hebt gemaakt. Bijvoorbeeld, de volgende opdracht maakt u een nieuwe map onder `/mnt` (het pad is een absoluut pad).

    ```bash
    sudo mkdir /mnt/MyAzureFileShare
    ```

3. **Maak een referentie-bestand voor het opslaan van de gebruikersnaam (naam van het opslagaccount) en het wachtwoord (de opslagaccountsleutel) voor de bestandsshare.** Vervang `<storage-account-name>` en `<storage-account-key>` met de juiste informatie voor uw omgeving. 

    ```bash
    if [ -d "/etc/smbcredentials" ]; then
        sudo mkdir /etc/smbcredentials
    fi

    if [ ! -f "/etc/smbcredentials/<storage-account-name>.cred" ]; then
        sudo bash -c 'echo "username=<storage-account-name>" >> /etc/smbcredentials/<storage-account-name>.cred'
        sudo bash -c 'echo "password=<storage-account-key>" >> /etc/smbcredentials/<storage-account-name>.cred'
    fi
    ```

4. **Machtigingen voor de referentie-bestand wijzigen zodat alleen hoofdmap kan lezen of het wachtwoordbestand te wijzigen.** Aangezien de opslagaccountsleutel in feite een absoluut beheerderswachtwoord voor het opslagaccount is, is instellen van de machtigingen voor het bestand zodanig dat alleen de hoofd-toegankelijk belangrijk, waarbij lagere bevoegdheid gebruikers kunnen niet ophalen van de opslagaccountsleutel.   

    ```bash
    sudo chmod 600 /etc/smbcredentials/<storage-account-name>.cred
    ```

5. **Gebruik de volgende opdracht toe te voegen van de volgende regel om `/etc/fstab`** : Vervang `<storage-account-name>`, `<share-name>`, `<smb-version>`, en `<mount-point>` met de juiste informatie voor uw omgeving. Als uw Linux-distributie biedt ondersteuning voor SMB 3.0 met-codering (Zie [clientvereisten voor informatie over SMB](#smb-client-reqs) voor meer informatie), gebruikt u `3.0` voor `<smb-version>`. Voor Linux-distributies die bieden geen ondersteuning voor SMB 3.0 met versleuteling, gebruikt u `2.1` voor `<smb-version>`. Houd er rekening mee dat een Azure-bestandsshare alleen buiten een Azure-regio kan worden gekoppeld (met inbegrip van on-premises of in een andere Azure-regio) met SMB 3.0. 

    ```bash
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> <mount-point> cifs nofail,vers=<smb-version>,credentials=/etc/smbcredentials/<storage-account-name>.cred,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> U kunt `sudo mount -a` naar de Azure-bestandsshare koppelen na het bewerken van `/etc/fstab` in plaats van opnieuw opstarten.

## <a name="feedback"></a>Feedback
Linux-gebruikers die we horen graag van u!

De Azure-bestanden voor Linux-gebruikers groep biedt een forum voor u feedback delen evalueren en vast File storage in Linux. E-mailbericht [Linux-gebruikers van Azure-bestanden](mailto:azurefileslinuxusers@microsoft.com) voor deelname aan de gebruikers-groep.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende koppelingen voor meer informatie over Azure Files.
* [Inleiding tot Azure Files](storage-files-introduction.md)
* [Implementatie van Azure Files plannen](storage-files-planning.md)
* [Veelgestelde vragen](../storage-files-faq.md)
* [Problemen oplossen](storage-troubleshoot-linux-file-connection-problems.md)
