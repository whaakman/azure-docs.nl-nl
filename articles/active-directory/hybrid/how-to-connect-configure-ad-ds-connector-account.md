---
title: 'Azure AD Connect: Accountmachtigingen voor AD DS-Connector configureren | Microsoft Docs'
description: Dit document wordt uitgelegd hoe u de AD DS-Connector-account configureert met de nieuwe ADSyncConfig PowerShell-module
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 10/12/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: f57a5a2413103ddcf7484f3b1fc5b4170b7bdc98
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412853"
---
# <a name="azure-ad-connectconfigure-ad-ds-connector-account-permissions"></a>Azure AD Connect: Accountmachtigingen voor AD DS-Connector configureren 

Een nieuwe PowerShell-Module met de naam [ADSyncConfig.psm1](reference-connect-adsyncconfig.md) werd geïntroduceerd in build 1.1.880.0 (uitgebracht in augustus 2018) een verzameling van cmdlets bevat kunt u de juiste Active Directory-machtigingen configureren voor uw Azure AD Verbinding met het maken van de implementatie. 

## <a name="overview"></a>Overzicht 
De volgende PowerShell-cmdlets kan worden gebruikt voor installatie van Active Directory-machtigingen van de AD DS-Connector-account voor elke functie die u alleen om in te schakelen in Azure AD Connect. Om te voorkomen dat problemen ondervindt, moet u bereid Active Directory-machtigingen op voorhand voor wanneer u wilt installeren, Azure AD Connect met behulp van een aangepast domein-account verbinding maken met uw forest. Deze module ADSyncConfig kan ook worden gebruikt om machtigingen te configureren na de implementatie van Azure AD Connect.

![](media/how-to-connect-configure-ad-ds-connector-account/configure1.png)

Voor installatie van de Azure AD verbinding maken met Express, is een automatisch gegenereerde (MSOL_nnnnnnnnnn)-account gemaakt in Active Directory met de vereiste machtigingen, dus u hoeft niet op deze module ADSyncConfig, tenzij u machtigingen hebt geblokkeerd overname op organisatie-eenheden of specifieke Active Directory-objecten die u wilt synchroniseren met Azure AD. 
 
### <a name="permissions-summary"></a>Overzicht van bevoegdheden 
De volgende tabel bevat een samenvatting van de vereiste machtigingen voor AD-objecten: 

