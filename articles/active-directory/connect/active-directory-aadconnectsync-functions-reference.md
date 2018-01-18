---
title: 'Azure AD Connect-synchronisatie: functieverwijzing | Microsoft Docs'
description: Verwijzing van expressies declaratieve inrichting in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 9ce27ca217f99b4f12ca1af0b5a178f5d61a1c89
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect-synchronisatie: functieverwijzing
In Azure AD Connect worden functies gebruikt voor het bewerken van een kenmerkwaarde tijdens de synchronisatie.  
De syntaxis van de functies wordt uitgedrukt in de volgende notatie:  
`<output type> FunctionName(<input type> <position name>, ..)`

Als een functie is overbelast en meerdere syntaxes accepteert, worden alle geldige syntaxis vermeld.  
De functies zijn sterk getypeerd en ze controleren dat het doorgegeven type komt overeen met het gedocumenteerde type.  
Als het type niet overeenkomt, wordt een fout gegenereerd.

De typen worden uitgedrukt met de volgende syntaxis:

* **opslaglocatie** – binaire
* **BOOL** – Booleaanse
* **DT** – UTC-datum/tijd
* **Enum** – opsomming van de bekende constanten
* **EXP** -expressie die moet worden geëvalueerd tot een Booleaanse waarde
* **mvbin** – meerwaardige binaire
* **mvstr** – meerwaardige tekenreeks
* **mvref** – meerwaardige verwijzing
* **NUM** – numerieke
* **REF** : verwijzing
* **Str** – tekenreeks
* **VAR** : een variant van (bijna) alle andere type
* **VOID** – geen waarde als resultaat

De functies van de typen **mvbin**, **mvstr**, en **mvref** kunt werken alleen op kenmerken met meerdere waarden. Functies met **bin**, **str**, en **ref** werk op de kenmerken van zowel één waarde en meerdere waarden.

