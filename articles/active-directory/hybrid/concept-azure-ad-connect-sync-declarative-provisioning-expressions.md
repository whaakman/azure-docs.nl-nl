---
title: 'Azure AD Connect: Declaratieve Inrichtingsexpressies | Microsoft Docs'
description: De declaratieve inrichtingsexpressies uitgelegd.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 26c835e6c2f658f7ad852fdd02dc8974db33e47b
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46312065"
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect-synchronisatie: inzicht in declaratieve inrichting expressies
Azure AD Connect-synchronisatie is gebaseerd op het declaratieve inrichting is geïntroduceerd in Forefront Identity Manager 2010. Hiermee kunt u voor het implementeren van uw volledige identity integration bedrijfslogica zonder de noodzaak gecompileerde code te schrijven.

Een essentieel onderdeel van de declaratieve inrichting wordt de expressietaal die wordt gebruikt in kenmerkstromen. De gebruikte taal is een subset van Microsoft Visual Basic® for Applications (VBA). Deze taal wordt gebruikt in Microsoft Office en ook door gebruikers met de ervaring van VBScript worden herkend. De declaratieve inrichting expressietaal wordt alleen gebruikt voor functies en is niet een gestructureerde taal. Er zijn geen methoden of instructies. Functies zijn in plaats daarvan naar snelle programmastroom genest.

Zie voor meer informatie, [Welkom bij de Visual Basic for Applications taalverwijzing voor Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

De kenmerken zijn sterk getypeerd. Een functie accepteert alleen de kenmerken van het juiste type. Het is ook hoofdlettergevoelig. Zowel de functienamen van de en kenmerk moet juiste hoofdlettergebruik of een fout gegenereerd.

## <a name="language-definitions-and-identifiers"></a>Definities van de taal en id 's
* Functies hebben een naam, gevolgd door argumenten tussen vierkante haken: functienaam (1 argument, argument N).
* Kenmerken worden geïdentificeerd door de vierkante haken: [attributeName]
* Parameters worden aangeduid met procenttekens: % ParameterName %
* Tekenreeksconstanten worden omringd door aanhalingstekens: bijvoorbeeld 'Contoso' (Opmerking: rechte aanhalingstekens "' en niet slimme aanhalingstekens" ")
* Numerieke waarden worden uitgedrukt zonder aanhalingstekens en verwacht decimaal. Hexadecimale waarden worden voorafgegaan door & h Bijvoorbeeld, 98052 & HFF
* Booleaanse waarden worden uitgedrukt met constanten: True, False.
* Ingebouwde constanten en letterlijke waarden worden uitgedrukt met alleen de naam: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>Functions
Declaratieve inrichting maakt gebruik van veel functies waarmee de mogelijkheid om te zetten van kenmerkwaarden. Deze functies kunnen worden genest, zodat het resultaat van een functie aan een andere functie doorgegeven.

`Function1(Function2(Function3()))`

De volledige lijst met functies vindt u de [functie verwijzing](reference-connect-sync-functions-reference.md).

### <a name="parameters"></a>Parameters
Een parameter wordt gedefinieerd door een Connector of door een beheerder met behulp van PowerShell. Parameters bevatten gewoonlijk waarden die verschillen van systeem, bijvoorbeeld de naam van het domein de gebruiker zich bevindt in. Deze parameters kunnen worden gebruikt in kenmerkstromen.

De Active Directory-Connector die de volgende parameters voor binnenkomende synchronisatieregels:

| Parameternaam | Opmerking |
| --- | --- |
| Domain.Netbios |NetBIOS-indeling van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld FABRIKAMSALES |
| Domain.FQDN |FQDN-indeling van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld sales.fabrikam.com |
| Domain.LDAP |LDAP-indeling van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld DC = verkoop, DC = fabrikam, DC = com |
| Forest.Netbios |NetBIOS-indeling van de naam van het forest dat momenteel wordt geïmporteerd, bijvoorbeeld FABRIKAMCORP |
| Forest.FQDN |FQDN-indeling van de naam van het forest dat momenteel wordt geïmporteerd, bijvoorbeeld fabrikam.com |
| Forest.LDAP |LDAP-indeling van de naam van het forest dat momenteel wordt geïmporteerd, bijvoorbeeld DC = fabrikam, DC = com |

Het systeem biedt de volgende parameter, die wordt gebruikt om op te halen van de id van de Connector die momenteel wordt uitgevoerd:  
`Connector.ID`

Hier volgt een voorbeeld waarin vult het domein van de metaverse-kenmerk met de netbios-naam van het domein waar de gebruiker zich bevindt:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operators
De volgende operators kunnen worden gebruikt:

* **Vergelijking**: <, < =, <>, =, >, > =
* **Wiskunde**: +, -, \*, -
* **Tekenreeks**: & (samenvoegen)
* **Logische**: & & (en) || (of)
* **Evaluatievolgorde**:)

Operators links naar rechts worden geëvalueerd en dezelfde evaluatie prioriteit hebben. Dat wil zeggen, de \* (vermenigvuldiger) wordt niet geëvalueerd vóór - (aftrekken). 2\*(5 + 3) is niet gelijk zijn aan 2\*5 + 3. De vierkante haken () worden gebruikt om de evaluatievolgorde wijzigen wanneer links naar rechts evaluatieorder niet juist.

## <a name="multi-valued-attributes"></a>Kenmerken met meerdere waarden
De functies kunnen worden uitgevoerd op zowel één waarde als meerwaardige kenmerken. Voor kenmerken met meerdere waarden, de functie werkt via elke waarde en dezelfde functie is van toepassing op elke waarde.

Bijvoorbeeld:  
`Trim([proxyAddresses])` Voer een Trim van elke waarde in het proxyAddress-kenmerk.  
`Word([proxyAddresses],1,"@") & "@contoso.com"` Voor elke waarde met een @-sign, vervangt u het domein met @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])` Het SIP-adres zoekt en verwijdert u het uit de waarden.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de configuratiemodel in [Understanding declaratieve inrichting](concept-azure-ad-connect-sync-declarative-provisioning.md).
* Zie hoe declaratieve inrichting is gebruikte out-of-box in [inzicht in de standaardconfiguratie](concept-azure-ad-connect-sync-default-configuration.md).
* Zien hoe u een praktische wijzigen met behulp van declaratieve inrichting [hoe u een wijziging in de standaardconfiguratie](how-to-connect-sync-change-the-configuration.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht in en synchronisatie aanpassen](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)

**Onderwerpen met naslaginformatie**

* [Azure AD Connect-synchronisatie: functieverwijzing](reference-connect-sync-functions-reference.md)

