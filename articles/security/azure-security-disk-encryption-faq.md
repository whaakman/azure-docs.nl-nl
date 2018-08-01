---
title: Azure Disk Encryption Veelgestelde vragen over | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over Microsoft Azure Disk Encryption voor Windows en Linux IaaS-VM's.
services: security
documentationcenter: na
author: mestew
manager: MBaldwin
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/30/2018
ms.author: mstewart
ms.openlocfilehash: 98b8883a5ab0096102ab7daf90b5b2791a6f7e41
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389572"
---
# <a name="azure-disk-encryption-faq"></a>Azure Disk Encryption Veelgestelde vragen

In dit artikel vindt u antwoorden op veelgestelde vragen (FAQ) over Azure Disk Encryption voor Windows en Linux IaaS-VM's. Zie voor meer informatie over deze service [Azure Disk Encryption voor Windows en Linux IaaS-VM's](azure-security-disk-encryption-overview.md).

## <a name="where-is-azure-disk-encryption-in-general-availability-ga"></a>Waar is Azure Disk Encryption in algemene beschikbaarheid (GA)?

Azure Disk Encryption voor Windows en Linux IaaS-VM's is algemeen beschikbaar in alle Azure openbare regio's.

## <a name="what-user-experiences-are-available-with-azure-disk-encryption"></a>Welke gebruiker ondervindt zijn beschikbaar met Azure Disk Encryption?

Azure Disk Encryption-algemene beschikbaarheid biedt ondersteuning voor Azure Resource Manager-sjablonen, Azure PowerShell en Azure CLI. De meerdere gebruikerservaringen bieden flexibiliteit. U hebt drie verschillende opties voor het inschakelen van schijfversleuteling voor IaaS-VM's. Zie voor meer informatie over de gebruikerservaring en stapsgewijze aanwijzingen die beschikbaar zijn in Azure Disk Encryption [inschakelen Azure Disk Encryption voor Windows](azure-security-disk-encryption-windows.md) en [Azure Disk Encryption inschakelen voor Linux](azure-security-disk-encryption-linux.md).

## <a name="how-much-does-azure-disk-encryption-cost"></a>Wat kost Azure Disk Encryption?

Er zijn geen kosten voor het versleutelen van VM-schijven met Azure Disk Encryption maar er zijn kosten die zijn gekoppeld aan het gebruik van Azure Key Vault. Zie voor meer informatie over de kosten voor Azure Key Vault, de [prijzen voor Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) pagina.


## <a name="which-virtual-machine-tiers-does-azure-disk-encryption-support"></a>Welke Prijscategorieën voor virtuele machines biedt ondersteuning voor Azure Disk Encryption?

