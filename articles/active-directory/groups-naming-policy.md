---
title: De naam van groepsbeleidsinstellingen voor Office 365-groepen in Azure Active Directory (preview) | Microsoft Docs
description: Het instellen van de vervaldatum voor Office 365-groepen in Azure Active Directory (preview)
services: active-directory
documentationcenter: 
author: curtand
manager: michael.tillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 02/28/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: cc3ea7f81a924f3f4baa6fd2866c4e552b7c160e
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="enforce-a-naming-policy-for-office-365-groups-in-azure-active-directory-preview"></a>Afdwingen van een naamgevingscontainer beleid voor Office 365-groepen in Azure Active Directory (preview)

Als u wilt afdwingen consistent naamgevingsregels voor Office 365-groepen gemaakt of bewerkt door uw gebruikers, een groep die beleid naamgeving voor uw tenants in Azure Active Directory (Azure AD) instellen. Bijvoorbeeld, u kunt de naamgevingsconventie beleidsregel gebruiken om te communiceren, de functie van een groep, lidmaatschap, geografische regio of die de groep hebt gemaakt. U kunt ook het naming beleid om te categoriseren groepen in het adresboek. Het beleid kunt u specifieke woorden worden gebruikt in namen en aliassen blokkeren.

> [!IMPORTANT]
> De evaluatieversie van Office 365-groepen Naming beleid, moet Azure Active Directory Premium P1-licenties voor elke unieke gebruiker die lid is van een of meer Office 365-groepen.

De naamgevingscontainer beleid wordt toegepast op het maken of bewerken van groepen die zijn gemaakt via werkbelastingen (bijvoorbeeld, Outlook, Microsoft-Teams, SharePoint, Exchange of Planner). Deze wordt toegepast op de groepsnaam en de groepsalias. Als u uw naming beleid In Azure AD instellen en u een bestaande Exchange-groep die beleid naming hebt, wordt de Azure AD naming beleid toegepast.

## <a name="naming-policy-features"></a>Naamgeving van functies
U kunt naming beleid voor Office 365-groepen afdwingen op twee verschillende manieren:

