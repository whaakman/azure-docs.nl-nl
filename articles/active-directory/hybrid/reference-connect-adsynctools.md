---
title: 'Azure AD Connect: ADSyncTools PowerShell Reference | Microsoft Docs'
description: Dit document bevat referentie-informatie voor de ADSyncTools.psm1 PowerShell-module.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 10/19/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: ceb00572d352954eea67d2b5a2a35bab996c3d7e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203875"
---
# <a name="azure-ad-connect--adsynctools-powershell-reference"></a>Azure AD Connect:  ADSyncTools PowerShell Reference
De volgende documentatie bevat referentie-informatie voor de ADSyncTools.psm1 PowerShell-Module die is opgenomen in Azure AD Connect.

## <a name="clear-adsynctoolsconsistencyguid"></a>Clear-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>SAMENVATTING
Schakel de mS-Ds-ConsistencyGuid van AD-gebruiker

### <a name="syntax"></a>SYNTAXIS

```
Clear-ADSyncToolsConsistencyGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Wis de waarde in mS-Ds-ConsistencyGuid voor de-AD-doelgebruiker

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-user"></a>-User
Doelgebruiker in AD instellen

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="confirm-adsynctoolsadmoduleloaded"></a>Confirm-ADSyncToolsADModuleLoaded

### <a name="synopsis"></a>SAMENVATTING
{{Vult u de samenvatting}}

### <a name="syntax"></a>SYNTAXIS

```
Confirm-ADSyncToolsADModuleLoaded
```

### <a name="description"></a>BESCHRIJVING
{{Vullen in de beschrijving}}

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>Voorbeeld 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Toevoegen voorbeeld beschrijving hier}}

## <a name="connect-adsyncdatabase"></a>Connect-AdSyncDatabase

### <a name="synopsis"></a>SAMENVATTING
{{Vult u de samenvatting}}

### <a name="syntax"></a>SYNTAXIS

```
Connect-AdSyncDatabase [-Server] <String> [[-Instance] <String>] [[-Database] <String>] [[-UserName] <String>]
 [[-Password] <String>] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
{{Vullen in de beschrijving}}

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>Voorbeeld 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Toevoegen voorbeeld beschrijving hier}}

### <a name="parameters"></a>PARAMETERS

