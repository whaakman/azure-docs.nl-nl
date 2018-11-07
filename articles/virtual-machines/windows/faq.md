---
title: Veelgestelde vragen over Windows-VM's in Azure | Microsoft Docs
description: Vindt u antwoorden op enkele veelgestelde vragen over Windows-machines die zijn gemaakt met het Resource Manager-model.
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
ms.openlocfilehash: 65d4326763ef9754159e94c9426f3aee69f80ffd
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51253360"
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Veelgestelde vragen over Windows Virtual Machines
In dit artikel komen enkele veelgestelde vragen over Windows virtuele machines die in Azure met behulp van de Resource Manager-implementatiemodel zijn gemaakt. Zie voor de Linux-versie van dit onderwerp, [Veelgestelde vragen over virtuele Linux-Machines](../linux/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Wat kan ik uitvoeren op een VM van Azure?
Alle abonnees kunnen serversoftware uitvoeren op een virtuele machine van Azure. Zie voor meer informatie over het ondersteuningsbeleid voor actieve Microsoft-serversoftware in Azure [ondersteuning van Microsoft-serversoftware voor Azure Virtual Machines](https://support.microsoft.com/kb/2721672)

Bepaalde versies van Windows 7, Windows 8.1 en Windows 10 zijn beschikbaar voor MSDN Azure-voordeel abonnees en abonnees van MSDN ontwikkelen en testen van betalen per gebruik, voor ontwikkelings- en taken. Zie het Engelstalige blogbericht [Windows Client images for MSDN subscribers](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/) voor meer informatie, zoals instructies en beperkingen. 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Hoeveel opslagruimte kan ik gebruiken met een virtuele machine?
Elke gegevensschijf kan tot 4 TB (4095 GB) zijn. Het aantal gegevensschijven dat u kunt gebruiken, is afhankelijk van de grootte van de virtuele machine. Zie [Grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor meer informatie.

Azure Managed Disks is de aanbevolen product voor schijfopslag gebruiken met Azure Virtual Machines voor permanente opslag van gegevens. U kunt voor elke virtuele machine Managed Disks gebruiken. Azure Managed Disks biedt twee typen duurzame opslag: Premium en Standard Managed Disks. Zie voor informatie over de prijzen [Managed Disks-prijzen](https://azure.microsoft.com/pricing/details/managed-disks).

Azure storage-accounts kunnen ook opslag bieden voor de schijf van het besturingssysteem en eventuele gegevensschijven. Elke schijf is een VHD-bestand dat wordt opgeslagen als een pagina-blob. Zie [deze pagina](https://azure.microsoft.com/pricing/details/storage/) voor prijsinformatie.

## <a name="how-can-i-access-my-virtual-machine"></a>Hoe krijg ik toegang tot mijn virtuele machine?
Een externe verbinding maken met het Remote Desktop Connection (RDP) voor een Windows-VM. Zie voor instructies [hoe u verbinding maken met en meld u aan een virtuele Azure-machine waarop Windows wordt uitgevoerd bij](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Maximaal twee gelijktijdige verbindingen worden ondersteund, tenzij de server is geconfigureerd als een extern bureaublad-Services-sessiehost.  

Als u problemen met extern bureaublad ondervindt, raadpleegt u [problemen oplossen met extern bureaublad-verbindingen met een Windows-gebaseerde virtuele Machine in Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Als u bekend bent met Hyper-V, verwacht u misschien een hulpprogramma zoals VMConnect. Azure biedt echter geen vergelijkbaar hulpprogramma omdat consoletoegang tot een virtuele machine niet wordt ondersteund.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Kan ik de tijdelijke schijf (standaard de D:-schijf) gebruiken voor het opslaan van gegevens?
Gebruik niet de tijdelijke schijf voor het opslaan van gegevens. Dit is alleen tijdelijke opslag, zodat u risico loopt gegevens kwijt die niet worden hersteld. Verlies van gegevens kan optreden wanneer de virtuele machine wordt verplaatst naar een andere host. Hiervoor zijn verschillende redenen te bedenken, zoals het aanpassen van de grootte van een virtuele machine, het bijwerken van de host of een hardwarefout op de host.

Als u een toepassing die moet worden gebruikt de letter D: hebt, kunt u de stationsletters toewijzen zodat de tijdelijke schijf wordt gebruikt dan D:. Zie [Use the D: drive as a data drive on a Windows VM](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (Het station D: als gegevensstation gebruiken op een Windows-VM) voor instructies.


## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Hoe wijzig ik de stationsletter van de tijdelijke schijf?
U kunt de stationsletter wijzigen door het wisselbestand te verplaatsen en stationsletters, maar moet u ervoor dat u de stappen in een specifieke volgorde. Zie [Use the D: drive as a data drive on a Windows VM](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (Het station D: als gegevensstation gebruiken op een Windows-VM) voor instructies.

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Kan ik een bestaande virtuele machine toevoegen aan een beschikbaarheidsset?
Nee. Als u wilt dat uw virtuele machine moet deel uitmaken van een beschikbaarheidsset, moet u de virtuele machine in de set maken. Er is momenteel geen een manier om een VM toevoegen aan een beschikbaarheidsset nadat deze is gemaakt.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Kan ik een virtuele machine uploaden naar Azure?
Ja. Zie voor instructies [migreren on-premises machines naar Azure](on-prem-to-azure.md).

## <a name="can-i-resize-the-os-disk"></a>Kan ik het formaat van de besturingssysteemschijf?
Ja. Zie voor instructies [over het uitbreiden van de besturingssysteemschijf van een virtuele Machine in een Azure-resourcegroep](expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kan ik kopiëren of klonen van een bestaande VM in Azure?
Ja. Beheerde installatiekopieën gebruikt, kunt u een installatiekopie van een virtuele machine maken en deze vervolgens met de installatiekopie van het bouwen van meerdere nieuwe virtuele machines. Zie voor instructies [een aangepaste installatiekopie van een virtuele machine maken](tutorial-custom-images.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Waarom zie ik Canada-centraal en Canada-Oost regio's via Azure Resource Manager?

De twee nieuwe regio's Canada-centraal en Canada-Oost worden niet automatisch geregistreerd voor virtuele machines worden gemaakt voor bestaande Azure-abonnementen. Deze registratie gebeurt automatisch wanneer een virtuele machine wordt geïmplementeerd via Azure portal naar elke andere regio met Azure Resource Manager. Nadat een virtuele machine op een andere Azure-regio is geïmplementeerd, moeten de nieuwe regio's beschikbaar zijn voor latere virtuele machines zijn.

## <a name="does-azure-support-linux-vms"></a>Azure biedt ondersteuning voor Linux-VM's?
Ja. Zie voor het snel maken van een Linux-VM om uit te proberen, [een Linux-VM maken op Azure met behulp van de Portal](../linux/quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kan ik een NIC toevoegen aan mijn virtuele machine nadat deze gemaakt?
Ja, dit is nu mogelijk. De virtuele machine eerst moet worden gestopt toewijzing ongedaan gemaakt. Vervolgens kunt u toevoegen of verwijderen van een NIC (tenzij dit de laatste NIC op de virtuele machine is). 

## <a name="are-there-any-computer-name-requirements"></a>Zijn er vereisten voor elke computer de naam?
Ja. Naam van de computer kan een maximum van 15 tekens lang zijn. Zie [Naming conventions regels en beperkingen](/azure/architecture/best-practices/naming-conventions#compute) voor meer informatie over de naamgeving van uw resources.

## <a name="are-there-any-resource-group-name-requirements"></a>Zijn er een resource vereisten voor de naam groep?
Ja. Naam van de resourcegroep mag maximaal 90 tekens lang zijn. Zie [Naming conventions regels en beperkingen](/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions) voor meer informatie over resourcegroepen.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Wat zijn de vereisten van de gebruikersnaam bij het maken van een virtuele machine?

Gebruikersnamen kan maximaal 20 tekens lang zijn en mag niet eindigen op een punt ("."). 


De volgende gebruikersnamen zijn niet toegestaan:
<table>
    <tr>
        <td style="text-align:center">1</td><td style="text-align:center">123</td><td style="text-align:center">a</td><td style="text-align:center">actuser</td>
    </tr>
    <tr>
        <td style="text-align:center">adm</td><td style="text-align:center">beheerder</td><td style="text-align:center">admin1</td><td style="text-align:center">admin2</td>
    </tr>   <tr>
        <td style="text-align:center">de beheerder</td><td style="text-align:center">ASPNET</td><td style="text-align:center">Back-up</td><td style="text-align:center">console</td>
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
        <td style="text-align:center">Gebruiker2</td><td style="text-align:center">gebruiker3</td><td style="text-align:center">Gebruiker4</td><td style="text-align:center">gebruiker5</td>
    </tr>
</table>

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Wat zijn de wachtwoordvereisten bij het maken van een virtuele machine?
Wachtwoorden moeten tussen 12 en 123 tekens lang zijn en voldoen aan 3 van de volgende 4 complexiteitsvereisten voldoen:

* Lagere tekens bevatten
* Bovenste tekens bevatten
* Een cijfer zijn
* Een speciaal teken bevatten (reguliere expressie komt overeen met [\W_])

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
