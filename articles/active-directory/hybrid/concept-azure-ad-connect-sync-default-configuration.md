---
title: 'Azure AD Connect-synchronisatie: Inzicht in de standaardconfiguratie | Microsoft Docs'
description: Dit artikel beschrijft de standaardconfiguratie in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ed876f22-6892-4b9d-acbe-6a2d112f1cd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 6de48b0f4c7c69ab0c6acb4099234b853d2c1523
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54478566"
---
# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect-synchronisatie: Inzicht in de standaardconfiguratie
Dit artikel wordt uitgelegd dat de van de out-of-box-configuratieregels. Deze documenten in de regels en hoe deze regels invloed hebben op de configuratie. Ook helpt u bij de standaardconfiguratie van Azure AD Connect-synchronisatie. Het doel is dat de lezer hoe de configuratiemodel begrijpt, met de naam declaratieve inrichting werkt in een voorbeeld van een echte. In dit artikel wordt ervan uitgegaan dat u al hebt geïnstalleerd en Azure AD Connect-synchronisatie met de installatiewizard configureren.

Lees voor meer informatie over de details van het configuratiemodel, [Understanding declaratieve inrichting](concept-azure-ad-connect-sync-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Out-of-box-regels van on-premises naar Azure AD
De volgende expressies kunnen worden gevonden in de out-of-box-configuratie.

### <a name="user-out-of-box-rules"></a>Gebruiker out-of-box-regels
Deze regels zijn ook van toepassing op het type iNetOrgPerson-object.

Een gebruikersobject moet voldoen aan de volgende moeten worden gesynchroniseerd:

* Beschikken over een sourceAnchor.
* SourceAnchor wijzigen niet nadat het object is gemaakt in Azure AD. Als de waarde gewijzigd on-premises wordt, stopt het object synchroniseren totdat het sourceAnchor wordt gewijzigd naar de vorige waarde.
* Hebben moet het kenmerk accountEnabled (userAccountControl) ingevuld. Met een on-premises Active Directory, dit kenmerk is altijd aanwezig zijn en is ingevuld.

De volgende objecten zijn **niet** gesynchroniseerd met Azure AD:

* `IsPresent([isCriticalSystemObject])`. Zorg ervoor dat veel out-of-box-objecten in Active Directory, zoals het ingebouwde administrator-account, zijn niet gesynchroniseerd.
* `IsPresent([sAMAccountName]) = False`. Zorg ervoor dat op gebruikersobjecten met geen kenmerk sAMAccountName zijn niet gesynchroniseerd. In dit geval zou alleen nagenoeg gebeuren in een domein een upgrade uitgevoerd van NT4.
* `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Het serviceaccount dat wordt gebruikt door Azure AD Connect-synchronisatie en de eerdere versies niet synchroniseren.
* Niet synchroniseren met Exchange-accounts die niet in Exchange Online werken.
  * `[sAMAccountName] = "SUPPORT_388945a0"`
  * `Left([mailNickname], 14) = "SystemMailbox{"`
  * `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
  * `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
* Objecten die niet in Exchange Online werken niet synchroniseren.
  `CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
  Deze bitmasker (& H21C07000) filtert u de volgende objecten:
  * E-mailadres openbare map (In Preview vanaf versie 1.1.524.0 en hoger)
  * Eigen Postvak
  * Postvak Database-postbus (systeempostvak)
  * Universele beveiligingsgroep (wouldn't toepassen voor een gebruiker, maar aanwezig is voor verouderde redenen)
  * Niet-universele groep (wouldn't toepassen voor een gebruiker, maar aanwezig is voor verouderde redenen)
  * Plan voor Postvak
  * Detectie-Postvak
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Replicatie slachtoffer objecten niet gesynchroniseerd.

De volgende regels voor het kenmerk zijn van toepassing:

* `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. Het kenmerk sourceAnchor is niet van een gekoppeld postvak bijgedragen. Ervan wordt uitgegaan dat als een gekoppeld postvak is gevonden, de daadwerkelijke account deel uitmaakt van later opnieuw.
* Exchange gerelateerde kenmerken alleen gesynchroniseerd als het kenmerk **mailNickName** heeft een waarde.
* Wanneer er meerdere forests, worden de kenmerken in de volgende volgorde verbruikt:
  1. Kenmerken met betrekking tot aanmelden (bijvoorbeeld userPrincipalName) worden bijgedragen van het forest met een ingeschakeld account.
  2. Kenmerken die kunnen worden gevonden in een Exchange-GAL (globale adreslijst) worden van het forest met een Exchange-postvak bijgedragen.
  3. Als er geen postvak kan worden gevonden, klikt u vervolgens deze kenmerken kunnen afkomstig zijn van een forest.
  4. Exchange gerelateerde kenmerken (technische kenmerken niet zichtbaar in de GAL) worden aangeleverd vanuit de forest waar `mailNickname ISNOTNULL`.
  5. Als er meerdere forests die u zou een van deze regels voldoen, wordt de volgorde voor het maken van (datum/tijd) van de Connectors (forests) gebruikt om te bepalen welk forest deel uitmaakt van de kenmerken.

### <a name="contact-out-of-box-rules"></a>Neem contact op met out-of-box-regels
Een verbindingsobject moet voldoen aan de volgende moeten worden gesynchroniseerd:

* Het contact moet worden ingeschakeld voor e-mail. Het is geverifieerd met de volgende regels:
  * `IsPresent([proxyAddresses]) = True)`. Het kenmerk proxyAddresses moet invullen.
  * Een primaire e-mailadres kan worden gevonden in het proxyAddresses-kenmerk of het e-mailkenmerk. De aanwezigheid van een \@ wordt gebruikt om te controleren of de inhoud een e-mailadres. Een van deze twee regels moet worden geëvalueerd op waar.
    * `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Is er een vermelding met ' SMTP: "en als er, kunt u een \@ vindt u in de tekenreeks?
    * `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Is het e-mailkenmerk ingevuld en als dat is, kunt een \@ vindt u in de tekenreeks?

De volgende contact op met objecten zijn **niet** gesynchroniseerd met Azure AD:

* `IsPresent([isCriticalSystemObject])`. Zorg ervoor dat er geen contact op met objecten die als kritiek zijn gemarkeerd zijn gesynchroniseerd. Mag niet een met een standaardconfiguratie.
* `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
* `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Deze objecten goed niet in Exchange Online.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Replicatie slachtoffer objecten niet gesynchroniseerd.

### <a name="group-out-of-box-rules"></a>Groep out-of-box-regels
Een group-object moet voldoen aan de volgende moeten worden gesynchroniseerd:

* Moet minder dan 50.000 leden hebben. Deze waarde is het aantal leden in de on-premises-groep.
  * Als er meer leden voordat synchronisatie de eerste keer wordt gestart, wordt de groep niet gesynchroniseerd.
  * Als het aantal leden laten wanneer deze is in eerste instantie gemaakt groeien, klikt u vervolgens wanneer het 50.000 leden bereikt stopt totdat het aantal lidmaatschap lager dan 50.000 opnieuw is synchroniseren.
  * Opmerking: Het aantal 50.000 lidmaatschap wordt ook afgedwongen door Azure AD. U bent niet kunnen synchroniseren van groepen met meer leden, zelfs als u wijzigen of verwijderen van deze regel.
* Als de groep is een **distributiegroep**, en vervolgens het moet ook de e-mailtoegang. Zie [Neem contact op met out-of-box regels](#contact-out-of-box-rules) voor deze regel wordt afgedwongen.

De volgende groepsobjecten zijn **niet** gesynchroniseerd met Azure AD:

* `IsPresent([isCriticalSystemObject])`. Zorg ervoor dat veel out-of-box-objecten in Active Directory, zoals de ingebouwde groep administrators, zijn niet gesynchroniseerd.
* `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Verouderde groep die wordt gebruikt door DirSync.
* `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Rolgroepen.
* `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Replicatie slachtoffer objecten niet gesynchroniseerd.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal out-of-box-regels
FSP's zijn gekoppeld aan 'een' (\*)-object in de metaverse. In werkelijkheid gebeurt deze samenvoeging alleen voor gebruikers en beveiligingsgroepen. Deze configuratie zorgt ervoor dat forest-overschrijdende lidmaatschappen zijn opgelost en correct wordt weergegeven in Azure AD.

### <a name="computer-out-of-box-rules"></a>Computer out-of-box-regels
Een nieuw computerobject moet voldoen aan de volgende moeten worden gesynchroniseerd:

* `userCertificate ISNOTNULL`. Alleen Windows 10-computers vullen dit kenmerk. Alle computerobjecten met een waarde in dit kenmerk worden gesynchroniseerd.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Informatie over de regels voor out-of-box-scenario
In dit voorbeeld gebruiken we een implementatie met één accountforest (A), een bron-forest (R) en een Azure AD-directory.

![Afbeelding met scenariobeschrijving](./media/concept-azure-ad-connect-sync-default-configuration/scenario.png)

In deze configuratie wordt ervan uitgegaan dat er is een ingeschakeld account in het forest en een uitgeschakeld account in de bron-forest met een gekoppeld postvak.

Het is ons doel met de standaard-configuratie:

* Kenmerken met betrekking tot aanmelden worden gesynchroniseerd vanuit het forest met de ingeschakeld account.
* Kenmerken die kunnen worden gevonden in de GAL (globale adreslijst) worden gesynchroniseerd vanuit het forest met de postvak. Als er geen postvak kan worden gevonden, wordt een andere forest wordt gebruikt.
* Als een gekoppeld postvak wordt gevonden, moet u de gekoppelde ingeschakeld account gevonden voor het object moet worden geëxporteerd naar Azure AD.

### <a name="synchronization-rule-editor"></a>Synchronization Rule Editor
De configuratie kan worden bekeken en gewijzigd met het hulpprogramma voor synchronisatie regels Editor (SRE) en een snelkoppeling waarmee u deze kunt u vinden in het startmenu.

![Pictogram van de Synchronization Rules Editor](./media/concept-azure-ad-connect-sync-default-configuration/sre.png)

De SRE is een resource kit-hulpprogramma en met Azure AD Connect-synchronisatie is geïnstalleerd. Om te kunnen starten, moet u lid zijn van de groep ADSyncAdmins zijn. Wanneer deze wordt gestart, ziet er ongeveer als volgt:

![Synchronisatieregels inkomend](./media/concept-azure-ad-connect-sync-default-configuration/syncrulesinbound.png)

In dit deelvenster ziet u alle synchronisatieregels die zijn gemaakt voor uw configuratie. Elke regel in de tabel is een regel voor synchronisatie. Aan de linkerkant onder regeltypen staan de twee verschillende typen: Binnenkomend en uitgaand. Inkomend en uitgaand afkomstig is van de weergave van de metaverse. Voornamelijk gaat u zich richten op de regels voor binnenkomende verbindingen in dit overzicht. De daadwerkelijke lijst synchronisatieregels is afhankelijk van de gedetecteerde schema in AD. De accountforest (fabrikamonline.com) beschikt niet over services, zoals Exchange en Lync, en geen synchronisatieregels zijn gemaakt voor deze services in de bovenstaande afbeelding. Echter, in de bron-forest (res.fabrikamonline.com) vindt u synchronisatieregels voor deze services. De inhoud van de regels is verschillend, afhankelijk van de versie gedetecteerd. Bijvoorbeeld in een implementatie met Exchange 2013 zijn er meer kenmerkstromen geconfigureerd dan in Exchange 2010/2007.

### <a name="synchronization-rule"></a>Synchronisatieregel
Een Synchronisatieregel is een configuratieobject met een set kenmerken weergegeven als een voorwaarde wordt voldaan. Het wordt ook gebruikt om te beschrijven hoe een object in een connectorgebied is gerelateerd aan een object in de metaverse, ook wel **join** of **overeenkomen met**. De synchronisatieregels hebben een hogere prioriteitswaarde die aangeeft hoe ze aan elkaar zijn gerelateerd. Een regel voor synchronisatie met een lagere numerieke waarde heeft een hogere prioriteit en een kenmerk stroom conflict optreedt, hogere prioriteit de conflictoplossing wins.

Bekijk een voorbeeld: de Synchronisatieregel **In uit Active Directory-gebruiker AccountEnabled**. Deze regel in de SRE en selecteer markeren **bewerken**.

Omdat met deze regel een regel voor out-of-box wordt, ontvangt u een waarschuwing weergegeven bij het openen van de regel. U maakt een [wijzigingen aan de regels voor out-of-box](how-to-connect-sync-best-practices-changing-default-configuration.md), zodat u wordt gevraagd wat uw intenties zijn. In dit geval wilt u alleen om de regel weer te geven. Selecteer **Nee**.

![Synchronisatie van regels voor waarschuwing](./media/concept-azure-ad-connect-sync-default-configuration/warningeditrule.png)

Een regel voor synchronisatie heeft vier configuratiesecties: Beschrijving, Scoping-filter, lid worden van regels en transformaties.

#### <a name="description"></a>Description
De eerste sectie bevat algemene informatie, zoals een naam en beschrijving.

![Beschrijving van tabblad synchroon regeleditor ](./media/concept-azure-ad-connect-sync-default-configuration/syncruledescription.png)

Ook vindt u informatie over welke verbonden systeem met deze regel is gerelateerd aan, waarmee het verbonden systeem dat is van toepassing op type en het metaverse-objecttype-object. Het metaverse-objecttype is altijd persoon ongeacht wanneer het brontype van het object een gebruiker, iNetOrgPerson of neem contact op met is. Het metaverse-objecttype moet nooit wijzigen zodat deze als een generiek type is gemaakt. Het koppelingstype kan worden ingesteld op Join, StickyJoin of inrichten. Deze instelling werkt samen met de sectie met regels voor lid worden en wordt later behandeld.

U ziet ook dat deze synchronisatieregel wordt gebruikt voor Wachtwoordsynchronisatie. Als een gebruiker binnen het bereik van deze synchronisatieregel is, wordt het wachtwoord van on-premises gesynchroniseerd naar de cloud (ervan uitgaande dat u de functie van de synchronisatie van wachtwoord hebt ingeschakeld).

#### <a name="scoping-filter"></a>Bereikfilter
De sectie bereikfilter voor wordt gebruikt om te configureren wanneer een regel voor synchronisatie moet worden toegepast. Omdat de naam van de Synchronisatieregel u kijkt moet alleen worden toegepast voor ingeschakelde gebruikers aangeeft, het bereik is geconfigureerd zodat de AD-kenmerk **userAccountControl** moet hebt niet de bit 2 ingesteld. Als de synchronisatie-engine wordt een gebruiker in AD gevonden, wordt deze synchronisatie toegepast wanneer regel **userAccountControl** is ingesteld op de decimale waarde 512 (ingeschakeld normale gebruiker). De regel is niet van toepassing wanneer de gebruiker heeft **userAccountControl** ingesteld op 514 (uitgeschakelde normale gebruiker).

![Scoping tabblad in de editor voor synchronisatie-regel ](./media/concept-azure-ad-connect-sync-default-configuration/syncrulescopingfilter.png)

De bereikfilter heeft groepen en -componenten die kunnen worden genest. Alle componenten binnen een groep moeten worden voldaan voor een regel voor synchronisatie om toe te passen. Wanneer er meerdere groepen zijn gedefinieerd, moet ten minste één groep worden voldaan voor de regel wilt toepassen. Dat wil zeggen, een logische of tussen groepen en een logische wordt geëvalueerd en wordt geëvalueerd binnen een groep. Een voorbeeld van deze configuratie kunt u vinden in de regel voor uitgaande synchronisatie **Out voor AAD-groep Join**. Er zijn verschillende filter groepen voor synchronisatie, bijvoorbeeld één voor beveiligingsgroepen (`securityEnabled EQUAL True`) en één voor distributiegroepen (`securityEnabled EQUAL False`).

![Scoping tabblad in de editor voor synchronisatie-regel ](./media/concept-azure-ad-connect-sync-default-configuration/syncrulescopingfilterout.png)

Deze regel wordt gebruikt om te definiëren welke groepen moeten worden ingericht met Azure AD. Distributiegroepen moeten zijn ingeschakeld om te worden gesynchroniseerd met Azure AD e-mail, maar voor beveiligingsgroepen een e-mailbericht is niet vereist.

#### <a name="join-rules"></a>Regels toevoegen
De derde sectie wordt gebruikt om te configureren hoe objecten in het connectorgebied zich verhouden tot objecten in de metaverse. De regel die u eerder hebt bekeken heeft geen geen configuratie voor regels voor lid worden, dus in plaats daarvan u gaat kijken **In uit Active Directory-gebruiker toevoegen**.

![Tabblad regels in de editor voor synchronisatie-regel toevoegen ](./media/concept-azure-ad-connect-sync-default-configuration/syncrulejoinrules.png)

De inhoud van de join-regel, is afhankelijk van de overeenkomende optie is geselecteerd in de installatiewizard. De evaluatie begint met een object in het connectorgebied bron voor een inkomende regel en elke groep in de join-regels in de reeks wordt geëvalueerd. Als een bronobject wordt vastgesteld zodat deze overeenkomt met één object in de metaverse met behulp van een van de regels voor lid worden, worden de objecten zijn gekoppeld. Als alle regels zijn geëvalueerd en er geen overeenkomst is, wordt het koppelingstype op de beschrijvingspagina gebruikt. Als deze configuratie is ingesteld op **inrichten**, en vervolgens een nieuw object wordt gemaakt in het doel, de metaverse. Voor het inrichten van een nieuw object naar de metaverse staat ook bekend als aan **project** een object naar de metaverse.

De join-regels zijn slechts één keer geëvalueerd. Wanneer een object van connector space en metaverse-object zijn gekoppeld, blijven deze is toegevoegd aan, zolang het bereik van de regel voor synchronisatie is nog steeds voldaan aan.

Bij het evalueren van synchronisatieregels moet slechts één regel voor synchronisatie met regels voor lid worden gedefinieerd binnen het bereik. Als meerdere synchronisatieregels met regels voor lid worden voor één object worden gevonden, wordt er een fout opgetreden. De aanbevolen procedure is om deze reden hebben slechts één regel voor synchronisatie met join gedefinieerd wanneer er meerdere synchronisatieregels zijn binnen het bereik van een object. In de out-of-box-configuratie voor Azure AD Connect-synchronisatie, deze regels kunnen vinden door te kijken naar de naam en degenen waarbij het woord **Join** aan het einde van de naam. Een regel zonder een join-regels gedefinieerd voor het synchroniseren van toepassing is de kenmerkstromen wanneer andere synchronisatieregels de objecten zijn samengevoegd of een nieuw object in de doel-ingericht.

Als u de bovenstaande afbeelding bekijkt, kunt u zien dat de regel is proberen toe te voegen **objectSID** met **msExchMasterAccountSid** (Exchange) en **msRTCSIP-OriginatorSid** (Lync) Dit is wat we verwachten dat in de topologie van een account-resource forest. U vindt de dezelfde regel op alle forests. De veronderstelling is dat elk forest een account of een bron-forest kan worden. Deze configuratie werkt ook als u accounts die bevinden zich in één forest en hoeft niet te worden toegevoegd.

#### <a name="transformations"></a>Transformaties
Het gedeelte transformatie definieert alle kenmerkstromen die betrekking hebben op het beoogde doelobject als lid zijn van de objecten en het filter voor zoekbereik is voldaan. Ga terug naar de **In uit Active Directory-gebruiker AccountEnabled** Synchronisatieregel, vindt u de volgende transformaties:

![Transformaties tabblad synchroon regeleditor ](./media/concept-azure-ad-connect-sync-default-configuration/syncruletransformations.png)

Om deze configuratie in de context in een implementatie met een Account-Resource-forests is het waarschijnlijk een ingeschakeld account in het forest en een uitgeschakeld account vinden in de bron-forest met instellingen voor Exchange en Lync. De Synchronisatieregel die u wilt de kenmerken die vereist zijn voor aanmelding bij bevat en deze kenmerken moeten stromen van het forest waarbij er een ingeschakeld account. Deze kenmerkstromen zijn in één regel voor synchronisatie samengesteld.

Een transformatie kan verschillende typen hebben: Een constante, Direct, and -expressie.

* Een constante stroom stromen altijd een waarde vastgelegd. In dit geval wordt de waarde altijd ingesteld **waar** in de metaverse-kenmerk met de naam **accountEnabled**.
* Een directe stroom altijd de waarde van het kenmerk in de bron naar het doelkenmerk als stromen-is.
* Het derde type van de stroom is expressie en kunt voor meer geavanceerde configuraties.

De expressietaal is VBA (Visual Basic for Applications), zodat mensen met de ervaring van Microsoft Office of VBScript herkent de indeling. Kenmerken zijn tussen vierkante haken, [attributeName]. Kenmerknamen van en de functie zijn hoofdlettergevoelig, maar de Synchronization Rules Editor evalueert de expressies en geef een waarschuwing als de expressie is niet geldig. Alle expressies worden uitgedrukt op één regel met geneste functies. Als u wilt weergeven van de kracht van de configuratie-taal, volgt de stroom voor pwdLastSet, maar met aanvullende opmerkingen ingevoegde:

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

Zie [Understanding declaratieve inrichting expressies](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md) voor meer informatie over de expressietaal voor kenmerkstromen.

### <a name="precedence"></a>Prioriteit
U hebt nu een afzonderlijke synchronisatieregels bekeken, maar de regels werken samen in de configuratie. In sommige gevallen een kenmerkwaarde bijgedragen uit meerdere synchronisatieregels voor met de dezelfde target-kenmerk. In dit geval wordt Kenmerkprioriteit gebruikt om te bepalen welk kenmerk wins. Bekijk een voorbeeld: het kenmerk sourceAnchor. Dit kenmerk is een belangrijk kenmerk te kunnen aanmelden bij Azure AD. U kunt een kenmerkstroom vinden voor dit kenmerk in twee verschillende synchronisatieregels **In uit Active Directory-gebruiker AccountEnabled** en **In uit Active Directory-gebruiker algemene**. Vanwege de prioriteit van Synchronisatieregel, is het kenmerk sourceAnchor bijgedragen van het forest met een ingeschakeld account eerst wanneer er meerdere objecten samengevoegd tot het metaverse-object. Als er geen accounts ingeschakeld zijn, wordt de synchronisatie-engine maakt gebruik van de Synchronisatieregel catch-all **In uit Active Directory-gebruiker algemene**. Deze configuratie zorgt ervoor dat ook voor accounts die zijn uitgeschakeld, er nog steeds een sourceAnchor.

![Synchronisatieregels inkomend](./media/concept-azure-ad-connect-sync-default-configuration/syncrulesinbound.png)

De prioriteit voor synchronisatieregels is ingesteld in groepen door de installatiewizard. Alle regels in een groep dezelfde naam hebben, maar ze zijn verbonden met verschillende verbonden mappen. De installatiewizard van de regel biedt **In uit Active Directory-gebruiker toevoegen** hoogste prioriteit en het doorloopt over alle zijn verbonden AD-mappen. Vervolgens gaat door met de volgende groepen van de regels in een vooraf gedefinieerde volgorde. Binnen een groep, worden de regels in de volgorde waarin die de Connectors zijn toegevoegd in de wizard toegevoegd. Als een andere Connector wordt toegevoegd met de wizard, de volgorde van de synchronisatieregels wordt gewijzigd en de nieuwe Connector regels laatst in elke groep worden ingevoegd.

### <a name="putting-it-all-together"></a>Alles samenvoegen
Weten we nu voldoende van synchronisatieregels kunnen om te begrijpen hoe de configuratie werkt met de andere synchronisatieregels. Als u een gebruiker en de kenmerken die zijn bijgedragen aan de metaverse bekijkt, worden de regels worden toegepast in de volgende volgorde:

| Name | Opmerking |
|:--- |:--- |
| In uit Active Directory: gebruikers lid worden |Regel voor het lidmaatschap van de connector space objecten met de metaverse. |
| In uit Active Directory-gebruikersaccount is ingeschakeld |Kenmerken die vereist zijn voor aanmelden bij Azure AD en Office 365. We willen deze kenmerken van het account ingeschakeld. |
| In uit Active Directory-gebruiker gemeenschappelijke in Exchange |Kenmerken gevonden in de globale adreslijst. We ervan uitgaan dat de kwaliteit van de gegevens wordt aanbevolen in het forest waar we hebben ontdekt dat postvak van de gebruiker. |
| In uit Active Directory-gebruiker gemeenschappelijke |Kenmerken gevonden in de globale adreslijst. Als we een postvak niet hebt gevonden, kan een andere gekoppelde object waarde van het kenmerk bijdragen. |
| In uit Active Directory-gebruiker Exchange |Alleen bestaat als Exchange is gedetecteerd. Hiervandaan stroomt het alle kenmerken van de infrastructuur voor Exchange. |
| In uit Active Directory-gebruiker Lync |Alleen bestaat als Lync is gedetecteerd. Hiervandaan stroomt het alle kenmerken van de infrastructuur Lync. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de configuratiemodel in [Understanding declaratieve inrichting](concept-azure-ad-connect-sync-declarative-provisioning.md).
* Meer informatie over de expressietaal in [Understanding declaratieve inrichting expressies](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md).
* Lees ook de werking van de configuratie van de out-of-box in [inzicht krijgen in gebruikers en contactpersonen](concept-azure-ad-connect-sync-user-and-contacts.md)
* Zien hoe u een praktische wijzigen met behulp van declaratieve inrichting [hoe u een wijziging in de standaardconfiguratie](how-to-connect-sync-change-the-configuration.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)

