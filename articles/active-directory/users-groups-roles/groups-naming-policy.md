---
title: Groepsbeleid naam (preview) - Office 365-groepen - Azure Active Directory | Microsoft Docs
description: Over het instellen van het naamgevingsbeleid voor Office 365-groepen in Azure Active Directory (preview)
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/28/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: c9ca7f457f74202735d3abdf7faaed4ee66745f2
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191211"
---
# <a name="enforce-a-naming-policy-for-office-365-groups-in-azure-active-directory-preview"></a>Afdwingen van een naamgevingsbeleid voor Office 365-groepen in Azure Active Directory (preview)

Als u wilt afdwingen consistente naamconventies voor Office 365-groepen gemaakt of bewerkt door uw gebruikers, instellen van een beleid voor naamgeving voor uw tenants in Azure Active Directory (Azure AD). Bijvoorbeeld, u kunt de naamgeving beleidsregel gebruiken om te communiceren van de functie van een groep, het lidmaatschap, de geografische regio of die de groep hebt gemaakt. U kunt ook het naamgevingsbeleid voor groepen in het adresboek categoriseren. U kunt het beleid moet worden geblokkeerd dat specifieke woorden worden gebruikt in de namen en aliassen.

> [!IMPORTANT]
> Azure Active Directory Premium P1-licenties of Azure AD Basic EDU-licenties vereist met behulp van de Preview-versie voor de naamgeving van beleid voor Office 365-groepen voor elke unieke gebruikers die deel uitmaakt van een of meer Office 365-groepen.

Een naamgevingsbeleid wordt toegepast op het maken of bewerken van groepen die zijn gemaakt voor workloads (bijvoorbeeld Outlook, Microsoft Teams, SharePoint, Exchange of Planner). Deze wordt toegepast op de naam van groep en de groepsalias. Als u het naamgevingsbeleid In Azure AD instellen en u een bestaande Exchange-groep naamgevingsbeleid hebt, wordt de Azure AD naamgevingsbeleid toegepast.

## <a name="naming-policy-features"></a>Naamgeving van policy-functies
U kunt naamgevingsbeleid voor Office 365-groepen afdwingen op twee verschillende manieren:

