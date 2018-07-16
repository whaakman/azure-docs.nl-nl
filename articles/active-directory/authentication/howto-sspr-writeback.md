---
title: Azure AD SSPR met wachtwoord terugschrijven | Microsoft Docs
description: Gebruik Azure AD en Azure AD Connect terugschrijven van wachtwoorden met een on-premises directory
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: fce92845591f20f7f2e9cff1a856e0904629255b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054791"
---
# <a name="password-writeback-overview"></a>Overzicht van de Write-back van wachtwoord

Met het terugschrijven van wachtwoorden, kunt u Azure Active Directory (Azure AD) voor het terugschrijven van wachtwoorden naar uw on-premises Active Directory configureren. Write-back van wachtwoord hoeven instellen en beheren van een gecompliceerd on-premises Self-service voor wachtwoord opnieuw instellen (SSPR)-oplossing, en biedt een handige manier cloud-gebaseerde voor uw gebruikers hun on-premises wachtwoorden opnieuw instellen waar ze ook zijn. Wachtwoord terugschrijven is een onderdeel van [Azure Active Directory Connect](./../connect/active-directory-aadconnect.md) die kan worden ingeschakeld en die worden gebruikt door de huidige abonnees van Premium [Azure Active Directory-edities](../fundamentals/active-directory-whatis.md).

Wachtwoord terugschrijven biedt de volgende functies:

