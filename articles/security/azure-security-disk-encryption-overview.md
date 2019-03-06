---
title: Overzicht - Azure Disk Encryption voor IaaS-VM's | Microsoft Docs
description: In dit artikel biedt een overzicht van Microsoft Azure-schijfversleuteling voor IaaS-VM's.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 03/05/2019
ms.custom: seodec18
ms.openlocfilehash: 67f1488af330ac666aa3f9fe83e5b5ee42e6df2f
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57410040"
---
# <a name="azure-disk-encryption-for-iaas-vms"></a>Azure Disk Encryption voor IaaS-VM 's

Microsoft Azure is het belangrijk ervoor te zorgen dat de privacy van gegevens en de gegevensonafhankelijkheid. Met Azure kunt u voor het beheren van uw gegevens wordt gehost op Azure door een bereik met geavanceerde technologieën voor het coderen, beheren en versleutelingssleutels en controle en audit toegang krijgen tot gegevens beheren. Dit besturingselement biedt Azure klanten de flexibiliteit om te kiezen welke oplossing het beste voldoet aan de behoeften van uw bedrijf. In dit artikel vindt u een technologieoplossing: 'Azure Disk Encryption voor Windows en Linux IaaS virtuele machines (VM's)'. Deze technologie kunt beschermen en beveiligen van uw gegevens om te voldoen aan uw organisatie beveiligings- en nalevingsverplichtingen. 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Overzicht

Azure Disk Encryption is een functie waarmee u uw Windows- en Linux IaaS-VM-schijven te versleutelen. Schijfversleuteling maakt gebruik van de industrienorm [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) functie van Windows en de [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) -functie van Linux om volumeversleuteling voor het besturingssysteem en gegevensschijven te bieden. De oplossing is geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te bepalen en de schijf-versleutelingssleutels en geheimen beheren. De oplossing zorgt er ook voor dat alle gegevens op de VM-schijven worden versleuteld in rust in uw Azure-opslag.

Schijf versleuteling voor Windows en Linux IaaS-VM's is algemeen beschikbaar in alle regio's Azure Government voor Standard VM's en virtuele machines met Azure Premium Storage en Azure openbare regio's. Wanneer u het beheersysteem voor schijfversleuteling toepast, kunt u voldoen aan de volgende zaken dat vereisen:

* IaaS-VM's worden beveiligd at-rest met behulp van industriestandaard versleutelingstechnologie om de organisatorische vereisten voor beveiliging en naleving.
* IaaS-VM's worden opnieuw opgestart bij de klant beheerde sleutels en beleidsregels. U kunt het gebruik ervan controleren in uw key vault.

Als u Azure Security Center gebruikt, bent u gewaarschuwd als er virtuele machines die niet zijn versleuteld. De waarschuwingen worden weergegeven als hoge urgentie en de aanbeveling is voor het versleutelen van deze VM's.

