---
title: Azure Disk Encryption for Windows and Linux IaaS VM's | Microsoft Docs
description: Dit artikel bevat een overzicht van Microsoft Azure Disk Encryption for Windows en Linux IaaS VM's.
services: security
documentationcenter: na
author: DevTiw
manager: avibm
editor: barclayn
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: devtiw;ejarvi;mayank88mahajan;vermashi;sudhakarareddyevuri;aravindthoram
ms.openlocfilehash: d6a19334b369c54ff6bad3404b4cf2ffe3b47c70
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Azure Disk Encryption for Windows and Linux IaaS VM 's
Microsoft Azure is sterk doorgevoerd om ervoor te zorgen voor uw privacy van gegevens, onafhankelijkheid van gegevens en schakelt u de controle uw Azure gegevens via een bereik van gehoste technologieën geavanceerde voor het versleutelen, beheren en beheren van versleutelingssleutels besturingselement & audit toegang van gegevens. Dit biedt Azure-klanten de flexibiliteit om te kiezen welke oplossing het beste voldoet aan de behoeften van hun bedrijf. In dit artikel vindt we u een nieuwe technologieoplossing 'Azure Disk Encryption for Windows and Linux IaaS VM van' om te helpen beveiligen en bescherming van uw gegevens om te voldoen aan de beveiliging van de organisatie en de naleving verplichtingen. Het artikel biedt gedetailleerde richtlijnen over het gebruik van de Azure disk encryption functies met inbegrip van de ondersteunde scenario's en de gebruiker optreedt.

> [!NOTE]
> Bepaalde aanbevelingen mogelijk meer gegevens, netwerk of compute-Resourcegebruik, wat leidt tot extra kosten in licentie of abonnement.

