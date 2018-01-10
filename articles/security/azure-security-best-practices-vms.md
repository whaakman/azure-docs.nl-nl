---
title: Aanbevolen beveiligingsprocedures voor virtuele machine van Azure
description: In dit artikel biedt tal van best practices voor beveiliging moet worden gebruikt in virtuele machines in Azure.
services: security
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 5e757abe-16f6-41d5-b1be-e647100036d8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: yurid
ms.openlocfilehash: 6541d09d7f1a7e85333f54797dba7db79328e9de
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="best-practices-for-azure-vm-security"></a>Aanbevolen procedures voor beveiliging van de virtuele machine in Azure

In de meeste infrastructuur als een dienst (IaaS)-scenario's [virtuele Azure-machines (VM's)](https://docs.microsoft.com/azure/virtual-machines/) zijn de belangrijkste werkbelasting voor organisaties die gebruikmaken van de cloud computing. Dit is vooral duidelijk in [hybride scenario's](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) organisaties waar langzaam migreren van werkbelastingen naar de cloud. In dergelijke scenario's, voert u de [algemene beveiligingsoverwegingen voor IaaS](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx), en aanbevolen procedures voor beveiliging toepassen op alle virtuele machines.

Dit artikel worden de verschillende VM best practices voor beveiliging, elk afgeleid van onze klanten en ons eigen direct ervaringen met virtuele machines.

De best practices zijn gebaseerd op een consensus van advies en ze werken met de huidige Azure-platformmogelijkheden en functies sets. Omdat adviezen en technologieën na verloop van tijd veranderen kunnen, die we willen dit artikel regelmatig zodat de wijzigingen worden bijgewerkt.

Voor elke aanbevolen procedure is om het artikel wordt uitgelegd:

* Wat de beste handelswijze is.
* Waarom is het een goed idee in te schakelen.
* U leert hoe deze in te schakelen.
* Wat kan gebeuren als u niet in te schakelen.
* Mogelijke alternatieven voor de aanbevolen procedure.

Het artikel onderzoekt de volgende aanbevolen procedures van de VM-beveiliging:

* VM-verificatie en toegangsbeheer
* Toegang tot de beschikbaarheid en netwerken van de virtuele machine
* Gegevens in rust in virtuele machines beveiligen door af te dwingen van versleuteling
* Uw VM-updates beheren
* Uw beveiligingspostuur VM beheren
* VM-prestaties bewaken

## <a name="vm-authentication-and-access-control"></a>VM-verificatie en toegangsbeheer

De eerste stap bij het beveiligen van uw virtuele machine is om ervoor te zorgen dat alleen geautoriseerde gebruikers kunnen nieuwe VM's instellen. U kunt [Azure beleid](../azure-policy/azure-policy-introduction.md) om vast te stellen conventies voor resources in uw organisatie, aangepaste beleidsregels maken en deze beleidsregels van toepassing op resources, zoals [resourcegroepen](../azure-resource-manager/resource-group-overview.md).

Virtuele machines die deel uitmaken van een resourcegroep natuurlijk neemt de beleidsregels. Hoewel we deze benadering voor het beheren van virtuele machines wordt aangeraden, kunt u ook toegang tot afzonderlijke VM-beleid beheren met behulp van [op rollen gebaseerde toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md).

Wanneer u beleidsregels voor Resource Manager en RBAC VM toegangsbeheer inschakelt, kunt u helpen verbeteren de algehele beveiliging van de virtuele machine. Het is raadzaam dat u virtuele machines met de dezelfde levenscyclus in dezelfde resourcegroep samenvoegen. Met behulp van resourcegroepen kunt u implementeren, bewaken en samengevouwen facturering kosten voor uw resources. Gebruiken om gebruikers te openen en het instellen van virtuele machines, een [minimale bevoegdheid benadering](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models). En als u rechten aan gebruikers toewijst, wilt gebruiken de volgende ingebouwde Azure-rollen:

- [Virtual Machine Contributor](../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor): VM's, maar niet het virtuele netwerk of opslag account waaraan ze zijn verbonden kunt beheren.
- [Klassieke Virtual Machine Contributor](../active-directory/role-based-access-built-in-roles.md#classic-virtual-machine-contributor): virtuele machines die zijn gemaakt met behulp van het klassieke implementatiemodel, maar niet het virtuele netwerk of opslag account waarmee de virtuele machines zijn verbonden kunt beheren.
- [Beveiligingsbeheer](../active-directory/role-based-access-built-in-roles.md#security-manager): beveiligingsonderdelen, beveiligingsbeleid en virtuele machines kunt beheren.
- [DevTest Labs gebruiker](../active-directory/role-based-access-built-in-roles.md#devtest-labs-user): kunt Alles weergeven en verbinding maken, starten, opnieuw opstarten en afsluiten van virtuele machines.

Deel geen accounts en wachtwoorden tussen beheerders en gebruik wachtwoorden niet niet meerdere gebruikersaccounts of services, met name de wachtwoorden voor sociale media of andere niet-administratieve activiteiten. In het ideale geval moet u [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) sjablonen voor het instellen van uw virtuele machines veilig. U kunt met deze benadering versterking van uw implementatie-instellingen en beveiligingsinstellingen in de implementatie worden afgedwongen.

Organisaties die gegevens toegangsbeheer niet afdwingen door gebruik te maken van mogelijkheden, zoals RBAC mogelijk verleend op basis van hun gebruikers meer bevoegdheden beschikt dan noodzakelijk is. Ongeschikte gebruikerstoegang tot bepaalde gegevens kan die gegevens rechtstreeks beschadigen.

## <a name="vm-availability-and-network-access"></a>Toegang tot de beschikbaarheid en netwerken van de virtuele machine

Als uw virtuele machine wordt uitgevoerd voor kritieke toepassingen die moeten een hoge beschikbaarheid hebben, wordt aangeraden dat u meerdere virtuele machines. Voor betere beschikbaarheid, maakt u ten minste twee virtuele machines in de [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md).

[Azure Load Balancer](../load-balancer/load-balancer-overview.md) vereist ook dat netwerktaakverdeling virtuele machines deel uitmaken van dezelfde beschikbaarheidsset. Als deze virtuele machines moeten worden geopend via Internet, moet u een [Internet gerichte load balancer](../load-balancer/load-balancer-internet-overview.md).

Bij virtuele machines worden blootgesteld aan Internet, is het belangrijk dat u [netwerkverkeer met netwerkbeveiligingsgroepen (nsg's) bepalen](../virtual-network/virtual-networks-nsg.md). Omdat nsg's kunnen worden toegepast op subnetten, kunt u het aantal nsg's minimaliseren door uw resources groeperen per subnet en nsg's vervolgens naar de subnetten toe te passen. De bedoeling is om het maken van een laag van netwerkisolatie, u doen kunt door het instellen van de [netwerkbeveiliging](../best-practices-network-security.md) mogelijkheden in Azure.

U kunt ook de functie voor just-in-time-(Just in time) VM-toegang van Azure Security Center gebruiken om te bepalen wie externe toegang tot een specifieke virtuele machine en hoe lang is.

Organisaties die niet netwerk toegangsbeperkingen internetgerichte virtuele machines afdwingen worden blootgesteld aan beveiligingsrisico's, zoals een beveiligingsaanval van Remote Desktop Protocol (RDP).

## <a name="protect-data-at-rest-in-your-vms-by-enforcing-encryption"></a>Gegevens in rust in uw virtuele machines beveiligen door af te dwingen van versleuteling

[Gegevensversleuteling in rust](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is een verplichte stap naar privacy, naleving en gegevens onafhankelijkheid van gegevens. [Azure Disk Encryption](../security/azure-security-disk-encryption.md) kunnen IT-beheerders voor het versleutelen van Windows en Linux IaaS VM-schijven. Schijfversleuteling combineert de industriestandaard Windows BitLocker-onderdeel en de functie Linux dm-crypt voor volumeversleuteling voor het besturingssysteem en de gegevensschijven.

Uw gegevens om te voldoen aan uw organisatie beveiligings- en nalevingsvereisten, kunt u schijfversleuteling helpt bij de beveiliging toepassen. Uw organisatie moet rekening houden met behulp van versleuteling op risico's met betrekking tot niet-geautoriseerde gegevenstoegang verminderen. Bovendien aangeraden om uw stations te versleutelen, voordat u gevoelige gegevens naar deze schrijft.

Zorg ervoor dat voor het versleutelen van uw VM gegevensvolumes om ze te beveiligen in rust in uw Azure storage-account. Bescherming van de versleutelingssleutels en geheim met behulp van [Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-whatis/).

Organisaties die niet gegevensversleuteling afdwingen worden meer blootgesteld aan problemen van de integriteit van gegevens. Onbevoegde of rogue gebruikers kunnen bijvoorbeeld gestolen gegevens in de accounts waarmee is geknoeid of onbevoegde toegang te krijgen tot gegevens die zijn gecodeerd in ClearFormat. Behalve op dergelijke risico's, om te voldoen aan de regelgeving vanuit de sector, moeten bedrijven bewijzen dat zij toewijding inzetten uitoefenen en met behulp van besturingselementen voor de juiste beveiliging om de gegevensbeveiliging van hun te garanderen.

Zie voor meer informatie over schijfversleuteling, [Azure Disk Encryption for Windows en Linux IaaS VM's](azure-security-disk-encryption.md).


## <a name="manage-your-vm-updates"></a>Uw VM-updates beheren

Omdat Azure VM's, zoals alle lokale VM's zijn bedoeld voor gebruikers wordt beheerd, push niet Azure-Windows-updates toe. U bent, echter aangeraden te laat u de automatische Update van Windows-instelling is ingeschakeld. Er is een andere optie voor het implementeren van [Windows Server Update Services (WSUS)](https://technet.microsoft.com/windowsserver/bb332157.aspx) of een ander geschikte updatebeheer product op een andere virtuele machine of on-premises. Houd virtuele machines huidige zowel WSUS als Windows Update. Bovendien aangeraden om een scannen product te gebruiken om te controleren of alle IaaS-VM's zijn bijgewerkt.

Voorraad installatiekopieën die zijn verstrekt door Azure zijn regelmatig bijgewerkt met de meest recente ronde van Windows-updates. Er is echter geen garantie dat de installatiekopieën van het huidige tijdens de implementatie worden. Een kleine vertraging (van niet meer dan een paar weken) na openbare releases mogelijk. Controleren en installeren van alle Windows-updates moet de eerste stap van elke implementatie. Deze meting is vooral belangrijk bij het implementeren van installatiekopieën die afkomstig van u of uw eigen bibliotheek zijn toepassen. Installatiekopieën die worden geleverd als onderdeel van de Azure Marketplace worden standaard automatisch bijgewerkt.

Organisaties die afdwingen van beleid voor software-update niet zijn meer zichtbaar voor bedreigingen die gebruikmaken van bekende, eerder vaste beveiligingsproblemen. Behalve dat u dergelijke bedreigingen, om te voldoen aan de regelgeving vanuit de sector, moeten bedrijven bewijzen dat zij toewijding inzetten uitoefenen en met behulp van de juiste beveiligingscontroles om te zorgen voor de beveiliging van hun werkbelasting die in de cloud bevindt.

Het is belangrijk om te benadrukken dat software-update aanbevolen procedures voor traditionele datacentra en Azure IaaS veel overeenkomsten hebben. Daarom raden we aan het evalueren van uw huidige beleid voor software-update om op te nemen van virtuele machines.

## <a name="manage-your-vm-security-posture"></a>Uw beveiligingspostuur VM beheren

Cyberbeveiliging bedreigingen zijn in ontwikkeling en beveiliging van uw virtuele machines moet een uitgebreide bewaking mogelijkheid die u kunnen snel bedreigingen te detecteren, voorkomen dat onbevoegde toegang tot uw resources, waarschuwing is geactiveerd en fout-positieven te reduceren. De beveiligingspostuur voor dergelijke workload omvat alle beveiligingsaspecten van de virtuele machine uit het updatebeheer voor het beveiligen van toegang tot het netwerk.

Voor het bewaken van de beveiligingsstatus van uw [Windows](../security-center/security-center-virtual-machine.md) en [virtuele Linux-machines](../security-center/security-center-linux-virtual-machine.md), gebruik [Azure Security Center](../security-center/security-center-intro.md). In Azure Security Center bescherming van uw virtuele machines door gebruik te maken van de volgende mogelijkheden:

* OS-beveiligingsinstellingen aan de regels van de aanbevolen configuratie toepassen
* Identificeren en beveiliging van het systeem en essentiële updates ontbreken mogelijk downloaden
* Eindpunt antimalware bescherming aanbevelingen implementeren
* Schijfversleuteling valideren
* Beoordelen en herstellen van beveiligingsproblemen
* Bedreigingen detecteren

Security Center actief voor bedreigingen kunt bewaken en mogelijke bedreigingen worden weergegeven onder **beveiligingswaarschuwingen**. Gecorreleerde bedreigingen zijn geaggregeerd in één weergave aangeroepen **Security Incident**.

Om te begrijpen hoe Security Center kan u helpen identificeren mogelijke bedreigingen in uw virtuele machines die zich in Azure, moet u de volgende video bekijken:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Security-Center-in-Incident-Response/player]

Organisaties die een sterke beveiligingspostuur niet voor hun virtuele machines afdwingen blijven niet op de hoogte van mogelijke pogingen door onbevoegden te omzeilen tot stand gebrachte beveiligingsmechanismen.

## <a name="monitor-vm-performance"></a>VM-prestaties bewaken

Misbruik van de resource is een probleem als VM processen meer bronnen dan zou moeten gebruiken. Prestatieproblemen met een VM kunnen leiden tot een onderbreking van de service die niet voldoet aan de beveiligingsprincipal van beschikbaarheid. Daarom is het noodzakelijk is voor het bewaken van VM-toegang is het niet alleen reactief terwijl er een probleem optreedt, maar ook proactief tegen basislijn gemeten tijdens normale werking.

Door te analyseren [Azure diagnostische logboekbestanden](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/), kunt u uw VM-resources bewaken en mogelijke problemen die op de prestaties en beschikbaarheid hebben kunnen. De extensie voor diagnostische gegevens van Azure biedt mogelijkheden voor controle en diagnostische gegevens op Windows gebaseerde virtuele machines. U kunt deze mogelijkheden inschakelen door de uitbreiding als onderdeel van de [Azure Resource Manager-sjabloon](../virtual-machines/windows/extensions-diagnostics-template.md).

U kunt ook [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-metrics.md) meer inzicht verkrijgen in de status van uw resources.

Organisaties die geen bewaking van prestaties van de virtuele machine zijn kan niet bepalen of bepaalde wijzigingen in prestatiepatronen normale of abnormale zijn. Als de virtuele machine meer bronnen dan normaal verbruikt, kan deze een afwijkingsdetectie duiden op bij een aanval van een externe resource of een waarmee is geknoeid proces in de virtuele machine wordt uitgevoerd.