* **Levert feedback nul vertraging**: wachtwoord terugschrijven is een synchrone bewerking. Uw gebruikers zijn onmiddellijk een melding als hun wachtwoord niet voldoet aan het beleid kan niet worden opnieuw ingesteld of gewijzigd voor een bepaalde reden.
* **Ondersteunt wachtwoorden opnieuw instellen voor gebruikers die gebruikmaken van Active Directory Federation Services (AD FS) of andere technologieën federation**: met het terugschrijven van wachtwoorden, zolang de federatieve gebruikersaccounts worden gesynchroniseerd met uw Azure AD-tenant kunnen worden naar hun on-premises Active Directory-wachtwoorden beheren vanuit de cloud.
* **Ondersteunt wachtwoorden opnieuw instellen voor gebruikers die gebruikmaken van** [wachtwoord-hashsynchronisatie](./../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md): als de service voor het opnieuw instellen wordt gedetecteerd dat een gesynchroniseerd gebruikersaccount is ingeschakeld voor synchronisatie van wachtwoordhashes, we dit opnieuw instellen account van on-premises en cloud-wachtwoord tegelijkertijd. Wachtwoord terugschrijven is niet afhankelijk van wachtwoord-hashsynchronisatie.
* **Wachtwoorden voor gebruikers die gebruikmaken van Pass-Through-verificatie ondersteunt**: met het terugschrijven van wachtwoorden, zolang de Pass through-verificatie-accounts zijn gesynchroniseerd met uw Azure AD-tenant kunnen worden voor het beheren van hun on-premises actief Directory-wachtwoorden vanuit de cloud.
* **Ondersteunt het wachtwoord wordt gewijzigd in het toegangsvenster en de Office 365**: wanneer federatieve of wachtwoord gesynchroniseerd gebruikers hun wachtwoord moeten wijzigen is verlopen of niet-verlopen, wordt deze wachtwoorden terugschrijven naar uw lokale Active Directory-omgeving worden geleverd.
* **Biedt ondersteuning voor write-back van wachtwoorden wanneer een beheerder stelt ze opnieuw vanuit de Azure-portal**: wanneer een beheerder stelt het wachtwoord van een gebruiker in de [Azure-portal](https://portal.azure.com), als die gebruiker is gefedereerd of het wachtwoord die zijn gesynchroniseerd, we het wachtwoord stellen de beheerder selecteren in de lokale Active Directory en. Deze functionaliteit is momenteel niet ondersteund in de Office-beheerportal.
* **Uw on-premises Active Directory-wachtwoordbeleid afgedwongen**: wanneer een gebruiker hun wachtwoord opnieuw instellen, wij Zorg ervoor dat het voldoet aan met uw on-premises Active Directory-beleid voordat we deze in die map doorvoeren. Deze beoordeling omvat de geschiedenis, complexiteit, leeftijd, wachtwoordfilters en andere wachtwoordbeperkingen die u hebt gedefinieerd in de lokale Active Directory controleren.
* **Vereist geen inkomende firewallregels**: wachtwoord terugschrijven wordt een Azure Service Bus relay gebruikt als een onderliggende communicatiekanaal. U hebt geen geen inkomende poorten op de firewall voor deze functie te gebruiken te openen.
* **Wordt niet ondersteund voor gebruikersaccounts die zijn opgeslagen in beveiligde groepen in Active Directory on-premises**: Zie voor meer informatie over beveiligde groepen [beveiligde accounts en groepen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>De werking van wachtwoord terugschrijven

Wanneer een federatieve of het wachtwoord-hash gesynchroniseerde gebruiker opnieuw instellen of wijzigen van het wachtwoord in de cloud is, gebeurt het volgende:

1. We controleren om te zien welke type wachtwoord op de gebruiker heeft. Als we zien dat het wachtwoord on-premises worden beheerd is:
   * We controleren of de Write-back-service actief en werkend. Als het actief en werkend is, laten we de gebruiker gaan.
   * Als de Write-back-service niet actief is, zien we de gebruiker dat hun wachtwoord nu kan niet worden teruggezet.
2. Vervolgens de gebruiker de juiste verificatie-gates is geslaagd en bereikt de **wachtwoord opnieuw instellen** pagina.
3. De gebruiker selecteert een nieuw wachtwoord en bevestigt het.
4. Wanneer de gebruiker selecteert **indienen**, we het leesbare wachtwoord versleutelen met een symmetrische sleutel die is gemaakt tijdens het installatieproces van het terugschrijven van wachtwoorden.
5. Na het versleutelen van het wachtwoord, zijn we deze in een nettolading wordt verzonden via een HTTPS-kanaal aan uw tenant-specifieke service bus-omleiding (die we ook voor u ingesteld tijdens de installatieprocedure voor het terugschrijven van wachtwoorden). Deze relay wordt beveiligd door een willekeurig gegenereerd wachtwoord dat alleen de installatie van uw on-premises kent.
6. Nadat het bericht heeft bereikt van de servicebus, wordt het eindpunt opnieuw instellen van wachtwoorden automatisch ontwaakt en ziet dat er een aanvraag in behandeling.
7. De gebruiker zoekt de service vervolgens met behulp van de cloud ankerkenmerk. Voor deze zoekactie te voltooien:

    * Het gebruikersobject moet bestaan in het Active Directory-connectorgebied.
    * Het gebruikersobject moet worden gekoppeld aan het bijbehorende (MV) metaverse-object.
    * Het gebruikersobject moet worden gekoppeld aan het bijbehorende Azure Active Directory-connector-object.
    * De koppeling van de Active Directory-connector-object naar de MV ze beschikken over de synchronisatieregel `Microsoft.InfromADUserAccountEnabled.xxx` op de koppeling. <br> <br>
    De synchronisatie-engine wordt gebruikt als de oproep afkomstig van de cloud is, de **waarde voor cloudAnchor** kenmerk om te controleren of de Azure Active Directory-connector space-object. Het volgt de koppeling op naar het MV-object, en volgt de koppeling op naar het Active Directory-object. Omdat er meerdere Active Directory-objecten (meerdere forests) voor dezelfde gebruiker zijn, de synchronisatie-engine is afhankelijk van de `Microsoft.InfromADUserAccountEnabled.xxx` koppeling naar het juiste abonnement kiezen.

    > [!Note]
    > Als gevolg van deze logica voor wachtwoord moet terugschrijven in om te werken van Azure AD Connect kunnen communiceren met de primaire domeincontroller (PDC) emulator. Als u deze handmatig inschakelt moet, kunt u Azure AD Connect verbinding met de PDC-emulator. Met de rechtermuisknop op de **eigenschappen** van de Active Directory-synchronisatieconnector, selecteert u vervolgens **mappartities configureren**. Zoek daar de **verbindingsinstellingen** sectie en selecteer het vak met de titel **gebruik alleen domeincontrollers**. Zelfs als de gewenste domeincontroller niet een PDC-emulator is, probeert Azure AD Connect om verbinding met de PDC voor het terugschrijven van wachtwoorden te maken.

8. Nadat de gebruiker wordt account gevonden, wordt er geprobeerd voor wachtwoordherstel voor rechtstreeks in de juiste Active Directory-forest.
9. Als het wachtwoord set-bewerking geslaagd is, zien we de gebruiker dat het wachtwoord is gewijzigd.
    > [!NOTE]
    > Als het wachtwoord van de gebruiker is gesynchroniseerd met Azure AD met behulp van synchronisatie van wachtwoord, is er een kans dat de on-premises Wachtwoordbeleid toepassen zwakkere dan het wachtwoordbeleid van de cloud is. In dit geval is nog steeds van kracht ongeacht de on-premises-beleid is, en gebruik in plaats daarvan wachtwoord-hashsynchronisatie om te synchroniseren van de hash van dit wachtwoord. Dit beleid zorgt ervoor dat uw on-premises-beleid wordt afgedwongen in de cloud, ongeacht als u Wachtwoordsynchronisatie of Federatie gebruiken voor eenmalige aanmelding.

10. Als het wachtwoord bewerking mislukt instelt, we fout worden geretourneerd naar de gebruiker en laat ze probeer het opnieuw. De bewerking kan mislukken omdat:
    * De service is niet beschikbaar.
    * Het wachtwoord dat ze geselecteerd niet voldoet aan beleidsregels van de organisatie.
    * We kunnen de gebruiker niet vinden in de lokale Active Directory.

    We hebben een specifiek bericht voor veel van deze gevallen en laat de gebruiker weten wat ze kunnen doen om het probleem te verhelpen.

## <a name="configure-password-writeback"></a>Configureer wachtwoord terugschrijven

Het is raadzaam dat u de functie voor automatisch bijwerken van [Azure AD Connect](./../connect/active-directory-aadconnect-get-started-express.md) als u wilt het terugschrijven van wachtwoorden gebruiken.

DirSync en Azure AD Sync worden niet meer ondersteund als een manier om wachtwoord terugschrijven inschakelen. Zie voor meer informatie om te helpen bij de overgang [upgraden van DirSync en Azure AD Sync](../connect/active-directory-aadconnect-dirsync-deprecated.md).

De volgende stappen wordt ervan uitgegaan dat u Azure AD Connect al hebt geconfigureerd in uw omgeving met behulp van de [Express](./../connect/active-directory-aadconnect-get-started-express.md) of [aangepaste](./../connect/active-directory-aadconnect-get-started-custom.md) instellingen.

1. Als u wilt configureren en schakel wachtwoord terugschrijven, meld u aan met uw Azure AD Connect-server en start de **Azure AD Connect** configuratiewizard.
2. Op de **Welkom** weergeeft, schakelt **configureren**.
3. Op de **extra taken** weergeeft, schakelt **aanpassen Synchronisatieopties**, en selecteer vervolgens **volgende**.
4. Op de **verbinding maken met Azure AD** pagina, voer de referenties van een globale beheerder en selecteer vervolgens **volgende**.
5. Op de **verbinding maken met mappen** en **domein/OE** pagina's filteren, selecteert u **volgende**.
6. Op de **optionele functies** pagina, schakel het selectievakje in naast **wachtwoord terugschrijven** en selecteer **volgende**.
   ![Wachtwoord terugschrijven inschakelen in Azure AD Connect][Writeback]
7. Op de **klaar om te configureren** weergeeft, schakelt **configureren** en wacht totdat het proces is voltooid.
8. Wanneer u de configuratie voltooien ziet, selecteert u **afsluiten**.

Voor het oplossen van problemen met taken met betrekking tot het terugschrijven van wachtwoorden, Zie de sectie [problemen met wachtwoord terugschrijven oplossen](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) in ons artikel over probleemoplossing.

## <a name="active-directory-permissions"></a>Machtigingen voor Active Directory

Het account dat is opgegeven in het hulpprogramma Azure AD Connect moeten hebben de volgende items als u wilt worden binnen het bereik van self-service voor Wachtwoordherstel:

* **Wachtwoord opnieuw instellen** 
* **Wachtwoord wijzigen** 
* **Schrijfmachtigingen** op `lockoutTime`  
* **Schrijfmachtigingen** op `pwdLastSet`
* **Uitgebreide rechten** in elk:
   * Het hoofdobject van *elk domein* in dat forest
   * De gebruiker organisatie-eenheden (OU's) die u wilt in de scope voor SSPR

Als u niet zeker welk account de beschreven account verwijst, open de Azure Active Directory Connect configuratie-UI en selecteer de **de huidige configuratie weergeven** optie. Het account dat u toevoegen van machtigingen wilt voor wordt vermeld onder **mappen gesynchroniseerd**.

Als u deze machtigingen instelt, kan het MA-serviceaccount voor elk forest wachtwoorden beheren namens de gebruikersaccounts in dat forest. 

> [!IMPORTANT]
> Als u niet de volgende machtigingen toe te wijzen, vervolgens terugschrijven weergegeven om te worden geconfigureerd, maar gebruikers worden er fouten optreden bij de poging voor het beheren van hun on-premises wachtwoorden vanuit de cloud.
>

> [!NOTE]
> Het kan duren tot een uur of meer machtigingen zijn gerepliceerd naar alle objecten in uw directory.
>

Om in te stellen op de juiste machtigingen voor het terugschrijven van wachtwoorden worden uitgevoerd, voert u de volgende stappen uit:

1. Open Active Directory: gebruikers en Computers met een account met de juiste machtigingen voor domeinbeheer.
2. Uit de **weergave** menu, zorg ervoor dat **geavanceerde functies** is ingeschakeld.
3. In het linkerdeelvenster met de rechtermuisknop op het object dat staat voor de hoofdmap van het domein en selecteer **eigenschappen** > **Security** > **Geavanceerd**.
4. Uit de **machtigingen** tabblad **toevoegen**.
5. Kies het account dat machtigingen worden toegepast op (van de Azure AD Connect-installatie).
6. In de **is van toepassing op** vervolgkeuzelijst, selecteer **onderliggende gebruiker** objecten.
7. Onder **machtigingen**, selecteert u het volgende:
    * **Wachtwoord opnieuw instellen**
    * **Wachtwoord wijzigen**
    * **LockoutTime schrijven**
    * **PwdLastSet schrijven**
8. Selecteer **toepassen/OK** de wijzigingen toepassen en afsluiten van alle geopende dialoogvensters.

## <a name="licensing-requirements-for-password-writeback"></a>Licentievereisten voor wachtwoord terugschrijven

Zie voor meer informatie over licentieverlening [licenties vereist zijn voor het terugschrijven van wachtwoorden](concept-sspr-licensing.md) of de volgende sites:

* [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-that-are-supported-for-password-writeback"></a>On-premises verificatiemodi die worden ondersteund voor het terugschrijven van wachtwoorden

Ondersteuning voor write-back van wachtwoord voor de volgende typen van de gebruiker-wachtwoord:

* **Cloudgebruikers**: het terugschrijven van wachtwoorden is niet van toepassing in dit geval, omdat er geen on-premises wachtwoord.
* **Wachtwoord-gesynchroniseerde gebruikers**: wachtwoord terugschrijven wordt ondersteund.
* **Federatieve gebruikers**: wachtwoord terugschrijven wordt ondersteund.
* **Pass Through-verificatiegebruikers**: wachtwoord terugschrijven wordt ondersteund.

### <a name="user-and-admin-operations-that-are-supported-for-password-writeback"></a>Gebruikers- en beheerdersactiviteiten bewerkingen die worden ondersteund voor het terugschrijven van wachtwoorden

Wachtwoorden worden teruggeschreven in de volgende situaties:

* **Ondersteunde eindgebruikers**
  * Alle eindgebruikers Self-servicegebruikers vrijwillige bewerking wachtwoord wijzigen
  * Alle eindgebruikers Self-servicegebruikers force wijzigen wachtwoord bewerking, bijvoorbeeld: wachtwoord verloopt
  * Een eindgebruiker Self-service voor wachtwoord opnieuw instellen dat afkomstig van is de [portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com)
* **De beheerder van de ondersteunde bewerkingen**
  * Een beheerder selfservice vrijwillige bewerking wachtwoord wijzigen
  * Een beheerder selfservice-force wijzigen wachtwoord bewerking, bijvoorbeeld: wachtwoord verloopt
  * Een beheerder van self-service voor wachtwoord opnieuw instellen dat afkomstig van is de [portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com)
  * Een eindgebruiker door de beheerder voor wachtwoord opnieuw instellen van de [Azure-portal](https://portal.azure.com)

### <a name="user-and-admin-operations-that-are-not-supported-for-password-writeback"></a>Gebruikers- en beheerdersactiviteiten bewerkingen die worden niet ondersteund voor het terugschrijven van wachtwoorden

Wachtwoorden zijn *niet* teruggeschreven in een van de volgende situaties:

* **Bewerkingen voor niet-ondersteunde eindgebruikers**
  * Een eindgebruiker hun eigen wachtwoord opnieuw instellen met behulp van PowerShell-versie 1, versie 2 of de Azure AD Graph API
* **Niet-ondersteunde bewerkingen**
  * Een eindgebruiker door de beheerder voor wachtwoord opnieuw instellen van de [Office-beheerportal](https://portal.office.com)
  * Een eindgebruiker door de beheerder voor wachtwoord opnieuw instellen van de PowerShell-versie 1, versie 2 of de Azure AD Graph API

We werken als u wilt verwijderen van deze beperkingen, maar we hebben geen een specifieke tijdlijn die we nog kunt delen.

## <a name="password-writeback-security-model"></a>Beveiligingsmodel voor write-back van wachtwoord

Wachtwoord terugschrijven is een zeer veilige service. Als u wilt zorgen dat uw gegevens worden beschermd, we in staat stellen een beveiligingsmodel vier lagen die wordt beschreven als volgt:

* **Tenant-specifieke service bus relay**
  * Bij het instellen van de service, u stelt een tenant-specifieke service bus relay die wordt beveiligd door een willekeurig gegenereerd sterk wachtwoord dat Microsoft nooit toegang tot heeft.
* **De versleutelingssleutel vergrendeld, cryptografisch sterk wachtwoord**
  * Nadat de service bus relay is gemaakt, maken we een sterke symmetrische sleutel die we gebruiken voor het versleutelen van het wachtwoord als het gaat via de kabel. Deze sleutel is alleen aanwezig in de geheime store van uw bedrijf in de cloud, die sterk is vergrendeld en gecontroleerd, net als elk ander wachtwoord in de map.
* **Branche standaard Transport Layer Security (TLS)**
  1. Wanneer een wachtwoord opnieuw instellen of wijzigen bewerking wordt uitgevoerd in de cloud, we nemen het leesbare wachtwoord en versleutelen met de openbare sleutel.
  2. We geven die in een HTTPS-bericht dat wordt verzonden via een versleuteld kanaal met behulp van Microsoft-SSL-certificaten naar uw service bus relay.
  3. Nadat het bericht wordt ontvangen in de servicebus, wordt uw on-premises-agent wordt weer actief en wordt geverifieerd bij de servicebus met behulp van het wachtwoord dat eerder is gegenereerd.
  4. De on-premises agent neemt het gecodeerde bericht en ontsleutelt deze met behulp van de persoonlijke sleutel die wordt gegenereerd.
  5. De on-premises-agent probeert het wachtwoord via de API van AD DS SetPassword in te stellen. Deze stap is wat kunnen we om af te dwingen van uw Active Directory on-premises Wachtwoordbeleid toepassen (zoals de complexiteit, leeftijd, geschiedenis en filters) in de cloud.
* **Bericht verloopbeleid voor** 
  * Als het bericht bevindt zich in servicebus, omdat uw on-premises service niet actief is, een time-out optreedt en wordt verwijderd na enkele minuten. De time-out en het verwijderen van het bericht verhoogt de beveiliging nog verder.

### <a name="password-writeback-encryption-details"></a>Versleutelingsgegevens voor write-back van wachtwoord

Nadat een gebruiker een wachtwoord opnieuw instellen is ingediend, gaat de aanvraag opnieuw instellen via verschillende stappen voor het versleutelen voordat ze worden ontvangen in uw on-premises omgeving. Deze stappen voor het versleutelen Zorg ervoor dat de service met maximale beschikbaarheid en beveiliging. Ze worden als volgt beschreven:

* **Stap 1: Wachtwoordversleuteling met 2048-bits RSA-sleutel**: nadat een gebruiker een wachtwoord worden teruggeschreven naar on-premises verzendt, het opgegeven wachtwoord zelf is versleuteld met een 2048-bits RSA-sleutel.
* **Stap 2: Pakket versleuteling met AES-GCM**: het volledige pakket, het wachtwoord en de vereiste metagegevens is versleuteld met AES-GCM. Deze versleuteling zorgt ervoor dat niemand met directe toegang naar de onderliggende service bus-kanaal weergeven of knoeien met de inhoud.
* **Stap 3: Alle communicatie vindt plaats via TLS/SSL**: alle communicatie met Service Bus gebeurt in een SSL/TLS-kanaal. Deze versleuteling voor beveiliging van de inhoud van een niet-geautoriseerde derde partij.
* **Automatische sleutelrollover na zes maanden**: elke zes maanden of elke keer het terugschrijven van wachtwoorden is uitgeschakeld en vervolgens weer wordt ingeschakeld op Azure AD Connect. Er is automatisch rollover van alle sleutels om ervoor te zorgen voor maximale service en veiligheid.

### <a name="password-writeback-bandwidth-usage"></a>Bandbreedtegebruik van wachtwoord terugschrijven

Wachtwoord terugschrijven is een service met een lage bandbreedte die alleen aanvragen naar de on-premises-agent in de volgende omstandigheden verzendt:

* Twee berichten worden verzonden wanneer de functie is ingeschakeld of uitgeschakeld met Azure AD Connect.
* Een bericht wordt elke vijf minuten als een heartbeat van de service voor verzonden als de service wordt uitgevoerd.
* Twee berichten worden verzonden op elk moment een nieuw wachtwoord wordt ingediend:
    * Het eerste bericht is een verzoek om de bewerking niet uitvoeren.
    * Het tweede bericht bevat het resultaat van de bewerking en wordt verzonden in de volgende omstandigheden:
        * Telkens wanneer een nieuw wachtwoord wordt ingediend bij de self-service voor wachtwoord opnieuw instellen van een gebruiker.
        * Telkens wanneer een nieuw wachtwoord wordt ingediend bij een wachtwoordwijziging van gebruiker.
        * Telkens wanneer een nieuw wachtwoord wordt ingediend bij een beheerder geïnitieerde gebruikerswachtwoord opnieuw instellen (alleen vanaf de Azure admin-Portal).

#### <a name="message-size-and-bandwidth-considerations"></a>Overwegingen voor de grootte en bandbreedte van bericht

De grootte van elk van de eerder beschreven bericht is meestal dan 1 KB. Zelfs bij extreme belasting, is de service voor het terugschrijven van wachtwoord zelf enkele kilobits per seconde van de bandbreedte verbruikt. Omdat elk bericht wordt verzonden in realtime, is alleen in indien nodig door een bewerking voor bijwerken, en omdat de berichtgrootte zo klein is, is het bandbreedtegebruik van de mogelijkheid Write-back te klein om een merkbare impact heeft.

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](howto-sspr-deployment.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](../user-help/active-directory-passwords-update-your-own-password.md).
* [Registreer u voor selfservice voor wachtwoordherstel](../user-help/active-directory-passwords-reset-register.md).
* [Hebt u een vraag over licenties?](concept-sspr-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](howto-sspr-authenticationdata.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](concept-sspr-howitworks.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](concept-sspr-policy.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](howto-sspr-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](concept-sspr-howitworks.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Wachtwoord terugschrijven inschakelen in Azure AD Connect"
