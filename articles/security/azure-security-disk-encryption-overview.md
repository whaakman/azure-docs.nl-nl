---
title: Wat is Azure Disk Encryption?
description: In dit artikel biedt een overzicht van Azure Disk Encryption
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 06/05/2019
ms.custom: seodec18
ms.openlocfilehash: 58c5c8321e505fe2c1c7d19c58fe0d031b75b3e4
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67294868"
---
# <a name="azure-disk-encryption-overview"></a>Overzicht van Azure Disk Encryption

Azure Disk Encryption helpt beschermen en beveiligen van uw gegevens om te voldoen aan de beveiligings- en nalevingsverplichtingen van de organisatie. Hierbij de [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) functie van Windows en de [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) -functie van Linux om volumeversleuteling voor het besturingssysteem en de gegevensschijven van virtuele Azure-machines (VM's) te bieden. Het is ook geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te bepalen en de sleutels en geheimen, beheren en ervoor zorgt dat alle gegevens op de VM-schijven worden versleuteld in rust in de Azure-opslag. Azure Disk Encryption voor Windows en Linux-machines is algemeen beschikbaar in alle regio's Azure Government voor Standard VM's en virtuele machines met Azure Premium Storage en Azure openbare regio's. 

Als u Azure Security Center gebruikt, bent u gewaarschuwd als er virtuele machines die niet zijn versleuteld. De waarschuwingen worden weergegeven als hoge urgentie en de aanbeveling is voor het versleutelen van deze VM's.

![Azure Security Center schijf versleuteling waarschuwing](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Bepaalde aanbevelingen verhogen gegevens, netwerk, of gebruik van de compute-bronnen en leiden tot extra kosten in licentie of abonnement.


## <a name="encryption-scenarios"></a>Scenario's voor versleuteling

U kunt met Azure Disk Encryption beveiliging van de organisatie en vereisten voor naleving oplossen door het beveiligen van uw virtuele Azure-machines at-rest met behulp van van industriestandaard-versleutelingstechnologie. U kunt ook virtuele machines om op te starten onder de klant beheerde sleutels en beleidsregels (BYOK) configureren en controleren van het gebruik van deze sleutels in uw key vault.

Azure Disk Encryption ondersteunt de volgende klant-scenario's:

* In- en uitschakelen van versleuteling op nieuwe virtuele machines die zijn gemaakt op basis van de ondersteunde Azure-galerie-installatiekopieën.
* In- en uitschakelen van versleuteling op bestaande virtuele machines die worden uitgevoerd in Azure.
* In- en uitschakelen van versleuteling op nieuwe Windows-VM's gemaakt op basis van vooraf gecodeerde VHD- en versleutelingssleutels.
* Inschakelen en uitschakelen van versleuteling op virtuele-machineschaalset in Windows wordt ingesteld.
* -Schijven voor schaalsets voor Linux-machines inschakelen en uitschakelen van versleuteling van gegevens.
* In- en uitschakelen van versleuteling van virtuele machines op beheerde schijven.
* Bijwerken van instellingen voor versleuteling van een bestaande versleutelde Premium en niet - Premium Storage VM.
* Back-up en herstellen van versleutelde virtuele machines.
* Breng uw eigen versleuteling (BYOE) en breng uw eigen sleutel (BYOK) scenario's, waarin de klanten hun eigen sleutels voor versleuteling en op te slaan in een Azure-sleutelkluis.

Het ondersteunt ook de volgende scenario's voor virtuele machines wanneer deze zijn ingeschakeld in Microsoft Azure:

* Integratie met Azure Key Vault.
* [Standard-laag virtuele machines](https://azure.microsoft.com/pricing/details/virtual-machines/) die voldoen aan de [minimale geheugenvereiste](azure-security-disk-encryption-prerequisites.md#supported-vm-sizes). 
* Virtuele machines inschakelen van versleuteling op Windows en Linux-VM's, beheerde schijf en schaal worden ingesteld via de ondersteunde Azure-galerie-installatiekopieën.
* Uitschakelen van versleuteling op besturingssysteem en schijven voor virtuele machines van Windows, schaal virtuele machines en virtuele machines op schijven beheerd.
* Uitschakelen versleuteling op gegevensstations voor Linux-VM's, schaal virtuele machines en virtuele machines op schijven beheerd.
* Inschakelen van versleuteling op VM's waarop het Windows Client-besturingssysteem wordt uitgevoerd.
* Versleuteling voor volumes met koppelpunt paden is ingeschakeld.
* Versleuteling op Linux-VM's die zijn geconfigureerd met de schijf (RAID) met behulp van mdadm striping is ingeschakeld.
* Inschakelen van versleuteling op Linux-VM's die gebruikmaken van LVM voor gegevensschijven.
* Inschakelen van versleuteling op de Linux VM-besturingssysteem en gegevensschijven.

   > [!NOTE]
   > OS-stationsversleuteling voor bepaalde Linux-distributies wordt niet ondersteund. Zie voor meer informatie de [Azure Disk Encryption ondersteunde besturingssystemen: Linux](azure-security-disk-encryption-prerequisites.md#linux).
   
* Inschakelen van versleuteling op virtuele machines die zijn geconfigureerd met Windows Storage Spaces begin in Windows Server 2016.
* Back-up en herstel van versleutelde virtuele machines voor zowel de sleutel van versleutelingssleutel (KEK-sleutel) als de niet-KEK-scenario's.

Azure Disk Encryption werkt niet voor de volgende scenario's, functies en technologieën voor:

* Basic-laag virtuele machine of virtuele machines die zijn gemaakt via de klassieke methode voor het maken van de virtuele machine versleutelen.
* Het uitschakelen van versleuteling op een station van het besturingssysteem of gegevensstation van een Linux-VM als de besturingssysteemschijf is gecodeerd.
* Versleutelen van de besturingssysteemschijf voor virtuele-machineschaalset in Linux wordt ingesteld.
* Windows-VM's geconfigureerd met software gebaseerde RAID-systemen te coderen.
* Aangepaste installatiekopieën voor virtuele Linux-machines versleutelen.
* Integratie met een on-premises sleutelbeheersysteem.
* Azure-bestanden (gedeelde bestandssysteem).
* Network File System (NFS).
* Dynamische volumes.

## <a name="encryption-features"></a>Versleutelingsfuncties

Als u inschakelt en implementeren van Azure Disk Encryption voor Azure VM's, kunt u de volgende mogelijkheden om te worden ingeschakeld:

* Versleutelen van het volume met het besturingssysteem ter bescherming van het opstartvolume at-rest in uw opslag.
* Versleutelen gegevensvolumes om de gegevensvolumes in rust in uw opslag te beveiligen.
* Het uitschakelen van versleuteling op de OS- en gegevensstations voor Windows-VM's.
* Uitschakelen van versleuteling op de gegevens schijven voor virtuele Linux-machines (alleen als de besturingssysteemschijf wordt niet versleuteld).
* Bescherming van de versleutelingssleutels en geheimen in uw Azure Key Vault-abonnement.
* Rapporteert de status voor schijfversleuteling van de versleutelde VM.
* De instellingen voor schijfconfiguratie versleuteling wordt verwijderd uit de virtuele machine.
* Back-up en herstellen van de versleutelde virtuele machines met behulp van de Azure Backup-service.

Azure Disk Encryption voor virtuele machines voor Windows en Linux bevat:

* [De schijf encryption-uitbreiding voor Windows](../virtual-machines/extensions/azure-disk-enc-windows.md).
* [De schijf encryption-extensie voor Linux](../virtual-machines/extensions/azure-disk-enc-linux.md).
* [De PowerShell-cmdlets voor de versleuteling van schijf](/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-2.2.0).
* [De cmdlets van Azure CLI schijf versleuteling](/cli/azure/vm/encryption?view=azure-cli-latest).
* [De Azure Resource Manager-sjablonen voor schijf versleuteling](azure-security-disk-encryption-appendix.md#resource-manager-templates).

> [!NOTE]
> Er zijn geen extra kosten voor het versleutelen van VM-schijven met Azure Disk Encryption. Standard [prijzen voor Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) is van toepassing op de sleutelkluis die wordt gebruikt voor het opslaan van de versleutelingssleutels. 

## <a name="encryption-workflow"></a>Werkstroom voor versleuteling

Om in te schakelen disk encryption voor Windows en Linux-machines, moet u de volgende stappen uitvoeren:

1. U meldt zich aan het inschakelen van versleuteling van schijf via de Azure Disk Encryption Resource Manager-sjabloon, PowerShell-cmdlets of de Azure CLI en geeft u de versleutelingsconfiguratie.

   * Voor het scenario van de VHD klant versleuteld door de versleutelde VHD te uploaden naar uw opslagaccount en de versleuteling sleutelmateriaal tot uw key vault. Klik, geef de versleutelingsconfiguratie inschakelen van versleuteling op een nieuwe virtuele machine.
   * Voor nieuwe virtuele machines die zijn gemaakt op basis van ondersteunde galerie met installatiekopieën en bestaande VM's die al worden uitgevoerd in Azure, geeft u de versleutelingsconfiguratie inschakelen van versleuteling op de virtuele machine.

1. Toegang verlenen tot de Azure-platform het sleutelmateriaal van versleuteling (BitLocker-versleutelingssleutels voor Windows-systemen) en de wachtwoordzin voor Linux lezen uit uw key vault om in te schakelen van versleuteling op de virtuele machine.

1. Azure werkt bij het VM-servicemodel met versleuteling en de configuratie van de key vault en stelt u de versleutelde VM.

   ![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Werkstroom voor ontsleuteling
Als u wilt uitschakelen disk encryption voor virtuele machines, de volgende stappen op hoog niveau:

1. Kies om versleuteling (decodering) te schakelen op een actieve virtuele machine in Azure en geeft u de configuratie voor ontsleuteling. U kunt uitschakelen via de Azure Disk Encryption Resource Manager-sjabloon, PowerShell-cmdlets of de Azure CLI.

   Deze stap schakelt versleuteling van het besturingssysteem of het gegevensvolume of beide op de actieve virtuele machine van Windows. Zoals vermeld in de vorige sectie, wordt niet uitschakelen van OS-schijfversleuteling voor Linux ondersteund. De stap ontsleuteling is alleen toegestaan voor schijven op virtuele Linux-machines, zolang de besturingssysteemschijf wordt niet versleuteld.

1. Azure-updates het servicemodel van de virtuele machine en de virtuele machine is gemarkeerd als ontsleuteld. De inhoud van de virtuele machine worden niet meer in rust versleuteld.

   > [!NOTE]
   > Versleuteling met de bewerking verwijderd niet uit uw key vault en de versleuteling sleutelmateriaal (BitLocker-versleutelingssleutels voor Windows-systemen) of de wachtwoordzin voor Linux.
   >
   > Uitschakelen van OS-schijfversleuteling voor Linux wordt niet ondersteund. De stap ontsleuteling is alleen toegestaan voor schijven op virtuele Linux-machines.
   >
   > Uitschakelen van de schijf van gegevensversleuteling voor Linux wordt niet ondersteund als de besturingssysteemschijf is versleuteld.


## <a name="encryption-workflow-previous-release"></a>Versleuteling werkstroom (vorige versie)

De nieuwe versie van Azure Disk Encryption wordt voorkomen dat de vereiste voor een parameter van de toepassing Azure Active Directory (Azure AD) om in te schakelen van VM-schijfversleuteling. Met de nieuwe release, bent u niet langer vereist voor een Azure AD-referenties opgeven tijdens de stap van de versleuteling inschakelen. Alle nieuwe virtuele machines moeten worden versleuteld zonder de parameters van de toepassing Azure AD wanneer u de nieuwe versie. Virtuele machines die al zijn versleuteld met Azure AD-toepassing parameters worden nog steeds ondersteund en worden onderhouden met de syntaxis van de Azure AD moeten blijven. Om in te schakelen disk encryption voor Windows en Linux-machines (vorige versie), voer de volgende stappen uit:

1. Kies een versleutelingsscenario in de scenario's die worden vermeld in de [versleutelingsscenario](#encryption-scenarios) sectie.

1. U meldt zich aan het inschakelen van versleuteling van schijf via de Azure Disk Encryption Resource Manager-sjabloon, PowerShell-cmdlets of de Azure CLI en geeft u de versleutelingsconfiguratie.

   * Voor het scenario van de VHD klant versleuteld door de versleutelde VHD te uploaden naar uw opslagaccount en de versleuteling sleutelmateriaal tot uw key vault. Klik, geef de versleutelingsconfiguratie inschakelen van versleuteling op een nieuwe virtuele machine.
   * Voor nieuwe virtuele machines die zijn gemaakt vanuit de Marketplace en bestaande VM's die al worden uitgevoerd in Azure, geeft u de versleutelingsconfiguratie inschakelen van versleuteling op de virtuele machine.

1. Toegang verlenen tot de Azure-platform het sleutelmateriaal van versleuteling (BitLocker-versleutelingssleutels voor Windows-systemen) en de wachtwoordzin voor Linux lezen uit uw key vault om in te schakelen van versleuteling op de virtuele machine.

1. Geef de Azure AD-toepassings-id voor het schrijven van de versleutelingssleutel materiaal naar uw key vault. Deze stap kunt u versleuteling op de virtuele machine voor de scenario's die worden vermeld in stap 2.

1. Azure werkt bij het VM-servicemodel met versleuteling en de configuratie van de key vault en stelt u de versleutelde VM.


## <a name="terminology"></a>Terminologie
De volgende tabel bevat enkele van de algemene termen gebruikt in Azure disk encryption-documentatie:

| Terminologie | Definitie |
| --- | --- |
| Azure AD | Een [Azure AD](https://azure.microsoft.com/documentation/services/active-directory/) account wordt gebruikt om te verifiëren, opslaan en ophalen van geheimen van een key vault. |
| Azure Key Vault | Key Vault is een cryptografische, key management-service die op Federal Information Processing Standards (FIPS) gevalideerde hardware security modules is gebaseerd. Deze standaarden helpen om uw cryptografische sleutels en gevoelige geheimen te beveiligen. Zie voor meer informatie de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentatie. |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is een industrie herkend Windows volume versleutelingstechnologie die wordt gebruikt om in te schakelen schijfversleuteling op Windows-VM's. |
| BEK | BitLocker-versleutelingssleutels (BEK) worden gebruikt voor het versleutelen van het opstartvolume OS en gegevensvolumes. BEKs worden beveiligd in een key vault als geheimen. |
| Azure-CLI | [De Azure CLI](/cli/azure/install-azure-cli) is geoptimaliseerd voor het beheren en beheer van Azure-resources vanaf de opdrachtregel.|
| DM-Crypt |[DM-Crypt](https://gitlab.com/cryptsetup/cryptsetup/wikis/DMCrypt) is het subsysteem voor Linux gebaseerde, transparante schijfversleuteling die wordt gebruikt om in te schakelen schijfversleuteling voor de virtuele Linux-machines. |
| Sleutel van versleutelingssleutel (KEK) | De asymmetrische sleutel (RSA 2048) die u gebruiken kunt om te beveiligen of de geheime sleutel verpakken. U kunt opgeven dat een hardware security module (HSM)- of software beschermde sleutel beschermd. Zie voor meer informatie de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentatie. |
| PowerShell-cmdlets | Zie voor meer informatie, [Azure PowerShell-cmdlets](/powershell/azure/overview). |

## <a name="next-steps"></a>Volgende stappen

Als u wilt beginnen, Zie de [vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md).

