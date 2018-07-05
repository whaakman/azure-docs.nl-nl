---
title: Azure Disk Encryption voor Windows en Linux IaaS-VM's | Microsoft Docs
description: Dit artikel bevat een overzicht van Microsoft Azure Disk Encryption voor Windows en Linux IaaS-VM's.
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
ms.date: 03/13/2018
ms.author: devtiw
ms.openlocfilehash: f350716d0ca906376f3eadce9e117694ff14515c
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2018
ms.locfileid: "35756393"
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Azure Disk Encryption voor Windows en Linux IaaS-VM 's
Microsoft Azure is sterk belangrijk ervoor te zorgen dat de privacy van gegevens, gegevensonafhankelijkheid en kunt u de controle uw door Azure gegevens via een reeks gehoste technologieën geavanceerde voor het coderen, beheren en beheren van versleutelingssleutels, controle en audit toegang van gegevens. Dit biedt Azure-klanten de flexibiliteit om de oplossing die het beste aan hun behoeften van uw bedrijf te kiezen. In dit artikel vindt we u een nieuwe technologieoplossing 'Azure Disk Encryption voor Windows en Linux IaaS VM van' om u te helpen te beschermen en beveiligen van uw gegevens om te voldoen aan uw organisatie beveiligings- en nalevingsverplichtingen. Het artikel bevat gedetailleerde richtlijnen over het gebruik van de Azure disk encryption functies, met inbegrip van de ondersteunde scenario's en de gebruiker optreedt.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Overzicht
Azure Disk Encryption is een nieuwe functie waarmee u uw Windows- en Linux IaaS VM-schijven te versleutelen. Azure Disk Encryption maakt gebruik van de industrienorm [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows en de [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) -functie van Linux om volumeversleuteling voor het besturingssysteem en de gegevensschijven te bieden. De oplossing is geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te controleren en beheren van de schijf-versleutelingssleutels en geheimen in uw key vault-abonnement. De oplossing zorgt er ook voor dat alle gegevens op de virtuele-machineschijven zijn versleuteld in rust in uw Azure-opslag.

Azure disk encryption voor Windows en Linux IaaS-machines is nu in **algemene beschikbaarheid** in alle Azure-openbare regio's en AzureGov regio's voor standaard-VM's en VM's met premium storage.

> [!NOTE]
> Bepaalde aanbevelingen verhogen gegevens-, netwerk- of computerresources, wat resulteert in extra kosten in licentie of abonnement.


### <a name="encryption-scenarios"></a>Scenario's voor versleuteling
De Azure Disk Encryption-oplossing ondersteunt de volgende klant-scenario's:

* Schakelt u versleuteling op nieuwe IaaS VM's die worden gemaakt op basis van vooraf gecodeerde VHD- en versleutelingssleutels
* Schakelt u versleuteling op nieuwe IaaS VM's die worden gemaakt op basis van de installatiekopieën van de ondersteunde Azure-galerie
* Schakelt u versleuteling op bestaande IaaS-VM's die worden uitgevoerd in Azure
* Schakel versleuteling uit op virtuele Windows IaaS-machines
* Schakel versleuteling uit op gegevensstations voor Linux IaaS-VM 's
* Inschakelen van versleuteling van virtuele machines op beheerde schijven
* Instellingen voor versleuteling van een bestaande versleutelde premium- en niet-premium-opslag virtuele machine bijwerken
* Back-up en herstel van versleutelde virtuele machines

De oplossing ondersteunt de volgende scenario's voor IaaS-VM's als ze zijn ingeschakeld in Microsoft Azure:

* Integratie met Azure Key Vault
* Standard-laag virtuele machines: [A, D, DS, G, GS, F en enzovoort reeks IaaS-VM's](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Schakelt u versleuteling op Windows en Linux IaaS-VM's en virtuele machines op beheerde schijven van de ondersteunde Azure-galerie afbeeldingen
* Versleuteling voor stations met besturingssysteem- en Windows IaaS-VM's en virtuele machines op beheerde schijven uitschakelen
* Schakel versleuteling uit op gegevensstations voor Linux IaaS-VM's en virtuele machines op beheerde schijven
* Schakelt u versleuteling op IaaS-VM's met het Windows Client-besturingssysteem
* Versleuteling voor volumes met koppelpunt paden inschakelen
* Schakelt u versleuteling op Linux-VM's geconfigureerd met schijf striping (RAID) met behulp van mdadm
* Schakelt u versleuteling op virtuele Linux-machines met behulp van LVM voor gegevensschijven
* Schakelt u versleuteling op Linux LVM 7.3 voor besturingssysteem en gegevensschijven 
* Schakelt u versleuteling op Windows-VM's geconfigureerd met opslagruimten
* Instellingen voor versleuteling van een bestaande versleutelde premium- en niet-premium-opslag virtuele machine bijwerken
* Back-up en herstel van versleutelde virtuele machines, voor zowel niet-KEK-sleutel en KEK-scenario's (KEK - Key-versleutelingssleutel)
* Alle openbare Azure- en AzureGov regio's worden ondersteund

De oplossing biedt geen ondersteuning voor de volgende scenario's, functies en -technologie:

* Basic-laag IaaS-VM 's
* Uitschakelen van versleuteling op een station van het besturingssysteem voor Linux IaaS-VM 's
* Uitschakelen van versleuteling op een schijf als de besturingssysteemschijf is versleuteld voor Linux Iaas-VM 's
* IaaS-VM's die zijn gemaakt met behulp van de klassieke methode voor het maken van de virtuele machine
* Hiermee schakelt u versleuteling op Windows en Linux IaaS-VM's klanten aangepaste installatiekopieën wordt niet ondersteund.
* Integratie met uw on-premises Key Management Service
* Azure Files (gedeelde bestandssysteem), Network File System (NFS), dynamische volumes en virtuele machines van Windows die zijn geconfigureerd met software gebaseerde RAID-systemen

### <a name="encryption-features"></a>Versleutelingsfuncties
Als u inschakelt en implementeren van Azure Disk Encryption voor Azure IaaS VM's, worden de volgende mogelijkheden ingeschakeld, afhankelijk van de opgegeven configuratie:

* Versleuteling van het volume met het besturingssysteem op het opstartvolume at-rest in de opslag beveiligen
* Versleuteling van gegevensvolumes om de gegevensvolumes in rust in uw opslag te beveiligen
* Uitschakelen van versleuteling op de OS- en gegevensstations voor Windows IaaS-VM 's
* Uitschakelen van versleuteling op de gegevens schijven voor Linux IaaS-VM's (alleen als het besturingssysteem IS niet versleuteld station)
* Bescherming van de versleutelingssleutels en geheimen in uw key vault-abonnement
* Rapportage van de versleutelingsstatus van de versleutelde IaaS-VM
* Het verwijderen van configuratie-instellingen van de virtuele machine voor IaaS-schijfversleuteling
* Back-up en herstel van versleutelde virtuele machines met behulp van de Azure Backup-service

Azure Disk Encryption voor IaaS VMS voor Windows en Linux-oplossing omvat:

* De versleuteling van schijf-extensie voor Windows.
* De versleuteling van schijf-extensie voor Linux.
* De versleuteling van schijf PowerShell-cmdlets.
* De-schijfversleuteling cmdlets van Azure-opdrachtregelinterface (CLI).
* De versleuteling van schijf Azure Resource Manager-sjablonen.

De Azure Disk Encryption-oplossing wordt ondersteund op IaaS-VM's waarop Windows of Linux-besturingssysteem. Zie de sectie "Vereisten" voor meer informatie over de ondersteunde besturingssystemen.

> [!NOTE]
> Er is geen extra kosten in rekening gebracht voor het versleutelen van VM-schijven met Azure Disk Encryption.

### <a name="value-proposition"></a>Waardevoorstel
Wanneer u de Azure Disk Encryption-management-oplossing toepast, kunt u voldoen aan de volgende zaken dat vereisen:

* IaaS-VM's worden beveiligd in rust, omdat u industriestandaard versleutelingstechnologie gebruiken kunt om de organisatorische vereisten voor beveiliging en naleving.
* Opstarten van de IaaS-VM's onder de klant beheerde sleutels en beleidsregels en u kunt het gebruik ervan in uw key vault controleren.

### <a name="encryption-workflow"></a>Werkstroom voor versleuteling
Om in te schakelen schijfversleuteling voor Windows en Linux-VM's, het volgende doen:

1. Kies een versleutelingsscenario uit de bovenstaande scenario's voor versleuteling.
2. U meldt zich aan het inschakelen van schijfversleuteling via de Azure Disk Encryption Resource Manager-sjabloon, de PowerShell-cmdlets of de CLI-opdracht en geeft u de versleutelingsconfiguratie van de.

   * Voor het scenario van de VHD klant versleuteld door de versleutelde VHD te uploaden naar uw opslagaccount en de versleuteling sleutelmateriaal tot uw key vault. Klik, geef de versleutelingsconfiguratie inschakelen van versleuteling op een nieuwe IaaS-VM.
   * Voor nieuwe virtuele machines die zijn gemaakt vanuit de Marketplace en bestaande VM's die al worden uitgevoerd in Azure, geeft u de versleutelingsconfiguratie inschakelen van versleuteling op de IaaS-VM.

3. Toegang verlenen tot de Azure-platform om te lezen van de versleutelingssleutel materiaal (BitLocker-versleutelingssleutels voor Windows-systemen) en de wachtwoordzin voor Linux uit uw key vault om in te schakelen van versleuteling op de IaaS-VM.

4. Geef de toepassings-id van de Azure Active Directory (Azure AD) voor het schrijven van de versleutelingssleutel materiaal naar uw key vault. In dat geval kunt u versleuteling op de IaaS-VM voor de scenario's die worden vermeld in stap 2.

5. Azure werkt bij het VM-servicemodel met versleuteling en de configuratie van de key vault en stelt u de versleutelde VM.

 ![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Werkstroom voor ontsleuteling
Als u wilt uitschakelen schijfversleuteling voor IaaS-VM's, de volgende stappen op hoog niveau:

1. Kies om versleuteling (decodering) te schakelen op een actieve IaaS-VM in Azure via de Azure Disk Encryption Resource Manager-sjabloon of het PowerShell-cmdlets en geeft u de configuratie voor ontsleuteling.

 Deze stap schakelt versleuteling van het besturingssysteem of het gegevensvolume of beide op de actieve Windows IaaS-virtuele machine. Echter, zoals vermeld in de vorige sectie, uitschakelen van OS-schijfversleuteling voor Linux wordt niet ondersteund. De stap ontsleuteling is alleen toegestaan voor schijven op virtuele Linux-machines, zolang de besturingssysteemschijf is niet versleuteld.
2. Azure werkt het servicemodel van de virtuele machine en de IaaS-VM is gemarkeerd als ontsleutelde. De inhoud van de virtuele machine worden niet meer in rust versleuteld.

> [!NOTE]
> De bewerking uitschakelen-codering wordt niet verwijderd voor uw key vault en de versleuteling sleutelmateriaal (BitLocker-versleutelingssleutels voor Windows-systemen) of de wachtwoordzin voor Linux.
 > Uitschakelen van OS-schijfversleuteling voor Linux wordt niet ondersteund. De stap ontsleuteling is alleen toegestaan voor schijven op virtuele Linux-machines.
Uitschakelen van de schijf van gegevensversleuteling voor Linux wordt niet ondersteund als de besturingssysteemschijf is versleuteld.

## <a name="prerequisites"></a>Vereisten
Voordat u Azure Disk Encryption voor Azure IaaS VM's voor de ondersteunde scenario's die zijn beschreven in de sectie 'Overzicht' inschakelt, raadpleegt u de volgende vereisten:

* U moet een geldige actief Azure-abonnement om resources te maken in Azure in de ondersteunde regio's hebben.
* Azure Disk Encryption wordt ondersteund op de volgende versies van Windows Server: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016.
* Azure Disk Encryption wordt ondersteund op de volgende versies van Windows-client: client voor Windows 8 en Windows 10-client.

> [!NOTE]
> Voor Windows Server 2008 R2 moet u .NET Framework 4.5 zijn geïnstalleerd voordat u versleuteling in Azure inschakelen. U kunt deze via Windows Update installeren door het installeren van de optionele update voor Microsoft .NET Framework 4.5.2 voor Windows Server 2008 R2 x64 64-systemen ([KB2901983](https://support.microsoft.com/kb/2901983)).

* Azure Disk Encryption is alleen ondersteund op bepaalde Azure-galerie op basis van Linux-server-distributies en versies.  Raadpleeg voor de lijst met ondersteunde versies, de [Veelgestelde vragen over Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq).

* Azure Disk Encryption is vereist dat uw sleutelkluis en de virtuele machines zich in de dezelfde Azure-regio en het abonnement bevinden.

> [!NOTE]
> Configureren van de resources in verschillende regio's, zorgt ervoor dat een fout opgetreden bij het inschakelen van de Azure Disk Encryption-functie.

* Als u wilt instellen en configureren uw key vault voor Azure Disk Encryption, Zie de sectie **stelt u en uw key vault configureren voor Azure Disk Encryption** in de *vereisten* sectie van dit artikel.
* Als u wilt instellen en configureren van Azure AD-toepassing in Azure Active directory voor Azure Disk Encryption, Zie de sectie **instellen van de Azure AD-toepassing in Azure Active Directory** in de *vereisten* sectie van in dit artikel.
* Als u wilt instellen en configureren van het toegangsbeleid voor key vault voor de Azure AD-toepassing, Zie de sectie **instellen van het toegangsbeleid voor key vault voor de Azure AD-toepassing** in de *vereisten* sectie van dit artikel.
* Om voor te bereiden op een Windows VHD met vooraf zijn versleuteld, Zie de sectie **vooraf gecodeerde Windows VHD voorbereiden** in de *bijlage*.
* Om voor te bereiden op een VHD met Linux-vooraf zijn versleuteld, Zie de sectie **vooraf gecodeerde Linux-VHD voorbereiden** in de *bijlage*.
* Het Azure-platform moet toegang hebben tot de versleutelingssleutels of geheimen in uw key vault zodat ze beschikbaar zijn voor de virtuele machine is wanneer deze wordt opgestart en het volume van de virtuele machine-besturingssysteem ontsleutelt. Instellen om machtigingen te verlenen aan Azure-platform, de **EnabledForDiskEncryption** eigenschap in de key vault. Zie voor meer informatie, **stelt u en uw key vault configureren voor Azure Disk Encryption** in de bijlage.
* Uw key vault-geheim en de KEK-sleutel-URL's moeten worden bijgehouden. Azure wordt deze beperking van versiebeheer afgedwongen. Zie de volgende voorbeelden voor geldige geheim en KEK-sleutel-URL's:

  * Voorbeeld van een geldige geheime URL:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Voorbeeld van een geldige KEK-sleutel-URL:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk Encryption biedt geen ondersteuning voor het opgeven van de poortnummers als onderdeel van de key vault-geheimen en KEK-sleutel-URL's. Voor voorbeelden van niet-ondersteunde en ondersteunde key vault-URL's, Zie de volgende:

  * URL voor key vault niet toegestaan.  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * URL voor acceptabel key vault:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Functie, de IaaS-VM's moet voldoen aan de volgende configuratievereisten voor netwerk-eindpunt voor de Azure Disk Encryption inschakelen:
  * Als u een token voor het verbinding maken met uw key vault, moet de IaaS-VM geen verbinding maken met een Azure Active Directory-eindpunt, zijn \[login.microsoftonline.com\].
  * Als de versleutelingssleutels wilt opslaan op uw key vault, moet de IaaS-VM geen verbinding maken met het eindpunt van de sleutelkluis.
  * De IaaS-VM moet geen verbinding maken met een Azure storage-eindpunt dat als host fungeert voor de Azure-extensie-opslagplaats en een Azure storage-account dat als host fungeert voor de VHD-bestanden.

  > [!NOTE]
  > Als uw beveiligingsbeleid beperkt de toegang van Azure VM's tot het Internet, kunt u deze kunt oplossen door de voorgaande URI en configureren van een specifieke regel voor het toestaan van uitgaande verbinding met de IP-adressen.
  >
  >Om te configureren en toegang tot Azure Key Vault achter een firewall (https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall)

* Gebruik de nieuwste versie van Azure PowerShell SDK-versie van Azure Disk Encryption te configureren. Download de nieuwste versie van [release van Azure PowerShell](https://github.com/Azure/azure-powershell/releases)

 > [!NOTE]
  > Azure Disk Encryption wordt niet ondersteund op [Azure PowerShell SDK versie 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Als u ontvangt een fout met betrekking tot het gebruik van Azure PowerShell 1.1.0, raadpleegt u [Azure Disk Encryption fout met betrekking tot Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

* Voor elke Azure CLI-opdracht uitvoeren en deze koppelen aan uw Azure-abonnement, moet u eerst de Azure CLI installeren:
  * Azure CLI installeren en deze koppelen aan uw Azure-abonnement, Zie [installeren en configureren van Azure CLI](../cli-install-nodejs.md).
  * Zie voor het gebruik van Azure CLI voor Mac, Linux en Windows met Azure Resource Manager, [Azure CLI-opdrachten in Resource Manager-modus](../virtual-machines/azure-cli-arm-commands.md).

* Bij het versleutelen van een beheerde schijf, is deze verplichte vereiste voor het uitvoeren van een momentopname van de beheerde schijf of een back-up van de schijf buiten Azure Disk Encryption voorafgaand aan de versleuteling is ingeschakeld.  Zonder een back-up op locatie, kan een onverwachte fout opgetreden tijdens het versleutelen weergegeven van de schijf en VM niet toegankelijk zonder een optie voor siteherstel.  Set-AzureRmVMDiskEncryptionExtension momenteel geen back-up van beheerde schijven en fout wordt als voor een beheerde schijf gebruikt, tenzij de parameter - skipVmBackup is opgegeven.  Deze parameter is veilig om te gebruiken, tenzij een back-up al buiten Azure Disk Encryption is gemaakt.   Als de parameter - skipVmBackup is opgegeven, wordt de cmdlet een back-up van de beheerde schijf voorafgaand aan de versleuteling niet maken.  Daarom wordt deze beschouwd als een verplichte vereisten om te controleren of een back-up van de beheerde schijf dat VM is ingesteld vóór het inschakelen van Azure Disk Encryption in het geval herstel later is vereist.  
> [!NOTE]
 > De parameter - skipVmBackup moet nooit worden gebruikt, tenzij een momentopname of back-up al buiten Azure Disk Encryption is gemaakt. 

* De Azure Disk Encryption-oplossing maakt gebruik van de externe BitLocker-sleutelbeveiliging voor Windows IaaS-VM's. Push een Groepsbeleid die TPM beveiligingstoepassingen afdwingen voor domein is toegevoegd aan virtuele machines, niet. Zie voor meer informatie over het groepsbeleid voor "Toestaan BitLocker zonder een compatibele TPM" [BitLocker Group Policy Reference](https://technet.microsoft.com/library/ee706521).
* BitLocker-Groepsbeleid op virtuele machines van een domein voor de aangepaste omvat de volgende instelling: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption mislukt wanneer u aangepaste instellingen voor Groepsbeleid voor Bitlocker niet compatibel zijn. Op computers die niet het juiste beleid instelling, het nieuwe beleid afdwingen van het nieuwe beleid om bij te werken (gpupdate.exe/Force) en vervolgens opnieuw te starten is mogelijk vereist.  
* Voor het maken van een Azure AD-toepassing, een sleutelkluis maken of instellen van een bestaande sleutelkluis en versleuteling inschakelen, raadpleegt de [vereiste PowerShell-script voor Azure Disk Encryption](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
* Als u wilt configureren met behulp van de Azure CLI-schijfversleuteling-vereisten, Zie [deze Bash-script](https://github.com/ejarvi/ade-cli-getting-started).
* Als u de Azure Backup-service wilt back-up en herstel van versleutelde virtuele machines, als versleuteling is ingeschakeld met Azure Disk Encryption, uw virtuele machines te versleutelen met behulp van de configuratie van de Azure Disk Encryption. De Backup-service biedt ondersteuning voor virtuele machines die zijn versleuteld met behulp van niet-KEK-sleutel of KEK-configuraties. Zie [back-up en herstellen versleutelde virtuele machines met Azure Backup-versleuteling](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

* Bij het versleutelen van het volume met een Linux-besturingssysteem, houd er rekening mee dat een virtuele machine opnieuw opstarten momenteel vereist aan het einde van het proces is. Dit kan worden gedaan via de portal, powershell of CLI.   Voor het volgen van de voortgang van de versleuteling, moet u regelmatig het statusbericht dat is geretourneerd door Get-AzureRmVMDiskEncryptionStatus pollen https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.  Als versleuteling voltooid is, wordt dit door het statusbericht dat is geretourneerd door deze opdracht aangegeven. Bijvoorbeeld, "ProgressMessage: de besturingssysteemschijf is versleuteld, start de virtuele machine ' op dit moment de virtuele machine kan worden gestart en gebruikt.  

* Azure Disk Encryption voor Linux is vereist voor gegevensschijven hebben een gekoppeld bestandssysteem in Linux voordat versleuteling

* Recursief gekoppelde schijven worden niet ondersteund door de Azure Disk Encryption voor Linux. Bijvoorbeeld, als het doelsysteem een schijf is gekoppeld op de balk/foo/en vervolgens een andere op /foo/bar/baz, de versleuteling van /foo/bar/baz slaagt, maar versleuteling van balk/foo/mislukken. 

* Azure Disk Encryption wordt alleen ondersteund op ondersteunde Azure-galerie-installatiekopieën die voldoen aan de bovengenoemde vereisten. Klanten aangepaste installatiekopieën worden niet ondersteund vanwege de proces-gedrag die mogelijk aanwezig zijn op deze installatiekopieën en aangepaste partitieschema. Bovendien is zelfs galerijafbeelding op basis van VM's die in eerste instantie vereisten voldaan maar mogelijk zijn gewijzigd nadat het maken van mogelijk niet compatibel.  Reden, de aanbevolen procedure voor het versleutelen van een Linux-VM is voor die starten vanuit een schone galerijafbeelding, versleutelen van de virtuele machine en vervolgens aangepaste software en gegevens toevoegen aan de virtuele machine zo nodig.  

* Azure Disk Encryption en lokale gegevensvolume - Bek Volume voor Windows- en/mnt/azure_bek_disk voor Linux IaaS-VM's voor het veilig opslaan van de versleutelingssleutel. Niet verwijderen of bewerken van alle inhoud van deze schijf. De schijf niet worden ontkoppeld nadat de belangrijkste aanwezigheid codering nodig is voor versleutelingsbewerkingen op de IaaS-VM. Leesmij-bestand die zijn opgenomen in het volume bevat meer informatie.

#### <a name="set-up-the-azure-ad-application-in-azure-active-directory"></a>Instellen van de Azure AD-toepassing in Azure Active Directory
Wanneer u versleuteling wordt ingeschakeld op een actieve virtuele machine in Azure nodig hebt, wordt Azure Disk Encryption genereert en schrijft de versleutelingssleutels in uw key vault. Beheer van versleutelingssleutels in uw key vault, vereist Azure AD-verificatie.

Maak een Azure AD-toepassing voor dit doel. U vindt gedetailleerde stappen voor het registreren van een toepassing in de sectie 'Een identiteit voor de toepassing ophalen' van de blogpost [Azure Key Vault - stap voor stap](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Dit bericht bevat ook een aantal handige voorbeelden voor het instellen en configureren uw key vault. Voor verificatiedoeleinden wordt gebruikt, kunt u op basis van een geheim clientverificatie of clientverificatie Azure AD op basis van certificaten.

#### <a name="client-secret-based-authentication-for-azure-ad"></a>Clientverificatie op basis van een geheim voor Azure AD
De volgende secties kunt u een client op basis van een geheim verificatie voor Azure AD configureren.

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a>Een Azure AD-toepassing maken met behulp van Azure PowerShell
Gebruik de volgende PowerShell-cmdlet om een Azure AD-toepassing te maken:

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> $azureAdApplication.ApplicationId is de Azure AD-ClientID en $aadClientSecret is het clientgeheim die u later gebruiken moet om Azure Disk Encryption inschakelen. Het clientgeheim van de Azure AD op de juiste manier beveiligen.

##### <a name="setting-up-the-azure-ad-client-id-and-secret-from-the-azure-portal"></a>Instellen van het Azure AD-client-ID en het geheim van de Azure-portal
U kunt ook instellen van uw Azure AD-client-ID en -geheim met behulp van de Azure-Portal. Om deze taak uitvoeren, het volgende doen:

1. Selecteer **alle Services > Azure Active Directory**

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/aad-service.png)

2. Selecteer **App-registraties > nieuwe toepassing registreren**

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/aad-app-registration.png)

3. Geef de gevraagde informatie en de toepassing maken:

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/aad-create-app.png)

4. Selecteer de zojuist gemaakte toepassing om weer te geven van de eigenschappen, met inbegrip van toepassings-ID.  Voor het maken van een sleutel voor de toepassing selecteert **instellingen > sleutels**, een beschrijving en de vervaldatum voor de sleutel toevoegen en klik op **opslaan**

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/aad-create-pw.png)

5. De gegenereerde geheime waarde kopiëren en op de juiste manier beveiligen.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/aad-save-pw.png)


##### <a name="use-an-existing-application"></a>Gebruik van een bestaande toepassing
Voor het uitvoeren van de volgende opdrachten, verkrijgen en gebruiken van de [Azure AD PowerShell-module](https://technet.microsoft.com/library/jj151815.aspx).

> [!NOTE]
> De volgende opdrachten moeten worden uitgevoerd vanaf een nieuwe PowerShell-venster. Gebruik Azure PowerShell of de Azure Resource Manager-venster niet aan de opdrachten uit te voeren. We raden deze benadering omdat deze cmdlets in de MSOnline-module of Azure AD PowerShell worden.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Verificatie op basis van certificaten voor Azure AD
> [!NOTE]
> Azure AD-certificaten gebaseerde verificatie wordt momenteel niet ondersteund op virtuele Linux-machines.

De volgende secties laten zien hoe het configureren van een verificatie op basis van certificaten voor Azure AD.

##### <a name="create-an-azure-ad-application"></a>Een Azure AD-toepassing maken
Voer de volgende PowerShell-cmdlets voor het maken van een Azure AD-toepassing:

> [!NOTE]
> Vervang de volgende `yourpassword` tekenreeks met uw beveiligd wachtwoord en het wachtwoord te beveiligen.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Nadat u deze stap hebt voltooid, wordt een PFX-bestand uploaden naar uw key vault en inschakelen van het toegangsbeleid die nodig zijn voor het implementeren van dat certificaat aan een virtuele machine.

##### <a name="use-an-existing-azure-ad-application"></a>Gebruik van een bestaande Azure AD-toepassing
Als u verificatie op basis van certificaten voor een bestaande toepassing configureert, gebruikt u de PowerShell-cmdlets die hier worden weergegeven. Zorg ervoor dat ze worden uitgevoerd vanuit een nieuwe PowerShell-venster.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Nadat u deze stap hebt voltooid, wordt een PFX-bestand uploaden naar uw key vault en het beleid dat nodig is voor het implementeren van het certificaat aan een virtuele machine inschakelen.

##### <a name="upload-a-pfx-file-to-your-key-vault"></a>Een PFX-bestand uploaden naar uw key vault
Zie voor een gedetailleerde uitleg van dit proces [de officiële Azure Key Vault-teamblog](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx). De volgende PowerShell-cmdlets zijn echter Alles die wat u nodig hebt voor de taak. Zorg ervoor dat ze worden uitgevoerd vanuit Azure PowerShell-console.

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

##### <a name="deploy-a-certificate-in-your-key-vault-to-an-existing-vm"></a>Implementeer een certificaat in uw key vault op een bestaande virtuele machine
Nadat u klaar bent met het uploaden van de PFX, implementeert u een certificaat in de key vault aan een bestaande virtuele machine met het volgende:
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

#### <a name="set-up-the-key-vault-access-policy-for-the-azure-ad-application"></a>Het toegangsbeleid voor key vault voor de Azure AD-toepassing instellen
Uw Azure AD-toepassing moet de rechten voor toegang tot de sleutels of geheimen in de kluis. Gebruik de [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) cmdlet voor het verlenen van machtigingen voor de toepassing, met behulp van de client-ID (die is gegenereerd toen de toepassing is geregistreerd) als de _– ServicePrincipalName_ parameterwaarde. Zie voor meer informatie het blogbericht [Azure Key Vault - stap voor stap](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Hier volgt een voorbeeld van hoe u deze taak via PowerShell uit te voeren:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> Azure Disk Encryption moet u het volgende toegangsbeleid naar uw Azure AD-clienttoepassing configureren: _WrapKey_ en _ingesteld_ machtigingen.

## <a name="terminology"></a>Terminologie
Voor meer informatie over enkele van de algemene voorwaarden die worden gebruikt door deze technologie, gebruik de volgende tabel voor de terminologie:

| Terminologie | Definitie |
| --- | --- |
| Azure AD | Azure AD is [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Een Azure AD-account is een vereiste voor verificatie, opslaan en ophalen van geheimen van een key vault. |
| Azure Key Vault | Key Vault is een cryptografische, key management-service die gebaseerd op de Federal Information Processing Standards FIPS gevaliseerde hardware security modules, die beter te beveiligen van uw cryptografische sleutels en gevoelige geheimen. Zie voor meer informatie, [Key Vault](https://azure.microsoft.com/services/key-vault/) documentatie. |
| ARM | Azure Resource Manager |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is een industrie herkend Windows volume versleutelingstechnologie die wordt gebruikt om in te schakelen schijfversleuteling voor IaaS-VM's van Windows. |
| BEK | BitLocker-versleutelingssleutels worden gebruikt voor het versleutelen van het opstartvolume OS en gegevensvolumes. De BitLocker-sleutels worden beveiligd in een key vault als geheimen. |
| CLI | Zie [Azure-opdrachtregelinterface](../cli-install-nodejs.md). |
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is het subsysteem voor Linux gebaseerde, transparante schijfversleuteling die wordt gebruikt om in te schakelen schijfversleuteling op Linux IaaS-VM's. |
| KEK-SLEUTEL | Sleutel van versleutelingssleutel is een asymmetrische sleutel (RSA 2048) die u gebruiken kunt om te beveiligen of de geheime sleutel verpakken. U krijgt u een hardware security modules (HSM)- of software beschermde sleutel beschermd. Zie voor meer informatie, [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentatie. |
| PS-cmdlets | Zie [Azure PowerShell-cmdlets](/powershell/azure/overview). |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a>Instellen en configureren uw key vault voor Azure Disk Encryption
Azure Disk Encryption helpt sleutels en geheimen in uw key vault beveiligen de schijfversleuteling. Als u uw key vault voor Azure Disk Encryption instelt, de stappen in elk van de volgende secties.

#### <a name="create-a-key-vault"></a>Een sleutelkluis maken
Voor het maken van een key vault, gebruikt u een van de volgende opties:

* [Resource Manager-sjabloon '101-Key-Vault-maken'](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [Azure key vault PowerShell-cmdlets](/powershell/module/azurerm.keyvault/#key_vault)
* Azure Resource Manager
* Hoe u [uw key vault beveiligen](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)

> [!NOTE]
> Als u al een key vault voor uw abonnement instellen hebt, gaat u naar de volgende sectie.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a>Instellen van een sleutel van versleutelingssleutel (optioneel)
Als u een KEK-sleutel voor een extra beveiligingslaag voor het BitLocker-versleutelingssleutels te gebruiken wilt, moet u een KEK-sleutel toevoegen aan uw key vault. Gebruik de [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet voor het maken van een sleutel van versleutelingssleutel in de key vault. U kunt ook een KEK-sleutel van uw on-premises Sleutelbeheer HSM importeren. Zie voor meer informatie, [Key Vault-documentatie](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

U kunt de KEK-sleutel toevoegen door te gaan naar Azure Resource Manager of met behulp van de interface van uw sleutelkluis.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a>Key vault-machtigingen instellen
Het Azure-platform moet toegang hebben tot de sleutels of geheimen in uw key vault zodat ze beschikbaar voor de virtuele machine voor opstarten en ontsleutelen van de volumes. Instellen om machtigingen te verlenen aan het Azure-platform, de **EnabledForDiskEncryption** eigenschap in de key vault met behulp van de key vault PowerShell-cmdlet:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

U kunt ook instellen de **EnabledForDiskEncryption** eigenschap door naar de pagina de [Azure Resource Explorer](https://resources.azure.com).

Zoals eerder vermeld, moet u instellen de **EnabledForDiskEncryption** eigenschap op uw key vault. Anders mislukt de implementatie.

U kunt beleidsregels voor toegang instellen voor uw Azure AD-toepassing uit de sleutelkluis-interface, zoals hier wordt weergegeven:

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Op de **geavanceerde Toegangsbeleidsregels** tabblad, zorg ervoor dat uw key vault voor Azure Disk Encryption is ingeschakeld:

![Azure key vault](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Schijfversleuteling implementatiescenario's en gebruikerservaringen
U kunt veel schijfversleuteling scenario's inschakelen en de stappen kunnen variëren op basis van het scenario. De volgende secties voor de scenario's in meer detail.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-the-marketplace"></a>Schakelt u versleuteling op nieuwe IaaS-VM's die zijn gemaakt vanuit de Marketplace
U kunt schijfversleuteling voor de nieuwe Windows van IaaS-VM vanuit de Marketplace in Azure inschakelen met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

1. Klik op de Azure quick start-sjabloon, **implementeren in Azure**, voert u de versleutelingsconfiguratie van de op de **Parameters** blade en klik vervolgens op **OK**.

2. Selecteer het abonnement, de resourcegroep, de locatie voor resourcegroep, de juridische voorwaarden en de overeenkomst en klik vervolgens op **maken** versleuteling op een nieuwe IaaS-VM inschakelen.

> [!NOTE]
> Deze sjabloon maakt een nieuwe VM voor het maken van versleutelde Windows die gebruikmaakt van de installatiekopie van de galerie met Windows Server 2012.

U kunt schijfversleuteling op een nieuwe IaaS Red Hat Linux 7.2 met een matrix van 200 GB RAID-0 inschakelen met behulp van dit [Resource Manager-sjabloon](https://aka.ms/fde-rhel). Nadat u de sjabloon hebt geïmplementeerd, Controleer of de status van de VM-versleuteling met behulp van de `Get-AzureRmVmDiskEncryptionStatus` cmdlet, zoals beschreven in [versleutelen OS-stations op een actieve Linux-VM](#encrypting-os-drive-on-a-running-linux-vm). Wanneer de machine de status retourneert _VMRestartPending_, start de VM opnieuw.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor nieuwe virtuele machines uit de Marketplace-scenario met behulp van Azure AD-client-ID:

| Parameter | Beschrijving |
| --- | --- |
| adminUserName | De beheerdersnaam voor de virtuele machine. |
| adminPassword | Beheerderswachtwoord voor de virtuele machine. |
| newStorageAccountName | De naam van het opslagaccount voor het opslaan van besturingssysteem- en VHD's. |
| vmSize | Grootte van de virtuele machine. Op dit moment worden alleen Standard A, D en G-serie ondersteund. |
| virtualNetworkName | De naam van het VNet waarmee de VM NIC tot behoren moet. |
| subnetName | De naam van het subnet in het VNet waarmee de VM NIC tot behoren moet. |
| AADClientID | Client-ID van de Azure AD-toepassing die gemachtigd is te schrijven geheimen voor uw key vault. |
| AADClientSecret | Clientgeheim van de Azure AD-toepassing met machtigingen voor het schrijven van geheimen voor uw key vault. |
| keyVaultURL | URL van de sleutelkluis waarin de BitLocker-sleutel moet worden geüpload naar. U kunt deze ophalen met behulp van de cmdlet `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`. |
| keyEncryptionKeyURL | URL van de sleutel van versleutelingssleutel die wordt gebruikt voor het versleutelen van de gegenereerde BitLocker-sleutel (optioneel). |
| keyVaultResourceGroup | De resourcegroep van de key vault. |
| vmName | De naam van de virtuele machine die de versleutelingsbewerking moet worden uitgevoerd op. |

> [!NOTE]
> _KeyEncryptionKeyURL_ is een optionele parameter. U brengen uw eigen KEK naar verder beschermen de versleutelingssleutel voor servicegegevens (geheime wachtwoordzin) in uw key vault.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Schakelt u versleuteling op nieuwe IaaS-VM's die zijn gemaakt op basis van VHD klant versleuteld en versleutelingssleutels
In dit scenario kunt u inschakelen met behulp van de Resource Manager-sjabloon, de PowerShell-cmdlets of de CLI-opdrachten te coderen. De volgende secties wordt uitgelegd in meer detail de Resource Manager-sjabloon en de CLI-opdrachten.

Volg de instructies uit een van deze secties voor het maken van vooraf gecodeerde-installatiekopieën die kunnen worden gebruikt in Azure. Nadat de installatiekopie is gemaakt, kunt u de stappen in de volgende sectie om een versleutelde Azure-VM te maken.

* [Vooraf gecodeerde Windows VHD voorbereiden](#preparing-a-pre-encrypted-windows-vhd)
* [Vooraf gecodeerde Linux-VHD voorbereiden](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-the-resource-manager-template"></a>Met behulp van de Resource Manager-sjabloon
U kunt schijfversleuteling op uw versleutelde VHD inschakelen met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. Klik op de Azure quick start-sjabloon, **implementeren in Azure**, voert u de versleutelingsconfiguratie van de op de **Parameters** blade en klik vervolgens op **OK**.

2. Selecteer het abonnement, de resourcegroep, de locatie voor resourcegroep, de juridische voorwaarden en de overeenkomst en klik vervolgens op **maken** versleuteling op de nieuwe IaaS-VM inschakelen.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor uw versleutelde VHD:

| Parameter | Beschrijving |
| --- | --- |
| newStorageAccountName | De naam van het opslagaccount voor het opslaan van de versleutelde OS VHD. Dit opslagaccount moet al zijn gemaakt in dezelfde resourcegroep en dezelfde locatie als de virtuele machine. |
| osVhdUri | De URI van de VHD met het besturingssysteem van het opslagaccount. |
| besturingssysteemtype | Type besturingssysteem product (Windows/Linux). |
| virtualNetworkName | De naam van het VNet waarmee de VM NIC tot behoren moet. De naam moet al zijn gemaakt in dezelfde resourcegroep en dezelfde locatie als de virtuele machine. |
| subnetName | Naam van het subnet in het VNet waarmee de VM NIC tot behoren moet. |
| vmSize | Grootte van de virtuele machine. Op dit moment worden alleen Standard A, D en G-serie ondersteund. |
| keyVaultResourceID | De ResourceID die de sleutelkluis-resource in Azure Resource Manager. U kunt deze ophalen met behulp van de PowerShell-cmdlet `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`. |
| keyVaultSecretUrl | URL van de schijf-versleutelingssleutel op die ingesteld in de key vault. |
| keyVaultKekUrl | URL van de sleutel van versleutelingssleutel voor het versleutelen van de gegenereerde versleutelingssleutel van de schijf. |
| vmName | De naam van de IaaS-VM. |

#### <a name="using-powershell-cmdlets"></a>Met behulp van PowerShell-cmdlets
U kunt schijfversleuteling op uw versleutelde VHD inschakelen met behulp van de PowerShell-cmdlet [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).  

#### <a name="using-cli-commands"></a>Met behulp van CLI-opdrachten
Om in te schakelen schijfversleuteling voor dit scenario met behulp van CLI-opdrachten, het volgende doen:

1. Toegangsbeleid in uw key vault instellen:

   * Stel de **EnabledForDiskEncryption** vlag:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Machtigingen instellen voor Azure AD-toepassing geheimen schrijven naar uw key vault:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Het inschakelen van versleuteling op een bestaande of actieve virtuele machine, typt u:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Versleutelingsstatus ophalen:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Het inschakelen van versleuteling op een nieuwe virtuele machine van de versleutelde VHD, gebruikt u de volgende parameters op in de `azure vm create` opdracht:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Schakelt u versleuteling op bestaande of uitvoeren van IaaS Windows VM in Azure
In dit scenario kunt u inschakelen met behulp van de Resource Manager-sjabloon, de PowerShell-cmdlets of de CLI-opdrachten te coderen. De volgende secties wordt in meer detail uitgelegd hoe u deze inschakelen met behulp van de Resource Manager-sjabloon en de CLI-opdrachten.

#### <a name="using-the-resource-manager-template"></a>Met behulp van de Resource Manager-sjabloon
U kunt inschakelen schijfversleuteling voor de bestaande of IaaS Windows-VM's uitvoeren in Azure met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).

1. Klik op de Azure quick start-sjabloon, **implementeren in Azure**, voert u de versleutelingsconfiguratie van de op de **Parameters** blade en klik vervolgens op **OK**.

2. Selecteer het abonnement, de resourcegroep, de locatie voor resourcegroep, de juridische voorwaarden en de overeenkomst en klik vervolgens op **maken** versleuteling op de bestaande of actieve IaaS-VM inschakelen.

De volgende tabel bevat de Resource Manager-Sjabloonparameters voor bestaande of uitvoeren van virtuele machines die gebruikmaken van een Azure AD-client-ID:

| Parameter | Beschrijving |
| --- | --- |
| AADClientID | Client-ID van de Azure AD-toepassing met machtigingen voor geheimen schrijven naar de key vault. |
| AADClientSecret | Clientgeheim van de Azure AD-toepassing met machtigingen voor geheimen schrijven naar de key vault. |
| keyVaultName | De naam van de sleutelkluis waarin de BitLocker-sleutel moet worden geüpload naar. U kunt deze ophalen met behulp van de cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | URL van de sleutel van versleutelingssleutel die wordt gebruikt voor het versleutelen van de gegenereerde BitLocker-sleutel. Deze parameter is optioneel als u selecteert **nokek** in de vervolgkeuzelijst UseExistingKek. Als u selecteert **kek** in de vervolgkeuzelijst UseExistingKek voert u de _keyEncryptionKeyURL_ waarde. |
| volumeType | Het type volume dat de versleutelingsbewerking wordt uitgevoerd op. Geldige waarden zijn _OS_, _gegevens_, en _alle_. |
| sequenceVersion | De versie van de volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens wanneer een schijfversleuteling bewerking wordt uitgevoerd op dezelfde VM. |
| vmName | De naam van de virtuele machine die de versleutelingsbewerking moet worden uitgevoerd op. |

> [!NOTE]
> _KeyEncryptionKeyURL_ is een optionele parameter. U kunt uw eigen KEK overbrengen naar verdere beveiligen de gegevensversleutelingssleutel (geheime BitLocker-versleuteling) in de key vault.

#### <a name="using-powershell-cmdlets"></a>Met behulp van PowerShell-cmdlets
Zie voor informatie over het inschakelen van versleuteling met Azure Disk Encryption met behulp van PowerShell-cmdlets, de blogberichten [Azure Disk Encryption verkennen met Azure PowerShell - deel 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) en [Azure Disk Encryption verkennen met Azure PowerShell - deel 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

#### <a name="using-cli-commands"></a>Met behulp van CLI-opdrachten
Het inschakelen van versleuteling op bestaande of uitvoeren van IaaS Windows VM in Azure met behulp van CLI-opdrachten, het volgende doen:

1. Toegangsbeleid instellen in de key vault:
   * Stel de **EnabledForDiskEncryption** vlag:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Machtigingen instellen voor Azure AD-toepassing geheimen schrijven naar uw key vault:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. Versleuteling op een bestaande of actieve virtuele machine inschakelen:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. Versleutelingsstatus ophalen:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. Het inschakelen van versleuteling op een nieuwe virtuele machine van de versleutelde VHD, gebruikt u de volgende parameters op in de `azure vm create` opdracht:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a>Schakelt u versleuteling op een bestaande of actieve IaaS Linux VM in Azure
U kunt schijfversleuteling op een bestaande of actieve IaaS Linux VM in Azure inschakelen met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. Klik op **implementeren in Azure** op de Azure quick start-sjabloon, voert u de versleutelingsconfiguratie van de op de **Parameters** blade en klik vervolgens op **OK**.

2. Selecteer het abonnement, de resourcegroep, de locatie voor resourcegroep, de juridische voorwaarden en de overeenkomst en klik vervolgens op **maken** versleuteling op de bestaande of actieve IaaS-VM inschakelen.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor bestaande of uitvoeren van virtuele machines die gebruikmaken van een Azure AD-client-ID:

| Parameter | Beschrijving |
| --- | --- |
| AADClientID | Client-ID van de Azure AD-toepassing met machtigingen voor geheimen schrijven naar de key vault. |
| AADClientSecret | Clientgeheim van de Azure AD-toepassing met machtigingen voor het schrijven van geheimen voor uw key vault. |
| keyVaultName | De naam van de sleutelkluis waarin de BitLocker-sleutel moet worden geüpload naar. U kunt deze ophalen met behulp van de cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | URL van de sleutel van versleutelingssleutel die wordt gebruikt voor het versleutelen van de gegenereerde BitLocker-sleutel. Deze parameter is optioneel als u selecteert **nokek** in de vervolgkeuzelijst UseExistingKek. Als u selecteert **kek** in de vervolgkeuzelijst UseExistingKek voert u de _keyEncryptionKeyURL_ waarde. |
| volumeType | Het type volume dat de versleutelingsbewerking wordt uitgevoerd op. Geldige ondersteunde waarden zijn _OS_ of _alle_ (Zie ondersteund Linux-distributies en hun versies voor het besturingssysteem en gegevensschijven in prerequisiteis sectie eerder). |
| sequenceVersion | De versie van de volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens wanneer een schijfversleuteling bewerking wordt uitgevoerd op dezelfde VM. |
| vmName | De naam van de virtuele machine die de versleutelingsbewerking moet worden uitgevoerd op. |
| Wachtwoordzin | Typ een sterke wachtwoordzin in als de versleutelingssleutel voor servicegegevens. |

> [!NOTE]
> _KeyEncryptionKeyURL_ is een optionele parameter. U brengen uw eigen KEK naar verder beschermen de versleutelingssleutel voor servicegegevens (geheime wachtwoordzin) in uw key vault.

#### <a name="cli-commands"></a>CLI-opdrachten
U kunt schijfversleuteling inschakelen op uw versleutelde VHD te installeren en gebruiken de [CLI-opdracht](../cli-install-nodejs.md). Het inschakelen van versleuteling op bestaande of IaaS virtuele Linux-machines uitvoeren in Azure met behulp van CLI-opdrachten, het volgende doen:

1. Toegangsbeleid instellen in de key vault:

 * Stel de **EnabledForDiskEncryption** vlag:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * Machtigingen instellen voor Azure AD-toepassing geheimen schrijven naar uw key vault:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Versleuteling op een bestaande of actieve virtuele machine inschakelen:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Versleutelingsstatus ophalen:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Het inschakelen van versleuteling op een nieuwe virtuele machine van de versleutelde VHD, gebruikt u de volgende parameters op in de `azure vm create` opdracht:
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-the-encryption-status-of-an-encrypted-iaas-vm"></a>De status voor schijfversleuteling van een versleutelde IaaS VM ophalen
U kunt de coderingsstatus ophalen met behulp van Azure Resource Manager [PowerShell-cmdlets](/powershell/azure/overview), of de CLI-opdrachten. De volgende secties wordt uitgelegd hoe de Azure Portal en CLI-opdrachten gebruiken om op te halen van de status voor schijfversleuteling.

#### <a name="get-the-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a>Ophalen van de status voor schijfversleuteling van een versleutelde Windows-VM met behulp van Azure Resource Manager
U kunt de status voor schijfversleuteling van de IaaS-VM van Azure Resource Manager krijgen door het volgende te doen:

1. Aanmelden bij de [Azure Portal](https://portal.azure.com/), en klik vervolgens op **virtuele machines** in het linkerdeelvenster om te zien van een samenvatting weergegeven van de virtuele machines in uw abonnement. U kunt de virtuele machines filteren door de naam van het abonnement in de **abonnement** vervolgkeuzelijst.

2. Aan de bovenkant van de **virtuele machines** pagina, klikt u op **kolommen**.

3. Op de **kiezen kolom** Selecteer **schijfversleuteling**, en klik vervolgens op **Update**. U ziet de schijfversleuteling kolom met de status van de versleuteling _ingeschakeld_ of _niet ingeschakeld_ voor elke virtuele machine, zoals wordt weergegeven in de volgende afbeelding:

 ![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-the-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-the-disk-encryption-powershell-cmdlet"></a>De status voor schijfversleuteling van een versleutelde (Windows/Linux) IaaS VM ophalen met behulp van de PowerShell-cmdlet schijfversleuteling
U kunt de status voor schijfversleuteling van de IaaS-VM ophalen uit de PowerShell-cmdlet schijfversleuteling `Get-AzureRmVMDiskEncryptionStatus`. Als u de versleutelingsinstellingen voor uw virtuele machine, voert u het volgende:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

U kunt de uitvoer van inspecteren _Get-AzureRmVMDiskEncryptionStatus_ sleutel voor versleuteling van URL's.

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

De instellingen voor OSVolumeEncrypted en DataVolumesEncrypted waarden zijn ingesteld op _versleutelde_, waarin wordt getoond dat beide volumes zijn versleuteld met Azure Disk Encryption. Zie voor informatie over het inschakelen van versleuteling met Azure Disk Encryption met behulp van PowerShell-cmdlets, de blogberichten [Azure Disk Encryption verkennen met Azure PowerShell - deel 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) en [Azure Disk Encryption verkennen met Azure PowerShell - deel 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

> [!NOTE]
> Op Linux-VM's, duurt het drie tot vier minuten voor de `Get-AzureRmVMDiskEncryptionStatus` cmdlet om de versleutelingsstatus te rapporteren.

#### <a name="get-the-encryption-status-of-the-iaas-vm-from-the-disk-encryption-cli-command"></a>De coderingsstatus van de IaaS-VM ophalen uit de schijfversleuteling CLI-opdracht
U krijgt de status voor schijfversleuteling van de IaaS-VM met behulp van de CLI-opdracht voor schijfversleuteling `azure vm show-disk-encryption-status`. Als u de versleutelingsinstellingen voor uw virtuele machine, voert u Azure CLI-sessie:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Schakel versleuteling uit op het uitvoeren van Windows IaaS-VM
U kunt uitschakelen van versleuteling op een actieve Windows of Linux IaaS-VM via de Azure Disk Encryption Resource Manager-sjabloon of het PowerShell-cmdlets en geeft u de configuratie voor ontsleuteling.

##### <a name="windows-vm"></a>Windows-VM
De stap uitschakelen-codering Hiermee schakelt u versleuteling van het besturingssysteem, het gegevensvolume of beide op de actieve Windows IaaS-virtuele machine. U kunt geen volume met het besturingssysteem uitschakelen en laat u het aantal gegevens versleuteld. Wanneer de stap uitschakelen-versleuteling wordt uitgevoerd, het model van de klassieke Azure-implementatie werkt het servicemodel van de virtuele machine en de Windows IaaS-VM is gemarkeerd als ontsleutelde. De inhoud van de virtuele machine worden niet meer in rust versleuteld. Uw key vault en het sleutelmateriaal versleuteling (BitLocker-versleutelingssleutels voor Windows en de wachtwoordzin voor Linux) worden niet door de ontsleuteling worden verwijderd.

##### <a name="linux-vm"></a>Linux-VM
De stap uitschakelen-codering Hiermee schakelt u versleuteling van het gegevensvolume van de op de actieve virtuele machine voor Linux-IaaS. Deze stap werkt alleen als de besturingssysteemschijf is niet versleuteld.

> [!NOTE]
> Uitschakelen van versleuteling op de besturingssysteemschijf is niet toegestaan voor virtuele Linux-machines.

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Schakel versleuteling uit op een bestaande of actieve IaaS-VM
U kunt uitschakelen schijfversleuteling voor IaaS-VM's van Windows met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).

1. Klik op de Azure quick start-sjabloon, **implementeren in Azure**, voert u de configuratie van de ontsleuteling op de **Parameters** blade en klik vervolgens op **OK**.

2. Selecteer het abonnement, de resourcegroep, de locatie voor resourcegroep, de juridische voorwaarden en de overeenkomst en klik vervolgens op **maken** versleuteling op een nieuwe IaaS-VM inschakelen.

Voor virtuele Linux-machines, kunt u versleuteling uitschakelen met behulp van de [Schakel versleuteling uit op een actieve Linux VM](https://aka.ms/decrypt-linuxvm) sjabloon.

De volgende tabel bevat de parameters van de Resource Manager-sjabloon voor het uitschakelen van versleuteling op een actieve IaaS-VM:

| Parameter | Beschrijving |
| --- | --- |
| vmName | De naam van de virtuele machine die de versleutelingsbewerking moet worden uitgevoerd op.
| volumeType | Het type volume dat een ontsleutelingsbewerking op wordt uitgevoerd. Geldige waarden zijn _OS_, _gegevens_, en _alle_. U kunt versleuteling voor het uitvoeren van Windows IaaS-VM OS/opstartvolume zonder het uitschakelen van versleuteling op niet uitschakelen de _gegevens_ volume. Houd er ook rekening mee dat het uitschakelen van versleuteling op de besturingssysteemschijf is niet toegestaan voor virtuele Linux-machines. |
| sequenceVersion | De versie van de volgorde van de BitLocker-bewerking. Dit versienummer verhoogd telkens als een schijf ontsleuteling-bewerking wordt uitgevoerd op dezelfde VM. |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Schakel versleuteling uit op een bestaande of actieve IaaS-VM
Zie voor informatie over het uitschakelen van versleuteling op een bestaande of actieve IaaS-VM met behulp van de PowerShell-cmdlet [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). Deze cmdlet biedt ondersteuning voor zowel Windows als Linux-VM's. Als u wilt uitschakelen versleuteling, wordt een extensie geïnstalleerd op de virtuele machine. Als de _naam_ parameter niet wordt opgegeven, een uitbreiding met de naam van de standaard _AzureDiskEncryption voor Windows VM's_ wordt gemaakt.

Op Linux-VM's, wordt de AzureDiskEncryptionForLinux-extensie gebruikt.

> [!NOTE]
> Deze cmdlet opnieuw opgestart voor de virtuele machine.

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a>Schakelt u versleuteling op IaaS-VM vooraf zijn versleuteld met door Azure beheerde schijf
De Azure beheerde schijf ARM-sjabloon gebruiken om te maken van een versleutelde VM van een vooraf gecodeerde VHD met behulp van de ARM-sjabloon te vinden op   
[Maak een nieuwe versleutelde beheerde schijf van een vooraf gecodeerde VHD/opslag-blob] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a>Schakelt u versleuteling op een nieuwe Linux IaaS-VM met Azure beheerde schijf
De Azure beheerde schijf ARM-sjabloon gebruiken voor een nieuwe versleutelde Linux IaaS-VM maken met behulp van de ARM-sjabloon te vinden op   
[Implementatie van RHEL 7.2 met volledige schijfversleuteling] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a>Schakelt u versleuteling op een nieuwe Windows IaaS-VM met Azure beheerde schijf
 De Azure beheerde schijf ARM-sjabloon gebruiken voor een nieuwe versleutelde Linux IaaS-VM maken met behulp van de ARM-sjabloon te vinden op   
 [Maak een nieuwe versleutelde Windows IaaS beheerde schijf VM uit galerijafbeelding] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)

  > [!NOTE]
  >Dit is verplicht op momentopname en/of back-up een beheerde schijf op basis van de VM-exemplaar buiten en voorafgaand aan Azure Disk Encryption inschakelen.  Een momentopname van de beheerde schijf kan worden uitgevoerd vanuit de portal of Azure Backup kan worden gebruikt.  Back-ups zorgen ervoor dat een optie voor siteherstel mogelijk in het geval van een onverwachte fout opgetreden tijdens het versleutelen is.  Als u een back-up is gemaakt, kan de cmdlet Set-AzureRmVMDiskEncryptionExtension worden gebruikt voor het versleutelen van beheerde schijven met de parameter - skipVmBackup op te geven.  Met deze opdracht uitvoeren op basis van de beheerde schijf op basis van de virtuele machine totdat een back-up is gemaakt en deze parameter is opgegeven.    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a>Instellingen voor versleuteling van een bestaande versleutelde niet-premium VM bijwerken
  Gebruik de bestaande Azure-schijf encryptie-ondersteunde interfaces voor het uitvoeren van VM [PS-cmdlets, CLI of ARM-sjablonen] bijwerken van de versleutelingsinstellingen zoals AAD client-ID en-geheim, Key-versleutelingssleutel [KEK], BitLocker-versleutelingssleutel voor Windows virtuele machine of de wachtwoordzin voor Linux-VM enzovoort. De versleutelingsinstelling voor de update wordt ondersteund voor zowel premium als niet-premium storage-VM's.

## <a name="appendix"></a>Bijlage
### <a name="connect-to-your-subscription"></a>Verbinding maken met uw abonnement
Voordat u doorgaat, controleert u de *vereisten* sectie in dit artikel. Nadat u ervoor zorgen dat alle vereisten wordt voldaan, kunt u verbinding met uw abonnement door het volgende te doen:

1. Start een Azure PowerShell-sessie en meld u aan bij uw Azure-account met de volgende opdracht:

    `Connect-AzureRmAccount`

2. Als u meerdere abonnementen hebt en wilt opgeven om te gebruiken, typt u het volgende als u wilt zien van de abonnementen voor uw account:

    `Get-AzureRmSubscription`

3. Als u het abonnement dat u wilt gebruiken, typt u:

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. Om te controleren of het abonnement geconfigureerd juist is, typt u:

    `Get-AzureRmSubscription`

5. Om te bevestigen dat de Azure Disk Encryption-cmdlets zijn geïnstalleerd, typt u:

    `Get-command *diskencryption*`

6. De volgende uitvoer bevestigt dat de installatie van de Azure Disk Encryption PowerShell:

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a>Vooraf gecodeerde Windows VHD voorbereiden
De volgende secties zijn die nodig zijn voor het vooraf gecodeerde Windows VHD voorbereiden voor implementatie als een versleutelde VHD in Azure IaaS. Gebruik de informatie voor het voorbereiden en start een nieuwe Windows virtuele machine (VHD) voor Azure Site Recovery of Azure.

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Bijwerken van Groepsbeleid om toe te staan zonder TPM voor OS-beveiliging
Configureren van de groepsbeleidsinstellingen voor BitLocker-instelling **BitLocker-stationsversleuteling**, die u vindt hier onder **lokaal computerbeleid** > **Computerconfiguratie**  >  **Beheersjablonen** > **Windows-onderdelen**. Wijzig deze instelling in **besturingssysteem stations** > **aanvullende verificatie bij opstarten vereisen** > **BitLocker zonder een compatibele TPMtoestaan**, zoals wordt weergegeven in de volgende afbeelding:

![Microsoft Antimalware in Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>BitLocker-onderdelen installeren
Voor Windows Server 2012 en hoger, gebruikt u de volgende opdracht:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Voor Windows Server 2008 R2, gebruikt u de volgende opdracht uit:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>Volume met het besturingssysteem voorbereiden voor BitLocker met behulp van `bdehdcfg`
Voor het comprimeren van de OS-partitie en de machine voorbereiden voor BitLocker, voert u de volgende opdracht uit:

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-the-os-volume-by-using-bitlocker"></a>Volume met het besturingssysteem beveiligen met behulp van BitLocker
Gebruik de [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) opdracht schakelt u versleuteling op het opstartvolume met behulp van een externe sleutelbeveiliging. Ook de externe sleutel (.bek-bestand) op de externe schijf of volume plaatsen. Versleuteling is ingeschakeld op het volume/opstarten van het systeem na het opnieuw opstarten.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> De virtuele machine met een afzonderlijke gegevens/resource VHD voorbereiden voor het ophalen van de externe sleutel met BitLocker.

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Een besturingssysteemschijf op een actieve Linux-VM versleutelen

##### <a name="prerequisites-for-os-disk-encryption"></a>Vereisten voor versleuteling van de OS-schijf

* De virtuele machine moet worden met behulp van een distributiepunt dat compatibel is met schijfversleuteling van OS zoals vermeld in de [Veelgestelde vragen over Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq#what-linux-distributions-does-azure-disk-encryption-support) 
* De virtuele machine moet worden gemaakt via de Marketplace-installatiekopie in Azure Resource Manager.
* Azure-VM met ten minste 4 GB aan RAM-geheugen (aanbevolen grootte is 7 GB).
* (Voor RHEL en CentOS) SELinux uitschakelen. Als u wilt uitschakelen SELinux, Zie "4.4.2. Uitschakelen van SELinux' in de [SELinux gebruikers en beheerders van handleiding](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) op de virtuele machine.
* Nadat u SELinux hebt uitgeschakeld, start u de virtuele machine ten minste één keer opnieuw.

##### <a name="steps"></a>Stappen
1. Een virtuele machine maken met behulp van een van de distributies die eerder zijn opgegeven.

 OS-schijfversleuteling wordt ondersteund voor 7.2 CentOS, via een speciale installatiekopie. Geef voor het gebruik van deze installatiekopie, "7.2n' als de SKU bij het maken van de virtuele machine:
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. Configureer de virtuele machine op basis van uw behoeften. Als u schijven voor het versleutelen van alle de (OS + gegevens), de gegevensstations moeten worden opgegeven en koppelbaar uit/etc/fstab gaat.

 > [!NOTE]
 > Gebruik de UUID =... om op te geven gegevensstations in/etc/fstab in plaats van de apparaatnaam blokkeren (bijvoorbeeld/dev/sdb1) op te geven. Tijdens het versleutelen wijzigt de volgorde van de stations op de virtuele machine. Als uw virtuele machine is afhankelijk van een specifieke volgorde van de blokapparaten, mislukt dit ze na versleuteling te koppelen.

3. Afmelden bij de SSH-sessies.

4. Geef voor het versleutelen van het besturingssysteem, volumeType als **alle** of **OS** wanneer u [versleuteling inschakelen](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

 > [!NOTE]
 > Alle gebruikersruimte processen die niet worden uitgevoerd als `systemd` services moeten worden afgesloten met een `SIGKILL`. Start opnieuw op de virtuele machine. Wanneer u versleuteling van OS-schijf op een actieve virtuele machine inschakelt, plan dan op downtime van VM's.

5. Controleer regelmatig de voortgang van versleuteling met behulp van de instructies in de [volgende sectie](#monitoring-os-encryption-progress).

6. Nadat de Get-AzureRmVmDiskEncryptionStatus 'VMRestartPending' ziet, start u uw virtuele machine opnieuw aanmelden bij deze of via de portal, PowerShell of CLI.
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
Voordat u het opnieuw opstarten, wordt aangeraden dat u opslaat [diagnostische gegevens over opstarten](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) van de virtuele machine.

#### <a name="monitoring-os-encryption-progress"></a>Voortgang van de OS-versleuteling controleren
U kunt de OS-versleuteling wordt uitgevoerd op drie manieren controleren:

* Gebruik de `Get-AzureRmVmDiskEncryptionStatus` cmdlet en het veld ProgressMessage inspecteren:
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 Wanneer de virtuele machine bereikt 'OS-schijf versleuteling aan de slag', duurt het ongeveer 40 tot 50 minuten back virtuele machine op een Premium-opslag.

 Vanwege [uitgeven #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent, `OsVolumeEncrypted` en `DataVolumesEncrypted` weergegeven als `Unknown` in bepaalde distributies. Met versie 2.1.5 WALinuxAgent en later, dit probleem automatisch is opgelost. Als u ziet `Unknown` in de uitvoer kunt u schijfversleuteling status controleren met behulp van Azure Resource Explorer.

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

 Schuif omlaag om te zien van de coderingsstatus van uw schijven in de InstanceView.

 ![Instantieweergave van virtuele machine](./media/azure-security-disk-encryption/vm-instanceview.png)

* Bekijk [diagnostische gegevens over opstarten](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/). Berichten van de extensie ADE moeten worden voorafgegaan door `[AzureDiskEncryption]`.

* Meld u aan met de virtuele machine via SSH en ophalen van het extensielogboek uit:

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

 Het is raadzaam dat u niet bij de virtuele machine aanmelden zich terwijl OS-versleuteling uitgevoerd wordt. Kopieer de logboeken alleen als de andere twee methoden zijn mislukt.

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a>Vooraf gecodeerde Linux-VHD voorbereiden
##### <a name="ubuntu-16"></a>Ubuntu 16
Versleuteling tijdens de installatie van softwaredistributie configureren door het volgende te doen:

1. Selecteer **configureren van versleutelde volumes** wanneer u de schijven partitioneren.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Maak een afzonderlijke opstartstation, die niet moet worden versleuteld. Codeer uw basisstation.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Geef een wachtwoordzin. Dit is de wachtwoordzin op die u naar de key vault uploadt.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Voltooi partitioneren.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. Wanneer u de virtuele machine worden opgestart en wordt gevraagd een wachtwoordzin, gebruikt u de wachtwoordzin die u hebt opgegeven in stap 3.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. De virtuele machine voorbereiden voor het uploaden naar Azure met [deze instructies](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). Voer niet de laatste stap (opheffen van inrichting van de virtuele machine) nog.

Versleuteling met Azure werkt als volgt configureren:

1. Maak een bestand onder /usr/local/sbin/azure_crypt_key.sh, met de inhoud in het volgende script. Let op de KeyFileName, omdat het is de bestandsnaam van de wachtwoordzin die wordt gebruikt door Azure.

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

2. Wijzigen van de configuratie van de crypt in */etc/crypttab*. Dit ziet er als volgt uit:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Als u wilt bewerken *azure_crypt_key.sh* in Windows en u hebt gekopieerd naar Linux voert `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

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
6. Voer `update-initramfs -u -k all` om bij te werken van de initramfs waarmee de `keyscript` van kracht.

7. Nu kunt u de virtuele machine inrichting.

 ![Ubuntu 16.04 Setup](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Doorgaan naar de volgende stap en [uw VHD uploaden](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure.

##### <a name="opensuse-132"></a>openSUSE 13.2
Voor het configureren van versleuteling tijdens de installatie van het distributiepunt, het volgende doen:
1. Wanneer u de schijven partitioneren, selecteert u **Volumegroep versleutelen**, en voer een wachtwoord. Dit is het wachtwoord dat u naar de sleutelkluis uploaden zult.

 ![openSUSE 13.2 instellen](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Start de virtuele machine met behulp van uw wachtwoord.

 ![openSUSE 13.2 instellen](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. De virtuele machine voorbereiden voor het uploaden naar Azure door de instructies in [een SLES of opensuse gebaseerde virtuele machine voor Azure voorbereiden](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). Voer niet de laatste stap (opheffen van inrichting van de virtuele machine) nog.

Versleuteling om te werken met Azure, doet u het volgende configureren:
1. Bewerk de /etc/dracut.conf en voeg de volgende regel toe:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Opmerkingen bij de volgende regels aan het einde van het bestand /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
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
5. Voer `/usr/sbin/dracut -f -v` om bij te werken van de initrd.

6. Nu u de inrichting van de virtuele machine kunt ongedaan kunt maken en [uw VHD uploaden](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure.

##### <a name="centos-7"></a>CentOS 7
Voor het configureren van versleuteling tijdens de installatie van het distributiepunt, het volgende doen:
1. Selecteer **mijn gegevens versleutelen** wanneer u schijven partitioneren.

 ![CentOS 7-instellingen](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Zorg ervoor dat **versleutelen** is geselecteerd als de hoofdpartitie.

 ![CentOS 7-instellingen](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Geef een wachtwoordzin. Dit is de wachtwoordzin op die u naar uw key vault uploadt.

 ![CentOS 7-instellingen](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. Wanneer u de virtuele machine worden opgestart en wordt gevraagd een wachtwoordzin, gebruikt u de wachtwoordzin die u hebt opgegeven in stap 3.

 ![CentOS 7-instellingen](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. De virtuele machine voorbereiden voor het uploaden naar Azure met behulp van de instructies "CentOS 7.0 +" in [een CentOS gebaseerde virtuele machine voor Azure voorbereiden](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). Voer niet de laatste stap (opheffen van inrichting van de virtuele machine) nog.

6. Nu u de inrichting van de virtuele machine kunt ongedaan kunt maken en [uw VHD uploaden](#upload-encrypted-vhd-to-an-azure-storage-account) in Azure.

Versleuteling om te werken met Azure, doet u het volgende configureren:

1. Bewerk de /etc/dracut.conf en voeg de volgende regel toe:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Opmerkingen bij de volgende regels aan het einde van het bestand /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
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
4. Bewerk /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh en deze toevoegen na het '# Open LUKS apparaat':
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
5. Voer de ' / usr/sbin/dracut - f - v ' om bij te werken van de initrd.

![CentOS 7-instellingen](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Versleutelde VHD uploaden naar Azure storage-account
Nadat BitLocker-versleuteling of DM-Crypt-versleuteling is ingeschakeld, wordt de lokaal versleutelde VHD moet worden geüpload naar uw storage-account.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-the-disk-encryption-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a>Uploaden van het geheim schijfversleuteling voor de vooraf gecodeerde virtuele machine tot uw key vault
Het geheim voor versleuteling van de schijf die u hebt verkregen moet eerder worden geüpload als een geheim in uw key vault. De key vault moet schijfversleuteling en machtigingen die zijn ingeschakeld voor uw Azure AD-client bevatten.

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Versleutelingsgeheim van schijf niet is versleuteld met een KEK-sleutel
Als u het geheim in uw key vault instelt, gebruikt u [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret). Als u een Windows-machine hebt, de bek-bestand is als een tekenreeks met Base 64-codering en vervolgens geüpload naar uw sleutelkluis met de `Set-AzureKeyVaultSecret` cmdlet. Voor Linux, is de wachtwoordzin in als een tekenreeks met Base 64 gecodeerde en vervolgens geüpload naar de key vault. Bovendien moet u ervoor dat de volgende codes zijn ingesteld wanneer u het geheim in de key vault maakt.

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

Gebruik de `$secretUrl` in de volgende stap voor [de OS-schijf koppelen zonder KEK](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Versleutelingsgeheim van schijf versleuteld met een KEK-sleutel
Voordat u het geheim naar de key vault uploaden, kunt u deze desgewenst versleutelen met behulp van een sleutel van versleutelingssleutel. Gebruik de wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) voor het eerst het geheim met behulp van de sleutel van versleutelingssleutel te versleutelen. De uitvoer van deze bewerking wrap is een tekenreeks met Base 64 URL-codering, die u vervolgens als een geheim uploaden met behulp van kunt de [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.

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

Gebruik `$KeyEncryptionKey` en `$secretUrl` in de volgende stap voor [Bezig met koppelen van de besturingssysteemschijf met behulp van de KEK](#using-a-kek).

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>Geef een URL van accountsleutelgeheim wanneer u een besturingssysteemschijf te koppelen
#### <a name="without-using-a-kek"></a>Zonder een KEK-sleutel
Terwijl u de schijf met het besturingssysteem toevoegen wilt, moet u doorgeven `$secretUrl`. De URL is gegenereerd in de sectie 'schijfversleuteling geheim niet versleuteld met een KEK-sleutel'.

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
Wanneer u de besturingssysteemschijf koppelen, `$KeyEncryptionKey` en `$secretUrl`. De URL is gegenereerd in de sectie 'schijfversleuteling geheim niet versleuteld met een KEK-sleutel'.

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

## <a name="for-more-information"></a>Voor meer informatie
[Azure Disk Encryption verkennen met Azure PowerShell - deel 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[Azure Disk Encryption verkennen met Azure PowerShell - deel 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
