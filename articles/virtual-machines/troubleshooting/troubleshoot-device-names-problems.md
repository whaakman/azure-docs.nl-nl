---
title: Wijzigingen van de apparaatnaam Linux-VM in Azure oplossen | Microsoft Docs
description: Wordt uitgelegd waarom wijzigen-namen van Linux-VM-apparaat en hoe u het probleem op te lossen.
services: virtual-machines-linux
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: c21ee4d1d69145a442ad0af05da830548cded237
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50748049"
---
# <a name="troubleshoot-linux-vm-device-name-changes"></a>Wijzigingen van de apparaatnaam Linux-VM oplossen

In dit artikel wordt uitgelegd waarom apparaatnamen wijzigen nadat u een Linux-VM opnieuw opstarten of de gegevensschijven koppelen. Dit artikel bevat ook oplossingen voor dit probleem.

## <a name="symptoms"></a>Symptomen
U kunt de volgende problemen kan optreden bij het uitvoeren van Linux-VM's in Microsoft Azure:

- De virtuele machine kan niet worden opgestart na het opnieuw opstarten.
- Als gegevensschijven worden losgekoppeld en opnieuw gekoppeld, worden de namen van schijf gewijzigd.
- Een toepassing of script dat verwijst naar een schijf met behulp van de naam van het apparaat is mislukt omdat de naam van het apparaat is gewijzigd.

## <a name="cause"></a>Oorzaak

Apparaatpaden in Linux niet worden gegarandeerd consistent via opnieuw wordt opgestart. Apparaatnamen bestaan uit cijfers (letters) van de primaire en secundaire cijfers. Wanneer een nieuw apparaat wordt gedetecteerd door het stuurprogramma voor Linux-opslag, het stuurprogramma wijst primaire en secundaire cijfers van het beschikbare bereik toe aan het apparaat. Wanneer een apparaat wordt verwijderd, wordt de apparaat-getallen worden vrijgemaakt voor hergebruik.

Het probleem treedt op omdat het apparaat scannen in Linux is gepland door de SCSI-subsysteem worden asynchroon uitgevoerd. Als gevolg hiervan kan de padnaam van een apparaat variëren tussen opnieuw wordt opgestart. 

## <a name="solution"></a>Oplossing

U lost dit probleem, gebruikt u permanente naming. Er zijn vier manieren waarop u met de naamgeving van permanente: door het bestandssysteem label, door UUID, -ID of -pad. Het is raadzaam om met behulp van het bestandssysteem label of de UUID voor Azure Linux VM's. 

De meeste distributies bieden de `fstab` **nofail** of **nobootwait** parameters. Deze parameters kunt u een systeem om op te starten als de schijf koppelen bij het opstarten is mislukt. Raadpleeg de documentatie bij uw distributie voor meer informatie over deze parameters. Zie voor meer informatie over het configureren van een Linux-VM voor het gebruik van een UUID wanneer u een gegevensschijf toevoegen [verbinding maken met de Linux-VM om te koppelen van de nieuwe schijf](../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk). 

Wanneer de Azure Linux-agent is geïnstalleerd op een virtuele machine, de agent Udev-regels gebruikt om een set symbolische koppelingen onder het pad /dev/disk/azure samen te stellen. Toepassingen en scripts kunt u de Udev-regels gebruiken om schijven die zijn gekoppeld aan de virtuele machine, samen met het schijftype en schijf LUN's te identificeren.

### <a name="identify-disk-luns"></a>Schijf LUN's identificeren

Toepassingen gebruiken LUN's om alle van de gekoppelde schijven te zoeken en te maken van de symbolische koppelingen. De Azure Linux-agent bevat Udev-regels die symbolische koppelingen van een LUN ingesteld op de apparaten:

    $ tree /dev/disk/azure

    /dev/disk/azure
    ├── resource -> ../../sdb
    ├── resource-part1 -> ../../sdb1
    ├── root -> ../../sda
    ├── root-part1 -> ../../sda1
    └── scsi1
        ├── lun0 -> ../../../sdc
        ├── lun0-part1 -> ../../../sdc1
        ├── lun1 -> ../../../sdd
        ├── lun1-part1 -> ../../../sdd1
        ├── lun1-part2 -> ../../../sdd2
        └── lun1-part3 -> ../../../sdd3                                    
                                 
LUN-gegevens van de Linux-Gast-account is opgehaald met behulp van `lsscsi` of een vergelijkbaar hulpprogramma:

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

De Gast LUN-informatie wordt gebruikt met metagegevens van de Azure-abonnement te vinden van de VHD in Azure Storage dat het partitioneren van gegevens bevat. Bijvoorbeeld, kunt u de `az` CLI:

    $ az vm show --resource-group testVM --name testVM | jq -r .storageProfile.dataDisks                                        
    [                                                                                                                                                                  
    {                                                                                                                                                                  
    "caching": "None",                                                                                                                                              
      "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 1023,                                                                                                                                             
      "image": null,                                                                                                                                                   
    "lun": 0,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-114353",                                                                                                                    
    "vhd": {                                                                                                                                                          
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-114353.vhd"                                                       
    }                                                                                                                                                              
    },                                                                                                                                                                
    {                                                                                                                                                                   
    "caching": "None",                                                                                                                                               
    "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 512,                                                                                                                                              
    "image": null,                                                                                                                                                   
    "lun": 1,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-121516",                                                                                                                    
    "vhd": {                                                                                                                                                           
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-121516.vhd"                                                       
      }                                                                                                                                                            
      }                                                                                                                                                             
    ]

### <a name="discover-filesystem-uuids-by-using-blkid"></a>Bestandssysteem UUID's detecteren met behulp van blkid

Toepassingen en scripts en de uitvoer van lezen `blkid`, of vergelijkbare gegevensbronnen, om symbolische koppelingen in het pad /dev samen te stellen. De uitvoer ziet u de UUID's van alle schijven die zijn gekoppeld aan de virtuele machine en hun bijbehorende apparaatbestand:

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

De Azure Linux agent Udev-regels maken een set symbolische koppelingen onder het pad /dev/disk/azure:

    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1

De koppelingen toepassingen gebruiken om het apparaat van de schijf opstarten en de bronschijf (tijdelijke) te identificeren. In Azure ziet de toepassingen in de paden /dev/disk/azure/root-part1 of /dev/disk/azure-resource-part1 voor het detecteren van deze partities.

Alle aanvullende partities uit de `blkid` lijst zich bevinden op een gegevensschijf. Toepassingen de UUID voor deze partities beheren en gebruiken van een pad voor het detecteren van de naam van het apparaat tijdens runtime:

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-the-latest-azure-storage-rules"></a>De meest recente Azure Storage-regels ophalen

Als u de meest recente Azure Storage-regels, voer de volgende opdrachten:

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block

## <a name="see-also"></a>Zie ook

Raadpleeg voor meer informatie de volgende artikelen:

- [Ubuntu: UUID met behulp](https://help.ubuntu.com/community/UsingUUID)
- [Red Hat: Permanente naamgeving](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)
- [Linux: Wat UUID's voor u kunnen doen](https://www.linux.com/news/what-uuids-can-do-you)
- [Udev: Inleiding tot beheer van apparaten in een moderne Linux-systeem](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

