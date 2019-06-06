---
title: Azure Files gebruiken met Linux | Microsoft Docs
description: Leer hoe u een Azure-bestandsshare koppelen via SMB op Linux.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 03/29/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 73ed98bf950f7c9f52e2b8eeb431fe4b36bfe324
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427920"
---
# <a name="use-azure-files-with-linux"></a>Azure Files gebruiken met Linux

[Azure Files ](storage-files-introduction.md) is het eenvoudig te gebruiken cloudbestandssysteem van Microsoft. Azure-bestandsshares kunnen worden gekoppeld in Linux-distributies, met de [SMB-kernel-client](https://wiki.samba.org/index.php/LinuxCIFS). In dit artikel ziet u een Azure-bestandsshare koppelen op twee manieren: op aanvraag met de `mount` opdracht en op opstarten met het maken van een vermelding in `/etc/fstab`.

> [!NOTE]  
> Als u wilt koppelen van een Azure-bestandsshare buiten de Azure-regio waarin die deze wordt gehost in, bijvoorbeeld on-premises of in een andere Azure-regio moet het besturingssysteem ondersteuning bieden voor de versleutelingsfunctionaliteit van SMB 3.0.

## <a name="prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Vereisten voor het koppelen van een Azure-bestandsshare met Linux en de cifs-utils-pakket
<a id="smb-client-reqs"></a>

* **Een bestaande Azure storage-account en bestandsshare**: Om te voltooien in dit artikel, moet u een opslagaccount en een bestandsshare hebben. Als u dit nog niet hebt gemaakt, ziet u een van onze quickstarts over dit onderwerp: [Maken van bestandsshares - CLI](storage-how-to-use-files-cli.md).

* **Het storage-accountnaam en de sleutel** u de storage-accountnaam en de sleutel nodig om te voltooien in dit artikel. Als u hebt één met de CLI-snelstartgids moet u ze al hebt gemaakt, anders raadpleegt u de CLI-snelstartgids die eerder is gekoppeld als u wilt meer informatie over het ophalen van de sleutel van uw opslagaccount.

* **Kies een Linux-distributie op basis van de behoeften van uw koppelen.**  
      Azure Files kunnen worden gekoppeld via het SMB 2.1 als SMB 3.0. Voor verbindingen die afkomstig zijn van clients on-premises of in andere Azure-regio's, moet u SMB 3.0; Azure Files weigert SMB 2.1 (of SMB 3.0 zonder versleuteling). Als u toegang wilt de Azure-bestandsshare vanaf een virtuele machine binnen dezelfde Azure-regio tot krijgen, u mogelijk toegang tot de bestandsshare met behulp van SMB 2.1 als en alleen als *veilige overdracht vereist* is uitgeschakeld voor het opslagaccount die als host fungeert voor de Azure-bestandsshare. Er is altijd beter om veilige overdracht vereisen en het gebruik van alleen SMB 3.0 met versleuteling.

    Ondersteuning voor SMB 3.0-versleuteling is geïntroduceerd in Linux kernelversie 4.11 en is backported tot oudere kernelversies voor populaire Linux-distributies. Op het moment van publicatie van dit document ondersteunen de volgende distributies vanuit de Azure-galerie koppelen optie opgegeven in de headers-tabel. 

* **Minimaal aanbevolen versies met de bijbehorende mount-mogelijkheden (SMB-versie 2.1 vs SMB-versie 3.0)**    

    |   | SMB 2.1 <br>(Koppelingen op VM's binnen dezelfde Azure-regio) | SMB 3.0 <br>(Koppelingen van locatie en regio-overschrijdende) |
    | --- | :---: | :---: |
    | Ubuntu Server | 14.04+ | 16.04+ |
    | RHEL | 7+ | 7.5+ |
    | CentOS | 7+ |  7.5+ |
    | Debian | 8+ |   |
    | openSUSE | 13.2+ | 42.3+ |
    | SUSE Linux Enterprise Server | 12 | 12 SP3+ |

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

* **Zorg ervoor dat poort 445 open is**: SMB communiceert via TCP-poort 445 - controleer of de TCP-poort 445 van de clientcomputer niet door uw firewall wordt geblokkeerd.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>De Azure file share op aanvraag met koppelen `mount`

1. **[Installeer de cifs-utils-pakket voor uw Linux-distributie](#install-cifs-utils)** .

1. **Maak een map voor het koppelpunt**: Een map voor een koppelpunt overal kan worden gemaakt op het bestandssysteem, maar het algemene conventie om dit te maken onder een nieuwe map is. Bijvoorbeeld, de volgende opdracht maakt u een nieuwe map, Vervang **< naam_opslagaccount >** en **< file_share_name >** met de juiste informatie voor uw omgeving:

    ```bash
    mkdir -p <storage_account_name>/<file_share_name>
    ```

1. **Gebruik de koppelopdracht om de Azure-bestandsshare koppelen**: Vervang **< naam_opslagaccount >** , **< sharenaam > hebben**, **< smb_version >** , **< storage_account_key >** , en **< mount_point >** met de juiste informatie voor uw omgeving. Als uw Linux-distributie biedt ondersteuning voor SMB 3.0 met-codering (Zie [clientvereisten voor informatie over SMB](#smb-client-reqs) voor meer informatie), gebruikt u **3.0** voor **< smb_version >** . Voor Linux-distributies die bieden geen ondersteuning voor SMB 3.0 met versleuteling, gebruikt u **2.1** voor **< smb_version >** . Een Azure-bestandsshare kan alleen worden gekoppeld buiten een Azure-regio (met inbegrip van on-premises of in een andere Azure-regio) met SMB 3.0. 

    ```bash
    sudo mount -t cifs //<storage_account_name>.file.core.windows.net/<share_name> <mount_point> -o vers=<smb_version>,username=<storage_account_name>,password=<storage_account_key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Wanneer u klaar bent met behulp van de Azure-bestandsshare, mag u `sudo umount <mount_point>` ontkoppelen van de share.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Maken van een permanente koppelpunt voor de Azure-bestandsshare met `/etc/fstab`

1. **[Installeer de cifs-utils-pakket voor uw Linux-distributie](#install-cifs-utils)** .

1. **Maak een map voor het koppelpunt**: Een map voor een koppelpunt overal kan worden gemaakt op het bestandssysteem, maar het algemene conventie om dit te maken onder een nieuwe map is. Houd er rekening mee het absolute pad van de map waar u dit hebt gemaakt. Bijvoorbeeld, de volgende opdracht maakt u een nieuwe map, Vervang **< naam_opslagaccount >** en **< file_share_name >** met de juiste informatie voor uw omgeving.

    ```bash
    sudo mkdir -p <storage_account_name>/<file_share_name>
    ```

1. **Maak een referentie-bestand voor het opslaan van de gebruikersnaam (naam van het opslagaccount) en het wachtwoord (de opslagaccountsleutel) voor de bestandsshare.** Vervang **< naam_opslagaccount >** en **< storage_account_key >** met de juiste informatie voor uw omgeving.

    ```bash
    if [ ! -d "/etc/smbcredentials" ]; then
    sudo mkdir /etc/smbcredentials
    fi
    if [ ! -f "/etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred" ]; then
    sudo bash -c 'echo "username=<STORAGE ACCOUNT NAME>" >> /etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred'
    sudo bash -c 'echo "password=7wRbLU5ea4mgc<DRIVE LETTER>PIpUCNcuG9gk2W4S2tv7p0cTm62wXTK<DRIVE LETTER>CgJlBJPKYc4VMnwhyQd<DRIVE LETTER>UT<DRIVE LETTER>yR5/RtEHyT/EHtg2Q==" >> /etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred'
    fi
    ```

1. **Machtigingen voor de referentie-bestand wijzigen zodat alleen hoofdmap kan lezen of het wachtwoordbestand te wijzigen.** Aangezien de opslagaccountsleutel in feite een absoluut beheerderswachtwoord voor het opslagaccount is, is instellen van de machtigingen voor het bestand zodanig dat alleen de hoofd-toegankelijk belangrijk, waarbij lagere bevoegdheid gebruikers kunnen de toegangssleutel van het opslagaccount niet ophalen.   

    ```bash
    sudo chmod 600 /etc/smbcredentials/<storage_account_name>.cred
    ```

1. **Gebruik de volgende opdracht toe te voegen van de volgende regel om `/etc/fstab`** : Vervang **< naam_opslagaccount >** , **< sharenaam > hebben**, **< smb_version >** , en **< mount_point >** met de juiste informatie voor uw omgeving. Als uw Linux-distributie biedt ondersteuning voor SMB 3.0 met-codering (Zie [clientvereisten voor informatie over SMB](#smb-client-reqs) voor meer informatie), gebruikt u **3.0** voor **< smb_version >** . Voor Linux-distributies die bieden geen ondersteuning voor SMB 3.0 met versleuteling, gebruikt u **2.1** voor **< smb_version >** . Een Azure-bestandsshare kan alleen worden gekoppeld buiten een Azure-regio (met inbegrip van on-premises of in een andere Azure-regio) met SMB 3.0.

    ```bash
    sudo bash -c 'echo "//<STORAGE ACCOUNT NAME>.file.core.windows.net/<FILE SHARE NAME> /mount/<STORAGE ACCOUNT NAME>/<FILE SHARE NAME> cifs nofail,vers=3.0,credentials=/etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'

    sudo mount /mount/<STORAGE ACCOUNT NAME>/<FILE SHARE NAME>
    ```

> [!Note]  
> U kunt `sudo mount -a` naar de Azure-bestandsshare koppelen na het bewerken van `/etc/fstab` in plaats van opnieuw opstarten.

## <a name="feedback"></a>Feedback

Linux-gebruikers die we horen graag van u!

De Azure-bestanden voor Linux-gebruikers groep biedt een forum voor u feedback delen evalueren en vast File storage in Linux. E-mailbericht [Linux-gebruikers van Azure-bestanden](mailto:azurefileslinuxusers@microsoft.com) voor deelname aan de gebruikers-groep.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende koppelingen voor meer informatie over Azure Files:

* [Implementatie van Azure Files plannen](storage-files-planning.md)
* [Veelgestelde vragen](../storage-files-faq.md)
* [Problemen oplossen](storage-troubleshoot-linux-file-connection-problems.md)
