---
title: 'Azure AD Connect: Naslagdocumentatie voor PowerShell-ADConnectivityTools | Microsoft Docs'
description: Dit document bevat referentie-informatie voor de ADConnectivityTools.psm1 PowerShell-module.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 10/19/2018
ms.subservice: hybrid
ms.author: billmath
ms.topic: reference
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40b4be9aca5243b80151afac0ae221f0d44509c5
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650510"
---
# <a name="azure-ad-connect--adconnectivitytools-powershell-reference"></a>Azure AD Connect:  Naslagdocumentatie voor PowerShell-ADConnectivityTools

De volgende documentatie bevat referentie-informatie voor de ADConnectivityTools.psm1 PowerShell-Module die is opgenomen in Azure AD Connect.

## <a name="confirm-dnsconnectivity"></a>Confirm-DnsConnectivity

### <a name="synopsis"></a>SAMENVATTING

Detecteert lokale Dns-problemen.

### <a name="syntax"></a>SYNTAXIS

```
Confirm-DnsConnectivity [-Forest] <String> [-DCs] <Array> [-ReturnResultAsPSObject] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Lokale DNS-connectiviteit testen uitgevoerd.
Als u wilt de Active Directory-connector configureert, moet gebruiker beide naamomzetting hebben voor de forest he\she verbinding probeert is te ook zoals in de domeincontrollers die zijn gekoppeld aan dit forest.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Confirm-DnsConnectivity -Forest "TEST.CONTOSO.COM" -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>VOORBEELD 2

```powershell
Confirm-DnsConnectivity -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-forest"></a>-Forest

Hiermee geeft u de naam van het forest om te testen op basis van.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DC 's

Geef de DC's te testen op basis van.

```yml
Type: Array
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject

Retourneert het resultaat van deze diagnose in de vorm van een PSObject.
Tijdens handmatige interactie met dit hulpprogramma niet nodig.

```yml
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

## <a name="confirm-forestexists"></a>Controleer of ForestExists

### <a name="synopsis"></a>SAMENVATTING

Bepaalt of een opgegeven forest bestaat.

### <a name="syntax"></a>SYNTAXIS

```
Confirm-ForestExists [-Forest] <String> [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Vraagt een DNS-server voor de IP-adressen die zijn gekoppeld aan een forest.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-forest"></a>-Forest

Hiermee geeft u de naam van het forest om te testen op basis van.

```yml
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

## <a name="confirm-functionallevel"></a>Confirm-FunctionalLevel

### <a name="synopsis"></a>SAMENVATTING

Controleert of AD-forest-functionaliteitsniveau.

### <a name="syntax"></a>SYNTAXIS

#### <a name="samaccount"></a>SamAccount

```
Confirm-FunctionalLevel -Forest <String> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

#### <a name="forestfqdn"></a>ForestFQDN

```
Confirm-FunctionalLevel -ForestFQDN <Forest> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Controleert of het AD-forest-functionaliteitsniveau is gelijk aan of meer dan een opgegeven MinAdForestVersion (WindowsServer2003).
(DOMEIN\gebruikersnaam)-account en het wachtwoord mag worden aangevraagd.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Confirm-FunctionalLevel -Forest "test.contoso.com"
```

#### <a name="example-2"></a>VOORBEELD 2

```powershell
Confirm-FunctionalLevel -Forest "test.contoso.com" -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

#### <a name="example-3"></a>VOORBEELD 3

```powershell
Confirm-FunctionalLevel -ForestFQDN $ForestFQDN -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-forest"></a>-Forest

Doel-forest.
Standaardwaarde is de Forest van de momenteel aangemelde gebruiker.

```yml
Type: String
Parameter Sets: SamAccount
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-forestfqdn"></a>-ForestFQDN

ForestFQDN doelobject.

```yml
Type: Forest
Parameter Sets: ForestFQDN
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

De functie gebruikt de referenties van de gebruiker die momenteel is aangemeld op de computer, in plaats van aanvragende aangepaste referenties van de gebruiker.

```yml
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

## <a name="confirm-networkconnectivity"></a>Confirm-NetworkConnectivity

### <a name="synopsis"></a>SAMENVATTING

Detecteert lokale problemen met de netwerkverbinding.

### <a name="syntax"></a>SYNTAXIS

```
Confirm-NetworkConnectivity [-DCs] <Array> [-SkipDnsPort] [-ReturnResultAsPSObject] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Voert tests uit LAN-verbinding.

Voor de tests voor de lokale netwerken moet AAD Connect kunnen communiceren met de benoemde domeincontrollers op poort 53 (DNS), 88 (Kerberos) en 389 (LDAP)-de meeste organisaties DNS uitvoert op de DC's, daarom zijn deze test is momenteel geïntegreerd.
Poort 53 moet worden overgeslagen als een andere DNS-server is opgegeven.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Confirm-NetworkConnectivity -SkipDnsPort -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>VOORBEELD 2

```powershell
Confirm-NetworkConnectivity -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM" -Verbose
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-dcs"></a>-DC 's

Geef de DC's te testen op basis van.

