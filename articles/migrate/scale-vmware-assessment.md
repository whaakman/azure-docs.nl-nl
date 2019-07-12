---
title: Beoordeling van grote aantallen VMware-VM's voor migratie naar Azure met Azure Migrate | Microsoft Docs
description: Hierin wordt beschreven hoe u grote aantallen VMware-VM's voor migratie naar Azure met behulp van de service Azure Migrate beoordeelt.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: 6102a1c59be3627b95dc1e0cb1d1d712d5832d2f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811334"
---
# <a name="assess-large-numbers-of-vmware-vms-for-migration-to-azure"></a>Beoordeling van grote aantallen VMware-VM's voor migratie naar Azure


Dit artikel wordt beschreven hoe u grote aantallen (1000 35.000) beoordeelt on-premises VMware-VM's voor migratie naar Azure met behulp van het hulpprogramma Azure Migrate-Server-evaluatie

[Azure Migrate](migrate-services-overview.md) biedt een hub van hulpprogramma's die u helpen te detecteren, beoordelen en apps, infrastructuur en workloads migreren naar Microsoft Azure. De hub bevat Azure Migrate-hulpprogramma's en aanbiedingen van derden independent software vendor (ISV). 

In dit artikel leert u het volgende:
> [!div class="checklist"]
> * Plan voor evaluatie op schaal.
> * Azure-machtigingen configureren en VMware voorbereiden voor evaluatie.
> * Maak een Azure Migrate-project en een evaluatie maken.
> * Een nieuwe beoordeling als u van plan voor de migratie bent.


> [!NOTE]
> Als u een proof wilt-of-concept voor een aantal virtuele machines evalueren voordat het beoordelen van op grote schaal, gaat u als volgt proberen onze [reeks](tutorial-prepare-vmware.md)

## <a name="plan-for-assessment"></a>Plan voor de beoordeling

Bij het plannen van evaluatie van een groot aantal virtuele VMware-machines, moet u er een aantal dingen om na te denken over zijn:

- **Plannen van projecten Azure Migrate**: Over het implementeren van Azure Migrate-projecten te achterhalen. Bijvoorbeeld, als uw datacenters in verschillende fysieke locaties zijn of u moet voor het opslaan van detectie, beoordeling of metagegevens met betrekking tot migratie in een andere Geografie bevinden, moet u mogelijk meerdere projecten. 
- **Apparaten van plan bent**: Azure Migrate gebruikt een on-premises Azure Migrate apparaat, geïmplementeerd als een VMware-VM, VM's voortdurend te detecteren. Het apparaat controleert omgevingswijzigingen, zoals het toevoegen van virtuele machines, schijven en netwerkadapters. Het ook verzendt metagegevens en prestaties van gegevens over deze naar Azure. U wilt achterhalen hoeveel apparaten die u wilt implementeren.
- **Accounts voor detectie van plan bent**: Het apparaat Azure Migrate maakt gebruik van een account met toegang tot de vCenter-Server om te kunnen virtuele machines detecteren voor evaluatie en migratie. Als u meer dan 10.000 VM's detecteren bent, meerdere accounts instellen.


## <a name="planning-limits"></a>Grenzen plannen
 
Gebruik de limieten die zijn samengevat in deze tabel voor de planning.

**Planning** | **Limieten**
--- | --- 
**Azure Migrate-projecten** | Maximaal 35.000 virtuele machines in een project te beoordelen.
**Azure Migrate-apparaat** | Een apparaat kan alleen verbinding maken met een enkele vCenter-Server.<br/><br/> Een apparaat kan alleen worden gekoppeld aan een enkele Azure Migrate-project.<br/> Een apparaat kan maximaal 10.000 virtuele machines op een vCenter-Server te detecteren.
**Azure Migrate-evaluatie** | U kunt maximaal 35.000 virtuele machines in een enkele beoordeling beoordelen.

Met deze limieten in gedachten vindt hier u enkele Voorbeeldimplementaties:


