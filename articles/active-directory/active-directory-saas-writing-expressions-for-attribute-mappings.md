---
title: Expressies voor kenmerktoewijzingen schrijven in Azure Active Directory | Microsoft Docs
description: Informatie over het gebruik van expressie toewijzingen kenmerkwaarden transformeren naar een aanvaardbaar indeling tijdens de geautomatiseerde inrichting van objecten van de SaaS-app in Azure Active Directory.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: b13c51cd-1bea-4e5e-9791-5d951a518943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: markvi
ms.openlocfilehash: c944a355c07b96c27dcdd477f625638284eabdf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Expressies voor kenmerktoewijzingen schrijven in Azure Active Directory
Wanneer u configureert het inrichten van een SaaS-toepassing, is een van de soorten kenmerktoewijzingen die u kunt opgeven een expressie-toewijzing. Hiervoor moet u een script-achtige expressie waarmee u uw gebruikers om gegevens te transformeren naar indelingen die meer geschikt is voor de SaaS-toepassing kunt schrijven.

## <a name="syntax-overview"></a>Overzicht van de syntaxis
De syntaxis voor expressies voor kenmerktoewijzingen is doet denken aan van Visual Basic voor Applications (VBA)-functies.

* De volledige expressie moet worden gedefinieerd in termen van functies, die bestaan uit een naam die wordt gevolgd door haakjes argumenten: <br>
  *Functienaam (<< argument 1 >>, <<argument N>>)*
* U kunt functies binnen elkaar nesten. Bijvoorbeeld: <br> *FunctionOne (FunctionTwo (<<argument1>>))*
* U kunt drie soorten argumenten doorgegeven aan functies:
  
  1. Kenmerken, die moeten worden tussen vierkante vierkante haken. Bijvoorbeeld: [attributeName]
  2. Tekenreeksconstanten moeten tussen dubbele aanhalingstekens worden geplaatst. Bijvoorbeeld: 'Verenigde Staten'
  3. Andere functies. Bijvoorbeeld: FunctionOne (<<argument1>>, FunctionTwo (<<argument2>>))
