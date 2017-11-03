---
title: Azure AD SSPR met wachtwoord terugschrijven | Microsoft Docs
description: Met behulp van Azure AD en Azure AD Connect wachtwoorden terugschrijven naar on-premises directory
services: active-directory
keywords: Wachtwoordbeheer Active directory, wachtwoordbeheer, Azure AD self service voor wachtwoordherstel
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 8ce4d6d9024dc4ce3956220eb0678a6295b0b7ab
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="password-writeback-overview"></a>Overzicht van de Write-back van wachtwoord

Wachtwoord terugschrijven kunt u Azure AD om terugschrijven van wachtwoorden naar uw lokale Active Directory configureren. Verwijdert deze de noodzaak instellen en beheren van een complexe on-premises-oplossing zelf uw wachtwoord opnieuw instellen en biedt een handige manier cloud-gebaseerde voor uw gebruikers opnieuw in te stellen hun on-premises wachtwoorden waar ze ook zijn. Wachtwoord terugschrijven is een onderdeel van [Azure Active Directory Connect](./connect/active-directory-aadconnect.md) die kunnen worden ingeschakeld en gebruikt door bestaande Premium-abonnees [Azure Active Directory-edities](active-directory-editions.md).

Wachtwoord terugschrijven biedt de volgende functies:

