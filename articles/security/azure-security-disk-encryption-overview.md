---
title: Azure Disk Encryption voor IaaS VM's-overzicht | Microsoft Docs
description: In dit artikel biedt een overzicht van Microsoft Azure-schijfversleuteling voor IaaS-VM's.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 09/10/2018
ms.openlocfilehash: ea72e4ac778e52d4f6e4f5597d38a1de59ba43fc
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348936"
---
# <a name="azure-disk-encryption-for-iaas-vms"></a>Azure Disk Encryption voor IaaS-VM 's 
Microsoft Azure is het belangrijk ervoor te zorgen dat de privacy van gegevens, gegevensonafhankelijkheid en zodat u kunt om te bepalen uw door Azure gehoste gegevens door een bereik met geavanceerde technologieën voor het coderen, beheren en beheren van versleutelingssleutels en controle en audit toegang tot gegevens. Dit besturingselement biedt Azure-klanten de flexibiliteit om de oplossing die het beste aan hun behoeften van uw bedrijf te kiezen. In dit artikel vindt u een technologieoplossing, 'Azure schijf versleuteling voor Windows en Linux IaaS VM's ', om te beschermen en beveiligen van uw gegevens om te voldoen aan uw organisatie beveiligings- en nalevingsverplichtingen. 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Overzicht
Azure Disk Encryption (ADE) is een functie waarmee u uw Windows- en Linux IaaS VM-schijven te versleutelen. ADE maakt gebruik van de industrienorm [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) functie van Windows en de [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) -functie van Linux om volumeversleuteling voor het besturingssysteem en gegevensschijven te bieden. De oplossing is geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te bepalen en de schijf-versleutelingssleutels en geheimen beheren. De oplossing zorgt er ook voor dat alle gegevens op de virtuele-machineschijven zijn versleuteld in rust in uw Azure-opslag.

Azure disk encryption voor Windows en Linux IaaS-VM's zich in **algemene beschikbaarheid** in alle Azure-openbare regio's en AzureGov regio's voor standaard-VM's en VM's met premium storage. Wanneer u de Azure Disk Encryption-management-oplossing toepast, kunt u voldoen aan de volgende zaken dat vereisen:

* IaaS-VM's worden beveiligd met behulp van industriestandaard versleutelingstechnologie beveiligingsbehoeften voor organisatie-en nalevingsvereisten.
* Opstarten van de IaaS-VM's onder de klant beheerde sleutels en beleidsregels en u kunt het gebruik ervan in uw key vault controleren.


