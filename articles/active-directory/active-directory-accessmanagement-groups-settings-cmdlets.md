---
title: Groepsinstellingen met PowerShell in Azure Active Directory configureren | Microsoft Docs
description: Hoe beheert u de instellingen voor groepen met behulp van Azure Active Directory-cmdlets
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 9f2090e6-3af4-4f07-bbb2-1d18dae89b73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;
ms.openlocfilehash: 331dafc9164e315c84036fa0af11820e89066f36
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory cmdlets voor het configureren van groepsinstellingen
In dit artikel bevat instructies voor het gebruik van Azure Active Directory (Azure AD), PowerShell-cmdlets voor groepen maken en bijwerken. Deze inhoud geldt alleen voor Office 365-groepen. 

> [!IMPORTANT]
> Sommige instellingen vereist een Azure Active Directory Premium P1-licentie. Zie voor meer informatie de [sjablooninstellingen](#template-settings) tabel.

Voor meer informatie over het voorkomen dat gebruikers niet-beheerders maken *beveiliging* groepen ingesteld `Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False` zoals beschreven in [Set-MSOLCompanySettings](https://docs.microsoft.com/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0). 

Instellingen voor Office 365-groepen zijn geconfigureerd met een object-instellingen en een SettingsTemplate-object. In eerste instantie is er geen voor instellingenobjecten in uw directory omdat uw directory is geconfigureerd met de standaardinstellingen. Als u wilt de standaardinstellingen wilt wijzigen, moet u een nieuw object voor instellingen met een sjabloon voor instellingen. Instellingen voor sjablonen zijn gedefinieerd door Microsoft. Er zijn verschillende sjablonen met verschillende instellingen. Instellingen voor Office 365-groep voor uw directory configureren, moet u de sjabloon met de naam 'Group.Unified' gebruiken. Gebruik de sjabloon met de naam "Group.Unified.Guest" voor informatie over het configureren van instellingen voor Office 365-groep op één groep. Deze sjabloon wordt gebruikt voor het beheren van gasttoegang tot een Office 365-groep. 

De cmdlets maken deel uit van de Azure Active Directory PowerShell V2-module. Voor instructies het downloaden en installeren van de module op uw computer, Zie het artikel [Azure Active Directory PowerShell versie 2](https://docs.microsoft.com/powershell/azuread/). U kunt installeren de release versie 2 van de module op basis van [de PowerShell-galerie](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="retrieve-a-specific-settings-value"></a>De waarde van een specifieke instellingen ophalen
Als u de naam van de instelling die u wilt ophalen, kunt u de onderstaande cmdlet voor het ophalen van de huidige waarde van de instellingen. In dit voorbeeld ophaalt we de waarde voor een instelling met de naam 'UsageGuidelinesUrl'. U vindt dat meer informatie over instellingen voor directory en hun namen verder omlaag in dit artikel.

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-the-directory-level"></a>Instellingen op het niveau van de directory maken
Deze stappen maken instellingen op het niveau van de directory die van toepassing op alle Office 365-groepen (Unified groepen) in de map.

1. In de cmdlets DirectorySettings, moet u de ID van de SettingsTemplate die u wilt gebruiken. Als u deze ID niet weet, retourneert deze cmdlet de lijst met alle instellingen sjablonen:
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  De aanroep van deze cmdlet retourneert alle sjablonen die beschikbaar zijn:
  
  ```
  Id                                   DisplayName         Description
  --                                   -----------         -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Unified Group
  16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
  ```
2. Als u wilt een richtlijn gebruik URL kunt toevoegen, moet u eerst het SettingsTemplate-object dat de waarde van de URL in de gebruik richtlijn; definieert ophalen dat wil zeggen, de sjabloon Group.Unified:
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. Maak vervolgens een nieuw object van instellingen op basis van die sjabloon:
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. Werk vervolgens de waarde van de richtlijn gebruik:
  
  ```
  $setting["UsageGuidelinesUrl"] = "<https://guideline.com>"
  ```  
5. Ten slotte de instellingen toepassen:
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

Heeft voltooid wordt de cmdlet de ID van de nieuwe voor instellingenobject:
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
## <a name="template-settings"></a>Sjablooninstellingen
Hier vindt u de instellingen die zijn gedefinieerd in de Group.Unified SettingsTemplate. Tenzij anders vermeld, wordt door deze functies een Azure Active Directory Premium P1-licentie vereist. 

| **Instelling** | **Beschrijving** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Type: Booleaanse<li>Standaard: True |De vlag die aangeeft of het maken van Unified groep in de map is toegestaan door niet-beheerders. Deze instelling vereist niet dat voor een Azure Active Directory Premium P1-licentie.|
|  <ul><li>GroupCreationAllowedGroupId<li>Type: String<li>Standaardwaarde: ' " |GUID van de beveiligingsgroep waarvan de leden mogen Unified groepen maken, zelfs wanneer EnableGroupCreation == false. |
|  <ul><li>UsageGuidelinesUrl<li>Type: String<li>Standaardwaarde: ' " |Een koppeling naar de richtlijnen voor het gebruik van groep. |
|  <ul><li>ClassificationDescriptions<li>Type: String<li>Standaardwaarde: ' " | Een door komma's gescheiden lijst met beschrijvingen van de classificatie. |
|  <ul><li>DefaultClassification<li>Type: String<li>Standaardwaarde: ' " | De classificatie die moet worden gebruikt als de Standaardclassificatie voor een groep als deze is niet opgegeven.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Type: String<li>Standaardwaarde: ' " | Gebruik geen. Niet geïmplementeerd. |
| <ul><li>CustomBlockedWordsList<li>Type: String<li>Standaardwaarde: ' " | Gebruik geen. Niet geïmplementeerd. |
| <ul><li>EnableMSStandardBlockedWords<li>Type: Booleaanse<li>Standaardwaarde: 'False' | Gebruik geen
|  <ul><li>AllowGuestsToBeGroupOwner<li>Type: Booleaanse<li>Standaard: False | Een Boolean die aangeeft of een gastgebruiker moet een eigenaar van de groepen kan worden. |
|  <ul><li>AllowGuestsToAccessGroups<li>Type: Booleaanse<li>Standaard: True | Een Boolean die aangeeft of een gastgebruiker toegang tot inhoud Unified-groepen krijgen kan.  Deze instelling vereist niet dat voor een Azure Active Directory Premium P1-licentie.|
|  <ul><li>GuestUsageGuidelinesUrl<li>Type: String<li>Standaardwaarde: ' " | De url van een koppeling naar de richtlijnen voor het gebruik van Gast. |
|  <ul><li>AllowToAddGuests<li>Type: Booleaanse<li>Standaard: True | Een boolean die aangeeft of of niet is toegestaan gasten toevoegen aan deze map.|
|  <ul><li>ClassificationList<li>Type: String<li>Standaardwaarde: ' " |Een door komma's gescheiden lijst met geldige classificatiewaarden die kunnen worden toegepast op groepen Unified. |


## <a name="read-settings-at-the-directory-level"></a>Instellingen op het mapniveau van de lezen
Volgende stappen uit op het niveau van de directory-instellingen die van toepassing op alle Office-groepen in de map gelezen.

1. Lees alle bestaande directoryinstellingen:
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  Deze cmdlet retourneert een lijst met alle directoryinstellingen:
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. Lees alle instellingen voor een specifieke groep:
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. Lezen van alle waarden in de directory instellingen van een specifieke map instellingen-object met Id-GUID-instellingen:
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  Deze cmdlet retourneert de namen en waarden in deze object-instellingen voor deze specifieke groep:
  ```
  Name                          Value
  ----                          -----
  ClassificationDescriptions
  DefaultClassification
  PrefixSuffixNamingRequirement
  AllowGuestsToBeGroupOwner     False 
  AllowGuestsToAccessGroups     True
  GuestUsageGuidelinesUrl
  GroupCreationAllowedGroupId
  AllowToAddGuests              True
  UsageGuidelinesUrl            <https://guideline.com>
  ClassificationList
  EnableGroupCreation           True
  ```

## <a name="update-settings-for-a-specific-group"></a>Instellingen voor een specifieke groep bijwerken

1. Zoeken naar de instellingen sjabloon met de naam 'Groups.Unified.Guest'
  ```
  Get-AzureADDirectorySettingTemplate
  
  Id                                   DisplayName            Description
  --                                   -----------            -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Unified Group
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
  ```
2. Het ophalen van de sjabloonobject voor de sjabloon Groups.Unified.Guest:
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. Maak een nieuw object van de instellingen van de sjabloon:
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. Stel de instelling op de vereiste waarde:
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. De nieuwe instelling maken voor de vereiste groep in de map:
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-the-directory-level"></a>Update-instellingen op het niveau van de directory

Deze stappen bijwerken op het niveau van de directory-instellingen die van toepassing op alle Unified groepen in Active directory. Deze voorbeelden wordt ervan uitgegaan dat er is al een object-instellingen in uw directory.

1. Zoek het bestaande object-instellingen:
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

## <a name="remove-settings-at-the-directory-level"></a>Verwijder de instellingen op het niveau van de directory
Deze stap verwijdert instellingen op het niveau van de directory die van toepassing op alle Office-groepen in de map.
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a>Verwijzing naar de cmdlet
U vindt meer Azure Active Directory PowerShell-documentatie op [Azure Active Directory-Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="additional-reading"></a>Aanvullende bronnen

* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
