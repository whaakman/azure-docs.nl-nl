---
title: Azure Active Directory-verificatie inschakelen via SMB voor Azure Files (preview) - Azure Storage
description: Informatie over het inschakelen van verificatie op basis van identiteit via SMB (Server Message Block) (preview) voor Azure Files via Azure Active Directory (Azure AD) Domain Services. Uw domein Windows virtuele machines (VM's) hebben vervolgens toegang tot Azure-bestandsshares met behulp van Azure AD-referenties.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 07/05/2019
ms.author: rogarana
ms.openlocfilehash: cd4c952caa336f2602d3c30e0db3e10ebee59cb9
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67696125"
---
# <a name="enable-azure-active-directory-domain-service-authentication-over-smb-for-azure-files-preview"></a>Azure Active Directory Domain Services-verificatie inschakelen via SMB voor Azure Files (Preview)
[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Zie voor een overzicht van Azure AD-verificatie voor Azure Files via SMB, [overzicht van Azure Active Directory-verificatie voor Azure Files (Preview) via SMB](storage-files-active-directory-overview.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview-of-the-workflow"></a>Overzicht van de werkstroom
Voordat u Azure AD DS-verificatie via SMB voor Azure Files inschakelt, Controleer uw Azure AD en Azure Storage-omgevingen correct zijn geconfigureerd. Het wordt aanbevolen dat u stapsgewijs door de [vereisten](#prerequisites) om ervoor te zorgen dat u alle vereiste stappen hebt uitgevoerd. 

Vervolgens toegang verlenen tot Azure Files-resources met Azure AD-referenties door de volgende stappen: 

1. Azure AD DS-verificatie inschakelen via SMB voor uw storage-account voor het registreren van het storage-account met de gekoppelde Azure AD Domain Services-implementatie.
2. Toegangsmachtigingen voor het delen van toewijzen aan een Azure AD-identiteit (een gebruiker, groep of service-principal).
3. Configureer NTFS-machtigingen via SMB voor mappen en bestanden.
4. Een Azure-bestandsshare koppelen van een domein-virtuele machine.

Het volgende diagram ziet u de end-to-end-werkstroom voor het inschakelen van Azure AD DS-verificatie via SMB voor Azure Files. 

![Diagram van Azure AD via SMB voor Azure Files-werkstroom](media/storage-files-active-directory-enable/azure-active-directory-over-smb-workflow.png)

## <a name="prerequisites"></a>Vereisten 

Voordat u Azure AD via SMB voor Azure Files inschakelt, zorg er dan voor dat u hebt de volgende vereisten:

1.  **Selecteer of maak een Azure AD-tenant.**

    U kunt een nieuwe of bestaande tenant gebruiken voor Azure AD-verificatie via SMB. De tenant en de bestandsshare die u wilt openen moeten worden gekoppeld aan hetzelfde abonnement zijn.

    Maak een nieuwe Azure AD-tenant, kunt u [een Azure AD-tenant en een Azure AD-abonnement toevoegen](https://docs.microsoft.com/windows/client-management/mdm/add-an-azure-ad-tenant-and-azure-ad-subscription). Als u een bestaande Azure AD-tenant hebt, maar u wilt maken van een nieuwe tenant voor gebruik met Azure Files, Zie [maken van een Azure Active Directory-tenant](https://docs.microsoft.com/rest/api/datacatalog/create-an-azure-active-directory-tenant).

2.  **Azure AD Domain Services op de Azure AD-tenant inschakelen.**

    Ter ondersteuning van verificatie met Azure AD-referenties, moet u Azure AD Domain Services voor uw Azure AD-tenant inschakelen. Als u niet de beheerder van de Azure AD-tenant, neem contact op met de beheerder en volg de stapsgewijze instructies voor het [inschakelen Azure Active Directory Domain Services met behulp van de Azure-portal](../../active-directory-domain-services/create-instance.md).

    Het duurt meestal ongeveer 15 minuten voor de implementatie van een Azure AD Domain Services om te voltooien. Controleer of de status van uw Azure AD Domain Services wordt weergegeven dat **met**, met wachtwoord-hashsynchronisatie is ingeschakeld, voordat u doorgaat met de volgende stap.

3.  **Domain-join-een Azure-VM met Azure AD Domain Services.**

    Voor toegang tot een bestandsshare met behulp van Azure AD-referenties van een virtuele machine, moet uw virtuele machine worden toegevoegd aan Azure AD Domain Services. Zie voor meer informatie over hoe u een virtuele machine domain-join, [een Windows Server-machine toevoegen aan een beheerd domein](../../active-directory-domain-services/join-windows-vm.md).

    > [!NOTE]
    > Azure AD DS-verificatie via SMB met Azure-bestanden wordt alleen ondersteund op virtuele Azure-machines die worden uitgevoerd op versies van het besturingssysteem dan Windows 7 of Windows Server 2008 R2.

4.  **Selecteer of maak een Azure-bestandsshare.**

    Selecteer een nieuwe of bestaande bestandsshare die is gekoppeld aan hetzelfde abonnement als uw Azure AD-tenant. Zie voor meer informatie over het maken van een nieuwe bestandsshare [een bestandsshare maken in Azure Files](storage-how-to-create-file-share.md). 
    Voor optimale prestaties wordt aangeraden dat de bestandsshare zich in dezelfde regio als de virtuele machine van waaruit u plant voor toegang tot de share.

5.  **Azure Files-connectiviteit controleren door het koppelen van Azure-bestandsshares met behulp van de sleutel van uw opslagaccount.**

    Probeer om te controleren of uw virtuele machine en de bestandsshare correct zijn geconfigureerd, het koppelen van de bestandsshare met behulp van de sleutel van uw opslagaccount. Zie voor meer informatie, [een Azure-bestandsshare koppelen en de share openen in Windows](storage-how-to-use-files-windows.md).

## <a name="enable-azure-ad-ds-authentication-for-your-account"></a>Azure AD DS-verificatie voor uw account inschakelen

Azure AD DS als verificatie wilt inschakelen via SMB voor Azure Files, kunt u een eigenschap instellen op opslagaccounts die zijn gemaakt na 24 September 2018, met behulp van de Azure portal, Azure PowerShell of Azure CLI. Als u deze eigenschap instelt, wordt het storage-account met de gekoppelde implementatie van Azure AD Domain Services geregistreerd. Azure AD DS-verificatie via SMB wordt ingeschakeld voor alle nieuwe en bestaande bestandsshares in de storage-account. 

Houd er rekening mee dat u Azure AD DS-verificatie via SMB inschakelen kunt pas nadat u Azure AD Domain Services hebt geïmplementeerd met uw Azure AD-tenant. Raadpleeg voor meer informatie de [vereisten](#prerequisites).

### <a name="azure-portal"></a>Azure Portal

Azure AD DS-verificatie inschakelen via SMB met behulp van de [Azure-portal](https://portal.azure.com), als volgt te werk:

1. In de Azure-portal, gaat u naar een bestaand opslagaccount of [een opslagaccount maken](../common/storage-quickstart-create-account.md).
2. In de **instellingen** sectie, selecteer **configuratie**.
3. Schakel **Azure Active Directory-verificatie voor Azure Files (preview)** .

De volgende afbeelding ziet u hoe u Azure AD-verificatie via SMB inschakelt voor uw storage-account.

![Azure AD-verificatie inschakelen via SMB in Azure portal](media/storage-files-active-directory-enable/portal-enable-active-directory-over-smb.png)
  
### <a name="powershell"></a>PowerShell  

Azure AD DS-verificatie inschakelen via SMB met behulp van Azure PowerShell: Installeer eerst de meest recente Az-module (2,4 of nieuwer) of de module Az.Storage (versie 1.5 of hoger). Zie voor meer informatie over het installeren van PowerShell [Azure PowerShell installeren op Windows met PowerShellGet](https://docs.microsoft.com/powershell/azure/install-Az-ps):

```powershell
Install-Module -Name Az.Storage -AllowPrerelease -Force -AllowClobber
```

Maak vervolgens een nieuwe opslag Roep vervolgens account [Set AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount) en stel de **EnableAzureActiveDirectoryDomainServicesForFile** parameter **waar**. Houd er rekening mee in het volgende voorbeeld wordt de tijdelijke aanduiding voor waarden vervangen door uw eigen waarden. (Als u de vorige module in de Preview-versie, de parameter voor activering van de functie is **EnableAzureFilesAadIntegrationForSMB**.)

```powershell
# Create a new storage account
New-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -Location "<azure-region>" `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```
U schakelt deze functie op bestaande opslagaccounts, gebruik de volgende opdracht:

```powershell
# Update a storage account
Set-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```


### <a name="azure-cli"></a>Azure-CLI

Als u Azure AD-verificatie via SMB van Azure CLI 2.0, installeert u eerst de `storage-preview` extensie:

```cli-interactive
az extension add --name storage-preview
```
  
Maak vervolgens een nieuwe opslag Roep vervolgens account [az storage account update](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-update) en stel de `--file-aad` eigenschap **waar**. Houd er rekening mee in het volgende voorbeeld wordt de tijdelijke aanduiding voor waarden vervangen door uw eigen waarden.

```azurecli-interactive
# Create a new storage account
az storage account create -n <storage-account-name> -g <resource-group-name> --file-aad true
```

## <a name="assign-access-permissions-to-an-identity"></a>Machtigingen voor toegang toewijzen aan een identiteit 

Voor toegang tot Azure Files-resources met behulp van Azure AD-referenties, moet een identiteit (een gebruiker, groep of service-principal) de benodigde machtigingen hebben op het niveau van de share. Dit proces is vergelijkbaar met het opgeven van machtigingen voor delen voor Windows, waarin u het type toegang dat een bepaalde gebruiker om een bestandsshare heeft opgeven. De instructies in deze sectie ziet u hoe u om toe te wijzen, lezen, schrijven of verwijderen van machtigingen voor een bestandsshare bij een identiteit.

We hebben twee Azure ingebouwde rollen voor het verlenen van machtigingen voor delen op het niveau aan gebruikers geïntroduceerd: Opslag-bestand gegevens SMB-Share Reader en bestand SMB-Share Gegevensbijdrager voor. 

- **Opslag bestand SMB-Share gegevenslezer** geeft leestoegang in Azure Storage-bestandsshares via SMB
- **Bestand SMB-Share Gegevensbijdrager** kunt lezen, schrijven en verwijderen in Azure Storage-bestandsshares via SMB

> [!IMPORTANT]
> Volledig beheer van een bestandsshare, inclusief de mogelijkheid om een rol toewijzen aan een identiteit, moet u de opslagaccountsleutel. Beheer wordt niet ondersteund met Azure AD-referenties. 

U kunt de Azure portal, PowerShell of Azure CLI gebruiken de ingebouwde rollen toewijzen aan de Azure AD-identiteit van een gebruiker voor het verlenen van machtigingen op het niveau van delen. 

#### <a name="azure-portal"></a>Azure Portal
RBAC-rol toewijzen aan een Azure AD-identiteit, met behulp van de [Azure-portal](https://portal.azure.com), als volgt te werk:

1. In de Azure-portal, gaat u naar de bestandsshare of [een bestandsshare maken in Azure Files](storage-how-to-create-file-share.md).
2. Selecteer **toegangsbeheer (IAM)** .
3. Selecteer **een roltoewijzing toevoegen**
4. In de **roltoewijzing toevoegen** blade, selecteer de juiste ingebouwde rol (opslag bestand SMB-Share gegevenslezer, bestand SMB-Share Gegevensbijdrager) van de **rol** vervolgkeuzelijst. Houd de **toegang toewijzen aan** als standaard: 'Azure AD-gebruiker, groep of service-principal', en selecteer het doel-Azure AD-identiteit op naam of e-mailadres. 
5. Selecteer ten slotte **opslaan** om de rol toewijzingsbewerking te voltooien.

#### <a name="powershell"></a>PowerShell

De volgende PowerShell-opdracht laat zien hoe een RBAC-rol toewijzen aan een Azure AD-identiteit, op basis van aanmelding bij naam. Zie voor meer informatie over het toewijzen van RBAC-rollen met PowerShell [toegang met RBAC en Azure PowerShell beheren](../../role-based-access-control/role-assignments-powershell.md).

Wanneer u het volgende voorbeeld van een script uitvoert, moet u tijdelijke aanduiding voor waarden, inclusief punthaken door uw eigen waarden vervangen.

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

#### <a name="cli"></a>CLI
  
De volgende CLI 2.0-opdracht laat zien hoe een RBAC-rol toewijzen aan een Azure AD-identiteit, op basis van aanmelding bij naam. Zie voor meer informatie over het toewijzen van RBAC-rollen met Azure CLI [beheren van toegang via RBAC en Azure CLI](../../role-based-access-control/role-assignments-cli.md). 

Wanneer u het volgende voorbeeld van een script uitvoert, moet u tijdelijke aanduiding voor waarden, inclusief punthaken door uw eigen waarden vervangen.

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
```

## <a name="configure-ntfs-permissions-over-smb"></a>NTFS-machtigingen configureren via SMB 
Nadat u machtigingen voor share-niveau met RBAC toewijst, moet u de juiste NTFS-machtigingen op de basis-, map of bestandsniveau toewijzen. Machtigingen voor share-niveau beschouwen als de op hoog niveau gatekeeper waarmee wordt bepaald of een gebruiker toegang heeft tot de share, terwijl het NTFS-machtigingen reageren op een meer gedetailleerd niveau om te bepalen welke bewerkingen van de gebruiker op het niveau van de map of het bestand kunt uitvoeren. 

Azure Files ondersteunt de volledige reeks basisfuncties en geavanceerde machtigingen voor NTFS. U kunt bekijken en configureren van NTFS-machtigingen voor mappen en bestanden in een Azure-bestandsshare met koppelen van de share en voer de Windows [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) of [Set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl) opdracht. 

> [!NOTE]
> De preview-versie ondersteunt de alleen de weergavemachtigingen met Windows Verkenner. Machtigingen bewerken is nog niet ondersteund.

Voor het NTFS-machtigingen configureren met supergebruikersbevoegdheden, moet u de bestandsshare koppelen met de sleutel van uw storage-account van uw VM domein. Volg de instructies in de volgende sectie voor een Azure-bestandsshare koppelen vanaf de opdrachtprompt en NTFS-machtigingen dienovereenkomstig configureren.

De volgende sets machtigingen worden ondersteund in de hoofdmap van een bestandsshare:

- BUILTIN\Administrators:(OI)(CI)(F)
- NT-AUTHORITY\SYSTEM:(OI)(CI)(F)
- BUILTIN\USERS:(Rx)
- BUILTIN\USERS:(OI)(CI)(IO)(gr,ge)
- NT Authority\Geverifieerde Users:(OI)(CI)(M)
- NT AUTHORITY\SYSTEM:(F)
- MAKER OWNER:(OI)(CI)(IO)(F)

### <a name="mount-a-file-share-from-the-command-prompt"></a>Een bestandsshare koppelen vanaf de opdrachtprompt

Gebruik de Windows **netgebruik** opdracht voor het koppelen van de Azure-bestandsshare. Vergeet niet de waarden van de tijdelijke aanduiding in het voorbeeld vervangen door uw eigen waarden. Zie voor meer informatie over bestandsshares koppelen [een Azure-bestandsshare koppelen en de share openen in Windows](storage-how-to-use-files-windows.md).

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
```

### <a name="configure-ntfs-permissions-with-icacls"></a>NTFS-machtigingen configureren met icacls
Gebruik de volgende Windows-opdracht voor het verlenen van volledige machtigingen voor alle mappen en bestanden in de bestandsshare, met inbegrip van de hoofdmap. Vergeet niet om Vervang de tijdelijke aanduiding voor waarden aangegeven tussen haakjes in het voorbeeld door uw eigen waarden.

```
icacls <mounted-drive-letter>: /grant <user-email>:(f)
```

Voor meer informatie over het gebruik van icacls NTFS-machtigingen instellen en op het andere type van de machtigingen die worden ondersteund, Zie [de opdrachtregelreferentiewebpagina voor icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

## <a name="mount-a-file-share-from-a-domain-joined-vm"></a>Een bestandsshare vanaf een VM voor het domein koppelen 

Nu u klaar om te controleren of bent dat u hebt de bovenstaande stappen is voltooid met behulp van delen uw Azure AD-referenties voor toegang tot een Azure-bestand van een domein-virtuele machine. Zich eerst aanmelden bij de virtuele machine met behulp van de Azure AD-identiteit waarnaar u machtigingen hebt verleend, zoals wordt weergegeven in de volgende afbeelding.

![Schermafbeelding waarin Azure AD-aanmeldingsscherm voor gebruikersverificatie](media/storage-files-active-directory-enable/azure-active-directory-authentication-dialog.png)

Gebruik vervolgens de volgende opdracht uit om te koppelen van de Azure-bestandsshare. Vergeet niet om de waarden van de tijdelijke aanduiding vervangt door uw eigen waarden. Omdat u al zijn geverifieerd, hoeft u niet de opslagaccountsleutel of de Azure AD-gebruikersnaam en wachtwoord opgeven. Azure AD via SMB biedt ondersteuning voor eenmalige aanmelding mogelijk met behulp van Azure AD-referenties.

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```

U hebt nu is Azure AD-verificatie via SMB ingeschakeld en een aangepaste rol die toegang tot een bestandsshare bij een Azure AD-identiteit biedt is toegewezen. Volg de instructies hiervoor vindt u in stap 2 en 3 om toegang tot de bestandsshare aan extra gebruikers.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Azure Files en Azure AD via SMB, Zie de volgende bronnen:

- [Inleiding tot Azure Files](storage-files-introduction.md)
- [Overzicht van Azure Active Directory-verificatie via SMB voor Azure Files (Preview)](storage-files-active-directory-overview.md)
- [Veelgestelde vragen](storage-files-faq.md)
