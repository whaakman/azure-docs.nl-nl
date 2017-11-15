---
title: Maak een back-up van DPM-werkbelastingen naar de klassieke Azure portal | Microsoft Docs
description: Een inleiding tot de back-ups van DPM-servers via de Azure Backup-service
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
keywords: System Center Data Protection Manager, data protection manager, back-up van dpm
ms.assetid: 8f23972b-d167-4231-b331-e198db3b18b4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: nkolli;giridham;markgal
ms.openlocfilehash: 7f13739c2d3f7fba700b649f27e02913481ca5e5
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Met DPM voorbereiden op het maken van back-ups van workloads in Azure
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Azure Backup-Server (klassiek)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klassiek)](backup-azure-dpm-introduction-classic.md)
>
>

Dit artikel bevat een inleiding tot Microsoft Azure Backup gebruiken om uw System Center Data Protection Manager (DPM)-servers en werkbelastingen te beveiligen. Deze leest, moet u het volgende weten:

* De werking van Azure DPM-server back-up
* De vereisten voor een goede back-ervaring
* De typische fouten zijn opgetreden en het omgaan met hen
* Ondersteunde scenario's

System Center DPM een back-up bestand-en toepassingsgegevens. Gegevensback-ups naar DPM worden opgeslagen op tape op schijf of een back-up naar Azure met Microsoft Azure Backup. DPM communiceert met Azure Backup als volgt:

* **DPM geïmplementeerd als een fysieke server of on-premises virtuele machine** — als DPM wordt geïmplementeerd als een fysieke server of als een lokale Hyper-V virtuele machine, u kunt back-ups naar een Azure Backup-kluis naast de schijf en tape back-up.
* **DPM geïmplementeerd als een virtuele machine van Azure** : van System Center 2012 R2 met Update 3, kan DPM worden geïmplementeerd als een virtuele machine van Azure. Als DPM wordt geïmplementeerd als een Azure virtuele machine die u kunt back-ups naar Azure-schijven gekoppeld aan de DPM Azure virtuele machine of u de opslag van gegevens kan offloaden door een back-up naar een Azure Backup-kluis.

## <a name="why-backup-your-dpm-servers"></a>Waarom een back-up van uw DPM-servers?
De zakelijke voordelen van het gebruik van Azure Backup voor back-ups van DPM-servers zijn onder andere:

* Voor on-premises DPM-implementatie, kunt u de Azure backup gebruiken als alternatief voor implementatie van de lange termijn op tape.
* Voor implementaties van DPM in Azure kunt Azure Backup u de schijf van de Azure-opslag-offload zodat u kunt opschalen door oudere gegevens zijn opgeslagen in Azure Backup en nieuwe gegevens op schijf.

## <a name="how-does-dpm-server-backup-work"></a>Hoe werkt de back-up van DPM-server?
Als u wilt back-up van een virtuele machine, eerst een momentopname van een punt in tijd van de gegevens nodig. De Azure Backup-service initieert van de back-uptaak op het geplande tijdstip, en activeert de Backup-extensie om een momentopname. De Backup-extensie coördineert met de VSS-service in de Gast als u de consistentie en roept de API van de blob momentopname van de Azure Storage-service zodra de consistentiecontrole is bereikt. Dit wordt gedaan om een consistente momentopname van de schijven van de virtuele machine zonder af te sluiten.

Nadat de momentopname is genomen, wordt de gegevens overgedragen door de Azure Backup-service naar de back-upkluis. De service verzorgt te identificeren en alleen de blokken die zijn gewijzigd sinds de laatste back-up maken van de opslag van de back-ups en het netwerk efficiënt overbrengen. Wanneer de overdracht van gegevens is voltooid, wordt de momentopname is verwijderd en een herstelpunt wordt gemaakt. Dit herstelpunt u kunt bekijken in de klassieke Azure portal.

> [!NOTE]
> Voor virtuele Linux-machines is alleen bestandsconsistente back-ups het mogelijk.
>
>

