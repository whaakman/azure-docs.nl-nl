---
title: 'Azure AD Connect: Expressies declaratieve inrichting | Microsoft Docs'
description: Verklaart de declaratieve inrichting expressies.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 797c0949aceea415652a72df5ee23ef9888ab975
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect-synchronisatie: inzicht declaratieve inrichting expressies
Azure AD Connect-synchronisatie is gebaseerd op het declaratieve inrichting is geïntroduceerd in Forefront Identity Manager 2010. Hiermee kunt u voor het implementeren van uw identiteit van de volledige integratie bedrijfslogica zonder de noodzaak om gecompileerde code te schrijven.

Een essentieel onderdeel van de declaratieve inrichting is de expressie die wordt gebruikt in kenmerkstromen. De gebruikte taal is een subset van Microsoft Visual Basic® for Applications (VBA). Deze taal wordt gebruikt in Microsoft Office en ook door gebruikers met de ervaring van VBScript wordt herkend. De declaratieve inrichting expressietaal wordt alleen gebruikt voor functies en is niet een gestructureerde taal. Er zijn geen methoden of instructies. Functies worden in plaats daarvan aan snelle programma flow genest.

Zie voor meer informatie [Welkom bij de Visual Basic for Applications-taalreferentie voor Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

De kenmerken zijn sterk getypeerd. Een functie accepteert alleen de kenmerken van het juiste type. Het is ook hoofdlettergevoelig. Zowel functienamen en kenmerknamen moeten juiste hoofdlettergebruik of een fout gegenereerd.

## <a name="language-definitions-and-identifiers"></a>Taal definities en id 's
* Functies hebben een naam die wordt gevolgd door argumenten tussen vierkante haken: functienaam (argument 1, argument N).
* Kenmerken worden geïdentificeerd door vierkante haken: [attributeName]
* Parameters zijn geïdentificeerd door procenttekens: % ParameterName %
* Tekenreeksconstanten tussen aanhalingstekens worden geplaatst: bijvoorbeeld 'Contoso' (Opmerking: rechte aanhalingstekens moet gebruiken "" en niet slimme aanhalingstekens "")
* Numerieke waarden die zijn uitgedrukt zonder aanhalingstekens en decimale verwacht. Hexadecimale waarden worden voorafgegaan door & H. Bijvoorbeeld, 98052 & HFF
* Boole-waarden worden uitgedrukt met constanten: True, False.
* Ingebouwde constanten en letterlijke waarden worden uitgedrukt met alleen de naam: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>Functies
Declaratieve inrichting veel functies gebruikt om in te schakelen, de mogelijkheid voor het transformeren van kenmerkwaarden. Deze functies kunnen worden genest, zodat het resultaat van een functie aan een andere functie doorgegeven.

`Function1(Function2(Function3()))`

De volledige lijst met functies vindt u in de [werken verwijzing](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parameters
Een parameter is gedefinieerd door een Connector of door een beheerder met behulp van PowerShell. Parameters meestal bevatten waarden die verschillen van systeem, bijvoorbeeld de naam van het domein de gebruiker zich bevindt in. Deze parameters kunnen worden gebruikt in kenmerkstromen.

De volgende parameters beschikbaar de Active Directory-Connector voor binnenkomende synchronisatieregels:

| Parameternaam | Opmerking |
| --- | --- |
| Domain.Netbios |NetBIOS-indeling van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld FABRIKAMSALES |
| Domain.FQDN |FQDN-indeling van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld sales.fabrikam.com |
| Domain.LDAP |LDAP-indeling van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld DC = verkoop, DC = fabrikam, DC = com |
| Forest.Netbios |NetBIOS-indeling van de naam van het forest dat momenteel wordt geïmporteerd, bijvoorbeeld FABRIKAMCORP |
| Forest.FQDN |FQDN-indeling van de naam van het forest dat momenteel wordt geïmporteerd, bijvoorbeeld fabrikam.com |
| Forest.LDAP |LDAP-indeling van de naam van het forest dat momenteel wordt geïmporteerd, bijvoorbeeld DC = fabrikam, DC = com |

Het systeem biedt de volgende parameter die wordt gebruikt voor het ophalen van de id van de Connector die momenteel worden uitgevoerd:  
`Connector.ID`

Hier volgt een voorbeeld waarin vult het domein van de metaverse-kenmerk met de netbios-naam van het domein waarin de gebruiker zich bevindt:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operators
De volgende operators kunnen worden gebruikt:

* **Vergelijking**: <, < =, <>, =, >, > =
* **Wiskunde**: +, -, \*, -
* **Tekenreeks**: & (samenvoegen)
* **Logische**: & & (en) || (of)
* **Evaluatievolgorde**:)

Operators links naar rechts worden geëvalueerd en dezelfde evaluatie prioriteit hebben. Dat wil zeggen, de \* (vermenigvuldiger) wordt niet geëvalueerd vóór - (aftrekken). 2\*(5 + 3) is niet gelijk zijn aan 2\*5 + 3. De vierkante haken () worden gebruikt voor de evaluatievolgorde wijzigen wanneer van links naar rechts evaluatievolgorde niet juist.

## <a name="multi-valued-attributes"></a>Kenmerken met meerdere waarden
De functies kunnen van zowel één waarde als meerdere waarden kenmerken werken. Voor kenmerken met meerdere waarden, de functie werkt via elke waarde en dezelfde functie geldt voor elke waarde.

Bijvoorbeeld:  
`Trim([proxyAddresses])`Een ' trim ' van elke waarde in het kenmerk proxyAddress doen.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Voor elke waarde met een @-sign, vervangt u het domein met @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Voor het SIP-adres zoeken en verwijderen van de waarden.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de configuratiemodel in [Understanding declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Zie hoe declaratieve inrichting is gebruikte out of box in [inzicht in de standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md).
* Informatie over het maken van een praktische wijzigen met behulp van declaratieve inrichting [hoe een wijziging aanbrengt in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

**Onderwerpen met naslaginformatie**

* [Azure AD Connect-synchronisatie: functieverwijzing](active-directory-aadconnectsync-functions-reference.md)

