---
title: 2e generatie virtuele machines (preview) in Azure | Microsoft Docs
description: Overzicht van Azure van de 2e generatie virtuele machines
services: virtual-machines-windows
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2019
ms.author: lahugh
ms.openlocfilehash: 1dcc0d3a652ccbf365a18ce734a54dc78515b1a7
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388354"
---
# <a name="generation-2-vms-preview-on-azure"></a>2e generatie virtuele machines (preview) in Azure

> [!IMPORTANT]
> Virtuele machines van generatie 2 zijn momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Ondersteuning voor generatie 2 virtuele machines (VM's) is nu beschikbaar in openbare preview-versie van Azure. U kunt een virtuele machine generatie nadat u deze hebt gemaakt niet wijzigen. Dus, wordt aangeraden dat u de overwegingen bekijken [hier](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) en de informatie op deze pagina voordat u een generatie.

2e generatie virtuele machines ondersteuning voor belangrijke functies die niet worden ondersteund op virtuele machines van generatie 1, zoals: geheugen, de Intel® Software Guard-extensies (SGX) en virtuele permanent geheugen (vPMEM) verhoogd. Virtuele machines van generatie 2 hebben ook enkele functies die nog worden niet ondersteund op Azure. Zie voor meer informatie de [functies en mogelijkheden](#features-and-capabilities) sectie.

Virtuele machines van generatie 2 gebruiken in de plaats van de nieuwe opstarten op basis van UEFI-architectuur van de BIOS gebaseerde architectuur die wordt gebruikt door virtuele machines van generatie 1. Vergeleken met de virtuele machines van generatie 1, kunnen virtuele machines van generatie 2 hebben verbeterd opstart- en tijdnotatie. Zie voor een overzicht van virtuele machines van generatie 2 en sommige van de belangrijkste verschillen tussen de 1e en 2e generatie [zou ik een generatie 1 of 2 virtuele machine in Hyper-V maken?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="generation-2-vm-sizes"></a>Generatie 2 VM-grootten

Virtuele machines van generatie 1 worden ondersteund door alle VM-grootten in Azure. Azure biedt nu ondersteuning voor generatie 2 voor de volgende geselecteerde VM-reeks in openbare preview-versie:

* [Dsv2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv2-series) en [Dsv3-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)
* [Esv3-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#esv3-series)
* [Fsv2-series](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-compute#fsv2-series-1)
* [GS-series](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#gs-series)
* [Ls-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#ls-series) en [Lsv2-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-storage#lsv2-series)
* [Mv2-serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#mv2-series)

## <a name="generation-2-vm-images-in-azure-marketplace"></a>Generatie 2 VM-installatiekopieën in Azure Marketplace

Virtuele machines van generatie 2 ondersteunen de volgende Azure Marketplace-installatiekopieën:

* Windows Server 2019 Datacenter
* Windows Server 2016 Datacenter
* Windows Server 2012 R2 Datacenter
* Windows Server 2012 Datacenter

## <a name="on-premises-vs-azure-generation-2-vms"></a>On-premises vs Azure van de 2e generatie virtuele machines

Azure ondersteunt momenteel geen enkele van de functies die on-premises Hyper-V biedt ondersteuning voor virtuele machines van generatie 2.

| De functie van de 2e generatie                | On-premises Hyper-V | Azure |
|-------------------------------------|---------------------|-------|
| Beveiligd opstarten                         | :heavy_check_mark:  | : x:.   |
| Afgeschermde VM                         | :heavy_check_mark:  | : x:.   |
| vTPM                                | :heavy_check_mark:  | : x:.   |
| Beveiliging op basis van virtualisatie (VBS) | :heavy_check_mark:  | : x:.   |
| VHDX-indeling                         | :heavy_check_mark:  | : x:.   |

## <a name="features-and-capabilities"></a>Functies en mogelijkheden

### <a name="generation-1-vs-generation-2-features"></a>Generatie 1 en generatie 2 functies

| Functie | Generatie 1 | Generatie 2 |
|---------|--------------|--------------|
| Opstarten             | PCAT                      | UEFI                               |
| Schijf-controllers | IDE                       | SCSI                               |
| VM-grootten         | Beschikbaar op alle VM-grootten | Ondersteunde VM's alleen Premium-opslag |

### <a name="generation-1-vs-generation-2-capabilities"></a>Generatie 1 en generatie 2 mogelijkheden

| Mogelijkheid | Generatie 1 | Generatie 2 |
|------------|--------------|--------------|
| OS-schijf > 2 TB                    | : x:.                        | :heavy_check_mark: |
| Aangepaste installatiekopie-schijf/wisselen OS         | :heavy_check_mark:         | :heavy_check_mark: |
| Ondersteuning voor virtuele-machineschaalset | :heavy_check_mark:         | :heavy_check_mark: |
| ASR/Backup                        | :heavy_check_mark:         | : x:.                |
| Galerie met gedeelde installatiekopieën              | :heavy_check_mark:         | : x:.                |
| Azure Disk Encryption             | :heavy_check_mark:         | : x:.                |

## <a name="creating-a-generation-2-vm"></a>Het maken van een van de 2e generatie virtuele machine

### <a name="marketplace-image"></a>Marketplace-installatiekopie

Virtuele machines van generatie 2 kunnen worden gemaakt vanuit een marketplace-installatiekopie (die ondersteuning biedt voor opstarten via UEFI) via Azure portal of Azure CLI.

De `windowsserver-gen2preview` aanbieding bevat alleen installatiekopieën van Windows van de 2e generatie. Hiermee voorkomt u verwarring met betrekking tot installatiekopieën van generatie 1 vs van de 2e generatie. Selecteer voor het maken van generatie 2 virtuele machines, **installatiekopieën** van deze aanbieding en volgen het standaardproces voor het maken van virtuele machine.

Op dit moment de volgende Windows-generatie 2-installatiekopieën worden gepubliceerd in de Azure Marketplace:

* 2019-datacenter-gen2
* 2016-datacenter-gen2
* 2012-r2-datacenter-gen2
* 2012-datacenter-gen2

Zie de sectie mogelijkheden voor een lijst van ondersteunde marketplace-installatiekopieën, blijven we extra afbeeldingen die ondersteuning bieden voor generatie 2 toe te voegen.

### <a name="managed-image-or-managed-disk"></a>Beheerde installatiekopie of beheerde schijf

Generatie 2 virtuele machines kunnen worden gemaakt van beheerde installatiekopie of een beheerde schijf op dezelfde manier u een generatie 1 maakt VM.

### <a name="virtual-machine-scale-sets"></a>Virtuele-machineschaalsets

Generatie die 2 virtuele machines kunnen ook worden gemaakt met behulp van schaalsets voor virtuele machines. U kunt generatie 2 virtuele machines met behulp van schaalsets voor virtuele Azure-machine via de Azure CLI maken.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

* **Generatie 2 virtuele machines beschikbaar in alle Azure-regio's zijn?**  
    Ja. echter niet alle [generation 2 VM-grootten](#generation-2-vm-sizes) beschikbaar zijn in elke regio. De beschikbaarheid van generatie 2 virtuele machines is afhankelijk van de beschikbaarheid van de VM-grootte.

* **Is er een prijsverschil tussen generatie 1 en generatie 2 virtuele machines?**  
    Er is geen verschil in de prijzen van de 1e en 2e generatie virtuele machines.

* **Hoe ik de grootte van de Besturingssysteemschijf vergroten?**  
  Besturingssysteemschijven groter dan 2 TB niet bekend bent met de 2e generatie virtuele machines. De meeste OS-schijven zijn minder dan 2 TB voor virtuele machines van generatie 2, maar de grootte van de schijf kan worden verhoogd tot aanbevolen maximaal 4 TB. U kunt de grootte van de OS-schijf via de Azure CLI of Azure portal kunt verhogen. Zie voor meer informatie over het uitbreiden van schijven via een programma, [vergroten of verkleinen van een schijf](expand-os-disk.md).

  Vergroten van de grootte van de Besturingssysteemschijf via Azure portal:

  * Navigeer naar de eigenschappenpagina van virtuele machine in Azure portal.

  * Afsluiten en toewijzing ongedaan maken van de virtuele machine met behulp de **stoppen** knop op de Azure-portal.

  * In de **schijven** sectie, selecteert u de OS-schijf die u wilt verhogen.

  * Selecteer **configuratie** in de **schijven** sectie en update de **grootte** op de gewenste waarde.

  * Ga terug naar de eigenschappenpagina van de virtuele machine en **Start** de virtuele machine.
  
  Mogelijk ziet u een waarschuwing voor besturingssysteemschijven groter dan 2 TB. De waarschuwing is niet van toepassing op virtuele machines van generatie 2; OS-schijfformaten groter is dan 4 TB zijn echter **niet aanbevolen.**

* **Virtuele machines van generatie 2 ondersteunen versnelde netwerken?**  
    Ja, ondersteuning voor generatie 2 virtuele machines [versnelde netwerken](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **Wordt .vhdx op generatie 2 ondersteund?**  
    Nee, alleen VHD wordt ondersteund op virtuele machines van generatie 2.

* **Bieden ondersteuning voor virtuele machines van generatie 2 Ultra-stations SSD (Solid State)?**  
    Ja, ondersteuning voor virtuele machines van generatie 2 Ultra SSD.

* **Kan ik migreren van generatie 1 op virtuele machines van generatie 2?**  
    U kunt Nee, het genereren van een virtuele machine niet wijzigen nadat u deze hebt gemaakt. Als u schakelen tussen generaties van virtuele machine wilt, moet u een nieuwe virtuele machine van een andere generatie maken.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [2e generatie virtuele machines in Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

* Meer informatie over het [VHD voorbereiden](prepare-for-upload-vhd-image.md) voor het uploaden van on-premises naar Azure.
