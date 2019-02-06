---
title: Veelgestelde vragen over virtuele Linux-machines in Azure | Microsoft Docs
description: Vindt u antwoorden op enkele veelgestelde vragen over virtuele Linux-machines die zijn gemaakt met het Resource Manager-model.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-management
ms.assetid: 3648e09c-1115-4818-93c6-688d7a54a353
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: cynthn
ms.openlocfilehash: 8d421adfae335a976485ed463a69484a74be5b44
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55753926"
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Veelgestelde vragen over virtuele Linux-Machines
In dit artikel komen enkele veelgestelde vragen over Linux virtuele machines die in Azure met behulp van de Resource Manager-implementatiemodel zijn gemaakt. Zie voor de Windows-versie van dit onderwerp, [Veelgestelde vragen over Windows Virtual Machines](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Wat kan ik uitvoeren op een VM van Azure?
Alle abonnees kunnen serversoftware uitvoeren op een virtuele machine van Azure. Zie voor meer informatie, [Linux op door Azure-Endorsed distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hoeveel opslagruimte kan ik gebruiken met een virtuele machine?
Elke gegevensschijf kan tot 4 TB (4095 GB) zijn. Het aantal gegevensschijven dat u kunt gebruiken, is afhankelijk van de grootte van de virtuele machine. Zie [Grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie.

Azure Managed Disks is de aanbevolen product voor schijfopslag gebruiken met Azure Virtual Machines voor permanente opslag van gegevens. U kunt voor elke virtuele machine Managed Disks gebruiken. Beheerde schijven bieden twee typen duurzame opslag: Premium en Standard Managed Disks. Zie voor informatie over de prijzen [Managed Disks-prijzen](https://azure.microsoft.com/pricing/details/managed-disks).

Azure storage-accounts kunnen ook opslag bieden voor de schijf van het besturingssysteem en eventuele gegevensschijven. Elke schijf is een VHD-bestand dat wordt opgeslagen als een pagina-blob. Zie [deze pagina](https://azure.microsoft.com/pricing/details/storage/) voor prijsinformatie.

## <a name="how-can-i-access-my-virtual-machine"></a>Hoe krijg ik toegang tot mijn virtuele machine?
Een externe verbinding voor aanmelding bij de virtuele machine, met behulp van Secure Shell (SSH) tot stand brengen. Zie de instructies voor het verbinding maken [van Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [van Linux- en Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). SSH biedt standaard ondersteuning voor maximaal tien gelijktijdige verbindingen. U kunt dit aantal verhogen door het configuratiebestand te bewerken.

Als u problemen ondervindt, bekijk dan [oplossen Secure Shell (SSH)-verbindingen](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Kan ik de tijdelijke schijf (/ dev/sdb1) gebruiken voor het opslaan van gegevens?
Gebruik niet de tijdelijke schijf (/ dev/sdb1) voor het opslaan van gegevens. Het is alleen er voor de tijdelijke opslag. U het risico loopt gegevens die niet worden hersteld.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kan ik kopiëren of klonen van een bestaande VM in Azure?
Ja. Zie voor instructies [over het maken van een kopie van een virtuele Linux-machine in het Resource Manager-implementatiemodel](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Waarom zie ik Canada-centraal en Canada-Oost regio's via Azure Resource Manager?
De twee nieuwe regio's Canada-centraal en Canada-Oost worden niet automatisch geregistreerd voor virtuele machines worden gemaakt voor bestaande Azure-abonnementen. Deze registratie gebeurt automatisch wanneer een virtuele machine wordt geïmplementeerd via Azure portal naar elke andere regio met Azure Resource Manager. Nadat een virtuele machine op een andere Azure-regio is geïmplementeerd, moeten de nieuwe regio's beschikbaar zijn voor latere virtuele machines zijn.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kan ik een NIC toevoegen aan mijn virtuele machine nadat deze gemaakt?
Ja, dit is nu mogelijk. De virtuele machine eerst moet worden gestopt toewijzing ongedaan gemaakt. Vervolgens kunt u toevoegen of verwijderen van een NIC (tenzij dit de laatste NIC op de virtuele machine is). 

## <a name="are-there-any-computer-name-requirements"></a>Zijn er vereisten voor elke computer de naam?
Ja. Naam van de computer kan maximaal 64 tekens lang zijn. Zie [Naming conventions regels en beperkingen](/azure/architecture/best-practices/naming-conventions) voor meer informatie over de naamgeving van uw resources.

## <a name="are-there-any-resource-group-name-requirements"></a>Zijn er een resource vereisten voor de naam groep?
Ja. Naam van de resourcegroep mag maximaal 90 tekens lang zijn. Zie [Naming conventions regels en beperkingen](/azure/architecture/best-practices/naming-conventions) voor meer informatie over resourcegroepen.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Wat zijn de vereisten van de gebruikersnaam bij het maken van een virtuele machine?

Gebruikersnamen moet 1 en 32 tekens lang zijn.

De volgende gebruikersnamen zijn niet toegestaan:

<table>
    <tr>
        <td style="text-align:center">de beheerder </td><td style="text-align:center"> beheerder </td><td style="text-align:center"> user </td><td style="text-align:center"> Gebruiker1</td>
    </tr>
    <tr>
        <td style="text-align:center">test </td><td style="text-align:center"> Gebruiker2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> gebruiker3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> a</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> aspnet</td>
    </tr>
    <tr>
        <td style="text-align:center">Back-up </td><td style="text-align:center"> console </td><td style="text-align:center"> David </td><td style="text-align:center"> Gast</td>
    </tr>
    <tr>
        <td style="text-align:center">John </td><td style="text-align:center"> eigenaar </td><td style="text-align:center"> hoofdmap </td><td style="text-align:center"> server</td>
    </tr>
    <tr>
        <td style="text-align:center">sql </td><td style="text-align:center"> ondersteuning </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
    </tr>
    <tr>
        <td style="text-align:center">test2 </td><td style="text-align:center"> test3 </td><td style="text-align:center"> Gebruiker4 </td><td style="text-align:center"> gebruiker5</td>
    </tr>
</table>


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Wat zijn de wachtwoordvereisten bij het maken van een virtuele machine?
Wachtwoorden moeten 6-72 tekens lang zijn en aan 3 van de volgende 4 complexiteitsvereisten voldoen:

* Lagere tekens bevatten
* Bovenste tekens bevatten
* Een cijfer zijn
* Een speciaal teken bevatten (reguliere expressie komt overeen met [\W_])

De volgende wachtwoorden zijn niet toegestaan:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$$word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Wachtwoord!</td>
        <td style="text-align:center">Wachtwoord1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
