---
title: Azure Disk Encryption met App-vereisten voor Azure AD (vorige versie) | Microsoft Docs
description: Dit artikel bevat vereisten voor het gebruik van Microsoft Azure-schijfversleuteling voor IaaS-VM's.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 10/12/2018
ms.openlocfilehash: 8b0f682e481ef73019d3371af2b84f6270e021ee
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341885"
---
# <a name="azure-disk-encryption-prerequisites-previous-release"></a>Vereisten voor Azure Disk Encryption (vorige versie)

**De nieuwe versie van Azure Disk Encryption wordt voorkomen dat de vereiste voor het ontwikkelen van een Azure AD-toepassing-parameter voor schijfversleuteling van VM inschakelen. Met de nieuwe versie kunt u niet langer moet Azure AD-referenties opgeven tijdens de stap van de versleuteling inschakelen. Alle nieuwe virtuele machines moeten worden versleuteld zonder de parameters van Azure AD-toepassing met behulp van de nieuwe versie. Voor instructies voor het inschakelen van versleuteling van de VM-schijf met behulp van de nieuwe versie, raadpleegt u [vereisten voor Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md). Virtuele machines die al zijn versleuteld met Azure AD-toepassing parameters worden nog steeds ondersteund en worden onderhouden met de syntaxis van de AAD moeten blijven.**

 In dit artikel, de vereisten voor Azure Disk Encryption, wordt beschreven welke items die worden voldaan moet voordat u Azure Disk Encryption kunt gebruiken. Samen met algemene vereisten Azure Disk Encryption is geïntegreerd met [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) en maakt gebruik van een Azure AD-toepassing voor verificatie voor het beheer van versleutelingssleutels in de key vault. U kunt ook meer te gebruiken [Azure PowerShell](/powershell/azure/overview) of de [Azure CLI](/cli/azure/) instellen of configureren van Key Vault en de Azure AD-toepassing.

Voordat u Azure Disk Encryption op Azure IaaS Virtual Machines voor de ondersteunde scenario's die zijn beschreven inschakelen in de [overzicht van Azure Disk Encryption](azure-security-disk-encryption-overview.md) artikel, zorg ervoor dat u de vereisten hebt voldaan. 

> [!NOTE]
> Bepaalde aanbevelingen verhogen gegevens-, netwerk- of computerresources, wat resulteert in extra kosten in licentie of abonnement. U moet een geldige actief Azure-abonnement om resources te maken in Azure in de ondersteunde regio's hebben.


## <a name="bkmk_OSs"></a> Ondersteunde besturingssystemen
Azure Disk Encryption wordt ondersteund op de volgende besturingssystemen:

