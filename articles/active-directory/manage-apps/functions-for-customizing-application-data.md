---
title: Expressies schrijven voor kenmerktoewijzingen in Azure Active Directory | Microsoft Docs
description: Informatie over het gebruik van expressietoewijzingen kenmerkwaarden omzetten in een acceptabele indeling tijdens de geautomatiseerde inrichting van objecten van de SaaS-app in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: barbkess
ms.openlocfilehash: d8e390fc185c3cb0b63bcea56feb4b133652673d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258830"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Expressies schrijven voor kenmerktoewijzingen in Azure Active Directory
Bij het configureren van inrichting tot een SaaS-toepassing, is een van de typen kenmerktoewijzingen die u kunt opgeven een expressie-toewijzing. Voor deze, moet u een script-achtige-expressie waarmee u uw gebruikers om gegevens te transformeren naar indelingen die meer geschikt is voor de SaaS-toepassing kunt schrijven.

## <a name="syntax-overview"></a>Overzicht van de syntaxis
De syntaxis voor expressies voor kenmerktoewijzingen is doet denken aan van Visual Basic voor toepassingen (VBA)-functies.

* De volledige expressie moet worden gedefinieerd in termen van functies, die bestaan uit een naam, gevolgd door argumenten tussen haakjes: <br>
  *Functienaam (<< argument 1 >> <<argument N>>)*
* Functies in elkaar kan worden genest. Bijvoorbeeld: <br> *FunctionOne (FunctionTwo (<<argument1>>))*
* U kunt drie verschillende typen argumenten doorgeven in functies:
  
  1. Kenmerken moeten tussen rechte haakjes worden geplaatst. Bijvoorbeeld: [attributeName]
  2. Tekenreeksconstanten moeten tussen dubbele aanhalingstekens worden geplaatst. Bijvoorbeeld: "VS"
  3. Andere functies. Bijvoorbeeld: FunctionOne (<<argument1>>, FunctionTwo (<<argument2>>))