#### <a name="-database"></a>-Database
{{Beschrijving van de Database vullen}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-instance"></a>-Exemplaar
{{Vullen exemplaar beschrijving}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-password"></a>-Password
{{Invullen van wachtwoorden}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-server"></a>-Server
{{Beschrijving van de Server vullen}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-username"></a>-UserName
{{Fill UserName Description}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="export-adsynctoolsconsistencyguidmigration"></a>Export-ADSyncToolsConsistencyGuidMigration

### <a name="synopsis"></a>SAMENVATTING
ConsistencyGuid rapport exporteren

### <a name="syntax"></a>SYNTAXIS

```
Export-ADSyncToolsConsistencyGuidMigration [-AlternativeLoginId] [-UserPrincipalName] <String>
 [-ImmutableIdGUID] <String> [-Output] <String> [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Genereert een ConsistencyGuid rapport op basis van een CSV-bestand importeren van de Import-ADSyncToolsImmutableIdMigration

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Import-Csv .\AllSyncUsers.csv | Export-ADSyncToolsConsistencyGuidMigration -Output ".\AllSyncUsers-Report"
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-alternativeloginid"></a>-AlternativeLoginId
Gebruik alternatieve aanmeldings-ID (e-mail)

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-userprincipalname"></a>-UserPrincipalName
UserPrincipalName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-immutableidguid"></a>-ImmutableIdGUID
ImmutableIdGUID

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-output"></a>-Uitvoer
Uitvoerbestand voor CSV- en LOGBOEKBESTANDEN

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsyncsqlbrowserinstances"></a>Get-ADSyncSQLBrowserInstances

### <a name="synopsis"></a>SAMENVATTING
{{Vult u de samenvatting}}

### <a name="syntax"></a>SYNTAXIS

```
Get-ADSyncSQLBrowserInstances [[-hostName] <String>]
```

### <a name="description"></a>BESCHRIJVING
{{Vullen in de beschrijving}}

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>Voorbeeld 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Toevoegen voorbeeld beschrijving hier}}

### <a name="parameters"></a>PARAMETERS

#### <a name="-hostname"></a>-hostName
{{Vullen hostnaam beschrijving}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="get-adsynctoolsaduser"></a>Get-ADSyncToolsADuser

### <a name="synopsis"></a>SAMENVATTING
Gebruiker ophalen uit Active Directory

### <a name="syntax"></a>SYNTAXIS

```
Get-ADSyncToolsADuser [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Retourneert een AD-object te doen: Ondersteuning voor meerdere forests

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-user"></a>-User
Doelgebruiker in AD ConsistencyGuid instellen

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolsconsistencyguid"></a>Get-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>SAMENVATTING
MS-Ds-ConsistencyGuid van AD-gebruiker ophalen

### <a name="syntax"></a>SYNTAXIS

```
Get-ADSyncToolsConsistencyGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Retourneert de waarde in mS-Ds-ConsistencyGuid kenmerk van de-AD-doelgebruiker in GUID-indeling

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-user"></a>-User
Doelgebruiker in AD instellen

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolsobjectguid"></a>Get-ADSyncToolsObjectGuid

### <a name="synopsis"></a>SAMENVATTING
De ObjectGuid van AD-gebruiker ophalen

### <a name="syntax"></a>SYNTAXIS

```
Get-ADSyncToolsObjectGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Retourneert de waarde in het kenmerk ObjectGUID van de-AD-doelgebruiker in GUID-indeling

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-user"></a>-User
Doelgebruiker in AD instellen

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolsrunhistory"></a>Get-ADSyncToolsRunHistory

### <a name="synopsis"></a>SAMENVATTING
Get AAD Connect Uitvoeringsgeschiedenis

### <a name="syntax"></a>SYNTAXIS

```
Get-ADSyncToolsRunHistory [[-Days] <Int32>] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Functie die resulteert in de geschiedenis uitvoeren van AAD Connect in XML-indeling

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Get-ADSyncToolsRunHistory
```

#### <a name="example-2"></a>VOORBEELD 2
```
Get-ADSyncToolsRunHistory -Days 1
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-days"></a>-Dagen
{{Dagen doorvoeren beschrijving}}

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: 3
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="get-adsynctoolssourceanchorchanged"></a>Get-ADSyncToolsSourceAnchorChanged

### <a name="synopsis"></a>SAMENVATTING
Aan de gebruikers met fouten SourceAnchor gewijzigd

### <a name="syntax"></a>SYNTAXIS

```
Get-ADSyncToolsSourceAnchorChanged [-sourcePath] <Object> [-outputPath] <Object> [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Query's AAD Connect Run History functioneren en uitvoer van alle gebruikers die de fout te melden: "SourceAnchor-kenmerk is gewijzigd."

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
#Required Parameters
```

$sourcePath = Read-Host - Prompt 'Voer uw pad naar logboekbestand met de bestandsnaam van het ' # '\<Source_Path\>"$outputPath = Read-Host-vragen ' Voer uw out bestandspad met bestandsnaam ' # '\<Out_Path\>"
 
 Get-ADSyncToolsUsersSourceAnchorChanged -sourcePath $sourcePath -outputPath $outputPath

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-sourcepath"></a>-sourcePath
{{Vullen bronpad beschrijving}}

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-outputpath"></a>-outputPath
{{OutputPath beschrijving vullen}}

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="import-adsynctoolsimmutableidmigration"></a>Import-ADSyncToolsImmutableIdMigration

### <a name="synopsis"></a>SAMENVATTING
ImmutableID importeren uit AAD

### <a name="syntax"></a>SYNTAXIS

```
Import-ADSyncToolsImmutableIdMigration [-Output] <String> [-IncludeSyncUsersFromRecycleBin]
 [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Hiermee genereert u een bestand met alle Azure AD-gesynchroniseerde gebruikers met de waarde ImmutableID in GUID-indeling vereisten: MSOnline PowerShell-Module

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Import-ADSyncToolsImmutableIdMigration -OutputFile '.\AllSyncUsers.csv'
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-output"></a>-Uitvoer
CSV-bestand voor uitvoer

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-includesyncusersfromrecyclebin"></a>-IncludeSyncUsersFromRecycleBin
Gesynchroniseerd gebruikers van Azure AD-Prullenbak

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).


## <a name="invoke-adsyncdatabasequery"></a>Invoke-AdSyncDatabaseQuery

### <a name="synopsis"></a>SAMENVATTING
{{Vult u de samenvatting}}

### <a name="syntax"></a>SYNTAXIS

```
Invoke-AdSyncDatabaseQuery [-SqlConnection] <SqlConnection> [[-Query] <String>] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
{{Vullen in de beschrijving}}

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>Voorbeeld 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Toevoegen voorbeeld beschrijving hier}}

### <a name="parameters"></a>PARAMETERS

#### <a name="-query"></a>-Query
{{Querybeschrijving vullen}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-sqlconnection"></a>-SqlConnection
{{Vullen SqlConnection beschrijving}}

```yaml
Type: SqlConnection
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="remove-adsynctoolsexpiredcertificates"></a>Remove-ADSyncToolsExpiredCertificates

### <a name="synopsis"></a>SAMENVATTING
Script voor het kenmerk UserCertificate van verlopen certificaten verwijderen

### <a name="syntax"></a>SYNTAXIS

```
Remove-ADSyncToolsExpiredCertificates [-TargetOU] <String> [[-BackupOnly] <Boolean>] [-ObjectClass] <String>
 [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Met dit script neemt alle objecten van een doel voor organisatie-eenheid in uw Active Directory-domein - gefilterd op objectklasse (gebruiker/Computer) en Hiermee verwijdert u alle verlopen certificaten aanwezig zijn in het kenmerk UserCertificate.
Standaard (BackupOnly modus), die deze wordt alleen back-up van verlopen certificaten naar een bestand en niet alle wijzigingen in AD.
Als u gebruik - BackupOnly $false en vervolgens een certificaat verlopen aanwezig zijn in kenmerk UserCertificate voor deze objecten wordt verwijderd uit AD na wordt gekopieerd naar bestand.
Elk certificaat worden back-ups voor een gescheiden filename: Het script ObjectClass_ObjectGUID_CertThumprint.cer maakt ook een logboekbestand in CSV-indeling waarin alle gebruikers met certificaten die zijn ongeldig of verlopen met inbegrip van de werkelijke uitgevoerde actie (overgeslagen/geëxporteerde/verwijderd).

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Check all users in target OU - Expired Certificates will be copied to separated files and no certificates will be removed
```

Remove-ADSyncToolsExpiredCertificates -TargetOU "OU=Users,OU=Corp,DC=Contoso,DC=com" -ObjectClass user

#### <a name="example-2"></a>VOORBEELD 2
```
Delete Expired Certs from all Computer objects in target OU - Expired Certificates will be copied to files and removed from AD
```

Remove-ADSyncToolsExpiredCertificates -TargetOU "OU=Computers,OU=Corp,DC=Contoso,DC=com" -ObjectClass computer -BackupOnly $false

### <a name="parameters"></a>PARAMETERS

#### <a name="-targetou"></a>-TargetOU
Doel organisatie-eenheid lookup voor AD-objecten

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-backuponly"></a>-BackupOnly
BackupOnly worden niet alle certificaten verwijderd uit Active Directory

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: True
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-objectclass"></a>-ObjectClass
Filter voor object-klasse

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="repair-adsynctoolsautoupgradestate"></a>Repair-ADSyncToolsAutoUpgradeState

### <a name="synopsis"></a>SAMENVATTING
Korte beschrijving

### <a name="syntax"></a>SYNTAXIS

```
Repair-ADSyncToolsAutoUpgradeState
```

### <a name="description"></a>BESCHRIJVING
Lange beschrijving

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

## <a name="resolve-adsynchostaddress"></a>Resolve-ADSyncHostAddress

### <a name="synopsis"></a>SAMENVATTING
{{Vult u de samenvatting}}

### <a name="syntax"></a>SYNTAXIS

```
Resolve-ADSyncHostAddress [[-hostName] <String>]
```

### <a name="description"></a>BESCHRIJVING
{{Vullen in de beschrijving}}

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>Voorbeeld 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Toevoegen voorbeeld beschrijving hier}}

### <a name="parameters"></a>PARAMETERS

#### <a name="-hostname"></a>-hostName
{{Vullen hostnaam beschrijving}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="restore-adsynctoolsexpiredcertificates"></a>Restore-ADSyncToolsExpiredCertificates

### <a name="synopsis"></a>SAMENVATTING
(MOET IK DOEN) Kenmerk AD UserCertificate herstelt vanuit een bestand certicate

### <a name="syntax"></a>SYNTAXIS

```
Restore-ADSyncToolsExpiredCertificates
```

### <a name="description"></a>BESCHRIJVING
Lange beschrijving

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

## <a name="set-adsynctoolsconsistencyguid"></a>Set-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>SAMENVATTING
MS-Ds-ConsistencyGuid op AD-gebruiker instellen

### <a name="syntax"></a>SYNTAXIS

```
Set-ADSyncToolsConsistencyGuid [-User] <Object> [-Value] <Object> [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Een waarde in mS-Ds-ConsistencyGuid-kenmerk voor de doel-AD-gebruiker instellen

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-user"></a>-User
Doelgebruiker in AD ConsistencyGuid instellen

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-value"></a>-Waarde
ImmutableId (bytematrix, GUID, GUID-tekenreeks of Base64-tekenreeks)

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="test-adsyncnetworkport"></a>Test-ADSyncNetworkPort

### <a name="synopsis"></a>SAMENVATTING
{{Vult u de samenvatting}}

### <a name="syntax"></a>SYNTAXIS

```
Test-ADSyncNetworkPort [[-hostName] <String>] [[-port] <String>]
```

### <a name="description"></a>BESCHRIJVING
{{Vullen in de beschrijving}}

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>Voorbeeld 1
```powershell
PS C:\> {{ Add example code here }}
```

{{Toevoegen voorbeeld beschrijving hier}}

### <a name="parameters"></a>PARAMETERS

#### <a name="-hostname"></a>-hostName
{{Vullen hostnaam beschrijving}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-port"></a>-port
{{Vullen poort beschrijving}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="trace-adsynctoolsadimport"></a>Trace-ADSyncToolsADImport

### <a name="synopsis"></a>SAMENVATTING
Maakt een traceringsbestand van en de stap AD importeren

### <a name="syntax"></a>SYNTAXIS

```
Trace-ADSyncToolsADImport [[-ADConnectorXML] <String>] [[-dc] <String>] [[-rootDN] <String>]
 [[-filter] <String>] [-SkipCredentials] [[-ADwatermark] <String>] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Traceringen alle ldap-query's van een AAD Connect AD Import uitvoeren vanaf een controlepunt van een bepaalde AD watermerk (cookie partitie). Maakt een traceringsbestand '.\ADimportTrace_yyyyMMddHHmmss.log' op de huidige map.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectorxml"></a>-ADConnectorXML
{{Fill ADConnectorXML Description}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dc"></a>-dc
XML-bestand van het exporteren van de AD-Connector

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: DC1.domain.contoso.com
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-rootdn"></a>-rootDN
Doel-domeincontroller

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: DC=Domain,DC=Contoso,DC=com
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-filter"></a>-filter
Forest Root DN

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: (&(objectClass=*))
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipcredentials"></a>-SkipCredentials
Typen AD-objecten te traceren \> * = alle objecttypen

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adwatermark"></a>-ADwatermark
Als al wordt uitgevoerd als domeinbeheerder is er niet nodig voor AD-referenties.
Handmatige invoer van het watermerk, in plaats van XML-bestand bijvoorbeeld $ADwatermark = "TVNEUwMAAAAXyK9ir1zSAQAAAAAAAAAA(...)"

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 5
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="trace-adsynctoolsldapquery"></a>Trace-ADSyncToolsLdapQuery

### <a name="synopsis"></a>SAMENVATTING
Korte beschrijving

### <a name="syntax"></a>SYNTAXIS

```
Trace-ADSyncToolsLdapQuery [-Context] <String> [-Server] <String> [-Port] <Int32> [-Filter] <String>
 [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Lange beschrijving

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>VOORBEELD 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-context"></a>-Context
Beschrijving van param1 help

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-server"></a>-Server
Beschrijving van param2 help

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: Localhost
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-port"></a>-Port
Beschrijving van param2 help

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: 389
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-filter"></a>-Filter
Beschrijving van param2 help

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 4
Default value: (objectClass=*)
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="update-adsynctoolsconsistencyguidmigration"></a>Update-ADSyncToolsConsistencyGuidMigration

### <a name="synopsis"></a>SAMENVATTING
Gebruikers wordt bijgewerkt met de nieuwe ConsistencyGuid (ImmutableId)

### <a name="syntax"></a>SYNTAXIS

```
Update-ADSyncToolsConsistencyGuidMigration [[-DistinguishedName] <String>] [-ImmutableIdGUID] <String>
 [-Action] <String> [-Output] <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Gebruikers met de nieuwe ConsistencyGuid (ImmutableId)-waarde die is overgenomen uit de ConsistencyGuid rapport deze functie ondersteunt de switch WhatIf Opmerking updates: ConsistencyGuid rapport moet worden geïmporteerd met tabblad scheidingsteken

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Import-Csv .\AllSyncUsersTEST-Report.csv -Delimiter "`t"| Update-ADSyncToolsConsistencyGuidMigration -Output .\AllSyncUsersTEST-Result2 -WhatIf
```

#### <a name="example-2"></a>VOORBEELD 2
```
Import-Csv .\AllSyncUsersTEST-Report.csv -Delimiter "`t"| Update-ADSyncToolsConsistencyGuidMigration -Output .\AllSyncUsersTEST-Result2
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-distinguishedname"></a>-DistinguishedName
DistinguishedName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: False
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-immutableidguid"></a>-ImmutableIdGUID
ImmutableIdGUID

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-action"></a>-Actie
Bewerking

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-output"></a>-Uitvoer
Uitvoerbestand voor de logboekbestanden

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Geeft aan wat er gebeuren zou als de cmdlet wordt uitgevoerd.
De cmdlet wordt niet uitgevoerd.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Controleer of
Vraagt u om bevestiging voordat u de cmdlet uitvoert.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).