- Windows Server-versies: WindowsServer 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016.
    - Voor Windows Server 2008 R2 moet u .NET Framework 4.5 zijn geïnstalleerd voordat u versleuteling in Azure inschakelen. Installeer de aanmeldhulp vanaf Windows bijwerken met de optionele update voor Microsoft .NET Framework 4.5.2 voor Windows Server 2008 R2 x64 64-systemen ([KB2901983](https://support.microsoft.com/kb/2901983)).    
- Windows-clientversies: Windows 8-clients en Windows 10-client.
- Azure Disk Encryption is alleen ondersteund op bepaalde Azure-galerie op basis van Linux-server-distributies en versies. Voor de lijst met ondersteunde versies, raadpleegt u de [Veelgestelde vragen over Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).
- Azure Disk Encryption is vereist dat uw sleutelkluis en de virtuele machines zich in de dezelfde Azure-regio en het abonnement bevinden. Configureren van de resources in verschillende regio's, zorgt ervoor dat een fout opgetreden bij het inschakelen van de Azure Disk Encryption-functie.

## <a name="bkmk_LinuxPrereq"></a> Aanvullende vereisten voor Linux Iaas-VM 's 

- Azure Disk Encryption voor Linux is vereist voor 7 GB aan RAM-geheugen op de virtuele machine om te schakelen van OS-schijfversleuteling op [installatiekopieën ondersteund](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). Zodra het versleutelingsproces voor OS-schijf voltooid is, kan de virtuele machine worden geconfigureerd om te worden uitgevoerd met minder geheugen.
- Voordat u versleuteling inschakelt, moeten de gegevensschijven moeten worden versleuteld goed worden weergegeven in/etc/fstab. Gebruik een permanente blok apparaatnaam op voor deze vermelding als apparaat namen in de indeling '/ dev/sdX' kunnen niet worden gebruikt om te worden gekoppeld aan dezelfde schijf tijdens opnieuw opstarten, met name als versleuteling wordt toegepast. Zie voor meer informatie over dit probleem: [wijzigingen van de apparaatnaam Linux-VM oplossen](../virtual-machines/linux/troubleshoot-device-names-problems.md)
- Zorg ervoor dat de/etc/fstab-instellingen juist zijn geconfigureerd voor koppelen. Deze instellingen configureren, uitvoeren van de opdracht mount- of opnieuw opstarten van de virtuele machine en activeren van de beschadigingsgebeurtenissen op die manier. Zodra dat is voltooid, controleert u de uitvoer van de opdracht lsblk om te controleren dat de gewenste schijf nog steeds is gekoppeld. 
    - Als het bestand/etc/fstab niet op het station goed voordat versleuteling werd ingeschakeld koppelen, kunnen Azure Disk Encryption correct koppelen niet mogelijk.
    - De Azure Disk Encryption-proces worden de mount-gegevens uit/etc/fstab en in een eigen configuratiebestand als onderdeel van het versleutelingsproces verplaatst. Geen worden zorgen om te zien van de vermelding ontbreekt in/etc/fstab nadat gegevens stationsversleuteling is voltooid.
    -  Het duurt na opnieuw opstarten, tijd voor de Azure Disk Encryption-proces om de zojuist versleutelde schijven te koppelen. Ze zijn onmiddellijk niet beschikbaar na het opnieuw opstarten. Het proces tijd om te beginnen, ontgrendelen en koppel vervolgens de versleutelde schijven voorafgaand aan hun deze beschikbaar is voor andere processen voor toegang tot nodig heeft. Dit proces duurt langer dan een minuut na opnieuw opstarten, afhankelijk van de kenmerken van het systeem.

Een voorbeeld van de opdrachten die kan worden gebruikt om de gegevensschijven koppelen en de benodigde/etc/fstab-vermeldingen te maken kunt u vinden [regels 197-205 van dit scriptbestand](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L197-L205). 


## <a name="bkmk_GPO"></a> Netwerk- en Groepsbeleid

**Syntaxis van de parameter, de IaaS-VM's moet voldoen aan de volgende configuratievereisten voor netwerk-eindpunt voor de Azure Disk Encryption inschakelen met behulp van de oudere AAD:** 
  - Als u een token voor het verbinding maken met uw key vault, moet de IaaS-VM geen verbinding maken met een Azure Active Directory-eindpunt, zijn \[login.microsoftonline.com\].
  - Als de versleutelingssleutels wilt opslaan op uw key vault, moet de IaaS-VM geen verbinding maken met het eindpunt van de sleutelkluis.
  - De IaaS-VM moet geen verbinding maken met een Azure storage-eindpunt dat als host fungeert voor de Azure-extensie-opslagplaats en een Azure storage-account dat als host fungeert voor de VHD-bestanden.
  -  Als uw beveiligingsbeleid beperkt de toegang van Azure VM's tot het Internet, kunt u deze kunt oplossen door de voorgaande URI en configureren van een specifieke regel voor het toestaan van uitgaande verbinding met de IP-adressen. Zie voor meer informatie, [Azure Key Vault achter een firewall](../key-vault/key-vault-access-behind-firewall.md).
  - Op Windows, als TLS 1.0 expliciet is uitgeschakeld en de .NET-versie niet naar 4.6 of hoger bijgewerkt is, kunnen de volgende wijziging in het register ADE de recentere TLS-versie selecteren:
    
        [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001

        [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001` 
     

 


**Groepsbeleid:**
 - De Azure Disk Encryption-oplossing maakt gebruik van de externe BitLocker-sleutelbeveiliging voor Windows IaaS-VM's. Voor virtuele machines van een domein, niet een Groepsbeleid TPM beveiligingstoepassingen afdwingen pushen. Zie voor meer informatie over het groepsbeleid voor "Toestaan BitLocker zonder een compatibele TPM" [BitLocker Group Policy Reference](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#a-href-idbkmk-unlockpol1arequire-additional-authentication-at-startup).

-  BitLocker-Groepsbeleid op virtuele machines van een domein voor de aangepaste omvat de volgende instelling: [configureren gebruiker opslag van bitlocker-herstelgegevens -> toestaan 256-bits herstelsleutel](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Azure Disk Encryption mislukt wanneer u aangepaste instellingen voor Groepsbeleid voor Bitlocker niet compatibel zijn. Het nieuwe beleid toepassen op virtuele machines die niet de juiste instelling hebben, en vervolgens opnieuw te starten is mogelijk vereist afdwingen dat het nieuwe beleid om bij te werken (gpupdate.exe/Force).  


## <a name="bkmk_PSH"></a> Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) biedt een set cmdlets die gebruikmaakt van de [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) model voor het beheren van uw Azure-resources. U kunt deze gebruiken in uw browser met [Azure Cloud Shell](../cloud-shell/overview.md), of kunt u deze installeren op uw lokale computer met behulp van de onderstaande instructies om te worden gebruikt in een PowerShell-sessie. Als u al deze lokaal geïnstalleerd hebt, zorg er dan voor dat u de nieuwste versie van Azure PowerShell SDK-versie om te configureren van Azure Disk Encryption. Download de nieuwste versie van [Azure PowerShell versie](https://github.com/Azure/azure-powershell/releases).

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Azure PowerShell installeren voor gebruik op uw lokale computer (optioneel):  
1. Volg de instructies in de koppelingen voor uw besturingssysteem wordt voortgezet maar de rest van de onderstaande stappen.      
    - [Installeren en configureren van Azure PowerShell voor Windows](/powershell/azure/install-azurerm-ps). 
        - PowerShellGet, Azure PowerShell installeren en laden van de AzureRM-module. 
    - [Installeren en configureren van Azure Powershell in macOS en Linux](/powershell/azure/install-azurermps-maclinux).
        -  PowerShell Core, Azure PowerShell voor .NET Core, installeren en laden van de Az-module.
2. Installeer de [Azure Active Directory PowerShell-module](/powershell/azure/active-directory/install-adv2#installing-the-azure-ad-module). 

     ```powershell
     Install-Module AzureAD
     ```

3. Controleer of de geïnstalleerde versies van de modules.
      ```powershell
      Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
      Get-Module AzureAD -ListAvailable | Select-Object -Property Name,Version,Path
      ```
4. Aanmelden bij Azure met de [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet.
     
     ```powershell
     Connect-AzureRmAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzureRmSubscription and 
     specify it with Set-AzureRmContext.  #>
     Get-AzureRmSubscription
     Set-AzureRmContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

5.  Verbinding maken met Azure AD [Connect-AzureAD](/powershell/module/azuread/connect-azuread).
     
     ```powershell
     Connect-AzureAD
     ```

6. Beoordeling [aan de slag met Azure PowerShell](/powershell/azure/get-started-azureps) en [AzureAD](/powershell/module/azuread), indien nodig.

## <a name="bkmk_CLI"></a> Azure CLI

De [Azure CLI 2.0](/cli/azure) is een opdrachtregelprogramma voor het beheren van Azure-resources. De CLI is ontworpen voor flexibel gegevens op te vragen, langdurige bewerkingen als niet-blokkerende processen ondersteunen en scripts eenvoudig uitvoeren. U kunt PowerShell in uw browser gebruiken met [Azure Cloud Shell](/cloud-shell/overview.md), of installeren op uw lokale computer en dan gebruiken in een PowerShell-sessie.

1. [Azure CLI installeren](/cli/azure/install-azure-cli) voor gebruik op uw lokale computer (optioneel):

2. Controleer of de geïnstalleerde versie.
     
     ```azurecli-interactive
     az --version
     ``` 

3. Aanmelden bij Azure met [az login](/cli/azure/authenticate-azure-cli).
     
     ```azurecli
     az login
     
     # If you would like to select a tenant, use: 
     az login --tenant "<tenant>"

     # If you have multiple subscriptions, get your subscription list with az account list and specify with az account set.
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Beoordeling [aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) indien nodig. 


## <a name="prerequisite-workflow-for-key-vault-and-the-azure-ad-app"></a>Vereiste werkstroom voor Key Vault en de Azure AD-app

Als u al bekend met de Key Vault en Azure AD-vereisten voor Azure Disk Encryption bent, kunt u de [PowerShell script met vereisten voor Azure Disk Encryption](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Zie voor meer informatie over het gebruik van het script met vereisten voor de [een snelstartgids van Virtual machines versleutelen](quick-encrypt-vm-powershell.md) en de [Azure Disk Encryption bijlage](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. Een sleutelkluis maken. 
2. Instellen van een Azure AD-toepassing en service-principal.
3. Het toegangsbeleid voor key vault voor de Azure AD-app ingesteld.
4. Set-sleutelkluis geavanceerde toegangsbeleid.
 
## <a name="bkmk_KeyVault"></a> Een sleutelkluis maken 
Azure Disk Encryption is geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te controleren en beheren van de schijf-versleutelingssleutels en geheimen in uw key vault-abonnement. U kunt een key vault maken of gebruik een bestaande resourcegroep voor Azure Disk Encryption. Zie voor meer informatie over sleutelkluizen [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md) en [uw key vault beveiligen](../key-vault/key-vault-secure-your-key-vault.md). U kunt een Resource Manager-sjabloon, Azure PowerShell of Azure CLI gebruiken om een sleutelkluis te maken. 


>[!WARNING]
>Om te verzekeren dat de geheimen van de versleuteling niet overschreden door de regionale grenzen, moet Azure Disk Encryption de Key Vault en de virtuele machines in dezelfde regio worden geplaatst. Maak en gebruik van een Key Vault die zich in dezelfde regio als de virtuele machine moeten worden versleuteld. 


### <a name="bkmk_KVPSH"></a> Een sleutelkluis maken met PowerShell

U kunt een sleutelkluis maken met het gebruik van Azure PowerShell de [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/New-AzureRmKeyVault) cmdlet. Zie voor aanvullende cmdlets voor Key Vault [AzureRM.KeyVault](/powershell/module/azurerm.keyvault/). 

1. Indien nodig, [verbinding maken met uw Azure-abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH). 
2. Maak een nieuwe resourcegroep, indien nodig, met [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup).  Lijst met locaties voor data center, gebruikt [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocationn). 
     
     ```azurepowershell-interactive
     # Get-AzureRmLocation 
     New-AzureRmResourceGroup –Name 'MySecureRG' –Location 'East US'
     ```

3. Maak een nieuwe sleutelkluis met [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/New-AzureRmKeyVault)
    
      ```azurepowershell-interactive
     New-AzureRmKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -Location 'East US'
     ```

4. Houd er rekening mee de **Kluisnaam**, **Resourcegroepnaam**, **Resource-ID**, **Vault URI**, en de **Object-ID** die worden geretourneerd voor later gebruik wanneer u de schijven versleutelen. 


### <a name="bkmk_KVCLI"></a> Een sleutelkluis maken met Azure CLI
U kunt uw key vault met behulp van Azure CLI beheren de [az keyvault](/cli/azure/keyvault#commands) opdrachten. Gebruik voor het maken van een key vault [az keyvault maken](/cli/azure/keyvault#az-keyvault-create).

1. Indien nodig, [verbinding maken met uw Azure-abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Maak een nieuwe resourcegroep, indien nodig, met [az-groep maken](/cli/azure/groupt#az-group-create). Aan de lijst met locaties, gebruikt u [az account list-locations](/cli/azure/account#az-account-list) 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MySecureRG" -l "East US"
     ```

3. Maak een nieuwe sleutelkluis met [az keyvault maken](/cli/azure/keyvault#az-keyvault-create).
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MySecureRG" --location "East US"
     ```

4. Houd er rekening mee de **Kluisnaam** (naam), **groepsnaam voor Accountresources**, **Resource-ID** (ID), **Vault URI**, en de **Object-ID** die later worden geretourneerd voor gebruik. 

### <a name="bkmk_KVRM"></a> Een sleutelkluis maken met Resource Manager-sjabloon

U kunt een sleutelkluis maken met behulp van de [Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create).

1. Klik op de Azure-quickstart-sjabloon, **implementeren in Azure**.
2. Selecteer het abonnement, resourcegroeplocatie voor resourcegroep, Key Vault-naam, Object-ID, juridische voorwaarden en overeenkomst en klik vervolgens op **aankoop**. 


## <a name="bkmk_ADapp"></a> Instellen van een Azure AD-app en service principal 
Wanneer u versleuteling wordt ingeschakeld op een actieve virtuele machine in Azure nodig hebt, wordt Azure Disk Encryption genereert en schrijft de versleutelingssleutels in uw key vault. Beheer van versleutelingssleutels in uw key vault, vereist Azure AD-verificatie. Maak een Azure AD-toepassing voor dit doel. Voor verificatiedoeleinden wordt gebruikt, kunt u een van beide verificaties client op basis van een geheim of [Azure AD op basis van certificaten clientverificatie](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md).


### <a name="bkmk_ADappPSH"></a> Instellen van een Azure AD-app en service principal met de Azure PowerShell 
Voor het uitvoeren van de volgende opdrachten, halen en gebruik de [Azure AD PowerShell-module](/powershell/azure/active-directory/install-adv2). 

1. Indien nodig, [verbinding maken met uw Azure-abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH).
2. Gebruik de [New-AzureRmADApplication](/powershell/module/azurerm.resources/new-azurermadapplication) PowerShell-cmdlet voor het maken van een Azure AD-toepassing. MyApplicationHomePage en de MyApplicationUri mag geen waarden die u wilt.

     ```azurepowershell-interactive
     $aadClientSecret = "My AAD client secret"
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force
     $azureAdApplication = New-AzureRmADApplication -DisplayName "My Application Display Name" -HomePage "https://MyApplicationHomePage" -IdentifierUris "https://MyApplicationUri" -Password $aadClientSecretSec
     $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId
     ```

3. De $azureAdApplication.ApplicationId is de ClientID van Azure AD en de $aadClientSecret is het clientgeheim die u later gebruiken wilt voor Azure Disk Encryption inschakelen. Het clientgeheim van de Azure AD op de juiste manier beveiligen. Met `$azureAdApplication.ApplicationId` ApplicationID wordt weergegeven.


### <a name="bkmk_ADappCLI"></a> Instellen van een Azure AD-app en service principal met de Azure CLI

U kunt uw service-principals met Azure CLI met beheren de [az ad sp](/cli/azure/ad/sp) opdrachten. Zie voor meer informatie, [een Azure-service-principal maken ](/cli/azure/create-an-azure-service-principal-azure-cli).

1. Indien nodig, [verbinding maken met uw Azure-abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Maak een nieuwe serviceprincipal.
     
     ```azurecli-interactive
     az ad sp create-for-rbac --name "ServicePrincipalName" --password "My-AAD-client-secret" --skip-assignment 
     ```
3.  De toepassings-id geretourneerd aan de ClientID van Azure AD wordt gebruikt in andere opdrachten. Het is ook de SPN die u voor az keyvault-beleid instellen gebruikt. Het wachtwoord is het clientgeheim die u later gebruiken moet om Azure Disk Encryption inschakelen. Het clientgeheim van de Azure AD op de juiste manier beveiligen.
 
### <a name="bkmk_ADappRM"></a> Instellen van een Azure AD-app en service principal via de Azure-portal
Gebruik de stappen bij het [portal gebruiken voor het maken van een Azure Active Directory-toepassing en service-principal die toegang hebben tot resources](../azure-resource-manager/resource-group-create-service-principal-portal.md) artikel te maken van een Azure AD-toepassing. Elke stap hieronder gaat u rechtstreeks naar de sectie om te voltooien. 

1. [Controleer of de vereiste machtigingen](../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions)
2. [Een Azure Active Directory-toepassing maken](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application) 
     - U kunt een willekeurige naam gebruiken en aanmeldings-URL die u dat wilt bij het maken van de toepassing.
3. [De toepassings-ID en de verificatiesleutel](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key). 
     - De verificatiesleutel is het clientgeheim en wordt gebruikt als de AadClientSecret voor Set-AzureRmVMDiskEncryptionExtension. 
        - De verificatiesleutel wordt gebruikt door de toepassing als een referentie zich aanmeldt bij Azure AD. Dit geheim wordt aangeroepen sleutels in de Azure-portal, maar heeft geen relatie tot sleutelkluizen. Dit geheim op de juiste manier beveiligen. 
     - De toepassings-ID wordt later gebruikt als de AadClientId voor Set-AzureRmVMDiskEncryptionExtension en als de ServicePrincipalName voor Set-AzureRmKeyVaultAccessPolicy. 

## <a name="bkmk_KVAP"></a> Het toegangsbeleid voor key vault voor de Azure AD-app instellen
Azure Disk Encryption moet encryption geheimen worden geschreven naar een opgegeven Key Vault, de Client-ID en het Clientgeheim van de Azure Active Directory-toepassing met machtigingen voor geheimen schrijven naar de Key Vault. 

> [!NOTE]
> Azure Disk Encryption moet u het volgende toegangsbeleid naar uw Azure AD-clienttoepassing configureren: _WrapKey_ en _ingesteld_ machtigingen.

### <a name="bkmk_KVAPPSH"></a> Instellen van het toegangsbeleid voor key vault voor de Azure AD-app met Azure PowerShell
Uw Azure AD-toepassing moet de rechten voor toegang tot de sleutels of geheimen in de kluis. Gebruik de [Set AzureKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet voor het verlenen van machtigingen voor de toepassing, met behulp van de client-ID (die is gegenereerd toen de toepassing is geregistreerd) als de _– ServicePrincipalName_ parameterwaarde. Zie voor meer informatie het blogbericht [Azure Key Vault - stap voor stap](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). 

1. Indien nodig, [verbinding maken met uw Azure-abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH).
2. Het toegangsbeleid voor key vault voor de AD-toepassing met PowerShell instellen.

     ```azurepowershell-interactive
     $keyVaultName = 'MySecureVault'
     $aadClientID = 'MyAadAppClientID'
     $rgname = 'MySecureRG'
     Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname
     ```

### <a name="bkmk_KVAPCLI"></a> Het toegangsbeleid voor key vault voor de Azure AD-app met Azure CLI instellen
Gebruik [az keyvault-beleid instellen](https://docs.microsoft.com/cli/azure/keyvault.md#az-keyvault-set-policy) om in te stellen van het toegangsbeleid. Zie voor meer informatie, [Key Vault beheren met behulp van CLI 2.0](../key-vault/key-vault-manage-with-cli2.md#authorizing-an-application-to-use-a-key-or-secret).

1. Indien nodig, [verbinding maken met uw Azure-abonnement](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Geef de service-principal die u hebt gemaakt via de Azure CLI-toegang tot het ophalen van geheimen en tekstterugloop sleutels met de volgende opdracht:
 
     ```azurecli-interactive
     az keyvault set-policy --name "MySecureVault" --spn "<spn created with CLI/the Azure AD ClientID>" --key-permissions wrapKey --secret-permissions set
     ```

### <a name="bkmk_KVAPRM"></a> Het toegangsbeleid voor key vault voor de Azure AD-app met de portal instellen

1. Open de resourcegroep met uw key vault.
2. Selecteer uw key vault, gaat u naar **toegangsbeleid**, klikt u vervolgens op **nieuwe toevoegen**.
3. Onder **Selecteer principal**, zoeken naar de Azure AD-toepassing die u hebt gemaakt en selecteer deze. 
4. Voor **sleutelmachtigingen**, Controleer **sleutel verpakken en sleutel** onder **cryptografische bewerkingen**.
5. Voor **geheime machtigingen**, Controleer **ingesteld** onder **geheim beheerbewerkingen**.
6. Klik op **OK** om op te slaan van het toegangsbeleid. 

![Azure Key Vault cryptografische bewerkingen - sleutel Inpakken](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault-geheim machtigingen - instellen ](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

## <a name="bkmk_KVper"></a> Geavanceerde toegangsbeleid voor sleutelkluis instellen
Het Azure-platform moet toegang hebben tot de sleutels of geheimen in uw key vault zodat ze beschikbaar voor de virtuele machine voor opstarten en ontsleutelen van de volumes. Schijf-versleuteling inschakelen voor de sleutelkluis of implementaties mislukken.  

### <a name="bkmk_KVperPSH"></a> Sleutelkluis set geavanceerde toegangsbeleid met Azure PowerShell
 Gebruik de key vault PowerShell-cmdlet [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) om in te schakelen schijfversleuteling voor de key vault.

  - **Key Vault inschakelen voor schijfversleuteling:** EnabledForDiskEncryption is vereist voor Azure Disk encryption.
      
     ```azurepowershell-interactive 
     Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForDiskEncryption
     ```

  - **Key Vault inschakelen voor implementatie, indien nodig:** kunnen de Microsoft.Compute-resourceprovider geheimen ophalen uit deze key vault wanneer deze sleutelkluis wordt verwezen in de resources worden gemaakt, bijvoorbeeld bij het maken van een virtuele machine.

     ```azurepowershell-interactive
      Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForDeployment
     ```

  - **Key Vault inschakelen voor sjabloonimplementatie, indien nodig:** kunnen Azure Resource Manager geheimen ophalen uit deze key vault wanneer deze sleutelkluis wordt verwezen in de sjabloonimplementatie van een.

     ```azurepowershell-interactive             
     Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForTemplateDeployment`
     ```

### <a name="bkmk_KVperCLI"></a> Sleutelkluis set geavanceerde toegangsbeleid met de Azure CLI
Gebruik [az keyvault update](/cli/azure/keyvault#az-keyvault-update) om in te schakelen schijfversleuteling voor de key vault. 

 - **Key Vault inschakelen voor schijfversleuteling:** ingeschakeld voor disk encryption is vereist. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-disk-encryption "true"
     ```  

 - **Key Vault inschakelen voor implementatie, indien nodig:** virtuele Machines toestaan om op te halen van certificaten die zijn opgeslagen als geheimen van de kluis.
     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-deployment "true"
     ``` 

 - **Key Vault inschakelen voor sjabloonimplementatie, indien nodig:** toestaan Resource Manager geheimen ophalen uit de kluis.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a> Sleutelkluis set geavanceerde toegangsbeleid via Azure portal

1. Selecteer uw Key Vault, gaat u naar **toegangsbeleid**, en **Klik hierop om geavanceerde Toegangsbeleidsregels**.
2. Selecteer het selectievakje **toegang tot Azure Disk Encryption voor volumeversleuteling**.
3. Selecteer **toegang tot Azure Virtual Machines voor implementatie inschakelen** en/of **inschakelen toegang tot Azure Resource Manager voor sjabloonimplementatie**, indien nodig. 
4. Klik op **Opslaan**.

![Azure-sleutelkluis geavanceerde toegangsbeleid](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a> Instellen van een sleutel van versleutelingssleutel (optioneel)
Als u wilt een sleutel key-versleuteling (KEK) te gebruiken voor een extra beveiligingslaag voor versleutelingssleutels, moet u een KEK-sleutel toevoegen aan uw key vault. Gebruik de [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) cmdlet voor het maken van een sleutel van versleutelingssleutel in de key vault. U kunt ook een KEK-sleutel van uw on-premises Sleutelbeheer HSM importeren. Zie voor meer informatie, [Key Vault-documentatie](../key-vault/key-vault-hsm-protected-keys.md). Wanneer een sleutel van versleutelingssleutel is opgegeven, gebruikt Azure Disk Encryption die sleutel het verpakken van de geheimen van de versleuteling voor het schrijven naar de Key Vault. 

* Uw key vault-geheim en de KEK-sleutel-URL's moeten worden bijgehouden. Azure wordt deze beperking van versiebeheer afgedwongen. Zie de volgende voorbeelden voor geldige geheim en KEK-sleutel-URL's:

  * Voorbeeld van een geldige geheime URL:   *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Voorbeeld van een geldige KEK-sleutel-URL:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk Encryption biedt geen ondersteuning voor het opgeven van de poortnummers als onderdeel van de key vault-geheimen en KEK-sleutel-URL's. Zie voor voorbeelden van niet-ondersteunde en ondersteunde key vault-URL's in de volgende voorbeelden:

  * URL voor key vault niet toegestaan.  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * URL voor acceptabel key vault:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

### <a name="bkmk_KEKPSH"></a> Instellen van een sleutel van versleutelingssleutel met Azure PowerShell 
Voordat u het PowerShell-script gebruikt, moet u bekend bent met de Azure Disk Encryption-vereisten voor het begrijpen van de stappen in het script. Het voorbeeldscript mogelijk wijzigingen voor uw omgeving. Dit script maakt alle vereisten voor Azure Disk Encryption en een bestaande IaaS-VM, de versleutelingssleutel van de schijf met behulp van een sleutel van versleutelingssleutel wrapping versleutelt. 

 ```powershell
 # Step 1: Create a new resource group and key vault in the same location.
     # Fill in 'MyLocation', 'MySecureRG', and 'MySecureVault' with your values.
     # Use Get-AzureRmLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzureRmResourceGroup
     
     $Loc = 'MyLocation';
     $rgname = 'MySecureRG';
     $KeyVaultName = 'MySecureVault'; 
     New-AzureRmResourceGroup –Name $rgname –Location $Loc;
     New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc;
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $KeyVaultResourceId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).VaultUri;
     
 # Step 2: Create the AD application and service principal.
     # Fill in 'MyAADClientSecret', "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
     # MyApplicationHomePage and the MyApplicationUri can be any values you wish.
     
     $aadClientSecret =  'MyAADClientSecret';
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force;
     $azureAdApplication = New-AzureRmADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -Password $aadClientSecretSec
     $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId;
     $aadClientID = $azureAdApplication.ApplicationId;
     
 #Step 3: Enable the vault for disk encryption and set the access policy for the Azure AD application.
     
     Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption;
     Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname;
     
 #Step 4: Create a new key in the key vault with the Add-AzureKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'Software';
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 5: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' with your value. 
     
     $VMName = 'MySecureVM';
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```

## <a name="bkmk_Cert"></a> Verificatie op basis van certificaten (optioneel)
Als u wilt om certificaatverificatie te gebruiken, kunt u een voor uw key vault uploaden en deze implementeren naar de client. Voordat u het PowerShell-script gebruikt, moet u bekend bent met de Azure Disk Encryption-vereisten voor het begrijpen van de stappen in het script. Het voorbeeldscript mogelijk wijzigingen voor uw omgeving.

     
 ```powershell

 # Fill in "MySecureRG", "MySecureVault", and 'MyLocation' ('My location' only if needed)

   $rgname = 'MySecureRG'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption. No need to set 'My location' in this case.

   $Loc = 'MyLocation'
   New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption

   #Setting some variables with the key vault information 
   $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId

   # Create the Azure AD application and associate the certificate with it. 
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzureRmADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname
   
   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert".  

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for -EnabledForDeployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDeployment

   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' with your value.

   $VMName = 'MySecureVM'
   $CertUrl = (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId
   $VM = Get-AzureRmVM -ResourceGroupName $rgname -Name $VMName 
   $VM = Add-AzureRmVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzureRmVM -VM $VM -ResourceGroupName $rgname 

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
 ```

## <a name="bkmk_CertKEK"></a> Verificatie op basis van certificaten en een KEK-sleutel (optioneel)

Als u wilt het gebruik van certificaatverificatie en de versleutelingssleutel met een KEK-sleutel inpakken, kunt u het onderstaande script als voorbeeld. Voordat u het PowerShell-script gebruikt, moet u bekend bent met alle van de vorige vereisten voor Azure Disk Encryption wilt weten welke stappen in het script. Het voorbeeldscript mogelijk wijzigingen voor uw omgeving.

> [!IMPORTANT]
> Azure AD-certificaten gebaseerde verificatie wordt momenteel niet ondersteund op virtuele Linux-machines.



     
 ```powershell
# Fill in 'MySecureRG', 'MySecureVault', and 'MyLocation' (if needed)

   $rgname = 'MySecureRG'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption.

   $Loc = 'MyLocation'
   New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption

   # Create the Azure AD application and associate the certificate with it.  
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzureRmADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   ## Give access for setting secrets and wraping keys
   Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert". 

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for deployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDeployment

   #Setting some variables with the key vault information and generating a KEK 
   # FIll in 'KEKName'
   
   $KEKName ='KEKName'
   $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId
   $KEK = Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name $KEKName -Destination "Software"
   $KeyEncryptionKeyUrl = $KEK.Key.kid



   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' with your value.

   $VMName = 'MySecureVM'
   $CertUrl = (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId
   $VM = Get-AzureRmVM -ResourceGroupName $rgname -Name $VMName 
   $VM = Add-AzureRmVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzureRmVM -VM $VM -ResourceGroupName $rgname 

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

 
## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Schakel Azure Disk Encryption voor Windows](azure-security-disk-encryption-windows-aad.md)

> [!div class="nextstepaction"]
> [Schakel Azure Disk Encryption voor Linux](azure-security-disk-encryption-linux-aad.md)