-   **Voorvoegsel achtervoegsel naming beleid** kunt u definiëren voor- of achtervoegsels die vervolgens automatisch worden toegevoegd aan het afdwingen van een naamgevingsconventie voor groepen (bijvoorbeeld in de naam van de groep ' groep\_JAPAN\_mijn groep\_ Technisch team", groep\_JAPAN\_ is het voorvoegsel en \_Engineering is het achtervoegsel). 

-   **Aangepaste geblokkeerd woorden** kunt u een set van geblokkeerde woorden specifieke uploaden naar uw organisatie moeten worden geblokkeerd in groepen die zijn gemaakt door gebruikers (bijvoorbeeld ' algemeen Directeur, salaris, HR').

### <a name="prefix-suffix-naming-policy"></a>Voorvoegsel achtervoegsel naming beleid

De algemene structuur van de naamconventie is 'Achtervoegsel voorvoegsel [GroupName]'. U kunt slechts één exemplaar van de [GroupName] hebben in de instelling, terwijl u meerdere voorvoegsels en achtervoegsels kunt. De voor- of achtervoegsels zijn vaste tekenreeksen of gebruikerskenmerken zoals \[afdeling\] die op basis van de gebruiker die het maken van de groep worden vervangen. Het totale aantal tekens voor uw voor- en achtervoegsel tekenreeksen gecombineerd is 53 tekens. 

Voor- en achtervoegsels kunnen speciale tekens die worden ondersteund in de groepsnaam en groepsalias bevatten. Alle tekens in het voorvoegsel of achtervoegsel die worden niet ondersteund in de alias van de groep zijn nog steeds toegepast in de groepsnaam, maar verwijderd uit de alias van de groep. Vanwege deze beperking, de voor- en achtervoegsels toegepast op de naam van de groep mogelijk afwijken van de instellingen toegepast op de alias van de groep. 

#### <a name="fixed-strings"></a>Vaste tekenreeksen

Tekenreeksen kunt u makkelijker te scannen en te onderscheiden van groepen in de globale adreslijst en in de linkernavigatiebalk koppelingen van groep werkbelastingen. Sommige van de meest voorkomende voorvoegsels zijn trefwoorden zoals ' groep\_Name', '\#Name ','\_naam '

#### <a name="user-attributes"></a>Gebruikerskenmerken

Kunt u kenmerken waarmee u en uw gebruikers identificeren welke afdeling, office of geografische regio op waarvoor de groep is gemaakt. Bijvoorbeeld, als u uw naming beleid zo definiëren `PrefixSuffixNamingRequirement = “GRP [GroupName] [Department]”`, en `User’s department = Engineering`, en vervolgens een afgedwongen groepsnaam mogelijk "Mijn groep groep Engineering." Ondersteund Azure AD-kenmerk \[afdeling\], \[bedrijf\], \[Office\], \[als volgt\], \[CountryOrRegion \], \[Titel\]. Niet-ondersteunde gebruikerskenmerken worden behandeld als vaste tekenreeksen; bijvoorbeeld '\[postalCode\]'. Extensie-kenmerken en aangepaste kenmerken worden niet ondersteund.

Het is raadzaam dat u kenmerken die waarden ingevuld voor alle gebruikers in uw organisatie en niet gebruiken voor kenmerken met lange waarden.

### <a name="custom-blocked-words"></a>Aangepaste geblokkeerde woorden

Een lijst met geblokkeerde word is een door komma's gescheiden lijst van vermeldingen moeten worden geblokkeerd in namen en aliassen. Er is geen zoekbewerkingen subtekenreeks worden uitgevoerd. Een exacte overeenkomst tussen de groepsnaam en een of meer van de aangepaste geblokkeerde woorden is vereist voor het activeren van een storing. Zoeken in subtekenreeks wordt niet uitgevoerd, zodat gebruikers veelvoorkomende woorden zoals 'Class' gebruiken kunnen, zelfs als 'klasse' een geblokkeerde woord is.

Lijst met regels voor geblokkeerde word:
- Geblokkeerde woorden zijn niet hoofdlettergevoelig.
- Wanneer een gebruiker een geblokkeerde woord als onderdeel van de naam van een groep krijgt, zien ze een foutbericht met de geblokkeerde word.
- Er zijn geen lengtebeperkingen teken op geblokkeerde woorden.
- Er is een bovenlimiet van 5000 vermeldingen die kunnen worden geconfigureerd in de lijst met geblokkeerde woorden. 

### <a name="administrator-override"></a>Onderdrukking van de beheerder

Geselecteerde beheerders kunnen worden vrijgesteld van deze beleidsregels voor alle werkbelastingen van de groep en eindpunten, zodat ze kunnen groepen met behulp van geblokkeerde woorden maken en met hun eigen naamconventies. Hieronder vindt u de lijst met uitgesloten van de groep die beleid naming beheerdersrollen.

- Globale beheerder
- Partner laag 1-ondersteuning
- Partner laag 2-ondersteuning
- De beheerder van gebruiker
- Schrijvers van mappen

## <a name="install-powershell-cmdlets-to-configure-a-naming-policy"></a>PowerShell-cmdlets voor het configureren van een naamgevingscontainer beleid installeren

Zorg ervoor dat het verwijderen van een oudere versie van de Azure Active Directory PowerShell voor Graph-Module voor Windows PowerShell en installeren [Azure Active Directory PowerShell voor Graph - openbare Preview-versie 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) voordat u de PowerShell-opdrachten. 

1. Open de app Windows PowerShell als beheerder.
2. Een eerdere versie van AzureADPreview verwijderen.
  
  ````
  Uninstall-Module AzureADPreview
  ````
3. Installeer de nieuwste versie van AzureADPreview.
  
  ````
  Install-Module AzureADPreview
  ````
Als u over het openen van een niet-vertrouwde opslagplaats wordt gevraagd, typt u **Y**. Het kan enkele minuten duren voordat de nieuwe module te installeren.

## <a name="configure-the-group-naming-policy-for-a-tenant-using-azure-ad-powershell"></a>De naamgeving van beleid voor een tenant met behulp van Azure AD PowerShell beheergroep configureren

1. Open een Windows PowerShell-venster op uw computer. U kunt openen zonder verhoogde bevoegdheden.

2. Voer de volgende opdrachten om voor te bereiden voor het uitvoeren van de cmdlets.
  
  ````
  Import-Module AzureADPreview
  Connect-AzureAD
  ````
  In de **aanmelden bij uw Account** scherm dat wordt geopend, Voer uw beheerdersaccount en het wachtwoord dat u verbinding maken met uw service en selecteer **aanmelden**.

3. Volg de stappen in [Azure Active Directory-cmdlets voor het configureren van groepsbeleidsinstellingen](active-directory-accessmanagement-groups-settings-cmdlets.md) groepsinstellingen maken voor deze tenant.

### <a name="view-the-current-settings"></a>De huidige instellingen weergeven

1. Haal het huidige naming beleid om de huidige instellingen weer te geven.
  
  ````
  $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
  ````
  
2. De huidige groepsinstellingen wordt weergegeven.
  
  ````
  $Setting.Values
  ````
  
### <a name="set-the-naming-policy-and-custom-blocked-words"></a>Stel de naamgevingsconventie beleid en aangepaste geblokkeerde woorden

1. De groep naam voorvoegsels en achtervoegsels instellen in Azure AD PowerShell.
  
  ````
  $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
  ````
  
2. Stel de aangepaste geblokkeerde woorden die u wilt beperken. Het volgende voorbeeld ziet u hoe u uw eigen aangepaste woorden kunt toevoegen.
  
  ````
  $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
  ````
  
3. Sla de instellingen voor het nieuwe beleid voor het effectief, zoals in het volgende voorbeeld.
  
  ````
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ````
  
Dat is alles. U hebt uw naming beleid en de geblokkeerde woorden toegevoegd.

## <a name="export-or-import-the-list-of-custom-blocked-words"></a>De lijst met aangepaste geblokkeerde woorden importeren of exporteren

Zie voor meer informatie het artikel [Azure Active Directory-cmdlets voor het configureren van groepsbeleidsinstellingen](active-directory-accessmanagement-groups-settings-cmdlets.md).

Hier volgt een voorbeeld van een PowerShell-script meerdere geblokkeerde woorden exporteren:

````
$Words = (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value CustomBlockedWordsList -EQ 
Add-Content "c:\work\currentblockedwordslist.txt" -Value $words.value.Split(",").Replace("`"","")  
````

Hier volgt een voorbeeld van PowerShell-script voor het importeren van meerdere geblokkeerde woorden:

````
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
````

## <a name="naming-policy-experiences-across-office-365-apps"></a>Naamgeving beleid ervaringen via Office 365-apps

Nadat u een groep die beleid in Azure AD, als een gebruiker een groep in een Office 365-app maakt, zien ze naming instellen: 

* Een voorbeeld van de naam volgens uw naming beleid (met de voor- en achtervoegsels) zodra de gebruiker voert de groepsnaam
* Als de gebruiker geblokkeerde woorden invoert, ziet deze een foutbericht weergegeven zodat ze de geblokkeerde woorden kunnen verwijderen.

Workload | Naleving
----------- | -------------------------------
Azure Active Directory-portals | De Azure AD-portal en de portal Toegangsvenster weergeven naming afgedwongen beleidsnaam wanneer de gebruiker in de naam van een groep maken of bewerken van een groep. Wanneer een gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht met de geblokkeerde word weergegeven zodat de gebruiker kan alleen worden verwijderd.
Outlook Web Access (OWA) | Outlook Web Access toont de naamgevingsconventie beleid afgedwongen naam wanneer de gebruiker een groepsnaam of groepsalias. Wanneer een gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht weergegeven in de gebruikersinterface samen met de geblokkeerde word zodat de gebruiker kan alleen worden verwijderd.
Outlook Desktop | Er zijn groepen die zijn gemaakt in de Outlook-bureaublad voldoen aan de naamgeving beleidsinstellingen. Outlook-bureaubladtoepassing het voorbeeld van de naam van de afgedwongen nog niet wordt weergegeven en retourneert de geblokkeerde word aangepaste fouten niet wanneer de gebruiker krijgt de naam van de groep. Echter het naming beleid wordt automatisch toegepast tijdens het maken of bewerken van een groep en gebruikers zien foutberichten worden weergegeven als er aangepaste geblokkeerde woorden in de groepsnaam of alias.
Microsoft Teams | Microsoft-Teams ziet u de groep naamgeving van de naam van beleid afgedwongen wanneer de gebruiker in de teamnaam van een. Wanneer een gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht weergegeven samen met de geblokkeerde word zodat de gebruiker kan alleen worden verwijderd.
SharePoint  |  SharePoint bevat de naamgevingsconventie beleid afgedwongen wanneer de gebruikerstypen een site een naam of e-mailadres groep. Wanneer een gebruiker invoert voor een aangepaste geblokkeerde woord, wordt een foutbericht wordt weergegeven, samen met de geblokkeerde word zodat de gebruiker kan alleen worden verwijderd.
Microsoft Stream | Microsoft Stream ziet u de groep naamgeving van de naam van beleid afgedwongen wanneer de gebruiker een groepsnaam of alias voor de e-mailadres typt. Wanneer een gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht weergegeven met de geblokkeerde word zodat de gebruiker kan alleen worden verwijderd.
Outlook iOS en Android-App | Groepen die zijn gemaakt in de Outlook-apps zijn compatibel met het geconfigureerde naming beleid. Mobiele app voor Outlook de evaluatieversie van de beleidsnaam van de naming afgedwongen nog niet wordt weergegeven en geen aangepaste geblokkeerde word-fouten retourneren, wanneer de gebruiker krijgt de naam van de groep. Echter, het naming beleid automatisch wordt toegepast op het maken/bewerken en gebruikers zien foutberichten worden weergegeven als er aangepaste geblokkeerde woorden in de groepsnaam of alias.
Mobiele app van groepen | Groepen die zijn gemaakt in de mobiele app groepen zijn compatibel met de naamgevingsconventie beleid. Groepen mobiele app wordt niet weergegeven voor de evaluatieversie van het beleid voor naamgeving en aangepaste geblokkeerde word fouten niet weer wanneer de gebruiker krijgt de naam van de groep. Maar het naming beleid wordt automatisch toegepast tijdens het maken of bewerken van een groep en gebruikers wordt weergegeven met de juiste fouten als er aangepaste geblokkeerde woorden in de groepsnaam of alias.
Planner | Planner is compatibel met het beleid voor domeinnaamgeving. Planner toont de naamgevingsconventie beleid preview bij het invoeren van de naam van het abonnement. Wanneer een gebruiker een aangepaste geblokkeerde woord invoert, wordt een foutbericht weergegeven bij het maken van het plan.
Dynamics 365 voor betrokkenheid van de klant | Dynamics 365 voor betrokkenheid van de klant is compatibel met het beleid voor domeinnaamgeving. Dynamics 365 bevat de naamgevingsconventie beleid afgedwongen wanneer de gebruiker een groepsnaam of alias voor de e-mailadres typt. Wanneer de gebruiker in een aangepaste geblokkeerde woord, wordt een foutbericht weergegeven met de geblokkeerde word zodat de gebruiker kan alleen worden verwijderd.
Gegevenssynchronisatie school (SDS) | Groepen die zijn gemaakt via SDS voldoen aan de naamgeving van beleid, maar het naming beleid wordt niet automatisch toegepast. SDS-beheerders hebben de voor- en achtervoegsels toevoegen aan die groepen moeten worden gemaakt en vervolgens geüpload naar SDS klassenamen. Groep maken of bewerken, anders mislukken.
Outlook klant Manager OCM) | Outlook Customer Manager is compatibel met de naamgevingsconventie beleid, automatisch op de groep gemaakt in Outlook Customer Manager toegepast wordt. Als een aangepaste geblokkeerde woord wordt gedetecteerd, wordt het maken van de groep in OCM is geblokkeerd en wordt de gebruiker wordt geblokkeerd vanuit de App OCM.
Leslokaal app | Groepen die zijn gemaakt in een klaslokaal app voldoen aan de naamgeving beleid, maar het naming beleid wordt niet automatisch toegepast, en de naamgevingsconventie beleid preview wordt niet weergegeven aan gebruikers bij het invoeren van een groepsnaam leslokaal. Gebruikers moeten de naam van de groep afgedwongen leslokaal met de voor- en achtervoegsels invoeren. Zo niet, in de leslokaalgroep maken of bewerken mislukt met fouten.
Power BI | Power BI werkruimten zijn compatibel met de naamgevingsconventie beleid.    
Yammer | Yammer verbonden groepen niet de geconfigureerde naming beleid afdwingen. Voor organisaties met de naam van beleid is ingeschakeld, wordt met Yammer verouderde Yammer-groepen die niet zijn verbonden met Office 365 voor groepen die aan de naamgeving beleid voldoen Won't gemaakt.
StaffHub  | StaffHub teams Volg niet de naamgevingsconventie beleid, terwijl de onderliggende Office 365-groep heeft. StaffHub Teamnaam is niet van toepassing de voor- en achtervoegsels en controleert niet op aangepaste geblokkeerde woorden. Maar StaffHub geldt de voor- en achtervoegsels en geblokkeerde woorden verwijderd uit de onderliggende Office 365-groep.
Exchange PowerShell | Exchange PowerShell cmdlets zijn compatibel met de naamgevingsconventie beleid. Gebruikers ontvangen juiste foutberichten met voorgestelde voor- en achtervoegsels en voor aangepaste geblokkeerde woorden als ze niet het naming beleid in de groepsnaam en groepsalias (mailNickname volgen).
Azure Active Directory PowerShell-cmdlets | Azure Active Directory PowerShell-cmdlets zijn voldoen aan de naamgeving van beleid. Gebruikers ontvangen juiste foutberichten met voorgestelde voor- en achtervoegsels en voor aangepaste geblokkeerde woorden als ze niet de naamconventie in namen en groepsalias volgen.
Exchange-beheercentrum | Exchange-beheercentrum is compatibel met de naam van beleid. Gebruikers ontvangen juiste foutberichten met voorgestelde voor- en achtervoegsels en voor aangepaste geblokkeerde woorden als ze de naamconventie in de groepsnaam en de groepsalias niet volgen.
Office 365-beheercentrum | Office 365-beheercentrum is compatibel met de naam van beleid. Wanneer een gebruiker maakt of groepsnamen bewerkingen van het naming beleid wordt automatisch toegepast en gebruikers ontvangen het juiste fouten wanneer ze aangepaste geblokkeerde woorden invoeren. Het Office 365-beheercentrum niet nog een voorbeeld weergeven van het beleid voor naamgeving en geen aangepaste geblokkeerde word-fouten retourneren, wanneer de gebruiker krijgt de naam van de groep.

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure AD-groepen.

* [Zie de bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Verloopbeleid voor Office 365-groepen](active-directory-groups-lifecycle-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Lidmaatschap van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