```yml
Type: Array
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipdnsport"></a>-SkipDnsPort

Als gebruikers niet met behulp van DNS-services geleverd door de AD-Site / Logon DC en vervolgens he\she mogelijk wilt overslaan controleren of poort 53.
Gebruiker moet nog steeds mogelijk om op te lossen _.ldap._tcp. \<forestfqdn\> in volgorde voor de configuratie van Active Directory-Connector te voltooien.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject

Retourneert het resultaat van deze diagnose in de vorm van een PSObject.
Tijdens handmatige interactie met dit hulpprogramma niet nodig.

```yml
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

## <a name="confirm-targetsarereachable"></a>Confirm-TargetsAreReachable

### <a name="synopsis"></a>SAMENVATTING

Bepaalt of een opgegeven forest en de bijbehorende domeincontrollers bereikbaar zijn.

### <a name="syntax"></a>SYNTAXIS

```
Confirm-TargetsAreReachable [-Forest] <String> [-DCs] <Array> [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Wordt uitgevoerd pingen' ' tests (of een computer kunt bereiken een doelcomputer via het netwerk en/of het internet)

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM" -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>VOORBEELD 2

```powershell
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-forest"></a>-Forest

Hiermee geeft u de naam van het forest om te testen op basis van.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DC 's

Geef de DC's te testen op basis van.

