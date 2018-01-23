---
title: Het koppelen van Azure Blob-opslag als een bestandssysteem op Linux | Microsoft Docs
description: Koppelen van een Azure Blob storage-container met ZEKERING op Linux
services: storage
documentationcenter: linux
author: seguler
manager: jahogg
ms.service: storage
ms.devlang: bash
ms.topic: article
ms.date: 01/19/2018
ms.author: seguler
ms.openlocfilehash: 299b96c783fb3606347bb448d00d44f0071da429
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="how-to-mount-blob-storage-as-a-file-system-with-blobfuse-preview"></a>Het koppelen van Blob-opslag als een bestandssysteem met blobfuse (Preview)

## <a name="overview"></a>Overzicht
[Blobfuse](https://github.com/Azure/azure-storage-fuse) is een stuurprogramma van het virtuele bestandssysteem voor Azure Blob Storage, hebt u toegang tot uw bestaande blok-blob-gegevens in uw opslagaccount met het Linux-bestandssysteem. Azure Blob Storage is een object storage-service en daarom geen een hiërarchische naamruimte. Blobfuse biedt deze naamruimte met behulp van het schema van de virtuele map met het gebruik van slashes '/' als scheidingsteken.  

Deze handleiding wordt beschreven hoe u blobfuse gebruiken en het koppelen van een Blob storage-container op Linux-en toegangsgegevens. Lees voor meer informatie over blobfuse, de details in [de opslagplaats blobfuse](https://github.com/Azure/azure-storage-fuse).

> [!WARNING]
> Blobfuse garandeert geen 100% POSIX-compatibiliteit, zoals aanvragen in gewoon vertaalt [Blob REST-API's](https://docs.microsoft.com/en-us/rest/api/storageservices/blob-service-rest-api). Wijzig de naam van bewerkingen zijn bijvoorbeeld atomaire POSIX, maar niet in blobfuse.
> Voor een volledige lijst met verschillen tussen een systeemeigen bestandssysteem en blobfuse, gaat u naar [de opslagplaats blobfuse source code](https://github.com/azure/azure-storage-fuse).
> 

## <a name="install-blobfuse-on-linux"></a>Blobfuse installeren op Linux
Blobfuse binaire bestanden zijn beschikbaar op [de Microsoft-software-opslagplaatsen voor Linux](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software). Om te installeren blobfuse, configureert u een van deze opslagplaatsen.

### <a name="configure-the-microsoft-package-repository"></a>Configureren van de opslagplaats van het Microsoft-pakket
Configureer de [opslagplaats Linux-pakket voor Microsoft-producten](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software).

Als u bijvoorbeeld op een distributiepunt Enterprise Linux 6:
```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
```

Op dezelfde manier, wijzig de url in `.../rhel/7/...` om te verwijzen naar een distributiepunt Enterprise Linux 7.

Een ander voorbeeld van een virtuele Ubuntu 14.04:
```bash
wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
```

Op dezelfde manier, wijzig de url in `.../ubuntu/16.04/...` om te verwijzen naar een distributiepunt Ubuntu 16.04.

### <a name="install-blobfuse"></a>Blobfuse installeren

Op een distributiepunt Ubuntu/Debian:
```bash
sudo apt-get install blobfuse
```

Op een distributiepunt Enterprise Linux:
```bash
sudo yum install blobfuse
```

## <a name="prepare-for-mounting"></a>Voorbereiden voor koppelen
Een tijdelijke pad vereist Blobfuse in het bestandssysteem naar de buffer en alle geopende bestanden, waardoor biedt systeemeigen prestaties in de cache. Kies de meeste zodat schijf voor dit tijdelijke pad of een ramdisk gebruiken voor de beste prestaties. 

> [!NOTE]
> Alle inhoud van het geopende bestand Blobfuse opgeslagen in het tijdelijke pad. Zorg ervoor dat er genoeg ruimte is om te voorzien in alle geopende bestanden. 
> 

### <a name="optional-use-a-ramdisk-for-the-temporary-path"></a>(Optioneel) Een ramdisk gebruiken voor het tijdelijke pad
Het volgende voorbeeld wordt een RAM-schijf van 16 GB, alsmede een map voor blobfuse maken. Kies de grootte op basis van uw behoeften. Deze ramdisk kunt blobfuse naar geopende bestanden maximaal 16 GB groot. 
```bash
sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
sudo mkdir /mnt/ramdisk/blobfusetmp
sudo chown <youruser> /mnt/ramdisk/blobfusetmp
```

### <a name="use-an-ssd-for-temporary-path"></a>Een SSD voor tijdelijk pad gebruiken
In Azure, mag u de tijdelijke schijven (SSD) beschikbaar op uw virtuele machines te voorzien in een buffer lage latentie blobfuse. In Ubuntu distributies, deze kortstondige schijf is gekoppeld aan ' / mnt' dat deze is gekoppeld aan ' / mnt/resource /' in RedHat en CentOS-distributies.

Zorg ervoor dat de gebruiker heeft toegang tot het tijdelijke pad:
```bash
sudo mkdir /mnt/resource/blobfusetmp
sudo chown <youruser> /mnt/resource/blobfusetmp
```

### <a name="configure-your-storage-account-credentials"></a>Uw opslagaccountreferenties configureren
Blobfuse is vereist dat uw referenties worden opgeslagen in een tekstbestand in de volgende indeling: 

```
accountName myaccount
accountKey myaccesskey==
containerName mycontainer
```

Als u dit bestand hebt gemaakt, zorg ervoor dat de toegang beperken zodat andere gebruikers geen kunt lezen.
```bash
chmod 700 fuse_connection.cfg
```

### <a name="create-an-empty-directory-for-mounting"></a>Maak een lege map voor koppelen
```bash
mkdir ~/mycontainer
```

## <a name="mount"></a>koppelen

> [!NOTE]
> Controleer voor een volledige lijst van koppelpunten opties [de opslagplaats blobfuse](https://github.com/Azure/azure-storage-fuse#mount-options).  
> 

Voer de volgende opdracht met uw gebruikers om te koppelen blobfuse. Met deze opdracht koppelt de container die is opgegeven in ' / path/to/fuse_connection.cfg' naar de locatie ' / mycontainer'.

```bash
blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```

U moet nu toegang hebben tot uw blok-blobs via het reguliere bestandssysteem API's. Houd er rekening mee dat de gekoppelde map alleen kan worden gebruikt door de gebruiker koppelen, die u de toegang beveiligt. Als u toegang toestaan voor alle gebruikers wilt, kunt u koppelen via de optie ```-o allow_other```. 

```bash
cd ~/mycontainer
mkdir test
echo "hello world" > test/blob.txt
```

## <a name="next-steps"></a>Volgende stappen

* [Blobfuse-startpagina](https://github.com/Azure/azure-storage-fuse#blobfuse)
* [Rapport blobfuse problemen](https://github.com/Azure/azure-storage-fuse/issues) 

