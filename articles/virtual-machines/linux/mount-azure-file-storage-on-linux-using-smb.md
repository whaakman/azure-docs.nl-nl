---
title: Azure File storage koppelen op virtuele Linux-machines met behulp van SMB | Microsoft Docs
description: Hoe u Azure File storage koppelen in Linux-VM's met behulp van SMB met de Azure CLI
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: cynthn
ms.openlocfilehash: 4b3bba1da5238655ca749f6464c539e53ca48f27
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540054"
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>Azure File storage koppelen op virtuele Linux-machines met behulp van SMB

In dit artikel ziet u hoe u de Azure File storage-service op een Linux-VM met behulp van een SMB-koppelen met de Azure CLI. Azure File storage biedt bestandsshares in de cloud met behulp van de SMB-protocol. 

File storage biedt bestandsshares in de cloud die gebruikmaken van de SMB-protocol. U kunt een bestandsshare koppelen vanuit elk besturingssysteem dat ondersteuning biedt voor SMB 3.0. Als u een SMB-koppelen in Linux gebruikt, krijgt u eenvoudig back-ups naar een robuuste, permanente archiveren opslaglocatie die wordt ondersteund door een SLA.

Bestanden van een virtuele machine verplaatsen naar een SMB-koppeling die wordt gehost op File storage is een uitstekende manier om op te sporen Logboeken. De dezelfde SMB-share kan lokaal worden gekoppeld aan uw Mac, Linux of Windows-werkstation. SMB is de beste oplossing voor het streamen van Linux of toepassingslogboeken in realtime, omdat het SMB-protocol voor het afhandelen van dergelijke rechten zware logboekregistratie niet is ingebouwd. Een speciale, geïntegreerde logboekregistratie laag hulpprogramma zoals Fluentd zou een betere keuze dan SMB zijn voor het verzamelen van Linux- en uitvoer logboekregistratie-toepassing.

Deze handleiding is vereist dat u de Azure CLI versie 2.0.4 gebruikt of hoger. Voer **az --version** uit om de versie op te vragen. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de naam *myResourceGroup* in de *VS-Oost* locatie.

```bash
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Create a storage account

Maak een nieuw opslagaccount, binnen de resourcegroep die u hebt gemaakt, met behulp van [az storage-account maken](/cli/azure/storage/account). Dit voorbeeld maakt u een opslagaccount met de naam *mySTORAGEACCT\<willekeurig getal >* en plaatst u de naam van dat opslagaccount in de variabele **STORAGEACCT**. Namen van opslagaccounts moeten uniek zijn, met behulp van `$RANDOM` enkele toegevoegd aan het einde uniek te maken.

```bash
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

## <a name="get-the-storage-key"></a>De opslagsleutel ophalen

Wanneer u een opslagaccount maakt, worden de accountsleutels in paren gemaakt zodat ze zonder enige serviceonderbreking kunnen worden gedraaid. Wanneer u naar de tweede sleutel in het paar overschakelt, maakt u een nieuw sleutelpaar. Nieuwe storage-accountsleutels worden altijd aangemaakt in paren, zodat u altijd ten minste één niet-gebruikte opslagaccountsleutel gereed om te maken.

Weergeven met behulp van sleutels voor de opslagaccount [az storage account sleutels lijst](/cli/azure/storage/account/keys). In dit voorbeeld wordt de waarde van sleutel 1 in de **STORAGEKEY** variabele.

```bash
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-a-file-share"></a>Een bestandsshare maken

Maak het bestand met de opslag delen met [az storage share maken](/cli/azure/storage/share). 

De namen van shares moeten worden alle kleine letters, cijfers en afbreekstreepjes maar mag niet beginnen met een afbreekstreepje. Zie [Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) voor meer informatie over de naamgeving van bestandsshares en bestanden.

Dit voorbeeld maakt u een share met de naam *myshare* met een quotum van 10 GiB. 

```bash
az storage share create --name myshare \
    --quota 10 \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY
```

## <a name="create-a-mount-point"></a>Maken van een koppelpunt

Voor het koppelen van de Azure-bestandsshare op uw Linux-computer, moet u ervoor dat u hebt de **cifs-utils** pakket is geïnstalleerd. Zie voor installatie-instructies [Installeer het pakket cifs-utils voor uw Linux-distributie](../../storage/files/storage-how-to-use-files-linux.md#install-cifs-utils).

Azure Files maakt gebruik van SMB-protocol, dat communiceert via TCP-poort 445.  Als u problemen bij het koppelen van uw Azure-bestandsshare ondervindt, zorg er dan voor dat uw firewall TCP-poort 445 niet blokkeert.


```bash
mkdir -p /mnt/MyAzureFileShare
```

## <a name="mount-the-share"></a>De bestandsshare koppelen

Koppel de Azure-bestandsshare naar de lokale map. 

```bash
sudo mount -t cifs //$STORAGEACCT.file.core.windows.net/myshare /mnt/MyAzureFileShare -o vers=3.0,username=$STORAGEACCT,password=$STORAGEKEY,dir_mode=0777,file_mode=0777,serverino
```

De bovenstaande opdracht maakt gebruik van de [koppelen](https://linux.die.net/man/8/mount) opdracht voor het koppelen van de Azure-bestandsshare en de opties die specifiek zijn voor [cifs](https://linux.die.net/man/8/mount.cifs). Met name de file_mode en dir_mode opties setbestanden en mappen op machtiging `0777`. De `0777` machtiging kunt lezen, schrijven en uitvoeren van machtigingen voor alle gebruikers. U kunt deze machtigingen wijzigen door de waarden vervangen door andere [chmod machtigingen](https://en.wikipedia.org/wiki/Chmod). U kunt ook andere [cifs](https://linux.die.net/man/8/mount.cifs) opties zoals groeps-id of gebruikers-id. 


## <a name="persist-the-mount"></a>De koppeling behouden

Wanneer u de Linux-VM opnieuw opstarten, is de gekoppelde SMB-share is ontkoppeld tijdens het afsluiten. Als u wilt koppelen bij het opstarten van de SMB-share, moet u een regel toegevoegd aan de Linux-/ etc/fstab. Linux maakt gebruik van het fstab-bestand om de bestandssystemen die nodig is om te koppelen tijdens het opstarten weer te geven. Toevoegen van de SMB-share, zorgt u ervoor dat de File storage-share een permanent gekoppelde bestandssysteem voor de Linux-VM is. De SMB-share van File storage toe te voegen aan een nieuwe virtuele machine is mogelijk wanneer u cloud-init gebruiken.

```bash
//myaccountname.file.core.windows.net/mystorageshare /mnt/mymountpoint cifs vers=3.0,username=mystorageaccount,password=myStorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```
Voor een betere beveiliging in een productieomgeving, moet u uw referenties buiten fstab opslaan.

## <a name="next-steps"></a>Volgende stappen

- [Cloud-init gebruiken voor het aanpassen van een Linux-VM tijdens het maken van](using-cloud-init.md)
- [Een schijf toevoegen aan een virtuele Linux-machine](add-disk.md)
- [Schijven op een Linux-VM versleutelen met behulp van de Azure CLI](encrypt-disks.md)

