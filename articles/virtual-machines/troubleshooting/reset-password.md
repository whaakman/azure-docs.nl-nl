---
title: Het opnieuw instellen van wachtwoord van de lokale Linux op Azure Virtual machines | Microsoft Docs
description: De stappen om het wachtwoord van de lokale Linux op Azure virtuele machine opnieuw in te voeren
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: 37b02b657278782ea82f3eb129335793a0b5c98d
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413637"
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Het opnieuw instellen van wachtwoord van de lokale Linux op Azure Virtual machines

Dit artikel bevat verschillende methoden voor het opnieuw instellen van wachtwoorden voor lokale Linux virtuele Machine (VM). Als het gebruikersaccount dat is verlopen of als u alleen wilt maken van een nieuw account, kunt u de volgende methoden om te maken van een nieuwe lokale beheerdersaccount en opnieuw toegang te krijgen tot de virtuele machine.

## <a name="symptoms"></a>Symptomen

U kunt zich niet aanmelden bij de virtuele machine en u ontvangt een bericht weergegeven dat aangeeft dat het wachtwoord die u hebt gebruikt, onjuist is. U kunt de VMAgent bovendien niet gebruiken voor uw wachtwoord in de Azure portal opnieuw instellen.

## <a name="manual-password-reset-procedure"></a>Handmatige procedure voor wachtwoordherstel

1.  De virtuele machine verwijderen en behouden van de gekoppelde schijven.

2.  De Besturingssysteemschijf als gegevensschijf koppelen aan een andere tijdelijke virtuele machine op dezelfde locatie.

3.  Voer de volgende SSH-opdracht op de tijdelijke virtuele machine om te worden van een supergebruiker.

    ```bash
    sudo su
    ```

4.  Voer **fdisk -l** of kijken naar het systeemlogboek in logboeken naar de zojuist gekoppelde schijf vinden. Zoek de stationsnaam van het te koppelen. Klik op de tijdelijke virtuele machine, zoeken in het relevante logbestand.

    ```bash
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ```

    Hier volgt een van de voorbeelduitvoer van de opdracht grep:

    ```bash
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ```

5.  Maken van een koppelpunt met de naam **tempmount**.

    ```bash
    mkdir /tempmount
    ```

6.  Koppel de besturingssysteemschijf op het koppelpunt. Moet u doorgaans koppelen *sdc1* of *sdc2*. Dit is afhankelijk van de hosting-partitie in */etc* map van de machineschijf verbroken.

    ```bash
    mount /dev/sdc1 /tempmount
    ```

7.  Maak kopieën van de belangrijkste referentie bestanden voordat u wijzigingen aanbrengt:

    ```bash
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ```

8.  Opnieuw instellen van wachtwoord van de gebruiker die u nodig hebt:

    ```bash
    passwd <<USER>> 
    ```

9.  De gewijzigde bestanden verplaatsen naar de juiste locatie op van de machine van de beschadigde schijf.

    ```bash
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ```

10. Ga terug naar de hoofdmap en ontkoppel de schijf.

    ```bash
    cd /
    umount /tempmount
    ```

11. De schijf loskoppelen van de management portal.

12. Maak de virtuele machine opnieuw.

## <a name="next-steps"></a>Volgende stappen

* [Virtuele Azure-machine oplossen door besturingssysteemschijf te koppelen aan een andere Azure-VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: Het verwijderen en opnieuw implementeren van een VM op basis van VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