-   **Voorvoegsel-achtervoegsel naamgevingsbeleid** kunt u de voorvoegsels of achtervoegsels die vervolgens automatisch worden toegevoegd om af te dwingen een naamgevingsconventie voor uw groepen definiëren (bijvoorbeeld in de naam van de ' groep\_JAPAN\_mijn groep\_ Technisch team", groep\_JAPAN\_ is het voorvoegsel en \_Engineering is het achtervoegsel). 

-   **Aangepaste woorden geblokkeerd** kunt u een set van geblokkeerde woorden specifieke uploaden naar uw organisatie moeten worden geblokkeerd in groepen die zijn gemaakt door gebruikers (bijvoorbeeld "CEO, salarisadministratie, HR").

### <a name="prefix-suffix-naming-policy"></a>Naamgevingsbeleid voorvoegsel-achtervoegsel

De algemene structuur van de naamconventie is 'Voorvoegsel [GroupName] achtervoegsel'. Terwijl u meerdere voorvoegsels en achtervoegsels definiëren kunt, kunt u slechts één exemplaar van de [GroupName] in de instelling hebben. De voorvoegsels of achtervoegsels kunnen worden opgelost tekenreeksen of gebruikerskenmerken, zoals \[afdeling\] die op basis van de gebruiker die het maken van de groep worden vervangen. Het totale aantal tekens in voor uw voorvoegsel en het achtervoegsel tekenreeksen gecombineerd is 53 tekens. 

Voor- en achtervoegsels kunnen speciale tekens die worden ondersteund in de groepsnaam en groepsalias bevatten. Alle tekens in het voorvoegsel of achtervoegsel die worden niet ondersteund in de alias van de groep zijn nog steeds toegepast in de naam van de groep, maar verwijderd uit de alias van de groep. Vanwege deze beperking kunnen de voor- en achtervoegsels toegepast op de naam van de afwijken van de namen die zijn toegepast op de alias van de groep zijn. 

#### <a name="fixed-strings"></a>Vaste tekenreeksen

Tekenreeksen kunt u het eenvoudiger om te scannen en groepen in de globale adreslijst en in de navigatiebalk aan de linkerkant een koppeling van de groep werkbelastingen te onderscheiden. Some of the common prefixes are keywords like ‘Grp\_Name’ , ‘\#Name’, ‘\_Name’

#### <a name="user-attributes"></a>Gebruikerskenmerken

U kunt de kenmerken waarmee u kunnen gebruiken en uw gebruikers identificeren welke afdeling, office of geografische regio waarvan de groep is gemaakt. Bijvoorbeeld, als u het naamgevingsbeleid als definiëren `PrefixSuffixNamingRequirement = "GRP [GroupName] [Department]"`, en `User’s department = Engineering`, en vervolgens de groepsnaam van een afgedwongen mogelijk "Mijn groep groep Engineering." Ondersteunde Azure AD-kenmerken zijn \[afdeling\], \[bedrijf\], \[Office\], \[volgt\], \[CountryOrRegion \], \[Titel\]. Niet-ondersteunde gebruikerskenmerken worden behandeld als vaste tekenreeksen; bijvoorbeeld, "\[postcode\]'. Extensiekenmerken en aangepaste kenmerken worden niet ondersteund.

U wordt aangeraden dat u kenmerken die waarden ingevuld voor alle gebruikers in uw organisatie en niet gebruikmaken van kenmerken die lang waarden hebben.

### <a name="custom-blocked-words"></a>Geblokkeerde dan speciale woorden

Een lijst met geblokkeerde woord is een door komma's gescheiden lijst van items moeten worden geblokkeerd in de namen en aliassen. Geen subtekenreeks zoekopdrachten worden uitgevoerd. Een exacte overeenkomst tussen de groepsnaam en een of meer van de aangepaste geblokkeerde woorden is vereist voor het activeren van een storing. Subtekenreeks zoeken wordt niet uitgevoerd, zodat gebruikers algemene woorden als 'Class' gebruiken kunnen, zelfs als 'klasse' een geblokkeerde woord is.

Geblokkeerde woord lijst met regels:

- Geblokkeerde woorden zijn niet hoofdlettergevoelig.
- Wanneer een gebruiker moet een geblokkeerde woord als onderdeel van de naam van een groep invoeren, zien ze een foutbericht weergegeven met het geblokkeerde woord.
- Er zijn geen tekenbeperkingen voor geblokkeerde woorden.
- Er is een bovengrens van 5000 vermeldingen die kunnen worden geconfigureerd in de lijst met geblokkeerde woorden. 

### <a name="administrator-override"></a>Onderdrukking van de beheerder

Geselecteerde beheerders kunnen worden uitgesloten van deze beleidsregels voor alle werkbelastingen van de groep en eindpunten, zodat ze kunnen met behulp van geblokkeerde woorden groepen maken en met hun eigen naamconventies. Hieronder vindt u de lijst met uitgesloten van het beleid voor naamgeving beheerdersrollen.

- Globale beheerder
- Laag 1-ondersteuning voor partner
- Laag 2-ondersteuning voor partner
- Beheerder van gebruikersaccounts
- Schrijvers van mappen

## <a name="install-powershell-cmdlets-to-configure-a-naming-policy"></a>PowerShell-cmdlets voor het configureren van een naamgevingsbeleid installeren

Verwijder een oudere versie van Azure Active Directory PowerShell voor Graph Module voor Windows PowerShell en installeer [Azure Active Directory PowerShell for Graph - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) (Azure Active Directory PowerShell voor Graph - Release 2.0.0.137 voor openbare preview) voordat u de PowerShell-opdrachten uitvoert. 

1. Open de Windows PowerShell-app als beheerder.
2. Verwijder eventuele oudere versies van AzureADPreview.
  
  ```
  Uninstall-Module AzureADPreview
  ```
3. Installeer de nieuwste versie van AzureADPreview.
  
  ```
  Install-Module AzureADPreview
  ```
Als u wordt gevraagd een niet-vertrouwde opslagplaats te openen, typt u **Y**. Het kan enkele minuten duren voordat de nieuwe module is geïnstalleerd.

## <a name="configure-the-group-naming-policy-for-a-tenant-using-azure-ad-powershell"></a>Het beleid voor naamgeving voor een tenant met behulp van Azure AD PowerShell configureren

1. Open een Windows PowerShell-venster op uw computer. U kunt deze zonder verhoogde bevoegdheden te openen.

2. Voer de volgende opdrachten uit als voorbereiding op het uitvoeren van de cmdlets.
  
  ```
  Import-Module AzureADPreview
  Connect-AzureAD
  ```
  In het scherm **Sign in to your Account** dat verschijnt, voert u uw beheerdersaccount en wachtwoord in om verbinding te maken met uw service. Selecteer vervolgens **Aanmelden**.

3. Volg de stappen in [Azure Active Directory-cmdlets voor het configureren van groepsinstellingen](groups-settings-cmdlets.md) om groepsinstellingen voor deze tenant te maken.

### <a name="view-the-current-settings"></a>Huidige instellingen weergeven

1. Ophalen van de huidige naamgevingsbeleid om de huidige instellingen weer te geven.
  
  ```
  $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
  ```
  
2. Geef de instellingen voor de huidige groep weer.
  
  ```
  $Setting.Values
  ```
  
### <a name="set-the-naming-policy-and-custom-blocked-words"></a>Stel de naamgevingsbeleid en de geblokkeerde dan speciale woorden

1. Stel de voor- en achtervoegsels van de groepsnaam in in Azure AD PowerShell. [GroupName] moet in de instelling worden opgenomen om de functie goed te laten werken.
  
  ```
  $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
  ```
  
2. Stel de aangepaste, geblokkeerde woorden in die u wilt verbieden. In het volgende voorbeeld wordt getoond hoe u uw eigen aangepaste woorden kunt toevoegen.
  
  ```
  $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
  ```
  
3. Sla de instellingen voor het nieuwe beleid op zodat het van kracht wordt, zoals in het volgende voorbeeld.
  
  ```
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ```
  
Dat is alles. U hebt het beleid voor naamgeving instellen en de geblokkeerde woorden toegevoegd.

## <a name="export-or-import-the-list-of-custom-blocked-words"></a>De lijst met geblokkeerde dan speciale woorden importeren of exporteren

Zie voor meer informatie het artikel [Azure Active Directory-cmdlets voor het configureren van groepsinstellingen](groups-settings-cmdlets.md).

Hier volgt een voorbeeld van een PowerShell-script voor het exporteren van meerdere geblokkeerde woorden:

```
$Words = (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value CustomBlockedWordsList -EQ 
Add-Content "c:\work\currentblockedwordslist.txt" -Value $words.value.Split(",").Replace("`"","")  
```

Hier volgt een voorbeeld van PowerShell-script voor het importeren van meerdere geblokkeerde woorden:

```
$BadWords = Get-Content "C:\work\currentblockedwordslist.txt"
$BadWords = [string]::join(",", $BadWords)
$Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}
if ($Settings.Count -eq 0)
    {$Template = Get-AzureADDirectorySettingTemplate | Where-Object {$_.DisplayName -eq "Group.Unified"}
    $Settings = $Template.CreateDirectorySetting()
    New-AzureADDirectorySetting -DirectorySetting $Settings
    $Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}}
