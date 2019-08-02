---
title: Azure Active Directory authenticatie via SMB voor Azure Files inschakelen (preview)-Azure Storage
description: Meer informatie over het inschakelen van verificatie op basis van identiteit via SMB (Server Message Block) voor Azure Files via Azure Active Directory Domain Services. Uw virtuele Windows-machines (Vm's) die lid zijn van een domein, kunnen vervolgens toegang krijgen tot Azure-bestands shares met behulp van Azure AD-referenties.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: rogarana
ms.openlocfilehash: c0cfb8b7f0d6e3988ccdfa51cae2748b7008308d
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68699770"
---
# <a name="enable-azure-active-directory-domain-services-authentication-over-smb-for-azure-files-preview"></a>Azure Active Directory Domain Services authenticatie via SMB voor Azure Files inschakelen (preview)
[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Voor een overzicht van Azure AD-verificatie via SMB voor Azure Files raadpleegt u [overzicht van Azure Active Directory-verificatie via SMB voor Azure files (preview)](storage-files-active-directory-overview.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview-of-the-workflow"></a>Overzicht van de werk stroom
Voordat u Azure AD DS-verificatie via SMB voor Azure Files inschakelt, moet u controleren of uw Azure AD-en Azure Storage-omgevingen op de juiste wijze zijn geconfigureerd. U kunt het beste de [vereisten](#prerequisites) door lopen om ervoor te zorgen dat u alle vereiste stappen hebt voltooid.

Verleen vervolgens toegang tot Azure Files resources met Azure AD-referenties door de volgende stappen uit te voeren: 

1. Schakel Azure AD DS-verificatie via SMB in voor uw opslag account om het opslag account te registreren bij de bijbehorende Azure AD DS-implementatie.
2. Toegangs machtigingen voor een share toewijzen aan een Azure AD-identiteit (een gebruiker, groep of Service-Principal).
3. Configureer NTFS-machtigingen via SMB voor mappen en bestanden.
4. Een Azure-bestands share koppelen vanaf een virtuele machine die lid is van een domein.

In het volgende diagram ziet u de end-to-end werk stroom voor het inschakelen van Azure AD DS-verificatie via SMB voor Azure Files. 

![Diagram van Azure AD over SMB voor Azure Files werk stroom](media/storage-files-active-directory-enable/azure-active-directory-over-smb-workflow.png)

## <a name="prerequisites"></a>Vereisten

Voordat u Azure AD via SMB voor Azure Files inschakelt, moet u ervoor zorgen dat u de volgende vereisten hebt voltooid:

1.  **Selecteer of maak een Azure AD-Tenant.**

    U kunt een nieuwe of bestaande Tenant voor Azure AD-verificatie via SMB gebruiken. De Tenant en de bestands share die u wilt openen, moeten zijn gekoppeld aan hetzelfde abonnement.

    Als u een nieuwe Azure AD-Tenant wilt maken, kunt u [een Azure AD-Tenant en een Azure AD-abonnement toevoegen](https://docs.microsoft.com/windows/client-management/mdm/add-an-azure-ad-tenant-and-azure-ad-subscription). Als u een bestaande Azure AD-Tenant hebt, maar een nieuwe Tenant wilt maken voor gebruik met Azure Files, raadpleegt u [een Azure Active Directory-Tenant maken](https://docs.microsoft.com/rest/api/datacatalog/create-an-azure-active-directory-tenant).

2.  **Schakel Azure AD Domain Services in voor de Azure AD-Tenant.**

    Als u verificatie met Azure AD-referenties wilt ondersteunen, moet u Azure AD DS inschakelen voor uw Azure AD-Tenant. Als u niet de beheerder van de Azure AD-Tenant bent, neemt u contact op met de beheerder en volgt u de stapsgewijze richt lijnen om Azure Active Directory Domain Services in te [scha kelen met behulp van de Azure Portal](../../active-directory-domain-services/create-instance.md).

    Het duurt doorgaans ongeveer 15 minuten voordat een Azure AD DS-implementatie is voltooid. Controleer of de status van de Azure-AD DS **wordt weer gegeven**, waarbij de wachtwoord hash-synchronisatie is ingeschakeld, voordat u doorgaat met de volgende stap.

3.  **Domein-lid worden van een Azure-VM met Azure AD DS.**

    Als u toegang wilt krijgen tot een bestands share via Azure AD-referenties van een virtuele machine, moet uw virtuele machine lid zijn van een domein met Azure AD DS. Zie [een virtuele machine van Windows Server toevoegen aan een beheerd domein](../../active-directory-domain-services/join-windows-vm.md)voor meer informatie over het toevoegen van een domein aan een VM.

    > [!NOTE]
    > Azure AD DS-verificatie via SMB met Azure Files wordt alleen ondersteund op Azure-Vm's die worden uitgevoerd op versies van het besturings systeem die ouder zijn dan Windows 7 of Windows Server 2008 R2.

4.  **Selecteer of maak een Azure-bestands share.**

    Selecteer een nieuwe of bestaande bestands share die is gekoppeld aan hetzelfde abonnement als uw Azure AD-Tenant. Zie [een bestands share maken in azure files](storage-how-to-create-file-share.md)voor meer informatie over het maken van een nieuwe bestands share. 
    Voor optimale prestaties raden we aan dat uw bestands share zich in dezelfde regio bevinden als de VM waarvan u de share wilt openen.

5.  **Controleer Azure Files connectiviteit door Azure-bestands shares te koppelen met behulp van de sleutel van het opslag account.**

    Als u wilt controleren of de virtuele machine en de bestands share correct zijn geconfigureerd, kunt u de bestands share koppelen met de sleutel van het opslag account. Zie [een Azure-bestands share koppelen en de share openen in Windows](storage-how-to-use-files-windows.md)voor meer informatie.

## <a name="enable-azure-ad-ds-authentication-for-your-account"></a>Azure AD DS-verificatie inschakelen voor uw account

Als u Azure AD DS-verificatie via SMB voor Azure Files wilt inschakelen, kunt u een eigenschap instellen voor opslag accounts die zijn gemaakt na 24 september 2018, met behulp van de Azure Portal, de Azure PowerShell of de Azure CLI. Als u deze eigenschap instelt, wordt het opslag account geregistreerd bij de bijbehorende Azure AD DS-implementatie. Azure AD DS-verificatie via SMB wordt vervolgens ingeschakeld voor alle nieuwe en bestaande bestands shares in het opslag account.

Denk eraan dat u Azure AD DS-verificatie via SMB pas kunt inschakelen nadat u Azure AD DS hebt geïmplementeerd in uw Azure AD-Tenant. Zie de [vereisten](#prerequisites)voor meer informatie.

### <a name="azure-portal"></a>Azure Portal

Voer de volgende stappen uit om Azure AD DS-verificatie via SMB met de [Azure Portal](https://portal.azure.com)in te scha kelen:

1. Ga in het Azure Portal naar uw bestaande opslag account of [Maak een opslag account](../common/storage-quickstart-create-account.md).
2. Selecteer in de sectie **instellingen** de optie **configuratie**.
3. Schakel **Azure Active Directory verificatie in voor Azure files (preview)** .

In de volgende afbeelding ziet u hoe u Azure AD DS-verificatie via SMB inschakelt voor uw opslag account.

![Azure AD-verificatie via SMB inschakelen in de Azure Portal](media/storage-files-active-directory-enable/portal-enable-active-directory-over-smb.png)
  
### <a name="powershell"></a>PowerShell  

Als u Azure AD DS-verificatie via SMB met Azure PowerShell wilt inschakelen, installeert u de nieuwste AZ-module (2,4 of hoger) of de AZ. Storage-module (1,5 of hoger). Zie voor meer informatie over het installeren van Power shell [Azure PowerShell installeren in Windows met PowerShellGet](https://docs.microsoft.com/powershell/azure/install-Az-ps):

```powershell
Install-Module -Name Az.Storage -AllowPrerelease -Force -AllowClobber
```

Maak een nieuw opslag account, roep [set-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount)en stel de para meter **EnableAzureActiveDirectoryDomainServicesForFile** in op **True**. In het volgende voor beeld moet u de waarden voor de tijdelijke aanduiding vervangen door uw eigen waarden. (Als u de vorige preview-module gebruikt, is de para meter voor functie-activering **EnableAzureFilesAadIntegrationForSMB**.)

```powershell
# Create a new storage account
New-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -Location "<azure-region>" `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```
Gebruik de volgende opdracht om deze functie in te scha kelen op bestaande opslag accounts:

```powershell
# Update a storage account
Set-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```


### <a name="azure-cli"></a>Azure-CLI

Als u Azure AD-verificatie via SMB vanuit Azure CLI 2,0 wilt inschakelen, `storage-preview` installeert u eerst de extensie:

```cli-interactive
az extension add --name storage-preview
```
  
Maak vervolgens een nieuw opslag account, roep [AZ Storage account update](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-update)aan en stel de `--file-aad` eigenschap in op **True**. In het volgende voor beeld moet u de waarden voor de tijdelijke aanduiding vervangen door uw eigen waarden.

```azurecli-interactive
# Create a new storage account
az storage account create -n <storage-account-name> -g <resource-group-name> --file-aad true
```

## <a name="assign-access-permissions-to-an-identity"></a>Toegangs machtigingen toewijzen aan een identiteit

Om toegang te krijgen tot Azure Files resources met Azure AD-referenties, moet een identiteit (een gebruiker, groep of Service-Principal) de benodigde machtigingen op het share niveau hebben. Dit proces is vergelijkbaar met het opgeven van machtigingen voor Windows share, waar u het type toegang opgeeft dat een bepaalde gebruiker heeft voor een bestands share. De richt lijnen in deze sectie laten zien hoe u machtigingen voor lezen, schrijven of verwijderen voor een bestands share toewijst aan een identiteit.

We hebben twee ingebouwde rollen voor Azure geïntroduceerd voor het verlenen van machtigingen op share niveau voor gebruikers:

- Met de **SMB-share lezer van het opslag bestand** wordt lees toegang tot Azure Storage bestands shares via SMB toegestaan.
- **Gegevens van het opslag bestand SMB delen Inzender** maakt lees-, schrijf-en verwijder toegang mogelijk in azure Storage bestands shares via SMB.

> [!IMPORTANT]
> Volledige administratieve controle van een bestands share, inclusief de mogelijkheid om een rol toe te wijzen aan een identiteit, vereist het gebruik van de sleutel voor het opslag account. Beheer beheer wordt niet ondersteund met Azure AD-referenties.

U kunt de Azure Portal, Power shell of Azure CLI gebruiken om de ingebouwde rollen toe te wijzen aan de Azure AD-identiteit van een gebruiker voor het verlenen van machtigingen op share niveau.

#### <a name="azure-portal"></a>Azure Portal
Voer de volgende stappen uit om een RBAC-rol toe te wijzen aan een Azure AD-identiteit met behulp van de [Azure Portal](https://portal.azure.com):

1. Ga in het Azure Portal naar uw bestands share of [Maak een bestands share in azure files](storage-how-to-create-file-share.md).
2. Selecteer **toegangsbeheer (IAM)** .
3. Selecteer **een roltoewijzing toevoegen**
4. Selecteer in de Blade **roltoewijzing toevoegen** de geschikte ingebouwde rol (opslag BESTANDS gegevens SMB-share, opslag BESTANDS gegevens SMB delen Inzender) uit de lijst met **functies** . Houd de optie **toegang toewijzen aan** op de standaard instelling: **Gebruikers-, groeps-of Service-Principal van Azure AD**. Selecteer de Azure AD-doel-id op naam of e-mail adres.
5. Selecteer **Opslaan** om de roltoewijzings bewerking te volt ooien.

#### <a name="powershell"></a>PowerShell

De volgende Power shell-opdracht laat zien hoe u een RBAC-rol toewijst aan een Azure AD-identiteit, op basis van aanmeldings naam. Zie [toegang beheren met RBAC en Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)voor meer informatie over het toewijzen van RBAC-rollen met Power shell.

Voordat u het volgende voorbeeld script uitvoert, moet u de waarden van de tijdelijke aanduidingen vervangen door uw eigen waarden.

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

#### <a name="cli"></a>CLI
  
De volgende CLI 2,0-opdracht laat zien hoe u een RBAC-rol toewijst aan een Azure AD-identiteit, op basis van aanmeldings naam. Zie [toegang beheren met RBAC en Azure cli](../../role-based-access-control/role-assignments-cli.md)voor meer informatie over het toewijzen van RBAC-rollen met Azure cli. 

Voordat u het volgende voorbeeld script uitvoert, moet u de waarden van de tijdelijke aanduidingen vervangen door uw eigen waarden.

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
```

## <a name="configure-ntfs-permissions-over-smb"></a>NTFS-machtigingen configureren via SMB 
Nadat u machtigingen op share niveau hebt toegewezen met RBAC, moet u de juiste NTFS-machtigingen toewijzen op het niveau root, Directory of file. U kunt machtigingen op share niveau beschouwen als de gate keeper op hoog niveau die bepaalt of een gebruiker toegang heeft tot de share. Terwijl NTFS-machtigingen op een meer gedetailleerd niveau handelen om te bepalen welke bewerkingen de gebruiker kan uitvoeren op het niveau van de map of het bestand.

Azure Files ondersteunt de volledige set met NTFS Basic-en geavanceerde machtigingen. U kunt NTFS-machtigingen voor mappen en bestanden in een Azure-bestands share weer geven en configureren door de share te koppelen en vervolgens de opdracht Windows [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) of [set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl) uit te voeren. 

> [!NOTE]
> De preview-versie ondersteunt alleen het weer geven van machtigingen met Windows Verkenner. Bewerkings machtigingen worden nog niet ondersteund.

Als u NTFS wilt configureren met Gebruikersaccountbeheer, moet u de share koppelen met behulp van de sleutel van uw opslag account van de virtuele machine die lid is van het domein. Volg de instructies in de volgende sectie om een Azure-bestands share te koppelen vanaf de opdracht prompt en de NTFS-machtigingen dienovereenkomstig te configureren.

De volgende sets machtigingen worden ondersteund in de hoofdmap van een bestands share:

- BUILTIN\Administrators: (OI) (CI) (F)
- NT AUTHORITY\SYSTEM: (OI) (CI) (F)
- BUILTIN\Users: (RX)
- BUILTIN\Users: (OI) (CI) (i/o) (GR, GE)
- NT AUTHORITY\Authenticated-gebruikers: (OI) (CI) (M)
- NT AUTHORITY\SYSTEM: (F)
- MAKER EIGENAAR: (OI) (CI) (I/O) (F)

### <a name="mount-a-file-share-from-the-command-prompt"></a>Een bestands share koppelen vanaf de opdracht prompt

Gebruik de Windows **net use** -opdracht om de Azure-bestands share te koppelen. Vergeet niet om de waarden van de tijdelijke aanduidingen in het volgende voor beeld te vervangen door uw eigen waarden. Zie [een Azure-bestands share koppelen en de share openen in Windows](storage-how-to-use-files-windows.md)voor meer informatie over het koppelen van bestands shares.

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
```

### <a name="configure-ntfs-permissions-with-icacls"></a>NTFS-machtigingen configureren met icacls
Gebruik de volgende Windows-opdracht om volledige machtigingen te verlenen aan alle mappen en bestanden onder de bestands share, met inbegrip van de hoofdmap. Vergeet niet om de waarden van de tijdelijke aanduidingen in het voor beeld te vervangen door uw eigen waarden.

```
icacls <mounted-drive-letter>: /grant <user-email>:(f)
```

Zie voor meer informatie over het gebruik van icacls voor het instellen van NTFS-machtigingen en voor de verschillende typen ondersteunde machtigingen [de opdracht regel verwijzing voor icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

## <a name="mount-a-file-share-from-a-domain-joined-vm"></a>Een bestands share koppelen vanaf een virtuele machine die lid is van een domein

Met het volgende proces wordt gecontroleerd of uw Azure AD-referenties correct zijn ingesteld en of u toegang hebt tot een Azure-bestands share vanaf een VM die is toegevoegd aan een domein: 

Meld u aan bij de virtuele machine met behulp van de Azure AD-identiteit waaraan u machtigingen hebt verleend, zoals wordt weer gegeven in de volgende afbeelding.

![Scherm afbeelding van het aanmeldings scherm van Azure AD voor gebruikers verificatie](media/storage-files-active-directory-enable/azure-active-directory-authentication-dialog.png)

Gebruik de volgende opdracht om de Azure-bestands share te koppelen. Vergeet niet om de waarden van de tijdelijke aanduidingen te vervangen door uw eigen waarden. Omdat u al bent geverifieerd, hoeft u de sleutel voor het opslag account of de gebruikers naam en het wacht woord voor Azure AD niet op te geven. Azure AD via SMB ondersteunt eenmalige aanmelding met Azure AD-referenties.

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```

U hebt nu Azure AD-verificatie via SMB ingeschakeld en er is een aangepaste rol toegewezen waarmee u toegang krijgt tot een Azure-bestands share met een Azure AD-identiteit. Als u extra gebruikers toegang wilt verlenen tot uw bestands share, volgt u de instructies in de [toegangs machtigingen toewijzen aan een identiteit](#assign-access-permissions-to-an-identity) en [NTFS-machtigingen configureren via SMB-](#configure-ntfs-permissions-over-smb) secties.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over Azure Files en het gebruik van Azure AD via SMB:

- [Inleiding tot Azure Files](storage-files-introduction.md)
- [Overzicht van Azure Active Directory-verificatie via SMB voor Azure Files (preview)](storage-files-active-directory-overview.md)
- [Veelgestelde vragen](storage-files-faq.md)
