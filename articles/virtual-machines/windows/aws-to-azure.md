---
title: Een AWS Windows-VM's verplaatsen naar Azure | Microsoft Docs
description: Een Amazon Web Services (AWS) EC2 Windows-exemplaar naar een virtuele machine van Azure verplaatsen.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: d6a4c5b2b6d9818dffdb1c1fee8c4c0df7cad77c
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58539799"
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-to-an-azure-virtual-machine"></a>Een Windows-VM vanuit Amazon Web Services (AWS) te verplaatsen naar een virtuele machine van Azure

Als u virtuele machines voor het hosten van uw workloads in Azure evalueert, kunt u het exporteren van een bestaande Amazon Web Services (AWS) EC2 Windows VM-instantie vervolgens de virtuele harde schijf (VHD) uploaden naar Azure. Als de VHD is geüpload, kunt u een nieuwe virtuele machine maken in Azure van de VHD. 

In dit artikel bevat informatie over het verplaatsen van een enkele virtuele machine van AWS naar Azure. Als u verplaatsen van virtuele machines van AWS naar Azure op schaal wilt, Zie [migreren van virtuele machines in Amazon Web Services (AWS) naar Azure met Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).

## <a name="prepare-the-vm"></a>De virtuele machine voorbereiden 
 
U kunt zowel gegeneraliseerde en gespecialiseerde VHD's uploaden naar Azure. Elk type is vereist dat u de virtuele machine voorbereiden voordat u exporteert van AWS. 

- **Gegeneraliseerde VHD** -een gegeneraliseerde VHD heeft al uw persoonlijke accountinformatie verwijderd met behulp van Sysprep. Als u van plan bent de VHD als een installatiekopie gebruiken om te maken van nieuwe virtuele machines uit, moet u het: 
 
    * [Voorbereiden van een Windows VM](prepare-for-upload-vhd-image.md).  
    * Generaliseren met Sysprep de virtuele machine.  

 
- **Gespecialiseerde VHD** -een gespecialiseerde VHD onderhoudt de gebruikersaccounts, toepassingen en andere statusgegevens van de oorspronkelijke virtuele machine. Als u van plan bent de VHD als gebruiken-is een nieuwe virtuele machine maakt, zorg ervoor dat de volgende stappen zijn voltooid.  
    * [Voorbereiden van een Windows-VHD te uploaden naar Azure](prepare-for-upload-vhd-image.md). **Geen** generaliseren van de virtuele machine met behulp van Sysprep. 
    * Verwijder alle Gast virtualisatie-hulpprogramma's en de agents die zijn geïnstalleerd op de virtuele machine (dat wil zeggen VMware-hulpprogramma's genoemd). 
    * Zorg ervoor dat de virtuele machine is geconfigureerd voor het ophalen van de IP-adres en DNS-instellingen via DHCP. Dit zorgt ervoor dat de server een IP-adres binnen het VNet wordt wanneer deze opgestart wordt.  


## <a name="export-and-download-the-vhd"></a>Exporteren en downloaden van de VHD 

Exporteer de EC2-instantie naar een VHD in een Amazon S3-bucket. Volg de stappen in de Amazon-documentatieartikel [exporteren van een exemplaar als een virtuele machine met behulp van virtuele machine importeren/exporteren](https://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) en voer de [-exemplaar-export-taak maken](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) opdracht voor het exporteren van de EC2-instantie naar een VHD-bestand. 

De geëxporteerde VHD-bestand wordt opgeslagen in de Amazon S3-bucket die u opgeeft. De syntaxis van de basis voor het exporteren van de VHD is hieronder, vervang de tijdelijke tekstaanduiding in \<vierkante haken > met uw gegevens.

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

Nadat de VHD is geëxporteerd, volg de instructies in [hoe kan ik een Object downloaden vanaf een S3-Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) te downloaden van het VHD-bestand van de S3-bucket. 

> [!IMPORTANT]
> AWS het kosten van gegevensoverdracht kosten voor het downloaden van de VHD. Zie [prijzen van Amazon S3](https://aws.amazon.com/s3/pricing/) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

U kunt nu de VHD uploaden naar Azure en maak een nieuwe virtuele machine. 

- Als u Sysprep uitgevoerd op de bron aan **Generaliseren** deze voordat u exporteert, Zie [een gegeneraliseerde VHD uploaden en maken van een nieuwe virtuele machines in Azure](upload-generalized-managed.md)
- Als u niet Sysprep hebt uitgevoerd voordat u exporteert, de VHD wordt beschouwd als **gespecialiseerde**, Zie [een gespecialiseerde VHD uploaden naar Azure en een nieuwe virtuele machine maken](create-vm-specialized.md)

 