* **Vertraging feedback nul** -wachtwoord terugschrijven is een synchrone bewerking. Uw gebruikers zijn onmiddellijk een melding als hun wachtwoord niet voldeed aan beleid of kon niet worden ingesteld of gewijzigd voor een of andere reden.
* **Ondersteunt het opnieuw instellen van wachtwoorden voor gebruikers met AD FS of andere technologieën federation** -met terugschrijven van wachtwoorden, zolang de federatieve gebruikersaccounts worden gesynchroniseerd naar Azure AD-tenant ze kunnen voor het beheren van hun on-premises AD-wachtwoorden vanuit de cloud.
* **Ondersteunt het opnieuw instellen van wachtwoorden voor gebruikers met behulp van [wachtwoordhashsynchronisatie](./connect/active-directory-aadconnectsync-implement-password-synchronization.md)**  : wanneer u het wachtwoord opnieuw instellen van service detecteert dat een gesynchroniseerde gebruikersaccount is ingeschakeld voor wachtwoordhashsynchronisatie, we tegelijkertijd zowel dit account on-premises en cloud-wachtwoord instellen.
* **Ondersteunt het wijzigen van wachtwoorden van het toegangsvenster en Office 365** : wanneer federatieve of wachtwoord is gesynchroniseerd gebruikers hun wachtwoorden verlopen of niet-verlopen wijzigen we deze wachtwoorden terugschrijven naar uw lokale AD-omgeving worden geleverd.
* **Ondersteunt het terugschrijven van wachtwoorden wanneer een beheerder deze opnieuw vanuit de Azure-portal** : wanneer een beheerder stelt het wachtwoord van een gebruiker in de [Azure-portal](https://portal.azure.com), als die gebruiker is gefedereerd of wachtwoord is gesynchroniseerd, moet we het wachtwoord dat de beheerder wordt geselecteerd op uw lokale AD ook is ingesteld. Dit wordt momenteel niet ondersteund in de Office-beheerportal.
* **Dwingt uw on-premises AD-wachtwoordbeleid** : wanneer een gebruiker het wachtwoord opnieuw instelt zorgen wij ervoor dat het voldoet aan uw on-premises Active Directory-beleid alvorens toe te wijzen aan die directory. Dit omvat de geschiedenis, complexiteit, leeftijd, wachtwoordfilters en alle andere wachtwoordbeperkingen die u hebt gedefinieerd in uw lokale AD.
* **Alle firewallregels voor binnenkomend verkeer geen vereist** -wachtwoord terugschrijven een Azure Service Bus relay gebruikt als een onderliggende communicatiekanaal, wat betekent dat u niet hoeft te openen van poorten voor inkomend verkeer op uw firewall voor deze functie te gebruiken.
* **Wordt niet ondersteund voor gebruikersaccounts die zijn opgeslagen in de beveiligde groepen in uw lokale Active Directory** - Zie voor meer informatie over de beveiligde groepen [beveiligde Accounts en groepen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>De werking van wachtwoord terugschrijven

Wanneer een federatieve of het wachtwoord-hash gesynchroniseerde gebruiker wordt geleverd opnieuw instellen of hun wachtwoord wijzigen in de cloud, gebeurt het volgende:

1. We controleren om te zien welke type wachtwoord op de gebruiker heeft.
    * Als we wordt het wachtwoord beheerd on-premises
        * We controleren of als de Write-back-service is en wordt uitgevoerd, als het, we aan de gebruiker laten gaan
        * Als de Write-back-service niet is, wordt de gebruiker melden dat hun wachtwoord kan niet worden nu opnieuw instellen
2. Vervolgens de gebruiker geeft de juiste verificatie-poorten en het wachtwoord opnieuw instellen van scherm bereikt.
3. De gebruiker selecteert een nieuw wachtwoord en bevestigt het.
4. Wanneer u op verzenden klikt, wordt het leesbare wachtwoord versleutelen met een symmetrische sleutel die is gemaakt tijdens de installatie Write-back van.
5. Nadat het wachtwoord versleutelt, opnemen we in een nettolading die via een HTTPS-kanaal wordt verzonden naar uw tenantspecifieke service bus relay (die we ook voor u ingesteld tijdens de installatie writeback). Deze relay is beveiligd met een willekeurig gegenereerd wachtwoord dat alleen de lokale installatie kent.
6. Zodra het bericht servicebus bereikt, wordt het wachtwoord opnieuw instellen van eindpunt automatisch ontwaakt en ziet dat er een aanvraag in behandeling.
7. De service zoekt vervolgens naar de gebruiker in kwestie met behulp van de cloud ankerkenmerk. Voor deze zoekactie mislukt:

    * Het gebruikersobject moet bestaan in de ruimte van AD-connector
    * Het gebruikersobject moet worden gekoppeld aan het bijbehorende MV-object
    * Het gebruikersobject moet worden gekoppeld aan het bijbehorende object van de AAD-connector.
    * De koppeling van AD-connector-object naar MV moet hebben tot de synchronisatieregel `Microsoft.InfromADUserAccountEnabled.xxx` op de koppeling. <br> <br>
    Wanneer de aanroep wordt geleverd vanuit de cloud, wordt de synchronisatie-engine gebruikt het kenmerk cloudAnchor om het object in de ruimte van AAD-connector, volgt de koppeling terug naar de MV-object te zoeken en vervolgens voert de koppeling naar het AD-object. Omdat het is mogelijk meerdere AD-objecten (met meerdere forests) voor dezelfde gebruiker, de synchronisatie-engine is afhankelijk van de `Microsoft.InfromADUserAccountEnabled.xxx` koppeling naar de juiste account kiezen.

    > [!Note]
    > Als gevolg van deze logica moet Azure AD Connect kunnen communiceren met de PDC-Emulator voor write-back van wachtwoord om te werken. Als u deze handmatig inschakelt moet, kunt u Azure AD Connect aansluiten op de PDC-Emulator door met de rechtermuisknop op de **eigenschappen** van de Active Directory-synchronisatie-connector te selecteren **mappartities configureren**. Ga van daaruit naar de sectie **Verbindingsinstellingen voor domeincontrollers** en schakel het vakje **Alleen voorkeursdomeincontrollers gebruiken** in. Zelfs als de voorkeur DC niet een PDC-emulator, probeert Azure AD Connect verbinding maken met de PDC voor write-back van wachtwoord.

8. Als het gebruikersaccount wordt gevonden, wordt er geprobeerd voor wachtwoordherstel rechtstreeks in de juiste AD-forest.
9. Als het wachtwoord set-bewerking geslaagd is, zien we de gebruiker dat het wachtwoord is gewijzigd.
    > [!NOTE]
    > In het geval wanneer het wachtwoord van de gebruiker is gesynchroniseerd naar Azure AD met Wachtwoordsynchronisatie is een kans dat het lokale wachtwoordbeleid zwakkere dan de cloud-wachtwoordbeleid is. In dit geval afdwingen we nog steeds van wat het beleid van de on-premises is en in plaats daarvan synchronisatie van wachtwoordhash om te synchroniseren van de hash van dat wachtwoord toestaan. Dit zorgt ervoor dat het beleid van uw on-premises wordt afgedwongen in de cloud, ongeacht of u Wachtwoordsynchronisatie of Federatie gebruiken voor eenmalige aanmelding.

10. Als het wachtwoord ingesteld mislukt, wordt geretourneerd met de fout aan de gebruiker en laat ze probeer het opnieuw.
    * De bewerking kan mislukken vanwege de volgende
        * De service is niet actief
        * Het wachtwoord dat ze geselecteerd voldeed niet aan beleid van organisatie
        * Er kan de gebruiker niet vinden in de lokale AD

    We hebben een specifieke bericht voor veel van deze gevallen en de gebruiker vertellen wat ze kunnen doen om het probleem te verhelpen.

## <a name="configuring-password-writeback"></a>Wachtwoord terugschrijven configureren

Het is raadzaam dat u de functie voor automatisch bijwerken van [Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md) als u wilt gebruiken terugschrijven van wachtwoorden.

DirSync en Azure AD Sync zijn niet langer ondersteund middel aan voor het inschakelen van wachtwoord terugschrijven het artikel [upgraden van DirSync en Azure AD Sync](connect/active-directory-aadconnect-dirsync-deprecated.md) bevat meer informatie om te helpen bij de overgang.

De volgende stappen wordt ervan uitgegaan dat u Azure AD Connect al hebt geconfigureerd in uw omgeving met de [Express](./connect/active-directory-aadconnect-get-started-express.md) of [aangepaste](./connect/active-directory-aadconnect-get-started-custom.md) instellingen.

1. Om te configureren en inschakelen van wachtwoord terugschrijven melden bij uw Azure AD Connect-server en start de **Azure AD Connect** configuratiewizard.
2. Klik op het welkomstscherm op **configureren**.
3. Klik op het scherm Extra taken **aanpassen Synchronisatieopties** en kies vervolgens **volgende**.
4. Voer de referenties van een globale beheerder op het scherm verbinding maken met Azure AD, en kies **volgende**.
5. Op de verbinding maken uw mappen en domein en OE filteren schermen kunt u **volgende**.
6. Op het scherm optionele functies, schakel het selectievakje in naast **wachtwoord terugschrijven** en klik op **volgende**.
   ![Wachtwoord terugschrijven in Azure AD Connect inschakelen][Writeback]
7. Klik op de gereed voor configuratie scherm, **configureren** en wacht totdat dit proces te voltooien.
8. Als u ziet dat de configuratie is voltooid, klikt u op **afsluiten**

## <a name="active-directory-permissions"></a>Machtigingen voor Active Directory

Het account opgegeven in het Azure AD Connect-hulpprogramma moet wachtwoord opnieuw instellen, wachtwoord wijzigen, schrijfmachtigingen hebben op lockoutTime en schrijfmachtigingen op pwdLastSet, uitgebreide rechten van een object in de root van **elk domein** in dat forest **of** op de organisatie-eenheden u wilt gebruiken als binnen het bereik van de SSPR-gebruiker.

Als u niet zeker weet welk account de bovenstaande naar verwijst de Azure Active Directory Connect configuratiegebruikersinterface openen en klik op de huidige configuratie-optie voor weergave. Het account dat u wilt toevoegen, mag wordt vermeld onder 'Mappen gesynchroniseerd'

Deze machtigingen kan het MA-serviceaccount voor elk forest de wachtwoorden beheren namens de gebruikersaccounts in dat forest. **Als u niet de volgende machtigingen toe te wijzen, klikt u vervolgens terugschrijven weergegeven correct worden geconfigureerd, maar gebruikers er fouten optreden bij een poging tot het beheren van hun on-premises wachtwoorden vanuit de cloud.**

> [!NOTE]
> Het kan duren tot een uur of meer voor de machtigingen zijn gerepliceerd naar alle objecten in uw directory.
>

Voor het instellen van de juiste machtigingen voor write-back van wachtwoord om te worden uitgevoerd

1. Open Active Directory: gebruikers en Computers met een account met de juiste machtigingen voor domeinbeheer
2. Controleer of dat geavanceerde functies is ingeschakeld op in het menu Beeld
3. In het linkerdeelvenster met de rechtermuisknop op het object waarmee de hoofdmap van het domein en kies Eigenschappen
    * Klik op het tabblad Beveiliging
    * Klik vervolgens op Geavanceerd.
4. Klik op toevoegen op het tabblad machtigingen
5. Het account kiezen dat machtigingen worden toegepast op (van Azure AD Connect-installatie)
6. Selecteer in de van toepassing op de vervolgkeuzelijst onderliggende gebruikersobjecten
7. Schakel de selectievakjes voor de volgende onder machtigingen
    * Unexpire wachtwoord
    * Wachtwoord opnieuw instellen
    * Wachtwoord wijzigen
    * LockoutTime schrijven
    * PwdLastSet schrijven
8. Klik op toepassen/OK via wilt toepassen en sluit alle geopende dialoogvensters.

## <a name="licensing-requirements-for-password-writeback"></a>Licentievereisten voor write-back van wachtwoord

Voor informatie over licentieverlening, Zie [licenties nodig zijn voor write-back van wachtwoord](active-directory-passwords-licensing.md#licenses-required-for-password-writeback) of de volgende sites

* [Azure Active Directory-prijzen site](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Beveiligde productief Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-supported-for-password-writeback"></a>Lokale verificatiemodi ondersteund voor write-back van wachtwoord

Wachtwoord terugschrijven werkt voor de volgende typen van de gebruiker-wachtwoord:

* **Alleen in de cloud gebruikers**: wachtwoord terugschrijven is niet van toepassing in dit geval, omdat er geen on-premises-wachtwoord
* **Het wachtwoord is gesynchroniseerd gebruikers**: wachtwoord terugschrijven ondersteund
* **Federatieve gebruikers**: wachtwoord terugschrijven ondersteund
* **Pass through-verificatie gebruikers**: wachtwoord terugschrijven ondersteund

### <a name="user-and-admin-operations-supported-for-password-writeback"></a>Gebruikers- en Admin bewerkingen die worden ondersteund voor write-back van wachtwoord

Wachtwoorden worden teruggeschreven in de volgende situaties:

* **Ondersteunde bewerkingen van eindgebruikers**
  * Alle eindgebruikers selfservice vrijwillige bewerking wachtwoord wijzigen
  * Eindgebruikers selfservice force wijzigen wachtwoord bewerkingen (bijvoorbeeld verlopen van wachtwoorden)
  * Alle eindgebruikers selfservice voor wachtwoordherstel die afkomstig zijn van de [Portal voor wachtwoord opnieuw instellen](https://passwordreset.microsoftonline.com)
* **Ondersteunde bewerkingen van de beheerder**
  * Een beheerder selfservice vrijwillige bewerking wachtwoord wijzigen
  * Een beheerder selfservice force wijzigen wachtwoord bewerking (bijvoorbeeld verlopen van wachtwoorden)
  * Een beheerder selfservice voor wachtwoordherstel die afkomstig zijn van de [Portal voor wachtwoord opnieuw instellen](https://passwordreset.microsoftonline.com)
  * Een beheerder worden geïnitieerd door eindgebruikers wachtwoord opnieuw instellen van de [klassieke Azure-portal](https://manage.windowsazure.com)
  * Een beheerder worden geïnitieerd door eindgebruikers wachtwoord opnieuw instellen van de [Azure-portal](https://portal.azure.com)

### <a name="user-and-admin-operations-not-supported-for-password-writeback"></a>Gebruikers- en Admin bewerkingen worden niet ondersteund voor write-back van wachtwoord

De wachtwoorden **niet** teruggeschreven in een van de volgende situaties:

* **Niet-ondersteunde bewerkingen van eindgebruikers**
  * Iedere gebruiker opnieuw instellen van hun eigen wachtwoord met behulp van PowerShell v1, v2, of de Azure AD Graph API
* **Niet-ondersteunde bewerkingen van de beheerder**
  * Een beheerder worden geïnitieerd door eindgebruikers wachtwoord opnieuw instellen van de [Office-beheerportal](https://portal.office.com)
  * Een beheerder worden geïnitieerd door eindgebruikers voor wachtwoordherstel van PowerShell v1, v2 of de Azure AD Graph API

Terwijl we proberen te verwijderen van deze beperkingen, hoeft u niet een specifieke tijdlijn die we nog kunt delen.

## <a name="password-writeback-security-model"></a>Wachtwoord terugschrijven beveiligingsmodel

Wachtwoord terugschrijven is een maximaal beveiligde service.  Uw gegevens worden beschermd, zodat inschakelen we een beveiligingsmodel 4 lagen die wordt hieronder beschreven.

* **Tenant-specifieke service bus relay**
  * Bij het instellen van de service, u stelt een tenantspecifieke service bus relay die wordt beveiligd door een willekeurig gegenereerd sterk wachtwoord dat Microsoft nooit toegang tot heeft.
* **De versleutelingssleutel vergrendelde, cryptografisch sterk wachtwoord**
  * Nadat de service bus relay is gemaakt, maken we een sterke symmetrische sleutel die we gebruiken voor het versleutelen van het wachtwoord als ze afkomstig zijn via de kabel. Deze sleutel woont alleen in geheime store in de cloud, die sterk is vergrendeld en gecontroleerd, net als elk wachtwoord in de map van uw bedrijf.
* **De standaard TLS bedrijfstak**
  1. Wanneer u een wachtwoord opnieuw instellen of wijzigen bewerking doet zich voor in de cloud, we nemen het leesbare wachtwoord coderen met uw openbare sleutel.
  2. We vervolgens plaatsen die in een HTTPS-mailbericht wordt verzonden via een versleuteld kanaal met behulp van Microsoft SSL-certificaten voor uw service bus relay.
  3. Nadat het bericht naar Service Bus binnenkomt, uw lokale ontwaakt en de agent zich verifieert bij Service Bus het sterk wachtwoord dat eerder is gegenereerd.
  4. Lokale agent het gecodeerde bericht opneemt, ontsleutelt met behulp van de persoonlijke sleutel die wordt gegenereerd.
  5. Lokale agent probeert vervolgens via de API van AD DS SetPassword-wachtwoord instellen.
     * Deze stap is wat kunnen worden uitgevoerd op het afdwingen van uw beleid AD on-premises wachtwoord (complexiteit, leeftijd, geschiedenis, filters, enzovoort) in de cloud.
* **Het verloopbeleid bericht** 
  * Als het bericht bevindt zich in de Service Bus omdat uw on-premises service niet actief is, wordt beëindigd en worden verwijderd na enkele minuten beveiliging nog verder verbeteren.

### <a name="password-writeback-encryption-details"></a>Wachtwoord terugschrijven versleuteling details

De stappen voor het versleutelen een aanvraag voor wachtwoordherstel doorloopt nadat een gebruiker verzendt, voordat het wordt geleverd in uw on-premises-omgeving, om ervoor te zorgen maximale service betrouwbaarheid en beveiliging worden hieronder beschreven.

* **Stap 1 - wachtwoordversleuteling met 2048-bits RSA-sleutel** - zodra een gebruiker een wachtwoord worden teruggeschreven naar verzendt on-premises, eerst het opgegeven wachtwoord zelf is versleuteld met een 2048-bits RSA-sleutel.
* **Stap 2: versleuteling van het pakket op met AES-GCM** -en vervolgens het volledige pakket (wachtwoord + vereiste metagegevens) is versleuteld met AES-GCM. Deze versleuteling voorkomt dat iedereen met directe toegang naar de onderliggende service bus-kanaal weergeven/knoeien met de inhoud.
* **Stap 3: alle communicatie vindt plaats via TLS / SSL** -alle communicatie met Service Bus gebeurt in een SSL/TLS-kanaal. Deze versleuteling beveiligt de inhoud van een niet-geautoriseerde derde partij.
* **Automatische sleutelrollover om de zes maanden** : elke 6 maanden of elke keer dat wachtwoord terugschrijven is uitgeschakeld of opnieuw worden ingeschakeld op Azure AD Connect we automatisch alle rollover-codes om ervoor te zorgen maximale service en veiligheid.

### <a name="password-writeback-bandwidth-usage"></a>Wachtwoord terugschrijven bandbreedtegebruik

Wachtwoord terugschrijven is een lage bandbreedte-service die aanvragen teruggestuurd naar de lokale agent alleen in de volgende omstandigheden wordt:

1. Twee berichten worden verzonden wanneer het in- of uitschakelen van de functie met Azure AD Connect.
2. Een bericht wordt elke vijf minuten als een heartbeat-service voor verzonden als de service wordt uitgevoerd.
3. Twee berichten worden verzonden op elk moment een nieuw wachtwoord wordt ingediend
    * Eerste bericht, als een aanvraag voor de bewerking uit te voeren
    * Tweede bericht die het resultaat van de bewerking bevat en worden verzonden in de volgende omstandigheden:
        * Telkens wanneer een nieuw wachtwoord wordt ingediend tijdens een gebruiker zelf uw wachtwoord opnieuw instellen.
        * Telkens wanneer een nieuw wachtwoord wordt ingediend tijdens een wachtwoordwijziging van gebruiker.
        * Telkens wanneer een nieuw wachtwoord wordt ingediend tijdens een beheerder geïnitieerde gebruikerswachtwoord opnieuw instellen (van de Azure-beheerder portals alleen).

#### <a name="message-size-and-bandwidth-considerations"></a>Overwegingen voor het grootte en bandbreedte van het bericht

De grootte van elk van de hierboven beschreven bericht is doorgaans onder 1 kb, zelfs onder extreme laadt, de service voor het terugschrijven van wachtwoord zelf verbruikt enkele kilobits per seconde van de bandbreedte. Omdat elk bericht wordt verzonden in realtime, is alleen indien vereist door een updatebewerking wachtwoord, en de berichtgrootte zo klein is het bandbreedtegebruik van de mogelijkheid Write-back effectief te klein voor een echte merkbare impact hebben.

## <a name="next-steps"></a>Volgende stappen

* [Hoe ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Opnieuw instellen of wijzigen van uw wachtwoord](active-directory-passwords-update-your-own-password.md).
* [Registreren voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).
* [Hebt u een vraag licentieverlening?](active-directory-passwords-licensing.md)
* [Welke gegevens wordt gebruikt door de SSPR en welke gegevens moet u voor uw gebruikers vullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties met SSPR?](active-directory-passwords-policy.md)
* [Hoe meld ik op activiteit in SSPR?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in de SSPR en wat ze aan de hand?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets is verbroken. Hoe kan ik SSPR oplossen?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag waarvoor is geen ergens anders](active-directory-passwords-faq.md)

[Writeback]: ./media/active-directory-passwords-writeback/enablepasswordwriteback.png "Wachtwoord terugschrijven in Azure AD Connect inschakelen"
