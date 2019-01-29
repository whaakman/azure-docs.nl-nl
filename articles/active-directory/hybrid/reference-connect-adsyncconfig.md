---
title: 'Azure AD Connect: ADSyncConfig PowerShell Reference | Microsoft Docs'
description: Dit document bevat referentie-informatie voor de ADSyncConfig.psm1 PowerShell-module.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 01/24/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 19b7cebd1c75e1a2bbfe34e3ab4e657051e8a6d4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159513"
---
# <a name="azure-ad-connect--adsyncconfig-powershell-reference"></a>Azure AD Connect:  ADSyncConfig PowerShell Reference
De volgende documentatie bevat referentie-informatie voor de ADSyncConfig.psm1 PowerShell-Module die is opgenomen in Azure AD Connect.


## <a name="get-adsyncadconnectoraccount"></a>Get-ADSyncADConnectorAccount

### <a name="synopsis"></a>SAMENVATTING
Haalt de accountnaam en het domein dat is geconfigureerd in elk AD-Connector

### <a name="syntax"></a>SYNTAXIS

```
Get-ADSyncADConnectorAccount
```

### <a name="description"></a>BESCHRIJVING
Deze functie maakt gebruik van de cmdlet 'Get-ADSyncConnector' dat is opgenomen in het AAD Connect om op te halen van connectiviteit Parameters van een tabel met de AD connector (s)-account.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Get-ADSyncADConnectorAccount
```

## <a name="get-adsyncobjectswithinheritancedisabled"></a>Get-ADSyncObjectsWithInheritanceDisabled

### <a name="synopsis"></a>SAMENVATTING
AD-objecten opgehaald met de overname van machtigingen is uitgeschakeld

### <a name="syntax"></a>SYNTAXIS

```
Get-ADSyncObjectsWithInheritanceDisabled [-SearchBase] <String> [[-ObjectClass] <String>] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Zoekt in de parameter SearchBase vanaf AD en retourneert alle objecten, gefilterd op de objectklasse-parameter met de overname van ACL op dit moment uitgeschakeld.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Find objects with disabled inheritance in 'Contoso' domain (by default returns 'organizationalUnit' objects only)
```

Get-ADSyncObjectsWithInheritanceDisabled -SearchBase 'Contoso'

#### <a name="example-2"></a>VOORBEELD 2
```
Find 'user' objects with disabled inheritance in 'Contoso' domain
```

Get-ADSyncObjectsWithInheritanceDisabled -SearchBase 'Contoso' -ObjectClass 'user'

#### <a name="example-3"></a>VOORBEELD 3
```
Find all types of objects with disabled inheritance in a OU
```

Get-ADSyncObjectsWithInheritanceDisabled -SearchBase OU=AzureAD,DC=Contoso,DC=com -ObjectClass '*'

### <a name="parameters"></a>PARAMETERS

#### <a name="-searchbase"></a>-SearchBase
De SearchBase voor de LDAP-query die een DistinguishedName AD-domein of een FQDN-naam worden kan

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

#### <a name="-objectclass"></a>-ObjectClass
De klasse van de objecten te zoeken die kan worden ' *' (voor elke objectklasse), 'user', 'groep', 'container', enzovoort. Deze functie wordt standaard zoeken voor de objectklasse 'organizationalUnit'.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: OrganizationalUnit
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).

## <a name="set-adsyncbasicreadpermissions"></a>Set-ADSyncBasicReadPermissions

### <a name="synopsis"></a>SAMENVATTING
Uw Active Directory-forest en domein voor eenvoudige lees-en schrijfmachtigingen worden geïnitialiseerd.

### <a name="syntax"></a>SYNTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [-SkipAdminSdHolders]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
De functie Set ADSyncBasicReadPermissions krijgt de vereiste machtigingen voor de AD-synchronisatie-account, waaronder de volgende:
1.
Toegang van de eigenschap lezen op alle kenmerken voor alle onderliggende computerobjecten
2.
Toegang van de eigenschap lezen op alle kenmerken voor alle onderliggende apparaatobjecten
3.
Eigenschap lezen-toegang op alle kenmerken voor alle onderliggende foreignsecurityprincipal objecten
5.
Toegang van de eigenschap lezen op alle kenmerken voor alle onderliggende objecten
6.
Toegang van de eigenschap lezen op alle kenmerken voor alle onderliggende inetorgperson-objecten
7.
Toegang van de eigenschap lezen op alle kenmerken voor alle objecten van de onderliggende groep
8.
Eigenschap lezen-toegang op alle kenmerken voor alle descendantobjecten contact op met

Deze machtigingen worden toegepast op alle domeinen in het forest.
U kunt eventueel een DistinguishedName in ADobjectDN parameter voor het instellen van deze machtigingen voor die AD-Object alleen (inclusief overname sub-objecten) opgeven.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>VOORBEELD 2
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>VOORBEELD 3
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>VOORBEELD 4
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
De naam van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Het domein van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
De DistinguishedName van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName van het doelobject AD machtigingen in te stellen (optioneel)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Optionele parameter om aan te geven als AdminSDHolder container niet met deze machtigingen moet worden bijgewerkt

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

## <a name="set-adsyncexchangehybridpermissions"></a>Set-ADSyncExchangeHybridPermissions

### <a name="synopsis"></a>SAMENVATTING
Uw Active Directory-forest en domein voor de functie Hybride Exchange worden geïnitialiseerd.

### <a name="syntax"></a>SYNTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [-SkipAdminSdHolders]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
De functie Set ADSyncExchangeHybridPermissions krijgt de vereiste machtigingen voor de AD-synchronisatie-account, waaronder de volgende:
1.
Eigenschap voor lezen/schrijven toegang op alle kenmerken voor alle onderliggende objecten
2.
Toegang voor lezen/schrijven-eigenschap op alle kenmerken voor alle onderliggende inetorgperson-objecten
3.
Toegang voor lezen/schrijven-eigenschap op alle kenmerken voor alle objecten van de onderliggende groep
4.
Eigenschap voor lezen/schrijven toegang op alle kenmerken voor alle onderliggende contact op met objecten

Deze machtigingen worden toegepast op alle domeinen in het forest.
U kunt eventueel een DistinguishedName in ADobjectDN parameter voor het instellen van deze machtigingen voor die AD-Object alleen (inclusief overname sub-objecten) opgeven.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>VOORBEELD 2
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>VOORBEELD 3
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>VOORBEELD 4
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
De naam van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Het domein van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
De DistinguishedName van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName van het doelobject AD machtigingen in te stellen (optioneel)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Optionele parameter om aan te geven als AdminSDHolder container niet met deze machtigingen moet worden bijgewerkt

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

## <a name="set-adsyncexchangemailpublicfolderpermissions"></a>Set-ADSyncExchangeMailPublicFolderPermissions

### <a name="synopsis"></a>SAMENVATTING
Uw Active Directory-forest en domein voor openbare map Exchange Mail-functie worden geïnitialiseerd.

### <a name="syntax"></a>SYNTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName <String>
 -ADConnectorAccountDomain <String> [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
De functie Set ADSyncExchangeMailPublicFolderPermissions krijgt de vereiste machtigingen voor de AD-synchronisatie-account, waaronder de volgende:
1.
Eigenschap lezen-toegang op alle kenmerken voor alle onderliggende publicfolder objecten

Deze machtigingen worden toegepast op alle domeinen in het forest.
U kunt eventueel een DistinguishedName in ADobjectDN parameter voor het instellen van deze machtigingen voor die AD-Object alleen (inclusief overname sub-objecten) opgeven.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>VOORBEELD 2
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>VOORBEELD 3
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>VOORBEELD 4
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
De naam van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Het domein van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
De DistinguishedName van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName van het doelobject AD machtigingen in te stellen (optioneel)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Optionele parameter om aan te geven als AdminSDHolder container niet met deze machtigingen moet worden bijgewerkt

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

## <a name="set-adsyncmsdsconsistencyguidpermissions"></a>Set-ADSyncMsDsConsistencyGuidPermissions

### <a name="synopsis"></a>SAMENVATTING
Uw Active Directory-forest en domein voor mS-DS-ConsistencyGuid functie initialiseren.

### <a name="syntax"></a>SYNTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
De functie Set ADSyncMsDsConsistencyGuidPermissions krijgt de vereiste machtigingen voor de AD-synchronisatie-account, waaronder de volgende:
1.
Eigenschap voor lezen/schrijven toegang op mS-DS-ConsistencyGuid kenmerk voor alle onderliggende objecten

Deze machtigingen worden toegepast op alle domeinen in het forest.
U kunt eventueel een DistinguishedName in ADobjectDN parameter voor het instellen van deze machtigingen voor die AD-Object alleen (inclusief overname sub-objecten) opgeven.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>VOORBEELD 2
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>VOORBEELD 3
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>VOORBEELD 4
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
De naam van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Het domein van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
De DistinguishedName van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName van het doelobject AD machtigingen in te stellen (optioneel)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Optionele parameter om aan te geven als AdminSDHolder container niet met deze machtigingen moet worden bijgewerkt

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

## <a name="set-adsyncpasswordhashsyncpermissions"></a>Set-ADSyncPasswordHashSyncPermissions

### <a name="synopsis"></a>SAMENVATTING
Uw Active Directory-forest en domein voor de wachtwoord-hashsynchronisatie worden geïnitialiseerd.

### <a name="syntax"></a>SYNTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
De functie Set ADSyncPasswordHashSyncPermissions krijgt de vereiste machtigingen voor de AD-synchronisatie-account, waaronder de volgende:
1.
Directorywijzigingen repliceren
2.
Alle Active Directory repliceren gewijzigd

Deze machtigingen zijn opgegeven voor alle domeinen in het forest.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>VOORBEELD 2
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
De naam van het Active Directory-account dat wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Het domein van het Active Directory-account dat wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
De DistinguishedName van het Active Directory-account dat wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
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

## <a name="set-adsyncpasswordwritebackpermissions"></a>Set-ADSyncPasswordWritebackPermissions

### <a name="synopsis"></a>SAMENVATTING
Uw Active Directory-forest en domein voor het terugschrijven van Azure AD worden geïnitialiseerd.

### <a name="syntax"></a>SYNTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
De functie Set ADSyncPasswordWritebackPermissions krijgt de vereiste machtigingen voor de AD-synchronisatie-account, waaronder de volgende:
1.
Wachtwoord opnieuw instellen van onderliggende gebruikersobjecten
2.
De eigenschap schrijftoegang op lockoutTime kenmerk voor alle onderliggende objecten
3.
De eigenschap schrijftoegang op pwdLastSet kenmerk voor alle onderliggende objecten

Deze machtigingen worden toegepast op alle domeinen in het forest.
U kunt eventueel een DistinguishedName in ADobjectDN parameter voor het instellen van deze machtigingen voor die AD-Object alleen (inclusief overname sub-objecten) opgeven.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>VOORBEELD 2
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>VOORBEELD 3
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>VOORBEELD 4
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
De naam van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Het domein van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
De DistinguishedName van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName van het doelobject AD machtigingen in te stellen (optioneel)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Optionele parameter om aan te geven als AdminSDHolder container niet met deze machtigingen moet worden bijgewerkt

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

## <a name="set-adsyncrestrictedpermissions"></a>Set-ADSyncRestrictedPermissions

### <a name="synopsis"></a>SAMENVATTING
De machtigingen van een AD-object dat niet anders is opgenomen in een beveiligde AD-beveiligingsgroep uitbreiden.
Een typisch voorbeeld is de AD Connect-account (MSOL) automatisch gemaakt door AAD Connect.
Dit account heeft repliceren machtigingen voor alle domeinen, maar kan worden tamelijk eenvoudig als het niet is beveiligd.

### <a name="syntax"></a>SYNTAXIS

```
Set-ADSyncRestrictedPermissions [-ADConnectorAccountDN] <String> [-Credential] <PSCredential>
 [-DisableCredentialValidation] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