Azure Disk Encryption is beschikbaar op standard-laag virtuele machines met inbegrip van [A, D, DS, G, GS en F](https://azure.microsoft.com/pricing/details/virtual-machines/) reeks IaaS-VM's. Het is ook beschikbaar voor virtuele machines met premium storage. Het is niet beschikbaar in basic-laag virtuele machines.

## <a name="bkmk_LinuxOSSupport"></a> Wat Linux-distributies biedt ondersteuning voor Azure Disk Encryption?

Azure Disk Encryption wordt ondersteund op de volgende server Linux-distributies en versies:

| Linux-distributie | Versie | Volumetype wordt ondersteund voor versleuteling|
| --- | --- |--- |
| Ubuntu | 16.04-DAILY-LTS | Besturingssysteem- en schijf |
| Ubuntu | 14.04.5-DAILY-LTS | Besturingssysteem- en schijf |
| RHEL | 7.5 | Gegevens schijf * |
| RHEL | 7.4 | Gegevens schijf * |
| RHEL | 7.3 | Gegevens schijf * |
| RHEL | 7.2 | Gegevens schijf * |
| RHEL | 6.8 | Gegevens schijf * |
| RHEL | 6.7 | Gegevens schijf * |
| CentOS | 7.4 | Besturingssysteem- en schijf |
| CentOS | 7.3 | Besturingssysteem- en schijf |
| CentOS | 7.2n | Besturingssysteem- en schijf |
| CentOS | 6.8 | Besturingssysteem- en schijf |
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


*__ADE wordt ondersteund voor RHEL voor de gegevensschijf. De huidige ADE-implementatie voor de besturingssysteemschijf werkt wel, maar momenteel gezamenlijk niet wordt ondersteund. Zowel Microsoft en Red Hat werken aan een gezamenlijk ondersteunde oplossing. In de tussentijd kunt u verwijzen naar de [Azure Disk Encryption voor Linux](azure-security-disk-encryption-linux.md) artikel.__

## <a name="how-can-i-start-using-azure-disk-encryption"></a>Hoe kan ik beginnen met Azure Disk Encryption?

Aan de slag, lees de [overzicht van Azure Disk Encryption](azure-security-disk-encryption-overview.md).

## <a name="can-i-encrypt-both-boot-and-data-volumes-with-azure-disk-encryption"></a>Kan ik volumes van zowel de opstart- en de gegevens met Azure Disk Encryption coderen?

Ja, kunt u de opstart- en volumes voor Windows en Linux IaaS-machines versleutelen. Voor Windows-VM's, kunt u de gegevens niet coderen zonder eerst het versleutelen van het volume met het besturingssysteem. Voor virtuele Linux-machines is het mogelijk voor het versleutelen van het gegevensvolume zonder te coderen volume met het besturingssysteem eerst. Nadat u hebt het volume met het besturingssysteem versleuteld voor Linux, wordt uitschakelen van versleuteling op een volume met het besturingssysteem voor Linux IaaS-VM's niet ondersteund.

## <a name="does-azure-disk-encryption-allow-you-to-bring-your-own-key-byok-capability"></a>Azure Disk Encryption bent u in staat om de mogelijkheden van uw eigen sleutel (BYOK)?

Ja, kunt u uw eigen sleutels key-versleuteling opgeven. Deze sleutels worden beveiligd in Azure Key Vault, die het sleutelarchief voor Azure Disk Encryption is. Voor meer informatie over de belangrijkste versleutelingssleutels scenario's ondersteunen, Zie [vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md).

## <a name="can-i-use-an-azure-created-key-encryption-key"></a>Kan ik een sleutel gemaakt van Azure key-versleuteling gebruiken?

Ja, u kunt Azure Key Vault gebruiken voor het genereren van een sleutel van versleutelingssleutel voor Azure disk encryption gebruik. Deze sleutels worden beveiligd in Azure Key Vault, die het sleutelarchief voor Azure Disk Encryption is. Zie voor meer informatie over de sleutel van versleutelingssleutel [vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md).

## <a name="can-i-use-an-on-premises-key-management-service-or-hsm-to-safeguard-the-encryption-keys"></a>Kan ik een on-premises-service voor sleutelbeheer of een HSM gebruiken ter bescherming van de versleutelingssleutels?

U kunt de service voor sleutelbeheer on-premises of de HSM niet gebruiken ter bescherming van de versleutelingssleutels met Azure Disk Encryption. U kunt alleen de Azure Key Vault-service gebruiken ter bescherming van de versleutelingssleutels. Zie voor meer informatie over de key-versleuteling ondersteuning voor belangrijke scenario's [vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md).

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption"></a>Wat zijn de vereisten voor het configureren van Azure Disk Encryption?

Er zijn vereisten voor Azure Disk Encryption. Zie de [vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) artikel te maken van een Azure Active Directory-toepassing, een nieuwe sleutelkluis maken of instellen van een bestaande sleutelkluis voor toegang tot de schijf versleuteling versleuteling inschakelen en geheimen beveiligen en sleutels. Zie voor meer informatie over de key-versleuteling ondersteuning voor belangrijke scenario's [overzicht van Azure Disk Encryption](azure-security-disk-encryption-overview.md).

## <a name="where-can-i-get-more-information-on-how-to-use-powershell-for-configuring-azure-disk-encryption"></a>Waar vind ik meer informatie over het gebruik van PowerShell voor het configureren van Azure Disk Encryption

Er zijn enkele geweldige artikelen over hoe u Azure Disk Encryption basistaken, evenals de meer geavanceerde scenario's kunt uitvoeren. Zie voor de basistaken [Azure Disk Encryption verkennen met Azure PowerShell-deel 1](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Zie voor meer geavanceerde scenario's, [Azure Disk Encryption verkennen met Azure PowerShell-deel 2](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

## <a name="what-version-of-azure-powershell-does-azure-disk-encryption-support"></a>Welke versie van Azure PowerShell biedt ondersteuning voor Azure Disk Encryption?

De meest recente versie van de SDK van Azure PowerShell gebruiken om te configureren van Azure Disk Encryption. Download de nieuwste versie van [Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure Disk Encryption is *niet* ondersteund door Azure SDK versie 1.1.0.

> [!NOTE]
> De Linux Azure disk encryption-uitbreiding met Preview-versie is afgeschaft. Zie voor meer informatie, [Azure niet meer ondersteund schijf versleuteling preview-extensie voor Linux IaaS-VM's](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/).

## <a name="can-i-apply-azure-disk-encryption-on-my-custom-linux-image"></a>Kan ik Azure Disk Encryption toepassen op mijn aangepaste installatiekopie van Linux?

U kunt geen Azure Disk Encryption toepassen op uw aangepaste installatiekopie van Linux. Alleen de galerie met Linux-installatiekopieën voor de ondersteunde distributies eerder genoemd, worden ondersteund. Aangepaste Linux-installatiekopieën worden momenteel niet ondersteund.

## <a name="can-i-apply-updates-to-a-linux-red-hat-vm-that-uses-the-yum-update"></a>Kan ik updates toepassen op een Linux Red Hat virtuele machine die gebruikmaakt van de update yum?

Ja, kunt u een update uitvoert, of te patchen van een Red Hat Linux-VM. Zie voor meer informatie, [toepassen van updates op een versleutelde virtuele Azure IaaS Red Hat-machine met behulp van de update yum](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/).

## <a name="what-is-the-recommended-azure-disk-encryption-workflow-for-linux"></a>Wat is de aanbevolen Azure disk encryption-werkstroom voor Linux?

De volgende werkstroom wordt aanbevolen om de beste resultaten op Linux:
* Starten vanuit de ongewijzigd aandelen galerijafbeelding overeenkomt met de gewenste OS-distributie en -versie
* Back-up van gekoppelde stations die worden versleuteld.  Hierdoor kan herstellen als er een storing optreedt, bijvoorbeeld als de virtuele machine opnieuw wordt opgestart voordat de codering is voltooid.
* Versleutelen (kan duren meerdere uren of zelfs dagen afhankelijk van de kenmerken van de virtuele machine en de grootte van alle gekoppelde gegevensschijven)
* Aanpassen en software toevoegen aan de installatiekopie, indien nodig.

Als deze werkstroom niet mogelijk is, afhankelijk [Storage Service Encryption](../storage/common/storage-service-encryption.md) (SSE) op de platform-opslag is mogelijk account laag een alternatief voor volledige schijfversleuteling met behulp van dm-crypt.

## <a name="what-is-the-disk-bek-volume-or-mntazurebekdisk"></a>Wat is de schijf "Bek Volume" of '/ mnt/azure_bek_disk'?

"Bek volume" voor Windows of '/ mnt/azure_bek_disk' voor Linux is een lokale gegevensvolume die veilig de versleutelingssleutels voor versleutelde virtuele Azure IaaS-machines slaat.
> [!NOTE]
> Niet verwijderen of bewerken van alle inhoud van deze schijf. De schijf niet worden ontkoppeld nadat de belangrijkste aanwezigheid codering nodig is voor versleutelingsbewerkingen op de IaaS-VM.

## <a name="where-can-i-go-to-ask-questions-or-provide-feedback"></a>Waar vind ik vragen of feedback geven?

U kunt vragen of feedback geven over de [forum voor Azure Disk Encryption](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u meer informatie over de meest voorkomende vragen met betrekking tot Azure Disk Encryption geleerd. Zie voor meer informatie over deze service en de mogelijkheden ervan, de volgende artikelen:

- [Schijfversleuteling in Azure Security Center toepassen](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Een virtuele machine van Azure versleutelen](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Azure gegevensversleuteling in rust](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
