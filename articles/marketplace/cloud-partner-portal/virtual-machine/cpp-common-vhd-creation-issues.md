---
title: Veelvoorkomende problemen tijdens het maken van VHD (FAQ) voor de Azure Marketplace | Microsoft Docs
description: Veelgestelde vragen over het maken van VHD en bijbehorende problemen.
services: Azure Marketplace
documentationcenter: ''
author: HannibalSII
manager: Patrick.Butler
editor: ''
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 10/02/2018
ms.author: hascipio; v-divte; v-miclar
ms.openlocfilehash: 381f88c4641417bceca0f988d4b1a187aedaa642
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56327303"
---
# <a name="common-issues-during-vhd-creation-faq"></a>Veelvoorkomende problemen tijdens het maken van VHD (FAQ)

De volgende veelgestelde vragen (FAQ) voor veelvoorkomende problemen opgetreden tijdens de virtuele harde schijf (VHD) en de virtuele machine (VM) maken voor aanbiedingen van de virtuele machine. 

## <a name="how-do-you-create-a-vm-from-the-azure-portal-using-the-vhd-that-is-uploaded-to-premium-storage"></a>Hoe maak ik een virtuele machine vanuit de Azure-portal met behulp van de VHD die is geüpload naar premium storage?

Azure Marketplace biedt momenteel geen ondersteuning voor het maken van het VM-aanbiedingen van installatiekopieën op beheerde opslag of naar Azure Premium Storage.  Zie voor meer informatie over deze opslagopties [overzicht van Azure Managed Disks](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview).


## <a name="can-you-use-generation-2-vms-for-offers"></a>Kunt u virtuele machines van generatie 2 gebruiken voor aanbiedingen?

Nee, alleen generatie 1 VHD's worden ondersteund.  Echter, momenteel werken we met het Team van Microsoft Azure-Platform voor het onderzoeken van ondersteuning voor virtuele machines van generatie 2.  Zie voor meer informatie over de verschillen [zou ik een generatie 1 of 2 virtuele machine in Hyper-V maken?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)


## <a name="how-do-you-change-the-name-of-the-host"></a>Hoe kan u de naam van de host wijzigen?

U kunt geen.  Zodra de virtuele machine is gemaakt, kunnen de naam van de host gebruikers (waaronder eigenaren) niet bijwerken.


## <a name="how-do-you-reset-the-remote-desktop-service-or-its-sign-in-password"></a>Hoe opnieuw u de extern bureaublad-service of het bijbehorende wachtwoord aanmelden?

De volgende artikelen wordt uitgelegd hoe u extern bureaublad-services opnieuw instellen van wachtwoorden voor Windows en Linux gebaseerde virtuele machines uitvoert:   

