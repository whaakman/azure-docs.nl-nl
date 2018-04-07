---
title: Windows AWS-VM's verplaatsen naar Azure | Microsoft Docs
description: Verplaatsen van een Amazon Web Services (AWS) EC2 Windows-exemplaar op virtuele Machines van Azure met Azure PowerShell.
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
ms.date: 06/01/2017
ms.author: cynthn
ms.openlocfilehash: 4b7e794cb08647dde6fe59b6d4b06a9cbfab06e1
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-to-azure-using-powershell"></a>Een Windows VM Amazon Web Services (AWS) verplaatsen naar Azure met behulp van PowerShell

Als u virtuele machines voor het hosten van uw workloads in Azure, kunt u een bestaand exemplaar van de virtuele machine met Amazon Web Services (AWS) EC2 Windows exporteren vervolgens de virtuele harde schijf (VHD) uploaden naar Azure. Als de VHD is geüpload, kunt u een nieuwe virtuele machine maken in Azure van de VHD. 

Dit onderwerp bevat een enkele virtuele machine van de AWS verplaatst naar Azure. Als u wilt verplaatsen van virtuele machines van AWS naar Azure op schaal, Zie [virtuele machines in Amazon Web Services (AWS) migreren naar Azure met Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).

## <a name="prepare-the-vm"></a>De virtuele machine voorbereiden 
 
U kunt algemene en gespecialiseerde VHD's uploaden naar Azure. Elk type vereist dat u de virtuele machine voorbereiden voordat u exporteert van AWS. 

- **VHD gegeneraliseerd** -een gegeneraliseerde VHD heeft al uw persoonlijke accountgegevens verwijderd met behulp van Sysprep. Als u van plan bent om de VHD te maken van nieuwe virtuele machines van als afbeelding, moet u het volgende doen: 
 
    * [Voorbereiden van een Windows VM](prepare-for-upload-vhd-image.md).  
    * De virtuele machine met behulp van Sysprep generalize.  

 
- **VHD gespecialiseerde** -een gespecialiseerde VHD onderhoudt de gebruikersaccounts, toepassingen en andere statusgegevens van uw oorspronkelijke VM. Als u wilt gebruiken van de VHD-is een nieuwe virtuele machine maken, controleert u de volgende stappen zijn voltooid.  
    * [Voorbereiden van een Windows-VHD te uploaden naar Azure](prepare-for-upload-vhd-image.md). **Geen** generalize van de virtuele machine met behulp van Sysprep. 
    * Verwijder eventuele Gast virtualisatie-hulpprogramma's en de agents die zijn geïnstalleerd op de virtuele machine (dat wil zeggen VMware tools). 
    * Zorg ervoor dat de virtuele machine is geconfigureerd om op te halen van de IP-adres en DNS-instellingen via DHCP. Dit zorgt ervoor dat de server een IP-adres binnen het VNet verkrijgt wanneer deze opgestart wordt.  


## <a name="export-and-download-the-vhd"></a>Exporteren en de VHD te downloaden 

Het exemplaar EC2 exporteren naar een VHD in een Amazon S3-bucket. Volg de stappen in de Amazon-documentatie-onderwerp [exporteren van een exemplaar als een virtuele machine met behulp van virtuele machine importeren/exporteren](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) en voer de [-exemplaar-export-taak maken](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) opdracht het exemplaar EC2 exporteren naar een VHD-bestand. 

Het geëxporteerde VHD-bestand wordt opgeslagen in de Amazon S3-bucket die u opgeeft. De syntaxis van de basis voor het exporteren van de VHD is hieronder, net Vervang de tijdelijke aanduiding voor tekst in <brackets> met uw gegevens.

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

Zodra de VHD is geëxporteerd, volg de instructies in [hoe kan ik een Object downloaden van een S3-Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) downloaden van het VHD-bestand van de S3-bucket. 

> [!IMPORTANT]
> Gegevens over kosten AWS overdragen kosten voor het downloaden van de VHD. Zie [Amazon S3 prijzen](https://aws.amazon.com/s3/pricing/) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

U kunt nu de VHD te uploaden naar Azure en een nieuwe virtuele machine maken. 

- Als u Sysprep uitgevoerd op de bron naar **generalize** deze voordat u exporteert, Zie [een gegeneraliseerde VHD uploaden en deze gebruiken voor het maken van een nieuwe virtuele machines in Azure](upload-generalized-managed.md)
- Als u niet Sysprep hebt uitgevoerd voordat u exporteert, de VHD wordt beschouwd als **gespecialiseerde**, Zie [een gespecialiseerde VHD uploaden naar Azure en een nieuwe virtuele machine maken](create-vm-specialized.md)

 
