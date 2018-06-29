---
title: Koppelpunt Azure File storage in virtuele Linux-machines met SMB | Microsoft Docs
description: Het koppelen van Azure File storage op Linux VM's met behulp van SMB met de Azure CLI
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
ms.openlocfilehash: 2019324030b2e4c469d0b9ba937fb40a9d0675f1
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099708"
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>Koppelpunt Azure File storage in virtuele Linux-machines met SMB


In dit artikel leest u hoe de Azure File storage-service gebruiken op een Linux-VM met een SMB-koppelen met de Azure CLI. Azure File storage biedt bestandsshares in de cloud met behulp van het standaard SMB-protocol. 

File storage biedt bestandsshares in de cloud die het standaard SMB-protocol gebruiken. U kunt een bestandsshare koppelen vanuit elk besturingssysteem dat ondersteuning biedt voor SMB 3.0. Als u een SMB-koppelen op Linux gebruikt, krijgt u eenvoudig back-ups naar een robuuste, permanente archiveren opslaglocatie die wordt ondersteund door een SLA.

Verplaatsen van bestanden van een virtuele machine naar een SMB-koppelpunt die wordt gehost op File storage is een uitstekende manier om de logboeken voor foutopsporing. De dezelfde SMB-share worden gekoppeld, lokaal naar uw Mac, Linux of Windows-werkstation. SMB is de beste oplossing voor het streamen van Linux of toepassingslogboeken in real-time, omdat het SMB-protocol niet is gebouwd voor het afhandelen van deze rechten zware logboekregistratie. Een speciale, uniforme logboekregistratie laag hulpprogramma zoals Fluentd zou een betere keuze dan SMB zijn voor het verzamelen van Linux- en uitvoer logboekregistratie toepassing.

Deze handleiding is vereist dat u de Azure CLI versie 2.0.4 uitvoert of hoger. Voer **az --version** uit om de versie op te vragen. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de naam *myResourceGroup* in de *VS-Oost* locatie.

```bash
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Create a storage account

Maak een nieuw opslagaccount binnen de resourcegroep die u hebt gemaakt, met [az storage-account maken](/cli/azure/storage/account#create). In dit voorbeeld maakt u een opslagaccount met de naam *mySTORAGEACCT<random number>*  en plaatst u de naam van het storage-account in de variabele **STORAGEACCT**. Namen van opslagaccounts moeten uniek zijn, met behulp van `$RANDOM` een cijfer toegevoegd aan het einde uniek te maken.

```bash
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

## <a name="get-the-storage-key"></a>De opslagsleutel ophalen

Wanneer u een opslagaccount maakt, worden de sleutels in paren worden gemaakt zodat ze kunnen worden gedraaid zonder onderbreking van de service. Wanneer u naar de tweede sleutel in het paar overschakelt, maakt u een nieuw sleutelpaar. Nieuwe toegangscodes voor opslag worden altijd gemaakt in paren, zodat u altijd ten minste één niet-gebruikte opslagaccountsleutel gereed om te activeren.

De opslagaccountsleutels met weergeven [lijst met opslagaccounts die sleutels az](/cli/azure/storage/account/keys#list). In dit voorbeeld Slaat de waarde van sleutel 1 in de **STORAGEKEY** variabele.

```bash
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-a-file-share"></a>Een bestandsshare maken

Maak het bestand opslag delen met [az storage-share maken](/cli/azure/storage/share#create). 

De namen van bestandsshares moeten worden alle kleine letters, cijfers en afbreekstreepjes één maar mag niet beginnen met een afbreekstreepje. Zie [Naming and Referencing Shares, Directories, Files, and Metadata](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.

In dit voorbeeld maakt u een share met de naam *mijnshare* met een target 10 GiB. 

```bash
az storage share create --name myshare \
    --quota 10 \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY
```

## <a name="create-a-mount-point"></a>Maken van een koppelpunt

De Azure-bestandsshare koppelen op de Linux-computer, moet u om ervoor te zorgen dat u hebt de **cifs utils** pakket is geïnstalleerd. Zie voor installatie-instructies [Installeer het pakket cifs-utils voor uw Linux-distributie](../../storage/files/storage-how-to-use-files-linux.md#install-cifs-utils).

Azure Files maakt gebruik van SMB-protocol communiceert via TCP-poort 445.  Als u problemen bij het koppelen van uw Azure-bestandsshare ondervindt, zorg er dan voor dat uw firewall niet wordt geblokkeerd door TCP-poort 445.


```bash
mkdir -p /mnt/MyAzureFileShare
```

## <a name="mount-the-share"></a>De share koppelen

Koppel de Azure-bestandsshare naar een lokale map. 

```bash
sudo mount -t cifs //$STORAGEACCT.file.core.windows.net/myshare /mnt/MyAzureFileShare -o vers=3.0,username=$STORAGEACCT,password=$STORAGEKEY,dir_mode=0777,file_mode=0777,serverino
```



## <a name="persist-the-mount"></a>De koppeling behouden

Wanneer u de Linux-VM opnieuw opstart, wordt tijdens het afsluiten van de gekoppelde SMB-share ontkoppeld. Als u wilt opnieuw koppelen van de SMB-share op opstarten, moet u een regel toegevoegd aan de /etc/fstab Linux. Linux gebruikt het fstab-bestand voor een lijst met de bestandssystemen die nodig is om te koppelen tijdens het opstarten. Het toevoegen van de SMB-share zorgt ervoor dat de File storage-share een permanent gekoppelde bestandssysteem voor de Linux-VM. De File storage SMB-share toe te voegen aan een nieuwe virtuele machine is mogelijk wanneer u cloud-init gebruiken.

```bash
//myaccountname.file.core.windows.net/mystorageshare /mnt/mymountpoint cifs vers=3.0,username=mystorageaccount,password=myStorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```
Voor een betere beveiliging in productieomgevingen moet u uw referenties buiten fstab opslaan.

## <a name="next-steps"></a>Volgende stappen

- [Gebruik van cloud-init voor het aanpassen van een Linux-VM tijdens het maken van](using-cloud-init.md)
- [Een schijf toevoegen aan een virtuele Linux-machine](add-disk.md)
- [Versleutelen van schijven op een Linux-VM met behulp van de Azure CLI](encrypt-disks.md)