- [De service Extern bureaublad of het bijbehorende aanmeldingswachtwoord opnieuw instellen in een virtuele Windows-machine](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
- [Hoe u een Linux-VM-wachtwoord of SSH-sleutel opnieuw instellen, los van de SSH-configuratie en de consistentie van de schijf met behulp van de VMAccess-extensie controleren](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)


## <a name="how-do-you-generate-new-ssh-certificates"></a>Hoe u nieuwe SSH-certificaten genereren?

Het genereren van certificaten in het artikel wordt uitgelegd [Get shared access signature URI voor uw VM-installatiekopie](./cpp-get-sas-uri.md) in de volgende sectie [technische assets voor een VM-aanbieding maken](./cpp-create-technical-assets.md).


## <a name="how-do-you-configure-a-virtual-private-network-vpn-to-work-with-my-vms"></a>Hoe configureer ik een virtueel particulier netwerk (VPN) om te werken met mijn virtuele machines?

Als u van het Azure Resource Manager-implementatiemodel gebruikmaakt, hebt u drie algemene opties van het instellen van een VPN-verbinding:
- [Een op route gebaseerde VPN-gateway maken met behulp van de Azure portal](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-portal)
- [Een op route gebaseerde VPN-gateway maken met behulp van PowerShell](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-powershell)
- [Een op route gebaseerde VPN-gateway maken met behulp van CLI](https://docs.microsoft.com/azure/vpn-gateway/create-routebased-vpn-gateway-cli)


## <a name="what-are-microsoft-support-policies-for-running-microsoft-server-software-on-azure-based-vms"></a>Wat zijn Microsoft-ondersteuningsbeleid voor het uitvoeren van Microsoft-serversoftware op virtuele machines op basis van Azure?

Deze beleidsregels worden beschreven in het artikel ondersteuning [ondersteuning van Microsoft-serversoftware voor virtuele machines die Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).


## <a name="do-virtual-machines-have-unique-identifiers-associated-with-them"></a>Hebben virtuele machines unieke id's die zijn gekoppeld aan deze?

Ja, als die worden gehost op Azure.  Azure wijst een unieke id, de unieke ID van Azure virtuele Machine naar een nieuwe VM-resource die is gemaakt met de naam toe.  Lees voor meer informatie het blogbericht [Azure de unieke ID van de virtuele Machine](https://blogs.msdn.microsoft.com/wasimbloch/2016/10/20/azure-virtual-machine-unique-id/).  U vindt ook deze id via een programma via de [lijst API](https://docs.microsoft.com/rest/api/compute/virtualmachines/list).


## <a name="in-a-vm-how-do-you-manage-the-custom-script-extension-in-the-startup-task"></a>In een virtuele machine, hoe beheer ik de aangepaste scriptextensie in de opstarttaak?

Het volgende artikel wordt uitgelegd hoe u de aangepaste Scriptextensie met behulp van de Azure PowerShell-module, Azure Resource Manager-sjablonen en details van stappen voor probleemoplossing in Windows-systemen: [Aangepaste Scriptextensie voor Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)


## <a name="are-32-bit-applications-or-services-supported-in-the-azure-marketplace"></a>Zijn de 32-bits toepassingen of services die worden ondersteund in de Azure Marketplace?

In het algemeen niet.  De ondersteunde besturingssystemen en standard-services voor Azure VM's zijn alle 64-bits.  Vanuit technisch oogpunt, meest 64-bits besturingssystemen ondersteunen echter met 32-bits versies van toepassingen voor achterwaartse compatibiliteit.  Het gebruik van 32-bits toepassingen als onderdeel van uw VM-oplossing wordt niet ondersteund en daarom is *sterk afgeraden*.  In plaats daarvan uw toepassing als een 64-bits-project compileren.

Raadpleeg voor meer informatie de volgende artikelen:
- [32-bits toepassingen](https://docs.microsoft.com/windows/desktop/WinProg64/running-32-bit-applications)
- [Ondersteuning voor 32-bits besturingssystemen in virtuele machines van Azure](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)
- [Ondersteuning van Microsoft-serversoftware voor virtuele Microsoft Azure-machines](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)


## <a name="every-time-i-try-to-create-an-image-from-my-vhds-i-get-the-error-vhd-is-already-registered-with-image-repository-as-the-resource-in-powershell-i-did-not-create-any-image-before-nor-did-i-find-any-image-with-this-name-in-azure-how-do-i-resolve-this-issue"></a>Telkens wanneer ik wil een installatiekopie maken van mijn VHD's, verschijnt het foutbericht `.VHD is already registered with image repository as the resource` in PowerShell. Een andere afbeelding voordat ik niet hebben gemaakt en heeft ik een afbeelding met deze naam vinden in Azure. Hoe kan ik dit probleem oplossen?

Dit probleem treedt meestal op als de gebruiker een virtuele machine van een VHD met een vergrendeling op ingericht.  Controleer of er geen virtuele machine toegewezen vanaf deze VHD en voer de bewerking vervolgens opnieuw uit.  Als dit probleem zich blijft voordoen, open een ondersteuningsticket, zoals uitgelegd in [ondersteuning voor Cloud Partner-Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-support-for-cloud-partner-portal). 