## <a name="prerequisites"></a>Vereisten
Azure back-up naar back-up van DPM-gegevens als volgt voorbereiden:

1. **Maak een back-upkluis**. Als u een back-upkluis hebt gemaakt in uw abonnement, ziet u de Azure portal-versie van dit artikel - [voorbereiden op back-up van werkbelastingen naar Azure met DPM](backup-azure-dpm-introduction.md).

  > [!IMPORTANT]
  > Vanaf maart 2017 is het niet meer mogelijk om de klassieke portal te gebruiken voor het maken van back-upkluizen.
  > U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Na 30 November 2017 zich u niet PowerShell gebruiken voor het maken van de Backup-kluizen. **Per 30 November 2017**:
  >- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
  >- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.
  >

2. **Kluisreferenties downloaden** — In Azure Backup uploadt u het beheercertificaat dat u hebt gemaakt naar de kluis.
3. **De Azure Backup-Agent installeren en registreren van de server** : van Azure Backup agent te installeren op elke DPM-server en de DPM-server registreren in de back-upkluis.

[!INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[!INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## <a name="requirements-and-limitations"></a>Vereisten (en beperkingen)
* DPM kan worden uitgevoerd als een fysieke server of een Hyper-V virtuele machine is geïnstalleerd op System Center 2012 SP1 of System Center 2012 R2. Kan ook worden uitgevoerd als een virtuele machine van Azure met System Center 2012 R2 met ten minste DPM 2012 R2 updatepakket 3 of een virtuele Windows-machine in VMWare uitgevoerd op System Center 2012 R2 met ten minste updatepakket 5.
* Als u DPM met System Center 2012 SP1 uitvoert, moet u de Update Rollup 2 voor System Center Data Protection Manager SP1 installeren. Dit is vereist voordat u de Azure Backup Agent kunt installeren.
* De DPM-server moet Windows PowerShell en .net Framework 4.5 geïnstalleerd.
* DPM kan back-up meeste workloads naar Azure Backup. De Azure Backup ondersteuning voor een volledige lijst met wat is er ondersteund Zie onderstaande items.
* Gegevens die zijn opgeslagen in Azure Backup kunnen niet worden hersteld met de optie 'kopiëren naar tape'.
* U moet een Azure-account met de Azure Backup-functie ingeschakeld. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Meer informatie over [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
* Azure Backup, moet de Azure Backup-Agent moet worden geïnstalleerd op de servers die u back wilt-up. Elke server moet ten minste 10% van de grootte van de gegevens die worden back-up, beschikbaar als lokale vrije opslagruimte hebben. Bijvoorbeeld, vereist een back-up 100 GB aan gegevens een minimum van 10 GB aan vrije ruimte op de nieuwe locatie. Het minimum is 10%, wordt 15% van de lokale opslagruimte vrij moet worden gebruikt voor de locatie van de cache aanbevolen.
* Gegevens worden opgeslagen in de kluis van Azure-opslag. Er is geen limiet voor de hoeveelheid gegevens die u kunt back-up naar een Azure Backup-kluis, maar de grootte van een gegevensbron (bijvoorbeeld een virtuele machine of een database) mag niet groter zijn dan 54,400 GB.

Deze bestandstypen worden ondersteund voor back-up naar Azure:

* Versleuteld (volledige back-ups alleen)
* Gecomprimeerd (incrementele back-ups ondersteund)
* Sparse (incrementele back-ups ondersteund)
* Gecomprimeerd en sparse (behandeld als Sparse)

En deze worden niet ondersteund:

* Servers op hoofdlettergevoelige bestandssystemen worden niet ondersteund.
* Vaste koppelingen (overgeslagen)
* Reparsepunten (overgeslagen)
* Versleuteld en gecomprimeerd (overgeslagen)
* Versleuteld en sparse (overgeslagen)
* Gecomprimeerde stream
* Sparse stream

> [!NOTE]
> Van in System Center 2012 DPM met SP1 en hoger, u kunt back-up van de werkbelasting die zijn beveiligd door DPM naar Azure met Microsoft Azure Backup.
>
>
