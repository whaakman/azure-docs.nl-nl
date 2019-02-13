---
title: PowerShell-voorbeelden voor het beheren van groepen - Azure Active Directory | Microsoft Docs
description: Deze pagina vindt u voorbeelden van PowerShell om u te helpen bij het beheren van uw groepen in Azure Active Directory
keywords: Azure AD, Azure Active Directory, PowerShell, beheer van groepen, groepen
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c1f0b99c3a388dcfd0dabaf874e03f276c494553
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56176863"
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Azure Active Directory-cmdlets van versie 2 voor groepsbeheer

> [!div class="op_single_selector"]
> * [Azure Portal](../fundamentals/active-directory-groups-create-azure-portal.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
> * [PowerShell](groups-settings-v2-cmdlets.md)
>
>

In dit artikel bevat voorbeelden van hoe u PowerShell gebruiken voor het beheren van uw groepen in Azure Active Directory (Azure AD).  Deze ook wordt uitgelegd hoe u met de Azure AD PowerShell-module instellen. Eerst moet u [downloaden van de Azure AD PowerShell-module](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="install-the-azure-ad-powershell-module"></a>De Azure AD PowerShell-module installeren
Gebruik de volgende opdrachten voor het installeren van de Azure AD PowerShell-module:

    PS C:\Windows\system32> install-module azuread
    PS C:\Windows\system32> import-module azuread

Om te controleren dat de module kan worden gebruikt, gebruik de volgende opdracht:

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

U kunt nu beginnen met de cmdlets in de module. Raadpleeg voor een volledige beschrijving van de cmdlets in de Azure AD-module, de online documentatie voor [Azure Active Directory PowerShell versie 2](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="connect-to-the-directory"></a>Verbinding maken met de directory
Voordat u met het beheren van groepen met behulp van Azure AD PowerShell-cmdlets beginnen kunt, moet u de PowerShell-sessie verbinding maken met de map die u wilt beheren. Gebruik de volgende opdracht:

    PS C:\Windows\system32> Connect-AzureAD

De cmdlet vraagt u om de referenties die u gebruiken wilt voor toegang tot uw directory. In dit voorbeeld gebruiken we karen@drumkit.onmicrosoft.com voor toegang tot de demonstratie-map. De cmdlet retourneert een bevestiging voor het weergeven van dat de sessie is verbonden met uw adreslijst:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

U kunt nu beginnen met de AzureAD-cmdlets voor het beheren van groepen in uw directory.

## <a name="retrieve-groups"></a>Groepen ophalen
Gebruik de cmdlet Get-AzureADGroups om op te halen bestaande groepen uit uw directory. 

Gebruik de cmdlet zonder parameters voor het ophalen van alle groepen in de map:

    PS C:\Windows\system32> get-azureadgroup

De cmdlet retourneert alle groepen in de verbonden adreslijst.

U kunt de parameter - object-id gebruiken om op te halen van een specifieke groep waarvoor u de objectID van de groep opgeven:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

De cmdlet retourneert nu de groep waarvan object-id overeenkomt met de waarde van de parameter die u hebt ingevoerd:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

U kunt zoeken naar een specifieke groep met de parameter - filter. Deze parameter heeft een ODATA-filtercomponent en resulteert in alle groepen die overeenkomen met het filter, zoals in het volgende voorbeeld:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

> [!NOTE] 
> De Azure AD PowerShell-cmdlets implementeren van de OData-query-standaard. Zie voor meer informatie, **$filter** in [OData-queryopties voor systeem met behulp van het OData-eindpunt](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="create-groups"></a>Groepen maken
Voor het maken van een nieuwe groep in uw directory, gebruikt u de cmdlet New-AzureADGroup. Met deze cmdlet maakt een nieuwe beveiligingsgroep met de naam 'Verkoop':

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="update-groups"></a>Groepen bijwerken
Gebruik de cmdlet Set-AzureADGroup voor het bijwerken van een bestaande groep. In dit voorbeeld wordt de eigenschap DisplayName van de groep "Intune-beheerders." is gewijzigd Eerst we de groep met de cmdlet Get-AzureADGroup bent zoeken en filteren met behulp van het kenmerk DisplayName:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

We vervolgens wijzigt de eigenschap Description naar de nieuwe waarde 'Intune apparaat-beheerders':

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Nu als we de groep opnieuw kunt vinden, we dat de eigenschap Description is bijgewerkt zien naar aanleiding van de nieuwe waarde:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="delete-groups"></a>Groepen verwijderen
Als u wilt verwijderen groepen uit uw directory, gebruikt u de cmdlet Remove-AzureADGroup als volgt:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="manage-group-membership"></a>Groepslidmaatschap beheren 
### <a name="add-members"></a>Leden toevoegen
U kunt nieuwe leden toevoegen aan een groep door de cmdlet Add-AzureADGroupMember te gebruiken. Met deze opdracht wordt een lid toegevoegd aan de Intune-beheerders-groep die wordt gebruikt in het vorige voorbeeld:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

De parameter - object-id is de object-id van de groep waarop we willen een lid toevoegen en de RefObjectId - is de object-id van de gebruiker die we willen toevoegen als een lid aan de groep.

### <a name="get-members"></a>Ophalen van leden
Als u de bestaande leden van een groep, gebruikt u de cmdlet Get-AzureADGroupMember, zoals in dit voorbeeld:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

### <a name="remove-members"></a>Leden verwijderen
Als u wilt het lid dat we eerder hebt toegevoegd aan de groep verwijderen, gebruikt u de cmdlet Remove-AzureADGroupMember, zoals hier wordt weergegeven:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

### <a name="verify-members"></a>Controleer of de leden
Als u wilt controleren of de groepslidmaatschappen van een gebruiker, de Select-AzureADGroupIdsUserIsMemberOf-cmdlet te gebruiken. Deze cmdlet wordt gebruikt als de parameters ervan de object-id van de gebruiker waarvoor u om te controleren of de groepslidmaatschappen en een lijst met groepen waarvoor u om te controleren of het lidmaatschap. De lijst met groepen moet worden opgegeven in de vorm van een complex variabele van het type 'Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck', dus eerst we een variabele met dat type maken moet:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Vervolgens geven we waarden voor de groupIds om te controleren in het kenmerk 'GroupIds' van deze complexe variabele:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Nu, als we willen om te controleren of de groepslidmaatschappen van een gebruiker met ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea op basis van de groepen in $g, dat we moet gebruiken:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


De geretourneerde waarde is een lijst van groepen die deze gebruiker lid is. U kunt ook deze methode om te controleren of contactpersonen, groepen of Service-Principals lidmaatschap voor een bepaalde lijst van groepen, met behulp van de Select-AzureADGroupIdsContactIsMemberOf, selecteer AzureADGroupIdsGroupIsMemberOf toepassen of Selecteer AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="disable-group-creation-by-your-users"></a>Groep maken door uw gebruikers uitschakelen
U kunt voorkomen dat niet-beheerders het maken van beveiligingsgroepen. Het standaardgedrag in Microsoft Online Directory Services (MSODS) is niet-beheerders gebruikers groepen maken, dat al dan niet ook Self-service groepsbeheer (SSGM) is ingeschakeld. De SSGM-instelling wordt bepaald gedrag alleen in het toegangsvenster van mijn Apps. 

Groep maken voor niet-beheerders uitschakelen:

1. Controleer of dat niet-beheerders zijn toegestaan om groepen te maken:
   
  ```
  PS C:\> Get-MsolCompanyInformation | fl UsersPermissionToCreateGroupsEnabled
  ```
  
2. Als het resultaat `UsersPermissionToCreateGroupsEnabled : True`, niet-beheerders groepen kunnen maken. Deze functie uitschakelen:
  
  ``` 
  Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
  ```
  
## <a name="manage-owners-of-groups"></a>Eigenaren van groepen beheren
Eigenaren toevoegen aan een groep, gebruikt u de cmdlet Add-AzureADGroupOwner:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

De parameter - object-id is de object-id van de groep waarop we willen een eigenaar toevoegen en de RefObjectId - is de object-id van de gebruiker die we willen toevoegen als een eigenaar van de groep.

Als u wilt ophalen van de eigenaren van een groep, gebruikt u de cmdlet Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

De cmdlet retourneert de lijst met eigenaren voor de opgegeven groep:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Als u een eigenaar van een groep wilt verwijderen wilt, gebruikt u de cmdlet Remove-AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a>Gereserveerde aliassen 
Wanneer een groep is gemaakt, bepaalde-eindpunten kunt de eindgebruiker een mailNickname of alias moet worden gebruikt als onderdeel van het e-mailadres van de groep op te geven. Groepen met de volgende maximaal beschermde e-aliassen kunnen alleen worden gemaakt door een globale beheerder van Azure AD. 
  
* misbruik 
* beheerder 
* de beheerder 
* hostmaster 
* majordomo 
* postbeheerder 
* hoofdmap 
* beveiligen 
* security 
* ssl-admin 
* beheerder 

## <a name="next-steps"></a>Volgende stappen
U vindt meer Azure Active Directory PowerShell-documentatie op [Azure Active Directory-Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).

* [Managing access to resources with Azure Active Directory groups](../fundamentals/active-directory-manage-groups.md?context=azure/active-directory/users-groups-roles/context/ugr-context) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Uw on-premises identiteiten integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