| Functie | Machtigingen |
| --- | --- |
| MS-DS-ConsistencyGuid functie |Schrijfmachtigingen heeft voor het kenmerk ms-DS-ConsistencyGuid is beschreven in [ontwerpconcepten - ms-DS-ConsistencyGuid gebruiken als sourceAnchor](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor). | 
| Synchronisatie van wachtwoordhashes |<li>Directorywijzigingen repliceren</li>  <li>Alle repliceren Directory gewijzigd |
| Hybride implementatie voor Exchange |Schrijfmachtigingen heeft voor de kenmerken beschreven in [Exchange hybrid terugschrijven](reference-connect-sync-attributes-synchronized.md#exchange-hybrid-writeback) voor gebruikers, groepen en contactpersonen. |
| Openbare map voor Exchange-e-Mail |Leesmachtigingen voor de kenmerken beschreven in [Exchange Mail openbare map](reference-connect-sync-attributes-synchronized.md#exchange-mail-public-folder) voor openbare mappen. | 
| Wachtwoord terugschrijven |Schrijfmachtigingen heeft voor de kenmerken beschreven in [aan de slag met wachtwoordbeheer](../authentication/howto-sspr-writeback.md) voor gebruikers. |
| Apparaat terugschrijven |Schrijfmachtigingen heeft voor apparaatobjecten en containers die worden beschreven in [Write-back van apparaat](how-to-connect-device-writeback.md). |
| Groep terugschrijven |Lezen, maken, bijwerken en verwijderen van groep objecten voor gesynchroniseerd **Office 365-groepen**.  Zie voor meer informatie [Write-back van groep](how-to-connect-preview.md#group-writeback).|

## <a name="using-the-adsyncconfig-powershell-module"></a>Met behulp van de ADSyncConfig PowerShell-Module 
De module ADSyncConfig vereist de [Remote Server Administration Tools (RSAT) voor AD DS](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) omdat deze afhankelijk van de AD DS-PowerShell-module en de hulpprogramma's is. Als u wilt installeren RSAT voor AD DS, open een Windows PowerShell-venster met 'Als Administrator uitvoeren' en uitvoeren: 

``` powershell
Install-WindowsFeature RSAT-AD-Tools 
```
![Configureren](media/how-to-connect-configure-ad-ds-connector-account/configure2.png)

>[!NOTE]
>U kunt ook het bestand kopiëren **C:\Program Files\Microsoft Azure Active Directory Connect\AdSyncConfig\ADSyncConfig.psm1** naar een domeincontroller waarop RSAT al heeft voor AD DS geïnstalleerd en deze PowerShell-module van daaruit gebruiken.

Als u wilt gaan met behulp van de ADSyncConfig die u wilt laden van de module in een Windows PowerShell-venster: 

``` powershell
Import-Module "C:\Program Files\Microsoft Azure Active Directory Connect\AdSyncConfig\AdSyncConfig.psm1" 
```

Om te controleren of alle cmdlets die zijn opgenomen in deze module die kunt u typen:  

``` powershell
Get-Command -Module AdSyncConfig  
```

![Selecteren](media/how-to-connect-configure-ad-ds-connector-account/configure3.png)

Elke cmdlet heeft dezelfde parameters voor het invoeren van de AD DS-Connector-Account en een switch AdminSDHolder. Als u uw AD DS-Connector-Account, kunt u de accountnaam en -domein of alleen het account bieden DN (Distinguished Name),

bijvoorbeeld: 

`Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName ADaccount -ADConnectorAccountDomain Contoso`

Of; 

`Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN 'CN=ADaccount,OU=AADconnect,DC=Contoso,DC=com'`

Als u niet wilt om machtigingen voor de container AdminSDHolder te wijzigen, gebruikt u de schakeloptie `-SkipAdminSdHolders`. 

Standaard probeert alle cmdlets van de set machtigingen in te stellen van AD DS-machtigingen in de hoofdmap van elk domein in het Forest, wat betekent dat de gebruiker de PowerShell-sessie die is vereist voor domein Administrator-rechten op elk domein in het Forest.  Vanwege deze vereiste is het aanbevolen gebruik van een Enterprise-beheerder van het foresthoofddomein. Als uw Azure AD Connect-implementatie meerdere AD DS-Connectors heeft, wordt deze moet de dezelfde cmdlet uitvoeren op elke forest met een AD DS-Connector. 

U kunt ook machtigingen instellen voor een specifieke organisatie-eenheid of AD DS-object met behulp van de parameter `-ADobjectDN` gevolgd door de DN-naam van het doelobject waar u machtigingen wilt instellen. Wanneer u een doel ADobjectDN gebruikt, wordt de cmdlet machtigingen voor dit object ingesteld en niet op het hoofddomein of de AdminSDHolder container. Deze parameter kan handig zijn wanneer er een bepaalde organisatie-eenheden of AD DS-objecten dat overname van machtigingen uitgeschakeld (Zie zoeken naar AD DS-objecten met de overname van machtigingen uitgeschakeld) 

Uitzonderingen op deze algemene parameters zijn de `Set-ADSyncRestrictedPermissions` cmdlet die wordt gebruikt voor de machtigingen instellen voor het AD DS-Connector-Account zelf, en de `Set-ADSyncPasswordHashSyncPermissions` cmdlet omdat de machtigingen die zijn vereist voor synchronisatie van Wachtwoordhashes alleen op de domeinhoofdmap, daarom zijn ingesteld Deze cmdlet omvat niet de `-ObjectDN` of `-SkipAdminSdHolders` parameters.

### <a name="determine-your-ad-ds-connector-account"></a>Bepalen van uw AD DS-Connector Account 
Azure AD Connect is al geïnstalleerd en u wilt controleren wat het AD DS-Connector-Account dat momenteel in gebruik door Azure AD Connect is, kunt u de cmdlet uitvoert: 

``` powershell
Get-ADSyncADConnectorAccount 
```
### <a name="locate-ad-ds-objects-with-permission-inheritance-disabled"></a>Ga naar AD DS-objecten met de overname van machtigingen is uitgeschakeld 
Als u controleren wilt of er AD DS-object met de overname van machtigingen is uitgeschakeld, kunt u het volgende uitvoeren: 

``` powershell
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase '<DistinguishedName>' 
```
Standaard deze cmdlet wordt alleen gezocht naar organisatie-eenheden met uitgeschakelde overname, maar kunt u andere klassen van de AD DS-object in `-ObjectClass` parameter of gebruik ' *' voor alle, als volgt objectklassen: 

``` powershell
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase '<DistinguishedName>' -ObjectClass * 
```
 
### <a name="view-ad-ds-permissions-of-an-object"></a>AD DS-machtigingen van een object weergeven 
U kunt onderstaande cmdlet gebruiken om de lijst met machtigingen die is ingesteld op een Active Directory-object door te geven van de DistinguishedName weer te geven: 

``` powershell
Show-ADSyncADObjectPermissions -ADobjectDN '<DistinguishedName>' 
```

## <a name="configure-ad-ds-connector-account-permissions"></a>Accountmachtigingen voor AD DS-Connector configureren 
 
### <a name="configure-basic-read-only-permissions"></a>Basic alleen-lezen-machtigingen configureren 
Om in te stellen basic alleen-lezen machtigingen voor het AD DS-Connector-account als een functie van Azure AD Connect niet gebruikt, voert u de volgende uit: 

``` powershell
Set-ADSyncBasicReadPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


Of; 

``` powershell
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```


Deze cmdlet wordt de volgende machtigingen ingesteld: 
 

|Type |Naam |Access |Van toepassing op| 
|-----|-----|-----|-----|
|Toestaan |AD DS-Connector-Account |Alle eigenschappen lezen |Onderliggende apparaatobjecten| 
|Toestaan |AD DS-Connector-Account|Alle eigenschappen lezen |Onderliggende InetOrgPerson-objecten| 
|Toestaan |AD DS-Connector-Account |Alle eigenschappen lezen |Onderliggende objecten van de Computer| 
|Toestaan |AD DS-Connector-Account |Alle eigenschappen lezen |Onderliggende foreignSecurityPrincipal objecten| 
|Toestaan |AD DS-Connector-Account |Alle eigenschappen lezen |Onderliggende groep objecten worden weergegeven| 
|Toestaan |AD DS-Connector-Account |Alle eigenschappen lezen |Onderliggende gebruikersobjecten| 
|Toestaan |AD DS-Connector-Account |Alle eigenschappen lezen |Onderliggende objecten van de contactpersoon| 

 
### <a name="configure-ms-ds-consistency-guid-permissions"></a>MS-DS-consistentie-Guid-machtigingen configureren 
Om machtigingen voor het AD DS-Connector-account bij het gebruik van het kenmerk ms-Ds-consistentie-Guid als het bronankerkenmerk (ook wel "Laten beheren door Azure het bronanker voor mij" optie), voert u de volgende uit: 

``` powershell
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```

Of; 

``` powershell
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```

Deze cmdlet wordt de volgende machtigingen ingesteld: 

|Type |Naam |Access |Van toepassing op|
|-----|-----|-----|-----| 
|Toestaan|AD DS-Connector-Account|Eigenschap voor lezen/schrijven|MS-DS-consistentie-Guid|Onderliggende gebruikersobjecten|

### <a name="permissions-for-password-hash-synchronization"></a>Machtigingen voor de wachtwoord-Hashsynchronisatie 
Om machtigingen voor het AD DS-Connector-account bij het gebruik van wachtwoord-Hashsynchronisatie, voert u de volgende uit: 

``` powershell
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [<CommonParameters>] 
```


Of; 

``` powershell
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN <String> [<CommonParameters>] 
```

Deze cmdlet wordt de volgende machtigingen ingesteld: 

|Type |Naam |Access |Van toepassing op|
|-----|-----|-----|-----| 
|Toestaan |AD DS-Connector-Account |Directorywijzigingen repliceren |Alleen dit object (hoofddomein)| 
|Toestaan |AD DS-Connector-Account |Alle Active Directory repliceren gewijzigd |Alleen dit object (hoofddomein)| 
  
### <a name="permissions-for-password-writeback"></a>Machtigingen voor het terugschrijven van wachtwoorden 
Om machtigingen voor het AD DS-Connector-account wanneer u het terugschrijven van wachtwoorden, voert u de volgende uit: 

``` powershell
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


Of;

``` powershell
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```
Deze cmdlet wordt de volgende machtigingen ingesteld: 

|Type |Naam |Access |Van toepassing op|
|-----|-----|-----|-----| 
|Toestaan |AD DS-Connector-Account |Wachtwoord opnieuw instellen |Onderliggende gebruikersobjecten| 
|Toestaan |AD DS-Connector-Account |De eigenschap lockoutTime schrijven |Onderliggende gebruikersobjecten| 
|Toestaan |AD DS-Connector-Account |De eigenschap pwdLastSet schrijven |Onderliggende gebruikersobjecten| 

### <a name="permissions-for-group-writeback"></a>Machtigingen voor write-back van groep 
Voer het volgende als u wilt instellen van machtigingen voor de AD DS-Connector-account bij het gebruik van Write-back van groep: 

``` powershell
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```
Of; 

``` powershell
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>]
```
 
Deze cmdlet wordt de volgende machtigingen ingesteld: 

|Type |Naam |Access |Van toepassing op|
|-----|-----|-----|-----| 
|Toestaan |AD DS-Connector-Account |Algemene lezen/schrijven |Onderliggende groep objecten worden weergegeven| 
|Toestaan |AD DS-Connector-Account |Onderliggend object maken/verwijderen |Dit object en alle onderliggende objecten| 
|Toestaan |AD DS-Connector-Account |Een object verwijderen/verwijderen en alle van de onderliggende items |Dit object en alle onderliggende objecten|

### <a name="permissions-for-exchange-hybrid-deployment"></a>Machtigingen voor hybride implementatie voor Exchange 
Om machtigingen voor het AD DS-Connector-account bij het gebruik van hybride implementatie voor Exchange, voert u de volgende uit: 

``` powershell
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


Of; 

``` powershell
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```

Deze cmdlet wordt de volgende machtigingen ingesteld:  
 

|Type |Naam |Access |Van toepassing op|
|-----|-----|-----|-----| 
|Toestaan |AD DS-Connector-Account |Lezen/schrijven alle eigenschappen |Onderliggende gebruikersobjecten| 
|Toestaan |AD DS-Connector-Account |Lezen/schrijven alle eigenschappen |Onderliggende InetOrgPerson-objecten| 
|Toestaan |AD DS-Connector-Account |Lezen/schrijven alle eigenschappen |Onderliggende groep objecten worden weergegeven| 
|Toestaan |AD DS-Connector-Account |Lezen/schrijven alle eigenschappen |Onderliggende objecten van de contactpersoon| 

### <a name="permissions-for-exchange-mail-public-folders-preview"></a>Machtigingen voor Exchange Mail openbare mappen (Preview) 
Om machtigingen voor het AD DS-Connector-account als u de functie openbare e-mailmappen Exchange gebruikt, voert u de volgende uit: 

``` powershell
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


Of; 

``` powershell
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```
Deze cmdlet wordt de volgende machtigingen ingesteld: 

|Type |Naam |Access |Van toepassing op|
|-----|-----|-----|-----| 
|Toestaan |AD DS-Connector-Account |Alle eigenschappen lezen |Onderliggende PublicFolder objecten| 

### <a name="restrict-permissions-on-the-ad-ds-connector-account"></a>Machtigingen voor de AD DS-Connector beperken Account 
Dit PowerShell-script worden machtigingen voor het AD-Connector-Account dat is opgegeven als parameter webserverbeheerders. Machtigingen verstrakking omvat de volgende stappen: 

- Overname in het opgegeven object uitschakelen 
- Alle ACE's op het specifieke object, met uitzondering van ACE's die specifiek zijn voor zelf worden verwijderd omdat we de standaardmachtigingen behouden willen als het gaat om zelf. 
 
 De parameter - ADConnectorAccountDN is het AD-serviceaccount waarvan de machtigingen wilt hoger worden ingesteld. Dit is doorgaans het domeinaccount MSOL_nnnnnnnnnnnn die is geconfigureerd in de AD DS-Connector (Zie bepalen de Connector-Account van uw AD DS). De - referentie parameter is nodig om op te geven van de Administrator-account waarvoor de vereiste bevoegdheden beschikt om te beperken van machtigingen voor Active Directory van het doelobject AD. Dit is meestal de onderneming of een domeinbeheerder.  

``` powershell
Set-ADSyncRestrictedPermissions [-ADConnectorAccountDN] <String> [-Credential] <PSCredential> [-DisableCredentialValidation] [-WhatIf] [-Confirm] [<CommonParameters>] 
```
 
Bijvoorbeeld: 

``` powershell
$credential = Get-Credential 
Set-ADSyncRestrictedPermissions -ObjectDN 'CN=ADConnectorAccount,CN=Users,DC=Contoso,DC=com' -Credential $credential  
```

Deze cmdlet wordt de volgende machtigingen ingesteld: 

|Type |Naam |Access |Van toepassing op|
|-----|-----|-----|-----| 
|Toestaan |SYSTEEM |Volledig beheer |Dit object 
|Toestaan |Ondernemingsadministrators |Volledig beheer |Dit object 
|Toestaan |Domeinadministrators |Volledig beheer |Dit object 
|Toestaan |Beheerders |Volledig beheer |Dit object 
|Toestaan |Enterprise-domeincontrollers |Inhoud weergeven |Dit object 
|Toestaan |Enterprise-domeincontrollers |Alle eigenschappen lezen |Dit object 
|Toestaan |Enterprise-domeincontrollers |Machtigingen lezen |Dit object 
|Toestaan |Geverifieerde gebruikers |Inhoud weergeven |Dit object 
|Toestaan |Geverifieerde gebruikers |Alle eigenschappen lezen |Dit object 
|Toestaan |Geverifieerde gebruikers |Machtigingen lezen |Dit object 

## <a name="next-steps"></a>Volgende stappen
- [Azure AD Connect: accounts en machtigingen](reference-connect-accounts-permissions.md)
- [Snelle installatie](how-to-connect-install-express.md)
- [Aangepaste installatie](how-to-connect-install-custom.md)
- [ADSyncConfig verwijzing](reference-connect-adsyncconfig.md)

