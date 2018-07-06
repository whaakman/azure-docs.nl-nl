---
title: Op kenmerken gebaseerde dynamische regels voor groepslidmaatschap in Azure Active Directory | Microsoft Docs
description: Het maken van geavanceerde regels voor dynamische groepslidmaatschap met inbegrip van expressie regel operators en parameters ondersteunde.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 07/05/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: a48dcff6eedc2aa6e8bb6cd5b0668af72259493b
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869084"
---
# <a name="create-attribute-based-rules-for-dynamic-group-membership-in-azure-active-directory"></a>Op kenmerken gebaseerde regels voor dynamisch groepslidmaatschap maken in Azure Active Directory
In Azure Active Directory (Azure AD), kunt u aangepaste regels voor het inschakelen van complexe op kenmerken gebaseerde dynamisch lidmaatschap voor groepen. Dit artikel worden de kenmerken en de syntaxis voor het maken van dynamische lidmaatschapsregels voor gebruikers of apparaten. U kunt een regel instellen voor dynamisch lidmaatschap voor beveiligingsgroepen of Office 365-groepen.

Wanneer kenmerken van een gebruiker of apparaat wijzigt, evalueert het systeem alle dynamische groepsregels in een map om te controleren of de wijziging wilt activeren, een groep toevoegt of verwijdert. Als een gebruiker of het apparaat voldoet aan een regel voor een groep, worden ze toegevoegd als lid van die groep. Als ze niet langer voldoen aan de regel, worden ze verwijderd.

> [!NOTE]
> Deze functie is een Azure AD Premium P1-licentie vereist voor elke unieke gebruiker die lid is van een of meer dynamische groepen. U hebt geen licenties toewijzen aan gebruikers voor ze lid zijn van dynamische groepen, maar hebt u het minimale aantal licenties in de tenant om te kunnen krijgen alle gebruikers. Bijvoorbeeld, als u had een totaal van 1000 unieke gebruikers in alle dynamische groepen in uw tenant, dan moet u ten minste 1000 licenties voor Azure AD Premium P1 om te voldoen aan de licentievereiste.
>
> U kunt een dynamische groep voor apparaten of voor gebruikers maken, maar u kunt een regel waarmee gebruikers en apparaten bevat kan niet maken.
> 
> Op dit moment is het niet mogelijk te maken van een groep apparaten op basis van kenmerken van de gebruiker die eigenaar is. Regels voor groepslidmaatschap apparaat kunnen alleen verwijzen naar direct kenmerken van apparaatobjecten in de map.

## <a name="to-create-an-advanced-rule"></a>Een geavanceerde regel maken
1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat een globale beheerder of een beheerder van gebruikersaccounts.
2. Selecteer **gebruikers en groepen**.
3. Selecteer **alle groepen**, en selecteer **nieuwe groep**.

   ![Nieuwe groep toevoegen](./media/groups-dynamic-membership/new-group-creation.png)

4. Op de **groep** blade, voer een naam en beschrijving voor de nieuwe groep. Selecteer een **lidmaatschapstype** van een van beide **dynamische gebruiker** of **dynamisch apparaat**, afhankelijk van of u wilt maken van een regel voor gebruikers of apparaten, en selecteer vervolgens **Dynamische query toevoegen**. U kunt de opbouwfunctie voor regel gebruiken om te maken van een eenvoudige regel of een geavanceerde regel zelf schrijven. In dit artikel bevat meer informatie over beschikbare gebruikers- en -kenmerken als voorbeelden van geavanceerde regels.

   ![Regel voor dynamisch lidmaatschap toevoegen](./media/groups-dynamic-membership/add-dynamic-group-rule.png)

5. Na het maken van de regel, selecteer **query toevoegen** aan de onderkant van de blade.
6. Selecteer **maken** op de **groep** blade om de groep te maken.

> [!TIP]
> Het maken van beveiligingsgroepen mislukt als de regel die u hebt ingevoerd onjuist gevormd of ongeldig is. Een melding wordt weergegeven in de rechterbovenhoek hoek rechtsboven van de portal, met een uitleg waarom de regel kan niet worden verwerkt. Lees zorgvuldig om te begrijpen hoe moet u de regel zodat deze geldige aanpassen.

