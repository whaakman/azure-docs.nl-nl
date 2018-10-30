---
title: Dynamische automatische regels voor groepslidmaatschap verwijzen naar in Azure Active Directory | Microsoft Docs
description: Het maken van regels voor groepslidmaatschap voor het automatisch vullen van groepen, en een verwijzing naar de regel.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 10/26/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 13d6ed9feab4654d3574a5aced72efa0345365a6
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215324"
---
# <a name="dynamic-membership-rules-for-groups-in-azure-active-directory"></a>Regels voor dynamisch lidmaatschap voor groepen in Azure Active Directory

In Azure Active Directory (Azure AD), kunt u complexe op kenmerken gebaseerde regels voor het inschakelen van dynamisch lidmaatschap voor groepen. Dynamisch groepslidmaatschap vermindert de administratieve overhead van het toevoegen en verwijderen van gebruikers. Dit artikel worden de eigenschappen en de syntaxis voor het maken van dynamische lidmaatschapsregels voor gebruikers of apparaten. U kunt een regel instellen voor dynamisch lidmaatschap voor beveiligingsgroepen of Office 365-groepen.

Wanneer kenmerken van een gebruiker of apparaat wijzigt, evalueert het systeem alle dynamische groepsregels in een map om te controleren of de wijziging wilt activeren, een groep toevoegt of verwijdert. Als een gebruiker of het apparaat voldoet aan een regel voor een groep, worden ze toegevoegd als lid van die groep. Als ze niet langer voldoen aan de regel, worden ze verwijderd. U kunt geen handmatig toevoegen of verwijderen van een lid van een dynamische groep.

* U kunt een dynamische groep voor apparaten of voor gebruikers maken, maar u kunt een regel waarmee gebruikers en apparaten bevat kan niet maken.
* U kunt een groep apparaten op basis van kenmerken van de eigenaren van het apparaat kan niet maken. Regels voor groepslidmaatschap apparaat kunnen alleen verwijzen naar apparaatkenmerken.

> [!NOTE]
> Deze functie is een Azure AD Premium P1-licentie vereist voor elke unieke gebruiker die lid is van een of meer dynamische groepen. U hebt geen licenties toewijzen aan gebruikers voor ze lid zijn van dynamische groepen, maar hebt u het minimale aantal licenties in de tenant om te kunnen krijgen alle gebruikers. Bijvoorbeeld, als u had een totaal van 1000 unieke gebruikers in alle dynamische groepen in uw tenant, dan moet u ten minste 1000 licenties voor Azure AD Premium P1 om te voldoen aan de licentievereiste.
>

## <a name="constructing-the-body-of-a-membership-rule"></a>De hoofdtekst van de regel voor een verzameling maken

Een lidmaatschapsregel dat automatisch wordt ingevuld een groep met gebruikers of apparaten is een binaire expressie die in een resultaat true of false resulteert. De drie onderdelen van een eenvoudige regel zijn:

* Eigenschap
* Operator
* Waarde

De volgorde van de onderdelen in een expressie zijn belangrijk om te voorkomen dat syntaxisfouten.

### <a name="rules-with-a-single-expression"></a>Regels met één expressie

Één expressie is de eenvoudigste vorm van de regel voor een verzameling en heeft alleen de drie onderdelen die hierboven worden vermeld. Een regel met een enkele expressie ziet er ongeveer als volgt uit: `Property Operator Value`, waarbij de syntaxis voor de eigenschap de naam van object.property.

Hier volgt een voorbeeld van een correct samengestelde lidmaatschapsregel met één expressie:

```
user.department -eq "Sales"
```

Haakjes zijn optioneel voor één expressie. De totale lengte van de hoofdtekst van het lidmaatschapsregel niet langer zijn dan 2048 tekens.

## <a name="supported-properties"></a>Ondersteunde eigenschappen

Er zijn drie typen eigenschappen die kunnen worden gebruikt om een lidmaatschapsregel samen te stellen.

* Booleaans
* Reeks
* Tekenreeks-verzameling

Hieronder vindt u de eigenschappen van de gebruiker die u gebruiken kunt om een enkele expressie te maken.

### <a name="properties-of-type-boolean"></a>Eigenschappen van het type boolean

| Eigenschappen | Toegestane waarden | Gebruik |
| --- | --- | --- |
| accountEnabled |waar onwaar |user.accountEnabled - eq true |
| dirSyncEnabled |waar onwaar |user.dirSyncEnabled - eq true |

### <a name="properties-of-type-string"></a>Eigenschappen van het typetekenreeks

