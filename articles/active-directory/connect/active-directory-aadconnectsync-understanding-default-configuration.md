---
title: 'Azure AD Connect-synchronisatie: inzicht in de standaardconfiguratie | Microsoft Docs'
description: In dit artikel beschrijft de standaardconfiguratie in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: ed876f22-6892-4b9d-acbe-6a2d112f1cd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 32a693c059a1b4261f33a3d6f50f397365e9dac4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect-synchronisatie: inzicht in de standaardconfiguratie
Dit artikel wordt uitgelegd dat de van de out-of-box-configuratieregels. Deze documenten in de regels en de invloed van deze regels op de configuratie. Ook helpt u bij de standaardconfiguratie van Azure AD Connect-synchronisatie. Het doel is dat de lezer hoe het configuratiemodel begrijpt, met de naam declaratieve inrichting werkt in een voorbeeld van een echte. In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd en configureren van Azure AD Connect-synchronisatie met de installatiewizard.

Om de details van het configuratiemodel te begrijpen, lezen [Understanding declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Out-of-box-regels van on-premises naar Azure AD
De volgende expressies kunnen worden gevonden in de out-of-box-configuratie.

### <a name="user-out-of-box-rules"></a>Gebruiker out-of-box-regels
Deze regels zijn ook van toepassing op het objecttype iNetOrgPerson.

Een gebruikersobject moet voldoen aan de volgende moeten worden gesynchroniseerd:

* Moet een sourceAnchor hebben.
* Nadat het object is gemaakt in Azure AD, klikt u vervolgens wijzigen sourceAnchor niet. Als de waarde gewijzigd lokale is, stopt het object synchroniseren totdat de sourceAnchor terug naar de vorige waarde wordt gewijzigd.
* Hebben moet het kenmerk accountEnabled (userAccountControl) ingevuld. Met een lokale Active Directory dit kenmerk is altijd aanwezig zijn en worden ingevuld.

De volgende gebruikersobjecten zijn **niet** gesynchroniseerd naar Azure AD:

* `IsPresent([isCriticalSystemObject])`. Zorg ervoor dat veel out-of-box-objecten in Active Directory, zoals de ingebouwde administrator-account, zijn niet gesynchroniseerd.
* `IsPresent([sAMAccountName]) = False`. Zorg ervoor dat gebruikersobjecten zonder sAMAccountName-kenmerk zijn niet gesynchroniseerd. Deze aanvraag zou alleen vrijwel gebeuren in een upgrade uitgevoerd van NT4-domein.
* `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Het serviceaccount dat wordt gebruikt door Azure AD Connect-synchronisatie en de eerdere versies niet synchroniseren.
* Niet synchroniseren met Exchange-accounts die niet in Exchange Online werkt.
  * `[sAMAccountName] = "SUPPORT_388945a0"`
  * `Left([mailNickname], 14) = "SystemMailbox{"`
  * `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
  * `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
* Objecten die niet in Exchange Online werkt niet synchroniseren.
  `CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
  Deze bitmasker (& H21C07000) filtert u uit de volgende objecten:
  * E-mailfunctionaliteit openbare map
  * Eigen Postvak
  * Postvak Database postvak (systeempostvak)
  * De universele beveiligingsgroep (wouldn't gelden voor een gebruiker, maar is omwille van de verouderde aanwezig)
  * Niet-universele groep (wouldn't gelden voor een gebruiker, maar is omwille van de verouderde aanwezig)
  * Postvak plannen
  * Detectie Postvak
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. De replicatie slachtoffer objecten niet synchroniseren.

Er gelden de volgende kenmerk regels:

* `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. Het kenmerk sourceAnchor is niet een gekoppeld postvak bijgedragen. Ervan wordt uitgegaan dat de daadwerkelijke account als later wordt gekoppeld als een gekoppeld postvak is gevonden.
* Exchange gerelateerde kenmerken worden alleen gesynchroniseerd als het kenmerk **mailNickName** heeft een waarde.
* Wanneer er meerdere forests, worden de kenmerken in de volgende volgorde verbruikt:
  1. Kenmerken die betrekking hebben op de aanmeldingspagina (bijvoorbeeld userPrincipalName) worden aangeleverd uit het forest met een account ingeschakeld.
  2. Kenmerken die kunnen worden gevonden in een Exchange-GAL (globale adreslijst) worden van het forest met een Exchange-postvak aangeleverd.
  3. Als geen postvak kan worden gevonden, vervolgens deze kenmerken kunnen afkomstig zijn van een forest.
  4. Exchange gerelateerde kenmerken (niet zichtbaar in de algemene Adreslijst technische kenmerken) van het forest worden aangeleverd waar `mailNickname ISNOTNULL`.
  5. Als er meerdere forests die zijn voor een van deze regels voldoende, wordt de volgorde maken (datum/tijd) van de verbindingslijnen (forests) gebruikt om te bepalen welk forest draagt bij de kenmerken.

### <a name="contact-out-of-box-rules"></a>Neem contact op met out-of-box-regels
Een contact-object moet voldoen aan de volgende moeten worden gesynchroniseerd:

* De contactpersoon moet e-mailfunctionaliteit. Het is geverifieerd met de volgende regels:
  * `IsPresent([proxyAddresses]) = True)`. Het kenmerk proxyAddresses moet worden ingevuld.
  * Een primaire e-mailadres vindt u in het kenmerk proxyAddresses of het e-mailkenmerk. De aanwezigheid van een @ wordt gebruikt om te controleren of de inhoud een e-mailadres. Een van deze twee regels moet worden geëvalueerd op True.
    * `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Is er een vermelding met ' SMTP: ' en als er, kunt u een @ worden gevonden in de tekenreeks?
    * `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Is het e-mailkenmerk ingevuld en als dit het geval is, kunt een @ worden gevonden in de tekenreeks?

De volgende contact op met objecten zijn **niet** gesynchroniseerd naar Azure AD:

* `IsPresent([isCriticalSystemObject])`. Zorg ervoor dat er geen contact op met objecten die als kritiek zijn gemarkeerd zijn gesynchroniseerd. Mag niet bestaan uit een met een standaardconfiguratie.
* `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
* `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Deze objecten goed niet in Exchange Online.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. De replicatie slachtoffer objecten niet synchroniseren.

### <a name="group-out-of-box-rules"></a>Groep out-of-box-regels
Een group-object moet voldoen aan de volgende moeten worden gesynchroniseerd:

* Moet minder dan 50.000 leden hebben. Dit aantal is het aantal leden in de lokale groep.
  * Als er meer leden voordat synchronisatie van de eerste keer wordt gestart, wordt de groep is niet gesynchroniseerd.
  * Als het aantal leden Breid uit wanneer het aanvankelijk is gemaakt, klikt u vervolgens wanneer deze 50.000 leden bereikt wordt gestopt tot het aantal lidmaatschap lager dan 50.000 opnieuw is synchroniseren.
  * Opmerking: Het aantal 50.000 lidmaatschap wordt ook afgedwongen door Azure AD. U bent geen groepen met meer leden synchroniseren, zelfs als u wijzigen of verwijderen van deze regel.
* Als de groep is een **distributiegroep**, moet dit ook e-mailtoegang. Zie [Neem contact op met out of box regels](#contact-out-of-box-rules) voor deze regel wordt afgedwongen.

De volgende groepsobjecten zijn **niet** gesynchroniseerd naar Azure AD:

* `IsPresent([isCriticalSystemObject])`. Zorg ervoor dat veel out-of-box-objecten in Active Directory, zoals de ingebouwde groep administrators, zijn niet gesynchroniseerd.
* `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Verouderde groep wordt gebruikt door DirSync.
* `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Groep van de rol.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. De replicatie slachtoffer objecten niet synchroniseren.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal out-of-box-regels
FSP's zijn gekoppeld aan 'een' (\*)-object in de metaverse. In werkelijkheid gebeurt deze koppeling alleen voor gebruikers en beveiligingsgroepen. Deze configuratie zorgt ervoor dat forest-overschrijdende lidmaatschappen zijn opgelost en correct worden weergegeven in Azure AD.

### <a name="computer-out-of-box-rules"></a>Computer out-of-box-regels
Een nieuw computerobject moet voldoen aan de volgende moeten worden gesynchroniseerd:

* `userCertificate ISNOTNULL`. Alleen Windows 10-computers vullen dit kenmerk. Alle computerobjecten met een waarde in dit kenmerk worden gesynchroniseerd.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Inzicht in het scenario voor out-of-box-regels
In dit voorbeeld gebruiken we een implementatie met één account-forest (A), een bronforest (R) en een Azure AD-directory.

![Afbeelding met scenariobeschrijving](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

In deze configuratie wordt ervan uitgegaan dat er is een ingeschakeld-account in het forest en een uitgeschakeld account in de bron-forest met een gekoppeld postvak.

Ons doel met de standaardconfiguratie is:

* Kenmerken die betrekking hebben op de aanmeldingspagina worden gesynchroniseerd vanuit de forest met het account ingeschakeld.
* Kenmerken die kunnen worden gevonden in de algemene Adreslijst (globale adreslijst) gesynchroniseerd zijn van het forest met het postvak. Als geen postvak kan worden gevonden, wordt een andere forest wordt gebruikt.
* Als een gekoppeld postvak wordt gevonden, moet u de gekoppelde ingeschakeld-account gevonden voor het object dat moet worden geëxporteerd naar Azure AD.

### <a name="synchronization-rule-editor"></a>Synchronization Rule Editor
De configuratie kan worden bekeken en gewijzigd met het hulpprogramma voor synchronisatie regels Editor (SRE) en een snelkoppeling naar vindt u in het startmenu.

![Synchronisatie regeleditor pictogram](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

De SRE is een hulpprogramma van de resource kit en deze met Azure AD Connect-synchronisatie is geïnstalleerd. Als u wilt starten, moet u lid zijn van de groep ADSyncAdmins. Wanneer deze wordt gestart, ziet u ongeveer het volgende:

![Inkomende synchronisatieregels](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

In dit deelvenster ziet u alle synchronisatieregels die zijn gemaakt voor uw configuratie. Elke regel in de tabel is een Synchronisatieregel. Aan de linkerkant onder regeltypen, de twee verschillende typen worden vermeld: binnenkomend en uitgaand. Inkomend en uitgaand is uit de weergave van de metaverse. U gaat voornamelijk focus op de regels voor binnenkomende verbindingen in dit overzicht. De daadwerkelijke lijst met synchronisatieregels is afhankelijk van de gedetecteerde schema in AD. Het forest (fabrikamonline.com) heeft geen eventuele services, zoals Exchange en Lync, en er geen synchronisatie-regels zijn gemaakt voor deze services in de bovenstaande afbeelding. Echter, in de bron-forest (res.fabrikamonline.com) u synchronisatieregels voor deze services. De inhoud van de regels is verschillend, afhankelijk van de versie gedetecteerd. Bijvoorbeeld in een implementatie met Exchange 2013 zijn er meer kenmerkstromen geconfigureerd dan in Exchange 2010-2007.

### <a name="synchronization-rule"></a>Synchronisatieregel
Een Synchronisatieregel is een configuratieobject met een set kenmerken die als een voorwaarde wordt voldaan. Het wordt ook gebruikt om te beschrijven hoe een object in de connectorruimte van een is gerelateerd aan een object in de metaverse, ook wel **join** of **overeen met**. De synchronisatieregels hebben een hogere prioriteitswaarde die aangeeft hoe ze aan elkaar gerelateerd. Een regel voor het synchroniseren met een lagere numerieke waarde heeft een hogere prioriteit en in een kenmerk stroom-conflict hogere prioriteit de conflictoplossing wins.

Bekijk een voorbeeld: de Synchronisatieregel **In uit Active Directory-gebruiker AccountEnabled**. Deze regel in de SRE en selecteer markeren **bewerken**.

Aangezien deze regel een regel voor out-of-box is, ontvangt u een waarschuwing wanneer u de regel opent. U maakt een [wijzigingen aan de regels voor out-of-box](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), zodat u zeker weet wat uw bedoelingen zijn. In dit geval wilt u alleen om de regel weer te geven. Selecteer **Nee**.

![Synchronisatie van regels voor waarschuwing](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Een Synchronisatieregel heeft vier configuratiesecties: beschrijving, Scoping filter, regels voor lid worden en transformaties.

#### <a name="description"></a>Beschrijving
De eerste sectie bevat algemene informatie zoals een naam en beschrijving.

![Beschrijving tabblad synchroon regeleditor ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Ook vindt u informatie over welke verbonden systeem met deze regel is gerelateerd aan, die de object-type in het verbonden systeem dat is van toepassing op en het metaverseobjecttype-object. Het metaverseobjecttype-object is altijd persoon ongeacht wanneer de bron van het type een gebruiker, iNetOrgPerson of neem contact op met is. Het metaverseobjecttype-object moet nooit wijzigen zodat deze is gemaakt als een algemeen type. Het koppelingstype kan worden ingesteld op Join, StickyJoin of inrichten. Deze instelling werkt samen met de sectie deelnemen aan regels en later wordt besproken.

U ziet ook dat deze synchronisatieregel wordt gebruikt voor Wachtwoordsynchronisatie. Als een gebruiker binnen het bereik van deze synchronisatieregel is, is het wachtwoord van on-premises naar cloud (ervan uitgaande dat u hebt de wachtwoordsynchronisatiefunctie ingeschakeld) gesynchroniseerd.

#### <a name="scoping-filter"></a>Bereikfilter
De sectie bereikfilter wordt gebruikt om te configureren als een regel voor synchronisatie moet worden toegepast. Omdat de naam van de Synchronisatieregel u kijkt moet alleen worden toegepast voor ingeschakelde gebruikers aangeeft, de scope is geconfigureerd zodat de AD-kenmerk **userAccountControl** moet niet de bit 2 ingesteld. Als de synchronisatie-engine vindt een gebruiker in AD, geldt dit deze synchronisatie regel wanneer **userAccountControl** is ingesteld op de decimale waarde 512 (ingeschakelde normale gebruiker). De regel is niet van toepassing wanneer de gebruiker heeft **userAccountControl** ingesteld op 514 (uitgeschakelde normale gebruiker).

![Tabblad in de regeleditor Sync scoping ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

Het bereik filter heeft groepen en de componenten die kunnen worden genest. Alle componenten in een groep moeten worden voldaan voor een Synchronisatieregel om toe te passen. Wanneer meerdere groepen zijn gedefinieerd, moet ten minste één groep worden voldaan voor de regel wilt toepassen. Dat wil zeggen, een logische of tussen groepen en een logische wordt geëvalueerd en wordt geëvalueerd binnen een groep. Een voorbeeld van deze configuratie kunt u vinden in de uitgaande Synchronisatieregel **buiten het AAD-groep toevoegen**. Er zijn verschillende filter groepen voor synchronisatie, bijvoorbeeld één voor beveiligingsgroepen (`securityEnabled EQUAL True`) en één voor distributiegroepen (`securityEnabled EQUAL False`).

![Tabblad in de regeleditor Sync scoping ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Deze regel wordt gebruikt voor het definiëren van welke groepen moeten worden ingericht met Azure AD. Distributiegroepen moet een e-mailtoegang kunnen worden gesynchroniseerd met Azure AD, maar voor beveiligingsgroepen een e-mailbericht is niet vereist.

#### <a name="join-rules"></a>Regels toevoegen
Het derde gedeelte wordt gebruikt voor het configureren van de relatie van objecten in het connectorgebied overgebracht naar objecten in de metaverse. De regel die u eerder hebt bekeken heeft geen configuratie voor regels voor lid worden dus in plaats daarvan gaat u om te kijken naar **In uit Active Directory-gebruiker toevoegen**.

![Tabblad regels koppelen in de regeleditor synchronisatie ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

De inhoud van de join-regel is afhankelijk van de overeenkomende optie is geselecteerd in de installatiewizard. De evaluatie begint met een object in de ruimte van de connector bron voor een inkomende regel en elke groep in de join-regels in de reeks wordt geëvalueerd. Als een bronobject wordt geëvalueerd zodat deze overeenkomt met één object in de metaverse met een van de join-regels, worden de objecten toegevoegd. Als alle regels zijn geëvalueerd en er geen overeenkomst is, wordt het koppelingstype op de beschrijvingspagina gebruikt. Als deze configuratie is ingesteld op **inrichten**, en vervolgens een nieuw object in het doel, de metaverse maakt. Om in te richten, een nieuw object aan de metaverse ook bekend als naar staat **project** een object naar de metaverse.

De join-regels worden slechts eenmaal geëvalueerd. Wanneer een object van de ruimte connector en een metaverse-object zijn gekoppeld, blijven deze gekoppelde zolang het bereik van de Synchronisatieregel nog steeds voldaan is.

Bij het evalueren van synchronisatieregels moet slechts één regel voor het synchroniseren met de join-regels die zijn gedefinieerd in het bereik. Als meerdere synchronisatieregels met join regels voor één object worden gevonden, wordt er een fout gegenereerd. Daarom is de beste manier om slechts één regel voor het synchroniseren met join gedefinieerd wanneer er meerdere synchronisatieregels zijn binnen het bereik van een object. In de out-of-box-configuratie voor Azure AD Connect-synchronisatie, deze regels kunnen worden gevonden door te kijken in de naam en vinden die met het woord **Join** aan het einde van de naam. Een Synchronisatieregel zonder join-regels die zijn gedefinieerd, geldt de kenmerkstromen wanneer andere synchronisatieregels de objecten worden samengevoegd of een nieuw object in de doel-ingericht.

Als u de bovenstaande afbeelding bekijkt, ziet u dat de regel probeert te koppelen **objectSID** met **msExchMasterAccountSid** (Exchange) en **msRTCSIP-OriginatorSid** (Lync) Dit is wat we verwachten in de topologie van een account-resource forest. U dezelfde regel op alle forests. De veronderstelling is dat elk forest een account of een bron-forest kan worden. Deze configuratie werkt ook als er accounts die bevinden zich in één forest en hoeft niet te worden toegevoegd.

#### <a name="transformations"></a>Transformaties
Het onderdeel transformatie gedefinieerd alle kenmerkstromen die betrekking hebben op het doelobject wanneer de objecten zijn gekoppeld en het bereik-filter is voldaan. Ga terug naar de **In uit Active Directory-gebruiker AccountEnabled** Synchronisatieregel, vindt u de volgende transformaties:

![Transformaties tabblad synchroon regeleditor ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Zouden een ingeschakeld-account in het forest en een uitgeschakeld account vinden in de bron-forest met Exchange en Lync-instellingen om deze configuratie in de context is in een implementatie met Account-Resource forest. De Synchronisatieregel die u naar kijkt bevat de kenmerken die vereist zijn voor aanmelding en deze kenmerken moeten stromen van het forest wanneer er een ingeschakeld-account. Deze kenmerkstromen worden samen in één Synchronisatieregel geplaatst.

Een transformatie kan verschillende typen hebben: constante, Direct en -expressie.

* Een constante stroom loopt altijd een waarde vastgelegd. In het geval is het altijd de waarde ingesteld **True** in het metaverse-kenmerk met de naam **accountEnabled**.
* Een directe stroom loopt altijd de waarde van het kenmerk in de bron met het kenmerk target als-is.
* De derde stroomtype expressie is en kunt u meer geavanceerde configuraties.

De expressietaal is VBA (Visual Basic for Applications), zodat mensen met ervaring van Microsoft Office of VBScript herkent de indeling. Kenmerken zijn tussen vierkante haken, [attributeName]. Kenmerknamen en functienamen zijn hoofdlettergevoelig, maar de regeleditor synchronisatie evalueert de expressies en geef een waarschuwing als de expressie is niet geldig. Alle expressies worden uitgedrukt in op één regel met geneste functies. Om weer te geven de kracht van de configuratie-taal, is dit de stroom voor pwdLastSet, maar met aanvullende opmerkingen ingevoegd:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Zie [Understanding declaratieve inrichting expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) voor meer informatie over de expressietaal voor kenmerkstromen.

### <a name="precedence"></a>Prioriteit
U hebt nu een aantal afzonderlijke synchronisatieregels bekeken, maar de regels werken samen in de configuratie. In sommige gevallen wordt een kenmerkwaarde van meerdere synchronisatieregels bijgedragen tot hetzelfde doelkenmerk. In dit geval wordt Kenmerkprioriteit gebruikt om te bepalen welk kenmerk wins. Bekijk een voorbeeld: het kenmerk sourceAnchor. Dit kenmerk is een belangrijk kenmerk te kunnen aanmelden bij Azure AD. U vindt een kenmerkstroom voor dit kenmerk in twee verschillende synchronisatieregels **In uit Active Directory-gebruiker AccountEnabled** en **In uit Active Directory-gebruiker algemene**. Als gevolg van de prioriteit van Synchronisatieregel, is het kenmerk sourceAnchor bijgedragen uit het forest met een ingeschakelde account eerst wanneer er meerdere objecten samengevoegd tot het metaverse-object. Als er geen ingeschakelde rekeningen zijn, wordt de synchronisatie-engine maakt gebruik van de Synchronisatieregel catch all- **In uit Active Directory-gebruiker algemene**. Deze configuratie zorgt ervoor dat ook voor accounts die zijn uitgeschakeld, is er nog steeds een sourceAnchor.

![Inkomende synchronisatieregels](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

De prioriteit voor synchronisatieregels is ingesteld in groepen met de installatiewizard. Alle regels in een groep dezelfde naam hebben, maar ze zijn verbonden met verschillende verbonden mappen. De installatiewizard geeft de regel **In uit Active Directory-gebruiker toevoegen** hoogste prioriteit en dit doorloopt over alle verbonden AD-mappen. Vervolgens gaat door met de volgende groepen van regels in een vooraf gedefinieerde volgorde. In een groep, worden de regels in de volgorde waarin die de Connectors zijn toegevoegd in de wizard toegevoegd. Als een andere Connector met de wizard wordt toegevoegd, de synchronisatieregels wordt gewijzigd en de nieuwe Connector regels op de laatste worden ingevoegd in elke groep.

### <a name="putting-it-all-together"></a>Kort samengevat
We nu weten voldoende van synchronisatieregels om te begrijpen hoe de configuratie werkt met de andere synchronisatieregels. Als u een gebruiker en de kenmerken die worden aangeleverd voor de metaverse bekijkt, worden de regels worden toegepast in de volgende volgorde:

| Naam | Opmerking |
|:--- |:--- |
| In uit Active Directory-gebruiker koppelen |Regel voor het lidmaatschap van de connector ruimteobjecten van het metaverse. |
| In uit Active Directory-gebruikersaccount is ingeschakeld |Kenmerken die vereist zijn voor aanmelding bij Azure AD en Office 365. We willen dat deze kenmerken van het account ingeschakeld. |
| In uit Active Directory-gebruiker gemeenschappelijke in Exchange |Gevonden kenmerken in de globale adreslijst. We ervan uitgaan dat de kwaliteit van de gegevens wordt aanbevolen in het forest waar we postvak van de gebruiker hebt gevonden. |
| In uit Active Directory-gebruiker gemeenschappelijke |Gevonden kenmerken in de globale adreslijst. Als er een postvak gevonden, kan geen andere gekoppelde objecten de kenmerkwaarde bijdragen. |
| In uit Active Directory-gebruiker Exchange |Is alleen aanwezig als Exchange is gedetecteerd. Deze loopt alle kenmerken van de infrastructuur Exchange. |
| In uit Active Directory-gebruiker Lync |Alleen bestaat of Lync is gedetecteerd. Deze loopt alle kenmerken van de infrastructuur Lync. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de configuratiemodel in [Understanding declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Meer informatie over de expressietaal in [Understanding declaratieve inrichting expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
* Doorgaan met het lezen van de werking van de configuratie van de out-of-box in [inzicht krijgen in gebruikers en contactpersonen](active-directory-aadconnectsync-understanding-users-and-contacts.md)
* Informatie over het maken van een praktische wijzigen met behulp van declaratieve inrichting [hoe een wijziging aanbrengt in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

