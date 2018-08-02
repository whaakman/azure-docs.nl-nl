---
title: Hoe u Azure Blob-opslag als een bestandssysteem koppelen in Linux | Microsoft Docs
description: Een Azure Blob storage-container met FUSE koppelen in Linux
services: storage
author: seguler
ms.service: storage
ms.topic: article
ms.date: 05/10/2018
ms.author: seguler
ms.openlocfilehash: 9964aa4d263e0b75eb59b4e1434a9b3f0aac6ea1
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39400439"
---
# <a name="how-to-mount-blob-storage-as-a-file-system-with-blobfuse"></a>Over het koppelen van Blob-opslag als een bestandssysteem met blobfuse

## <a name="overview"></a>Overzicht
[Blobfuse](https://github.com/Azure/azure-storage-fuse) is een stuurprogramma van het virtuele bestandssysteem voor Azure Blob Storage, waarmee u uw bestaande blok-blobgegevens in uw Storage-account via het bestandssysteem van Linux. Azure Blob-opslag is een object storage-service en daarom geen een hiërarchische naamruimte. Blobfuse biedt deze naamruimte met behulp van het schema van de virtuele map met het gebruik van de slashes (/) als scheidingsteken.  

Deze handleiding wordt beschreven hoe u blobfuse gebruikt, en het koppelen van een Blob storage-container op Linux- en toegang tot gegevens. Lees de details in voor meer informatie over blobfuse [de opslagplaats blobfuse](https://github.com/Azure/azure-storage-fuse).

> [!WARNING]
> Blobfuse is geen garantie voor 100% POSIX-naleving als gewoon vertaalt aanvragen in [Blob REST-API's](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api). Rename-bewerkingen zijn bijvoorbeeld atomic in POSIX, maar niet in blobfuse.
> Voor een volledige lijst met verschillen tussen een systeemeigen bestandssysteem en blobfuse, gaat u naar [opslagplaats van de broncode blobfuse](https://github.com/azure/azure-storage-fuse).
> 

## <a name="install-blobfuse-on-linux"></a>Blobfuse installeren in Linux
Blobfuse binaire bestanden zijn beschikbaar op [de Microsoft-software-opslagplaatsen voor Linux](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software). Als u wilt installeren blobfuse, een van deze opslagplaatsen te configureren.

### <a name="configure-the-microsoft-package-repository"></a>De opslagplaats van het Microsoft-pakket configureren
Configureer de [Linux Pakketopslagplaats voor Microsoft-producten](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software).

Een voorbeeld: op een distributiepunt Enterprise Linux 6:
```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
```

Op dezelfde manier, wijzig de url in `.../rhel/7/...` om te verwijzen naar een Enterprise Linux 7-distributie.

Een ander voorbeeld van een Ubuntu 14.04:
```bash
wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
```

Op dezelfde manier, wijzig de url in `.../ubuntu/16.04/...` om te verwijzen naar een distributiepunt Ubuntu 16.04.

### <a name="install-blobfuse"></a>Blobfuse installeren

Op een Ubuntu/Debian-distributiepunt:
```bash
sudo apt-get install blobfuse
```

Op een Enterprise Linux-distributie:
```bash
sudo yum install blobfuse
```

## <a name="prepare-for-mounting"></a>Voorbereiden voor koppelen
Een tijdelijke pad vereist Blobfuse in het bestandssysteem van de buffer en alle geopende bestanden, waarmee de prestaties van de native-achtige biedt in de cache. Voor dit tijdelijke pad, kiest u de meeste goed presterende schijf of gebruik een RAM-schijf voor de beste prestaties. 

> [!NOTE]
> Blobfuse slaat alle inhoud van het bestand openen in de tijdelijke pad. Zorg ervoor dat u er genoeg ruimte is om alle geopende bestanden. 
> 

### <a name="optional-use-a-ramdisk-for-the-temporary-path"></a>(Optioneel) Een RAM-schijf voor de tijdelijke pad gebruiken
Het volgende voorbeeld wordt een RAM-schijf van 16 GB, evenals het maken van een map voor blobfuse. Kies de grootte op basis van uw behoeften. Deze ramdisk kunt blobfuse naar geopende bestanden tot 16 GB groot. 
```bash
sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
sudo mkdir /mnt/ramdisk/blobfusetmp
sudo chown <youruser> /mnt/ramdisk/blobfusetmp
```

### <a name="use-an-ssd-for-temporary-path"></a>Gebruik een SSD voor tijdelijke pad
In Azure, kunt u de tijdelijke schijven (SSD) beschikbaar gebruiken voor uw virtuele machines voor een buffer met lage latentie voor blobfuse. In Ubuntu-distributies, deze tijdelijke schijf is gekoppeld op ' / mnt' dat deze is gekoppeld op ' / mnt/resource /' in Red Hat- en CentOS-distributies.

Zorg ervoor dat de gebruiker heeft toegang tot het tijdelijke pad:
```bash
sudo mkdir /mnt/resource/blobfusetmp
sudo chown <youruser> /mnt/resource/blobfusetmp
```

### <a name="configure-your-storage-account-credentials"></a>De referenties van uw Storage-account configureren
Blobfuse moet uw referenties worden opgeslagen in een tekstbestand met de volgende indeling: 

```
accountName myaccount
accountKey myaccesskey==
containerName mycontainer
```

Als u dit bestand hebt gemaakt, zorg ervoor dat de toegang beperken, zodat er geen andere gebruiker kan worden gelezen.
```bash
chmod 700 fuse_connection.cfg
```

### <a name="create-an-empty-directory-for-mounting"></a>Maak een lege map voor koppelen
```bash
mkdir ~/mycontainer
```

## <a name="mount"></a>Koppelen

> [!NOTE]
> Controleer voor een volledige lijst van koppelingsopties [de opslagplaats blobfuse](https://github.com/Azure/azure-storage-fuse#mount-options).  
> 

Om te koppelen blobfuse, voer de volgende opdracht met uw gebruikers. Met deze opdracht koppelt de container die is opgegeven in ' / path/to/fuse_connection.cfg' naar de locatie ' / mycontainer'.

```bash
blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```

U hebt nu toegang tot uw blok-BLOB via de gewone bestandssysteem-API's. Houd er rekening mee dat de gekoppelde map alleen kan worden geopend door de gebruiker koppelen, die het beveiligt de toegang. Als u toegang toestaan voor alle gebruikers wilt, kunt u koppelen via de optie ```-o allow_other```. 

```bash
cd ~/mycontainer
mkdir test
echo "hello world" > test/blob.txt
```

## <a name="next-steps"></a>Volgende stappen

* [Blobfuse-startpagina](https://github.com/Azure/azure-storage-fuse#blobfuse)
* [Rapport blobfuse problemen](https://github.com/Azure/azure-storage-fuse/issues) 

