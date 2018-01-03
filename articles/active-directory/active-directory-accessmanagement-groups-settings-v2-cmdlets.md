---
title: PowerShell-voorbeelden voor het beheren van groepen in Azure Active Directory | Microsoft Docs
description: Deze pagina vindt u voorbeelden van PowerShell om u te helpen bij het beheren van uw groepen in Azure Active Directory
keywords: Azure AD, Azure Active Directory PowerShell, groepen, groepsbeheer
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 7a5023dc-2727-4c25-8254-b531fc3244ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: 3f57e1a0ded679325c8c739e73cc79f69c037191
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>Azure Active Directory-cmdlets van versie 2 voor groepsbeheer
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Dit artikel bevat voorbeelden van hoe u PowerShell gebruikt voor het beheren van uw groepen in Azure Active Directory (Azure AD).  Deze ook wordt uitgelegd hoe u met de Azure AD PowerShell-module ophalen instellen. Eerst moet u [downloaden van de Azure AD PowerShell-module](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="install-the-azure-ad-powershell-module"></a>Installeer de Azure AD PowerShell-module
Gebruik de volgende opdrachten voor het installeren van de Azure AD PowerShell-module:

    PS C:\Windows\system32> install-module azuread

Controleer of de module is geïnstalleerd, moet u de volgende opdracht gebruiken:

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

U kunt nu starten met de cmdlets in de module. Voor een volledige beschrijving van de cmdlets in de Azure AD-module raadpleegt u de online documentatie bij [Azure Active Directory PowerShell versie 2](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="connect-to-the-directory"></a>Verbinding maken met de directory
Voordat u kunt het beheer van groepen met behulp van Azure AD PowerShell-cmdlets, moet u de PowerShell-sessie verbinding maken met de map die u wilt beheren. Gebruik de volgende opdracht:

    PS C:\Windows\system32> Connect-AzureAD

De cmdlet wordt u gevraagd om de referenties die u gebruiken wilt voor toegang tot uw directory. In dit voorbeeld gebruiken we karen@drumkit.onmicrosoft.com voor toegang tot de map demonstratie. De cmdlet retourneert een bevestiging voor het weergeven van dat de sessie met succes is verbonden met uw directory:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

U kunt nu starten met de AzureAD-cmdlets voor het beheren van groepen in uw directory.

## <a name="retrieve-groups"></a>Groepen ophalen
Voor het bestaande groepen ophalen uit de directory, gebruikt u de cmdlet Get-AzureADGroups. 

Gebruik de cmdlet zonder parameters voor het ophalen van alle groepen in de map:

    PS C:\Windows\system32> get-azureadgroup

De cmdlet retourneert alle groepen in de gekoppelde map.

U kunt de parameter - object-id gebruiken voor het ophalen van een specifieke groep waarvoor u de groep objectID opgeven:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

De cmdlet retourneert nu de groep waarvan objectID overeenkomt met de waarde van de parameter die u hebt ingevoerd:

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

U kunt zoeken naar een specifieke groep met de - filterparameter. Deze parameter heeft een ODATA-filter-component en resulteert in alle groepen die overeenkomen met het filter, zoals in het volgende voorbeeld:

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
> De Azure AD PowerShell-cmdlets implementeren de OData-query-norm. Zie voor meer informatie **$filter** in [OData-query-opties voor systeem met behulp van de OData-eindpunt](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="create-groups"></a>Groepen maken
Voor het maken van een nieuwe groep in uw directory, gebruikt u de cmdlet New-AzureADGroup. Deze cmdlet maakt een nieuwe beveiligingsgroep met de naam 'Marketing':

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="update-groups"></a>Groepen bijwerken
Gebruik de cmdlet Set-AzureADGroup voor het bijwerken van een bestaande groep. In dit voorbeeld wilt wordt de eigenschap DisplayName van de groep 'Intune-beheerders.' wijzigen Eerst we de groep met de cmdlet Get-AzureADGroup bent zoeken en filteren met het kenmerk DisplayName:

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

Vervolgens wordt de eigenschap Description wijzigt op de nieuwe waarde 'Intune apparaat Administrators':

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Nu als we de groep opnieuw vinden, ziet u dat de eigenschap Description is bijgewerkt, zodat de nieuwe waarde:

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

## <a name="delete-groups"></a>Verwijderen van groepen
Als u wilt verwijderen van groepen van uw directory, gebruikt u de cmdlet Remove-AzureADGroup als volgt:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="manage-group-membership"></a>Groepslidmaatschap beheren 
### <a name="add-members"></a>Leden toevoegen
U kunt nieuwe leden toevoegen aan een groep door de cmdlet Add-AzureADGroupMember te gebruiken. Een lid met deze opdracht toegevoegd aan de groep van de Intune-beheerders die hebben we in het vorige voorbeeld gebruikt:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

De parameter - object-id is de object-id van de groep waarop we willen geen lid toevoegen en de RefObjectId - id van de gebruiker wilt toevoegen als lid aan de groep is.

### <a name="get-members"></a>Leden ophalen
Als u de bestaande leden van een groep, gebruikt u de cmdlet Get-AzureADGroupMember, zoals in dit voorbeeld:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

### <a name="remove-members"></a>Leden verwijderen
U kunt het lid dat we eerder zijn toegevoegd aan de groep verwijderen met de cmdlet Remove-AzureADGroupMember, zoals hier wordt weergegeven:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

### <a name="verify-members"></a>Controleer of leden
Om te controleren of de groepslidmaatschappen van een gebruiker, selecteer AzureADGroupIdsUserIsMemberOf cmdlet te gebruiken. Deze cmdlet heeft als de parameters de object-id van de gebruiker waarvoor u wilt controleren van de groepslidmaatschappen en een lijst met groepen waarvoor u de lidmaatschappen controleren. De lijst met groepen moet zijn opgegeven in de vorm van een complex variabele van type 'Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck', zodat we eerst moet een variabele met dit type maken:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

We bieden vervolgens waarden voor de groupIds om te controleren in het kenmerk 'GroupIds' van deze variabele complex:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Nu als we het groepslidmaatschap van een gebruiker met ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea tegen de groepen in $g controleren willen, dat we moeten gebruiken:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


De geretourneerde waarde is een lijst met groepen waarvan deze gebruiker lid is. U kunt deze methode om te controleren, contactpersonen, groepen of Service-Principals lidmaatschap voor een opgegeven lijst met groepen met behulp van de Select-AzureADGroupIdsContactIsMemberOf, selecteer AzureADGroupIdsGroupIsMemberOf ook toepassen of Selecteer AzureADGroupIdsServicePrincipalIsMemberOf

## <a name="disable-group-creation-by-your-users"></a>Groep maken door uw gebruikers uitschakelen
U kunt voorkomen dat gebruikers zonder beheerdersrechten maken van beveiligingsgroepen. Het standaardgedrag in Microsoft Online Directory Services (MSODS) is niet-beheerders gebruikers groepen maken, kunnen al dan niet Self-service groepsbeheer (SSGM) ook is ingeschakeld. De instelling SSGM bepaalt gedrag alleen in het deelvenster Mijn Apps toegang. 

Het maken van de groep voor niet-beheerders uitschakelen:

1. Controleer of de niet-beheerders mogen om groepen te maken:
   
  ````
  PS C:\> Get-MsolCompanyInformation | fl UsersPermissionToCreateGroupsEnabled
  ````
  
2. Als het resultaat `UsersPermissionToCreateGroupsEnabled : True`, en vervolgens gebruikers niet-beheerders kunnen groepen maken. Deze functie uitschakelen:
  
  ```` 
  Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
  ````
  
## <a name="manage-owners-of-groups"></a>Eigenaren van groepen beheren
Als eigenaars toevoegen aan een groep, kunt u de cmdlet Add-AzureADGroupOwner:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

De parameter - object-id is de object-id van de groep die we wilt toevoegen van een eigenaar en de RefObjectId - id van de gebruiker wilt toevoegen als een eigenaar van de groep is.

Gebruik de cmdlet Get-AzureADGroupOwner voor het ophalen van de eigenaren van een groep:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

De cmdlet retourneert de lijst met eigenaren voor de opgegeven groep:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Als u een eigenaar verwijderen uit een groep wilt, gebruikt u de cmdlet Remove-AzureADGroupOwner:

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a>Gereserveerde aliassen 
Wanneer een groep is gemaakt, bepaalde eindpunten toestaan dat de eindgebruiker een mailNickname of alias moet worden gebruikt als onderdeel van het e-mailadres van de groep opgeven. Groepen met de volgende bijzondere rechten e-aliassen kunnen alleen worden gemaakt door een globale beheerder van Azure AD. 
  
* misbruik 
* Beheerder 
* Beheerder 
* hostmaster 
* majordomo 
* postbeheerder 
* hoofdmap 
* Beveiligen 
* security 
* SSL-beheerder 
* beheerder 

## <a name="next-steps"></a>Volgende stappen
U vindt meer Azure Active Directory PowerShell-documentatie op [Azure Active Directory-Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).

* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