* Voor tekenreeksconstanten, als u een backslash (\) of een aanhalingsteken (") in de tekenreeks, moet moet deze worden voorafgegaan door het symbool backslash (\). Bijvoorbeeld: ' Bedrijfsnaam: \"Contoso\"'

## <a name="list-of-functions"></a>Lijst met functies
[Append](#append) &nbsp; &nbsp; &nbsp; &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; &nbsp; [koppelen](#join) &nbsp; &nbsp; &nbsp; &nbsp; [Mid](#mid) &nbsp; &nbsp; &nbsp; &nbsp; [niet](#not) &nbsp; &nbsp; &nbsp; &nbsp; [Vervangen](#replace) &nbsp; &nbsp; &nbsp; &nbsp; [StripSpaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; [Switch](#switch)

- - -
### <a name="append"></a>Toevoegen
**Functie:**<br> Append(Source, suffix)

**Beschrijving:**<br> Neemt een tekenreekswaarde voor de bron en het achtervoegsel toegevoegd aan het einde van deze.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **bron** |Vereist |Tekenreeks |Doorgaans de naam van het kenmerk van het bronobject |
| **achtervoegsel** |Vereist |Tekenreeks |De tekenreeks die u wilt toevoegen aan het einde van de bronwaarde. |

- - -
### <a name="formatdatetime"></a>formatDateTime
**Functie:**<br> FormatDateTime (bron, inputFormat, outputFormat)

**Beschrijving:**<br> Een tekenreeks van de datum van de ene indeling en converteert naar een andere indeling.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **bron** |Vereist |Tekenreeks |Doorgaans de naam van het kenmerk van het bronobject. |
| **inputFormat** |Vereist |Tekenreeks |De verwachte notatie van de bronwaarde. Zie voor ondersteunde indelingen [http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Vereist |Tekenreeks |Indeling van de uitvoerdatum. |

- - -
### <a name="join"></a>Koppelen
**Functie:**<br> Join (scheidingsteken, bron1, bron2,...)

**Beschrijving:**<br> Join() is vergelijkbaar met Append(), behalve dat deze meerdere kunt combineren **bron** tekenreeks waarden in één tekenreeks en elke waarde worden gescheiden door een **scheidingsteken** tekenreeks.

Als een van de bronwaarden is een kenmerk met meerdere waarden wordt elke waarde in wordt dat kenmerk zal worden samengevoegd, gescheiden van de waarde van het scheidingsteken.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **scheidingsteken** |Vereist |Tekenreeks |De tekenreeks die wordt gebruikt voor het scheiden van bronwaarden wanneer ze worden samengevoegd tot één tekenreeks. Kan ' ' als er geen scheidingsteken vereist is. |
| ** bron1... bronN ** |Vereiste, variabele-aantal keren |Tekenreeks |Tekenreekswaarden samen worden toegevoegd. |

- - -
### <a name="mid"></a>Mid
**Functie:**<br> Mid (bron, start, lengte)

**Beschrijving:**<br> Retourneert een subtekenreeks van de bronwaarde. Een subtekenreeks is een tekenreeks zijn met slechts enkele van de tekens uit de brontekenreeks.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **bron** |Vereist |Tekenreeks |Doorgaans de naam van het kenmerk. |
| **start** |Vereist |geheel getal |Index in de **bron** tekenreeks waar subtekenreeks moet beginnen. Eerste teken in de tekenreeks heeft de index van 1, tweede teken wordt index 2 hebben, enzovoort. |
| **lengte** |Vereist |geheel getal |Lengte van de subtekenreeks. Als de lengte buiten eindigt de **bron** tekenreeks, functie substring van resultaat **start** index tot het einde van **bron** tekenreeks. |

- - -
### <a name="not"></a>niet
**Functie:**<br> NOT(Source)

**Beschrijving:**<br> Gespiegeld de Boole-waarde van de **bron**. Als **bron** waarde is '*True*', retourneert '*False*'. Anders retourneert '*True*'.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **bron** |Vereist |Booleaanse tekenreeks |Verwacht **bron** waarden zijn 'True' of 'False'... |

- - -
### <a name="replace"></a>Vervangen
**Functie:**<br> ObsoleteReplace (bron, oldValue, regexPattern, regexGroupName, vervangende waarde, replacementAttributeName, sjabloon)

**Beschrijving:**<br>
Vervangt waarden binnen een tekenreeks. Het werkt anders, afhankelijk van de opgegeven parameters:

* Wanneer **oldValue** en **vervangende waarde** zijn beschikbaar:
  
  * Alle instanties van oldValue in de bron vervangen door de vervangende waarde
* Wanneer **oldValue** en **sjabloon** zijn beschikbaar:
  
  * Vervangt alle instanties van de **oldValue** in de **sjabloon** met de **bron** waarde
* Wanneer **oldValueRegexPattern**, **oldValueRegexGroupName**, **vervangende waarde** zijn beschikbaar:
  
  * Vervangt alle waarden die overeenkomen met oldValueRegexPattern in de brontekenreeks met de vervangende waarde
* Wanneer **oldValueRegexPattern**, **oldValueRegexGroupName**, **replacementPropertyName** zijn beschikbaar:
  
  * Als **bron** waarde heeft, **bron** wordt geretourneerd
  * Als **bron** heeft geen waarde, gebruikt de **oldValueRegexPattern** en **oldValueRegexGroupName** vervangende waarde ophalen uit de eigenschap met  **replacementPropertyName**. Vervangende waarde wordt geretourneerd als resultaat

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **bron** |Vereist |Tekenreeks |Doorgaans de naam van het kenmerk van het bronobject. |
| **oldValue** |Optioneel |Tekenreeks |Waarde moet worden vervangen **bron** of **sjabloon**. |
| **regexPattern** |Optioneel |Tekenreeks |Regex-patroon voor de waarde moet worden vervangen **bron**. Of, als replacementPropertyName wordt gebruikt, patroon Haal de waarde van eigenschap vervanging. |
| **regexGroupName** |Optioneel |Tekenreeks |Naam van de groep binnen **regexPattern**. Alleen wanneer replacementPropertyName wordt gebruikt, wordt er Haal de waarde van deze groep als vervangende waarde van eigenschap vervanging. |
| **vervangende waarde** |Optioneel |Tekenreeks |Nieuwe waarde ter vervanging van oude met. |
| **replacementAttributeName** |Optioneel |Tekenreeks |De naam van het kenmerk moet worden gebruikt voor de vervangende waarde wanneer de bron heeft geen waarde. |
| **sjabloon** |Optioneel |Tekenreeks |Wanneer **sjabloon** waarde wordt opgegeven, gaan we voor **oldValue** in de sjabloon en vervang deze door de bronwaarde. |

- - -
### <a name="stripspaces"></a>StripSpaces
**Functie:**<br> StripSpaces(source)

**Beschrijving:**<br> Verwijdert alle spaties ("") tekens uit de brontekenreeks.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **bron** |Vereist |Tekenreeks |**bron** waarde om te werken. |

- - -
### <a name="switch"></a>Switch
**Functie:**<br> Switch (bron, defaultValue, key1, value1, key2, waarde2,...)

**Beschrijving:**<br> Wanneer **bron** waarde komt overeen met een **sleutel**, retourneert **waarde** voor die **sleutel**. Als **bron** waarde komt niet overeen met alle sleutels, retourneert **defaultValue**.  **Sleutel** en **waarde** parameters moeten altijd komen in paren worden gebruikt. De functie verwacht altijd een even aantal parameters.

**Parameters:**<br> 

| Naam | Vereiste / herhalende | Type | Opmerkingen |
| --- | --- | --- | --- |
| **bron** |Vereist |Tekenreeks |**Bron** waarde om te werken. |
| **Standaardwaarde** |Optioneel |Tekenreeks |De standaardwaarde moet worden gebruikt als bron komt niet overeen met alle sleutels. Lege tekenreeks (""). |
| **sleutel** |Vereist |Tekenreeks |**Sleutel** vergelijken **bron** waarde met. |
| **waarde** |Vereist |Tekenreeks |Vervangende waarde voor de **bron** die overeenkomt met de sleutel. |

## <a name="examples"></a>Voorbeelden
### <a name="strip-known-domain-name"></a>Strook bekende domeinnaam
U moet een bekende domeinnaam in van een gebruiker e-mail met een gebruikersnaam verkrijgen van strook /. <br>
Bijvoorbeeld, als het domein 'contoso.com' is, kan u de volgende expressie:

**Expressie:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Voorbeeld van invoer / uitvoer:** <br>

* **INVOER** (e-mail): 'john.doe@contoso.com'
* **UITVOER**: 'john.doe'

### <a name="append-constant-suffix-to-user-name"></a>Constante achtervoegsel toevoegen aan de gebruikersnaam van de
Als u van een Sandbox met Salesforce gebruikmaakt, moet u mogelijk een extra achtervoegsel toevoegen aan uw gebruikersnamen voordat deze worden gesynchroniseerd.

**Expressie:** <br>
`Append([userPrincipalName], ".test"))`

**I/o-voorbeeld:** <br>

* **INVOER**: (userPrincipalName): 'John.Doe@contoso.com'
* **UITVOER**: 'John.Doe@contoso.com.test'

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Gebruikersalias genereren met cookievalidatie delen van de voornaam en achternaam
U moet een gebruiker alias genereren door middel van eerste 3 letters van de voornaam van de gebruiker en de eerste 5 letters van de achternaam van de gebruiker.

**Expressie:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**I/o-voorbeeld:** <br>

* **INVOER** (givenName): "Jan"
* **INVOER** (voornaam): 'De Vries'
* **UITVOER**: 'JohDoe'

### <a name="output-date-as-a-string-in-a-certain-format"></a>Uitvoerdatum als een tekenreeks in een bepaalde indeling
Wilt u datums verzenden naar een SaaS-toepassing in een bepaalde indeling. <br>
U wilt bijvoorbeeld datums voor ServiceNow opmaken.

**Expressie:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**I/o-voorbeeld:**

* **INVOER** (extensionAttribute1): '20150123105347.1Z'
* **UITVOER**: '2015-01-23'

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Een waarde op basis van vooraf gedefinieerde set opties vervangen
U moet de tijdzone van de gebruiker op basis van de status code opgeslagen in Azure AD te definiëren. <br>
Als de status code komt niet met de vooraf gedefinieerde opties overeen, gebruikt u standaardwaarde van 'Australië/Sydney'.

**Expressie:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**I/o-voorbeeld:**

* **INVOER** (status): 'QLD'
* **UITVOER**: ' Australië/Brisbane'

## <a name="related-articles"></a>Verwante artikelen
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Automatisch gebruikers inrichten/opheffen van inrichting tot SaaS-Apps](active-directory-saas-app-provisioning.md)
* [Kenmerktoewijzingen voor gebruikers inrichten aanpassen](active-directory-saas-customizing-attribute-mappings.md)
* [Bereikfilters voor gebruikers inrichten](active-directory-saas-scoping-filters.md)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](active-directory-scim-provisioning.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)
* [Meldingen inrichten van een account](active-directory-saas-account-provisioning-notifications.md)
* [Lijst met zelfstudies over het integreren van SaaS-Apps](active-directory-saas-tutorial-list.md)