![Azure Security Center schijf versleuteling waarschuwing](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Bepaalde aanbevelingen verhogen gegevens, netwerk, of gebruik van de compute-bronnen en leiden tot extra kosten in licentie of abonnement.


## <a name="encryption-scenarios"></a>Scenario's voor versleuteling

Het Disk Encryption-oplossing ondersteunt de volgende klant-scenario's:

* Hiermee schakelt u versleuteling op nieuwe Windows IaaS-VM's gemaakt op basis van vooraf gecodeerde VHD en versleuteling sleutels.
* Hiermee schakelt u versleuteling op nieuwe IaaS VM's die worden gemaakt op basis van de ondersteunde Azure-galerie-installatiekopieën.
* Hiermee schakelt u versleuteling op bestaande IaaS-VM's die worden uitgevoerd in Azure.
* Versleuteling op Windows virtual machine scale sets inschakelen.
* Hiermee schakelt u versleuteling op gegevensstations voor schaalsets voor Linux-machines.
* Schakel versleuteling uit op virtuele Windows IaaS-machines.
* Schakel versleuteling uit op gegevensstations voor Linux IaaS-VM's.
* Op Windows virtual machine scale sets-versleuteling uitschakelen.
* Schakel versleuteling uit op schijven voor schaalsets voor Linux-machines.
* Schakel versleuteling van virtuele machines op beheerde schijven.
* Versleutelingsinstellingen voor van een bestaande versleutelde Premium en niet - Premium Storage VM bijwerken.
* Back-up en herstel van versleutelde virtuele machines.

De oplossing ondersteunt de volgende scenario's voor IaaS-VM's wanneer deze zijn ingeschakeld in Microsoft Azure:

* Integratie met Azure Key Vault.
* Standard-laag virtuele machines: [A, D, DS, G, GS, F, enzovoort, reeks IaaS-VM's](https://azure.microsoft.com/pricing/details/virtual-machines/). [Virtuele Linux-machines](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) binnen deze lagen als moeten voldoen aan de minimale geheugenvereiste van 7 GB.
* Versleuteling inschakelen op Windows en Linux IaaS-VM's, beheerde schijf en schaal instellen virtuele machines van de ondersteunde Azure-galerie afbeeldingen.
* Disable-versleuteling voor stations met besturingssysteem- en voor Windows IaaS-VM's, schaal virtuele machines en virtuele machines op schijven beheerd.
* Schakel versleuteling op gegevensstations voor Linux IaaS-VM's, schaal virtuele machines en virtuele machines op schijven beheerd.
* Hiermee schakelt u versleuteling op IaaS-VM's waarop het Windows Client-besturingssysteem wordt uitgevoerd.
* Versleuteling voor volumes met koppelpunt paden inschakelen.
* Versleuteling op Linux-VM's die zijn geconfigureerd met de schijf (RAID) met behulp van mdadm striping inschakelen.
* Hiermee schakelt u versleuteling op Linux-VM's die gebruikmaken van LVM voor gegevensschijven.
* Hiermee schakelt u versleuteling op de Linux VM-besturingssysteem en gegevensschijven.

   > [!NOTE]
   > OS-stationsversleuteling voor bepaalde Linux-distributies wordt niet ondersteund. Zie voor meer informatie de [Veelgestelde vragen over Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) artikel.
   
* Hiermee schakelt u versleuteling op virtuele machines die zijn geconfigureerd met Windows Storage Spaces begin in Windows Server 2016.
* Versleutelingsinstellingen voor een bestaande versleutelde Premium en niet - Premium Storage VM bijwerken.
* Back-up en herstel van versleutelde virtuele machines voor zowel de sleutel van versleutelingssleutel (KEK-sleutel) als de niet-KEK-scenario's.
* Alle openbare Azure- en Azure Government-regio's worden ondersteund.

De oplossing biedt geen ondersteuning voor de volgende scenario's, functies en -technologie:

* Basic-laag IaaS-VM's.
* Schakel versleuteling uit op een station van het besturingssysteem voor Linux IaaS-VM's.
* Schakel versleuteling uit op een schijf als de besturingssysteemschijf is gecodeerd voor Linux IaaS-VM's.
* IaaS-VM's die zijn gemaakt met behulp van de klassieke methode voor het maken van de virtuele machine.
* Versleuteling van aangepaste installatiekopieën van de klant op Linux IaaS-VM's inschakelen.
* Integratie met uw on-premises sleutelbeheersysteem.
* Azure-bestanden (gedeelde bestandssysteem).
* Network File System (NFS).
* Dynamische volumes.
* Windows-VM's die zijn geconfigureerd met software gebaseerde RAID-systemen.

## <a name="encryption-features"></a>Versleutelingsfuncties

Wanneer u inschakelen en Disk Encryption voor Azure IaaS VM's geïmplementeerd, worden de volgende mogelijkheden zijn ingeschakeld, afhankelijk van de opgegeven configuratie:

* Versleuteling van het volume met het besturingssysteem op het opstartvolume at-rest in uw opslag te beveiligen.
* Versleuteling van gegevensvolumes om de gegevensvolumes in rust in uw opslag te beveiligen.
* Schakel versleuteling uit op de OS- en gegevensstations voor Windows IaaS-VM's.
* Schakel versleuteling uit op de schijven voor Linux IaaS-VM's (alleen als de besturingssysteemschijf wordt niet versleuteld).
* Beveilig de versleutelingssleutels en geheimen in uw Azure Key Vault-abonnement.
* Rapporteert de versleutelingsstatus van de versleutelde IaaS-VM.
* Verwijder de instellingen voor schijfconfiguratie versleuteling van de IaaS-VM.
* Back-up en herstellen van de versleutelde virtuele machines met behulp van de Azure Backup-service.

Azure Disk Encryption voor IaaS VMS voor Windows en Linux-oplossing omvat:

* De schijf encryption-uitbreiding voor Windows.
* De schijf encryption-extensie voor Linux.
* De PowerShell-cmdlets voor schijf-codering.
* De Azure CLI-cmdlets voor schijf-codering.
* De Azure Resource Manager-sjablonen voor schijf-codering.

De Azure Disk Encryption-oplossing wordt ondersteund op IaaS-VM's waarop Windows of Linux-besturingssysteem wordt uitgevoerd. Zie voor meer informatie over de ondersteunde besturingssystemen, de [vereisten](azure-security-disk-encryption-prerequisites.md) artikel.

> [!NOTE]
> Er zijn geen extra kosten voor het versleutelen van VM-schijven met Azure Disk Encryption. Standard [prijzen voor Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) is van toepassing op de sleutelkluis die wordt gebruikt voor het opslaan van de versleutelingssleutels. 


## <a name="encryption-workflow"></a>Werkstroom voor versleuteling

Om in te schakelen disk encryption voor Windows en Linux-machines, moet u de volgende stappen uitvoeren:

1. Kies een versleutelingsscenario in de scenario's die worden vermeld in de [versleutelingsscenario](#encryption-scenarios) sectie.

1. U meldt zich aan het inschakelen van versleuteling van schijf via de Azure Disk Encryption Resource Manager-sjabloon, PowerShell-cmdlets of de Azure CLI en geeft u de versleutelingsconfiguratie.

   * Voor het scenario van de VHD klant versleuteld door de versleutelde VHD te uploaden naar uw opslagaccount en de versleuteling sleutelmateriaal tot uw key vault. Klik, geef de versleutelingsconfiguratie inschakelen van versleuteling op een nieuwe IaaS-VM.
   * Voor nieuwe virtuele machines die zijn gemaakt vanuit de Marketplace en bestaande VM's die al worden uitgevoerd in Azure, geeft u de versleutelingsconfiguratie inschakelen van versleuteling op de IaaS-VM.

1. Toegang verlenen tot de Azure-platform het sleutelmateriaal van versleuteling (BitLocker-versleutelingssleutels voor Windows-systemen) en de wachtwoordzin voor Linux lezen uit uw key vault om in te schakelen van versleuteling op de IaaS-VM.

1. Azure werkt bij het VM-servicemodel met versleuteling en de configuratie van de key vault en stelt u de versleutelde VM.

   ![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Werkstroom voor ontsleuteling
Als u wilt uitschakelen schijfversleuteling voor IaaS-VM's, de volgende stappen op hoog niveau:

1. Kies om versleuteling (decodering) te schakelen op een actieve IaaS-VM in Azure en geeft u de configuratie voor ontsleuteling. U kunt uitschakelen via de Azure Disk Encryption Resource Manager-sjabloon, PowerShell-cmdlets of de Azure CLI.

   Deze stap schakelt versleuteling van het besturingssysteem of het gegevensvolume of beide op de actieve Windows IaaS-virtuele machine. Zoals vermeld in de vorige sectie, wordt niet uitschakelen van OS-schijfversleuteling voor Linux ondersteund. De stap ontsleuteling is alleen toegestaan voor schijven op virtuele Linux-machines, zolang de besturingssysteemschijf wordt niet versleuteld.

1. Azure-updates het servicemodel van de virtuele machine en de IaaS-VM is gemarkeerd als ontsleuteld. De inhoud van de virtuele machine worden niet meer in rust versleuteld.

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

   * Voor het scenario van de VHD klant versleuteld door de versleutelde VHD te uploaden naar uw opslagaccount en de versleuteling sleutelmateriaal tot uw key vault. Klik, geef de versleutelingsconfiguratie inschakelen van versleuteling op een nieuwe IaaS-VM.
   * Voor nieuwe virtuele machines die zijn gemaakt vanuit de Marketplace en bestaande VM's die al worden uitgevoerd in Azure, geeft u de versleutelingsconfiguratie inschakelen van versleuteling op de IaaS-VM.

1. Toegang verlenen tot de Azure-platform het sleutelmateriaal van versleuteling (BitLocker-versleutelingssleutels voor Windows-systemen) en de wachtwoordzin voor Linux lezen uit uw key vault om in te schakelen van versleuteling op de IaaS-VM.

1. Geef de Azure AD-toepassings-id voor het schrijven van de versleutelingssleutel materiaal naar uw key vault. Deze stap kunt u versleuteling op de IaaS-VM voor de scenario's die worden vermeld in stap 2.

1. Azure werkt bij het VM-servicemodel met versleuteling en de configuratie van de key vault en stelt u de versleutelde VM.


## <a name="terminology"></a>Terminologie
De volgende tabel worden enkele van de algemene voorwaarden die worden gebruikt in deze technologie gedefinieerd:

| Terminologie | Definitie |
| --- | --- |
| Azure AD | Een [Azure AD](https://azure.microsoft.com/documentation/services/active-directory/) account wordt gebruikt om te verifiëren, opslaan en ophalen van geheimen van een key vault. |
| Azure Key Vault | Key Vault is een cryptografische, key management-service die op Federal Information Processing Standards (FIPS) gevalideerde hardware security modules is gebaseerd. Deze standaarden helpen om uw cryptografische sleutels en gevoelige geheimen te beveiligen. Zie voor meer informatie de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentatie. |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is een industrie herkend Windows volume versleutelingstechnologie die wordt gebruikt om in te schakelen schijfversleuteling voor IaaS-VM's van Windows. |
| BEK | BitLocker-versleutelingssleutels (BEK) worden gebruikt voor het versleutelen van het opstartvolume OS en gegevensvolumes. BEKs worden beveiligd in een key vault als geheimen. |
| Azure-CLI | [De Azure CLI](/cli/azure/install-azure-cli) is geoptimaliseerd voor het beheren en beheer van Azure-resources vanaf de opdrachtregel.|
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is het subsysteem voor Linux gebaseerde, transparante schijfversleuteling die wordt gebruikt om in te schakelen schijfversleuteling op Linux IaaS-VM's. |
| KEK-SLEUTEL | Sleutel van versleutelingssleutel (KEK) is de asymmetrische sleutel (RSA 2048) die u gebruiken kunt om te beveiligen of de geheime sleutel verpakken. U kunt opgeven dat een hardware security module (HSM)- of software beschermde sleutel beschermd. Zie voor meer informatie de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentatie. |
| PowerShell-cmdlets | Zie voor meer informatie, [Azure PowerShell-cmdlets](/powershell/azure/overview). |

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md)