| Eigenschappen | Toegestane waarden | Gebruik |
| --- | --- | --- |
| city |Een tekenreekswaarde of *null* |(user.city - eq "waarde") |
| Land/regio |Een tekenreekswaarde of *null* |(zoals user.country - eq "waarde") |
| Bedrijfsnaam | Een tekenreekswaarde of *null* | (user.companyName - eq "waarde") |
| Afdeling |Een tekenreekswaarde of *null* |(user.department - eq "waarde") |
| displayName |een string-waarde |(user.displayName - eq "waarde") |
| werknemer-id |een string-waarde |(user.employeeId - eq "waarde")<br>(user.employeeId - ne *null*) |
| facsimileTelephoneNumber |Een tekenreekswaarde of *null* |(user.facsimileTelephoneNumber - eq "waarde") |
| givenName |Een tekenreekswaarde of *null* |(user.givenName - eq "waarde") |
| Functie |Een tekenreekswaarde of *null* |(user.jobTitle - eq "waarde") |
| mail |Een tekenreekswaarde of *null* (SMTP-adres van de gebruiker) |(user.mail - eq "waarde") |
| mailNickName |Een string-waarde (e-mailalias van de gebruiker) |(user.mailNickName - eq "waarde") |
| mobiele |Een tekenreekswaarde of *null* |(user.mobile - eq "waarde") |
| object-id |GUID van het gebruikersobject |(user.objectId - eq "11111111-1111-1111-1111-111111111111") |
| onPremisesSecurityIdentifier | On-premises beveiligings-id (SID) voor gebruikers die zijn gesynchroniseerd van on-premises naar de cloud. |(user.onPremisesSecurityIdentifier - eq "S-1-1-11-1111111111-1111111111-1111111111-1111111") |
| passwordPolicies |Geen DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |(user.passwordPolicies - eq "DisableStrongPassword") |
| physicalDeliveryOfficeName |Een tekenreekswaarde of *null* |(user.physicalDeliveryOfficeName - eq "waarde") |
| Postcode |Een tekenreekswaarde of *null* |(user.postalCode - eq "waarde") |
| preferredLanguage |ISO 639-1-code |(user.preferredLanguage - eq "en-US") |
| sipProxyAddress |Een tekenreekswaarde of *null* |(user.sipProxyAddress - eq "waarde") |
| state |Een tekenreekswaarde of *null* |(user.state - eq "waarde") |
| streetAddress |Een tekenreekswaarde of *null* |(user.streetAddress - eq "waarde") |
| Achternaam |Een tekenreekswaarde of *null* |(user.surname - eq "waarde") |
| telephoneNumber |Een tekenreekswaarde of *null* |(user.telephoneNumber - eq "waarde") |
| usageLocation |Twee letters landcode |(user.usageLocation - eq "US") |
| userPrincipalName |een string-waarde |(user.userPrincipalName -eq "alias@domain") |
| UserType |lid Gast *null* |(user.userType - eq "Lid") |

### <a name="properties-of-type-string-collection"></a>Eigenschappen van het type tekenreeks-verzameling

| Eigenschappen | Toegestane waarden | Gebruik |
| --- | --- | --- |
| otherMails |een string-waarde |(user.otherMails-bevat "alias@domain") |
| proxyAddresses |SMTP: alias@domain smtp: alias@domain |(user.proxyAddresses-bevat "SMTP: alias@domain") |

