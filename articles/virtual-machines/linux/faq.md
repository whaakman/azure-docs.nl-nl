---
title: Veelgestelde vragen over virtuele Linux-machines in Azure | Microsoft Docs
description: Hier vindt u antwoorden op enkele van de veelgestelde vragen over Linux virtuele machines is gemaakt met het Resource Manager-model.
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
ms.openlocfilehash: 8a4d93ff12affac56c12c0eab85168c609400ee2
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
ms.locfileid: "30242435"
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Veelgestelde vragen over virtuele Linux-Machines
In dit artikel komen enkele veelgestelde vragen over virtuele Linux-machines in Azure met behulp van het Resource Manager-implementatiemodel zijn gemaakt. Zie voor de Windows-versie van dit onderwerp [Veelgestelde vragen over Windows virtuele Machines](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Wat kan ik uitvoeren op een VM van Azure?
Alle abonnees kunnen serversoftware uitvoeren op een virtuele machine van Azure. Zie voor meer informatie [Linux op Azure-Endorsed distributies](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hoeveel opslagruimte kan ik gebruiken met een virtuele machine?
Elke gegevensschijf mag maximaal 4 TB (4095 GB). Het aantal gegevensschijven dat u kunt gebruiken, is afhankelijk van de grootte van de virtuele machine. Zie [Grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie.

Azure-schijven die worden beheerd, zijn de aangeraden schijfruimte opslag aanbiedingen voor gebruik met Azure Virtual Machines voor permanente opslag van gegevens. U kunt voor elke virtuele machine Managed Disks gebruiken. Azure Managed Disks biedt twee typen duurzame opslag: Premium en Standard Managed Disks. Zie voor informatie over prijzen, [beheerd schijven prijzen](https://azure.microsoft.com/pricing/details/managed-disks).

Azure storage-accounts kunnen ook opslag bieden voor de besturingssysteemschijf en alle gegevensschijven. Elke schijf is een VHD-bestand dat wordt opgeslagen als een pagina-blob. Zie [deze pagina](https://azure.microsoft.com/pricing/details/storage/) voor prijsinformatie.

## <a name="how-can-i-access-my-virtual-machine"></a>Hoe kan ik toegang tot mijn virtuele machine?
Stel een externe verbinding aan te melden op de virtuele machine met behulp van Secure Shell (SSH). Zie de instructies voor het verbinding maken met [vanuit Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [van Linux- en Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). SSH biedt standaard ondersteuning voor maximaal tien gelijktijdige verbindingen. U kunt dit aantal verhogen door het configuratiebestand te bewerken.

Als u problemen ondervindt, uitchecken [oplossen Secure Shell (SSH) verbindingen](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Kan ik de tijdelijke schijf (/ dev/sdb1) gebruiken voor het opslaan van gegevens?
Gebruik niet de tijdelijke schijf (/ dev/sdb1) om het opslaan van gegevens. Het is alleen er voor de tijdelijke opslag. U risico verlies van gegevens die kunnen niet worden hersteld.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kan ik kopiëren of kloon van een bestaande virtuele machine in Azure?
Ja. Zie voor instructies [een kopie van een virtuele Linux-machine maken in het Resource Manager-implementatiemodel](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Waarom krijg ik niet zien Canada centraal en Canada Oost regio's via Azure Resource Manager?
De twee nieuwe gebieden van Canada centraal en Canada Oost worden niet automatisch geregistreerd voor het maken van de virtuele machine voor bestaande Azure-abonnementen. Deze registratie gebeurt automatisch wanneer een virtuele machine wordt geïmplementeerd via de Azure portal naar een andere regio met Azure Resource Manager. Nadat een virtuele machine op een andere Azure-regio is geïmplementeerd, moeten de nieuwe regio's beschikbaar zijn voor latere virtuele machines zijn.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kan ik een NIC toevoegen aan mijn VM nadat deze gemaakt?
Ja, dit is nu mogelijk. De virtuele machine eerst moet worden gestopt toewijzing ongedaan is gemaakt. Vervolgens kunt u toevoegen of verwijderen van een NIC (tenzij deze de laatste NIC op de virtuele machine is). 

## <a name="are-there-any-computer-name-requirements"></a>Zijn er naam computervereisten?
Ja. De computernaam mag maximaal 64 tekens lang zijn. Zie [Naming conventions regels en beperkingen](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie over de naamgeving van uw resources.

## <a name="are-there-any-resource-group-name-requirements"></a>Zijn er een resource vereisten voor de naam groep?
Ja. Naam van de resourcegroep mag maximaal 90 tekens lang zijn. Zie [Naming conventions regels en beperkingen](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie over resourcegroepen.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Wat zijn de vereisten van de gebruikersnaam bij het maken van een virtuele machine?

Gebruikersnamen moet 1 en 32 tekens lang zijn.

De volgende gebruikersnamen zijn niet toegestaan:

<table>
    <tr>
        <td style="text-align:center">Beheerder </td><td style="text-align:center"> beheerder </td><td style="text-align:center"> Gebruiker </td><td style="text-align:center"> Gebruiker1</td>
    </tr>
    <tr>
        <td style="text-align:center">test </td><td style="text-align:center"> Gebruiker2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> User3</td>
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
        <td style="text-align:center">test2 </td><td style="text-align:center"> test3 </td><td style="text-align:center"> User4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Wat zijn de wachtwoordvereisten bij het maken van een virtuele machine?
Wachtwoorden moeten 6-72 tekens lang zijn en 3 uit de volgende 4 complexiteitsvereisten voldoen aan:

* Lagere tekens bevatten
* Bovenste tekens bevatten
* Een cijfer zijn
* Hebt u een speciaal teken (Regex overeenkomen met [\W_])

De volgende wachtwoorden zijn niet toegestaan:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Wachtwoord!</td>
        <td style="text-align:center">Wachtwoord1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