**vCenter-server** | **Virtuele machines op de server** | **Aanbeveling** | **Actie**
---|---|---
Een | < 10,000 | Een Azure Migrate-project.<br/> Een apparaat.<br/> Een vCenter-account voor detectie. | Apparaat instellen, verbinding maken met vCenter-Server met een account.
Een | > 10,000 | Een Azure Migrate-project.<br/> Meerdere apparaten.<br/> Meerdere vCenter-accounts. | Apparaat instellen voor elke 10.000 VM's.<br/><br/> VCenter-accounts instellen en delen van inventaris om toegang voor een account met minder dan 10.000 VM's te beperken.<br/> Elk apparaat verbinden met vCenter-server met een account.<br/> U kunt de afhankelijkheden tussen computers die zijn gedetecteerd met verschillende apparaten analyseren.
Meerdere | < 10,000 |  Een Azure Migrate-project.<br/> Meerdere apparaten.<br/> Een vCenter-account voor detectie. | Apparaten instellen, verbinding maken met vCenter-Server met een account.<br/> U kunt de afhankelijkheden tussen computers die zijn gedetecteerd met verschillende apparaten analyseren.
Meerdere | > 10,000 | Een Azure Migrate-project.<br/> Meerdere apparaten.<br/> Meerdere vCenter-accounts. | Als detectie van vCenter Server < 10.000 VM's, stelt u een apparaat voor elke vCenter-Server.<br/><br/> Als detectie van vCenter-Server > 10.000 virtuele machines, instellen van een apparaat voor elke 10.000 virtuele machines.<br/> VCenter-accounts instellen en delen van inventaris om toegang voor een account met minder dan 10.000 VM's te beperken.<br/> Elk apparaat verbinden met vCenter-server met een account.<br/> U kunt de afhankelijkheden tussen computers die zijn gedetecteerd met verschillende apparaten analyseren.


## <a name="plan-discovery-in-a-multi-tenant-environment"></a>Plannen van detectie in een omgeving met meerdere tenants

Als u van plan bent voor een omgeving met meerdere tenants, kunt u het bereik van detectie van vCenter-Server.

- U kunt het detectiebereik apparaat instellen op een vCenter Server datacenter, cluster of map van clusters, host of de map van de hosts of afzonderlijke virtuele machines.
- Als uw omgeving wordt gedeeld door tenants en u niet wilt dat elke tenant afzonderlijk detecteren, kunt u het bereik van toegang tot de vCenter-account dat het apparaat voor detectie gebruikt. 
    - Als de tenants hosts delen, maakt u referenties met alleen-lezentoegang voor de virtuele machines die deel uitmaken van de specifieke tenant. 
    - Gebruikt deze referenties voor de detectie van apparaat Azure Migrate.
    - Azure Migrate-evaluatie kan geen virtuele machines detecteren als de vCenter-account toegang verleend op het niveau van de vCenter-VM-map heeft. Mappen van hosts en clusters worden ondersteund. 

## <a name="prepare-for-assessment"></a>Voorbereiden voor de beoordeling

Azure en VMware voorbereiden voor server-evaluatie. 

1. Controleer of [VMware ondersteunen vereisten en beperkingen](migrate-support-matrix-vmware.md).
2. Instellen van machtigingen voor uw Azure-account om te communiceren met Azure Migrate.
3. VMware voorbereiden voor evaluatie.


Volg de instructies in [in deze zelfstudie](tutorial-prepare-vmware.md) om deze instellingen te configureren.


## <a name="create-a-project"></a>Een project maken

In overeenstemming met uw planning vereisten, het volgende doen:

1. Maak een Azure Migrate-projecten.
2. Het hulpprogramma Azure Migrate-evaluatie-Server toevoegen aan de projecten.

[Meer informatie](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Maken en een evaluatie bekijken

1. Een evaluatie voor VMware-VM's maken.
1. Bekijk de evaluaties in voorbereiding voor beheerders voor migratieplanning.


Volg de instructies in [in deze zelfstudie](tutorial-assess-vmware.md) om deze instellingen te configureren.
    

## <a name="next-steps"></a>Volgende stappen

In dit artikel leert u het volgende:
 
> [!div class="checklist"] 
> * Gepland voor het schalen van Azure Migrate-evaluaties voor virtuele VMware-machines
> * Bereid Azure en VMware voor de beoordeling
> * Een Azure Migrate-project gemaakt en uitgevoerd evaluaties
> * Evaluaties ter voorbereiding op voor de migratie gecontroleerd.

Nu [informatie over hoe](concepts-assessment-calculation.md) evaluaties worden berekend, en hoe u [evaluaties wijzigen](how-to-modify-assessment.md).
