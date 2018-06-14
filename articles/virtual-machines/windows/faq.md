---
title: Veelgestelde vragen over Windows virtuele machines in Azure | Microsoft Docs
description: Hier vindt u antwoorden op enkele van de veelgestelde vragen over Windows virtuele machines is gemaakt met het Resource Manager-model.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-management
ms.assetid: 757da816-a050-4889-a010-6f75d7978eb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: cynthn
ms.openlocfilehash: c60b7df4d766ddf321ce2c807994f3b83046f1ef
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
ms.locfileid: "34058060"
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Veelgestelde vragen over virtuele Windows-Machines
In dit artikel komen enkele veelgestelde vragen over Windows virtuele machines die in Azure met behulp van het Resource Manager-implementatiemodel zijn gemaakt. Zie voor de Linux-versie van dit onderwerp [Veelgestelde vragen over virtuele Linux-Machines](../linux/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Wat kan ik uitvoeren op een VM van Azure?
Alle abonnees kunnen serversoftware uitvoeren op een virtuele machine van Azure. Zie voor informatie over het ondersteuningsbeleid voor Microsoft-serversoftware uitgevoerd in Azure, [Microsoft server softwareondersteuning voor Azure Virtual Machines](https://support.microsoft.com/kb/2721672)

Bepaalde versies van Windows 7, Windows 8.1 en Windows 10 zijn beschikbaar voor MSDN Azure voordeel abonnees en abonnees van een MSDN ontwikkelen en testen betalen naar gebruik voor ontwikkel- en taken. Zie het Engelstalige blogbericht [Windows Client images for MSDN subscribers](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/) voor meer informatie, zoals instructies en beperkingen. 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hoeveel opslagruimte kan ik gebruiken met een virtuele machine?
Elke gegevensschijf mag maximaal 4 TB (4095 GB). Het aantal gegevensschijven dat u kunt gebruiken, is afhankelijk van de grootte van de virtuele machine. Zie [Grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor meer informatie.

Azure-schijven die worden beheerd, zijn de aangeraden schijfruimte opslag aanbiedingen voor gebruik met Azure Virtual Machines voor permanente opslag van gegevens. U kunt voor elke virtuele machine Managed Disks gebruiken. Azure Managed Disks biedt twee typen duurzame opslag: Premium en Standard Managed Disks. Zie voor informatie over prijzen, [beheerd schijven prijzen](https://azure.microsoft.com/pricing/details/managed-disks).

Azure storage-accounts kunnen ook opslag bieden voor de besturingssysteemschijf en alle gegevensschijven. Elke schijf is een VHD-bestand dat wordt opgeslagen als een pagina-blob. Zie [deze pagina](https://azure.microsoft.com/pricing/details/storage/) voor prijsinformatie.

## <a name="how-can-i-access-my-virtual-machine"></a>Hoe kan ik toegang tot mijn virtuele machine?
Stel een externe verbinding verbinding met extern bureaublad (RDP) voor een virtuele machine van Windows. Zie voor instructies [verbinding maken met en meld u aan een virtuele machine van Azure waarop Windows wordt uitgevoerd bij](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Maximaal twee gelijktijdige verbindingen worden ondersteund, tenzij de server is geconfigureerd als host voor extern bureaublad-Services-sessie.  

Als u problemen met extern bureaublad ondervindt, Zie [problemen met extern bureaublad-verbindingen naar een op basis van Windows Azure virtuele Machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Als u bekend bent met Hyper-V, verwacht u misschien een hulpprogramma zoals VMConnect. Azure biedt echter geen vergelijkbaar hulpprogramma omdat consoletoegang tot een virtuele machine niet wordt ondersteund.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Kan ik de tijdelijke schijf (het station D: standaard) gebruiken voor het opslaan van gegevens?
Gebruik geen tijdelijke schijf voor het opslaan van gegevens. Het is alleen tijdelijke opslag, zodat u zou risico verlies van gegevens die kunnen niet worden hersteld. Verlies van gegevens kan optreden wanneer de virtuele machine wordt verplaatst naar een andere host. Hiervoor zijn verschillende redenen te bedenken, zoals het aanpassen van de grootte van een virtuele machine, het bijwerken van de host of een hardwarefout op de host.

Als u een toepassing die moet worden gebruikt, de stationsletter D: hebt, kunt u stationsletters kunt toewijzen, zodat de tijdelijke schijf gebruikmaakt van iets anders dan D:. Zie [Use the D: drive as a data drive on a Windows VM](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (Het station D: als gegevensstation gebruiken op een Windows-VM) voor instructies.


## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Hoe wijzig ik de stationsletter van de tijdelijke schijf?
U kunt de stationsletter wijzigen door het wisselbestand verplaatsen en stationsletters opnieuw toewijzen, maar u moet ervoor zorgen dat u de stappen in een bepaalde volgorde doen. Zie [Use the D: drive as a data drive on a Windows VM](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (Het station D: als gegevensstation gebruiken op een Windows-VM) voor instructies.

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Kan ik een bestaande virtuele machine toevoegen aan een beschikbaarheidsset
Nee. Als u wilt dat uw virtuele machine deel uitmaken van een beschikbaarheidsset, moet u de virtuele machine binnen de set maken. Er is momenteel niet een manier om een virtuele machine toevoegen aan een beschikbaarheidsset nadat deze is gemaakt.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Kan ik een virtuele machine uploaden naar Azure
Ja. Zie voor instructies [migreren lokale virtuele machines naar Azure](on-prem-to-azure.md).

## <a name="can-i-resize-the-os-disk"></a>Kan ik het formaat van de besturingssysteemschijf?
Ja. Zie voor instructies [het uitbreiden van de OS-schijf van een virtuele Machine in een Azure-resourcegroep](expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kan ik kopiëren of kloon van een bestaande virtuele machine in Azure?
Ja. Beheerde installatiekopieën gebruikt, kunt u een installatiekopie van een virtuele machine maken en gebruik vervolgens de installatiekopie van het bouwen van meerdere nieuwe virtuele machines. Zie voor instructies [een aangepaste installatiekopie van een VM maken](tutorial-custom-images.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Waarom krijg ik niet zien Canada centraal en Canada Oost regio's via Azure Resource Manager?

De twee nieuwe gebieden van Canada centraal en Canada Oost worden niet automatisch geregistreerd voor het maken van de virtuele machine voor bestaande Azure-abonnementen. Deze registratie gebeurt automatisch wanneer een virtuele machine wordt geïmplementeerd via de Azure portal naar een andere regio met Azure Resource Manager. Nadat een virtuele machine op een andere Azure-regio is geïmplementeerd, moeten de nieuwe regio's beschikbaar zijn voor latere virtuele machines zijn.

## <a name="does-azure-support-linux-vms"></a>Azure biedt ondersteuning voor virtuele Linux-machines?
Ja. Zie snel maken van een Linux-VM om uit te proberen [maken van een Linux-VM op Azure met behulp van de Portal](../linux/quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kan ik een NIC toevoegen aan mijn VM nadat deze gemaakt?
Ja, dit is nu mogelijk. De virtuele machine eerst moet worden gestopt toewijzing ongedaan is gemaakt. Vervolgens kunt u toevoegen of verwijderen van een NIC (tenzij deze de laatste NIC op de virtuele machine is). 

## <a name="are-there-any-computer-name-requirements"></a>Zijn er naam computervereisten?
Ja. De computernaam mag maximaal 15 tekens lang zijn. Zie [Naming conventions regels en beperkingen](/azure/architecture/best-practices/naming-conventions#compute) voor meer informatie over de naamgeving van uw resources.

## <a name="are-there-any-resource-group-name-requirements"></a>Zijn er een resource vereisten voor de naam groep?
Ja. Naam van de resourcegroep mag maximaal 90 tekens lang zijn. Zie [Naming conventions regels en beperkingen](/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions) voor meer informatie over resourcegroepen.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Wat zijn de vereisten van de gebruikersnaam bij het maken van een virtuele machine?

Gebruikersnamen mag maximaal 20 tekens lang zijn en mag niet eindigen op een punt ('. '). 


De volgende gebruikersnamen zijn niet toegestaan:
<table>
    <tr>
        <td style="text-align:center">1</td><td style="text-align:center">123</td><td style="text-align:center">a</td><td style="text-align:center">actuser</td>
    </tr>
    <tr>
        <td style="text-align:center">adm</td><td style="text-align:center">Beheerder</td><td style="text-align:center">admin1</td><td style="text-align:center">admin2</td>
    </tr>   <tr>
        <td style="text-align:center">Beheerder</td><td style="text-align:center">ASPNET</td><td style="text-align:center">Back-up</td><td style="text-align:center">console</td>
    </tr>
    <tr>
        <td style="text-align:center">David </td><td style="text-align:center">Gast</td><td style="text-align:center">John</td><td style="text-align:center">eigenaar</td>
    </tr>
    <tr>
        <td style="text-align:center">hoofdmap</td><td style="text-align:center">server</td><td style="text-align:center">sql</td><td style="text-align:center">ondersteuning</td>
    </tr>
    <tr>
        <td style="text-align:center">support_388945a0</td><td style="text-align:center">sys</td><td style="text-align:center">test</td><td style="text-align:center">test1</td>
    </tr>
    <tr>
        <td style="text-align:center">test2</td><td style="text-align:center">test3</td><td style="text-align:center">Gebruiker</td><td style="text-align:center">Gebruiker1</td>
    </tr>
    <tr>
        <td style="text-align:center">Gebruiker2</td><td style="text-align:center">User3</td><td style="text-align:center">User4</td><td style="text-align:center">user5</td>
    </tr>
</table>

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Wat zijn de wachtwoordvereisten bij het maken van een virtuele machine?
Wachtwoorden moeten 12 123 tekens lang zijn en 3 uit de volgende 4 complexiteitsvereisten voldoen:

* Lagere tekens bevatten
* Bovenste tekens bevatten
* Een cijfer zijn
* Hebt u een speciaal teken (Regex overeenkomen met [\W_])

De volgende wachtwoorden zijn niet toegestaan:

<table>
    <tr>
        <td>abc@123</td>
        <td>ILOVEYOU!</td>
        <td>P@$$w0rd</td>
        <td>P@ssw0rd</td>
        <td>P@ssword123</td>
    </tr>
    <tr>
        <td>Pa$ $word</td>
        <td>pass@word1</td>
        <td>Wachtwoord!</td>
        <td>Wachtwoord1</td>
        <td>Password22</td>
    </tr>
</table>
