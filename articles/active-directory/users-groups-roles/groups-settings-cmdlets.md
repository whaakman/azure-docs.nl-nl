---
title: Instellingen voor met behulp van PowerShell in Azure Active Directory configureren | Microsoft Docs
description: Beheren hoe de instellingen voor groepen met behulp van Azure Active Directory-cmdlets
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/13/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: f453afee6bc26c5ddcdb5018405ec69455f8f7e8
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295135"
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory cmdlets voor het configureren van groepsinstellingen
In dit artikel bevat instructies voor het gebruik van Azure Active Directory (Azure AD) PowerShell-cmdlets voor groepen maken en bijwerken. Deze inhoud geldt alleen voor Office 365-groepen (ook wel gecombineerde groepen). 

> [!IMPORTANT]
> Sommige instellingen voor nodig een Azure Active Directory Premium P1-licentie. Zie voor meer informatie de [sjablooninstellingen](#template-settings) tabel.

Voor meer informatie over hoe u om te voorkomen dat gebruikers die geen beheerder maken *security* groepen instellen `Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False` zoals beschreven in [Set-MSOLCompanySettings](https://docs.microsoft.com/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0). 

Office 365-groepen instellingen worden geconfigureerd met behulp van een object-instellingen en een SettingsTemplate-object. In eerste instantie wordt er geen instellingenobjecten in uw directory omdat uw directory is geconfigureerd met de standaardinstellingen. Als u wilt de standaardinstellingen wijzigen, moet u een nieuwe instellingenobject met behulp van een sjabloon instellingen maken. Instellingen voor sjablonen zijn gedefinieerd door Microsoft. Er zijn verschillende sjablonen met verschillende instellingen. Office 365-groep om instellingen te configureren voor uw directory, moet u de sjabloon met de naam 'Group.Unified' gebruiken. Gebruik de sjabloon met de naam 'Group.Unified.Guest' voor informatie over het configureren van instellingen voor Office 365-groep op één groep. Deze sjabloon wordt gebruikt voor het beheren van toegang voor gasten voor een Office 365-groep. 

De cmdlets zijn onderdeel van de Azure Active Directory PowerShell V2-module. Voor instructies over het downloaden en installeren van de module op uw computer, Zie het artikel [Azure Active Directory PowerShell versie 2](https://docs.microsoft.com/powershell/azuread/). U kunt de release van versie 2 van de module op basis van installeren [de PowerShell gallery](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="retrieve-a-specific-settings-value"></a>De waarde van een specifieke instellingen ophalen
Als u bekend bent met de naam van de instelling die u wilt ophalen, kunt u de volgende cmdlet voor het ophalen van de huidige waarde van de instellingen. In dit voorbeeld zijn we ophalen van de waarde voor de instelling met de naam "UsageGuidelinesUrl." Meer dat informatie over de directoryinstellingen en hun namen verderop in dit artikel.

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-the-directory-level"></a>Instellingen op het niveau van de map maken
Deze stappen maakt u instellingen op het niveau van de map die van toepassing op alle Office 365-groepen in de map zijn. De cmdlet Get-AzureADDirectorySettingTemplate is alleen beschikbaar in de [Preview van Azure AD PowerShell-module voor Graph](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

1. In de DirectorySettings-cmdlets, moet u de ID van de SettingsTemplate die u wilt gebruiken. Als u deze ID niet weet, wordt de lijst van alle instellingen voor sjablonen in deze cmdlet geretourneerd:
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  Deze cmdlet-aanroep retourneert alle sjablonen die beschikbaar zijn:
  
  ```
  Id                                   DisplayName         Description
  --                                   -----------         -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Office 365 group
  16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
  ```
2. Als u wilt toevoegen een URL van de richtlijn gebruik, moet u eerst het SettingsTemplate-object dat de waarde van de URL in de gebruik richtlijn; definieert ophalen dat wil zeggen, de sjabloon Group.Unified:
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. Maak vervolgens een nieuwe instellingenobject op basis van die sjabloon:
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. Werk vervolgens de waarde van de richtlijn gebruik:
  
  ```
  $setting["UsageGuidelinesUrl"] = "https://guideline.example.com"

  ```  
5. Ten slotte de instellingen van toepassing:
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

De cmdlet retourneert na voltooiing, de ID van de nieuwe voor instellingenobject:
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
## <a name="template-settings"></a>Sjablooninstellingen
Hier vindt u de instellingen die zijn gedefinieerd in de Group.Unified SettingsTemplate. Tenzij anders aangegeven, wordt met deze functies een Azure Active Directory Premium P1-licentie nodig. 

| **Instelling** | **Beschrijving** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Type: Boolean-waarde<li>Standaard: True |De vlag die aangeeft of Office 365-groep maken in de map is toegestaan door niet-beheerders. Deze instelling is niet vereist voor een Azure Active Directory Premium P1-licentie.|
|  <ul><li>GroupCreationAllowedGroupId<li>Type: String<li>Standaard: ' " |GUID van de beveiligingsgroep waarvan de leden kunnen Office 365-groepen maken, zelfs wanneer EnableGroupCreation == false. |
|  <ul><li>UsageGuidelinesUrl<li>Type: String<li>Standaard: ' " |Een koppeling naar de groepsgebruik. |
|  <ul><li>ClassificationDescriptions<li>Type: String<li>Standaard: ' " | Een door komma's gescheiden lijst met beschrijvingen van de classificatie. De waarde van ClassificationDescriptions is alleen geldig in deze indeling:
  $setting ["ClassificationDescriptions"] = 'Classificatie: beschrijving, classificatie: Description', waarbij de classificatie komt overeen met de tekenreeksen in de ClassificationList.|
|  <ul><li>DefaultClassification<li>Type: String<li>Standaard: ' " | De classificatie die moet worden gebruikt als de Standaardclassificatie voor een groep als niets is opgegeven.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Type: String<li>Standaard: ' " | Tekenreeks met een maximale lengte van 64 tekens die de naamconventie die is geconfigureerd voor Office 365-groepen definieert. Zie voor meer informatie, [afdwingen van een naamgevingsbeleid voor Office 365-groepen](groups-naming-policy.md). |
| <ul><li>CustomBlockedWordsList<li>Type: String<li>Standaard: ' " | Door komma's gescheiden tekenreeks van zinnen die gebruikers niet mogen gebruiken in namen of aliassen. Zie voor meer informatie, [afdwingen van een naamgevingsbeleid voor Office 365-groepen](groups-naming-policy.md). |
| <ul><li>EnableMSStandardBlockedWords<li>Type: Boolean-waarde<li>Standaard: "False" | Gebruik geen
|  <ul><li>AllowGuestsToBeGroupOwner<li>Type: Boolean-waarde<li>Standaard: False | Booleaanse waarde waarmee wordt aangegeven of een gastgebruiker moet een eigenaar van de groepen kan worden. |
|  <ul><li>AllowGuestsToAccessGroups<li>Type: Boolean-waarde<li>Standaard: True | Booleaanse waarde waarmee wordt aangegeven of een gastgebruiker toegang tot inhoud voor Office 365-groepen kan hebben.  Deze instelling is niet vereist voor een Azure Active Directory Premium P1-licentie.|
|  <ul><li>GuestUsageGuidelinesUrl<li>Type: String<li>Standaard: ' " | De url van een koppeling naar de richtlijnen voor het gebruik van Gast. |
|  <ul><li>AllowToAddGuests<li>Type: Boolean-waarde<li>Standaard: True | Een Booleaanse waarde die aangeeft of gasten toevoegen aan deze map is toegestaan of niet.|
|  <ul><li>ClassificationList<li>Type: String<li>Standaard: ' " |Een door komma's gescheiden lijst van geldige classificatiewaarden die kunnen worden toegepast op Office 365-groepen. |

## <a name="read-settings-at-the-directory-level"></a>Instellingen op het niveau van de map lezen
Deze stappen lezen instellingen op het niveau van de map die van toepassing op alle Office-groepen in de map zijn.

1. Alle bestaande directoryinstellingen lezen:
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  Deze cmdlet retourneert een lijst van alle directoryinstellingen:
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. Lees alle instellingen voor een specifieke groep:
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. Lees alle waarden in de map instellingen van een specifieke map instellingen-object met behulp van Id-GUID-instellingen:
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  Deze cmdlet retourneert de namen en waarden in dit instellingenobject voor deze specifieke groep:
  ```
  Name                          Value
  ----                          -----
  ClassificationDescriptions
  DefaultClassification
  PrefixSuffixNamingRequirement
  CustomBlockedWordsList        
  AllowGuestsToBeGroupOwner     False 
  AllowGuestsToAccessGroups     True
  GuestUsageGuidelinesUrl
  GroupCreationAllowedGroupId
  AllowToAddGuests              True
  UsageGuidelinesUrl            https://guideline.example.com
  ClassificationList
  EnableGroupCreation           True
  ```

## <a name="update-settings-for-a-specific-group"></a>Instellingen voor een specifieke groep bijwerken

1. Zoeken naar de instellingen-sjabloon met de naam "Groups.Unified.Guest"
  ```
  Get-AzureADDirectorySettingTemplate
  
  Id                                   DisplayName            Description
  --                                   -----------            -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Office 365 group
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
  ```
2. Het ophalen van de sjabloonobject voor de sjabloon Groups.Unified.Guest:
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. Maak een nieuw instellingenobject van de sjabloon:
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. De instelling ingesteld op de vereiste waarde:
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. Maak de nieuwe instelling voor de vereiste groep in de map:
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-the-directory-level"></a>Update-instellingen op het niveau van de map

Deze stappen werken instellingen op het niveau van de map die van toepassing op alle Office 365-groepen in de map. Deze voorbeelden wordt ervan uitgegaan dat er is al een object-instellingen in uw directory.

1. Zoek het bestaande object in de instellingen:
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. Werk de waarde:
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. Werk de instelling:
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-the-directory-level"></a>Instellingen op het niveau van de map verwijderen
Deze stap worden de instellingen op het niveau van de map die van toepassing op alle Office-groepen in de map verwijderd.
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a>Naslaginformatie over de syntaxis van cmdlets
U vindt meer Azure Active Directory PowerShell-documentatie op [Azure Active Directory-Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="additional-reading"></a>Meer lezen

* [Managing access to resources with Azure Active Directory groups](../fundamentals/active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Uw on-premises identiteiten integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
