---
title: 'Azure AD Connect-synchronisatie: Naslaginformatie over functies | Microsoft Docs'
description: Verwijzing van declaratieve inrichtingsexpressies in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b33e993dbddc9c1567a1a6f7d3dca28af240a000
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650662"
---
# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect-synchronisatie: Functieverwijzing
In Azure AD Connect, worden functies gebruikt voor het bewerken van een kenmerkwaarde tijdens de synchronisatie.  
De syntaxis van de functies wordt uitgedrukt in de volgende notatie:  
`<output type> FunctionName(<input type> <position name>, ..)`

Als een functie is overbelast en meerdere parameters accepteert, worden alle geldige syntaxis weergegeven.  
De functies zijn sterk getypeerd en ze controleren of het doorgegeven type komt overeen met de gedocumenteerde type.  
Als het type niet overeenkomt, wordt een fout opgetreden.

De typen worden uitgedrukt met de volgende syntaxis:

* **bIn** : binaire
* **BOOL** – Booleaanse
* **DT** – UTC-datum/tijd
* **Enum** – opsomming van de bekende constanten
* **EXP** -expressie die wordt verwacht te laten resulteren in een Booleaanse waarde
* **mvbin** – Multi-Valued Binary
* **mvstr** – meerwaardige tekenreeks
* **mvref** – meerwaardige verwijzing
* **num** – Numeric
* **REF** -verwijzing
* **Str** : tekenreeks
* **VAR** : een variant van (bijna) alle andere type
* **ongeldig** – geen waarde als resultaat

De functies met de typen **mvbin**, **mvstr**, en **mvref** kan alleen worden gebruikt voor kenmerken met meerdere waarden. Functies met **bin**, **str**, en **ref** werk op zowel één waarde als meerwaardige kenmerken.

## <a name="functions-reference"></a>Functieverwijzing

