---
title: Hoe u Azure Blob-opslag als een bestandssysteem koppelen in Linux | Microsoft Docs
description: Een Azure Blob storage-container met FUSE koppelen in Linux
services: storage
author: seguler
ms.service: storage
ms.topic: article
ms.date: 10/11/2018
ms.author: seguler
ms.openlocfilehash: 2374875512bba55409ef43906acb20238c77158f
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53268458"
---
# <a name="how-to-mount-blob-storage-as-a-file-system-with-blobfuse"></a>Over het koppelen van Blob-opslag als een bestandssysteem met blobfuse

## <a name="overview"></a>Overzicht
[Blobfuse](https://github.com/Azure/azure-storage-fuse) is een stuurprogramma van het virtuele bestandssysteem voor Azure Blob-opslag. Blobfuse kunt u uw bestaande blok-blobgegevens in uw storage-account via het bestandssysteem van Linux. Azure Blob-opslag heeft is een object storage-service en een hiërarchische naamruimte. Blobfuse biedt deze naamruimte met behulp van het schema van de virtuele map met de slashes (/) als scheidingsteken.  

Deze handleiding wordt beschreven hoe u blobfuse gebruikt, en het koppelen van een Blob storage-container op Linux- en toegang tot gegevens. Lees de details in voor meer informatie over blobfuse [de opslagplaats blobfuse](https://github.com/Azure/azure-storage-fuse).

> [!WARNING]
> Blobfuse niet 100% POSIX naleving garanderen als gewoon vertaalt aanvragen in [Blob REST-API's](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api). Rename-bewerkingen zijn bijvoorbeeld atomic in POSIX, maar niet in blobfuse.
> Voor een volledige lijst met verschillen tussen een systeemeigen bestandssysteem en blobfuse, gaat u naar [opslagplaats van de broncode blobfuse](https://github.com/azure/azure-storage-fuse).
> 

## <a name="install-blobfuse-on-linux"></a>Blobfuse installeren in Linux
Blobfuse binaire bestanden zijn beschikbaar op [de Microsoft-software-opslagplaatsen voor Linux](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software) voor Ubuntu en RHEL-distributies. Configureren voor het installeren van blobfuse op deze distributies, een van de opslagplaatsen in de lijst. U kunt ook maken met de binaire bestanden vanuit de bron-code hieronder de [Azure Storage-installatiestappen](https://github.com/Azure/azure-storage-fuse/wiki/1.-Installation#option-2---build-from-source) als er geen binaire bestanden beschikbaar voor uw distributie zijn.

Blobfuse ondersteunt installatie op Ubuntu 14.04 en 16.04. Voer deze opdracht om ervoor te zorgen dat u een van deze versies geïmplementeerd hebt:
```
lsb_release -a
```

### <a name="configure-the-microsoft-package-repository"></a>De opslagplaats van het Microsoft-pakket configureren
Configureer de [Linux Pakketopslagplaats voor Microsoft-producten](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software).

Een voorbeeld: op een distributiepunt Enterprise Linux 6:
```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
```

Op dezelfde manier, wijzig de URL in `.../rhel/7/...` om te verwijzen naar een Enterprise Linux 7-distributie.

Een ander voorbeeld op een distributiepunt Ubuntu 14.04:
```bash
wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
```

Op dezelfde manier, wijzig de URL in `.../ubuntu/16.04/...` om te verwijzen naar een distributiepunt Ubuntu 16.04.

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
Blobfuse biedt systeemeigen prestaties doordat een tijdelijk pad in het bestandssysteem van de buffer en alle geopende bestanden in de cache. Voor dit tijdelijke pad, kiest u de meeste goed presterende schijf of gebruik een RAM-schijf voor de beste prestaties. 

> [!NOTE]
> Blobfuse slaat alle inhoud van het bestand openen in de tijdelijke pad. Zorg ervoor dat er voldoende ruimte voor alle geopende bestanden. 
> 

### <a name="optional-use-a-ramdisk-for-the-temporary-path"></a>(Optioneel) Een RAM-schijf voor de tijdelijke pad gebruiken
Het volgende voorbeeld wordt een RAM-schijf van 16 GB en een map voor blobfuse. Kies de grootte op basis van uw behoeften. Deze ramdisk kunt blobfuse naar geopende bestanden tot 16 GB groot. 
```bash
sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
sudo mkdir /mnt/ramdisk/blobfusetmp
sudo chown <youruser> /mnt/ramdisk/blobfusetmp
```

### <a name="use-an-ssd-as-a-temporary-path"></a>Een SSD te gebruiken als een tijdelijke pad
In Azure, kunt u de tijdelijke schijven (SSD) beschikbaar gebruiken voor uw virtuele machines voor een buffer met lage latentie voor blobfuse. In Ubuntu-distributies, deze tijdelijke schijf is gekoppeld op ' / mnt'. In Red Hat- en CentOS-distributies, de schijf is gekoppeld op ' / mnt/resource /'.

Zorg ervoor dat de gebruiker heeft toegang tot het tijdelijke pad:
```bash
sudo mkdir /mnt/resource/blobfusetmp
sudo chown <youruser> /mnt/resource/blobfusetmp
```

### <a name="configure-your-storage-account-credentials"></a>De referenties van uw storage-account configureren
Blobfuse moet uw referenties worden opgeslagen in een tekstbestand met de volgende indeling: 

```
accountName myaccount
accountKey storageaccesskey
containerName mycontainer
```

Als u dit bestand hebt gemaakt, zorg ervoor dat de toegang beperken, zodat er geen andere gebruiker kan worden gelezen.
```bash
chmod 700 fuse_connection.cfg
```

> [!NOTE]
> Als u het configuratiebestand op Windows hebt gemaakt, controleert u of om uit te voeren `dos2unix` opschonen en het bestand converteren naar Unix-indeling. 
>

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
sudo blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```

U hebt nu toegang tot uw blok-BLOB via de gewone bestandssysteem-API's. De gebruiker die de directory koppelt is de enige persoon die hebben standaard toegang tot deze, die het beveiligt de toegang. Voor toegang tot alle gebruikers, kunt u koppelen via de optie ```-o allow_other```. 

```bash
cd ~/mycontainer
mkdir test
echo "hello world" > test/blob.txt
```

## <a name="next-steps"></a>Volgende stappen

* [Blobfuse-startpagina](https://github.com/Azure/azure-storage-fuse#blobfuse)
* [Rapport blobfuse problemen](https://github.com/Azure/azure-storage-fuse/issues) 