## <a name="overview"></a>Overzicht
Azure Disk Encryption is een nieuwe functie waarmee u uw Windows- en Linux IaaS-schijven voor virtuele machine versleutelen. Azure Disk Encryption maakt gebruik van het industrie-initiatief [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows en de [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) functie van Linux voor volumeversleuteling voor het besturingssysteem en de gegevensschijven. De oplossing is geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te controleren en beheren van de schijfversleuteling sleutels en geheimen in uw abonnement sleutelkluis. De oplossing zorgt er ook voor dat alle gegevens op de virtuele machine-schijven zijn versleuteld in rust in uw Azure-opslag.

Versleuteling van de schijf van Azure voor Windows en Linux IaaS VM's wordt nu **algemene beschikbaarheid** in alle openbare Azure-regio en AzureGov regio's voor de standaard virtuele machines en virtuele machines met premium-opslag.

### <a name="encryption-scenarios"></a>Versleuteling scenario 's
De oplossing voor Azure Disk Encryption ondersteunt de volgende klant-scenario's:

* Schakel versleuteling in op de nieuwe IaaS VM's die worden gemaakt op basis van vooraf zijn versleuteld VHD- en versleutelingssleutels
* Schakel versleuteling in op nieuwe gemaakt op basis van installatiekopieën van het ondersteunde galerie van Azure IaaS VM 's
* Schakel versleuteling in op bestaande IaaS virtuele machines die worden uitgevoerd in Azure
* Schakel versleuteling in op IaaS VM's van Windows
* Versleuteling op gegevensstations voor Linux IaaS VM's uitschakelen
* Versleuteling van beheerde schijf virtuele machines inschakelen
* Instellingen voor codering van een bestaande versleutelde premium en niet voor de premium-opslag virtuele machine bijwerken
* Back-up en herstel van versleutelde virtuele machines

De oplossing ondersteunt de volgende scenario's voor IaaS VM's wanneer ze zijn ingeschakeld in Microsoft Azure:

* Integratie met Azure Sleutelkluis
* Standard-laag VMs: [A, D, DS, G, GS, F en enzovoort reeks IaaS VM's](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Schakel versleuteling in op Windows- en Linux IaaS VM's en beheerde schijven virtuele machines van de ondersteunde Azure-galerie afbeeldingen
* Versleuteling op besturingssysteem en stations voor Windows IaaS VM's en beheerde schijf-VM's uitschakelen
* Versleuteling op gegevensstations voor Linux IaaS VM's en beheerde schijf-VM's uitschakelen
* Schakel versleuteling in op IaaS VM's met Windows Client-besturingssysteem
* Schakel versleuteling in op volumes met koppelpunten paden
* Schakel versleuteling in op Linux VM's zijn geconfigureerd met schijf striping (RAID) met behulp van mdadm
* Schakel versleuteling in op de virtuele Linux-machines met behulp van LVM voor gegevensschijven
* Schakel versleuteling in op Linux LVM 7.3 voor besturingssysteem en gegevensschijven 
* Schakel versleuteling in op Windows VM's zijn geconfigureerd met opslagruimten
* Instellingen voor codering van een bestaande versleutelde premium en niet voor de premium-opslag virtuele machine bijwerken
* Back-up en herstel van versleutelde virtuele machines, voor zowel Nee KEK en KEK-scenario's (KEK - sleutel versleutelingssleutel)
* Alle openbare Azure en AzureGov regio's worden ondersteund

De oplossing biedt geen ondersteuning voor de volgende scenario's, functies en -technologie:

* Basisstaffel IaaS VM 's
* Het uitschakelen van Linux IaaS VM's op een station OS versleuteling
* Het uitschakelen van versleuteling op een gegevensstation als de OS-schijf versleuteld voor Linux Iaas VM 's
* IaaS VM's die zijn gemaakt met behulp van de methode voor het maken van klassieke VM
* Schakel versleuteling in op Windows en Linux-IaaS VM's klanten aangepaste installatiekopieën wordt niet ondersteund.
* Integratie met uw on-premises Key Management Service
* Azure Files (gedeelde bestandssysteem), Network File System (NFS), dynamische volumes en virtuele machines van Windows die zijn geconfigureerd met software gebaseerde RAID-systemen

### <a name="encryption-features"></a>Coderingsfuncties
Wanneer u inschakelen en implementeren van Azure Disk Encryption voor Azure IaaS VM's, worden de volgende mogelijkheden ingeschakeld, afhankelijk van de opgegeven configuratie:

* Versleuteling van het volume met het besturingssysteem voor het beveiligen van het opstartvolume in rust in uw opslagruimte
* Versleuteling van gegevensvolumes ter bescherming van de gegevensvolumes in rust in uw opslagruimte
* Het uitschakelen van IaaS VM's van Windows op de stations besturingssysteem en versleuteling
* Codering van gegevens uitschakelen schijven voor virtuele machines van Linux IaaS (alleen als het besturingssysteem IS niet versleuteld station)
* Beveiliging van de versleutelingssleutels en geheimen in uw abonnement sleutelkluis
* De coderingsstatus van de versleutelde IaaS VM rapportage
* Verwijderen van configuratie-instellingen van de virtuele machine voor IaaS-schijfversleuteling
* Back-up en herstel van versleutelde virtuele machines met behulp van de Azure Backup-service

Azure Disk Encryption voor IaaS VMS voor Windows en Linux-oplossing omvat:

* De extensie van de schijf-codering voor Windows.
* De extensie van de schijf-codering voor Linux.
* De schijf-codering PowerShell-cmdlets.
* De schijf-codering cmdlets van Azure-opdrachtregelinterface (CLI).
* De schijf-codering Azure Resource Manager-sjablonen.

De oplossing voor Azure Disk Encryption wordt ondersteund op IaaS VM's met Windows of Linux-besturingssysteem. Zie de sectie 'Vereisten' voor meer informatie over de ondersteunde besturingssystemen.

> [!NOTE]
> Er zijn geen extra kosten voor het versleutelen van VM-schijven met Azure Disk Encryption.

### <a name="value-proposition"></a>Waardevoorstel
Wanneer u de Azure Disk Encryption-management-oplossing toepast, kunt u voldoen aan de volgende zakelijke behoeften:

* IaaS VM's worden beveiligd in rust, omdat u versleuteling van industriestandaard technologie gebruiken kunt om de organisatorische vereisten voor beveiliging en naleving.
* Opstarten van IaaS VM's onder de klant beheerde sleutel en beleid en u kunt hun gebruik in de sleutelkluis controleren.

### <a name="encryption-workflow"></a>Codering-werkstroom
Het inschakelen van schijfversleuteling voor Windows en Linux-machines, het volgende doen:

1. Kies een versleutelingsscenario uit de bovenstaande scenario's voor versleuteling.
2. U meldt zich aan het inschakelen van schijfversleuteling via het Azure Disk Encryption Resource Manager-sjabloon, het PowerShell-cmdlets of de CLI-opdracht en geeft u de configuratie van de codering.

   * Voor de klant versleuteld VHD-scenario de versleutelde harde schijf geüpload naar uw opslagaccount en het sleutelmateriaal versleuteling aan de sleutelkluis. De configuratie van de versleuteling om Schakel versleuteling in op een nieuwe IaaS VM vervolgens opgeven.
   * Voor nieuwe virtuele machines die zijn gemaakt van de Marketplace en bestaande virtuele machines die al worden uitgevoerd in Azure, geeft u de configuratie van de versleuteling om Schakel versleuteling in op de IaaS VM.

3. Toegang verlenen tot de Azure-platform het materiaal versleutelingssleutel (versleutelingssleutels BitLocker voor Windows-systemen) en de wachtwoordzin voor Linux lezen uit de sleutelkluis versleuteling op de VM IaaS in te schakelen.

4. Geef de toepassings-id van de Azure Active Directory (Azure AD) voor het schrijven van de versleutelingssleutel materiaal naar de sleutelkluis. In dat geval schakelt u versleuteling op de IaaS-VM voor de scenario's die worden vermeld in stap 2.

5. Azure werkt bij het VM-servicemodel met versleuteling en de configuratie van de sleutelkluis en stelt uw versleutelde VM.

 ![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Werkstroom voor ontsleuteling
Als u wilt uitschakelen schijfversleuteling voor IaaS VM's, de volgende stappen op hoog niveau:

1. Kies versleuteling (decodering) uitschakelen op een actieve IaaS VM in Azure via de Azure Disk Encryption Resource Manager-sjabloon of het PowerShell-cmdlets en geef de configuratie voor ontsleuteling.

 Deze stap wordt de versleuteling van het besturingssysteem gegevensvolume en/of op het actieve Windows IaaS VM uitgeschakeld. Echter, zoals vermeld in de vorige sectie, het uitschakelen van Linux OS schijf-versleuteling wordt niet ondersteund. De ontsleuteling stap is alleen toegestaan voor gegevensstations op virtuele Linux-machines, zolang de besturingssysteemschijf is niet versleuteld.
2. Azure werkt het VM-servicemodel en IaaS VM ontsleutelde is gemarkeerd. De inhoud van de virtuele machine worden niet langer in rust versleuteld.

> [!NOTE]
> De bewerking uitschakelen versleuteling verwijdert niet de sleutelkluis en de versleuteling sleutelmateriaal (versleutelingssleutels BitLocker voor Windows-systemen) of de wachtwoordzin voor Linux.
 > Het uitschakelen van Linux OS schijf-versleuteling wordt niet ondersteund. De ontsleuteling stap is alleen toegestaan voor gegevensstations op virtuele Linux-machines.
Het uitschakelen van de schijf van gegevensversleuteling voor Linux wordt niet ondersteund als de OS-schijf is versleuteld.

## <a name="prerequisites"></a>Vereisten
Voordat u Azure Disk Encryption op Azure IaaS VM's inschakelen voor de ondersteunde scenario's die zijn beschreven in de sectie 'Overzicht', Zie de volgende vereisten:

* U moet een geldige actief Azure-abonnement maken van resources in Azure in de ondersteunde regio's hebben.
* Azure Disk Encryption wordt ondersteund op de volgende versies van Windows Server: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016.
* Azure Disk Encryption wordt ondersteund op de volgende Windows-clientversies: Windows 8 client en Windows 10-client.

> [!NOTE]
> Voor Windows Server 2008 R2, moet u .NET Framework 4.5 is geïnstalleerd voordat u Azure-versleuteling inschakelen hebben. U kunt deze installeren via Windows Update door de optionele update Microsoft .NET Framework 4.5.2 voor Windows Server 2008 R2 x64 64-systemen te installeren ([KB2901983](https://support.microsoft.com/kb/2901983)).

* Azure Disk Encryption wordt ondersteund op de volgende galerie van Azure op basis van Linux-server distributies en versies:

| Linux-distributie | Versie | Type van het volume voor de versleuteling wordt ondersteund|
| --- | --- |--- |
| Ubuntu | 16.04-DAILY-LTS | Besturingssysteem en schijf |
| Ubuntu | 14.04.5-DAILY-LTS | Besturingssysteem en schijf |
| Ubuntu | 12.10 | Gegevensschijf |
| Ubuntu | 12.04 | Gegevensschijf |
| RHEL | 7.4 | Besturingssysteem en schijf |
| RHEL | 7.3 | Besturingssysteem en schijf |
| RHEL | LVM 7.3 | Besturingssysteem en schijf |
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
| SLES | 12-SP1 (Premium) | Gegevensschijf |
| SLES | HPC 12 | Gegevensschijf |
| SLES | 11-SP4 (Premium) | Gegevensschijf |
| SLES | 11 SP4 | Gegevensschijf |

* Azure Disk Encryption is vereist dat uw sleutelkluis en de virtuele machines zich in de dezelfde Azure-regio en het abonnement bevinden.

> [!NOTE]
> Configureren van de resources in afzonderlijke regio's zorgt ervoor dat een fout opgetreden bij het inschakelen van de functie Azure Disk Encryption.

* Als u wilt installeren en configureren van de sleutelkluis voor Azure Disk Encryption, Zie de sectie **instellen boven en uw sleutelkluis configureren voor Azure Disk Encryption** in de *vereisten* sectie van dit artikel.
* Als u wilt installeren en configureren van Azure AD-toepassing in Azure Active directory voor Azure Disk Encryption, Zie de sectie **instellen van de Azure AD-toepassing in Azure Active Directory** in de *vereisten* sectie in dit artikel.
* Als u wilt installeren en configureren van de sleutelkluis-toegangsbeleid voor de Azure AD-toepassing, Zie de sectie **de sleutelkluis-beleid instellen voor de Azure AD-toepassing** in de *vereisten* sectie van dit artikel.
* Als u een Windows-VHD vooraf zijn versleuteld, Zie de sectie **voorbereiden van een vooraf zijn versleuteld Windows VHD** in de *bijlage*.
* Als u een vooraf zijn versleuteld Linux VHD, Zie de sectie **voorbereiden van een vooraf zijn versleuteld Linux VHD** in de *bijlage*.
* De Azure-platform moet toegang tot de versleutelingssleutels of geheimen in de sleutelkluis zodat ze beschikbaar zijn op de virtuele machine is wanneer deze wordt opgestart en het besturingssysteemvolume virtuele machine ontsleutelt. Om machtigingen te verlenen voor Azure-platform, stel de **EnabledForDiskEncryption** eigenschap in de sleutelkluis. Zie voor meer informatie **instellen boven en uw sleutelkluis configureren voor Azure Disk Encryption** in de bijlage.
* Uw sleutelkluis geheim en de KEK-URL's moeten samengestelde zijn. Azure zorgt ervoor dat deze beperking versiebeheer. Zie de volgende voorbeelden voor geldige geheim en KEK-URL's:

  * Voorbeeld van een geldige URL geheime: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Voorbeeld van een geldige KEK-URL: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk Encryption biedt geen ondersteuning voor het opgeven van poortnummers als onderdeel van de sleutelkluis geheimen en KEK-URL's. Voor voorbeelden van niet-ondersteunde en ondersteunde sleutelkluis-URL's, Zie de volgende:

  * Onaanvaardbaar sleutelkluis-URL *https://contosovault.vault.azure.net:443/geheimen/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Acceptabele sleutelkluis-URL: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Als u wilt inschakelen, de Azure Disk Encryption functie, de IaaS VM's moet voldoen aan de volgende configuratievereisten voor netwerk-eindpunt:
  * Als u een token voor verbinding maken met uw sleutelkluis, de IaaS VM moet verbinding maken met een Azure Active Directory-eindpunt \[login.microsoftonline.com\].
  * Als de versleutelingssleutels opslaan op uw sleutelkluis, moet de IaaS VM verbinding kunnen maken met het eindpunt van de sleutelkluis.
  * De IaaS VM moet verbinding maken met een Azure-opslag-eindpunt dat als host fungeert voor de extensie Azure-opslagplaats en een Azure storage-account dat als host fungeert voor de VHD-bestanden.

  > [!NOTE]
  > Als het beveiligingsbeleid van uw Azure VM's toegang heeft tot Internet beperkt, kunt u de voorgaande URI oplossen en configureren van een specifieke regel voor het toestaan van uitgaande verbinding met de IP-adressen.
  >
  >Om te configureren en toegang tot Azure Key Vault achter een firewall (https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall)

* Gebruik de nieuwste versie van Azure PowerShell SDK-versie van Azure Disk Encryption te configureren. Download de nieuwste versie van [Azure PowerShell-versie](https://github.com/Azure/azure-powershell/releases)

 > [!NOTE]
  > Azure Disk Encryption wordt niet ondersteund op [Azure PowerShell SDK-versie 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Als u ontvangt een foutbericht over het gebruik van Azure PowerShell 1.1.0, raadpleegt u [Azure schijf versleuteling fout met betrekking tot Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

* Als u wilt willekeurige opdracht van de Azure CLI uitgevoerd en deze koppelen aan uw Azure-abonnement, moet u eerst de Azure CLI installeren:
  * Zie wilt Azure CLI installeren en deze koppelen aan uw Azure-abonnement, [installeren en configureren van Azure CLI](../cli-install-nodejs.md).
  * Zie voor het gebruik van Azure CLI voor Mac, Linux en Windows Azure Resource Manager, [Azure CLI-opdrachten in de modus Resource Manager](../virtual-machines/azure-cli-arm-commands.md).

* Bij het versleutelen van een beheerde schijf is verplicht vereiste voor het uitvoeren van een momentopname van de beheerde schijf of een back-up van de schijf buiten Azure Disk Encryption voorafgaand aan de codering inschakelen.  Zonder een back-up op locatie, ervoor een onverwachte fout opgetreden tijdens het versleutelen zorgen dat de schijf en de VM toegankelijk zijn zonder een hersteloptie.  Set-AzureRmVMDiskEncryptionExtension momenteel geen back-up beheerde schijven en wordt fout als op een beheerde schijf gebruikt, tenzij de parameter - skipVmBackup is opgegeven.  Deze parameter is niet veilig om te gebruiken, tenzij een back-up al is gemaakt buiten Azure Disk Encryption.   Als de parameter - skipVmBackup wordt opgegeven, wordt de cmdlet een back-up van de beheerde schijf vóór versleuteling niet maken.  Daarom wordt het beschouwd als een verplichte vereisten om te controleren of een back-up van de beheerde schijf dat VM is geïmplementeerd voordat u Azure Disk Encryption inschakelt in geval herstel later nodig.  
> [!NOTE]
 > De parameter - skipVmBackup mag nooit worden gebruikt tenzij een momentopname of een back-up al is gemaakt buiten Azure Disk Encryption. 

* De oplossing voor Azure Disk Encryption maakt gebruik van de externe BitLocker-sleutelbeveiliging voor IaaS VM's van Windows. Push Groepsbeleid dat TPM beveiligingen afdwingen voor domein gekoppelde virtuele machines, niet. Zie voor meer informatie over het groepsbeleid voor 'Toestaan dat BitLocker zonder een compatibele TPM' [BitLocker Group Policy Reference](https://technet.microsoft.com/library/ee706521).
* BitLocker-Groepsbeleid op virtuele machines die lid van domein voor de aangepaste vergezeld gaan van de volgende instelling: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption mislukken wanneer aangepaste groepsbeleidsinstellingen voor Bitlocker niet compatibel zijn. Op computers die niet het juiste beleid instelling, het nieuwe beleid wordt toegepast, waardoor het nieuwe beleid om bij te werken (gpupdate.exe/Force) en vervolgens opnieuw te starten mogelijk zijn vereist.  
* Een Azure AD-toepassing maken, een sleutelkluis maken of een bestaande sleutelkluis instellen en versleuteling inschakelen, raadpleegt de [vereiste PowerShell-script voor Azure Disk Encryption](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
* Als u wilt configureren met de Azure CLI-schijfversleuteling-vereisten, Zie [dit script Bash](https://github.com/ejarvi/ade-cli-getting-started).
* Als u de Azure Backup-service wilt back-up en herstel van versleutelde virtuele machines, als versleuteling is ingeschakeld met Azure Disk Encryption, uw virtuele machines te versleutelen met behulp van de configuratie van de Azure Disk Encryption. De Backup-service biedt ondersteuning voor virtuele machines die zijn versleuteld met Nee KEK of KEK-configuraties. Zie [back-up en herstellen versleuteld virtuele machines met Azure Backup-versleuteling](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

* Bij het versleutelen van een volume met het Linux-besturingssysteem, houd er rekening mee dat VM moet worden opgestart op dat moment aan het einde van het proces. Dit kan worden gedaan via de portal, powershell of CLI.   Om de voortgang van versleuteling volgen, het statusbericht dat is geretourneerd door Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus periodiek te pollen.  Zodra de codering is voltooid, de het statusbericht dat is geretourneerd door deze opdracht wordt dit aangegeven.  Bijvoorbeeld ' ProgressMessage: besturingssysteemschijf is versleuteld, start u de virtuele machine ' op dit moment de virtuele machine kan worden gestart en gebruikt.  

* Azure Disk Encryption voor Linux gegevensschijven bevatten een gekoppeld bestandssysteem Linux vóór versleuteling is vereist

* Recursief gekoppelde schijven worden niet ondersteund door de Azure Disk Encryption voor Linux. Als bijvoorbeeld het doelsysteem een schijf is gekoppeld op foo/balk en slaagt op /foo/bar/baz, de versleuteling van /foo/bar/baz, maar de versleuteling van foo/balk mislukken. 

* Azure Disk Encryption wordt alleen ondersteund op Azure-galerie ondersteund installatiekopieën die voldoen aan de hiervoor genoemde vereisten. Klanten aangepaste installatiekopieën worden niet ondersteund als gevolg van proces-gedrag die mogelijk aanwezig zijn op deze installatiekopieën en aangepaste partitieschema's. Bovendien is zelfs afbeelding op basis van de virtuele machine die in eerste instantie vereisten wordt voldaan, maar zijn gewijzigd nadat het maken van zijn mogelijk niet compatibel.  Voor die is daarom moet de aanbevolen procedure voor het versleutelen van een Linux-VM starten vanuit een schone afbeelding, versleutelen van de virtuele machine en vervolgens aangepaste software of gegevens toevoegen aan de virtuele machine zo nodig.  

* Azure Disk Encryption en lokale gegevensvolume - Bek Volume voor Windows en mnt/azure_bek_disk voor Linux IaaS VM's voor het veilig opslaan van de versleutelingssleutel. Verwijder of bewerken van alle inhoud van deze schijf niet. De schijf niet worden ontkoppeld omdat de belangrijkste aanwezigheid versleuteling is vereist voor alle versleutelingsbewerkingen op de IaaS-VM. Leesmij-bestanden die zijn opgenomen in het volume bevat aanvullende informatie.

#### <a name="set-up-the-azure-ad-application-in-azure-active-directory"></a>Instellen van de Azure AD-toepassing in Azure Active Directory
U moet de versleuteling zijn ingeschakeld op een actieve virtuele machine in Azure, Azure Disk Encryption genereert als de versleutelingssleutels schrijft naar de sleutelkluis. Het beheren van versleutelingssleutels in de sleutelkluis vereist Azure AD-verificatie.

Maak een Azure AD-toepassing voor dit doel. U vindt gedetailleerde stappen voor het registreren van een toepassing in de sectie 'Een identiteit voor de toepassing ophalen' van het blogbericht [Azure Sleutelkluis - stapsgewijze](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Dit bericht bevat ook een aantal handige voorbeelden voor het instellen en configureren van de sleutelkluis. U kunt op basis van een geheim clientverificatie of clientverificatie Azure AD op basis van certificaten gebruiken voor verificatiedoeleinden.

#### <a name="client-secret-based-authentication-for-azure-ad"></a>Clientverificatie op basis van een geheim voor Azure AD
De volgende secties kunt u een client geheim gebaseerde verificatie voor Azure AD configureren.

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a>Een Azure AD-toepassing maken met behulp van Azure PowerShell
Gebruik de volgende PowerShell-cmdlet voor het maken van een Azure AD-toepassing:

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> $azureAdApplication.ApplicationId is de Azure AD-ClientID en $aadClientSecret het clientgeheim die u later gebruiken moet om Azure Disk Encryption is. De Azure AD-clientgeheim op de juiste wijze beveiligt.

##### <a name="setting-up-the-azure-ad-client-id-and-secret-from-the-azure-portal"></a>Instellen van het Azure AD-client-ID en het geheim van de Azure-portal
U kunt ook van uw Azure AD-client-ID en geheim instellen met behulp van de Azure-Portal. Ga als volgt te werk als u wilt uitvoeren van deze taak:

1. Klik op de **Active Directory** tabblad.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. Klik op **toepassing toevoegen**, en typ vervolgens de naam van de toepassing.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. Klik op de pijl en configureer vervolgens de eigenschappen van de toepassing.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. Klik op het vinkje in de linkerbenedenhoek te voltooien. De configuratiepagina van de toepassing wordt weergegeven en de Azure AD-client-ID aan de onderkant van de pagina wordt weergegeven.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. Opslaan van de Azure AD-clientgeheim door te klikken op de **opslaan** knop. Let op het Azure AD-clientgeheim in het tekstvak sleutels. Op de juiste wijze beveiligt.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig7.png)


##### <a name="use-an-existing-application"></a>Gebruik een bestaande toepassing
De volgende opdrachten worden uitgevoerd, downloadt en gebruikt de [Azure AD PowerShell-module](https://technet.microsoft.com/library/jj151815.aspx).

> [!NOTE]
> De volgende opdrachten moeten worden uitgevoerd vanuit een nieuwe PowerShell-venster. Gebruik geen Azure PowerShell of het venster Azure Resource Manager de opdrachten uitvoeren. Deze aanpak wordt aangeraden omdat deze cmdlets in de MSOnline module of Azure AD PowerShell.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Verificatie op basis van certificaten voor Azure AD
> [!NOTE]
> Azure AD gebaseerde verificatie is momenteel niet ondersteund op virtuele Linux-machines.

De volgende secties laten zien hoe een verificatie op basis van certificaten voor Azure AD configureren.

##### <a name="create-an-azure-ad-application"></a>Een Azure AD-toepassing maken
Uitvoeren van de volgende PowerShell-cmdlets voor het maken van een Azure AD-toepassing:

> [!NOTE]
> Vervang de volgende `yourpassword` tekenreeks met uw beveiligd wachtwoord en het wachtwoord te beveiligen.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Nadat u deze stap, een PFX-bestand uploaden naar de sleutelkluis en het beleid voor toegang nodig is om te implementeren dat certificaat voor een virtuele machine inschakelen.

##### <a name="use-an-existing-azure-ad-application"></a>Gebruik een bestaande Azure AD-toepassing
Als u verificatie op basis van certificaten voor een bestaande toepassing configureren wilt, gebruikt u de PowerShell-cmdlets die hier worden weergegeven. Zorg ervoor dat deze van een nieuwe PowerShell-venster uitvoeren.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Nadat u deze stap, een PFX-bestand uploaden naar de sleutelkluis en het beleid dat nodig is voor het implementeren van het certificaat op een virtuele machine inschakelen.

##### <a name="upload-a-pfx-file-to-your-key-vault"></a>Een PFX-bestand uploaden naar de sleutelkluis
Zie voor een gedetailleerde uitleg van dit proces [de officiële Azure Key Vault teamblog](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx). De volgende PowerShell-cmdlets zijn echter alles wat die u nodig hebt voor de taak. Zorg ervoor dat ze van Azure PowerShell-console uitvoeren.

> [!NOTE]
> Vervang de volgende `yourpassword` tekenreeks met uw beveiligd wachtwoord en het wachtwoord te beveiligen.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-your-key-vault-to-an-existing-vm"></a>Een certificaat in de sleutelkluis implementeren naar een bestaande virtuele machine
Nadat u de PFX uploaden, moet u een certificaat in de sleutelkluis implementeren naar een bestaande virtuele machine met de volgende opties:
 ```
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName
 ```

#### <a name="set-up-the-key-vault-access-policy-for-the-azure-ad-application"></a>De sleutelkluis-beleid voor de Azure AD-toepassing instellen
Uw Azure AD-toepassing moet rechten voor toegang tot de sleutels of geheimen in de kluis. Gebruik de [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) cmdlet machtigen om de toepassing, de client-ID (die is gegenereerd toen de toepassing is geregistreerd) als de _– ServicePrincipalName_ parameterwaarde. Zie voor meer informatie het blogbericht [Azure Sleutelkluis - stapsgewijze](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Hier volgt een voorbeeld van hoe deze taak via PowerShell uit te voeren:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> Azure Disk Encryption moet u de volgende beleidsregels voor toegang op de clienttoepassing van uw Azure AD configureren: _WrapKey_ en _ingesteld_ machtigingen.

## <a name="terminology"></a>Terminologie
Om te begrijpen enkele van de algemene voorwaarden die door deze technologie gebruikt, gebruik de volgende terminologie-tabel:

| Terminologie | Definitie |
| --- | --- |
| Azure AD | Azure AD [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Een Azure AD-account is een vereiste voor verificatie, opslaan en ophalen van geheimen van een sleutelkluis. |
| Azure Key Vault | Sleutelkluis is een cryptografische, key management service die gebaseerd op de Federal Information Processing Standards FIPS-gevalideerde hardware security modules, die ter bescherming van uw cryptografische sleutels en geheimen gevoelige. Zie voor meer informatie [Sleutelkluis](https://azure.microsoft.com/services/key-vault/) documentatie. |
| ARM | Azure Resource Manager |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is een industrie herkend Windows volume versleuteling technologie die wordt gebruikt om in te schakelen schijfversleuteling op IaaS VM's van Windows. |
| BEK | BitLocker-versleutelingssleutels worden gebruikt voor het versleutelen van het opstartvolume OS en gegevensvolumes. De BitLocker-sleutels worden beveiligd in een sleutelkluis als geheimen. |
| CLI | Zie [Azure-opdrachtregelinterface](../cli-install-nodejs.md). |
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is het subsysteem voor op basis van Linux, transparant schijfversleuteling die wordt gebruikt voor het inschakelen van schijfversleuteling op Linux IaaS VM's. |
| KEK | Sleutelcodering sleutel is de asymmetrische sleutel (RSA 2048) die u gebruiken kunt om te beveiligen of het geheim inpakken. U kunt bieden een hardware security modules (HSM)-sleutel of met software beschermde sleutel beveiligd. Zie voor meer informatie [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentatie. |
| PS-cmdlets | Zie [Azure PowerShell-cmdlets](/powershell/azure/overview). |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a>Installeren en configureren van de sleutelkluis voor Azure Disk Encryption
Azure Disk Encryption helpt de de schijfversleuteling sleutels en geheimen in de sleutelkluis. Als u de sleutelkluis voor Azure Disk Encryption instelt, voltooi de stappen in elk van de volgende secties.

#### <a name="create-a-key-vault"></a>Een sleutelkluis maken
Als u wilt een sleutelkluis maken, gebruikt u een van de volgende opties:

* [Resource Manager-sjabloon '101-sleutel-kluis-maken'](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [Azure PowerShell-cmdlets voor sleutelkluis](/powershell/module/azurerm.keyvault/#key_vault)
* Azure Resource Manager
* Hoe [beveiligen van uw sleutelkluis](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)

> [!NOTE]
> Als u hebt al een sleutelkluis ingesteld voor uw abonnement, gaat u naar de volgende sectie.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a>Instellen van een coderingssleutel voor de sleutel (optioneel)
Als u een KEK-sleutel voor een extra beveiligingslaag voor de versleutelingssleutels BitLocker gebruiken wilt, moet u een KEK toevoegen aan de sleutelkluis. Gebruik de [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet voor het maken van een coderingssleutel voor de sleutel in de sleutelkluis. U kunt ook een KEK importeren uit uw lokale Sleutelbeheer HSM. Zie voor meer informatie [Sleutelkluis documentatie](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

U kunt de KEK toevoegen door te gaan in Azure Resource Manager of met behulp van de sleutelkluis-interface.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a>Sleutelkluis-machtigingen instellen
De Azure-platform moet toegang tot de versleutelingssleutels of geheimen in de sleutelkluis zodat ze beschikbaar met de virtuele machine voor het opstarten en ontsleutelen van de volumes. Om machtigingen te verlenen voor het Azure-platform, stel de **EnabledForDiskEncryption** eigenschap in de sleutel met behulp van de sleutelkluis PowerShell-cmdlet-kluis:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

U kunt ook instellen de **EnabledForDiskEncryption** eigenschap in via de [Azure Resource Explorer](https://resources.azure.com).

Zoals eerder vermeld, stelt u de **EnabledForDiskEncryption** eigenschap in de sleutelkluis. Anders mislukt de implementatie.

U kunt beleidsregels voor toegang instellen voor uw Azure AD-toepassing van de sleutelkluis-interface, zoals hier wordt weergegeven:

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Op de **geavanceerde toegangsbeleid** tabblad, zorg ervoor dat de sleutelkluis voor Azure Disk Encryption is ingeschakeld:

![Azure sleutelkluis](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Implementatiescenario's voor schijf-codering en gebruikerservaringen
U kunt veel scenario's voor schijf-codering inschakelen en de stappen kunnen variëren afhankelijk van het scenario. De volgende secties hebben betrekking op de scenario's met meer details.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-the-marketplace"></a>Schakel versleuteling in op de nieuwe IaaS VM's die zijn gemaakt van de Marketplace
U kunt schijfversleuteling op nieuwe IaaS virtuele machine van Windows uit in Azure Marketplace inschakelen met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

1. Klik op de Azure snel starten-sjabloon **implementeren in Azure**, voer de configuratie van de versleuteling in op de **Parameters** blade en klik vervolgens op **OK**.

2. Selecteer het abonnement, resourcegroep locatie voor resourcegroep, juridische voorwaarden en overeenkomst en klik vervolgens op **maken** versleuteling op een nieuwe IaaS VM in te schakelen.

> [!NOTE]
> Deze sjabloon maakt een nieuwe versleutelde Windows virtuele machine die gebruikmaakt van de installatiekopie van het Windows Server 2012-galerie.

U kunt schijfversleuteling op een nieuwe IaaS RedHat Linux 7.2 virtuele machine met een matrix van 200 GB RAID-0 inschakelen met behulp van dit [Resource Manager-sjabloon](https://aka.ms/fde-rhel). Nadat u de sjabloon hebt geïmplementeerd, de status van de versleuteling VM controleren met behulp van de `Get-AzureRmVmDiskEncryptionStatus` cmdlet, zoals beschreven in [OS versleutelen station op een actieve Linux VM](#encrypting-os-drive-on-a-running-linux-vm). Wanneer de machine de status retourneert _VMRestartPending_, opnieuw opstarten van de virtuele machine.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor de nieuwe virtuele machines van de Marketplace-scenario met behulp van Azure AD-client-ID:

| Parameter | Beschrijving |
| --- | --- |
| adminUserName | De beheerdersgebruikersnaam voor de virtuele machine. |
| adminPassword | Beheerderswachtwoord voor de virtuele machine. |
| newStorageAccountName | De naam van het opslagaccount voor het besturingssysteem en VHD's opslaan. |
| vmSize | De grootte van de virtuele machine. Op dit moment worden alleen standaard A, D en G reeksen ondersteund. |
| virtualNetworkName | Naam van het VNet dat de VM-NIC tot behoren moet. |
| subnetName | Naam van het subnet in het VNet dat de VM-NIC tot behoren moet. |
| AADClientID | Client-ID van de Azure AD-toepassing die machtigingen heeft voor het schrijven van geheimen naar de sleutelkluis. |
| AADClientSecret | Clientgeheim van de Azure AD-toepassing die machtigingen heeft voor het schrijven van geheimen naar de sleutelkluis. |
| keyVaultURL | URL van de sleutelkluis die de BitLocker-sleutel moet worden geüpload naar. U kunt dit downloaden via de cmdlet `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`. |
| keyEncryptionKeyURL | URL van de belangrijkste versleutelingssleutel die wordt gebruikt voor het versleutelen van de gegenereerde BitLocker-sleutel (optioneel). |
| keyVaultResourceGroup | De resourcegroep van de sleutelkluis. |
| vmName | Naam van de virtuele machine die de versleutelingsbewerking op worden uitgevoerd. |

> [!NOTE]
> _KeyEncryptionKeyURL_ is een optionele parameter. U kunt brengt uw eigen KEK voor verdere beveiliging de gegevensversleutelingssleutel (geheime wachtwoordzin) in de sleutelkluis.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Schakel versleuteling in op de nieuwe IaaS VM's die zijn gemaakt van de klant versleuteld VHD- en versleutelingssleutels
In dit scenario kunt u inschakelen versleutelen met behulp van de Resource Manager-sjabloon, het PowerShell-cmdlets of de CLI-opdrachten. De volgende secties wordt uitgelegd in meer detail de Resource Manager-sjabloon en de CLI-opdrachten.

Volg de instructies uit een van deze secties voor het voorbereiden van vooraf zijn versleuteld afbeeldingen die kunnen worden gebruikt in Azure. Nadat de installatiekopie is gemaakt, kunt u de stappen in de volgende sectie voor het maken van een versleutelde Azure VM.

* [Een vooraf zijn versleuteld Windows VHD voorbereiden](#preparing-a-pre-encrypted-windows-vhd)
* [Een vooraf zijn versleuteld Linux VHD voorbereiden](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-the-resource-manager-template"></a>Met behulp van de Resource Manager-sjabloon
U kunt schijfversleuteling op uw versleutelde VHD inschakelen met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. Klik op de Azure snel starten-sjabloon **implementeren in Azure**, voer de configuratie van de versleuteling in op de **Parameters** blade en klik vervolgens op **OK**.

2. Selecteer het abonnement, resourcegroep locatie voor resourcegroep, juridische voorwaarden en overeenkomst en klik vervolgens op **maken** versleuteling op de nieuwe IaaS VM in te schakelen.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor de versleutelde VHD:

| Parameter | Beschrijving |
| --- | --- |
| newStorageAccountName | De naam van het opslagaccount voor het opslaan van de versleutelde OS-VHD. Dit opslagaccount moet al zijn gemaakt in dezelfde resourcegroep en dezelfde locatie als de virtuele machine. |
| osVhdUri | De URI van de VHD OS vanuit het opslagaccount. |
| osType | Type besturingssysteem product (Windows of Linux). |
| virtualNetworkName | Naam van het VNet dat de VM-NIC tot behoren moet. De naam moet al zijn gemaakt in dezelfde resourcegroep en dezelfde locatie als de virtuele machine. |
| subnetName | Naam van het subnet in het VNet dat de VM-NIC tot behoren moet. |
| vmSize | De grootte van de virtuele machine. Op dit moment worden alleen standaard A, D en G reeksen ondersteund. |
| keyVaultResourceID | De ResourceID die de sleutelkluis-resource in Azure Resource Manager identificeert. U kunt dit downloaden met behulp van de PowerShell-cmdlet `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`. |
| keyVaultSecretUrl | De URL van de schijf-versleutelingssleutel die ingesteld in de sleutelkluis. |
| keyVaultKekUrl | De URL van de belangrijkste coderingssleutel voor het versleutelen van de versleutelingssleutel van de gegenereerde schijf. |
| vmName | Naam van de IaaS-VM. |

#### <a name="using-powershell-cmdlets"></a>Met behulp van PowerShell-cmdlets
U kunt schijfversleuteling op uw versleutelde VHD inschakelen met behulp van de PowerShell-cmdlet [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).  

#### <a name="using-cli-commands"></a>CLI-opdrachten gebruiken
Schakel schijfversleuteling voor dit scenario met behulp van de CLI-opdrachten door het volgende doen:

1. Toegangsbeleid instellen in de sleutelkluis:

   * Stel de **EnabledForDiskEncryption** vlag:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Stel machtigingen in op Azure AD-toepassing geheimen schrijven naar de sleutelkluis:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Het inschakelen van versleuteling op een bestaande of actieve virtuele machine, typt u:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Versleutelingsstatus ophalen:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Gebruik de volgende parameters met het inschakelen van versleuteling op een nieuwe virtuele machine van de versleutelde VHD, de `azure vm create` opdracht:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Schakel versleuteling in op de bestaande of IaaS virtuele Windows-machine in Azure wordt uitgevoerd
In dit scenario kunt u inschakelen versleutelen met behulp van de Resource Manager-sjabloon, het PowerShell-cmdlets of de CLI-opdrachten. De volgende secties wordt uitgelegd in meer detail hoe in te schakelen met behulp van de Resource Manager-sjabloon en de CLI-opdrachten.

#### <a name="using-the-resource-manager-template"></a>Met behulp van de Resource Manager-sjabloon
U kunt inschakelen schijfversleuteling op bestaande of IaaS VM's van Windows worden uitgevoerd in Azure met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).

1. Klik op de Azure snel starten-sjabloon **implementeren in Azure**, voer de configuratie van de versleuteling in op de **Parameters** blade en klik vervolgens op **OK**.

2. Selecteer het abonnement, resourcegroep locatie voor resourcegroep, juridische voorwaarden en overeenkomst en klik vervolgens op **maken** versleuteling op de bestaande of actief IaaS VM in te schakelen.

De volgende tabel bevat de Resource Manager-Sjabloonparameters voor bestaande of VM's die gebruikmaken van een client-ID van Azure AD worden uitgevoerd:

| Parameter | Beschrijving |
| --- | --- |
| AADClientID | Client-ID van de Azure AD-toepassing die gemachtigd is te schrijven geheimen tot de sleutelkluis. |
| AADClientSecret | Clientgeheim van de Azure AD-toepassing die gemachtigd is te schrijven geheimen tot de sleutelkluis. |
| keyVaultName | Naam van de sleutelkluis die de BitLocker-sleutel moet worden geüpload naar. U kunt dit downloaden via de cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | URL van de belangrijkste versleutelingssleutel die wordt gebruikt voor het versleutelen van de gegenereerde BitLocker-sleutel. Deze parameter is optioneel als u selecteert **nokek** in de vervolgkeuzelijst UseExistingKek. Als u selecteert **kek** in de vervolgkeuzelijst UseExistingKek moet u de _keyEncryptionKeyURL_ waarde. |
| volumeType | Het type van het volume dat de coderingsbewerking wordt uitgevoerd op. Geldige waarden zijn _OS_, _gegevens_, en _alle_. |
| sequenceVersion | De versie van de volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens wanneer een schijfversleuteling bewerking wordt uitgevoerd op dezelfde virtuele machine. |
| vmName | Naam van de virtuele machine die de versleutelingsbewerking op worden uitgevoerd. |

> [!NOTE]
> _KeyEncryptionKeyURL_ is een optionele parameter. U kunt uw eigen KEK de gegevensversleutelingssleutel (geheim BitLocker-versleuteling) in de sleutelkluis om verdere beveiliging te zetten.

#### <a name="using-powershell-cmdlets"></a>Met behulp van PowerShell-cmdlets
Zie voor informatie over het inschakelen van versleuteling met Azure Disk Encryption met behulp van PowerShell-cmdlets, de blogberichten [Azure Disk Encryption verkennen met Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) en [Azure Disk Encryption verkennen Deel 2 met Azure PowerShell -](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

#### <a name="using-cli-commands"></a>CLI-opdrachten gebruiken
Voor het inschakelen van versleuteling op bestaande of IaaS virtuele machine van Windows worden uitgevoerd in Azure CLI-opdrachten met het volgende doen:

1. Toegangsbeleid instellen in de sleutelkluis:
   * Stel de **EnabledForDiskEncryption** vlag:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Stel machtigingen in op Azure AD-toepassing geheimen schrijven naar de sleutelkluis:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. Schakel versleuteling in op een bestaande of actieve virtuele machine:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. Coderingsstatus ophalen:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. Gebruik de volgende parameters met het inschakelen van versleuteling op een nieuwe virtuele machine van de versleutelde VHD, de `azure vm create` opdracht:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a>Schakel versleuteling in op een bestaande of actief IaaS virtuele Linux-machine in Azure
U kunt schijfversleuteling op een bestaande of actief IaaS Linux virtuele machine in Azure inschakelen met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. Klik op **implementeren in Azure** op de sjabloon voor Azure snel starten, voert u de configuratie van de versleuteling op de **Parameters** blade en klik vervolgens op **OK**.

2. Selecteer het abonnement, resourcegroep locatie voor resourcegroep, juridische voorwaarden en overeenkomst en klik vervolgens op **maken** versleuteling op de bestaande of actief IaaS VM in te schakelen.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor bestaande of VM's die gebruikmaken van een client-ID van Azure AD worden uitgevoerd:

| Parameter | Beschrijving |
| --- | --- |
| AADClientID | Client-ID van de Azure AD-toepassing die gemachtigd is te schrijven geheimen tot de sleutelkluis. |
| AADClientSecret | Clientgeheim van de Azure AD-toepassing die machtigingen heeft voor het schrijven van geheimen naar de sleutelkluis. |
| keyVaultName | Naam van de sleutelkluis die de BitLocker-sleutel moet worden geüpload naar. U kunt dit downloaden via de cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | URL van de belangrijkste versleutelingssleutel die wordt gebruikt voor het versleutelen van de gegenereerde BitLocker-sleutel. Deze parameter is optioneel als u selecteert **nokek** in de vervolgkeuzelijst UseExistingKek. Als u selecteert **kek** in de vervolgkeuzelijst UseExistingKek moet u de _keyEncryptionKeyURL_ waarde. |
| volumeType | Het type van het volume dat de coderingsbewerking wordt uitgevoerd op. Geldige ondersteunde waarden zijn _OS_ of _alle_ (Zie ondersteunde Linux-distributies en versies voor het besturingssysteem en gegevensschijven in prerequisiteis sectie eerder). |
| sequenceVersion | De versie van de volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens wanneer een schijfversleuteling bewerking wordt uitgevoerd op dezelfde virtuele machine. |
| vmName | Naam van de virtuele machine die de versleutelingsbewerking op worden uitgevoerd. |
| passPhrase | Typ een sterke wachtwoordzin als de gegevensversleutelingssleutel. |

> [!NOTE]
> _KeyEncryptionKeyURL_ is een optionele parameter. U kunt brengt uw eigen KEK voor verdere beveiliging de gegevensversleutelingssleutel (geheime wachtwoordzin) in de sleutelkluis.

#### <a name="cli-commands"></a>CLI-opdrachten
U kunt schijfversleuteling inschakelen op uw versleutelde VHD te installeren en gebruiken de [CLI opdracht](../cli-install-nodejs.md). Ga als volgt te werk voor het inschakelen van versleuteling op bestaande of IaaS Linux VM's worden uitgevoerd in Azure met behulp van de CLI-opdrachten:

1. Toegangsbeleid instellen in de sleutelkluis:

 * Stel de **EnabledForDiskEncryption** vlag:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * Stel machtigingen in op Azure AD-toepassing geheimen schrijven naar de sleutelkluis:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Schakel versleuteling in op een bestaande of actieve virtuele machine:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Versleutelingsstatus ophalen:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Gebruik de volgende parameters met het inschakelen van versleuteling op een nieuwe virtuele machine van de versleutelde VHD, de `azure vm create` opdracht:
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-the-encryption-status-of-an-encrypted-iaas-vm"></a>De coderingsstatus van een versleutelde IaaS VM ophalen
U kunt de coderingsstatus ophalen met behulp van Azure Resource Manager [PowerShell-cmdlets](/powershell/azure/overview), of de CLI-opdrachten. De volgende secties wordt uitgelegd hoe de Azure Portal en de CLI-opdrachten gebruiken voor de coderingsstatus.

#### <a name="get-the-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a>De coderingsstatus van een versleutelde Windows VM ophalen met behulp van Azure Resource Manager
U kunt de coderingsstatus van IaaS VM van Azure Resource Manager krijgen als volgt:

1. Aanmelden bij de [Azure Portal](https://portal.azure.com/), en klik vervolgens op **virtuele machines** in het linkerdeelvenster om te zien van een samenvatting weergegeven van de virtuele machines in uw abonnement. U kunt de virtuele machines weergave filteren door de naam van het abonnement in de **abonnement** vervolgkeuzelijst.

2. Aan de bovenkant van de **virtuele machines** pagina, klikt u op **kolommen**.

3. Op de **Kies kolom** blade Selecteer **schijfversleuteling**, en klik vervolgens op **Update**. U ziet de schijfversleuteling kolom met de coderingsstatus _ingeschakeld_ of _niet ingeschakeld_ voor elke virtuele machine, zoals wordt weergegeven in de volgende afbeelding:

 ![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-the-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-the-disk-encryption-powershell-cmdlet"></a>De coderingsstatus van een versleutelde (Windows of Linux) IaaS VM ophalen met behulp van de PowerShell-cmdlet schijfversleuteling
U kunt de coderingsstatus van IaaS VM ophalen uit de PowerShell-cmdlet schijfversleuteling `Get-AzureRmVMDiskEncryptionStatus`. Als u de instellingen voor codering voor uw virtuele machine, voert u het volgende:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

U kunt de uitvoer van inspecteren _Get-AzureRmVMDiskEncryptionStatus_ sleutel URL's voor versleuteling.

    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey

    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

De instellingen OSVolumeEncrypted en DataVolumesEncrypted waarden zijn ingesteld op _versleutelde_, waarmee u aangeeft dat beide volumes zijn versleuteld met behulp van Azure Disk Encryption. Zie voor informatie over het inschakelen van versleuteling met Azure Disk Encryption met behulp van PowerShell-cmdlets, de blogberichten [Azure Disk Encryption verkennen met Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) en [Azure Disk Encryption verkennen Deel 2 met Azure PowerShell -](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

> [!NOTE]
> Op Linux virtuele machines, het drie tot vier minuten duurt voordat de `Get-AzureRmVMDiskEncryptionStatus` cmdlet om de versleutelingsstatus te rapporteren.

#### <a name="get-the-encryption-status-of-the-iaas-vm-from-the-disk-encryption-cli-command"></a>De coderingsstatus van IaaS VM ophalen uit de CLI schijfversleuteling opdracht
U kunt de coderingsstatus van IaaS VM ophalen door met de opdracht van de CLI-schijfversleuteling `azure vm show-disk-encryption-status`. Als u de instellingen voor codering voor uw virtuele machine, Voer uw Azure CLI-sessie:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Schakel versleuteling voor het uitvoeren van Windows IaaS VM
U kunt versleuteling op een actieve Windows of Linux IaaS VM via de Azure Disk Encryption Resource Manager-sjabloon of het PowerShell-cmdlets uitschakelen en de configuratie ontsleuteling opgeven.

##### <a name="windows-vm"></a>Windows VM
Versleuteling van het besturingssysteem, het gegevensvolume of beide op het actieve Windows IaaS VM Hiermee schakelt u de stap uitschakelen-codering. U kan het besturingssysteemvolume uitschakelen en laat het gegevensvolume dat is versleuteld. Wanneer de stap uitschakelen-versleuteling wordt uitgevoerd, het klassieke implementatiemodel Azure werkt het VM-servicemodel en de Windows IaaS VM ontsleutelde is gemarkeerd. De inhoud van de virtuele machine worden niet langer in rust versleuteld. De ontsleuteling verwijdert niet de sleutelkluis en de versleuteling sleutelmateriaal (versleutelingssleutels BitLocker voor Windows en de wachtwoordzin voor Linux).

##### <a name="linux-vm"></a>Virtuele Linux-machine
Versleuteling van het gegevensvolume van de op de actieve virtuele machine IaaS Linux Hiermee schakelt u de stap uitschakelen-codering. Deze stap werkt alleen als de besturingssysteemschijf is niet versleuteld.

> [!NOTE]
> Uitschakelen van versleuteling op de schijf met het besturingssysteem is niet toegestaan op de virtuele Linux-machines.

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Schakel versleuteling in op een bestaande of actief IaaS VM
U kunt uitschakelen schijfversleuteling op IaaS VM's van Windows worden uitgevoerd met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).

1. Klik op de Azure snel starten-sjabloon **implementeren in Azure**, voer de configuratie van de ontsleuteling op de **Parameters** blade en klik vervolgens op **OK**.

2. Selecteer het abonnement, resourcegroep locatie voor resourcegroep, juridische voorwaarden en overeenkomst en klik vervolgens op **maken** versleuteling op een nieuwe IaaS VM in te schakelen.

Virtuele Linux-machines, kunt u versleuteling uitschakelen met behulp van de [uitschakelen van versleuteling op een actieve Linux VM](https://aka.ms/decrypt-linuxvm) sjabloon.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor het uitschakelen van versleuteling op een actieve IaaS VM:

| Parameter | Beschrijving |
| --- | --- |
| vmName | Naam van de virtuele machine die de versleutelingsbewerking op worden uitgevoerd.
| volumeType | Het type van het volume dat een ontsleutelingsbewerking wordt uitgevoerd op. Geldige waarden zijn _OS_, _gegevens_, en _alle_. U kunt versleuteling voor het uitvoeren van Windows IaaS VM OS/opstartvolume zonder versleuteling wordt uitgeschakeld op het niet uitschakelen de _gegevens_ volume. Houd er ook rekening mee dat uitschakelen versleuteling op de schijf met het besturingssysteem is niet toegestaan voor virtuele Linux-machines. |
| sequenceVersion | De versie van de volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens wanneer een ontsleutelingsbewerking schijf op dezelfde virtuele machine wordt uitgevoerd. |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Schakel versleuteling in op een bestaande of actief IaaS VM
Zie voor informatie over het uitschakelen van versleuteling op een bestaande of actief IaaS-VM met behulp van de PowerShell-cmdlet [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). Deze cmdlet biedt ondersteuning voor Windows- en Linux-machines. Schakel codering wordt een extensie geïnstalleerd op de virtuele machine. Als de _naam_ parameter niet is opgegeven, een uitbreiding met de standaardnaam _AzureDiskEncryption voor VM's van Windows_ wordt gemaakt.

Op Linux virtuele machines, wordt de AzureDiskEncryptionForLinux-uitbreiding gebruikt.

> [!NOTE]
> Deze cmdlet opnieuw opgestart voor de virtuele machine.

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a>Schakel versleuteling in op vooraf zijn versleuteld IaaS VM met beheerde Azure-schijf
De Azure beheerd schijf ARM-sjabloon gebruiken om te maken van een versleutelde virtuele machine van een vooraf zijn versleuteld VHD met de ARM-sjabloon te vinden op   
[Een nieuwe beheerde versleutelde schijf van een vooraf zijn versleuteld VHD/storage-blob maken] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a>Schakel versleuteling in op een nieuwe Linux IaaS-VM met beheerde Azure-schijf
Met de Azure beheerd schijf ARM-sjabloon kunt u een nieuwe versleutelde Linux IaaS virtuele machine maken met de ARM-sjabloon te vinden op   
[Implementatie RHEL 7.2 met volledige schijfversleuteling] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a>Schakel versleuteling in op een nieuwe Windows IaaS-VM met beheerde Azure-schijf
 Met de Azure beheerd schijf ARM-sjabloon kunt u een nieuwe versleutelde Linux IaaS virtuele machine maken met de ARM-sjabloon te vinden op   
 [Een nieuwe versleutelde Windows IaaS beheerd schijf virtuele machine maken van de afbeelding] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)

  > [!NOTE]
  >Dit is verplicht op momentopname en/of back-up een beheerde schijven op basis van VM-instantie buiten en voordat u Azure Disk Encryption inschakelt.  Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of Azure Backup kan worden gebruikt.  Back-ups zorgen ervoor dat een hersteloptie mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen.  Als u een back-up is gemaakt, kan de cmdlet Set-AzureRmVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven door de parameter - skipVmBackup te geven.  Met deze opdracht uitvoeren tegen beheerde schijven op basis van de virtuele machine totdat een back-up is gemaakt en deze parameter is opgegeven.    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a>Versleutelingsinstellingen van een bestaande versleutelde niet premium virtuele machine bijwerken
  Gebruik de bestaande interfaces van de schijf van Azure-codering wordt ondersteund voor het uitvoeren van de virtuele machine [PS-cmdlets, CLI of ARM-sjablonen] de versleutelingsinstellingen bijwerken zoals AAD client-ID/geheime sleutel versleutelingssleutel [KEK] versleutelingssleutel BitLocker voor Windows-VM of de wachtwoordzin voor Linux-VM enz. De versleutelingsinstelling voor de update wordt ondersteund voor premium- en niet premium storage-VM's.

## <a name="appendix"></a>Bijlage
### <a name="connect-to-your-subscription"></a>Verbinding maken met uw abonnement
Lees voordat u doorgaat, de *vereisten* sectie in dit artikel. Nadat u ervoor te zorgen dat aan alle vereisten is voldaan, kunt u verbinding met uw abonnement als volgt:

1. Start een Azure PowerShell-sessie en meld u aan bij uw Azure-account met de volgende opdracht:

    `Login-AzureRmAccount`

2. Als u meerdere abonnementen hebt en opgeven dat een wilt om te gebruiken, typt u het volgende als u wilt zien van de abonnementen voor uw account:

    `Get-AzureRmSubscription`

3. Als u het abonnement dat u wilt gebruiken, typt u:

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. Om te controleren of het abonnement geconfigureerd juist is, typt u:

    `Get-AzureRmSubscription`

5. Om te bevestigen op dat de Azure Disk Encryption-cmdlets zijn geïnstalleerd, typt u:

    `Get-command *diskencryption*`

6. De volgende voorbeelduitvoer bevestigt dat de installatie van Azure schijf versleuteling PowerShell:

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a>Een vooraf zijn versleuteld Windows VHD voorbereiden
De volgende secties zijn nodig voor het voorbereiden van een vooraf zijn versleuteld Windows VHD voor de implementatie als een gecodeerde VHD in Azure IaaS. Gebruik de informatie voor het voorbereiden en een nieuwe Windows VM (VHD) op de Azure Site Recovery of Azure worden opgestart.

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Bijwerken van Groepsbeleid om toe te staan zonder TPM voor OS-beveiliging
De groepsbeleidsinstellingen voor BitLocker-instelling **BitLocker-stationsversleuteling**, die u vindt hier onder **lokaal computerbeleid** > **Computerconfiguratie**  >  **Beheersjablonen** > **Windows-onderdelen**. Deze instelling te wijzigen **besturingssysteem stations** > **aanvullende verificatie bij opstarten vereisen** > **BitLocker zonder een compatibele TPMtoestaan**, zoals wordt weergegeven in de volgende afbeelding:

![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>BitLocker-onderdelen installeren
Voor Windows Server 2012 en hoger, gebruikt u de volgende opdracht:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Gebruik de volgende opdracht voor Windows Server 2008 R2:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>Het volume met het besturingssysteem voorbereiden voor BitLocker met behulp van`bdehdcfg`
Voor het comprimeren van de partitie van het besturingssysteem en de machine voorbereiden voor BitLocker, voert u de volgende opdracht uit:

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-the-os-volume-by-using-bitlocker"></a>Het besturingssysteemvolume beveiligen met behulp van BitLocker
Gebruik de [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) opdracht versleuteling op het opstartvolume met behulp van een externe sleutelbeveiliging in te schakelen. Ook de externe sleutel (.bek-bestand) op de externe schijf of volume plaatsen. Versleuteling is ingeschakeld op het systeem/opstartvolume na het opnieuw opstarten.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> De virtuele machine met een afzonderlijke gegevens/resource VHD voorbereiden voor het ophalen van de externe sleutel met BitLocker.

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Versleutelen van een OS-station op een actieve Linux VM
Versleuteling van een OS-station op een actieve Linux VM wordt ondersteund door de volgende distributies:

* RHEL 7.2
* CentOS 7.2
* Ubuntu 16.04

##### <a name="prerequisites-for-os-disk-encryption"></a>Vereisten voor de OS-schijfversleuteling

* De virtuele machine moet worden gemaakt vanuit de Marketplace-installatiekopie in Azure Resource Manager.
* Azure virtuele machine met ten minste 4 GB aan RAM-geheugen (aanbevolen grootte is 7 GB).
* (Voor RHEL en CentOS) SELinux uitschakelen. Zie '4.4.2 SELinux uitschakelen. Uitschakelen van SELinux' in de [SELinux van de gebruiker en de Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) op de virtuele machine.
* Nadat u SELinux uitgeschakeld, start u de virtuele machine ten minste één keer opnieuw.

##### <a name="steps"></a>Stappen
1. Een virtuele machine maken met behulp van een van de verdelingen eerder hebt opgegeven.

 Voor CentOS 7.2, OS schijfversleuteling ondersteund via een speciale installatiekopie. Geef voor het gebruik van deze installatiekopie '7.2n' als de SKU bij het maken van de virtuele machine:
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. De virtuele machine naar wens configureren. Als u schijven voor het versleutelen van alle de (OS + gegevens), de gegevensstations moeten worden opgegeven en koppelbaar van /etc/fstab gaat.

 > [!NOTE]
 > Gebruik UUID =... om op te geven gegevensstations in/etc/fstab in plaats van de naam van het blok apparaat (bijvoorbeeld/dev/sdb1) opgeven. Tijdens het versleutelen wijzigt de volgorde van de stations op de virtuele machine. Als uw virtuele machine afhankelijk van een specifieke volgorde van apparaten is, mislukt het koppel deze na versleuteling.

3. Meld u af bij de SSH-sessies.

4. Geef voor het versleutelen van het besturingssysteem volumeType als **alle** of **OS** wanneer u [versleuteling inschakelen](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

 > [!NOTE]
 > Alle gebruikersruimte innemen processen die niet worden uitgevoerd als `systemd` services moeten worden afgesloten met een `SIGKILL`. Start opnieuw op de virtuele machine. Wanneer u OS schijfversleuteling op een actieve virtuele machine inschakelt, plan op VM uitvaltijd.

5. Controleer regelmatig de voortgang van versleuteling met behulp van de instructies in de [volgende sectie](#monitoring-os-encryption-progress).

6. Nadat Get AzureRmVmDiskEncryptionStatus toont 'VMRestartPending', start u uw virtuele machine opnieuw aanmelden bij deze of via de portal, PowerShell of CLI.
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
Voordat u opnieuw opstart, wordt aangeraden dat u opslaat [opstarten diagnostics](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) van de virtuele machine.

#### <a name="monitoring-os-encryption-progress"></a>OS-versleuteling voortgang controleren
U kunt voortgang OS versleuteling op drie manieren:

* Gebruik de `Get-AzureRmVmDiskEncryptionStatus` cmdlet en controleer het veld ProgressMessage:
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 Nadat de virtuele machine bereikt 'OS schijfversleuteling gestart', duurt het ongeveer 40 tot 50 minuten back VM op een Premium-opslag.

 Omdat [uitgeven #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent, `OsVolumeEncrypted` en `DataVolumesEncrypted` worden weergegeven als `Unknown` in een aantal distributies. Met de versie 2.1.5 WALinuxAgent en later kunt dit probleem automatisch opgelost. Als u ziet `Unknown` u kunt in de uitvoer schijfversleuteling status controleren met behulp van de Azure Resourceverkenner.

 Ga naar [Azure Resource Explorer](https://resources.azure.com/), en vouw vervolgens deze hiërarchie in het deelvenster selectie op links:

 ~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

 Ga in de InstanceView voor de versleutelingsstatus van uw schijven.

 ![Instantieweergave van virtuele machine](./media/azure-security-disk-encryption/vm-instanceview.png)

* Bekijk [opstarten diagnostics](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/). Berichten van de extensie ADE moeten worden voorafgegaan door `[AzureDiskEncryption]`.

* Aanmelden bij de virtuele machine via SSH en ophalen van het logboek voor uitbreiding van:

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

 Het is raadzaam dat u niet met de virtuele machine aanmelden zich terwijl OS-versleuteling uitgevoerd wordt. Kopieer de logboeken alleen wanneer de twee methoden zijn mislukt.

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a>Een vooraf zijn versleuteld Linux VHD voorbereiden
##### <a name="ubuntu-16"></a>Ubuntu 16
Configureren van versleuteling tijdens de installatie van het distributiepunt als volgt:

1. Selecteer **configureren van versleutelde volumes** wanneer u de schijven partitioneren.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Maak een afzonderlijke opstartstation, moet niet worden versleuteld. Codeer uw basisstation.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Geef een wachtwoordzin. Dit is de wachtwoordzin op die u naar de sleutelkluis uploadt.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Voltooi partitioneren.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. Wanneer u de virtuele machine worden opgestart en wordt gevraagd een wachtwoordzin, gebruikt u de wachtwoordzin die u hebt opgegeven in stap 3.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. De virtuele machine voorbereiden voor het uploaden naar Azure met behulp [deze instructies](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). Voer niet de laatste stap (de virtuele machine opheffen van inrichting) nog.

Codering met Azure werkt als volgt configureren:

1. Maak een bestand onder /usr/local/sbin/azure_crypt_key.sh, met de inhoud in het volgende script. Let op de KeyFileName omdat deze de naam van het wachtwoordzin die wordt gebruikt door Azure.

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. Wijzig de configuratie crypt in */etc/crypttab*. Dit ziet er als volgt uit:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Als u wilt bewerken *azure_crypt_key.sh* in Windows en u gekopieerd naar Linux, voer `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

4. Uitvoerbare machtigingen toevoegen aan het script:
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. Bewerken */etc/initramfs-tools/modules* door regels toe te voegen:
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. Uitvoeren `update-initramfs -u -k all` bijwerken van de initramfs waarmee de `keyscript` te laten treden.

7. Nu u de virtuele machine kunt inrichting ervan ongedaan.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Doorgaan naar volgende stap en [uw VHD uploaden](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure.

##### <a name="opensuse-132"></a>openSUSE 13.2
Voor het configureren van versleuteling tijdens de installatie van het distributiepunt, het volgende doen:
1. Wanneer u de schijven partitioneren, selecteert u **versleutelen Volume groep**, en voer vervolgens een wachtwoord. Dit is het wachtwoord dat u naar de sleutelkluis uploaden zult.

 ![openSUSE 13.2 instellen](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Start de virtuele machine met het wachtwoord.

 ![openSUSE 13.2 instellen](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. De virtuele machine voorbereiden voor het uploaden naar Azure door de instructies in [een SLES of openSUSE virtuele machine voorbereiden voor Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). Voer niet de laatste stap (de virtuele machine opheffen van inrichting) nog.

Versleuteling werkt met Azure, kunt u het volgende configureren:
1. Bewerk de /etc/dracut.conf, en voeg de volgende regel:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Deze regels uitcommentariëren aan het einde van het bestand /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. De volgende regel aan het begin van het bestand /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh toevoegen:
 ```
    DRACUT_SYSTEMD=0
 ```
En wijzig alle instanties van:
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
in:
```
    if [ 1 ]; then
```
4. Bewerk /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh en voegt deze toe aan '# Open LUKS apparaat':

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. Voer `/usr/sbin/dracut -f -v` de initrd bijwerken.

6. Nu u kunt inrichting ervan ongedaan maakt de virtuele machine en [uw VHD uploaden](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure.

##### <a name="centos-7"></a>CentOS 7
Voor het configureren van versleuteling tijdens de installatie van het distributiepunt, het volgende doen:
1. Selecteer **mijn gegevens versleutelen** wanneer u schijven partitioneren.

 ![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Zorg ervoor dat **versleutelen** is geselecteerd voor de basispartitie.

 ![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Geef een wachtwoordzin. Dit is de wachtwoordzin op die u naar de sleutelkluis uploaden zult.

 ![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. Wanneer u de virtuele machine worden opgestart en wordt gevraagd een wachtwoordzin, gebruikt u de wachtwoordzin die u hebt opgegeven in stap 3.

 ![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. De virtuele machine voorbereiden voor het uploaden naar Azure met behulp van de 'CentOS 7.0 +'-instructies in [een CentOS-virtuele machine voorbereiden voor Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). Voer niet de laatste stap (de virtuele machine opheffen van inrichting) nog.

6. Nu u kunt inrichting ervan ongedaan maakt de virtuele machine en [uw VHD uploaden](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure.

Versleuteling werkt met Azure, kunt u het volgende configureren:

1. Bewerk de /etc/dracut.conf, en voeg de volgende regel:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Deze regels uitcommentariëren aan het einde van het bestand /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. De volgende regel aan het begin van het bestand /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh toevoegen:
```
    DRACUT_SYSTEMD=0
```
En wijzig alle instanties van:
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
tot
```
    if [ 1 ]; then
```
4. Bewerk /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh en dit toevoegen na de '# Open LUKS apparaat':
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. Voer de ' / usr/sbin/dracut - f - v ' de initrd bijwerken.

![CentOS 7 Setup](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Versleutelde harde schijf geüpload naar Azure storage-account
Nadat BitLocker-versleuteling of DM-Crypt versleuteling is ingeschakeld, moet de lokale versleutelde VHD te uploaden naar uw opslagaccount.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-the-disk-encryption-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a>Uploaden van het geheim schijf-codering voor vooraf zijn versleuteld VM aan de sleutelkluis
Het geheim voor schijf-codering die u hebt verkregen moet eerder worden geüpload als een geheim in de sleutelkluis. De sleutelkluis moet beschikken over machtigingen die zijn ingeschakeld voor uw Azure AD-client en schijfversleuteling.

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Schijf versleuteling geheim niet versleuteld met een KEK-sleutel
Als u het geheim in de sleutelkluis instelt, gebruik [Set AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret). Als u een virtuele Windows-computer hebt, het bek-bestand is gecodeerd als een base64-tekenreeks en vervolgens geüpload naar uw sleutelkluis met de `Set-AzureKeyVaultSecret` cmdlet. De wachtwoordzin is voor Linux, gecodeerd als een base64-tekenreeks en vervolgens geüpload naar de sleutelkluis. Bovendien moet u ervoor dat de volgende codes zijn ingesteld bij het maken van het geheim in de sleutelkluis.

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

Gebruik de `$secretUrl` in de volgende stap voor [de OS-schijf koppelen zonder KEK](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Schijf versleuteling geheim is versleuteld met een KEK-sleutel
Voordat u het geheim naar de sleutelkluis uploaden, kunt u deze desgewenst met een sleutel versleutelingssleutel coderen. Gebruik de omloop [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) eerst het geheim met behulp van de versleutelingssleutel van de sleutel te versleutelen. De uitvoer van deze bewerking tekstterugloop is base64 gecodeerde URL string die u vervolgens uploaden als een geheim met behulp van kunt de [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

Gebruik `$KeyEncryptionKey` en `$secretUrl` in de volgende stap voor [de OS-schijf met behulp van de KEK koppelen](#using-a-kek).

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>Geef een geheime URL wanneer u een besturingssysteemschijf koppelen
#### <a name="without-using-a-kek"></a>Zonder een KEK-sleutel
Terwijl u de schijf met het besturingssysteem toevoegen wilt, moet u doorgeven `$secretUrl`. De URL is in de sectie 'schijfversleuteling geheim niet versleuteld met een KEK' gegenereerd.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Met behulp van een KEK-sleutel
Wanneer u de OS-schijf koppelen, `$KeyEncryptionKey` en `$secretUrl`. De URL is in de sectie 'schijfversleuteling geheim niet versleuteld met een KEK' gegenereerd.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a>Download deze handleiding
U kunt deze handleiding uit downloaden de [TechNet-galerie](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

## <a name="for-more-information"></a>Voor meer informatie
[Verken Azure Disk Encryption met Azure PowerShell - deel 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[Azure Disk Encryption met Azure PowerShell - deel 2 verkennen](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
