---
title: Beoordeling van grote aantallen Hyper-V VM's voor migratie naar Azure met Azure Migrate | Microsoft Docs
description: Hierin wordt beschreven hoe u grote aantallen Hyper-V VM's voor migratie naar Azure met behulp van de service Azure Migrate beoordeelt.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: 95704f2694892b349d0967fca2160dabd990b472
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811542"
---
# <a name="assess-large-numbers-of-hyper-v-vms-for-migration-to-azure"></a>Beoordeling van grote aantallen Hyper-V VM's voor migratie naar Azure

Dit artikel wordt beschreven hoe u beoordeelt groot (> 1000) on-premises Hyper-V VM's voor migratie naar Azure met behulp van het hulpprogramma Azure Migrate-Server-evaluatie.

[Azure Migrate](migrate-services-overview.md) biedt een hub van hulpprogramma's die u helpen te detecteren, beoordelen en apps, infrastructuur en workloads migreren naar Microsoft Azure. De hub bevat Azure Migrate-hulpprogramma's en aanbiedingen van derden independent software vendor (ISV). 


In dit artikel leert u het volgende:
> [!div class="checklist"]
> * Plan voor evaluatie op schaal.
> * Azure-machtigingen configureren en Hyper-V voorbereiden voor evaluatie.
> * Maak een Azure Migrate-project en een evaluatie maken.
> * Een nieuwe beoordeling als u van plan voor de migratie bent.


> [!NOTE]
> Als u een proof wilt-of-concept voor een aantal virtuele machines evalueren voordat het beoordelen van op grote schaal, gaat u als volgt proberen onze [reeks](tutorial-prepare-hyper-v.md)

## <a name="plan-for-assessment"></a>Plan voor de beoordeling

Bij het plannen van evaluatie van het grote aantal Hyper-V-machines, moet u er een aantal dingen om na te denken over zijn:

- **Plannen van projecten Azure Migrate**: Over het implementeren van Azure Migrate-projecten te achterhalen. Bijvoorbeeld, als uw datacenters in verschillende fysieke locaties zijn of u moet voor het opslaan van detectie, beoordeling of metagegevens met betrekking tot migratie in een andere Geografie bevinden, moet u mogelijk meerdere projecten.
- **Apparaten van plan bent**: Azure Migrate gebruikt een on-premises Azure Migrate apparaat, geïmplementeerd als een Hyper-V-VM, VM's voortdurend detecteren voor evaluatie en migratie. Het apparaat controleert omgevingswijzigingen, zoals het toevoegen van virtuele machines, schijven en netwerkadapters. Het ook verzendt metagegevens en prestaties van gegevens over deze naar Azure. U wilt achterhalen hoeveel apparaten te implementeren.


## <a name="planning-limits"></a>Grenzen plannen
 
Gebruik de limieten die zijn samengevat in deze tabel voor de planning.

**Planning** | **Limieten**
--- | --- 
**Azure Migrate-projecten** | Maximaal 10.000 virtuele machines in een project te beoordelen.
**Azure Migrate-apparaat** | Een apparaat kan maximaal 5000 VM's detecteren.<br/> Een apparaat kan verbinding maken met maximaal 300 Hyper-V-hosts.<br/> Een apparaat kan alleen worden gekoppeld aan een enkele Azure Migrate-project.<br/><br/> 
**Azure Migrate-evaluatie** | U kunt maximaal 10.000 virtuele machines in een enkele beoordeling beoordelen.



## <a name="other-planning-considerations"></a>Andere overwegingen bij het plannen

- Als u wilt detecteren starten vanaf het apparaat, die u moet elke Hyper-V-host selecteren. 
- Als u met een omgeving met meerdere tenants werkt, ontdekt u kan niet op dit moment alleen virtuele machines die deel uitmaken van een specifieke tenant. 

## <a name="prepare-for-assessment"></a>Voorbereiden voor de beoordeling

Voorbereiden op Azure en Hyper-V server-evaluatie. 

1. Controleer of [Hyper-V support vereisten en beperkingen](migrate-support-matrix-hyper-v.md).
2. Instellen van machtigingen voor uw Azure-account om te communiceren met Azure Migrate
3. Hyper-V-hosts en virtuele machines voorbereiden

Volg de instructies in [in deze zelfstudie](tutorial-prepare-hyper-v.md) om deze instellingen te configureren.

## <a name="create-a-project"></a>Een project maken

In overeenstemming met uw planning vereisten, het volgende doen:

1. Maak een Azure Migrate-projecten.
2. Het hulpprogramma Azure Migrate-evaluatie-Server toevoegen aan de projecten.

[Meer informatie](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Maken en een evaluatie bekijken

1. Een evaluatie voor Hyper-V-machines maken.
1. Bekijk de evaluaties in voorbereiding voor beheerders voor migratieplanning.

[Meer informatie](tutorial-assess-hyper-v.md) over het maken en controleren van evaluaties.
    

## <a name="next-steps"></a>Volgende stappen

In dit artikel leert u het volgende:
 
> [!div class="checklist"] 
> * Gepland voor het schalen van Azure Migrate-evaluaties voor Hyper-V-machines
> * Azure en Hyper-V voor evaluatie voorbereid
> * Een Azure Migrate-project gemaakt en uitgevoerd evaluaties
> * Evaluaties ter voorbereiding op voor de migratie gecontroleerd.

Nu [informatie over hoe](concepts-assessment-calculation.md) evaluaties worden berekend, en hoe u [evaluaties wijzigen](how-to-modify-assessment.md)