## <a name="functions-reference"></a>Functieverwijzing
| Lijst met functies |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
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
| [CRef](#cref) |[CStr](#cstr) |[StringFromGuid](#StringFromGuid) |[StringFromSid](#stringfromsid) | |
| **Datum / tijd** | | | | |
| [DateAdd](#dateadd) |[DateFromNum](#datefromnum) |[FormatDateTime](#formatdatetime) |[Nu](#now) | |
| [NumFromDate](#numfromdate) | | | | |
| **Directory** | | | | |
| [DNComponent](#dncomponent) |[DNComponentRev](#dncomponentrev) |[EscapeDNComponent](#escapedncomponent) | | |
| **Evaluatie** | | | | |
| [IsBitSet](#isbitset) |[IsDate](#isdate) |[IsEmpty](#isempty) |[IsGuid](#isguid) | |
| [IsNull](#isnull) |[IsNullOrEmpty](#isnullorempty) |[IsNumeric](#isnumeric) |[IsPresent](#ispresent) | |
| [IsString](#isstring) | | | | |
| **Math** | | | | |
| [BitAnd](#bitand) |[BitOr](#bitor) |[RandomNum](#randomnum) | | |
| **Met meerdere waarden** | | | | |
| [Bevat](#contains) |[Aantal](#count) |[Item](#item) |[ItemOrNull](#itemornull) | |
| [Koppelen](#join) |[RemoveDuplicates](#removeduplicates) |[Splitsen](#split) | | |
| **Programma-overdracht** | | | | |
| [Fout](#error) |[IIF](#iif) |[Selecteren](#select) |[Switch](#switch) | |
| [Waar](#where) |[Met](#with) | | | |
| **Tekst** | | | | |
| [GUID](#guid) |[InStr](#instr) |[InStrRev](#instrrev) |[LCase](#lcase) | |
| [Links](#left) |[Len](#len) |[LTrim](#ltrim) |[Mid](#mid) | |
| [PadLeft](#padleft) |[PadRight](#padright) |[PCase](#pcase) |[Vervangen](#replace) | |
| [ReplaceChars](#replacechars) |[Rechts](#right) |[RTrim](#rtrim) |[Trim](#trim) | |
| [UCase](#ucase) |[Word](#word) | | | |

- - -
### <a name="bitand"></a>BitAnd
**Beschrijving:**  
De functie BitAnd wordt opgegeven bits ingesteld op een waarde.

**Syntaxis:**  
`num BitAnd(num value1, num value2)`

* Value1, value2: numerieke waarden die functiesleutels samen moeten worden

**Opmerkingen:**  
Deze functie worden beide parameters geconverteerd naar binaire voorstelling en iets op:

* 0 - als een of beide van de bijbehorende bits in *masker* en *vlag* 0 zijn
* 1 - als de bijbehorende bits 1 zijn.

Met andere woorden, resultaat het 0 in alle gevallen, behalve wanneer de bijbehorende bits van beide parameters 1.

**Voorbeeld:**  
`BitAnd(&HF, &HF7)`  
Retourneert 7 omdat deze waarde hexadecimale ' F ' en 'F7' evalueren.

- - -
### <a name="bitor"></a>BitOr
**Beschrijving:**  
De functie BitOr wordt opgegeven bits ingesteld op een waarde.

**Syntaxis:**  
`num BitOr(num value1, num value2)`

* Value1, value2: numerieke waarden die samen worden moeten

**Opmerkingen:**  
Deze functie zet beide parameters om de binaire voorstelling en iets op 1 als er een of beide van de bijbehorende bits in masker en markering 1 zijn en ingesteld op 0 als beide van de bijbehorende onderdelen zijn aan 0. Met andere woorden, retourneert 1 in alle gevallen behalve wanneer de bijbehorende bits van beide parameters 0.

- - -
### <a name="cbool"></a>CBool
**Beschrijving:**  
De functie CBool retourneert een Booleaanse waarde op basis van de geëvalueerde expressie

**Syntaxis:**  
`bool CBool(exp Expression)`

**Opmerkingen:**  
Als de expressie op een andere waarde resulteert en True CBool retourneert, anders wordt onwaar.

**Voorbeeld:**  
`CBool([attrib1] = [attrib2])`  

Retourneert waar als beide kenmerken hebben dezelfde waarde.

- - -
### <a name="cdate"></a>CDate
**Beschrijving:**  
De functie CDate retourneert een UTC datetime-waarde van een tekenreeks. Datum/tijd is niet een systeemeigen kenmerktype synchroon maar wordt gebruikt door een aantal functies.

**Syntaxis:**  
`dt CDate(str value)`

* Waarde: Een tekenreeks met een datum, tijd en eventueel tijdzone

**Opmerkingen:**  
De geretourneerde tekenreeks is altijd in UTC.

**Voorbeeld:**  
`CDate([employeeStartTime])`  
Retourneert die een datum/tijd op basis van de werknemer begintijd

`CDate("2013-01-10 4:00 PM -8")`  
Retourneert een DateTime-waarde die aangeeft ' 2013-01-11-12:00 AM '


- - -
### <a name="certextensionoids"></a>CertExtensionOids
**Beschrijving:**  
Retourneert de Oid-waarden van de kritieke uitbreidingen van een certificaatobject.

**Syntaxis:**  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certformat"></a>CertFormat
**Beschrijving:**  
Retourneert de naam van de indeling van deze X.509v3-certificaat.

**Syntaxis:**  
`str CertFormat(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certfriendlyname"></a>CertFriendlyName
**Beschrijving:**  
Retourneert de bijbehorende alias voor een certificaat.

**Syntaxis:**  
`str CertFriendlyName(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certhashstring"></a>CertHashString
**Beschrijving:**  
Retourneert de SHA1-hash-waarde voor het X.509v3-certificaat als een hexadecimale tekenreeks.

**Syntaxis:**  
`str CertHashString(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certissuer"></a>CertIssuer
**Beschrijving:**  
Retourneert de naam van de certificeringsinstantie die het X.509v3-certificaat heeft uitgegeven.

**Syntaxis:**  
`str CertIssuer(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certissuerdn"></a>CertIssuerDN
**Beschrijving:**  
Retourneert de DN-naam van de uitgever van het certificaat.

**Syntaxis:**  
`str CertIssuerDN(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certissueroid"></a>CertIssuerOid
**Beschrijving:**  
Retourneert de Oid van de uitgever van het certificaat.

**Syntaxis:**  
`str CertIssuerOid(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certkeyalgorithm"></a>CertKeyAlgorithm
**Beschrijving:**  
Retourneert de algoritme van sleutel-gegevens voor dit X.509v3-certificaat als een tekenreeks.

**Syntaxis:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certkeyalgorithmparams"></a>CertKeyAlgorithmParams
**Beschrijving:**  
Retourneert de sleutelalgoritme parameters voor de X.509v3-certificaat als een hexadecimale tekenreeks.

**Syntaxis:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certnameinfo"></a>CertNameInfo
**Beschrijving:**  
Retourneert het onderwerp en de verlener namen van een certificaat.

**Syntaxis:**  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.
*   X509NameType: De X509NameType waarde voor het onderwerp.
*   includesIssuerName: waar als u de naam van de certificaatverlener; anders wordt onwaar.

- - -
### <a name="certnotafter"></a>CertNotAfter
**Beschrijving:**  
Retourneert de datum in plaatselijke tijd waarna een certificaat niet meer geldig is.

**Syntaxis:**  
`dt CertNotAfter(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certnotbefore"></a>CertNotBefore
**Beschrijving:**  
Retourneert de datum in plaatselijke tijd waarop een certificaat geldig wordt.

**Syntaxis:**  
`dt CertNotBefore(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certpublickeyoid"></a>CertPublicKeyOid
**Beschrijving:**  
Retourneert de Oid van de openbare sleutel voor het X.509v3-certificaat.

**Syntaxis:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certpublickeyparametersoid"></a>CertPublicKeyParametersOid
**Beschrijving:**  
Retourneert de Oid van de openbare sleutel parameters voor de X.509v3-certificaat.

**Syntaxis:**  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certserialnumber"></a>CertSerialNumber
**Beschrijving:**  
Retourneert het serienummer van het X.509v3-certificaat.

**Syntaxis:**  
`str CertSerialNumber(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certsignaturealgorithmoid"></a>CertSignatureAlgorithmOid
**Beschrijving:**  
Retourneert de Oid-algoritme dat wordt gebruikt om de handtekening van een certificaat te maken.

**Syntaxis:**  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certsubject"></a>CertSubject
**Beschrijving:**  
Hiermee haalt u de DN-naam van een certificaat.

**Syntaxis:**  
`str CertSubject(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certsubjectnamedn"></a>CertSubjectNameDN
**Beschrijving:**  
Retourneert de DN-naam van een certificaat.

**Syntaxis:**  
`str CertSubjectNameDN(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certsubjectnameoid"></a>CertSubjectNameOid
**Beschrijving:**  
Retourneert de Oid van de onderwerpnaam van een certificaat.

**Syntaxis:**  
`str CertSubjectNameOid(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certthumbprint"></a>certThumbprint
**Beschrijving:**  
Retourneert de vingerafdruk van een certificaat.

**Syntaxis:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="certversion"></a>CertVersion
**Beschrijving:**  
Retourneert de versie van het X.509-indeling van een certificaat.

**Syntaxis:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.

- - -
### <a name="cguid"></a>CGuid
**Beschrijving:**  
De functie CGuid converteert de tekenreeksweergave van een GUID voor de binaire weergave.

**Syntaxis:**  
`bin CGuid(str GUID)`

* Een tekenreeks in dit patroon worden opgemaakt: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx of {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

- - -
### <a name="contains"></a>Contains
**Beschrijving:**  
De functie bevat zoekt u naar een tekenreeks binnen een kenmerk met meerdere waarden

**Syntaxis:**  
`num Contains (mvstring attribute, str search)`-hoofdlettergevoelig  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-hoofdlettergevoelig

* kenmerk: het kenmerk met meerdere waarden om te zoeken.
* zoeken: tekenreeks om in het kenmerk te vinden.
* Casetype: CaseInsensitive of CaseSensitive.

Retourneert de index in het kenmerk met meerdere waarden waarin de tekenreeks is gevonden. 0 wordt geretourneerd als de tekenreeks is niet gevonden.

**Opmerkingen:**  
Voor tekenreekskenmerken met meerdere waarden subtekenreeksen gezocht in de waarden.  
Verwijzingskenmerken, de gezochte tekenreeks moet exact overeenkomen met de waarde om te worden beschouwd als een overeenkomst.

**Voorbeeld:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Als het kenmerk proxyAddresses een primaire e-mailadres heeft (aangeduid door hoofdletters ' SMTP: '), klikt u vervolgens het kenmerk proxyAddress retourneren, anders retourneren een foutmelding.

- - -
### <a name="convertfrombase64"></a>ConvertFromBase64
**Beschrijving:**  
De functie ConvertFromBase64 zet de waarde van de opgegeven base64-gecodeerd om een normale tekenreeks.

**Syntaxis:**  
`str ConvertFromBase64(str source)`-wordt ervan uitgegaan dat Unicode voor codering  
`str ConvertFromBase64(str source, enum Encoding)`

* bron: Base64-gecodeerde tekenreeks  
* -Codering: Unicode, ASCII, UTF8

**Voorbeeld**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Beide voorbeelden retourneren '*Hello world!*'

- - -
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex
**Beschrijving:**  
De functie ConvertFromUTF8Hex converteert de opgegeven codering UTF8-Hex-waarde naar een tekenreeks.

**Syntaxis:**  
`str ConvertFromUTF8Hex(str source)`

* bron: UTF8 2-bytes gecodeerde String

**Opmerkingen:**  
Het verschil tussen deze functie en ConvertFromBase64([],UTF8) in die het resultaat is beschrijvende voor het kenmerk DN-naam.  
Deze indeling wordt gebruikt door Azure Active Directory als de DN-naam.

**Voorbeeld:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Retourneert '*Hello world!*'

- - -
### <a name="converttobase64"></a>ConvertToBase64
**Beschrijving:**  
De functie ConvertToBase64 converteert naar een Unicode-base64-tekenreeks.  
De waarde van een matrix van gehele getallen converteert naar de equivalente tekenreeksvoorstelling die is gecodeerd met base 64-cijfers.

**Syntaxis:**  
`str ConvertToBase64(str source)`

**Voorbeeld:**  
`ConvertToBase64("Hello world!")`  
Retourneert 'SABlAGwAbABvACAAdwBvAHIAbABkACEA'

- - -
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Beschrijving:**  
De functie ConvertToUTF8Hex converteert naar een UTF8 hexadecimaal gecodeerde waarde.

**Syntaxis:**  
`str ConvertToUTF8Hex(str source)`

**Opmerkingen:**  
De indeling van de uitvoer van deze functie wordt gebruikt door Azure Active Directory als de indeling van het kenmerk DN-naam.

**Voorbeeld:**  
`ConvertToUTF8Hex("Hello world!")`  
Retourneert 48656C6C6F20776F726C6421

- - -
### <a name="count"></a>Count
**Beschrijving:**  
De functie Count retourneert het aantal elementen in een kenmerk met meerdere waarden

**Syntaxis:**  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a>CNum
**Beschrijving:**  
De functie CNum een tekenreeks en retourneert een numeriek gegevenstype.

**Syntaxis:**  
`num CNum(str value)`

- - -
### <a name="cref"></a>CRef
**Beschrijving:**  
Zet een tekenreeks naar een verwijzingskenmerk

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

* waarde: een numerieke waarde, verwijzingskenmerk of Booleaanse waarde.

**Voorbeeld:**  
`CStr([dn])`  
Kan retourneren "cn = Jan, dc = contoso, dc = com"

- - -
### <a name="dateadd"></a>DateAdd
**Beschrijving:**  
Retourneert een datum met een datum waarop een opgegeven tijdsinterval is toegevoegd.

**Syntaxis:**  
`dt DateAdd(str interval, num value, dt date)`

* interval: een tekenreeksexpressie die de tijdsinterval die u wilt toevoegen. De tekenreeks moet een van de volgende waarden hebben:
  * JJJJ jaar
  * q kwartaal
  * m maand
  * y-dag van jaar
  * d dag
  * w weekdag
  * ww Week
  * h uur
  * n minuut
  * s tweede
* waarde: het aantal eenheden dat u wilt toevoegen. Het kan zijn (voor datums in de toekomst) positief of negatief (voor datums in het verleden).
* datum: DateTime die datum waarop het interval wordt toegevoegd.

**Voorbeeld:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Drie maanden worden toegevoegd en retourneert een datetime-waarde die aangeeft '2001-04-01'.

- - -
### <a name="datefromnum"></a>DateFromNum
**Beschrijving:**  
De functie DateFromNum converteert een waarde in de AD-datum notatie naar een DateTime-type.

**Syntaxis:**  
`dt DateFromNum(num value)`

**Voorbeeld:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Retourneert een datetime-waarde die aangeeft 01-01-2012 23:00:00 uur

- - -
### <a name="dncomponent"></a>DNComponent
**Beschrijving:**  
De functie DNComponent retourneert de waarde van een opgegeven DN-onderdeel van links.

**Syntaxis:**  
`str DNComponent(ref dn, num ComponentNumber)`

* DN-naam: het verwijzingskenmerk interpreteren
* ComponentNumber: Het onderdeel in de DN-naam te retourneren

**Voorbeeld:**  
`DNComponent(CRef([dn]),1)`  
Als een DN-naam is "cn Kees, ou = =..., ' Jan wordt

- - -
### <a name="dncomponentrev"></a>DNComponentRev
**Beschrijving:**  
De functie DNComponentRev retourneert de waarde van een opgegeven DN-onderdeel van rechts (de end).

**Syntaxis:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* DN-naam: het verwijzingskenmerk interpreteren
* ComponentNumber - het onderdeel in de DN-naam te retourneren
* -Opties: DC-alle onderdelen met negeren ' dc = "

**Voorbeeld:**  
Als een DN-naam is "cn Kees, ou = Atlanta, ou = GA, ou = = US, dc = contoso, dc = com" vervolgens  
`DNComponentRev(CRef([dn]),3)`  
`DNComponentRev(CRef([dn]),1,"DC")`  
Beide retourneren ons.

- - -
### <a name="error"></a>Fout
**Beschrijving:**  
De functie Error wordt gebruikt om een aangepaste fout geretourneerd.

**Syntaxis:**  
`void Error(str ErrorMessage)`

**Voorbeeld:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Als het kenmerk accountName niet aanwezig is, genereert u een fout op het object.

- - -
### <a name="escapedncomponent"></a>EscapeDNComponent
**Beschrijving:**  
De functie EscapeDNComponent duurt één onderdeel van een DN-naam en verlaat u zodat deze kan worden weergegeven in LDAP.

**Syntaxis:**  
`str EscapeDNComponent(str value)`

**Voorbeeld:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Zorgt ervoor dat het object kan worden gemaakt in een LDAP-adreslijst, zelfs als het kenmerk displayName bevat tekens die moeten worden voorafgegaan in LDAP.

- - -
### <a name="formatdatetime"></a>formatDateTime
**Beschrijving:**  
De functie FormatDateTime wordt gebruikt om een datum/tijd naar een tekenreeks met de opgegeven notatie

**Syntaxis:**  
`str FormatDateTime(dt value, str format)`

* waarde: een waarde in de datum-/ tijdindeling
* indeling: een de indeling converteren naar type string.

**Opmerkingen:**  
De mogelijke waarden voor de indeling vindt u hier: [door de gebruiker gedefinieerde datum-/ tijdnotaties (functie Format)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Voorbeeld:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
De resultaten in '2007-12-25'.

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Kan leiden tot '20140905081453.0Z'

- - -
### <a name="guid"></a>GUID
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
* waardeindienwaar: als de voorwaarde wordt geëvalueerd op true, wordt de geretourneerde waarde.
* WaardeAlsOnwaar: als de voorwaarde wordt geëvalueerd als onwaar, de geretourneerde waarde.

**Voorbeeld:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Als de gebruiker een intern is, retourneert de alias van een gebruiker met "t-' toegevoegd aan het begin van deze anders retourneert de alias van de gebruiker is.

- - -
### <a name="instr"></a>InStr
**Beschrijving:**  
De functie InStr wordt het eerste exemplaar van een subtekenreeks in een tekenreeks

**Syntaxis:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* reekscontroleren: tekenreeks die moet worden doorzocht
* reeksvergelijken: de tekenreeks die u wilt zoeken
* Start: beginpositie om te zoeken naar de subtekenreeks
* Vergelijk: vbTextCompare of vbBinaryCompare

**Opmerkingen:**  
Retourneert de positie waar de subtekenreeks is gevonden of 0 als dat niet is gevonden.

**Voorbeeld:**  
`InStr("The quick brown fox","quick")`  
Evalues tot en met 5

`InStr("repEated","e",3,vbBinaryCompare)`  
Evalueert tot en met 7

- - -
### <a name="instrrev"></a>InStrRev
**Beschrijving:**  
De functie InStrRev vindt het laatste exemplaar van een subtekenreeks in een tekenreeks

**Syntaxis:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* reekscontroleren: tekenreeks die moet worden doorzocht
* reeksvergelijken: de tekenreeks die u wilt zoeken
* Start: beginpositie om te zoeken naar de subtekenreeks
* Vergelijk: vbTextCompare of vbBinaryCompare

**Opmerkingen:**  
Retourneert de positie waar de subtekenreeks is gevonden of 0 als dat niet is gevonden.

**Voorbeeld:**  
`InStrRev("abbcdbbbef","bb")`  
Retourneert 7

- - -
### <a name="isbitset"></a>IsBitSet
**Beschrijving:**  
De functie IsBitSet netwerktests als een bit is ingesteld of niet

**Syntaxis:**  
`bool IsBitSet(num value, num flag)`

* waarde: een numerieke waarde die is evaluated.flag: een numerieke waarde met de bits moet worden geëvalueerd

**Voorbeeld:**  
`IsBitSet(&HF,4)`  
Retourneert True als omdat "4"-bit is ingesteld in de hexadecimale waarde 'F'

- - -
### <a name="isdate"></a>IsDate
**Beschrijving:**  
Als de expressie kan worden geëvalueerd als een DateTime-type, en vervolgens de functie IsDate in waar resulteert.

**Syntaxis:**  
`bool IsDate(var Expression)`

**Opmerkingen:**  
Gebruikt om te bepalen of CDate() uitgevoerd worden kan.

- - -
### <a name="iscert"></a>IsCert
**Beschrijving:**  
Retourneert waar als de onbewerkte gegevens kunnen worden geserialiseerd in .NET X509Certificate2 certificaat-object.

**Syntaxis:**  
`bool CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Byte matrix representatie van een X.509-certificaat. De bytematrix mag binair (DER), gecodeerd of Base64-gecodeerd x.509-gegevens.
- - -
### <a name="isempty"></a>IsEmpty
**Beschrijving:**  
Als het kenmerk aanwezig in de CS of MV is maar in een lege tekenreeks resulteert, klikt u vervolgens resulteert de functie IsEmpty in waar.

**Syntaxis:**  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a>IsGuid
**Beschrijving:**  
Als de tekenreeks kan worden geconverteerd naar een GUID, wordt de functie IsGuid geëvalueerd op true.

**Syntaxis:**  
`bool IsGuid(str GUID)`

**Opmerkingen:**  
Een GUID is gedefinieerd als een tekenreeks die een van deze patronen te volgen: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx of {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Gebruikt om te bepalen of CGuid() uitgevoerd worden kan.

**Voorbeeld:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Als de StrAttribute heeft een GUID-indeling, retourneert een binaire voorstelling, anders een Null geretourneerd.

- - -
### <a name="isnull"></a>IsNull
**Beschrijving:**  
Als de expressie wordt geëvalueerd op Null, klikt u vervolgens de functie IsNull true ' geretourneerd.

**Syntaxis:**  
`bool IsNull(var Expression)`

**Opmerkingen:**  
Voor een kenmerk is een null-waarde door het ontbreken van het kenmerk uitgedrukt.

**Voorbeeld:**  
`IsNull([displayName])`  
Retourneert waar als het kenmerk niet aanwezig in de CS of MV is.

- - -
### <a name="isnullorempty"></a>IsNullOrEmpty
**Beschrijving:**  
Als de expressie null of een lege tekenreeks is, klikt u vervolgens de functie IsNullOrEmpty true ' geretourneerd.

**Syntaxis:**  
`bool IsNullOrEmpty(var Expression)`

**Opmerkingen:**  
Voor een kenmerk, zou dit resulteren in waar als het kenmerk ontbreekt of aanwezig is maar een lege tekenreeks.  
De naam van de omgekeerde waarde van deze functie is IsPresent.

**Voorbeeld:**  
`IsNullOrEmpty([displayName])`  
Retourneert waar als het kenmerk niet aanwezig is of een lege tekenreeks in de CS of MV is.

- - -
### <a name="isnumeric"></a>IsNumeric
**Beschrijving:**  
De functie IsNumeric retourneert een Booleaanse waarde die aangeeft of een expressie kan worden geëvalueerd als een type.

**Syntaxis:**  
`bool IsNumeric(var Expression)`

**Opmerkingen:**  
Gebruikt om te bepalen of CNum() kan worden uitgevoerd om te parseren van de expressie.

- - -
### <a name="isstring"></a>IsString
**Beschrijving:**  
Als de expressie kan worden geëvalueerd op het type string, klikt u vervolgens resulteert de functie IsString in waar.

**Syntaxis:**  
`bool IsString(var expression)`

**Opmerkingen:**  
Gebruikt om te bepalen of CStr() kan worden uitgevoerd om te parseren van de expressie.

- - -
### <a name="ispresent"></a>IsPresent
**Beschrijving:**  
Als de expressie in een tekenreeks die niet Null is en niet leeg is resulteert, klikt u vervolgens de functie IsPresent true ' geretourneerd.

**Syntaxis:**  
`bool IsPresent(var expression)`

**Opmerkingen:**  
De naam van de omgekeerde waarde van deze functie is IsNullOrEmpty.

**Voorbeeld:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a>Item
**Beschrijving:**  
De functie Item retourneert één item van een tekenreeks/kenmerk met meerdere waarden.

**Syntaxis:**  
`var Item(mvstr attribute, num index)`

* kenmerk: kenmerk met meerdere waarden
* index: index van een item in de tekenreeks met meerdere waarden.

**Opmerkingen:**  
De functie Item is nuttig in combinatie met de functie bevat sinds de laatste functie de index op een item in het kenmerk met meerdere waarden retourneert.

Genereert een fout als de index valt buiten het bereik.

**Voorbeeld:**  
`Mid(Item([proxyAddresses],Contains([proxyAddresses], "SMTP:")),6)`  
Retourneert het primaire e-mailadres.

- - -
### <a name="itemornull"></a>ItemOrNull
**Beschrijving:**  
De functie ItemOrNull retourneert één item van een tekenreeks/kenmerk met meerdere waarden.

**Syntaxis:**  
`var ItemOrNull(mvstr attribute, num index)`

* kenmerk: kenmerk met meerdere waarden
* index: index van een item in de tekenreeks met meerdere waarden.

**Opmerkingen:**  
De functie ItemOrNull is nuttig in combinatie met de functie bevat sinds de laatste functie de index op een item in het kenmerk met meerdere waarden retourneert.

Als de index valt buiten het bereik, retourneert een Null-waarde.

- - -
### <a name="join"></a>Koppelen
**Beschrijving:**  
De Join-functie een tekenreeks met meerdere waarden en retourneert een tekenreeks met één waarde met het opgegeven scheidingsteken ingevoegd tussen de verschillende items.

**Syntaxis:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* kenmerk: kenmerk met meerdere waarden met tekenreeksen worden toegevoegd.
* scheidingsteken: een willekeurige tekenreeks, gebruikt voor het scheiden van de subtekenreeksen in de tekenreeks geretourneerd. Als u dit weglaat, wordt het spatieteken ("") wordt gebruikt. Als scheidingsteken een tekenreeks met lengte nul is ("") of niets, alle items in de lijst worden samengevoegd met zonder scheidingstekens.

**Opmerkingen**  
Er is pariteit tussen de functies voor lid worden en splitsen. De Join-functie wordt een matrix met tekenreeksen en koppelt deze met een scheidingsteken tekenreeks, een één-tekenreeks geretourneerd. De Split-functie een tekenreeks en afscheiding op het scheidingsteken, om te retourneren van een matrix met tekenreeksen. Een belangrijk verschil is echter dat Join tekenreeksen met een willekeurige tekenreeks scheidingsteken kunt samenvoegen, gesplitste kunt alleen tekenreeksen met behulp van een enkel teken scheidingsteken scheiden.

**Voorbeeld:**  
`Join([proxyAddresses],",")`  
Kan retourneren: 'SMTP:john.doe@contoso.com,smtp:jd@contoso.com'

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

* tekenreeks: de tekenreeks om terug te keren tekens bevatten uit
* NumChars: een nummer voor het aantal tekens retourneren vanaf het begin (links) van een tekenreeks

**Opmerkingen:**  
Een tekenreeks met de eerste numChars tekens in de tekenreeks:

* Als numChars = 0, lege tekenreeks retourneren.
* Als numChars < 0, invoerreeks retourneren.
* Als de tekenreeks is null, leeg tekenreeks geretourneerd.

Als de tekenreeks bevat minder tekens dan het aantal opgegeven in numChars, wordt een tekenreeks die identiek is aan de tekenreeks (d.w.z., met alle tekens in parameter 1) geretourneerd.

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

* tekenreeks: de tekenreeks om terug te keren tekens bevatten uit
* Start: een nummer voor de eerste positie in de tekenreeks om terug te keren tekens bevatten uit
* NumChars: een nummer voor het aantal tekens retourneren vanaf positie in de tekenreeks

**Opmerkingen:**  
Retour numChars tekens vanaf positie start in de tekenreeks.  
Een tekenreeks met numChars tekens vanaf het begin van de positie in de tekenreeks:

* Als numChars = 0, lege tekenreeks retourneren.
* Als numChars < 0, invoerreeks retourneren.
* Als start > de lengte van tekenreeks, invoerreeks retourneren.
* Als starten < = 0, invoer-tekenreeks geretourneerd.
* Als de tekenreeks is null, leeg tekenreeks geretourneerd.

Als er niet numChar tekens worden resterend in de tekenreeks van positie-begin zoveel tekens mogelijk geretourneerd.

**Voorbeeld:**  
`Mid("John Doe", 3, 5)`  
Retourneert 'hn komen'.

`Mid("John Doe", 6, 999)`  
Retourneert 'De Vries'

- - -
### <a name="now"></a>Nu
**Beschrijving:**  
De functie nu retourneert een datetime-waarde opgeven van de huidige datum en tijd, volgens de systeemdatum en -tijd van de computer.

**Syntaxis:**  
`dt Now()`

- - -
### <a name="numfromdate"></a>NumFromDate
**Beschrijving:**  
De functie NumFromDate retourneert een datum in de AD-datumnotatie.

**Syntaxis:**  
`num NumFromDate(dt value)`

**Voorbeeld:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Retourneert 129699324000000000

- - -
### <a name="padleft"></a>PadLeft
**Beschrijving:**  
De PadLeft functie links-pad een tekenreeks aan een opgegeven lengte met behulp van een opgegeven opvulteken.

**Syntaxis:**  
`str PadLeft(str string, num length, str padCharacter)`

* tekenreeks: de tekenreeks worden opgevuld.
* lengte: een geheel getal dat de gewenste lengte van tekenreeks.
* padCharacter: een tekenreeks die bestaat uit een enkel teken te gebruiken als pad teken

**Opmerkingen:**

* Als de lengte van tekenreeks kleiner dan de lengte is, klikt u vervolgens padCharacter herhaaldelijk toegevoegd aan het begin (links) van een tekenreeks totdat er een lengte gelijk zijn aan lengte.
* padCharacter mag een spatie, maar dit kan een null-waarde niet.
* Als de lengte van tekenreeks gelijk aan of groter dan de lengte is, wordt de tekenreeks ongewijzigd geretourneerd.
* Als de tekenreeks een lengte groter dan of gelijk zijn aan lengte heeft, wordt een tekenreeks die identiek is aan de tekenreeks geretourneerd.
* Als de lengte van tekenreeks kleiner dan de lengte is, wordt een nieuwe tekenreeks van de gewenste lengte met tekenreeks opgevuld met een padCharacter geretourneerd.
* Als null-tekenreeks is, retourneert de functie een lege tekenreeks.

**Voorbeeld:**  
`PadLeft("User", 10, "0")`  
Retourneert '000000User'.

- - -
### <a name="padright"></a>PadRight
**Beschrijving:**  
De PadRight functie rechts-pad een tekenreeks aan een opgegeven lengte met behulp van een opgegeven opvulteken.

**Syntaxis:**  
`str PadRight(str string, num length, str padCharacter)`

* tekenreeks: de tekenreeks worden opgevuld.
* lengte: een geheel getal dat de gewenste lengte van tekenreeks.
* padCharacter: een tekenreeks die bestaat uit een enkel teken te gebruiken als pad teken

**Opmerkingen:**

* Als de lengte van tekenreeks kleiner dan de lengte is, is klikt u vervolgens padCharacter herhaaldelijk toegevoegd aan het einde (rechts) van een tekenreeks totdat er een lengte die gelijk zijn aan lengte.
* padCharacter mag een spatie, maar dit kan een null-waarde niet.
* Als de lengte van tekenreeks gelijk aan of groter dan de lengte is, wordt de tekenreeks ongewijzigd geretourneerd.
* Als de tekenreeks een lengte groter dan of gelijk zijn aan lengte heeft, wordt een tekenreeks die identiek is aan de tekenreeks geretourneerd.
* Als de lengte van tekenreeks kleiner dan de lengte is, wordt een nieuwe tekenreeks van de gewenste lengte met tekenreeks opgevuld met een padCharacter geretourneerd.
* Als null-tekenreeks is, retourneert de functie een lege tekenreeks.

**Voorbeeld:**  
`PadRight("User", 10, "0")`  
Retourneert 'User000000'.

- - -
### <a name="pcase"></a>PCase
**Beschrijving:**  
De functie PCase converteert het eerste teken van elk woord door spaties gescheiden in een tekenreeks naar hoofdletters en alle overige tekens worden omgezet in kleine letters.

**Syntaxis:**  
`String PCase(string)`

**Opmerkingen:**

* Deze functie biedt momenteel geen juiste hoofdlettergebruik als een woord dat volledig hoofdletters, zoals een acroniem wilt converteren.

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

* Start: een getal dat aangeeft van de ondergrens van een willekeurige waarde genereren
* end: een getal dat aangeeft van de bovengrens van een willekeurige waarde genereren

**Voorbeeld:**  
`Random(100,999)`  
734 kunnen worden geretourneerd.

- - -
### <a name="removeduplicates"></a>RemoveDuplicates
**Beschrijving:**  
De functie RemoveDuplicates heeft van een tekenreeks met meerdere waarden en zorg ervoor dat elke waarde uniek is.

**Syntaxis:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Voorbeeld:**  
`RemoveDuplicates([proxyAddresses])`  
Retourneert een kenmerk opgeschoonde proxyAddress waar alle dubbele waarden zijn verwijderd.

- - -
### <a name="replace"></a>Vervangen
**Beschrijving:**  
De functie vervangen vervangt alle instanties van een tekenreeks naar een andere tekenreeks.

**Syntaxis:**  
`str Replace(str string, str OldValue, str NewValue)`

* tekenreeks: een waarde voor het vervangen van waarden in.
* OldValue: De tekenreeks om naar te zoeken en vervangen.
* NewValue: De tekenreeks te vervangen.

**Opmerkingen:**  
De functie herkent de volgende speciale monikers:

* \n – nieuwe regel
* \r – regelterugloop
* \t – tabblad

**Voorbeeld:**  
`Replace([address],"\r\n",", ")`  
CRLF vervangt door een komma en een spatie en kan leiden tot "One Microsoft manier, Redmond, WA, VS"

- - -
### <a name="replacechars"></a>ReplaceChars
**Beschrijving:**  
De functie ReplaceChars vervangt alle instanties van de tekens in de tekenreeks ReplacePattern zijn gevonden.

**Syntaxis:**  
`str ReplaceChars(str string, str ReplacePattern)`

* tekenreeks: een tekenreeks op voor tekens in vervangen.
* ReplacePattern: een tekenreeks met een woordenlijst met de tekens wilt vervangen.

De indeling is {bron1}: {target1}, {bron2}: {target2}, {bronN}, {targetN} waarbij bron is het teken om te zoeken en doel van de tekenreeks die moet worden vervangen.

**Opmerkingen:**

* De functie heeft elk exemplaar van de gedefinieerde bronnen en de doelen vervangen.
* De bron moet precies één teken voor (unicode).
* De bron kan niet leeg zijn of langer is dan één teken (parseerfout).
* Het doel kan meerdere tekens, bijvoorbeeld ö:oe, β:ss hebben.
* Het doel mag leeg die aangeeft dat het teken moet worden verwijderd.
* De bron is hoofdlettergevoelig en moet een exacte overeenkomst.
* De (door komma's) en: (dubbele punt) zijn gereserveerde tekens en met deze functie kan niet worden vervangen.
* Spaties en andere witte tekens in de ReplacePattern tekenreeks worden genegeerd.

**Voorbeeld:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Retourneert Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Retourneert 'ONeil', de één maatstreepjes wordt gedefinieerd moeten worden verwijderd.

- - -
### <a name="right"></a>Rechts
**Beschrijving:**  
De juiste functie retourneert een opgegeven aantal tekens vanaf de rechterkant (end) van een tekenreeks.

**Syntaxis:**  
`str Right(str string, num NumChars)`

* tekenreeks: de tekenreeks om terug te keren tekens bevatten uit
* NumChars: een nummer voor het aantal tekens retourneren vanaf het einde (rechts) van een tekenreeks

**Opmerkingen:**  
NumChars tekens geretourneerd vanaf de laatste positie van een tekenreeks.

Een tekenreeks met de laatste numChars tekens in de tekenreeks:

* Als numChars = 0, lege tekenreeks retourneren.
* Als numChars < 0, invoerreeks retourneren.
* Als de tekenreeks is null, leeg tekenreeks geretourneerd.

Als de tekenreeks bevat minder tekens dan het aantal opgegeven in NumChars, wordt een tekenreeks die identiek is aan de tekenreeks geretourneerd.

**Voorbeeld:**  
`Right("John Doe", 3)`  
Retourneert 'De Vries'.

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
Proces alle waarden in een met meerdere waarden kenmerk (of uitvoer van een expressie) op basis van functie die is opgegeven.

**Syntaxis:**  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* item: Hiermee geeft u een element in het kenmerk met meerdere waarden
* kenmerk: het kenmerk met meerdere waarden
* expressie: een expressie die een verzameling waarden retourneert
* voorwaarde: de functie waarmee een item in het kenmerk kan worden verwerkt.

**Voorbeelden:**  
`Select($item,[otherPhone],Replace($item,"-",""))`  
Alle waarden in het kenmerk met meerdere waarden otherPhone retourneren nadat afbreekstreepjes (-) zijn verwijderd.

- - -
### <a name="split"></a>Splitsen
**Beschrijving:**  
De Split-functie een tekenreeks die van elkaar gescheiden door een scheidingsteken en maakt het een tekenreeks met meerdere waarden.

**Syntaxis:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* waarde: de tekenreeks met een scheidingsteken te scheiden.
* scheidingsteken: willekeurig teken als het scheidingsteken moet worden gebruikt.
* limiet: maximum aantal waarden die kunnen worden geretourneerd.

**Voorbeeld:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Retourneert een tekenreeks met meerdere waarden met 2 elementen nuttig voor het kenmerk proxyAddress.

- - -
### <a name="stringfromguid"></a>StringFromGuid
**Beschrijving:**  
De functie StringFromGuid duurt een binaire GUID en converteert deze naar een tekenreeks

**Syntaxis:**  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a>StringFromSid
**Beschrijving:**  
De functie StringFromSid converteert een bytematrix die een beveiligings-id in een tekenreeks.

**Syntaxis:**  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a>Switch
**Beschrijving:**  
De functie Switch wordt gebruikt om op basis van de geëvalueerde voorwaarden één waarde retourneren.

**Syntaxis:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* expr: Variant-expressie die u wilt evalueren.
* waarde: waarde wordt geretourneerd als de bijbehorende expressie waar is.

**Opmerkingen:**  
Lijst met argumenten functie Switch bestaat uit paren van expressies en waarden. De expressies worden geëvalueerd van links naar rechts en de waarde die is gekoppeld aan de eerste expressie te laten resulteren in True wordt geretourneerd. Als de onderdelen zijn niet correct is gekoppeld, wordt een runtime fout optreedt.

Als expr1 True is, retourneert Switch value1. Als expr-1 ingesteld op False is, maar expr 2 True is, retourneert Switch waarde 2, enzovoort.

Levert een switch als:

* Geen van de expressies is True.
* De eerste expressie van de waarde True heeft een overeenkomende waarde die Null is.

Switch evalueert alle expressies, ook al wordt slechts één van beide geretourneerd. Daarom moet u neveneffecten gecontroleerd. Als de evaluatie van een expressie in een deling door nul-fout resulteert, wordt bijvoorbeeld een fout optreedt.

Waarde kan ook worden de functie fout, die een aangepaste tekenreeks geretourneerd.

**Voorbeeld:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Retourneert de taal gesproken in sommige steden, anders wordt een fout geretourneerd.

- - -
### <a name="trim"></a>Knippen
**Beschrijving:**  
De functie spaties verwijdert voorloopspaties en afsluitende spaties uit een tekenreeks.

**Syntaxis:**  
`str Trim(str value)`  

**Voorbeeld:**  
`Trim(" Test ")`  
Retourneert 'Test'.

`Trim([proxyAddresses])`  
Verwijdert voorloopspaties en afsluitende spaties voor elke waarde in het kenmerk proxyAddress.

- - -
### <a name="ucase"></a>UCase
**Beschrijving:**  
De functie UCase converteert alle tekens in een tekenreeks naar hoofdletters.

**Syntaxis:**  
`str UCase(str string)`

**Voorbeeld:**  
`UCase("TeSt")`  
Retourneert 'TEST'.

- - -
### <a name="where"></a>Waar

**Beschrijving:**  
Retourneert een subset van de waarden van een met meerdere waarden kenmerk (of dezelfde uitvoer van een expressie) op basis van specifieke voorwaarde.

**Syntaxis:**  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* item: Hiermee geeft u een element in het kenmerk met meerdere waarden
* kenmerk: het kenmerk met meerdere waarden
* voorwaarde: een expressie die kan worden geëvalueerd op true of false
* expressie: een expressie die een verzameling waarden retourneert

**Voorbeeld:**  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
Retourneert de waarden van het certificaat in het kenmerk met meerdere waarden userCertificate die niet zijn verlopen.

- - -
### <a name="with"></a>Met
**Beschrijving:**  
De With-functie biedt een manier voor het vereenvoudigen van een samengestelde expressie met behulp van een variabele om weer te geven van een subexpressie die wordt weergegeven een of meer keer in de complexe expressie.

**Syntaxis:**
`With(var variable, exp subExpression, exp complexExpression)`  
* variabele: Hiermee geeft u de subexpressie.
* Subexpressie: subexpressie dat wordt vertegenwoordigd door de variabele.
* complexExpression: een complexe expressie.

**Voorbeeld:**  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
Is functioneel equivalent met:  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
Die alleen niet-verlopen certificaat waarden resulteert in het kenmerk userCertificate.


- - -
### <a name="word"></a>Word
**Beschrijving:**  
De functie woord retourneert een woord dat voorkomt in een tekenreeks, op basis van parameters met een beschrijving van de scheidingstekens moet gebruiken en het aantal word om terug te keren.

**Syntaxis:**  
`str Word(str string, num WordNumber, str delimiters)`

* tekenreeks: de tekenreeks om te retourneren van een woord uit.
* WordNumber: een getal dat aangeeft welke word-nummer moet worden geretourneerd.
* scheidingstekens: een tekenreeks voor de delimiter(s) die moeten worden gebruikt om te identificeren woorden

**Opmerkingen:**  
Elke reeks tekens in tekenreeks gescheiden door een van de tekens in scheidingstekens worden aangeduid als woorden:

* Als < 1 nummer, het resultaat een lege tekenreeks.
* Als tekenreeks null is, retourneert lege tekenreeks.

Als de tekenreeks bevat minder dan het aantal woorden of tekenreeks bevat geen woorden geïdentificeerd door scheidingstekens, wordt een lege tekenreeks geretourneerd.

**Voorbeeld:**  
`Word("The quick brown fox",3," ")`  
Retourneert 'bruine'

`Word("This,string!has&many separators",3,",!&#")`  
Retourneert 'is'

## <a name="additional-resources"></a>Aanvullende resources
* [Inzicht in expressies declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD Connect-synchronisatie: Synchronisatie-opties voor aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