* Voor tekenreeksconstanten, als u een backslash (\) of een aanhalingsteken (") in de tekenreeks, moet moet deze worden voorafgegaan door het symbool backslash (\). Bijvoorbeeld: "de naam van bedrijf: \"Contoso\""

## <a name="list-of-functions"></a>Lijst met functies
[Toevoeg-](#append) &nbsp; &nbsp; &nbsp; &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; &nbsp; [Join](#join) &nbsp; &nbsp; &nbsp; &nbsp; [Mid](#mid) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [NormalizeDiacritics](#normalizediacritics) [niet](#not) &nbsp; &nbsp; &nbsp; &nbsp; [vervangen](#replace) &nbsp; &nbsp; &nbsp; &nbsp; [SingleAppRoleAssignment](#singleapproleassignment) &nbsp; &nbsp; &nbsp; &nbsp; [StripSpaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; [Switch](#switch)

- - -
### <a name="append"></a>Toevoegen
**Functie:**<br> Append(Source, suffix)

**Beschrijving:**<br> Neemt een tekenreekswaarde bron en het achtervoegsel toegevoegd aan het einde van deze.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **Bron** |Vereist |Reeks |Doorgaans de naam van het kenmerk van het bronobject |
| **suffix** |Vereist |Reeks |De tekenreeks die u wilt toevoegen aan het einde van de bronwaarde. |

- - -
### <a name="formatdatetime"></a>formatDateTime
**Functie:**<br> FormatDateTime (bron, inputFormat, uitvoerindeling)

**Beschrijving:**<br> Neemt een tekenreeks met datum van de ene indeling en converteert naar een andere indeling.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **Bron** |Vereist |Reeks |Doorgaans de naam van het kenmerk van het bronobject. |
| **inputFormat** |Vereist |Reeks |De verwachte notatie van de bronwaarde. Zie voor ondersteunde indelingen [ http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx ](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **Uitvoerindeling** |Vereist |Reeks |Indeling van de uitvoerdatum. |

- - -
### <a name="join"></a>Koppelen
**Functie:**<br> Deelnemen aan (scheidingsteken, bron1, bron2,...)

**Beschrijving:**<br> Join() is vergelijkbaar met Append(), behalve dat deze meerdere kunt combineren **bron** tekenreeks waarden in één tekenreeks en elke waarde wordt gescheiden door een **scheidingsteken** tekenreeks.

Als een van de bronwaarden is een kenmerk meerdere waarden wordt elke waarde in dit kenmerk wordt samengevoegd, van de waarde van het scheidingsteken gescheiden.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **separator** |Vereist |Reeks |De tekenreeks die wordt gebruikt om de bronwaarden scheiden wanneer ze worden samengevoegd tot één tekenreeks. Kan ' ' als er geen scheidingsteken vereist is. |
| ** bron1... bronN ** |Vereist, variabele-aantal keren |Reeks |De tekenreeks die waarden die moeten worden samengevoegd. |

- - -
### <a name="mid"></a>Mid
**Functie:**<br> Mid (bron, start, lengte)

**Beschrijving:**<br> Retourneert een subtekenreeks van de bronwaarde. Een subtekenreeks is een tekenreeks zijn met slechts enkele van de tekens uit de brontekenreeks.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **Bron** |Vereist |Reeks |Doorgaans de naam van het kenmerk. |
| **start** |Vereist |geheel getal |Indexeren de **bron** tekenreeks waar de subtekenreeks moet beginnen. Het eerste teken in de tekenreeks index 1 hebben, tweede teken wordt index 2 hebben, enzovoort. |
| **Lengte** |Vereist |geheel getal |De lengte van de subtekenreeks. Als de lengte buiten eindigt de **bron** tekenreeks, functie retourneert de subtekenreeks uit **start** index tot het einde van **bron** tekenreeks. |

- - -
### <a name="normalizediacritics"></a>NormalizeDiacritics
**Functie:**<br> NormalizeDiacritics(source)

**Beschrijving:**<br> Vereist een tekenreeksargument. Retourneert de tekenreeks, maar met diakritische tekens vervangen door gelijkwaardige niet-diakritische tekens. Doorgaans gebruikt voor het omzetten van namen van de eerste en laatste diakritische tekens bevatten (accenttekens) in de geldige waarden die kunnen worden gebruikt in verschillende gebruikers-id, zoals de UPN-namen, SAM-accountnamen en e-mailadressen.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **Bron** |Vereist |Reeks | Doorgaans de naam van een eerste of laatste naamkenmerk |

- - -
### <a name="not"></a>niet
**Functie:**<br> NOT(Source)

**Beschrijving:**<br> Draait u de Booleaanse waarde van de **bron**. Als **bron** waarde is '*waar*", retourneert '*False*'. Anders retourneert '*waar*'.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **Bron** |Vereist |Booleaanse tekenreeks |Verwacht **bron** waarden zijn 'True' of 'False'... |

- - -
### <a name="replace"></a>Vervangen
**Functie:**<br> Vervang (bron, oldValue, regexPattern, regexGroupName, vervangende waarde, replacementAttributeName, sjabloon)

**Beschrijving:**<br>
Vervangt waarden binnen een tekenreeks. Het werkt anders, afhankelijk van de opgegeven parameters:

* Wanneer **oldValue** en **vervangende waarde** vindt:
  
  * Alle instanties van oldValue in de bron vervangen door de vervangende waarde
* Wanneer **oldValue** en **sjabloon** vindt:
  
  * Vervangt alle instanties van de **oldValue** in de **sjabloon** met de **bron** waarde
* Wanneer **regexPattern**, **regexGroupName**, **vervangende waarde** vindt:
  
  * Vervangt alle waarden die overeenkomen met oldValueRegexPattern in de brontekenreeks met de vervangende waarde
* Wanneer **regexPattern**, **regexGroupName**, **replacementPropertyName** vindt:
  
  * Als **bron** heeft geen waarde **bron** wordt geretourneerd
  * Als **bron** een waarde heeft, maakt gebruik van **regexPattern** en **regexGroupName** vervangende waarde ophalen uit de eigenschap met de **replacementPropertyName** . Vervangende waarde wordt als het resultaat geretourneerd

**Parameters:**<br> 
| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **Bron** |Vereist |Reeks |Doorgaans de naam van het kenmerk van het bronobject. |
| **oldValue** |Optioneel |Reeks |Waarde die moet worden vervangen **bron** of **sjabloon**. |
| **regexPattern** |Optioneel |Reeks |Regex-patroon voor de waarde die moet worden vervangen **bron**. Of, als replacementPropertyName wordt gebruikt, patroon voor het extraheren van waarde uit de vervanging van de eigenschap. |
| **regexGroupName** |Optioneel |Reeks |Naam van de groep binnen **regexPattern**. Alleen wanneer replacementPropertyName wordt gebruikt, wordt we Haal de waarde van deze groep als de vervangende waarde van de vervangende eigenschap. |
| **replacementValue** |Optioneel |Reeks |Nieuwe waarde te vervangen door oude met. |
| **replacementAttributeName** |Optioneel |Reeks |Naam van het kenmerk moet worden gebruikt voor de vervangende waarde, wanneer de gegevensbron heeft geen waarde. |
| **sjabloon** |Optioneel |Reeks |Wanneer **sjabloon** waarde is opgegeven, gaan we voor **oldValue** in de sjabloon en vervang deze door de bronwaarde. |

- - -
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**Functie:**<br> SingleAppRoleAssignment([appRoleAssignments])

**Beschrijving:**<br> Vereist een tekenreeksargument. Hiermee wordt de tekenreeks wordt geretourneerd, maar met een luiden diakritische tekens met gelijkwaardige niet-diakritische tekens.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **[appRoleAssignments]** |Vereist |Reeks |**[appRoleAssignments]**  object. |

- - -
### <a name="stripspaces"></a>StripSpaces
**Functie:**<br> StripSpaces(source)

**Beschrijving:**<br> Verwijdert alle spaties ("") tekens uit de brontekenreeks.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **Bron** |Vereist |Reeks |**bron** waarde om bij te werken. |

- - -
### <a name="switch"></a>Switch
**Functie:**<br> Switch (bron, defaultValue, key1, value1, key2, waarde2,...)

**Beschrijving:**<br> Wanneer **bron** waarde komt overeen met een **sleutel**, retourneert **waarde** voor die **sleutel**. Als **bron** waarde komt niet overeen met alle sleutels, retourneert **defaultValue**.  **Sleutel** en **waarde** parameters moeten altijd zijn verkrijgbaar in paren. De functie verwacht altijd een even aantal parameters.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **Bron** |Vereist |Reeks |**bron** waarde om bij te werken. |
| **defaultValue** |Optioneel |Reeks |De standaardwaarde moet worden gebruikt wanneer de bron komt niet overeen met alle sleutels. Lege tekenreeks (""). |
| **sleutel** |Vereist |Reeks |**Sleutel** vergelijken **bron** met de waarde. |
| **value** |Vereist |Reeks |Vervangende waarde voor de **bron** die overeenkomt met de sleutel. |

## <a name="examples"></a>Voorbeelden
### <a name="strip-known-domain-name"></a>Bekende domeinnaam van strook/lijn
U moet de domeinnaam van een bekend van e-mailadres van een gebruiker om op te halen van de naam van een gebruiker verwijderen. <br>
Bijvoorbeeld, als het domein 'contoso.com' is, kan u de volgende expressie:

**Expressie:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Voorbeeld van invoer / uitvoer:** <br>

* **INVOER** (e-mail): "john.doe@contoso.com"
* **UITVOER**: "john.doe"

### <a name="append-constant-suffix-to-user-name"></a>Constante achtervoegsel toevoegen aan de gebruikersnaam van
Als u van een Sandbox met Salesforce gebruikmaakt, moet u mogelijk een extra achtervoegsel toevoegen aan uw gebruikersnamen voordat deze worden gesynchroniseerd.

**Expressie:** <br>
`Append([userPrincipalName], ".test")`

**Voorbeeld van invoer/uitvoer:** <br>

* **INVOER**: (userPrincipalName): "John.Doe@contoso.com"
* **UITVOER**: "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Gebruikersalias genereren door samenvoegen van onderdelen van de voornaam en achternaam
U moet een gebruiker alias genereren door te nemen van de eerste 3 letters van de voornaam van de gebruiker en eerste 5 letters van de achternaam van de gebruiker.

**Expressie:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Voorbeeld van invoer/uitvoer:** <br>

* **INVOER** (givenName): "John"
* **INVOER** (voornaam): "Doe"
* **UITVOER**: "JohDoe"

### <a name="remove-diacritics-from-a-string"></a>Diakritische tekens verwijderen uit een tekenreeks
U moet vervangen van tekens lang zijn accenttekens met gelijkwaardige tekens die geen accenttekens bevatten.

**Expressie:** <br>
NormalizeDiacritics([givenName])

**Voorbeeld van invoer/uitvoer:** <br>

* **INVOER** (givenName): "Zoë"
* **UITVOER**: "Zoe"

### <a name="output-date-as-a-string-in-a-certain-format"></a>Uitvoerdatum als een tekenreeks in een bepaalde indeling
Wilt u datums verzenden naar een SaaS-toepassing in een bepaalde indeling. <br>
U wilt bijvoorbeeld datums voor ServiceNow.

**Expressie:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Voorbeeld van invoer/uitvoer:**

* **INVOER** (extensionAttribute1): "20150123105347.1Z"
* **UITVOER**: "23-01-2015"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Vervangen door een waarde op basis van vooraf gedefinieerde set opties
U moet voor het definiëren van de tijdzone van de gebruiker op basis van de status opgeslagen in Azure AD. <br>
Als de status code komt niet overeen met een van de vooraf gedefinieerde opties, gebruikt u standaardwaarde van 'Australië/Sydney'.

**Expressie:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Voorbeeld van invoer/uitvoer:**

* **INVOER** (status): "QLD"
* **UITVOER**: "Australië/Brisbane"

## <a name="related-articles"></a>Gerelateerde artikelen
* [Gebruiker inrichting/ongedaan maken van inrichting voor SaaS-toepassingen automatiseren](user-provisioning.md)
* [Kenmerktoewijzingen voor het inrichten van gebruikers aan te passen](customize-application-attributes.md)
* [Bereikfilters toevoegen voor het inrichten van gebruikers](define-conditional-rules-for-provisioning-user-accounts.md)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](use-scim-to-provision-users-and-groups.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)
* [Meldingen over accountinrichting](user-provisioning.md)
* [Lijst met zelfstudies over het integreren van SaaS-Apps](../saas-apps/tutorial-list.md)