Zie voor de eigenschappen voor regels voor apparaten gebruikt, [regels voor apparaten](#rules-for-devices).

## <a name="supported-operators"></a>Ondersteunde operators

De volgende tabel bevat de ondersteunde operators en de syntaxis voor één expressie. Operators kunnen worden gebruikt met of zonder het voorvoegsel van het koppelteken (-).

| Operator | Syntaxis |
| --- | --- |
| Niet gelijk aan |-ne |
| Is gelijk aan |-eq |
| Begint niet met |-notStartsWith |
| Begint met |-startsWith |
| Bevat niet |-notContains |
| Contains |-bevat |
| Komt niet overeen met |-notMatch |
| Overeenkomst |-overeen met |
| In | -in |
| Niet In | -notIn |

### <a name="using-the--in-and--notin-operators"></a>Met behulp van de - in en - notIn operators

Als u wilt vergelijken van de waarde van een gebruikerskenmerk op basis van een aantal verschillende waarden kunt u de - in of notIn - operators. Gebruik van de symbolen haakje ' [' en '] ' om te beginnen en eindigen van de lijst met waarden.

 In het volgende voorbeeld wordt resulteert de expressie in waar als de waarde van user.department gelijk is aan een van de waarden in de lijst:

```
   user.department -in ["50001","50002","50003",“50005”,“50006”,“50007”,“50008”,“50016”,“50020”,“50024”,“50038”,“50039”,“51100”]
```


### <a name="using-the--match-operator"></a>Met behulp van de - match-operator 
De **-overeenkomen met** operator wordt gebruikt voor die overeenkomt met een reguliere expressie. Voorbeelden:

```
user.displayName -match "Da.*"   
```
Da, Dav, David resulteren in waar, aDa wordt geëvalueerd als onwaar.

```
user.displayName -match ".*vid"
```
David resulteert in waar, Da wordt geëvalueerd als onwaar.

## <a name="supported-values"></a>Ondersteunde waarden

De waarden die worden gebruikt in een expressie kunnen bestaan uit verschillende typen, met inbegrip van:

* Tekenreeksen
* Boolean-waarde: true, false
* Getallen
* Aantal array, string-matrix-arrays

Bij het opgeven van een waarde in een expressie is het belangrijk dat u de juiste syntaxis gebruiken om fouten te voorkomen. Er zijn enkele tips syntaxis:

* Dubbele aanhalingstekens zijn optioneel, tenzij de waarde een tekenreeks is.
* Bewerkingen voor tekenreeks en reguliere expressie zijn niet hoofdlettergevoelig.
* Wanneer een string-waarde dubbele aanhalingstekens bevat, beide aanhalingstekens moeten worden weergegeven met behulp van de \` teken, bijvoorbeeld user.department - eq \`' Sales\`' de juiste syntaxis is als 'Verkoop' de waarde is.
* U kunt ook uitvoeren Null-controles met null als een waarde, bijvoorbeeld `user.department -eq null`.

### <a name="use-of-null-values"></a>Gebruik van Null-waarden

Als u wilt een null-waarde opgeven in een regel, kunt u de *null* waarde. 

* -Eq of -ne gebruiken bij het vergelijken van de *null* waarde in een expressie.
* Gebruik aanhalingstekens rond het woord *null* alleen als u wilt dat deze moeten worden geïnterpreteerd als een letterlijke tekenreeks-waarde.
* -Geen operator kan niet worden gebruikt als een vergelijkende operator op null-waarden. Als u deze gebruikt, krijgt u een fout of u null gebruiken of $null.

De juiste manier om te verwijzen naar de waarde null is als volgt:

```
   user.mail –ne null
```

## <a name="rules-with-multiple-expressions"></a>Regels met meerdere expressies

Een regel voor groepslidmaatschap kan bestaan uit meer dan één één expressie die zijn verbonden met de - en- of en - niet logische operators. Logische operators kunnen ook worden gebruikt in combinatie. 

Hier volgen enkele voorbeelden van correct samengestelde lidmaatschapsregels met meerdere expressies:

```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```

### <a name="operator-precedence"></a>Operatoren

Alle operators worden in volgorde van prioriteit van de hoogste naar laagste hieronder vermeld. Operators op dezelfde regel zijn van dezelfde prioriteit:

```
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
-not
-and
-or
-any -all
```

Hieronder volgt een voorbeeld van operatoren waarbij twee expressies worden geëvalueerd voor de gebruiker:

```
   user.department –eq "Marketing" –and user.country –eq "US"
```

Haakjes zijn alleen nodig wanneer prioriteit voldoet niet aan uw vereisten. Bijvoorbeeld, als u afdeling wilt moet eerst worden geëvalueerd, ziet hieronder u hoe haakjes kunnen worden gebruikt om volgorde te bepalen:

```
   user.country –eq "US" –and (user.department –eq "Marketing" –or user.department –eq "Sales")
```

## <a name="rules-with-complex-expressions"></a>Regels met complexe expressies

De regel voor een verzameling kan bestaan uit complexe expressies waarbij de eigenschappen, operators en waarden voor complexere formulieren nemen. Expressies worden beschouwd als complexe als een van de volgende voorwaarden voldaan wordt:

* De eigenschap bestaat uit een verzameling waarden. met name eigenschappen met meerdere waarden
* De expressies-een en -alle operators
* De waarde van de expressie kan zelf zijn een of meer expressies

## <a name="multi-value-properties"></a>Eigenschappen van meerdere waarden

Eigenschappen van meerdere waarden zijn verzamelingen van objecten van hetzelfde type. Ze kunnen worden gebruikt om te maken met behulp van regels voor groepslidmaatschap-een en -alle logische operators.

| Eigenschappen | Waarden | Gebruik |
| --- | --- | --- |
| gebruikt u assignedPlans | Elk object in de verzameling beschrijft de tekenreekseigenschappen van de volgende: capabilityStatus, service, servicePlanId |user.assignedPlans-een (assignedPlan.servicePlanId - eq "efb87545-963c-4e0d-99df-69c6916d9eb0"- en assignedPlan.capabilityStatus - eq "Ingeschakeld") |
| proxyAddresses| SMTP: alias@domain smtp: alias@domain | (user.proxyAddresses-een (\_ -bevat "contoso")) |

### <a name="using-the--any-and--all-operators"></a>Met behulp van de - een en -alle operators

U kunt gebruiken: een en -alle operators om toe te passen van een voorwaarde op een of meer van de items in de verzameling, respectievelijk.

* -een (voldaan wanneer ten minste één item in de verzameling komt overeen met de voorwaarde)
* -alle (voldaan als de voorwaarde die overeenkomen met alle items in de verzameling)

#### <a name="example-1"></a>Voorbeeld 1

gebruikt u assignedPlans is een eigenschap met meerdere waarden met een lijst met alle service-abonnementen toegewezen aan de gebruiker. De volgende expressie wordt gebruikers met de Exchange Online (abonnement 2) service-plan (als een GUID-waarde) die ook in de ingeschakelde status geselecteerd:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

Een regel zoals deze kan worden gebruikt voor het groeperen van alle gebruikers voor wie een Office 365 (of andere Microsoft Online Service) mogelijkheid is ingeschakeld. U kunt vervolgens aan de groep met een set beleidsregels toepassen.

#### <a name="example-2"></a>Voorbeeld 2

De volgende expressie selecteert alle gebruikers die beschikken over een service-plan dat is gekoppeld aan de Intune-service (aangeduid met de naam van de service "SCO"):

```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

### <a name="using-the-underscore--syntax"></a>Met behulp van het onderstrepingsteken (\_) syntaxis

Het onderstrepingsteken (\_) syntaxis komt overeen met een specifieke waarde in een van de eigenschappen van de tekenreeks met meerdere waarden verzameling gebruikers of apparaten toevoegen aan een dynamische groep. Deze wordt gebruikt met de - of - alle operators.

Hier volgt een voorbeeld van het gebruik van het onderstrepingsteken (\_) in een regel voor het toevoegen van leden op basis van user.proxyAddress (dit werkt hetzelfde voor user.otherMails). Deze regel wordt elke gebruiker met de proxy-adres met 'contoso' aan de groep toegevoegd.

```
(user.proxyAddresses -any (_ -contains "contoso"))
```

## <a name="other-properties-and-common-rules"></a>Andere eigenschappen en de algemene regels

### <a name="create-a-direct-reports-rule"></a>Maak een regel "Directe ondergeschikten"

U kunt een groep met alle direct ondergeschikten van een manager maken. Wanneer de manager van de direct ondergeschikten in de toekomst wijzigt, wordt het lidmaatschap van de groep automatisch aangepast.

De regel direct ondergeschikten is gebouwd met behulp van de volgende syntaxis:

```
Direct Reports for "{objectID_of_manager}"
```

Hier volgt een voorbeeld van een geldige regel waar '62e19b97-8b3d-4d4a-a106-4ce66896a863' de object-id van de manager is:

```
Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```

De volgende tips kunt u goed gebruik van de regel.

* De **Manager ID** is de object-ID van de manager. Het kan worden gevonden in de manager van de **profiel**.
* Voor de regel om te werken, zorg ervoor dat de **Manager** eigenschap correct is ingesteld voor gebruikers in uw tenant. U kunt de huidige waarde van de gebruiker controleren **profiel**.
* Deze regel ondersteunt alleen de manager van de direct ondergeschikten. Met andere woorden, u een groep kunt maken met de manager van de direct ondergeschikten *en* hun rapporten.
* Deze regel kan niet worden gecombineerd met andere lidmaatschapsregels.

### <a name="create-an-all-users-rule"></a>Een regel voor 'Alle gebruikers' maken

U kunt een groep met alle gebruikers in een tenant met behulp van de regel voor een verzameling kunt maken. Wanneer gebruikers worden toegevoegd of verwijderd uit de tenant in de toekomst, wordt het lidmaatschap van de groep wordt automatisch aangepast.

De regel 'Alle gebruikers' is gebouwd met behulp van één expressie met behulp van de operator - ne en de null-waarde. Met deze regel wordt gebruikers van B2B-gasten, evenals lidgebruikers toegevoegd aan de groep.

```
user.objectid -ne null
```

### <a name="create-an-all-devices-rule"></a>Een regel voor 'Alle apparaten' maken

U kunt een groep met alle apparaten in een tenant met behulp van de regel voor een verzameling kunt maken. Wanneer apparaten worden toegevoegd of verwijderd uit de tenant in de toekomst, wordt het lidmaatschap van de groep wordt automatisch aangepast.

De regel 'Alle apparaten' is gebouwd met behulp van één expressie met behulp van de operator - ne en de null-waarde:

```
device.objectid -ne null
```

## <a name="extension-properties-and-custom-extension-properties"></a>Extensie-eigenschappen en aangepaste extensie-eigenschappen

Extensiekenmerken en aangepaste extenson eigenschappen worden ondersteund als de eigenschappen van een verbindingsreeks in dynamisch-lidmaatschapregels. Extensiekenmerken vanuit on-premises Windows Server AD worden gesynchroniseerd en neemt de indeling van "ExtensionAttributeX", waarbij X gelijk is aan 1-15. Hier volgt een voorbeeld van een regel die gebruikmaakt van een extensiekenmerk als een eigenschap:

```
(user.extensionAttribute15 -eq "Marketing")
```

Aangepaste extensie-eigenschappen zijn gesynchroniseerd vanuit on-premises Windows Server AD of vanuit een verbonden SaaS-toepassing en zijn van de indeling van `user.extension_[GUID]__[Attribute]`, waarbij:

* [GUID] is de unieke id in Azure AD voor de toepassing die de eigenschap in Azure AD gemaakt
* [Kenmerk] is de naam van de eigenschap zoals deze is gemaakt

Een voorbeeld van een regel die gebruikmaakt van een aangepaste extensie-eigenschap is:

```
user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber -eq "123"
```

Naam van de aangepaste eigenschap kan worden gevonden in de map door het opvragen van een gebruiker de eigenschap met behulp van Graph Explorer en te zoeken naar de eigenschapsnaam. Bovendien kunt u nu kiezen **aangepaste extensie-eigenschappen ophalen** koppeling in de opbouwfunctie voor dynamische gebruiker groep regel voor een unieke app-ID invoeren en ontvangen van de volledige lijst met aangepaste extensie-eigenschappen om te gebruiken bij het maken van een regel voor dynamisch lidmaatschap. Deze lijst kan ook worden vernieuwd om op te halen van alle nieuwe aangepaste extensie-eigenschappen voor die app.

## <a name="rules-for-devices"></a>Regels voor apparaten

U kunt ook een regel waarmee apparaatobjecten voor lidmaatschap in een groep selecteert maken. U geen gebruikers en apparaten als leden van de beveiligingsgroep. De volgende apparaatkenmerken kunnen worden gebruikt.

 Apparaatkenmerk  | Waarden | Voorbeeld
 ----- | ----- | ----------------
 accountEnabled | waar onwaar | (device.accountEnabled - eq true)
 displayName | een string-waarde |(device.displayName - eq "Rob Iphone")
 deviceOSType | een string-waarde | (of device.deviceOSType - eq "iPad")- of (of device.deviceOSType - eq "iPhone")
 deviceOSVersion | een string-waarde | (apparaat. OSVersion - eq "9.1")
 deviceCategory | een geldige naam van de apparaatcategorie | (device.deviceCategory - eq "BYOD")
 deviceManufacturer | een string-waarde | (device.deviceManufacturer - eq "Samsung")
 deviceModel | een string-waarde | (device.deviceModel - eq "iPad lucht")
 deviceOwnership | Persoonlijk, bedrijf, onbekend | (device.deviceOwnership - eq 'Bedrijf')
 Domeinnaam | een string-waarde | (device.domainName - eq 'contoso.com')
 enrollmentProfileName | Naam van Apple-Inschrijvingsprofiel voor apparaten of Windows Autopilot-profiel | (device.enrollmentProfileName - eq "DEP iPhones")
 isRooted | waar onwaar | (device.isRooted - eq true)
 managementType | MDM (voor mobiele apparaten)<br>PC (voor computers die worden beheerd door de Intune-PC-agent) | (device.managementType - eq 'MDM')
 organizationalUnit | de waarde van een tekenreeks die overeenkomt met de naam van de organisatie-eenheid ingesteld door een on-premises Active Directory | (device.organizationalUnit - eq "VS-pc's")
 deviceId | een geldige Azure AD-apparaat-ID | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d")
 object-id | een geldige Azure AD-object-ID |  (device.objectId - eq 76ad43c9-32c5-45e8-a272-7b58b58f596d")

## <a name="next-steps"></a>Volgende stappen

Deze artikelen bevatten aanvullende informatie over groepen in Azure Active Directory.

* [Bestaande groepen weergeven](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toevoegen](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Lidmaatschappen van een groep beheren](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](groups-dynamic-membership.md)