De functie Set ADSyncRestrictedPermissions wordt webserverbeheerders oo machtigingen voor het account dat is opgegeven.
Machtigingen verstrakking omvat de volgende stappen:
1.
Overname in het opgegeven object uitschakelen
2.
Verwijder alle ACE's op het specifieke object, met uitzondering van ACE's die specifiek zijn voor zelf.
We willen de standaardmachtigingen behouden als het gaat om zelf.
3.
Deze specifieke machtigingen toewijzen:

        Type    Name                                        Access              Applies To
        =============================================================================================
        Allow   SYSTEM                                      Full Control        This object
        Allow   Enterprise Admins                           Full Control        This object
        Allow   Domain Admins                               Full Control        This object
        Allow   Administrators                              Full Control        This object

        Allow   Enterprise Domain Controllers               List Contents
                                                            Read All Properties
                                                            Read Permissions    This object

        Allow   Authenticated Users                         List Contents
                                                            Read All Properties
                                                            Read Permissions    This object

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Set-ADSyncRestrictedPermissions -ADConnectorAccountDN "CN=TestAccount1,CN=Users,DC=Contoso,DC=com" -Credential $(Get-Credential)
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
DistinguishedName van het Active Directory-account waarvan de machtigingen wilt hoger worden ingesteld.
Dit is meestal de MSOL_nnnnnnnnnn of een aangepast domein-account dat is geconfigureerd in uw AD-Connector.

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