Als u Azure Security Center gebruikt, geeft deze een melding als u virtuele machines die niet zijn versleuteld. Deze waarschuwingsberichten hebben een hoge prioriteit. Het wordt aangeraden om deze virtuele machines te versleutelen.
![Azure Security Center schijf versleuteling waarschuwing](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Bepaalde aanbevelingen verhogen gegevens-, netwerk- of computerresources, wat resulteert in extra kosten in licentie of abonnement.


## <a name="encryption-scenarios"></a>Scenario's voor versleuteling
De Azure Disk Encryption-oplossing ondersteunt de volgende klant-scenario's:

* Schakelt u versleuteling op nieuwe Windows IaaS-VM's gemaakt op basis van vooraf gecodeerde VHD en versleuteling sleutels 
* Schakelt u versleuteling op nieuwe IaaS VM's die worden gemaakt op basis van de installatiekopieën van de ondersteunde Azure-galerie
* Schakelt u versleuteling op bestaande IaaS-VM's die worden uitgevoerd in Azure
* Inschakelen van versleuteling op Windows virtual machine scale sets
* Schakelt u versleuteling op gegevensstations voor Linux virtuele-machineschaalsets
* Schakel versleuteling uit op virtuele Windows IaaS-machines
* Schakel versleuteling uit op gegevensstations voor Linux IaaS-VM 's
* Uitschakelen van versleuteling op Windows virtual machine scale sets
* Schakel versleuteling uit op de schijven voor schaalsets voor Linux-machines
* Inschakelen van versleuteling van virtuele machines op beheerde schijven
* Instellingen voor versleuteling van een bestaande versleutelde premium- en niet-premium-opslag virtuele machine bijwerken
* Back-up en herstel van versleutelde virtuele machines

De oplossing ondersteunt de volgende scenario's voor IaaS-VM's wanneer deze zijn ingeschakeld in Microsoft Azure:

* Integratie met Azure Key Vault
* Standard-laag virtuele machines: [A, D, DS, G, GS, F en enzovoort reeks IaaS-VM's](https://azure.microsoft.com/pricing/details/virtual-machines/)
    * [Virtuele Linux-machines](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) binnen deze lagen als moeten voldoen aan de minimale geheugenvereiste van 7 GB
* Versleuteling inschakelen op Windows en Linux IaaS-VM's, beheerde schijf en schaal instellen virtuele machines van de ondersteunde Azure-galerie afbeeldingen
* Disable-versleuteling voor stations met besturingssysteem- en voor Windows IaaS-VM's, schaal virtuele machines en beheerde schijf-VM 's
* Schakel versleuteling op gegevensstations voor Linux IaaS-VM's, schaal virtuele machines en beheerde schijf-VM 's
* Schakelt u versleuteling op IaaS-VM's met het Windows Client-besturingssysteem
* Versleuteling voor volumes met koppelpunt paden inschakelen
* Schakelt u versleuteling op Linux-VM's geconfigureerd met schijf striping (RAID) met behulp van mdadm
* Schakelt u versleuteling op virtuele Linux-machines met behulp van LVM voor gegevensschijven
* Schakelt u versleuteling op Linux VM-besturingssysteem en gegevensschijven 
* Schakelt u versleuteling op Windows-VM's geconfigureerd met opslagruimten
* Instellingen voor versleuteling van een bestaande versleutelde premium- en niet-premium-opslag virtuele machine bijwerken
* Back-up en herstel van versleutelde virtuele machines, voor zowel niet-KEK-sleutel en KEK-scenario's (KEK - Key-versleutelingssleutel)
* Alle openbare Azure- en AzureGov regio's worden ondersteund

De oplossing biedt geen ondersteuning voor de volgende scenario's, functies en -technologie:

* Basic-laag IaaS-VM 's
* Uitschakelen van versleuteling op een station van het besturingssysteem voor Linux IaaS-VM 's
* Uitschakelen van versleuteling op een schijf als de besturingssysteemschijf is versleuteld voor Linux Iaas-VM 's
* IaaS-VM's die zijn gemaakt met behulp van de klassieke methode voor het maken van de virtuele machine
* Inschakelen van versleuteling op Linux IaaS-VM's klanten aangepaste installatiekopieën
* Integratie met uw on-premises Key Management Service
* Azure Files (gedeelde bestandssysteem), Network File System (NFS), dynamische volumes en virtuele machines van Windows die zijn geconfigureerd met software gebaseerde RAID-systemen

## <a name="encryption-features"></a>Versleutelingsfuncties
Als u inschakelt en implementeren van Azure Disk Encryption voor Azure IaaS VM's, worden de volgende mogelijkheden ingeschakeld, afhankelijk van de opgegeven configuratie:

* Versleuteling van het volume met het besturingssysteem op het opstartvolume at-rest in de opslag beveiligen
* Versleuteling van gegevensvolumes om de gegevensvolumes in rust in uw opslag te beveiligen
* Uitschakelen van versleuteling op de OS- en gegevensstations voor Windows IaaS-VM 's
* Uitschakelen van versleuteling op de schijven voor Linux IaaS-VM's (alleen als besturingssysteemschijf wordt niet versleuteld)
* Bescherming van de versleutelingssleutels en geheimen in uw key vault-abonnement
* Rapportage van de versleutelingsstatus van de versleutelde IaaS-VM
* Het verwijderen van configuratie-instellingen van de virtuele machine voor IaaS-schijfversleuteling
* Back-up en herstel van versleutelde virtuele machines met behulp van de Azure Backup-service

Azure Disk Encryption voor IaaS VMS voor Windows en Linux-oplossing omvat:

* De schijf encryption-uitbreiding voor Windows.
* De schijf encryption-extensie voor Linux.
* De schijfversleuteling van PowerShell-cmdlets.
* Schijfversleuteling cmdlets van Azure-opdrachtregelinterface (CLI).
* De schijf versleuteling Azure Resource Manager-sjablonen.

De Azure Disk Encryption-oplossing wordt ondersteund op IaaS-VM's waarop Windows of Linux-besturingssysteem. Zie voor meer informatie over de ondersteunde besturingssystemen, de [vereisten](azure-security-disk-encryption-prerequisites.md) artikel.

> [!NOTE]
> Er is een extra kosten in rekening gebracht voor het versleutelen van VM-schijven met Azure Disk Encryption. Standard [prijzen voor Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) is van toepassing op de key vault gebruikt voor het opslaan van de versleutelingssleutels. 


## <a name="encryption-workflow"></a>Werkstroom voor versleuteling

 Om in te schakelen disk encryption voor Windows en Linux-machines, moet u de volgende stappen uitvoeren:

1. Kies een versleutelingsscenario uit de bovenstaande scenario's voor versleuteling.
2. U meldt zich aan het inschakelen van schijfversleuteling via de Azure Disk Encryption Resource Manager-sjabloon, de PowerShell-cmdlets of de CLI-opdracht en geeft u de versleutelingsconfiguratie van de.

   * Voor het scenario van de VHD klant versleuteld door de versleutelde VHD te uploaden naar uw opslagaccount en de versleuteling sleutelmateriaal tot uw key vault. Klik, geef de versleutelingsconfiguratie inschakelen van versleuteling op een nieuwe IaaS-VM.
   * Voor nieuwe virtuele machines die zijn gemaakt vanuit de Marketplace en bestaande VM's die al worden uitgevoerd in Azure, geeft u de versleutelingsconfiguratie inschakelen van versleuteling op de IaaS-VM.

3. Toegang verlenen tot de Azure-platform om te lezen van de versleutelingssleutel materiaal (BitLocker-versleutelingssleutels voor Windows-systemen) en de wachtwoordzin voor Linux uit uw key vault om in te schakelen van versleuteling op de IaaS-VM.

4. Azure werkt bij het VM-servicemodel met versleuteling, de configuratie van de key vault, en stelt u de versleutelde VM.

 ![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Werkstroom voor ontsleuteling
Als u wilt uitschakelen schijfversleuteling voor IaaS-VM's, de volgende stappen op hoog niveau:

1. Kies om versleuteling (decodering) te schakelen op een actieve IaaS-VM in Azure en geeft u de configuratie voor ontsleuteling. U kunt uitschakelen via de Azure Disk Encryption Resource Manager-sjabloon, PowerShell-cmdlets of Azure CLI.

 Deze stap schakelt versleuteling van het besturingssysteem of het gegevensvolume of beide op de actieve Windows IaaS-virtuele machine. Echter, uitschakelen van OS-schijfversleuteling voor Linux zoals vermeld in de vorige sectie, wordt niet ondersteund. De stap ontsleuteling is alleen toegestaan voor schijven op virtuele Linux-machines, zolang de besturingssysteemschijf wordt niet versleuteld.
2. Azure werkt het servicemodel van de virtuele machine en de IaaS-VM is gemarkeerd als ontsleutelde. De inhoud van de virtuele machine worden niet meer in rust versleuteld.

> [!NOTE]
> De bewerking uitschakelen-codering wordt niet verwijderd voor uw key vault en de versleuteling sleutelmateriaal (BitLocker-versleutelingssleutels voor Windows-systemen) of de wachtwoordzin voor Linux.
 > Uitschakelen van OS-schijfversleuteling voor Linux wordt niet ondersteund. De stap ontsleuteling is alleen toegestaan voor schijven op virtuele Linux-machines.
Uitschakelen van de schijf van gegevensversleuteling voor Linux wordt niet ondersteund als de besturingssysteemschijf is versleuteld.


## <a name="encryption-workflow-previous-release"></a>Versleuteling werkstroom (vorige versie)

De nieuwe versie van Azure disk encryption wordt voorkomen dat de vereiste voor het ontwikkelen van een Azure AD-toepassing-parameter voor schijfversleuteling van VM inschakelen. Met de nieuwe versie kunt u niet langer vereist op te geven van een referentie voor de Azure AD tijdens de stap van de versleuteling inschakelen. Alle nieuwe virtuele machines moeten worden versleuteld zonder de parameters van Azure AD-toepassing met behulp van de nieuwe versie. Virtuele machines die al zijn versleuteld met Azure AD-toepassing parameters worden nog steeds ondersteund en worden onderhouden met de syntaxis van de AAD moeten blijven. Om in te schakelen disk encryption voor Windows en Linux-machines (vorige versie), voer de volgende stappen uit:

1. Kies een versleutelingsscenario uit de bovenstaande scenario's voor versleuteling.
2. U meldt zich aan het inschakelen van schijfversleuteling via de Azure Disk Encryption Resource Manager-sjabloon, de PowerShell-cmdlets of de CLI-opdracht en geeft u de versleutelingsconfiguratie van de.

   * Voor het scenario van de VHD klant versleuteld door de versleutelde VHD te uploaden naar uw opslagaccount en de versleuteling sleutelmateriaal tot uw key vault. Klik, geef de versleutelingsconfiguratie inschakelen van versleuteling op een nieuwe IaaS-VM.
   * Voor nieuwe virtuele machines die zijn gemaakt vanuit de Marketplace en bestaande VM's die al worden uitgevoerd in Azure, geeft u de versleutelingsconfiguratie inschakelen van versleuteling op de IaaS-VM.

3. Toegang verlenen tot de Azure-platform om te lezen van de versleutelingssleutel materiaal (BitLocker-versleutelingssleutels voor Windows-systemen) en de wachtwoordzin voor Linux uit uw key vault om in te schakelen van versleuteling op de IaaS-VM.

4. Geef de toepassings-id van de Azure Active Directory (Azure AD) voor het schrijven van de versleutelingssleutel materiaal naar uw key vault. In dat geval kunt u versleuteling op de IaaS-VM voor de scenario's die worden vermeld in stap 2.

5. Azure werkt bij het VM-servicemodel met versleuteling en de configuratie van de key vault en stelt u de versleutelde VM.


## <a name="terminology"></a>Terminologie
Voor meer informatie over enkele van de algemene voorwaarden die worden gebruikt door deze technologie, gebruik de volgende tabel voor de terminologie:

| Terminologie | Definitie |
| --- | --- |
| Azure AD | Azure AD is [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Een Azure AD-account wordt gebruikt voor verificatie, opslaan en ophalen van geheimen van een key vault. |
| Azure Key Vault | Key Vault is een cryptografische, key management-service die gebaseerd op Federal Information Processing Standards (FIPS) gevalideerde hardware security modules, die helpt bij de beveiliging uw cryptografische sleutels en gevoelige geheimen. Zie voor meer informatie, [Key Vault](https://azure.microsoft.com/services/key-vault/) documentatie. |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is een industrie herkend Windows volume versleutelingstechnologie die wordt gebruikt om in te schakelen schijfversleuteling voor IaaS-VM's van Windows. |
| BEK | BitLocker-versleutelingssleutels worden gebruikt voor het versleutelen van het opstartvolume OS en gegevensvolumes. De BitLocker-sleutels worden beveiligd in een key vault als geheimen. |
| CLI | Zie [Azure-opdrachtregelinterface](/cli/azure/install-azure-cli).|
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is het subsysteem voor Linux gebaseerde, transparante schijfversleuteling die wordt gebruikt om in te schakelen schijfversleuteling op Linux IaaS-VM's. |
| KEK-SLEUTEL | Sleutel van versleutelingssleutel is een asymmetrische sleutel (RSA 2048) die u gebruiken kunt om te beveiligen of de geheime sleutel verpakken. U kunt opgeven dat een hardware security module (HSM)- of software beschermde sleutel beschermd. Zie voor meer informatie, [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentatie. |
| PS-cmdlets | Zie [Azure PowerShell-cmdlets](/powershell/azure/overview). |

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md)