## <a name="status-of-the-dynamic-rule"></a>Status van de dynamische regel

Hier ziet u het lidmaatschap van de verwerking van de status en het laatst bijgewerkt op op de overzichtspagina voor de dynamische groep.
  
  ![dynamische groep status weergeven](./media/groups-dynamic-membership/group-status.png)


De volgende statusberichten kunnen worden weergegeven voor **lidmaatschap verwerking** status:
* **Evaluatie van**: de groepswijziging is ontvangen en de updates worden geëvalueerd.
* **Verwerking van**: Updates worden verwerkt.
* **Bijwerken voltooid**: verwerking is voltooid en alle toepasselijke updates zijn aangebracht.
* **Verwerkingsfout**: Er is een fout opgetreden tijdens het evalueren van de regel voor de verzameling en verwerking kan niet worden voltooid.
* **Update onderbroken**: updates zijn onderbroken door de beheerder van de regel voor dynamisch lidmaatschap. MembershipRuleProcessingState is ingesteld op 'Onderbroken'.

De volgende statusberichten kunnen worden weergegeven voor **lidmaatschap laatst bijgewerkt** status:
* &lt;**Datum en tijd**&gt;: de laatste keer dat het lidmaatschap is bijgewerkt.
* **Bezig**: Updates worden momenteel uitgevoerd.
* **Onbekende**: de laatste updatetijd kan niet worden opgehaald. Het kan zijn vanwege de groep wordt pas gemaakt.

Als er een fout optreedt tijdens het verwerken van de lidmaatschapsregel voor een specifieke groep, een waarschuwing wordt weergegeven bovenaan het **overzichtspagina** voor de groep. Als niet in afwachting van lidmaatschap van dynamische updates kunnen worden verwerkt voor alle groepen in de tenant voor meer dan 24 uur, een waarschuwing wordt weergegeven boven in het **alle groepen**.

![verwerking van foutbericht](./media/groups-dynamic-membership/processing-error.png)

## <a name="constructing-the-body-of-an-advanced-rule"></a>De hoofdtekst van een geavanceerde regel maken
De geavanceerde regel die u voor de dynamische lidmaatschappen voor groepen maken kunt is in feite een binaire expressie die bestaat uit drie delen en resulteert in een resultaat true of false. De drie onderdelen zijn:

* Parameter naar links
* Binaire operator
* Juiste constante

Een volledige geavanceerde regel ziet er ongeveer als volgt uit: (leftParameter binaryOperator "RightConstant"), waarbij het openen en haakje-sluiten optioneel voor de hele binaire expressie, dubbele aanhalingstekens zijn optioneel, alleen vereist voor de juiste constante Wanneer het is tekenreeks en de syntaxis voor de parameter naar links is user.property. Een geavanceerde regel kan bestaan uit meer dan één binaire expressies gescheiden door het - en- of en - niet logische operators.