```yml
Type: Array
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

## <a name="confirm-validdomains"></a>Confirm-ValidDomains

### <a name="synopsis"></a>SAMENVATTING

Valideren dat de domeinen in de verkregen Forest FQDN bereikbaar zijn

### <a name="syntax"></a>SYNTAXIS

#### <a name="samaccount"></a>SamAccount

```
Confirm-ValidDomains [-Forest <String>] [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

#### <a name="forestfqdn"></a>ForestFQDN

```
Confirm-ValidDomains -ForestFQDN <Forest> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Valideren dat alle domeinen in de verkregen Forest FQDN-naam van het ophalen van DomainGuid en DomainDN bereikbaar zijn.
(DOMEIN\gebruikersnaam)-account en het wachtwoord mag worden aangevraagd.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Confirm-ValidDomains -Forest "test.contoso.com" -Verbose
```

#### <a name="example-2"></a>VOORBEELD 2

```powershell
Confirm-ValidDomains -Forest "test.contoso.com" -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

#### <a name="example-3"></a>VOORBEELD 3

```powershell
Confirm-ValidDomains -ForestFQDN $ForestFQDN -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-forest"></a>-Forest

Doel-forest.

```yml
Type: String
Parameter Sets: SamAccount
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-forestfqdn"></a>-ForestFQDN

ForestFQDN doelobject.

```yml
Type: Forest
Parameter Sets: ForestFQDN
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

De functie gebruikt de referenties van de gebruiker die momenteel is aangemeld op de computer, in plaats van aanvragende aangepaste referenties van de gebruiker.

```yml
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

## <a name="confirm-validenterpriseadmincredentials"></a>Confirm-ValidEnterpriseAdminCredentials

### <a name="synopsis"></a>SAMENVATTING

Hiermee wordt gecontroleerd of een gebruiker ondernemingsadministratorreferenties heeft.

### <a name="syntax"></a>SYNTAXIS

```
Confirm-ValidEnterpriseAdminCredentials [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Als de opgegeven gebruiker heeft ondernemingsadministratorreferenties zoekt.
(DOMEIN\gebruikersnaam)-account en het wachtwoord mag worden aangevraagd.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Confirm-ValidEnterpriseAdminCredentials -DomainName test.contoso.com -Verbose
```

#### <a name="example-2"></a>VOORBEELD 2

```powershell
Confirm-ValidEnterpriseAdminCredentials -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

De functie gebruikt de referenties van de gebruiker die momenteel is aangemeld op de computer, in plaats van aanvragende aangepaste referenties van de gebruiker.

```yml
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

## <a name="get-domainfqdndata"></a>Get-DomainFQDNData

### <a name="synopsis"></a>SAMENVATTING

Hiermee haalt u een DomainFQDN uit een combinatie van account en wachtwoord.

### <a name="syntax"></a>SYNTAXIS

```
Get-DomainFQDNData [[-DomainFQDNDataType] <String>] [-RunWithCurrentlyLoggedInUserCredentials]
 [-ReturnExceptionOnError] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Pogingen om een object domainFQDN buiten de opgegeven referenties te verkrijgen.
Als de domainFQDN geldig is, dat een DomainFQDNName of RootDomainName wordt geretourneerd, afhankelijk van de keuze van de gebruiker.
(DOMEIN\gebruikersnaam)-account en het wachtwoord mag worden aangevraagd.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Get-DomainFQDNData -DomainFQDNDataType DomainFQDNName -Verbose
```

#### <a name="example-2"></a>VOORBEELD 2

```powershell
Get-DomainFQDNData -DomainFQDNDataType RootDomainName -RunWithCurrentlyLoggedInUserCredentials
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-domainfqdndatatype"></a>-DomainFQDNDataType

Gewenste soort gegevens dat wordt opgehaald.
Momenteel beperkt tot 'DomainFQDNName' of 'RootDomainName'.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

De functie gebruikt de referenties van de gebruiker die momenteel is aangemeld op de computer, in plaats van aanvragende aangepaste referenties van de gebruiker.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnexceptiononerror"></a>-ReturnExceptionOnError

Aanvullende parameter die wordt gebruikt door de functie Start NetworkConnectivityDiagnosisTools

```yml
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

## <a name="get-forestfqdn"></a>Get-ForestFQDN

### <a name="synopsis"></a>SAMENVATTING

Hiermee haalt u een ForestFQDN uit een combinatie van account en wachtwoord.

### <a name="syntax"></a>SYNTAXIS

```
Get-ForestFQDN [-Forest] <String> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Pogingen om toegang te verkrijgen van een ForestFQDN buiten de opgegeven referenties.
(DOMEIN\gebruikersnaam)-account en het wachtwoord mag worden aangevraagd.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Get-ForestFQDN -Forest CONTOSO.MICROSOFT.COM -Verbose
```

#### <a name="example-2"></a>VOORBEELD 2

```powershell
Get-ForestFQDN -Forest CONTOSO.MICROSOFT.COM -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-forest"></a>-Forest

Doel-forest. Standaardwaarde is het domein van de momenteel aangemelde gebruiker.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

De functie gebruikt de referenties van de gebruiker die momenteel is aangemeld op de computer, in plaats van aanvragende aangepaste referenties van de gebruiker.

```yml
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

## <a name="start-connectivityvalidation"></a>Start-ConnectivityValidation

### <a name="synopsis"></a>SAMENVATTING

Main-functie.

### <a name="syntax"></a>SYNTAXIS

```
Start-ConnectivityValidation [-Forest] <String> [-AutoCreateConnectorAccount] <Boolean> [[-UserName] <String>]
 [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Uitvoeringen van de beschikbare mechanismen controleren of AD-referenties zijn geldig.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Start-ConnectivityValidation -Forest "test.contoso.com" -AutoCreateConnectorAccount $True -Verbose
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-forest"></a>-Forest

Doel-forest.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-autocreateconnectoraccount"></a>-AutoCreateConnectorAccount

Voor aangepaste-installaties: Markering waarmee wordt $True als de gebruiker heeft ervoor gekozen 'Maken nieuwe AD-account' in het venster AD-Forestaccount van de AADConnect-wizard.
$False als de gebruiker heeft ervoor gekozen 'Gebruik bestaande AD-account'.
Voor de Express-installaties: De waarde van deze variabele moet $True voor Express-installaties.

```yml
Type: Boolean
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-username"></a>-UserName

De parameter die het veld gebruikersnaam vooraf gevuld wanneer de referenties van gebruiker wordt gevraagd.

```yml
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

## <a name="start-networkconnectivitydiagnosistools"></a>Start-NetworkConnectivityDiagnosisTools

### <a name="synopsis"></a>SAMENVATTING

Main-functie voor verbinding met het netwerk wordt getest.

### <a name="syntax"></a>SYNTAXIS

```
Start-NetworkConnectivityDiagnosisTools [[-Forest] <String>] [-Credentials] <PSCredential>
 [[-LogFileLocation] <String>] [[-DCs] <Array>] [-DisplayInformativeMessage] [-ReturnResultAsPSObject]
 [-ValidCredentials] [<CommonParameters>]
```

### <a name="description"></a>BESCHRIJVING

Voert tests uit LAN-verbinding.

### <a name="examples"></a>VOORBEELDEN

#### <a name="example-1"></a>VOORBEELD 1

```powershell
Start-NetworkConnectivityDiagnosisTools -Forest "TEST.CONTOSO.COM"
```

#### <a name="example-2"></a>VOORBEELD 2

```powershell
Start-NetworkConnectivityDiagnosisTools -Forest "TEST.CONTOSO.COM" -DCs "DC1.TEST.CONTOSO.COM", "DC2.TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETERS

#### <a name="-forest"></a>-Forest

Hiermee geeft u de naam van het forest om te testen op basis van.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-credentials"></a>-Credentials

De gebruikersnaam en het wachtwoord van de gebruiker die de test wordt uitgevoerd.
Hiervoor hetzelfde niveau van de machtigingen die vereist is voor het uitvoeren van de Azure AD Connect-Wizard.

```yml
Type: PSCredential
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-logfilelocation"></a>-LogFileLocation

Hiermee geeft u een de locatie van een logboekbestand dat de uitvoer van deze functie bevat.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DC 's

Geef de DC's te testen op basis van.

```yml
Type: Array
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-displayinformativemessage"></a>-DisplayInformativeMessage

Vlag waarmee een bericht over het doel van deze functie wordt weergegeven.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject

Retourneert het resultaat van deze diagnose in de vorm van een PSObject.
Niet nodig om op te geven tijdens handmatige interactie met dit hulpprogramma.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-validcredentials"></a>-ValidCredentials

Geeft aan of de referenties die de gebruiker heeft getypt geldig zijn.
Niet nodig om op te geven tijdens handmatige interactie met dit hulpprogramma.

```yml
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