| Lijst met functies |  |  |  |  |
| --- | --- | --- | --- | --- |
| **Certificaat** | | | | |
| [CertExtensionOids](#certextensionoids) |[CertFormat](#certformat) |[CertFriendlyName](#certfriendlyname) |[CertHashString](#certhashstring) | |
| [CertIssuer](#certissuer) |[CertIssuerDN](#certissuerdn) |[CertIssuerOid](#certissueroid) |[CertKeyAlgorithm](#certkeyalgorithm) | |
| [CertKeyAlgorithmParams](#certkeyalgorithmparams) |[CertNameInfo](#certnameinfo) |[CertNotAfter](#certnotafter) |[CertNotBefore](#certnotbefore) | |
| [CertPublicKeyOid](#certpublickeyoid) |[CertPublicKeyParametersOid](#certpublickeyparametersoid) |[CertSerialNumber](#certserialnumber) |[CertSignatureAlgorithmOid](#certsignaturealgorithmoid) | |
| [CertSubject](#certsubject) |[CertSubjectNameDN](#certsubjectnamedn) |[CertSubjectNameOid](#certsubjectnameoid) |[CertThumbprint](#certthumbprint) | |
[CertVersion](#certversion) |[IsCert](#iscert) | | | |
| **Conversie** | | | | |
| [CBool](#cbool) |[CDate](#cdate) |[CGuid](#cguid) |[ConvertFromBase64](#convertfrombase64) | |
| [ConvertToBase64](#converttobase64) |[ConvertFromUTF8Hex](#convertfromutf8hex) |[ConvertToUTF8Hex](#converttoutf8hex) |[CNum](#cnum) | |
| [CRef](#cref) |[CStr](#cstr) |[StringFromGuid](#stringfromguid) |[StringFromSid](#stringfromsid) | |
| **Datum / tijd** | | | | |
| [DateAdd](#dateadd) |[DateFromNum](#datefromnum) |[FormatDateTime](#formatdatetime) |[nu](#now) | |
| [NumFromDate](#numfromdate) | | | | |
| **Directory** | | | | |
| [DNComponent](#dncomponent) |[DNComponentRev](#dncomponentrev) |[EscapeDNComponent](#escapedncomponent) | | |
| **Evaluatie** | | | | |
| [IsBitSet](#isbitset) |[IsDate](#isdate) |[IsEmpty](#isempty) |[IsGuid](#isguid) | |
| [IsNull](#isnull) |[IsNullOrEmpty](#isnullorempty) |[IsNumeric](#isnumeric) |[IsPresent](#ispresent) | |
| [IsString](#isstring) | | | | |
| **Math** | | | | |
| [BitAnd](#bitand) |[BitOr](#bitor) |[RandomNum](#randomnum) | | |
| **Meerdere waarden** | | | | |
| [bevat](#contains) |[Aantal](#count) |[Item](#item) |[ItemOrNull](#itemornull) | |
| [Koppelen](#join) |[RemoveDuplicates](#removeduplicates) |[Split](#split) | | |
| **Program Flow** | | | | |
| [Fout](#error) |[IIF](#iif) |[Selecteren](#select) |[Switch](#switch) | |
| [waar](#where) |[met](#with) | | | |
| **Tekst** | | | | |
| [GUID](#guid) |[InStr](#instr) |[InStrRev](#instrrev) |[LCase](#lcase) | |
| [Links](#left) |[Len](#len) |[LTrim](#ltrim) |[Mid](#mid) | |
| [PadLeft](#padleft) |[PadRight](#padright) |[PCase](#pcase) |[vervangen](#replace) | |
| [ReplaceChars](#replacechars) |[Right](#right) |[RTrim](#rtrim) |[Trim](#trim) | |
| [UCase](#ucase) |[Word](#word) | | | |

- - -
### <a name="bitand"></a>BitAnd
**Beschrijving:**  
De functie BitAnd wordt opgegeven bits ingesteld op een waarde.

**Syntaxis:**  
`num BitAnd(num value1, num value2)`

* waarde1, waarde2: numerieke waarden die functiesleutels moeten samen worden

**Opmerking:**  
Deze functie worden beide parameters geconverteerd naar een binaire indeling en iets op:

* 0 - als één of beide van de bijbehorende bits in *masker* en *vlag* 0 zijn
* 1 - als beide van de bijbehorende bits 1 zijn.

Met andere woorden, retourneert 0 in alle gevallen, behalve wanneer de bijbehorende bits van beide parameters 1.

**Voorbeeld:**  
`BitAnd(&HF, &HF7)`  
Retourneert 7 omdat hexadecimale "F" en "F7" evalueren met deze waarde.

- - -
### <a name="bitor"></a>BitOr
**Beschrijving:**  
De functie BitOr wordt opgegeven bits ingesteld op een waarde.

**Syntaxis:**  
`num BitOr(num value1, num value2)`

* waarde1, waarde2: numerieke waarden die bij elkaar worden moeten

**Opmerking:**  
Deze functie beide parameters converteert naar een binaire indeling en iets op 1 als een of beide van de bijbehorende bits masker en vlag liggen tussen 1 en ingesteld op 0 als beide van de bijbehorende bits 0 zijn. Met andere woorden, retourneert 1 in alle gevallen, behalve wanneer de bijbehorende bits van beide parameters zijn aan 0.

- - -
### <a name="cbool"></a>CBool
**Beschrijving:**  
De functie CBool retourneert een Booleaanse waarde op basis van de geëvalueerde expressie

**Syntaxis:**  
`bool CBool(exp Expression)`

**Opmerking:**  
Als resultaat van de expressie naar een andere waarde dan nul, en vervolgens CBool resulteert in waar, anders wordt onwaar retourneert.

**Voorbeeld:**  
`CBool([attrib1] = [attrib2])`  

Retourneert ' True ' wanneer beide kenmerken hebben dezelfde waarde.

- - -
### <a name="cdate"></a>CDate
**Beschrijving:**  
De functie CDate retourneert een UTC-datum-/ van een tekenreeks. Datum/tijd is niet een systeemeigen kenmerktype gesynchroniseerd, maar wordt gebruikt door sommige functies.

**Syntaxis:**  
`dt CDate(str value)`

* Waarde: Een tekenreeks zijn met een datum, tijd en eventueel tijdzone

**Opmerking:**  
De tekenreeks wordt geretourneerd is altijd ingesteld op UTC.

**Voorbeeld:**  
`CDate([employeeStartTime])`  
Retourneert die een datum/tijd op basis van de werknemer begintijd

`CDate("2013-01-10 4:00 PM -8")`  
Retourneert een datum/tijd die "2013-01-11-12:00 AM"


- - -
### <a name="certextensionoids"></a>CertExtensionOids
**Beschrijving:**  
Retourneert de waarden van de Oid van de kritieke uitbreidingen van een certificaatobject.

**Syntaxis:**  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certformat"></a>CertFormat
**Beschrijving:**  
Retourneert de naam van de indeling van deze X.509v3-certificaat.

**Syntaxis:**  
`str CertFormat(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certfriendlyname"></a>CertFriendlyName
**Beschrijving:**  
Retourneert de bijbehorende alias voor een certificaat.

**Syntaxis:**  
`str CertFriendlyName(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certhashstring"></a>CertHashString
**Beschrijving:**  
Retourneert de SHA1-hash-waarde voor de X.509v3-certificaat als een hexadecimale tekenreeks.

**Syntaxis:**  
`str CertHashString(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certissuer"></a>CertIssuer
**Beschrijving:**  
Retourneert de naam van de certificeringsinstantie die het X.509v3-certificaat heeft uitgegeven.

**Syntaxis:**  
`str CertIssuer(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certissuerdn"></a>CertIssuerDN
**Beschrijving:**  
Retourneert de DN-naam van de uitgever van het certificaat.

**Syntaxis:**  
`str CertIssuerDN(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certissueroid"></a>CertIssuerOid
**Beschrijving:**  
Retourneert de Oid van de uitgever van het certificaat.

**Syntaxis:**  
`str CertIssuerOid(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certkeyalgorithm"></a>CertKeyAlgorithm
**Beschrijving:**  
Retourneert de algoritme van sleutel-informatie voor deze X.509v3-certificaat als een tekenreeks.

**Syntaxis:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certkeyalgorithmparams"></a>CertKeyAlgorithmParams
**Beschrijving:**  
Retourneert de algoritme van sleutel-parameters voor de X.509v3-certificaat als een hexadecimale tekenreeks.

**Syntaxis:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certnameinfo"></a>CertNameInfo
**Beschrijving:**  
Retourneert het onderwerp en de verlener namen van een certificaat.

**Syntaxis:**  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.
*   X509NameType: De waarde X509NameType voor het onderwerp.
*   includesIssuerName: waar om op te nemen van de naam van de certificaatverlener; anders wordt onwaar.

- - -
### <a name="certnotafter"></a>CertNotAfter
**Beschrijving:**  
Retourneert de datum in plaatselijke tijd waarna een certificaat niet meer geldig is.

**Syntaxis:**  
`dt CertNotAfter(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certnotbefore"></a>CertNotBefore
**Beschrijving:**  
Retourneert de datum in plaatselijke tijd waarop een certificaat geldig wordt.

**Syntaxis:**  
`dt CertNotBefore(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certpublickeyoid"></a>CertPublicKeyOid
**Beschrijving:**  
Retourneert de Oid van de openbare sleutel voor de X.509v3-certificaat.

**Syntaxis:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certpublickeyparametersoid"></a>CertPublicKeyParametersOid
**Beschrijving:**  
Retourneert de Oid van de openbare sleutel parameters voor de X.509v3-certificaat.

**Syntaxis:**  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certserialnumber"></a>CertSerialNumber
**Beschrijving:**  
Retourneert het serienummer van het X.509v3-certificaat.

**Syntaxis:**  
`str CertSerialNumber(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certsignaturealgorithmoid"></a>CertSignatureAlgorithmOid
**Beschrijving:**  
Retourneert de Oid van het algoritme voor het maken van de handtekening van een certificaat.

**Syntaxis:**  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certsubject"></a>CertSubject
**Beschrijving:**  
Hiermee haalt u de DN-naam van een certificaat.

**Syntaxis:**  
`str CertSubject(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certsubjectnamedn"></a>CertSubjectNameDN
**Beschrijving:**  
Retourneert de DN-naam van een certificaat.

**Syntaxis:**  
`str CertSubjectNameDN(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certsubjectnameoid"></a>CertSubjectNameOid
**Beschrijving:**  
Retourneert de Oid van de onderwerpnaam van een certificaat.

**Syntaxis:**  
`str CertSubjectNameOid(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certthumbprint"></a>CertThumbprint
**Beschrijving:**  
Retourneert de vingerafdruk van een certificaat.

**Syntaxis:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="certversion"></a>CertVersion
**Beschrijving:**  
Retourneert de versie van de x.509-indeling van een certificaat.

**Syntaxis:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.

- - -
### <a name="cguid"></a>CGuid
**Beschrijving:**  
De functie CGuid converteert de tekenreeksweergave van een GUID naar de binaire indeling.

**Syntaxis:**  
`bin CGuid(str GUID)`

* Een tekenreeks die is opgemaakt in dit patroon: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx of {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

- - -
### <a name="contains"></a>Contains
**Beschrijving:**  
De functie Contains zoekt u naar een tekenreeks binnen een kenmerk met meerdere waarden

**Syntaxis:**  
`num Contains (mvstring attribute, str search)` -hoofdlettergevoelig  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)` -hoofdlettergevoelig

* kenmerk: het kenmerk met meerdere waarden om te zoeken.
* Search: tekenreeks die moet worden gevonden in het kenmerk.
* Casetype: CaseInsensitive of CaseSensitive.

Retourneert de index in het kenmerk met meerdere waarden waarin de tekenreeks is gevonden. 0 wordt geretourneerd als de tekenreeks is niet gevonden.

**Opmerking:**  
Voor meerdere waarden tekenreekskenmerken, subtekenreeksen gezocht in de waarden.  
Voor verwijzingskenmerken, moet de doorzochte tekenreeks exact overeenkomen met de waarde om te worden beschouwd als een overeenkomst.

**Voorbeeld:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Als het proxyAddresses-kenmerk een primaire e-mailadres heeft (aangegeven door hoofdletters ' SMTP: "), retourneert de proxyAddress-kenmerk, anders retourneert een fout.

- - -
### <a name="convertfrombase64"></a>ConvertFromBase64
**Beschrijving:**  
De functie ConvertFromBase64 zet de waarde van de opgegeven base64-gecodeerd om een normale tekenreeks.

**Syntaxis:**  
`str ConvertFromBase64(str source)` -wordt ervan uitgegaan dat Unicode voor encoding  
`str ConvertFromBase64(str source, enum Encoding)`

* Bron: Base64-gecodeerde tekenreeks  
* Codering: Unicode, ASCII, UTF8

**Voorbeeld**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Beide voorbeelden retourneren '*Hallo wereld!*"

- - -
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex
**Beschrijving:**  
De functie ConvertFromUTF8Hex converteert de opgegeven UTF8 hexadecimaal gecodeerde waarde naar een tekenreeks.

**Syntaxis:**  
`str ConvertFromUTF8Hex(str source)`

* Bron: UTF8-2-bytes gecodeerde String

**Opmerking:**  
Het verschil tussen deze functie en ConvertFromBase64([],UTF8) in die het resultaat is beschrijvende voor het kenmerk DN-naam.  
Deze indeling wordt gebruikt door Azure Active Directory als de DN-naam.

**Voorbeeld:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Retourneert '*Hallo wereld!*"

- - -
### <a name="converttobase64"></a>ConvertToBase64
**Beschrijving:**  
De functie ConvertToBase64 converteert een tekenreeks naar een met base64 Unicode-tekenreeks.  
De waarde van een matrix van gehele getallen converteert naar de equivalente tekenreeksweergave die is gecodeerd met base64 cijfers.

**Syntaxis:**  
`str ConvertToBase64(str source)`

**Voorbeeld:**  
`ConvertToBase64("Hello world!")`  
Returns "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

- - -
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Beschrijving:**  
De functie ConvertToUTF8Hex converteert een tekenreeks naar een UTF8 hexadecimaal gecodeerde waarde.

**Syntaxis:**  
`str ConvertToUTF8Hex(str source)`

**Opmerking:**  
De indeling van de uitvoer van deze functie wordt gebruikt door Azure Active Directory als de indeling van het kenmerk DN-naam.

**Voorbeeld:**  
`ConvertToUTF8Hex("Hello world!")`  
Returns 48656C6C6F20776F726C6421

- - -
### <a name="count"></a>Count
**Beschrijving:**  
De functie Count retourneert het aantal elementen in een kenmerk met meerdere waarden

**Syntaxis:**  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a>CNum
**Beschrijving:**  
De functie CNum neemt van een tekenreeks en retourneert een numeriek gegevenstype.

**Syntaxis:**  
`num CNum(str value)`

- - -
### <a name="cref"></a>CRef
**Beschrijving:**  
Converteert een tekenreeks naar een verwijzingskenmerk

**Syntaxis:**  
`ref CRef(str value)`

**Voorbeeld:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a>CStr
**Beschrijving:**  
De functie CStr converteert naar een tekenreeks-gegevenstype.

**Syntaxis:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* Waarde: Mag bestaan uit een numerieke waarde, verwijzingskenmerk of Booleaanse waarde.

**Voorbeeld:**  
`CStr([dn])`  
Kan retourneren "cn = Joe, dc = contoso, dc = com"

- - -
### <a name="dateadd"></a>DateAdd
**Beschrijving:**  
Retourneert een datum met een datum waarop een opgegeven tijdsinterval is toegevoegd.

**Syntaxis:**  
`dt DateAdd(str interval, num value, dt date)`

* interval: Tekenreeksexpressie die het tijdsinterval is dat u wilt toevoegen. De tekenreeks moet een van de volgende waarden hebben:
  * JJJJ jaar
  * q kwartaal
  * m maand
  * y-dag van jaar
  * d dag
  * w Weekday
  * ww Week
  * h uur
  * n minuut
  * s tweede
* Waarde: Het aantal eenheden dat u wilt toevoegen. Het kan zijn (voor datums in de toekomst) positief of negatief (voor datums in het verleden).
* datum: Datum en tijd vertegenwoordigt waarop het interval wordt toegevoegd.

**Voorbeeld:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Drie maanden wordt toegevoegd en retourneert een datum/tijd die '2001-04-01'.

- - -
### <a name="datefromnum"></a>DateFromNum
**Beschrijving:**  
De functie DateFromNum zet een waarde in de AD-datum op het type datum/tijd opmaken.

**Syntaxis:**  
`dt DateFromNum(num value)`

**Voorbeeld:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Retourneert een datum/tijd die 01-01-2012 23:00:00 uur

- - -
### <a name="dncomponent"></a>DNComponent
**Beschrijving:**  
De functie DNComponent retourneert de waarde van een opgegeven DN-component die van links.

**Syntaxis:**  
`str DNComponent(ref dn, num ComponentNumber)`

* DN-naam: het verwijzingskenmerk interpreteren
* ComponentNumber: Het onderdeel in de DN-naam om terug te keren

**Voorbeeld:**  
`DNComponent(CRef([dn]),1)`  
Als DN-naam is "cn Jan, ou = =...," Jaap geretourneerd

- - -
### <a name="dncomponentrev"></a>DNComponentRev
**Beschrijving:**  
De functie DNComponentRev retourneert de waarde van een opgegeven DN-component die van rechts (einde).

**Syntaxis:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* DN-naam: het verwijzingskenmerk interpreteren
* ComponentNumber - het onderdeel in de DN-naam om terug te keren
* Opties: DC-negeert alle onderdelen met "dc ="

**Voorbeeld:**  
Als DN-naam is "cn Jan, ou = Atlanta, ou = algemene beschikbaarheid, ou = = US, dc = contoso, dc = com" vervolgens  
`DNComponentRev(CRef([dn]),3)`  
`DNComponentRev(CRef([dn]),1,"DC")`  
Beide retourneren ons.

- - -
### <a name="error"></a>Fout
**Beschrijving:**  
De fout-functie wordt gebruikt om een aangepaste fout geretourneerd.

**Syntaxis:**  
`void Error(str ErrorMessage)`

**Voorbeeld:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Als de kenmerk-accountnaam niet aanwezig is, genereert u een fout op het object.

- - -
### <a name="escapedncomponent"></a>EscapeDNComponent
**Beschrijving:**  
De EscapeDNComponent-functie gebruikt een onderdeel van een DN-naam en verwijdert deze, zodat deze kan worden weergegeven in LDAP.

**Syntaxis:**  
`str EscapeDNComponent(str value)`

**Voorbeeld:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Zorgt ervoor dat het object kan worden gemaakt in een LDAP-adreslijst, zelfs als het kenmerk displayName bevat tekens die moeten worden weergegeven in LDAP.

- - -
### <a name="formatdatetime"></a>formatDateTime
**Beschrijving:**  
De functie FormatDateTime wordt gebruikt om de opmaak van een datum/tijd naar een tekenreeks met een opgegeven indeling

**Syntaxis:**  
`str FormatDateTime(dt value, str format)`

* waarde: een waarde in de datum-/ tijdindeling
* indeling: een tekenreeks voor de indeling moet worden geconverteerd naar.

**Opmerking:**  
De mogelijke waarden voor de indeling vindt u hier: [Aangepaste datum en tijd indelingen voor de functie FORMAT](https://docs.microsoft.com/dax/custom-date-and-time-formats-for-the-format-function).

**Voorbeeld:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
De resultaten in '2007-12-25'.

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Kan leiden tot "20140905081453.0Z"

- - -
### <a name="guid"></a>Guid
**Beschrijving:**  
De Guid van de functie genereert een nieuwe willekeurige GUID

**Syntaxis:**  
`str Guid()`

- - -
### <a name="iif"></a>IIF
**Beschrijving:**  
De functie IIF retourneert een van de mogelijke waarden op basis van een opgegeven voorwaarde.

**Syntaxis:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* voorwaarde: een waarde of expressie die kan worden geëvalueerd op waar of ONWAAR.
* valueIfTrue: Als de voorwaarde is geëvalueerd als waar, de geretourneerde waarde.
* WaardeAlsOnwaar: Als de voorwaarde wordt geëvalueerd op false, de geretourneerde waarde.

**Voorbeeld:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Als de gebruiker een intern is, retourneert de alias van een gebruiker met 't-' toegevoegd aan het begin van deze anders retourneert de alias van de gebruiker is.

- - -
### <a name="instr"></a>InStr
**Beschrijving:**  
De functie InStr Hiermee zoekt u het eerste exemplaar van een subtekenreeks in een tekenreeks

**Syntaxis:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* reekscontroleren: tekenreeks die moet worden gezocht
* reeksvergelijken: tekenreeks die moet worden gevonden
* Start: vanaf positie als u wilt de subtekenreeks
* Vergelijk: vbTextCompare of vbBinaryCompare

**Opmerking:**  
Retourneert de positie waar de subtekenreeks is gevonden of 0 als niet is gevonden.

**Voorbeeld:**  
`InStr("The quick brown fox","quick")`  
Evalues tot en met 5

`InStr("repEated","e",3,vbBinaryCompare)`  
Evalueert tot en met 7

- - -
### <a name="instrrev"></a>InStrRev
**Beschrijving:**  
De functie InStrRev Hiermee zoekt u het laatste exemplaar van een subtekenreeks in een tekenreeks

**Syntaxis:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* reekscontroleren: tekenreeks die moet worden gezocht
* reeksvergelijken: tekenreeks die moet worden gevonden
* Start: vanaf positie als u wilt de subtekenreeks
* Vergelijk: vbTextCompare of vbBinaryCompare

**Opmerking:**  
Retourneert de positie waar de subtekenreeks is gevonden of 0 als niet is gevonden.

**Voorbeeld:**  
`InStrRev("abbcdbbbef","bb")`  
Retourneert 7

- - -
### <a name="isbitset"></a>IsBitSet
**Beschrijving:**  
De functie IsBitSet netwerktests als een bit is ingesteld of niet

**Syntaxis:**  
`bool IsBitSet(num value, num flag)`

* waarde: een numerieke waarde die is evaluated.flag: een numerieke waarde die is van de bit moet worden geëvalueerd

**Voorbeeld:**  
`IsBitSet(&HF,4)`  
Retourneert ' True ' omdat "4"-bit is ingesteld in de hexadecimale waarde 'F'

- - -
### <a name="isdate"></a>IsDate
**Beschrijving:**  
Als de expressie kan worden geëvalueerd als een DateTime-type, en vervolgens de functie IsDate in waar resulteert.

**Syntaxis:**  
`bool IsDate(var Expression)`

**Opmerking:**  
Gebruikt om te bepalen als CDate() uitgevoerd worden kan.

- - -
### <a name="iscert"></a>IsCert
**Beschrijving:**  
Retourneert waar als de onbewerkte gegevens kunnen worden geserialiseerd in .NET X509Certificate2 certificaat-object.

**Syntaxis:**  
`bool CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix kan binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens zijn.
- - -
### <a name="isempty"></a>IsEmpty
**Beschrijving:**  
Als het kenmerk aanwezig in de CS of MV is, maar in een lege tekenreeks resulteert, klikt u vervolgens resulteert de functie IsEmpty in waar.

**Syntaxis:**  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a>IsGuid
**Beschrijving:**  
Als de tekenreeks kan worden geconverteerd naar een GUID, vervolgens de functie IsGuid geëvalueerd op waar.

**Syntaxis:**  
`bool IsGuid(str GUID)`

**Opmerking:**  
Een GUID wordt gedefinieerd als een tekenreeks een van deze patronen te volgen: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx of {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Gebruikt om te bepalen als CGuid() uitgevoerd worden kan.

**Voorbeeld:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Als de StrAttribute een GUID-indeling heeft, retourneert een binaire weergave, anders retourneert een null-waarde.

- - -
### <a name="isnull"></a>IsNull
**Beschrijving:**  
Als de expressie wordt geëvalueerd op Null, zijn de IsNull-functie retourneert ' True '.

**Syntaxis:**  
`bool IsNull(var Expression)`

**Opmerking:**  
Voor een kenmerk is een null-waarde door het ontbreken van het kenmerk uitgedrukt.

**Voorbeeld:**  
`IsNull([displayName])`  
Retourneert waar als het kenmerk niet aanwezig in de CS of MV is.

- - -
### <a name="isnullorempty"></a>IsNullOrEmpty
**Beschrijving:**  
Als de expressie null of een lege tekenreeks is, zijn de IsNullOrEmpty functie retourneert ' True '.

**Syntaxis:**  
`bool IsNullOrEmpty(var Expression)`

**Opmerking:**  
Voor een kenmerk evalueren dit op True als het kenmerk is niet aanwezig of aanwezig is, maar een lege tekenreeks is.  
De omgekeerde waarde van deze functie heet IsPresent.

**Voorbeeld:**  
`IsNullOrEmpty([displayName])`  
Retourneert waar als het kenmerk niet aanwezig is of een lege tekenreeks in de CS of MV is.

- - -
### <a name="isnumeric"></a>IsNumeric
**Beschrijving:**  
De functie IsNumeric retourneert een Booleaanse waarde die aangeeft of een expressie kan worden geëvalueerd als een getal-type.

**Syntaxis:**  
`bool IsNumeric(var Expression)`

**Opmerking:**  
Gebruikt om te bepalen als CNum() uitgevoerd worden kan op de expressie niet parseren.

- - -
### <a name="isstring"></a>IsString
**Beschrijving:**  
Als de expressie kan worden beoordeeld op het type string, klikt u vervolgens resulteert de functie IsString in waar.

**Syntaxis:**  
`bool IsString(var expression)`

**Opmerking:**  
Gebruikt om te bepalen als CStr() uitgevoerd worden kan op de expressie niet parseren.

- - -
### <a name="ispresent"></a>IsPresent
**Beschrijving:**  
Als de expressie in een tekenreeks die niet Null is en niet leeg zijn resulteert, zijn de IsPresent functie retourneert ' True '.

**Syntaxis:**  
`bool IsPresent(var expression)`

**Opmerking:**  
De omgekeerde waarde van deze functie heet IsNullOrEmpty.

**Voorbeeld:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a>Item
**Beschrijving:**  
De functie Item retourneert één item uit een tekenreeks/het kenmerk met meerdere waarden.

**Syntaxis:**  
`var Item(mvstr attribute, num index)`

* kenmerk: kenmerk met meerdere waarden
* index: index van een item in de tekenreeks die meerdere waarden.

**Opmerking:**  
De functie Item is nuttig, samen met de functie Contains sinds de laatste functie geeft als de index een item in het kenmerk met meerdere waarden resultaat.

Genereert een fout als de index is buiten het bereik.

**Voorbeeld:**  
`Mid(Item([proxyAddresses],Contains([proxyAddresses], "SMTP:")),6)`  
Retourneert het primaire e-mailadres.

- - -
### <a name="itemornull"></a>ItemOrNull
**Beschrijving:**  
De functie ItemOrNull retourneert één item uit een tekenreeks/het kenmerk met meerdere waarden.

**Syntaxis:**  
`var ItemOrNull(mvstr attribute, num index)`

* kenmerk: kenmerk met meerdere waarden
* index: index van een item in de tekenreeks die meerdere waarden.

**Opmerking:**  
De functie ItemOrNull is nuttig, samen met de functie Contains sinds de laatste functie geeft als de index een item in het kenmerk met meerdere waarden resultaat.

Als de index ligt buiten het bereik valt, retourneert vervolgens een Null-waarde.

- - -
### <a name="join"></a>Koppelen
**Beschrijving:**  
De Join-functie een tekenreeks met meerdere waarden en retourneert een tekenreeks met één waarde met opgegeven scheidingsteken ingevoegd tussen elk item.

**Syntaxis:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* kenmerk: Kenmerk met meerdere waarden met tekenreeksen die moeten worden toegevoegd.
* scheidingsteken: Een tekenreeks, die wordt gebruikt voor het scheiden van de subtekenreeksen in de geretourneerde tekenreeks. Als u dit weglaat, wordt de spatie ("") wordt gebruikt. Als scheidingsteken een tekenreeks met een lengte van nul is ("") of niets, alle items in de lijst worden samengevoegd met zonder scheidingstekens.

**Opmerkingen**  
Er is pariteit tussen de functies voor lid worden en splitsen. De Join-functie wordt een matrix met tekenreeksen en maakt ze met behulp van een scheidingstekentekenreeks om terug te keren een enkele tekenreeks lid. De functie Split neemt een tekenreeks en onderscheid te maken op het scheidingsteken, om te retourneren van een matrix met tekenreeksen. Een belangrijk verschil is echter dat Join kan tekenreeksen met een willekeurige scheidingstekentekenreeks, Split kunt alleen afzonderlijke tekenreeksen met een scheidingsteken voor één teken.

**Voorbeeld:**  
`Join([proxyAddresses],",")`  
Kan retourneren: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

- - -
### <a name="lcase"></a>LCase
**Beschrijving:**  
De functie LCase converteert alle tekens in een tekenreeks naar kleine letters.

**Syntaxis:**  
`str LCase(str value)`

**Voorbeeld:**  
`LCase("TeSt")`  
Retourneert 'test'.

- - -
### <a name="left"></a>Links
**Beschrijving:**  
De functie links retourneert een opgegeven aantal tekens vanaf de linkerkant van een tekenreeks.

**Syntaxis:**  
`str Left(str string, num NumChars)`

* tekenreeks: de tekenreeks om terug te keren tekens uit
* NumChars: een nummer van het aantal tekens retourneren vanaf het begin (links) van de tekenreeks

**Opmerking:**  
Een tekenreeks met de eerste numChars tekens in tekenreeks:

* Als numChars = 0, lege tekenreeks als resultaat.
* Als de invoertekenreeks numChars < 0, worden geretourneerd.
* Als null-tekenreeks is, retourneert u de lege tekenreeks.

Als de tekenreeks bevat minder tekens dan het aantal opgegeven in numChars, wordt een tekenreeks die identiek is aan de tekenreeks (die is, met alle tekens in parameter 1) geretourneerd.

**Voorbeeld:**  
`Left("John Doe", 3)`  
Retourneert 'Joh'.

- - -
### <a name="len"></a>Len
**Beschrijving:**  
De functie Len retourneert het aantal tekens in een tekenreeks.

**Syntaxis:**  
`num Len(str value)`

**Voorbeeld:**  
`Len("John Doe")`  
Retourneert 8

- - -
### <a name="ltrim"></a>LTrim
**Beschrijving:**  
De functie LTrim verwijdert voorloopspaties wit uit een tekenreeks.

**Syntaxis:**  
`str LTrim(str value)`

**Voorbeeld:**  
`LTrim(" Test ")`  
Retourneert 'Test'

- - -
### <a name="mid"></a>Mid
**Beschrijving:**  
De functie deel retourneert een opgegeven aantal tekens van een opgegeven positie in een tekenreeks.

**Syntaxis:**  
`str Mid(str string, num start, num NumChars)`

* tekenreeks: de tekenreeks om terug te keren tekens uit
* Start: een nummer voor de eerste positie in de tekenreeks die moet worden geretourneerd van de tekens uit
* NumChars: een nummer van het aantal tekens moet worden geretourneerd door de positie in tekenreeks

**Opmerking:**  
Geretourneerde numChars tekens vanaf positie start in de tekenreeks.  
Een tekenreeks met numChars tekens vanaf het begin van de positie in tekenreeks:

* Als numChars = 0, lege tekenreeks als resultaat.
* Als de invoertekenreeks numChars < 0, worden geretourneerd.
* Als start > de lengte van tekenreeks, invoerreeks retourneren.
* Als start < = 0, invoerreeks retourneren.
* Als null-tekenreeks is, retourneert u de lege tekenreeks.

Als er niet numChar tekens worden resterend in tekenreeks vanaf het begin van de positie, zo veel mogelijk tekens mogelijk geretourneerd.

**Voorbeeld:**  
`Mid("John Doe", 3, 5)`  
Retourneert "hn doen".

`Mid("John Doe", 6, 999)`  
Retourneert "Doe"

- - -
### <a name="now"></a>Nu
**Beschrijving:**  
De functie nu retourneert een datum/tijd op te geven van de huidige datum en tijd, op basis van datum en tijd van de computer.

**Syntaxis:**  
`dt Now()`

- - -
### <a name="numfromdate"></a>NumFromDate
**Beschrijving:**  
De functie NumFromDate retourneert een datum in de datumnotatie van AD.

**Syntaxis:**  
`num NumFromDate(dt value)`

**Voorbeeld:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Retourneert 129699324000000000

- - -
### <a name="padleft"></a>padLeft
**Beschrijving:**  
De PadLeft functie left-buiten een tekenreeks aan een opgegeven lengte met behulp van een opgegeven opvulteken.

**Syntaxis:**  
`str PadLeft(str string, num length, str padCharacter)`

* tekenreeks: de tekenreeks die moet worden opgevuld.
* lengte van: Een geheel getal dat de gewenste lengte van tekenreeks vertegenwoordigt.
* padCharacter: Een tekenreeks die bestaat uit een enkel teken te gebruiken als de pad-teken

**Opmerking:**

* Als de lengte van tekenreeks kleiner dan de lengte is, klikt u vervolgens padCharacter herhaaldelijk toegevoegd aan het begin (links) van de tekenreeks wanneer dat nodig is een lengte die gelijk zijn aan lengte.
* PadCharacter kan een spatie, maar mag niet een null-waarde.
* Als de lengte van de tekenreeks gelijk aan of groter is dan de lengte is, tekenreeks geretourneerd ongewijzigd.
* Als de tekenreeks een lengte die groter zijn dan of gelijk zijn aan lengte heeft, wordt een tekenreeks die identiek is aan de tekenreeks geretourneerd.
* Als de lengte van tekenreeks kleiner dan de lengte is, wordt een nieuwe tekenreeks van de gewenste lengte met tekenreeks die is opgevuld met een padCharacter geretourneerd.
* Als null-tekenreeks is, retourneert de functie een lege tekenreeks.

**Voorbeeld:**  
`PadLeft("User", 10, "0")`  
Retourneert '000000User'.

- - -
### <a name="padright"></a>PadRight
**Beschrijving:**  
De PadRight functie rechts-buiten een tekenreeks aan een opgegeven lengte met behulp van een opgegeven opvulteken.

**Syntaxis:**  
`str PadRight(str string, num length, str padCharacter)`

* tekenreeks: de tekenreeks die moet worden opgevuld.
* lengte van: Een geheel getal dat de gewenste lengte van tekenreeks vertegenwoordigt.
* padCharacter: Een tekenreeks die bestaat uit een enkel teken te gebruiken als de pad-teken

**Opmerking:**

* Als de lengte van tekenreeks kleiner dan de lengte is, is klikt u vervolgens padCharacter herhaaldelijk toegevoegd aan het einde (rechts) van de tekenreeks totdat het heeft een lengte die gelijk zijn aan lengte.
* PadCharacter kan een spatie, maar mag niet een null-waarde.
* Als de lengte van de tekenreeks gelijk aan of groter is dan de lengte is, tekenreeks geretourneerd ongewijzigd.
* Als de tekenreeks een lengte die groter zijn dan of gelijk zijn aan lengte heeft, wordt een tekenreeks die identiek is aan de tekenreeks geretourneerd.
* Als de lengte van tekenreeks kleiner dan de lengte is, wordt een nieuwe tekenreeks van de gewenste lengte met tekenreeks die is opgevuld met een padCharacter geretourneerd.
* Als null-tekenreeks is, retourneert de functie een lege tekenreeks.

**Voorbeeld:**  
`PadRight("User", 10, "0")`  
Retourneert 'User000000'.

- - -
### <a name="pcase"></a>PCase
**Beschrijving:**  
De functie PCase converteert het eerste teken van elk woord door spaties gescheiden in een tekenreeks naar hoofdletters, en alle overige tekens worden geconverteerd naar kleine letters.

**Syntaxis:**  
`String PCase(string)`

**Opmerking:**

* Deze functie biedt momenteel geen juiste hoofdlettergebruik als u wilt converteren van een woord dat is volledig hoofdletters, zoals een acroniem.

**Voorbeeld:**  
`PCase("TEsT")`  
Retourneert 'Test'.

`PCase(LCase("TEST"))`  
Retourneert 'Test'

- - -
### <a name="randomnum"></a>RandomNum
**Beschrijving:**  
De functie RandomNum retourneert een willekeurig getal tussen een opgegeven interval.

**Syntaxis:**  
`num RandomNum(num start, num end)`

* Start: een nummer voor het genereren van de ondergrens van een willekeurige waarde
* einde: een nummer voor het genereren van de bovengrens van de willekeurige waarde

**Voorbeeld:**  
`Random(100,999)`  
Kan 734 retourneren.

- - -
### <a name="removeduplicates"></a>RemoveDuplicates
**Beschrijving:**  
De functie RemoveDuplicates neemt een tekenreeks met meerdere waarden en zorg ervoor dat elke waarde is uniek.

**Syntaxis:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Voorbeeld:**  
`RemoveDuplicates([proxyAddresses])`  
Retourneert een opgeschoonde proxyAddress-kenmerk waar alle dubbele waarden zijn verwijderd.

- - -
### <a name="replace"></a>Vervangen
**Beschrijving:**  
De functie vervangen vervangt alle instanties van een tekenreeks naar een andere tekenreeks.

**Syntaxis:**  
`str Replace(str string, str OldValue, str NewValue)`

* tekenreeks: Een tekenreeks die moet worden vervangen door waarden in.
* OldValue: De tekenreeks die moet worden gezocht en te vervangen.
* NewValue: De tekenreeks die moet worden vervangen.

**Opmerking:**  
De functie herkent de volgende speciale monikers:

* \n – New Line
* \r – regelterugloop
* \t – tabblad

**Voorbeeld:**  
`Replace([address],"\r\n",", ")`  
CRLF vervangen door een komma en een spatie, en kan leiden tot "Één Microsoft manier, Redmond, WA, VS"

- - -
### <a name="replacechars"></a>ReplaceChars
**Beschrijving:**  
De functie ReplaceChars vervangt alle instanties van tekens in de tekenreeks ReplacePattern gevonden.

**Syntaxis:**  
`str ReplaceChars(str string, str ReplacePattern)`

* tekenreeks: Een tekenreeks die moet worden vervangen door de tekens in.
* ReplacePattern: een tekenreeks met een woordenlijst met de tekens wilt vervangen.

De indeling {bron1} is: {target1}, {bron2}: {target2}, {bronN}, {targetN} bron is waar het teken om te zoeken en het doel van de tekenreeks die moet worden vervangen.

**Opmerking:**

* De functie heeft elk exemplaar van de gedefinieerde bronnen en vervangt deze door de doelen.
* De bron moet precies één teken voor (unicode).
* De bron kan niet leeg of langer is dan één teken (parseerfout).
* Het doel kan meerdere tekens, bijvoorbeeld ö:oe, β:ss hebben.
* Het doel mag leeg zijn die aangeeft dat het teken moet worden verwijderd.
* De bron is hoofdlettergevoelig en moet exact overeenkomen.
* De (door komma's) en: (dubbele punt) gereserveerde tekens zijn en met behulp van deze functie kan niet worden vervangen.
* Spaties en andere wit tekens in de ReplacePattern tekenreeks worden genegeerd.

**Voorbeeld:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Retourneert Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Retourneert 'ONeil', de één tick is gedefinieerd om te worden verwijderd.

- - -
### <a name="right"></a>Rechts
**Beschrijving:**  
De juiste functie retourneert een opgegeven aantal tekens vanaf de rechterkant (einde) van een tekenreeks.

**Syntaxis:**  
`str Right(str string, num NumChars)`

* tekenreeks: de tekenreeks om terug te keren tekens uit
* NumChars: een nummer van het aantal tekens retourneren vanaf het einde (rechts) van de tekenreeks

**Opmerking:**  
NumChars tekens worden vanaf de laatste positie van de tekenreeks geretourneerd.

Een tekenreeks met de laatste numChars tekens in tekenreeks:

* Als numChars = 0, lege tekenreeks als resultaat.
* Als de invoertekenreeks numChars < 0, worden geretourneerd.
* Als null-tekenreeks is, retourneert u de lege tekenreeks.

Als de tekenreeks bevat minder tekens dan het aantal opgegeven in NumChars, wordt een tekenreeks die identiek is aan de tekenreeks geretourneerd.

**Voorbeeld:**  
`Right("John Doe", 3)`  
Retourneert "Doe".

- - -
### <a name="rtrim"></a>RTrim
**Beschrijving:**  
De functie RTrim verwijdert de afsluitende spaties uit een tekenreeks.

**Syntaxis:**  
`str RTrim(str value)`

**Voorbeeld:**  
`RTrim(" Test ")`  
Retourneert 'Test'.

- - -
### <a name="select"></a>Selecteer
**Beschrijving:**  
Het proces alle waarden in een meerdere waarden kenmerk (of uitvoer van een expressie) op basis van functie worden opgegeven.

**Syntaxis:**  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* item: Hiermee geeft u een element in het kenmerk met meerdere waarden
* kenmerk: het kenmerk met meerdere waarden
* expressie: een expressie waarmee een verzameling waarden wordt geretourneerd
* voorwaarde: een functie waarmee een item in het kenmerk kan worden verwerkt.

**Voorbeelden:**  
`Select($item,[otherPhone],Replace($item,"-",""))`  
Alle waarden in de otherPhone kenmerk met meerdere waarden retourneren nadat afbreekstreepjes (-) zijn verwijderd.

- - -
### <a name="split"></a>Splitsen
**Beschrijving:**  
De functie Split een tekenreeks van elkaar gescheiden met een scheidingsteken en maakt het een tekenreeks met meerdere waarden.

**Syntaxis:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* waarde: de tekenreeks met een scheidingsteken om te scheiden.
* scheidingsteken: Eén teken moet worden gebruikt als het scheidingsteken.
* limiet: maximum aantal waarden die kunnen retourneren.

**Voorbeeld:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Retourneert een tekenreeks met meerdere waarden met 2 elementen nuttig voor het proxyAddress-kenmerk.

- - -
### <a name="stringfromguid"></a>StringFromGuid
**Beschrijving:**  
De functie StringFromGuid wordt een binaire GUID en geconverteerd naar een tekenreeks

**Syntaxis:**  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a>StringFromSid
**Beschrijving:**  
De functie StringFromSid converteert een bytematrix die een beveiligings-id naar een tekenreeks.

**Syntaxis:**  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a>Switch
**Beschrijving:**  
De functie Switch wordt gebruikt om op basis van geëvalueerde voorwaarden één waarde retourneren.

**Syntaxis:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* expr: Variant-expressie die u wilt evalueren.
* Waarde: De waarde die worden geretourneerd als de bijbehorende expressie waar is.

**Opmerking:**  
De lijst met argumenten functie Switch bestaat uit combinaties van expressies en waarden. De expressies worden van links naar rechts geëvalueerd en wordt de waarde die is gekoppeld aan de eerste expressie die moet worden geëvalueerd tot ' True ' geretourneerd. Als de onderdelen zijn niet goed gekoppeld, wordt er een runtime-fout optreedt.

Bijvoorbeeld, als expr1 waar is, retourneert Switch value1. Als markering-1 ingesteld op False is, maar de markering-2 True is, retourneert de Switch waarde 2, enzovoort.

Switch een waarbij niets wordt geretourneerd als:

* Geen van de expressies zijn True.
* De eerste expressie die de waarde True heeft een overeenkomende waarde die Null is.

Switch evalueert alle expressies, zelfs als er slechts één provider retourneert. Daarom moet u bekijken op neveneffecten. Als de evaluatie van een expressie in een deling door nul-fout resulteert, wordt bijvoorbeeld een fout optreedt.

Waarde kan ook worden de functie fout, die een aangepaste tekenreeks geretourneerd.

**Voorbeeld:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Retourneert de gesproken taal in sommige steden, anders wordt een fout geretourneerd.

- - -
### <a name="trim"></a>Omloopspaties wissen
**Beschrijving:**  
De functie spaties verwijdert voorloopspaties en afsluitende spaties uit een tekenreeks.

**Syntaxis:**  
`str Trim(str value)`  

**Voorbeeld:**  
`Trim(" Test ")`  
Retourneert 'Test'.

`Trim([proxyAddresses])`  
Verwijdert voorloopspaties en afsluitende spaties voor elke waarde in het proxyAddress-kenmerk.

- - -
### <a name="ucase"></a>UCase
**Beschrijving:**  
De functie UCase converteert alle tekens in een tekenreeks naar hoofdletters.

**Syntaxis:**  
`str UCase(str string)`

**Voorbeeld:**  
`UCase("TeSt")`  
Returns "TEST".

- - -
### <a name="where"></a>Waar

**Beschrijving:**  
Retourneert een subset van waarden in een meerdere waarden kenmerk (of een uitvoer van een expressie) op basis van specifieke voorwaarde.

**Syntaxis:**  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* item: Hiermee geeft u een element in het kenmerk met meerdere waarden
* kenmerk: het kenmerk met meerdere waarden
* voorwaarde: een expressie die kan worden geëvalueerd op waar of ONWAAR
* expressie: een expressie waarmee een verzameling waarden wordt geretourneerd

**Voorbeeld:**  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
Retourneert de certificaatwaarden in het kenmerk met meerdere waarden userCertificate die niet zijn verlopen.

- - -
### <a name="with"></a>Met
**Beschrijving:**  
De With-functie biedt een manier om een complexe expressie vereenvoudigen met behulp van een variabele om weer te geven van een subexpressie die wordt weergegeven een of meer keer in de complexe expressie.

**Syntaxis:**
`With(var variable, exp subExpression, exp complexExpression)`  
* variabele: Hiermee geeft u de subexpressie.
* Subexpressie: subexpressie vertegenwoordigd door de variabele.
* complexExpression: Een complexe expressie.

**Voorbeeld:**  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
Is functioneel equivalent met:  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
Die retourneert alleen de waarden voor niet-verlopen certificaten in het kenmerk userCertificate.


- - -
### <a name="word"></a>Word
**Beschrijving:**  
De functie woord retourneert een woord dat is opgenomen in een tekenreeks, op basis van parameters met een beschrijving van de scheidingstekens moeten worden gebruikt en het aantal word om terug te keren.

**Syntaxis:**  
`str Word(str string, num WordNumber, str delimiters)`

* tekenreeks: de tekenreeks die moet worden geretourneerd van een woord uit.
* WordNumber: een getal dat aangeeft welke word-nummer moet worden geretourneerd.
* scheidingstekens: een tekenreeks voor de scheidingstekens dat moet worden gebruikt voor het identificeren van woorden

**Opmerking:**  
Elke reeks tekens in een tekenreeks van elkaar gescheiden door een van de tekens in scheidingstekens worden aangeduid als woorden:

* Als getal < 1, het resultaat een lege tekenreeks.
* Als null-tekenreeks is, retourneert de lege tekenreeks.

Als de tekenreeks bevat minder dan het aantal woorden of tekenreeks bevat geen woorden aangeduid met scheidingstekens, wordt een lege tekenreeks geretourneerd.

**Voorbeeld:**  
`Word("The quick brown fox",3," ")`  
Retourneert 'brown'

`Word("This,string!has&many separators",3,",!&#")`  
Retourneert 'is'

## <a name="additional-resources"></a>Aanvullende resources
* [Inzicht in verklarende Inrichtingsexpressies](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)
* [Azure AD Connect Sync: Synchronisatieopties aanpassen](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