$Settings["CustomBlockedWordsList"] = $BadWords
$Settings["EnableMSStandardBlockedWords"] = $True
Set-AzureADDirectorySetting -Id $Settings.Id -DirectorySetting $Settings 
```

## <a name="remove-the-naming-policy"></a>Een naamgevingsbeleid verwijderen

1. Wis de voor- en achtervoegsels van de groepsnaam in Azure AD PowerShell.
  
  ```
  $Setting["PrefixSuffixNamingRequirement"] =""
  ```
  
2. Maak de aangepaste lijst met geblokkeerde woorden leeg. 
  
  ```
  $Setting["CustomBlockedWordsList"]=""
  ```
  
3. Sla de instellingen op.
  
  ```
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ```


## <a name="naming-policy-experiences-across-office-365-apps"></a>Naamgevingsbeleid ervaringen in Office 365-apps

Nadat u een beleid voor naamgeving in Azure AD, wanneer een gebruiker een groep in een Office 365-app maakt, zien ze instellen: 

* Een voorbeeld van de naam volgens het naamgevingsbeleid (met voorvoegsels en achtervoegsels) zo snel als de gebruiker typt in de naam van de
* Als de gebruiker geblokkeerde woorden invoert, zien ze een foutbericht weergegeven, zodat ze de geblokkeerde woorden kunnen verwijderen.

Workload | Naleving
----------- | -------------------------------
Azure Active Directory-Portal | De Azure AD-portal en de portal Toegangsvenster tonen de beleidsnaam van de naming afgedwongen wanneer de gebruiker in de naam van een groep maken of bewerken van een groep. Wanneer een gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht weergegeven met het geblokkeerde woord weergegeven zodat de gebruiker kan worden verwijderd.
Outlook Web Access (OWA) | Outlook Web Access ziet u het naamgevingsbeleid afgedwongen naam wanneer de gebruiker een naam of groepsalias. Wanneer een gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht wordt weergegeven in de gebruikersinterface, samen met het geblokkeerde woord zodat de gebruiker kan worden verwijderd.
Outlook Desktop | Groepen die zijn gemaakt in de Outlook-bureaublad die compatibel zijn met de instellingen voor naamgeving. Outlook-bureaublad-app geen nog weergegeven voor de Preview-versie van de naam van de afgedwongen en de geblokkeerde woord aangepaste fouten niet worden geretourneerd wanneer de gebruiker krijgt de naam van de groep. Echter het naamgevingsbeleid wordt automatisch toegepast tijdens het maken of bewerken van een groep, en gebruikers zien foutberichten worden weergegeven als er aangepaste geblokkeerde woorden in de naam of alias.
Microsoft Teams | Microsoft Teams toont de naamgeving van de naam van beleid afgedwongen wanneer de gebruiker de teamnaam van een voert. Wanneer een gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht wordt weergegeven, samen met het geblokkeerde woord zodat de gebruiker kan worden verwijderd.
SharePoint  |  SharePoint bevat de beleidsnaam van de naming afgedwongen wanneer de gebruiker typt een site een naam of een e-mailadres. Wanneer een gebruiker invoert voor een aangepaste geblokkeerde woord, een foutbericht wordt weergegeven, samen met het geblokkeerde woord zodat de gebruiker kan worden verwijderd.
Microsoft Stream | Microsoft Stream toont de naamgeving van de naam van beleid afgedwongen wanneer de gebruiker een groepsnaam of een groep e-mailalias. Wanneer een gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht wordt weergegeven met het geblokkeerde woord, zodat de gebruiker kunt verwijderen.
Outlook-iOS en Android-App | Groepen die zijn gemaakt in de Outlook-apps die compatibel zijn met het geconfigureerde beleid voor naamgeving. Mobiele app van Outlook nog de Preview-versie van de beleidsnaam van de naming afgedwongen niet wordt weergegeven, en geen aangepaste geblokkeerde woord fouten retourneert wanneer de gebruiker krijgt de naam van de groep. Echter het naamgevingsbeleid wordt automatisch toegepast op het maken/bewerken te klikken en gebruikers zien foutberichten worden weergegeven als er aangepaste geblokkeerde woorden in de naam of alias.
Groepen mobiele app | Groepen die zijn gemaakt in de mobiele app van de groepen die compatibel zijn met het naamgevingsbeleid. Groepen mobiele app wordt niet weergegeven voor de Preview-versie van het beleid voor naamgeving en retourneert geen aangepaste geblokkeerde woord fouten als de gebruiker krijgt de naam van de groep. Maar het naamgevingsbeleid wordt automatisch toegepast tijdens het maken of bewerken van een groep en gebruikers wordt weergegeven met de juiste fouten als er aangepaste geblokkeerde woorden in de naam of alias.
Planner | Planner is compatibel met het naamgevingsbeleid. Planner toont de naamgeving voor Preview-versie bij het invoeren van de naam van het plan. Wanneer een gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht wordt weergegeven bij het maken van het abonnement.
Dynamics 365 for Customer Engagement | Dynamics 365 voor Customer Engagement is compatibel met het naamgevingsbeleid. Dynamics 365 ziet u de beleidsnaam van de naming afgedwongen wanneer de gebruiker een groepsnaam of een groep e-mailalias. Wanneer de gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht wordt weergegeven met het geblokkeerde woord zodat de gebruiker het kunt verwijderen.
School Data Sync (SDS) | Groepen die zijn gemaakt via SDS in overeenstemming met het naamgevingsbeleid, maar het naamgevingsbeleid wordt niet automatisch toegepast. SDS-beheerders hebben de voor- en achtervoegsels toevoegen aan klassenamen waarvoor groepen moeten worden gemaakt en vervolgens geüpload naar SDS. Groep maken of bewerken, anders mislukken.
Outlook Customer Manager (OCM) | Outlook Customer Manager is compatibel met het naamgevingsbeleid, automatisch op de groep hebt gemaakt in Outlook Customer Manager toegepast wordt. Als een aangepaste geblokkeerde woord wordt gedetecteerd, het maken van de groep in OCM is geblokkeerd en de gebruiker is geblokkeerd met behulp van de app OCM.
-App classroom | Groepen die zijn gemaakt in de app Classroom in overeenstemming met het naamgevingsbeleid, maar het naamgevingsbeleid wordt niet automatisch toegepast en de naamgeving voor Preview-versie wordt niet weergegeven aan de gebruikers bij het invoeren van de naam van een leslokaal-groep. Gebruikers moeten de naam van de groep afgedwongen leslokaal met voor- en achtervoegsels invoeren. Zo niet, in de classroom-groep maken of bewerken van de bewerking mislukt met fouten.
Power BI | Power BI-werkruimten die compatibel zijn met het naamgevingsbeleid.    
Yammer | Wanneer een gebruiker aangemeld bij Yammer met hun Azure Active Directory-account een groep maakt of bewerkt de naam van een groep, wordt de naam van de voldoen aan het naamgevingsbeleid. Dit geldt zowel voor Office 365 verbonden groepen en alle andere Yammer-groepen.<br>Als een verbonden Office 365-groep is gemaakt voordat het naamgevingsbeleid ingesteld is, wordt het naamgevingsbeleid niet automatisch volgt u de naam van de. Wanneer een gebruiker de naam van de bewerkt, wordt ze gevraagd om toe te voegen voor- en achtervoegsel.
StaffHub  | StaffHub-teams Volg niet de naamgevingsbeleid, maar de onderliggende Office 365-groep heeft. Naam van StaffHub-team is niet van toepassing de voor- en achtervoegsels en controleert niet op geblokkeerde dan speciale woorden. Maar StaffHub geldt de voor- en achtervoegsels en geblokkeerde woorden verwijdert uit de onderliggende Office 365-groep.
Exchange PowerShell | Exchange PowerShell-cmdlets zijn compatibel met het naamgevingsbeleid. Gebruikers ontvangen foutbericht verzonden met voorgestelde voor- en achtervoegsels en voor geblokkeerde dan speciale woorden als ze niet het naamgevingsbeleid in de naam van groep en de groepsalias (mailNickname) volgen.
Azure Active Directory PowerShell-cmdlets | Azure Active Directory PowerShell-cmdlets zijn compatibel met het naamgevingsbeleid. Gebruikers ontvangen foutbericht verzonden met voorgestelde voor- en achtervoegsels en voor geblokkeerde dan speciale woorden als ze de naamconventie in namen van groepen en groepsalias niet volgen.
Exchange-beheercentrum | Exchange-beheercentrum is compatibel met het naamgevingsbeleid. Gebruikers ontvangen foutbericht verzonden met voorgestelde voor- en achtervoegsels en voor geblokkeerde dan speciale woorden als ze de naamconventie in de naam van groep en de groepsalias niet volgen.
Office 365-beheercentrum | Office 365-beheercentrum is compatibel met het naamgevingsbeleid. Wanneer een gebruiker maakt of bewerkingen groepsnamen naamgevingsbeleid wordt automatisch toegepast en gebruikers ontvangen het juiste fouten wanneer ze geblokkeerde dan speciale woorden invoeren. Het Office 365-beheercentrum nog een Preview-versie van het naamgevingsbeleid niet wordt weergegeven en geen aangepaste geblokkeerde woord fouten retourneert wanneer de gebruiker krijgt de naam van de groep.

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure AD-groepen.

* [Bestaande groepen weergeven](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Verloopbeleid voor Office 365-groepen](groups-lifecycle.md)
* [Instellingen van een groep beheren](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Lidmaatschappen van een groep beheren](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](groups-dynamic-membership.md)