Hier volgen enkele voorbeelden van een correct samengestelde geavanceerde regel:
```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```
Zie de volgende secties voor de volledige lijst van ondersteunde parameters en regel operatoren. Zie voor de kenmerken die wordt gebruikt voor regels voor apparaten, [kenmerken gebruiken voor het maken van regels voor apparaatobjecten](#using-attributes-to-create-rules-for-device-objects).

De totale lengte van de hoofdtekst van de geavanceerde regel kan niet groter zijn dan 2048 tekens.

> [!NOTE]
> Bewerkingen voor tekenreeks en reguliere expressie zijn niet hoofdlettergevoelig. U kunt ook uitvoeren Null-controles, met behulp van *null* als een constante, bijvoorbeeld user.department - eq *null*.
> Tekenreeksen met aanhalingstekens "moet worden weergegeven met behulp van ' teken, bijvoorbeeld user.department - eq \`'Verkoop'.

## <a name="supported-expression-rule-operators"></a>Ondersteunde expressie regel operators
De volgende tabel bevat de operators voor de regel ondersteunde expressie en de syntaxis van moet worden gebruikt in de hoofdtekst van de geavanceerde regel:

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

## <a name="operator-precedence"></a>Operatoren

Hieronder vindt u alle Operators per prioriteit van lagere hoger. Operators op dezelfde regel zijn in dezelfde prioriteit:
````
-any -all
-or
-and
-not
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
````
Alle operators kunnen worden gebruikt met of zonder het voorvoegsel afbreekstreepje staat. Haakjes zijn alleen nodig wanneer prioriteit voldoet niet aan uw vereisten.
Bijvoorbeeld:
```
   user.department –eq "Marketing" –and user.country –eq "US"
```
is gelijk aan:
```
   (user.department –eq "Marketing") –and (user.country –eq "US")
```
## <a name="using-the--in-and--notin-operators"></a>Met behulp van de - In en - notIn operators

Als u wilt vergelijken van de waarde van een gebruikerskenmerk op basis van een aantal verschillende waarden kunt u de - In of notIn - operators. Hier volgt een voorbeeld met behulp van de - operator In:
```
   user.department -In ["50001","50002","50003",“50005”,“50006”,“50007”,“50008”,“50016”,“50020”,“50024”,“50038”,“50039”,“51100”]
```
Let op het gebruik van de ' [' en '] ' aan het begin en einde van de lijst met waarden. Deze voorwaarde resulteert in waar de waarde van user.department is gelijk aan een van de waarden in de lijst.


## <a name="query-error-remediation"></a>Herstel van query-fout
De volgende tabel geeft een lijst van veelvoorkomende fouten en aanwijzingen om ze te corrigeren

| Fout bij het parseren van query | Fout-gebruik | Gecorrigeerde syntaxis |
| --- | --- | --- |
| Fout: Kenmerk wordt niet ondersteund. |(user.invalidProperty - eq "Waarde") |(user.department - eq "waarde")<br/><br/>Zorg ervoor dat het kenmerk is ingesteld op de [eigenschappenlijst ondersteund](#supported-properties). |
| Fout: De Operator wordt niet ondersteund op kenmerk. |(user.accountEnabled-bevat waar) |(user.accountEnabled - eq true)<br/><br/>De operator die wordt gebruikt, wordt niet ondersteund voor de eigenschapstype (in dit voorbeeld-bevat kan niet worden gebruikt voor het type boolean). Gebruik de juiste operators voor het eigenschapstype. |
| Fout: Fout bij schemacompilatie Query. |1. (user.department - eq 'Verkoop') (user.department - eq "Marketing")<br/><br/>2. (user.userPrincipalName-overeenkomen met "*@domain.ext") |1. Ontbrekende operator. Gebruik de - en - of twee join-predicaten<br/><br/>(user.department - eq 'Verkoop')- of (user.department - eq "Marketing")<br/><br/>2. fout in reguliere expressie gebruikt in combinatie met - komt overeen met<br/><br/>(user.userPrincipalName-overeenkomen met '. *@domain.ext"), u kunt ook: (user.userPrincipalName-overeenkomen met"\@domain.ext$ ")|

## <a name="supported-properties"></a>Ondersteunde eigenschappen
Hieronder ziet u alle eigenschappen voor de gebruikersaccount die u in de geavanceerde regel gebruiken kunt:

### <a name="properties-of-type-boolean"></a>Eigenschappen van het type boolean
Toegestane operators

* -eq
* -ne

| Eigenschappen | Toegestane waarden | Gebruik |
| --- | --- | --- |
| accountEnabled |waar onwaar |user.accountEnabled - eq true |
| dirSyncEnabled |waar onwaar |user.dirSyncEnabled - eq true |

### <a name="properties-of-type-string"></a>Eigenschappen van het typetekenreeks
Toegestane operators

* -eq
* -ne
* -notStartsWith
* -StartsWith
* -bevat
* -notContains
* -overeen met
* -notMatch
* -in
* -notIn

| Eigenschappen | Toegestane waarden | Gebruik |
| --- | --- | --- |
| city |Een tekenreekswaarde of *null* |(user.city - eq "waarde") |
| land/regio |Een tekenreekswaarde of *null* |(zoals user.country - eq "waarde") |
| Bedrijfsnaam | Een tekenreekswaarde of *null* | (user.companyName - eq "waarde") |
| afdeling |Een tekenreekswaarde of *null* |(user.department - eq "waarde") |
| displayName |Een string-waarde |(user.displayName - eq "waarde") |
| werknemer-id |Een string-waarde |(user.employeeId - eq "waarde")<br>(user.employeeId - ne *null*) |
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
| userPrincipalName |Een string-waarde |(user.userPrincipalName -eq "alias@domain") |
| userType |lid Gast *null* |(user.userType - eq "Lid") |

### <a name="properties-of-type-string-collection"></a>Eigenschappen van het type tekenreeks-verzameling
Toegestane operators

* -bevat
* -notContains

| Eigenschappen | Toegestane waarden | Gebruik |
| --- | --- | --- |
| otherMails |Een string-waarde |(user.otherMails-bevat "alias@domain") |
| proxyAddresses |SMTP: alias@domain smtp: alias@domain |(user.proxyAddresses-bevat "SMTP: alias@domain") |

## <a name="multi-value-properties"></a>Eigenschappen van meerdere waarden
Toegestane operators

* -een (voldaan wanneer ten minste één item in de verzameling komt overeen met de voorwaarde)
* -alle (voldaan als de voorwaarde die overeenkomen met alle items in de verzameling)

| Eigenschappen | Waarden | Gebruik |
| --- | --- | --- |
| gebruikt u assignedPlans |Elk object in de verzameling beschrijft de tekenreekseigenschappen van de volgende: capabilityStatus, service, servicePlanId |user.assignedPlans-een (assignedPlan.servicePlanId - eq "efb87545-963c-4e0d-99df-69c6916d9eb0"- en assignedPlan.capabilityStatus - eq "Ingeschakeld") |

Eigenschappen van meerdere waarden zijn verzamelingen van objecten van hetzelfde type. U kunt gebruiken: een en -alle operators om toe te passen van een voorwaarde op een of meer van de items in de verzameling, respectievelijk. Bijvoorbeeld:

gebruikt u assignedPlans is een eigenschap met meerdere waarden met een lijst met alle service-abonnementen toegewezen aan de gebruiker. De hieronder de expressie wordt gebruikers met de Exchange Online (abonnement 2) service-plan dat ook in de ingeschakelde status selecteren:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(De Guid-id identificeert het Exchange Online (abonnement 2) service-plan.)

> [!NOTE]
> Dit is handig als u wilt bepalen van alle gebruikers voor wie een Office 365 (of andere Microsoft Online Service) mogelijkheid is ingeschakeld, bijvoorbeeld om ze een bepaalde set van het beleid is gericht.

De volgende expressie selecteert alle gebruikers die beschikken over een service-plan dat is gekoppeld aan de Intune-service (aangeduid met de naam van de service "SCO"):
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

## <a name="use-of-null-values"></a>Gebruik van Null-waarden

Als u wilt een null-waarde opgeven in een regel, kunt u de *null* waarde. Zorg ervoor dat u geen gebruik aanhalingstekens rond het woord *null* -als u dit doet, wordt dit geïnterpreteerd als een letterlijke tekenreeks-waarde. -Geen operator kan niet worden gebruikt als een vergelijkende operator op null-waarden. Als u deze gebruikt, krijgt u een fout of u null gebruiken of $null. Gebruik in plaats daarvan - eq of -ne. De juiste manier om te verwijzen naar de waarde null is als volgt:
```
   user.mail –ne $null
```

## <a name="extension-attributes-and-custom-attributes"></a>Extensiekenmerken en aangepaste kenmerken
Extensiekenmerken en aangepaste kenmerken die worden ondersteund in dynamisch-lidmaatschapregels.

Extensiekenmerken vanuit on-premises Windows Server AD worden gesynchroniseerd en neemt de indeling van "ExtensionAttributeX", waarbij X gelijk is aan 1-15.
Een voorbeeld van een regel die gebruikmaakt van een kenmerk toestelnummer
```
(user.extensionAttribute15 -eq "Marketing")
```
Aangepaste kenmerken worden gesynchroniseerd vanuit on-premises Windows Server AD of vanuit een verbonden SaaS-toepassing en de indeling van ' user.extension_[GUID]\__ [kenmerk] ', waarbij [GUID] de unieke id in AAD voor de toepassing die gemaakt is de kenmerk in AAD en [kenmerk] is de naam van het kenmerk zoals deze is gemaakt.
Een voorbeeld van een regel die gebruikmaakt van een aangepast kenmerk is
```
user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  
```
Naam van het aangepaste kenmerk kan worden gevonden in de map door het opvragen van een gebruiker het kenmerk met behulp van Graph Explorer en zoeken naar de naam van het kenmerk.

## <a name="direct-reports-rule"></a>De regel "Directe ondergeschikten"
U kunt een groep met alle direct ondergeschikten van een manager maken. Wanneer de manager van de direct ondergeschikten in de toekomst wijzigt, wordt het lidmaatschap van de groep automatisch worden aangepast.

> [!NOTE]
> 1. Voor de regel om te werken, zorg ervoor dat de **Manager ID** eigenschap correct is ingesteld op gebruikers in uw tenant. U kunt de huidige waarde voor een gebruiker controleren op hun **tabblad profiel**.
> 2. Deze regel ondersteunt alleen **direct** rapporten. Het is momenteel niet mogelijk te maken van een groep voor een geneste hiërarchie; bijvoorbeeld, een groep met directe ondergeschikten en hun rapporten.
> 3. Deze regel kan niet worden gecombineerd met andere geavanceerde regels.

**De groep configureren**

1. Volg de stappen 1-5 in sectie [de geavanceerde regel maken](#to-create-the-advanced-rule), en selecteer een **lidmaatschapstype** van **dynamische gebruiker**.
2. Op de **dynamisch-lidmaatschapregels** blade invoeren van de regel met de volgende syntaxis:

    *Direct ondergeschikten voor "{objectID_of_manager}"*

    Een voorbeeld van een geldige regel:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is the objectID of the manager. The object ID can be found on manager's **Profile tab**.
3. Na het opslaan van de regel, wordt alle gebruikers met de opgegeven waarde voor de ID van de Manager worden toegevoegd aan de groep.

## <a name="using-attributes-to-create-rules-for-device-objects"></a>Kenmerken gebruiken voor het maken van regels voor apparaatobjecten
U kunt ook een regel waarmee apparaatobjecten voor lidmaatschap in een groep selecteert maken. De volgende apparaatkenmerken kunnen worden gebruikt.

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



## <a name="changing-dynamic-membership-to-static-and-vice-versa"></a>Dynamische lidmaatschap wijzigen in statisch, en vice versa
Het is mogelijk om te wijzigen hoe lidmaatschap in een groep wordt beheerd. Dit is handig als u behouden van de dezelfde naam en de ID in het systeem wilt, zodat alle bestaande verwijzingen naar de groep nog steeds geldig zijn. het maken van een nieuwe groep zorgde deze verwijzingen worden bijgewerkt.

We hebben de Azure AD-beheercentrum om toe te voegen ondersteuning voor deze functionaliteit bijgewerkt. Klanten kunnen bestaande groepen nu converteren van een dynamisch lidmaatschap toegewezen lidmaatschap en vice versa via Azure AD-beheercentrum of PowerShell-cmdlets, zoals hieronder weergegeven.

> [!WARNING]
> Wanneer u een bestaande statische groep naar een dynamische groep, worden alle bestaande leden worden verwijderd uit de groep en klikt u vervolgens de lidmaatschapsregel om toe te voegen nieuwe leden worden verwerkt. Als de groep wordt gebruikt voor het beheren van toegang tot apps of resources, kunnen de oorspronkelijke leden toegang verliezen tot het lidmaatschapsregel volledig wordt verwerkt.
>
> Het is raadzaam dat u de nieuwe lidmaatschapsregel tevoren testen om ervoor te zorgen dat het nieuwe lidmaatschap in de groep is zoals verwacht.

### <a name="using-azure-ad-admin-center-to-change-membership-management-on-a-group"></a>Met behulp van Azure AD-beheercentrum lidmaatschap management voor een groep wijzigen 

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat een globale beheerder of een beheerder van gebruikersaccounts in uw tenant.
2. Selecteer **groepen**.
3. Uit de **alle groepen** weergeven, opent u de groep die u wilt wijzigen.
4. Selecteer **eigenschappen**.
5. Op de **eigenschappen** pagina voor de groep, selecteer een **lidmaatschapstype** van toegewezen (statisch), dynamische gebruiker of dynamische apparaat, afhankelijk van uw gewenste lidmaatschapstype. Voor dynamisch lidmaatschap, kunt u de opbouwfunctie voor regel selecteren opties voor een eenvoudige regel of een geavanceerde regel zelf schrijven. 

De volgende stappen zijn een voorbeeld van een groep wijzigen van statisch naar dynamisch lidmaatschap voor een groep gebruikers. 

1. Op de **eigenschappen** pagina voor de geselecteerde groep, selecteer een **lidmaatschapstype** van **dynamische gebruiker**, selecteert u Ja in het dialoogvenster waarin wordt uitgelegd van de wijzigingen aan de groep lidmaatschap om door te gaan. 
  
   ![Selecteer het lidmaatschapstype van dynamische gebruiker](./media/groups-dynamic-membership/select-group-to-convert.png)
  
2. Selecteer **dynamische query toevoegen**, en geef vervolgens de regel.
  
   ![Geef de regel](./media/groups-dynamic-membership/enter-rule.png)
  
3. Na het maken van de regel, selecteer **query toevoegen** aan de onderkant van de pagina.
4. Selecteer **opslaan** op de **eigenschappen** pagina voor de groep uw wijzigingen op te slaan. De **lidmaatschapstype** van de groep is direct bijgewerkt in de lijst met groepen.

> [!TIP]
> Groepsconversie kan mislukken als de geavanceerde regel die u hebt ingevoerd onjuist is. Een melding wordt weergegeven in de rechterbovenhoek hoek rechtsboven van de portal die deze bevat een uitleg waarom de regel kan niet worden geaccepteerd door het systeem. Lees zorgvuldig om te begrijpen hoe u de regel zodat deze geldige kunt aanpassen.

### <a name="using-powershell-to-change-membership-management-on-a-group"></a>Lidmaatschap van wijzigingsbeheer voor een groep met behulp van PowerShell

> [!NOTE]
> U moet het gebruik van cmdlets van eigenschappen voor dynamische groep wijzigen **de preview-versie van** [Azure AD PowerShell versie 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0). U kunt installeren de Preview-versie van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureADPreview).

Hier volgt een voorbeeld van de functies die het lidmaatschap management op een bestaande groep overschakelen. In dit voorbeeld is care uitgevoerd, correct manipuleren van de eigenschap GroupTypes en behouden van alle waarden die niet gerelateerd aan dynamisch lidmaatschap zijn.

```
#The moniker for dynamic groups as used in the GroupTypes property of a group object
$dynamicGroupTypeString = "DynamicMembership"

function ConvertDynamicGroupToStatic
{
    Param([string]$groupId)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -eq $null -or !$groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a static group. Aborting conversion.";
    }


    #remove the type for dynamic groups, but keep the other type values
    $groupTypes.Remove($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to remove the dynamic type, ii) pause execution of the current rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "Paused"
}

function ConvertStaticGroupToDynamic
{
    Param([string]$groupId, [string]$dynamicMembershipRule)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -ne $null -and $groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a dynamic group. Aborting conversion.";
    }
    #add the dynamic group type to existing types
    $groupTypes.Add($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to add the dynamic type, ii) start execution of the rule, iii) set the rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "On" -MembershipRule $dynamicMembershipRule
}
```
Een groep statische maken:
```
ConvertDynamicGroupToStatic "a58913b2-eee4-44f9-beb2-e381c375058f"
```
U een groep dynamisch:
```
ConvertStaticGroupToDynamic "a58913b2-eee4-44f9-beb2-e381c375058f" "user.displayName -startsWith ""Peter"""
```
## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over groepen in Azure Active Directory.

* [Zie de bestaande groepen](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toevoegen](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Lidmaatschap van een groep beheren](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](groups-dynamic-membership.md)
