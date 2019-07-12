---
title: Azure-ondersteuning voor virtuele machines van generatie 2 (preview) | Microsoft Docs
description: Overzicht van Azure-ondersteuning voor virtuele machines van generatie 2
services: virtual-machines-windows
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2019
ms.author: lahugh
ms.openlocfilehash: 7abc1b7cd3be775af03aeaf558f631060cc6f81c
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722725"
---
# <a name="support-for-generation-2-vms-preview-on-azure"></a>Ondersteuning voor generatie 2 virtuele machines (preview) op Azure

> [!IMPORTANT]
> Azure-ondersteuning voor virtuele machines van generatie 2 is momenteel in preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en het wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie voor meer informatie, [aanvullende gebruiksvoorwaarden voor Microsoft Azure previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Ondersteuning voor generatie 2 virtuele machines (VM's) is nu beschikbaar in preview in Azure. U kan wijzigen van een virtuele machine generatie nadat u deze hebt gemaakt, dus de overwegingen met betrekking tot op deze pagina bekijken voordat u een generatie. 

Belangrijke functies die niet worden ondersteund in virtuele machines van generatie 1 ondersteuning voor virtuele machines van generatie 2. Deze functies zijn van meer geheugen, Intel Software Guard-extensies (Intel SGX), en gevirtualiseerde permanent geheugen (vPMEM). Virtuele machines van generatie 2 hebben ook enkele functies die nog in Azure worden niet ondersteund. Zie voor meer informatie de [functies en mogelijkheden](#features-and-capabilities) sectie.

Virtuele machines van generatie 2 gebruiken de nieuwe op basis van UEFI-Opstartarchitectuur in plaats van het BIOS gebaseerde architectuur die worden gebruikt door generatie 1 VM's. Vergeleken met de virtuele machines van generatie 1, kunnen virtuele machines van generatie 2 hebben verbeterd opstart- en tijdnotatie. Zie voor een overzicht van virtuele machines van generatie 2 en enkele van de verschillen tussen 1e en 2e generatie [zou ik een generatie 1 of 2 virtuele machine in Hyper-V maken?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="generation-2-vm-sizes"></a>Generatie 2 VM-grootten

Virtuele machines van generatie 1 worden ondersteund door alle VM-grootten in Azure. Azure biedt nu ondersteuning voor Preview-versie van de 2e generatie voor de volgende geselecteerde VM-reeks:

* [Dsv2-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv2-series) en [Dsv3-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)
* [Esv3-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#esv3-series)
* [Fsv2-series](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-compute#fsv2-series-1)
* [GS-series](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#gs-series)
* [Ls-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#ls-series) en [Lsv2-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-storage#lsv2-series)
* [Mv2-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#mv2-series)

## <a name="generation-2-vm-images-in-azure-marketplace"></a>Generatie 2 VM-installatiekopieën in Azure Marketplace

Virtuele machines van generatie 2 ondersteunen de volgende Marketplace-installatiekopieën:

* Windows Server 2019 Datacenter
* Windows Server 2016 Datacenter
* Windows Server 2012 R2 Datacenter
* Windows Server 2012 Datacenter

## <a name="on-premises-vs-azure-generation-2-vms"></a>On-premises vs. Virtuele machines van Azure generatie 2

Azure ondersteunt momenteel geen enkele van de functies die on-premises Hyper-V biedt ondersteuning voor virtuele machines van generatie 2.

| De functie van de 2e generatie                | On-premises Hyper-V | Azure |
|-------------------------------------|---------------------|-------|
| Beveiligd opstarten                         | :heavy_check_mark:  | : x:.   |
| Afgeschermde VM                         | :heavy_check_mark:  | : x:.   |
| vTPM                                | :heavy_check_mark:  | : x:.   |
| Beveiliging op basis van virtualisatie (VBS) | :heavy_check_mark:  | : x:.   |
| VHDX-indeling                         | :heavy_check_mark:  | : x:.   |

## <a name="features-and-capabilities"></a>Functies en mogelijkheden

### <a name="generation-1-vs-generation-2-features"></a>Generatie 1 en generatie 2-functies

| Functie | Generatie 1 | Generatie 2 |
|---------|--------------|--------------|
| Opstarten             | PCAT                      | UEFI                               |
| Schijf-controllers | IDE                       | SCSI                               |
| Formaten van virtuele machines         | Alle VM-grootten | Alleen VM's die ondersteuning bieden voor premium-opslag |

### <a name="generation-1-vs-generation-2-capabilities"></a>Generatie 1 en generatie 2-mogelijkheden

| Mogelijkheid | Generatie 1 | Generatie 2 |
|------------|--------------|--------------|
| OS-schijf > 2 TB                    | : x:.                        | :heavy_check_mark: |
| Aangepaste/afbeelding/wisselen van besturingssysteem         | :heavy_check_mark:         | :heavy_check_mark: |
| Ondersteuning voor virtuele-machineschaalset | :heavy_check_mark:         | :heavy_check_mark: |
| ASR/backup                        | :heavy_check_mark:         | : x:.                |
| Galerie met installatiekopieën van gedeelde              | :heavy_check_mark:         | : x:.                |
| Azure disk encryption             | :heavy_check_mark:         | : x:.                |

## <a name="creating-a-generation-2-vm"></a>Het maken van een van de 2e generatie virtuele machine

### <a name="marketplace-image"></a>Marketplace-installatiekopie

In de Azure portal of Azure CLI, kunt u generatie 2 virtuele machines vanuit een Marketplace-installatiekopie die ondersteuning biedt voor opstarten via UEFI.

De `windowsserver-gen2preview` aanbieding bevat alleen installatiekopieën van Windows van de 2e generatie. Deze pakketten voorkomt verwarring tussen 1e en 2e generatie installatiekopieën. Maken van een van de 2e generatie virtuele machine, selecteer **installatiekopieën** van deze aanbieding en volgen het standaardproces voor het maken van de virtuele machine.

Op dit moment biedt Marketplace de volgende Windows-generatie 2-installatiekopieën:

* 2019-datacenter-gen2
* 2016-datacenter-gen2
* 2012-r2-datacenter-gen2
* 2012-datacenter-gen2

Zie de [functies en mogelijkheden](#features-and-capabilities) sectie voor een huidige lijst van ondersteunde Marketplace-installatiekopieën.

### <a name="managed-image-or-managed-disk"></a>Beheerde installatiekopie of beheerde schijf

U kunt een van de 2e generatie maken VM op basis van een beheerde installatiekopie of een beheerde schijf op dezelfde manier u een generatie 1 maakt VM.

### <a name="virtual-machine-scale-sets"></a>Virtuele-machineschaalsets

U kunt ook generatie 2 virtuele machines maken met behulp van schaalsets voor virtuele machines. Gebruik Azure-schaal ingesteld in de Azure-CLI generatie 2 virtuele machines maken.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

* **Generatie 2 virtuele machines beschikbaar in alle Azure-regio's zijn?**  
    Ja. Maar niet alle [generation 2 VM-grootten](#generation-2-vm-sizes) beschikbaar zijn in elke regio. De beschikbaarheid van de generatie die 2 virtuele machine is afhankelijk van de beschikbaarheid van de VM-grootte.

* **Is er een prijsverschil tussen generatie 1 en generatie 2 virtuele machines?**  
   Nee.

* **Hoe ik de grootte van de Besturingssysteemschijf vergroten?**  
  Besturingssysteemschijven groter dan 2 TB niet bekend bent met de 2e generatie virtuele machines. OS-schijven zijn standaard kleiner is dan 2 TB voor virtuele machines van generatie 2. U kunt de schijfgrootte aanbevolen maximaal 4 TB verhogen. De Azure CLI of Azure portal gebruiken voor het verhogen van de grootte van de Besturingssysteemschijf. Zie voor meer informatie over het uitbreiden van schijven programmatisch [vergroten of verkleinen van een schijf](expand-os-disk.md).

  Vergroten van de grootte van de OS-schijf van de Azure-portal:

  1. In de Azure-portal, gaat u naar de eigenschappenpagina van de virtuele machine.
  1. Als u wilt afsluiten en wijs de virtuele machine, selecteer de **stoppen** knop.
  1. In de **schijven** sectie, selecteert u de OS-schijf die u wilt verhogen.
  1. In de **schijven** sectie, selecteer **configuratie**, en werk de **grootte** naar de gewenste waarde.
  1. Ga terug naar de eigenschappenpagina van de virtuele machine en **Start** de virtuele machine.
  
  U ziet mogelijk een waarschuwing voor besturingssysteemschijven groter dan 2 TB. De waarschuwing niet van toepassing op virtuele machines van generatie 2. OS-schijfformaten groter is dan 4 TB zijn echter *niet aanbevolen.*

* **Generatie 2 VM's ondersteunen versnelde netwerken doen?**  
    Ja. Zie voor meer informatie, [maken van een virtuele machine met versneld netwerken](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **Wordt VHDX op generatie 2 ondersteund?**  
    Nee, virtuele machines van generatie 2 ondersteunen alleen VHD.

* **Bieden ondersteuning voor virtuele machines van generatie 2 Ultra Disk-opslag van Azure?**  
    Ja.

* **Kan ik een virtuele machine van generatie 1 migreren naar generatie 2?**  
    U kunt Nee, het genereren van een virtuele machine niet wijzigen nadat u dit hebt gemaakt. Als u schakelen tussen generaties van virtuele machine wilt, maakt u een nieuwe virtuele machine van een andere generatie.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [2e generatie virtuele machines in Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

* Meer informatie over het [VHD voorbereiden](prepare-for-upload-vhd-image.md) voor het uploaden van on-premises systemen naar Azure.
