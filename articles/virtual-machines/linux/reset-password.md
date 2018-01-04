---
title: Het opnieuw instellen van lokale Linux-wachtwoord op Azure Virtual machines | Microsoft Docs
description: De stappen om het lokale Linux-wachtwoord op Azure virtuele machine opnieuw in te voeren
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: delhan
ms.openlocfilehash: b9182ec2a974de06c2bd45928b9964f253653bf6
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Het wachtwoord voor de lokale Linux op Azure Virtual machines opnieuw instellen

Dit artikel bevat verschillende methoden beschreven voor het opnieuw instellen van wachtwoorden voor lokale Linux-virtuele Machine (VM). Als het gebruikersaccount is verlopen of u alleen wilt maken van een nieuw account, kunt u de volgende methoden gebruiken voor het maken van een nieuwe lokale beheerdersaccount en opnieuw toegang te krijgen tot de virtuele machine.

## <a name="symptoms"></a>Symptomen

U niet kunt aanmelden bij de virtuele machine en u ontvangt een bericht weergegeven dat aangeeft dat het wachtwoord die u hebt gebruikt, onjuist is. U kunt VMAgent bovendien niet gebruiken om uw wachtwoord in de Azure Portal te stellen. 

## <a name="manual-password-reset-procedure"></a>Handmatige procedure voor wachtwoord opnieuw instellen

1.  Verwijderen van de virtuele machine en de gekoppelde schijven behouden.

2.  De OS-station als een gegevensschijf koppelen aan een andere tijdelijke virtuele machine op dezelfde locatie bevinden.

3.  Voer de volgende SSH-opdracht op de tijdelijke virtuele machine als supergebruiker.


    ~~~~
    sudo su
    ~~~~

4.  Voer **fdisk -l** of kijk eens naar het systeemlogboek in Logboeken op de zojuist gekoppelde schijf vinden. Zoek de stationsnaam te koppelen. Klik op de tijdelijke virtuele machine, kijk in het relevante logbestand.

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    Hier volgt een voorbeeld van uitvoer van de opdracht grep:

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  Maken van een koppelpunt aangeroepen **tempmount**.

    ~~~~
    mkdir /tempmount
    ~~~~

6.  Koppel de OS-schijf op het koppelpunt wordt gehost. U moet meestal koppelpunt sdc1 of sdc2. Dit hangt af van de hosting partitie in de map/etc van de machineschijf verbroken.

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  Een back-up uitvoeren voordat u wijzigingen aanbrengt:

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  Opnieuw instellen van het wachtwoord van de gebruiker die u nodig hebt:

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  De gewijzigde bestanden verplaatsen naar de juiste locatie op de machine verbroken schijf.

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ~~~~

10. Ga terug naar de hoofdmap en ontkoppel de schijf.

    ~~~~
    cd /
    umount /tempmount
    ~~~~

11. Ontkoppel de schijf van de beheerportal.

12. Maak de virtuele machine opnieuw.

## <a name="next-steps"></a>Volgende stappen

* [Virtuele machine van Azure door de OS-schijf koppelen aan een andere virtuele machine in Azure oplossen](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: Het verwijderen en opnieuw implementeren van een virtuele machine van VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
