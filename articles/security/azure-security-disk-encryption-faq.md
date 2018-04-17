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
ms.date: 03/18/2018
ms.author: devtiw
ms.openlocfilehash: cb523b4fbf6e8abdf5c5158ab041d3485add9b23
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="azure-disk-encryption-faq"></a>Azure Disk Encryption Veelgestelde vragen

In dit artikel vindt u antwoorden op veelgestelde vragen (FAQ) over Azure Disk Encryption for Windows en Linux IaaS VM's. Zie voor meer informatie over deze service [Azure Disk Encryption for Windows en Linux IaaS VM's](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="where-is-azure-disk-encryption-in-general-availability-ga"></a>Waar bevindt zich Azure Disk Encryption in algemene beschikbaarheid (GA)?

Azure Disk Encryption for Windows en Linux IaaS VM's is in het algemeen beschikbaarheid in alle openbare Azure-regio.

## <a name="what-user-experiences-are-available-with-azure-disk-encryption"></a>Welke gebruiker merkt dat zijn beschikbaar bij Azure Disk Encryption?

Azure schijf versleuteling GA biedt ondersteuning voor Azure Resource Manager-sjablonen, Azure PowerShell en Azure CLI. Dit biedt u een grote flexibiliteit. U hebt drie verschillende opties voor schijf-codering inschakelen voor uw IaaS VM's. Zie voor meer informatie over de gebruikerservaring en stapsgewijze instructies die beschikbaar zijn in Azure Disk Encryption [implementatiescenario's voor Azure Disk Encryption en ervaringen](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="how-much-does-azure-disk-encryption-cost"></a>Wat kost Azure Disk Encryption?

Er zijn geen kosten voor het versleutelen van VM-schijven met Azure Disk Encryption.

## <a name="which-virtual-machine-tiers-does-azure-disk-encryption-support"></a>Welke lagen van de virtuele machine biedt ondersteuning voor Azure Disk Encryption?

Azure Disk Encryption is beschikbaar op de prijscategorie standard VM's met inbegrip van [A, D, DS, G, GS en F](https://azure.microsoft.com/pricing/details/virtual-machines/) reeks IaaS VM's. Het is ook beschikbaar voor virtuele machines met premium-opslag. Het is niet beschikbaar op virtuele machines in de basisstaffel.

## <a name="what-linux-distributions-does-azure-disk-encryption-support"></a>Welke Linux-distributies biedt ondersteuning voor Azure Disk Encryption?

Azure Disk Encryption wordt ondersteund door de volgende server Linux-distributies en versies:

| Linux-distributie | Versie | Type van het volume voor de versleuteling wordt ondersteund|
| --- | --- |--- |
| Ubuntu | 16.04-DAILY-LTS | Besturingssysteem en schijf |
| Ubuntu | 14.04.5-DAILY-LTS | Besturingssysteem en schijf |
| RHEL | 7.4 | Gegevens schijf * |
| RHEL | 7.3 | Gegevens schijf * |
| RHEL | 7.2 | Gegevens schijf * |
| RHEL | 6.8 | Gegevens schijf * |
| RHEL | 6.7 | Gegevens schijf * |
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

*__ADE wordt ondersteund voor RHEL voor gegevensschijf. De huidige implementatie van ADE werkt voor de besturingssysteemschijf maar gezamenlijk momenteel niet wordt ondersteund. Microsoft en Red Hat werken aan een gezamenlijk ondersteunde oplossing. In de tussentijd kunt u verwijzen naar de whitepaper ADE voor Linux-besturingssysteem schijfversleuteling [hier](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption).__

## <a name="how-can-i-start-using-azure-disk-encryption"></a>Hoe kan ik beginnen met behulp van Azure Disk Encryption?

Aan de slag, lees de [Azure Disk Encryption for Windows en Linux IaaS VM's](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) witboek.

## <a name="can-i-encrypt-both-boot-and-data-volumes-with-azure-disk-encryption"></a>Kan ik de opstart- en gegevens volumes met Azure Disk Encryption coderen?

Ja, kunt u de opstart- en gegevensvolumes coderen voor Windows en Linux IaaS VM's. Voor Windows-VM's kunt u de gegevens niet coderen zonder eerst het versleutelen van het volume met het besturingssysteem. U kunt het gegevensvolume voor Linux VM's versleutelen zonder eerst het besturingssysteemvolume versleutelen. Nadat u hebt het besturingssysteemvolume versleuteld voor Linux, wordt uitschakelen van IaaS VM's van Linux-versleuteling op een volume met het besturingssysteem niet ondersteund.

## <a name="does-azure-disk-encryption-allow-you-to-bring-your-own-key-byok-capability"></a>Azure Disk Encryption in staat om de functionaliteit van uw eigen key (BYOK)?

Ja, kunt u uw eigen sleutel versleutelingssleutels opgeven. Deze sleutels worden beveiligd in Azure Sleutelkluis, die de sleutelarchief voor Azure Disk Encryption is. Voor meer informatie over de belangrijkste versleutelingssleutels scenario's ondersteunen, Zie [implementatiescenario's voor Azure Disk Encryption en ervaringen](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="can-i-use-an-azure-created-key-encryption-key"></a>Kan ik een Azure gemaakt sleutel versleutelingssleutel gebruiken?

Ja, kunt u Azure Key Vault voor het genereren van een sleutel coderingssleutel voor Azure disk encryption gebruik. Deze sleutels worden beveiligd in Azure Sleutelkluis, die de sleutelarchief voor Azure Disk Encryption is. Zie voor meer informatie over de sleutelcodering belangrijke scenario's [implementatiescenario's voor Azure Disk Encryption en ervaringen](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="can-i-use-an-on-premises-key-management-service-or-hsm-to-safeguard-the-encryption-keys"></a>Kan ik een service voor sleutelbeheer lokale of een HSM gebruiken ter bescherming van de versleutelingssleutels?

U kunt de service voor sleutelbeheer lokale of HSM gebruiken ter bescherming van de versleutelingssleutels met Azure Disk Encryption. U kunt alleen de Azure Sleutelkluis-service gebruiken ter bescherming van de versleutelingssleutels. Zie voor meer informatie over de sleutelcodering belangrijke scenario's [implementatiescenario's voor Azure Disk Encryption en ervaringen](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption"></a>Wat zijn de vereisten voor Azure Disk Encryption configureren?

Er is een vereiste PowerShell-script. Met dit script kunt u een Azure Active Directory-toepassing maken, een nieuwe sleutelkluis maakt of instellen van een bestaande sleutelkluis voor toegang tot de schijf versleuteling inschakelen van versleuteling en beveiliging geheimen en sleutels. Zie voor meer informatie over de sleutelcodering belangrijke scenario's [vereisten voor Azure Disk Encryption en implementatiescenario's en ervaringen](azure-security-disk-encryption.md#prerequisites).

## <a name="where-can-i-get-more-information-on-how-to-use-powershell-for-configuring-azure-disk-encryption"></a>Waar vind ik meer informatie over het gebruik van PowerShell voor Azure Disk Encryption configureren

We hebben enkele grote artikelen op hoe u Azure Disk Encryption basistaken, evenals meer geavanceerde scenario's kunt uitvoeren. Zie voor de basistaken [Azure Disk Encryption verkennen met Azure PowerShell – Part 1](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Zie voor meer geavanceerde scenario's, [Azure Disk Encryption verkennen met Azure PowerShell – deel 2](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

## <a name="what-version-of-azure-powershell-does-azure-disk-encryption-support"></a>Welke versie van Azure PowerShell biedt ondersteuning voor Azure Disk Encryption?

Gebruik de nieuwste versie van de Azure PowerShell SDK Azure Disk Encryption te configureren. Download de nieuwste versie van [Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure Disk Encryption is *niet* ondersteund door Azure SDK-versie 1.1.0.

> [!NOTE]
> De extensie Linux Azure schijf versleuteling preview is afgeschaft. Zie voor meer informatie [bestandstypen Azure schijf versleuteling preview-extensie voor IaaS virtuele Linux-machines](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/).

## <a name="can-i-apply-azure-disk-encryption-on-my-custom-linux-image"></a>Kan ik Azure Disk Encryption toepassen op mijn aangepaste Linux-installatiekopie?

U kunt Azure Disk Encryption niet toepassen op uw aangepaste Linux-installatiekopie. Wij ondersteunen alleen de Linux-afbeeldingen voor de ondersteunde distributies eerder genoemd. Wij ondersteunen momenteel geen aangepaste Linux-installatiekopieën.

## <a name="can-i-apply-updates-to-a-linux-red-hat-vm-that-uses-the-yum-update"></a>Kan ik updates toepassen op een Linux Red Hat VM die gebruikmaakt van de update yum

Ja, kunt u een update uitvoert, of een Red Hat Linux VM patch. Zie voor meer informatie [updates toepassen op een versleutelde Azure IaaS Red Hat virtuele machine met behulp van de update yum](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/).

## <a name="what-is-the-recommended-azure-disk-encryption-workflow-for-linux"></a>Wat is de aanbevolen Azure disk encryption werkstroom voor Linux?

De volgende werkstroom wordt aanbevolen om de beste resultaten voor Linux:
* Start vanuit de ongewijzigd voorraad afbeelding overeenkomt met de gewenste OS distro en versie
* Back-up van gekoppelde stations die worden versleuteld.  Hierdoor kan herstellen in geval van storing, bijvoorbeeld als de virtuele machine opnieuw wordt opgestart voordat de codering is voltooid.
* Versleutelen (kan duren meerdere uren of zelfs dagen afhankelijk van de vm-kenmerken en de grootte van alle schijven bijgesloten gegevens)
* Aanpassen en software installeren op de installatiekopie van het naar behoefte.

Als deze werkstroom niet mogelijk is, vertrouwen op [Opslagversleuteling Service](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) (SSE) op de platform-opslag mogelijk account laag een alternatief voor het volledige schijfversleuteling dm-crypt met.

## <a name="what-is-the-disk-bek-volume-or-mntazurebekdisk"></a>Wat is de schijf 'Bek Volume' of '/ mnt/azure_bek_disk'?

'Bek volume' voor Windows of '/ mnt/azure_bek_disk' voor Linux is een lokale gegevensvolume op dat de versleutelingssleutels veilig heeft opgeslagen voor versleuteld virtuele Azure IaaS-machines.
> [!NOTE]
> Verwijder of bewerken van alle inhoud van deze schijf niet. De schijf niet worden ontkoppeld omdat de belangrijkste aanwezigheid versleuteling is vereist voor alle versleutelingsbewerkingen op de IaaS-VM.

## <a name="where-can-i-go-to-ask-questions-or-provide-feedback"></a>Waar kan ik gaan om te vragen of feedback geven?

U kunt vragen of feedback geven over de [Azure Disk Encryption forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd meer informatie over de meest voorkomende vragen met betrekking tot Azure Disk Encryption. Zie voor meer informatie over deze service en de mogelijkheden ervan, de volgende artikelen:

- [Schijfversleuteling in Azure Security Center toepassen](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Een virtuele machine van Azure versleutelen](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Azure gegevensversleuteling in rust](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)