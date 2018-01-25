---
title: Azure Disk Encryption Veelgestelde vragen | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over Microsoft Azure Disk Encryption for Windows en Linux IaaS VM's.
services: security
documentationcenter: na
author: DevTiw
manager: avibm
editor: barclayn
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: devtiw;ejarvi;mayank88mahajan;vermashi;sudhakarareddyevuri;aravindthoram
ms.openlocfilehash: 1144533cd64e80ed2b15c70732c94e332ea06c3d
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-disk-encryption-faq"></a>Azure Disk Encryption Veelgestelde vragen

In dit artikel vindt u antwoorden op veelgestelde vragen (FAQ) over Azure Disk Encryption for Windows en Linux IaaS VM's. Zie voor meer informatie over deze service [Azure Disk Encryption for Windows en Linux IaaS VM's](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="general-questions"></a>Algemene vragen
**V:** waarbij Azure Disk Encryption is in de algemene beschikbaarheid (GA)?

**A:** Azure Disk Encryption for Windows en Linux IaaS VM's is in het algemeen beschikbaarheid in alle openbare Azure-regio.

**V:** welke gebruiker merkt dat zijn beschikbaar bij Azure Disk Encryption?

**A:** Azure schijf versleuteling GA ondersteunt Azure Resource Manager-sjablonen, Azure PowerShell en Azure CLI. Dit biedt u een grote flexibiliteit. U hebt drie verschillende opties voor schijf-codering inschakelen voor uw IaaS VM's. Zie voor meer informatie over de gebruikerservaring en stapsgewijze instructies die beschikbaar zijn in Azure Disk Encryption implementatiescenario's voor Azure Disk Encryption en ervaringen.

**V:** hoeveel kost Azure Disk Encryption?

**A:** er zijn geen kosten voor het versleutelen van VM-schijven met Azure Disk Encryption.

**V:** welke lagen van de virtuele machine Azure Disk Encryption ondersteunt?

**A:** Azure Disk Encryption is beschikbaar op de prijscategorie standard VM's met inbegrip van [A, D, DS, G, GS en F](https://azure.microsoft.com/pricing/details/virtual-machines/) reeks IaaS VM's. Het is ook beschikbaar voor virtuele machines met premium-opslag. Het is niet beschikbaar op virtuele machines in de basisstaffel.

**V:** wat Linux-distributies biedt ondersteuning voor Azure Disk Encryption?

**A:** Azure Disk Encryption wordt ondersteund op de volgende server Linux-distributies en versies:

| Linux-distributie | Versie | Type van het volume voor de versleuteling wordt ondersteund|
| --- | --- |--- |
| Ubuntu | 16.04-DAILY-LTS | Besturingssysteem en schijf |
| Ubuntu | 14.04.5-DAILY-LTS | Besturingssysteem en schijf |
| RHEL | 7.3 | Besturingssysteem en schijf |
| RHEL | 7.2 | Besturingssysteem en schijf |
| RHEL | 6.8 | Besturingssysteem en schijf |
| RHEL | 6.7 | Gegevensschijf |
| CentOS | 7.3 | Besturingssysteem en schijf |
| CentOS | 7.2n | Besturingssysteem en schijf |
| CentOS | 6.8 | Besturingssysteem en schijf |
| CentOS | 7.1 | Gegevensschijf |
| CentOS | 7.0 | Gegevensschijf |
| CentOS | 6.7 | Gegevensschijf |
| CentOS | 6.6 | Gegevensschijf |
| CentOS | 6.5 | Gegevensschijf |
| openSUSE | 13.2 | Gegevensschijf |
| SLES | 12 SP1 | Gegevensschijf |
| SLES | Prioriteit: 12-SP1 | Gegevensschijf |
| SLES | HPC 12 | Gegevensschijf |
| SLES | Prioriteit: 11-SP4 | Gegevensschijf |
| SLES | 11 SP4 | Gegevensschijf |

**V:** hoe kan ik beginnen met behulp van Azure Disk Encryption?

**A:** aan de slag, lees de [Azure Disk Encryption for Windows en Linux IaaS VM's](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) witboek.

**V:** kan ik de opstart- en gegevens volumes met Azure Disk Encryption coderen?

**A:** Ja, u opstart- en gegevensvolumes voor Windows en Linux IaaS VM's kunt versleutelen. Voor Windows-VM's kunt u de gegevens niet coderen zonder eerst het versleutelen van het volume met het besturingssysteem. U kunt het gegevensvolume voor Linux VM's versleutelen zonder eerst het besturingssysteemvolume versleutelen. Nadat u hebt het besturingssysteemvolume versleuteld voor Linux, wordt uitschakelen van IaaS VM's van Linux-versleuteling op een volume met het besturingssysteem niet ondersteund.

**V:** biedt Azure Disk Encryption kunt u doen om uw eigen mogelijkheid key (BYOK)?

**A:** Ja, kunt u uw eigen sleutel versleutelingssleutels opgeven. Deze sleutels worden beveiligd in Azure Sleutelkluis, die de sleutelarchief voor Azure Disk Encryption is. Ondersteuning voor scenario's voor meer informatie over de belangrijkste versleutelingssleutels, Zie implementatiescenario's voor Azure Disk Encryption en ervaringen.

**V:** kan ik een Azure gemaakt sleutel versleutelingssleutel gebruiken?

**A:** Ja, u kunt Azure Sleutelkluis een sleutelcodering sleutel genereren voor Azure disk encryption gebruiken. Deze sleutels worden beveiligd in Azure Sleutelkluis, die de sleutelarchief voor Azure Disk Encryption is. Zie voor meer informatie over de sleutelcodering belangrijke scenario's, implementatiescenario's voor Azure Disk Encryption en ervaringen.

**V:** kan ik gebruiken een lokale Sleutelbeheerservice of HSM ter bescherming van de versleutelingssleutels?

**A:** u de service voor sleutelbeheer lokale of HSM ter bescherming van de versleutelingssleutels met Azure Disk Encryption niet gebruiken. U kunt alleen de Azure Sleutelkluis-service gebruiken ter bescherming van de versleutelingssleutels. Zie voor meer informatie over de sleutelcodering belangrijke scenario's, implementatiescenario's voor Azure Disk Encryption en ervaringen.

**V:** wat zijn de vereisten voor Azure Disk Encryption configureren?

**A:** er is een vereiste PowerShell-script. Met dit script kunt u een Azure Active Directory-toepassing maken, een nieuwe sleutelkluis maakt of instellen van een bestaande sleutelkluis voor toegang tot de schijf versleuteling inschakelen van versleuteling en beveiliging geheimen en sleutels. Zie voor meer informatie over de sleutelcodering belangrijke scenario's vereisten voor Azure Disk Encryption en implementatiescenario's en oplossingen.

**V:** waar vind ik meer informatie over het gebruik van PowerShell voor Azure Disk Encryption configureren?

**A:** hebben We sommige geweldige artikelen over hoe u Azure Disk Encryption basistaken, evenals meer geavanceerde scenario's kunt uitvoeren. Zie voor de basistaken [Azure Disk Encryption verkennen met Azure PowerShell – Part 1](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Zie voor meer geavanceerde scenario's, [Azure Disk Encryption verkennen met Azure PowerShell – deel 2](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

**V:** welke versie van Azure PowerShell Azure Disk Encryption ondersteunt?

**A:** de meest recente versie van de SDK van Azure PowerShell gebruiken voor het configureren van Azure Disk Encryption. Download de nieuwste versie van [Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure Disk Encryption is *niet* ondersteund door Azure SDK-versie 1.1.0.

> [!NOTE]
> De extensie Linux Azure schijf versleuteling preview is afgeschaft. Zie voor meer informatie [bestandstypen Azure schijf versleuteling preview-extensie voor IaaS virtuele Linux-machines](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/).

**V:** kan ik Azure Disk Encryption toepassen op mijn aangepaste Linux-installatiekopie?

**A:** u Azure Disk Encryption kan toepassen op uw aangepaste Linux-installatiekopie. Wij ondersteunen alleen de Linux-afbeeldingen voor de ondersteunde distributies eerder genoemd. Wij ondersteunen momenteel geen aangepaste Linux-installatiekopieën.

**V:** kunt ik updates toepassen op een Linux Red Hat VM die gebruikmaakt van de update yum?

**A:** Ja, u kunt een update uitvoert, of een Red Hat Linux VM patch. Zie voor meer informatie [updates toepassen op een versleutelde Azure IaaS Red Hat virtuele machine met behulp van de update yum](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/).

**V:** wat is de aanbevolen Azure disk encryption werkstroom voor Linux?

**A:** de volgende werkstroom wordt aanbevolen om de beste resultaten voor Linux:
* Start vanuit de ongewijzigd voorraad afbeelding overeenkomt met de gewenste OS distro en versie
* Back-up van gekoppelde stations die worden versleuteld.  Hierdoor kan herstellen in geval van storing, bijvoorbeeld als de virtuele machine opnieuw wordt opgestart voordat de codering is voltooid.
* Versleutelen (kan duren meerdere uren of zelfs dagen afhankelijk van de vm-kenmerken en de grootte van alle schijven bijgesloten gegevens)
* Aanpassen en software installeren op de installatiekopie van het naar behoefte.

Als deze werkstroom niet mogelijk is, vertrouwen op [Opslagversleuteling Service](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) (SSE) op de platform-opslag mogelijk account laag een alternatief voor het volledige schijfversleuteling dm-crypt met.

**V:** wat is de schijf 'Bek Volume' of '/ mnt/azure_bek_disk'?

**A:** 'Bek volume' voor Windows of '/ mnt/azure_bek_disk' voor Linux is een lokale gegevensvolume dat de versleutelingssleutels veilig heeft opgeslagen voor versleuteld virtuele Azure IaaS-machines.
> [!NOTE]
> Verwijder of bewerken van alle inhoud van deze schijf niet. De schijf niet worden ontkoppeld omdat de belangrijkste aanwezigheid versleuteling is vereist voor alle versleutelingsbewerkingen op de IaaS-VM.

**V:** waar kan ik heen te vragen of feedback geven?

**A:** kunt u vragen hebt of feedback geven over de [Azure Disk Encryption forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd meer informatie over de meest voorkomende vragen met betrekking tot Azure Disk Encryption. Zie voor meer informatie over deze service en de mogelijkheden ervan, de volgende artikelen:

- [Schijfversleuteling in Azure Security Center toepassen](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Een virtuele machine van Azure versleutelen](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Azure gegevensversleuteling in rust](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