#### <a name="-credential"></a>-Credential
Beheerdersreferentie waarvoor de vereiste bevoegdheden beschikt om de machtigingen voor het account ADConnectorAccountDN te beperken. Dit is meestal de beheerder van de Enterprise- of -domein. Gebruik de volledig gekwalificeerde domeinnaam van het administrator-account om te voorkomen dat account opzoeken fouten.
Voorbeeld: CONTOSO\admin

```yaml
Type: PSCredential
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-disablecredentialvalidation"></a>-DisableCredentialValidation
Wanneer DisableCredentialValidation wordt gebruikt, wordt de functie wordt niet gecontroleerd als de referenties die zijn opgegeven in - referenties zijn geldig in AD en of het opgegeven account heeft de machtigingen die nodig zijn om de machtigingen voor het account ADConnectorAccountDN te beperken.

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

## <a name="set-adsyncunifiedgroupwritebackpermissions"></a>Set-ADSyncUnifiedGroupWritebackPermissions

### <a name="synopsis"></a>SAMENVATTING
Uw Active Directory-forest en domein voor write-back van groep van Azure AD worden geïnitialiseerd.

### <a name="syntax"></a>SYNTAXIS

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
De functie Set ADSyncUnifiedGroupWritebackPermissions krijgt de vereiste machtigingen voor de AD-synchronisatie-account, waaronder de volgende:
1.
Algemene lezen/schrijven, verwijderen, structuur verwijderen en Create\Delete onderliggende voor alle objecttypen en subobjecten groeperen

Deze machtigingen worden toegepast op alle domeinen in het forest.
U kunt eventueel een DistinguishedName in ADobjectDN parameter voor het instellen van deze machtigingen voor die AD-Object alleen (inclusief overname sub-objecten) opgeven.
In dit geval worden ADobjectDN de DN-naam van de Container die u wenst te koppelen met de functie GroupWriteback.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>VOORBEELD 2
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>VOORBEELD 3
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>VOORBEELD 4
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
De naam van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Het domein van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
De DistinguishedName van het Active Directory-account dat is of wordt gebruikt door Azure AD Connect Sync om objecten in de directory te beheren.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
DistinguishedName van het doelobject AD machtigingen in te stellen (optioneel)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
Optionele parameter om aan te geven als AdminSDHolder container niet met deze machtigingen moet worden bijgewerkt

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

## <a name="show-adsyncadobjectpermissions"></a>Show-ADSyncADObjectPermissions

### <a name="synopsis"></a>SAMENVATTING
Ziet u de machtigingen van een opgegeven AD-object.

### <a name="syntax"></a>SYNTAXIS

```
Show-ADSyncADObjectPermissions [-ADobjectDN] <String> [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING
Deze functie retourneert alle AD-machtigingen die momenteel is ingesteld voor een bepaald AD-object dat is opgegeven in de parameter - ADobjectDN.
De ADobjectDN moet worden opgegeven in een indeling DistinguishedName.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1
```
Show-ADSyncADObjectPermissions -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-adobjectdn"></a>-ADobjectDN
{{Fill ADobjectDN Description}}

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

#### <a name="commonparameters"></a>CommonParameters
Deze cmdlet biedt ondersteuning voor de meest gebruikte parameters: -Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable - Verbose, - WarningAction en -WarningVariable.
Zie about_CommonParameters voor meer informatie (https://go.microsoft.com/fwlink/?LinkID=113216).
