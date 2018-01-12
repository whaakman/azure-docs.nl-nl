---
title: Azure AD SSPR met wachtwoord terugschrijven | Microsoft Docs
description: Gebruik Azure AD en Azure AD Connect voor write-back van wachtwoorden met een on-premises adreslijst
services: active-directory
keywords: Wachtwoordbeheer Active directory, wachtwoordbeheer, Azure AD self service voor wachtwoordherstel
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: b4a14d3c79f93988eeac1525da09cf70dc2de634
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="password-writeback-overview"></a>Overzicht van de Write-back van wachtwoord

U kunt Azure Active Directory (Azure AD) voor wachtwoorden terugschrijven naar uw lokale Active Directory configureren met terugschrijven van wachtwoorden. Wachtwoord terugschrijven verwijdert de behoefte instellen en beheren van een complexe on-premises-oplossing voor selfservice voor wachtwoordherstel (SSPR) opnieuw en biedt een handige manier cloud-gebaseerde voor uw gebruikers opnieuw in te stellen hun on-premises wachtwoorden waar ze ook zijn. Wachtwoord terugschrijven is een onderdeel van [Azure Active Directory Connect](./connect/active-directory-aadconnect.md) die kunnen worden ingeschakeld en gebruikt door bestaande Premium-abonnees [Azure Active Directory-edities](active-directory-whatis.md).

Wachtwoord terugschrijven biedt de volgende functies:

* **Biedt nul vertraging feedback**: wachtwoord terugschrijven is een synchrone bewerking. Uw gebruikers zijn onmiddellijk een melding als hun wachtwoord voldoet niet aan het beleid kan niet worden opnieuw ingesteld of gewijzigd voor een of andere reden.
* **Ondersteunt het instellen van wachtwoorden voor gebruikers die gebruikmaken van Active Directory Federation Services (AD FS) of andere technologieën federation**: met terugschrijven van wachtwoorden, zolang de federatieve gebruikersaccounts worden gesynchroniseerd naar uw Azure AD-tenant, ze kunnen geen hun on-premises Active Directory-wachtwoorden beheren vanuit de cloud.
* **Ondersteunt het instellen van wachtwoorden voor gebruikers die gebruikmaken van** [wachtwoordhashsynchronisatie](./connect/active-directory-aadconnectsync-implement-password-synchronization.md): als het wachtwoord opnieuw instellen van service detecteert dat een gesynchroniseerde gebruikersaccount is ingeschakeld voor wachtwoordhashsynchronisatie, we beide dit account lokale opnieuw instellen en wachtwoord tegelijkertijd cloud.
* **Ondersteunt het wachtwoord wordt gewijzigd van het toegangsvenster en Office 365**: wanneer gefedereerd of wachtwoord is gesynchroniseerd gebruikers hun wachtwoorden verlopen of niet-verlopen wijzigen we deze wachtwoorden terugschrijven naar uw lokale Active Directory-omgeving worden geleverd.
* **Biedt ondersteuning voor write-back van wachtwoorden wanneer een beheerder deze opnieuw via de Azure portal instelt**: wanneer een beheerder stelt het wachtwoord van een gebruiker in de [Azure-portal](https://portal.azure.com), als die gebruiker is gefedereerd of het wachtwoord is gesynchroniseerd, moet we het wachtwoord instellen de beheerder in de lokale Active Directory en geselecteerd. Deze functionaliteit is momenteel niet ondersteund in de Office-beheerportal.
* **Dwingt uw lokale Active Directory-wachtwoordbeleid**: wanneer een gebruiker het wachtwoord opnieuw instelt, zorgen wij ervoor dat het voldoet aan met uw beleid on-premises Active Directory voordat we het doorvoeren in die map. Deze evaluatie omvat controle van de geschiedenis, complexiteit, leeftijd, wachtwoordfilters en alle andere wachtwoordbeperkingen die u hebt gedefinieerd in de lokale Active Directory.
* **Alle firewallregels voor binnenkomend verkeer geen vereist**: wachtwoord terugschrijven een Azure Service Bus relay gebruikt als een onderliggende communicatiekanaal. U hoeft niet te openen van poorten voor inkomend verkeer op uw firewall voor deze functie te gebruiken.
* **Wordt niet ondersteund voor gebruikersaccounts die zijn opgeslagen in de beveiligde groepen in Active Directory lokale**: Zie voor meer informatie over de beveiligde groepen [beschermde accounts en groepen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>De werking van wachtwoord terugschrijven

Wanneer een federatieve of het wachtwoord-hash gesynchroniseerde gebruiker wordt geleverd opnieuw instellen of hun wachtwoord wijzigen in de cloud, gebeurt het volgende:

1. We controleren om te zien welke type wachtwoord op de gebruiker heeft. Als we zien dat het wachtwoord on-premises worden beheerd is:
   * We controleren of de Write-back-service actief is. Als het actief en werkend is, laten we de gebruiker worden voortgezet.
   * Als de Write-back-service niet van is, zien we de gebruiker dat hun wachtwoord nu kan niet worden teruggezet.
2. Vervolgens de gebruiker de juiste verificatie-poorten is geslaagd en bereikt de **wachtwoord opnieuw instellen** pagina.
3. De gebruiker selecteert een nieuw wachtwoord en bevestigt het.
4. Wanneer de gebruiker selecteert **indienen**, we het leesbare wachtwoord versleutelen met een symmetrische sleutel die is gemaakt tijdens de installatie terugschrijven.
5. Nadat het wachtwoord versleutelt, opnemen we in een nettolading die via een HTTPS-kanaal wordt verzonden naar uw tenantspecifieke service bus relay (die we ook voor u ingesteld tijdens de installatie writeback). Deze relay is beveiligd met een willekeurig gegenereerd wachtwoord dat alleen de lokale installatie kent.
6. Nadat het bericht heeft de servicebus bereikt, wordt het wachtwoord opnieuw instellen-eindpunt automatisch ontwaakt en blijkt dat er een aanvraag in behandeling.
7. De service wordt vervolgens gezocht naar de gebruiker met behulp van de cloud ankerkenmerk. Voor deze zoekactie mislukt:

    * Het gebruikersobject moet bestaan in de ruimte van Active Directory-connector.
    * Het gebruikersobject moet worden gekoppeld aan het bijbehorende (MV) metaverse-object.
    * Het gebruikersobject moet worden gekoppeld aan het bijbehorende object van de Azure Active Directory-connector.
    * De koppeling van het Active Directory-connectorobject naar de MV moet hebben tot de synchronisatieregel `Microsoft.InfromADUserAccountEnabled.xxx` op de koppeling. <br> <br>
    Wanneer u de oproep wordt geleverd vanuit de cloud, de synchronisatie-engine gebruikt de **cloudAnchor** kenmerk opzoeken van het object met Azure Active Directory-connector ruimte. Het volgt de koppeling op naar het MV-object, en volgt de koppeling op naar het Active Directory-object. Omdat er meerdere Active Directory-objecten (met meerdere forests) voor dezelfde gebruiker, de synchronisatie-engine is afhankelijk van de `Microsoft.InfromADUserAccountEnabled.xxx` koppeling naar de juiste account kiezen.

    > [!Note]
    > Als gevolg van deze logica voor wachtwoord moet terugschrijven naar Azure AD Connect werken communiceren met de emulatie primaire domeincontroller (PDC). Als u deze handmatig inschakelt moet, kunt u Azure AD Connect verbinding met de PDC-emulator. Met de rechtermuisknop op de **eigenschappen** van de connector Active Directory-synchronisatie Selecteer **mappartities configureren**. Van daaruit, zoekt u naar de **verbinding domeincontrollerinstellingen** sectie en schakelt u het met de titel **alleen gebruiken voorkeur domeincontrollers**. Zelfs als de gewenste domeincontroller niet een PDC-emulator is, wordt Azure AD Connect probeert te maken met de PDC voor write-back van wachtwoord.

8. Nadat de gebruiker wordt account gevonden, wordt er geprobeerd om het wachtwoord direct in Active Directory-forest te stellen.
9. Als het wachtwoord set-bewerking geslaagd is, zien we de gebruiker dat het wachtwoord is gewijzigd.
    > [!NOTE]
    > Als het wachtwoord van de gebruiker is gesynchroniseerd naar Azure AD met behulp van synchronisatie van wachtwoorden, is er een kans dat het lokale wachtwoordbeleid zwakkere dan de cloud-wachtwoordbeleid is. In dit geval afdwingen we nog steeds van wat het beleid van de on-premises is en in plaats daarvan synchronisatie van wachtwoordhash gebruiken om te synchroniseren van de hash van het wachtwoord. Dit beleid zorgt ervoor dat het beleid van uw on-premises wordt afgedwongen in de cloud, ongeacht of u Wachtwoordsynchronisatie of Federatie gebruiken voor eenmalige aanmelding.

10. Als het wachtwoord ingesteld mislukt, we retourneren een foutmelding voor de gebruiker en laat ze probeer het opnieuw. De bewerking kan mislukken omdat:
    * De service is niet beschikbaar.
    * Het wachtwoord dat ze geselecteerd voldeed niet aan de beleidsregels van de organisatie.
    * We kunnen de gebruiker niet vinden in de lokale Active Directory.

    We hebben een specifieke bericht voor veel van deze gevallen en de gebruiker vertellen wat ze kunnen doen om het probleem te verhelpen.

## <a name="configure-password-writeback"></a>Wachtwoord terugschrijven configureren

Het is raadzaam dat u de functie voor automatisch bijwerken van [Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md) als u wilt gebruiken terugschrijven van wachtwoorden.

DirSync en Azure AD Sync worden niet meer ondersteund als een manier wachtwoord terugschrijven inschakelen. Zie voor meer informatie om te helpen bij de overgang [upgraden van DirSync en Azure AD Sync](connect/active-directory-aadconnect-dirsync-deprecated.md).

De volgende stappen wordt ervan uitgegaan dat u Azure AD Connect al in uw omgeving hebt geconfigureerd met behulp van de [Express](./connect/active-directory-aadconnect-get-started-express.md) of [aangepaste](./connect/active-directory-aadconnect-get-started-custom.md) instellingen.

1. Aanmelden bij uw Azure AD Connect-server om te configureren en wachtwoord terugschrijven inschakelen, en start de **Azure AD Connect** configuratiewizard.
2. Op de **Welkom** pagina **configureren**.
3. Op de **extra taken** pagina **aanpassen Synchronisatieopties**, en selecteer vervolgens **volgende**.
4. Op de **verbinding maken met Azure AD** pagina, voer de referenties van een globale beheerder en selecteer vervolgens **volgende**.
5. Op de **mappen verbinding** en **domein/organisatie-eenheid** pagina's filteren, selecteert u **volgende**.
6. Op de **optionele functies** pagina, schakel het selectievakje naast **wachtwoord terugschrijven** en selecteer **volgende**.
   ![Wachtwoord terugschrijven in Azure AD Connect inschakelen][Writeback]
7. Op de **klaar om te configureren** pagina **configureren** en wacht totdat het proces te voltooien.
8. Wanneer u de configuratie voltooien ziet, selecteert u **afsluiten**.

Voor algemene probleemoplossing taken in verband met terugschrijven van wachtwoorden, Zie de sectie [wachtwoord terugschrijven oplossen](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) in onze probleemoplossingsartikel.

## <a name="active-directory-permissions"></a>Machtigingen voor Active Directory

Het account opgegeven in het Azure AD Connect-hulpprogramma moeten de volgende items ingesteld als u wilt worden binnen het bereik van SSPR:

* **Wachtwoord opnieuw instellen** 
* **Wachtwoord wijzigen** 
* **Schrijfmachtigingen** op`lockoutTime`  
* **Schrijfmachtigingen** op`pwdLastSet`
* **Uitgebreide rechten** van een:
   * Het hoofdobject van *elk domein* in dat forest
   * De gebruiker organisatie-eenheden (OE's) die u wilt worden binnen het bereik van SSPR

Als u niet zeker weet welk account het account beschreven verwijst, opent u de Azure Active Directory Connect configuratiegebruikersinterface en selecteer de **huidige weergaveconfiguratie** optie. Het account dat u toevoegen, mag wordt vermeld wilt onder **mappen gesynchroniseerd**.

Als u deze machtigingen instelt, kan het MA-serviceaccount voor elk forest wachtwoorden beheren namens de gebruikersaccounts in dat forest. 

> [!IMPORTANT]
> Als u niet de volgende machtigingen toe te wijzen, klikt u vervolgens treden terugschrijven weergegeven correct worden geconfigureerd, maar gebruikers fouten op bij de poging voor het beheren van hun on-premises wachtwoorden vanuit de cloud.
>

> [!NOTE]
> Het kan duren tot een uur of meer voor de machtigingen zijn gerepliceerd naar alle objecten in uw directory.
>

Als u de juiste machtigingen voor terugschrijving wachtwoord instelt, moet u de volgende stappen uitvoeren:

1. Open Active Directory: gebruikers en Computers met een account met de juiste machtigingen voor domeinbeheer.
2. Van de **weergave** menu, zorg ervoor dat **geavanceerde functies** is ingeschakeld.
3. In het linkerdeelvenster met de rechtermuisknop op het object waarmee de hoofdmap van het domein en selecteer **eigenschappen** > **beveiliging** > **Geavanceerd**.
4. Van de **machtigingen** tabblad **toevoegen**.
5. Kies het account dat machtigingen worden toegepast op (van de Azure AD Connect-installatie).
6. In de **is van toepassing op** vervolgkeuzelijst, selecteer **onderliggende gebruiker** objecten.
7. Onder **machtigingen**, schakelt u de selectievakjes voor het volgende:
    * **Wachtwoord opnieuw instellen**
    * **Wachtwoord wijzigen**
    * **LockoutTime schrijven**
    * **PwdLastSet schrijven**
8. Selecteer **toepassen/OK** worden de wijzigingen toegepast en sluit alle geopende dialoogvensters.

## <a name="licensing-requirements-for-password-writeback"></a>Licentievereisten voor write-back van wachtwoord

Zie voor meer informatie over licentieverlening [licenties nodig zijn voor write-back van wachtwoord](active-directory-passwords-licensing.md#licenses-required-for-password-writeback) of de volgende sites:

* [Azure Active Directory website prijzen](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-that-are-supported-for-password-writeback"></a>Lokale verificatiemodi die worden ondersteund voor write-back van wachtwoord

Wachtwoord terugschrijven ondersteuning voor de volgende typen van de gebruiker-wachtwoord:

* **Alleen in de cloud gebruikers**: wachtwoord terugschrijven is niet van toepassing in dit geval, omdat er geen on-premises wachtwoord.
* **Het wachtwoord is gesynchroniseerd gebruikers**: wachtwoord terugschrijven wordt ondersteund.
* **Federatieve gebruikers**: wachtwoord terugschrijven wordt ondersteund.
* **Pass through-verificatie gebruikers**: wachtwoord terugschrijven wordt ondersteund.

### <a name="user-and-admin-operations-that-are-supported-for-password-writeback"></a>Gebruikers- en admin bewerkingen die worden ondersteund voor write-back van wachtwoord

Wachtwoorden worden teruggeschreven in de volgende situaties:

* **Ondersteunde eindgebruikers**
  * Alle eindgebruikers selfservice vrijwillige bewerking wachtwoord wijzigen
  * Alle eindgebruikers selfservice force wachtwoord bewerking, bijvoorbeeld verlopen van wachtwoorden wijzigen
  * Alle eindgebruikers selfservice voor wachtwoordherstel dat afkomstig van is de [portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com)
* **Ondersteunde bewerkingen**
  * Een beheerder selfservice vrijwillige bewerking wachtwoord wijzigen
  * Een beheerder selfservice force wachtwoord bewerking, bijvoorbeeld verlopen van wachtwoorden wijzigen
  * Een beheerder selfservice voor wachtwoordherstel dat afkomstig van is de [portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com)
  * Een beheerder worden geïnitieerd door eindgebruikers wachtwoord opnieuw instellen van de [Azure-portal](https://portal.azure.com)

### <a name="user-and-admin-operations-that-are-not-supported-for-password-writeback"></a>Gebruikers- en admin bewerkingen die worden niet ondersteund voor write-back van wachtwoord

De wachtwoorden *niet* teruggeschreven in een van de volgende situaties:

* **Niet-ondersteunde eindgebruikers**
  * Een eindgebruiker hun eigen wachtwoord opnieuw instellen met behulp van PowerShell-versie 1, versie 2 of de Azure AD Graph API
* **De beheerder van de niet-ondersteunde bewerkingen**
  * Een beheerder worden geïnitieerd door eindgebruikers wachtwoord opnieuw instellen van de [Office-beheerportal](https://portal.office.com)
  * Een beheerder worden geïnitieerd door eindgebruikers voor wachtwoordherstel van PowerShell-versie 1, versie 2 of de Azure AD Graph API

We proberen te verwijderen van deze beperkingen, maar er zijn momenteel geen een specifieke tijdlijn die we nog kunt delen.

## <a name="password-writeback-security-model"></a>Wachtwoord terugschrijven beveiligingsmodel

Wachtwoord terugschrijven is een maximaal beveiligde service. Uw gegevens worden beschermd, zodat inschakelen we een beveiligingsmodel vier lagen die wordt beschreven als volgt:

* **Tenant-specifieke service bus relay**
  * Bij het instellen van de service, u stelt een tenantspecifieke service bus relay die wordt beveiligd door een willekeurig gegenereerd sterk wachtwoord dat Microsoft nooit toegang tot heeft.
* **De versleutelingssleutel vergrendelde, cryptografisch sterk wachtwoord**
  * Nadat de service bus relay is gemaakt, maken we een sterke symmetrische sleutel die wordt gebruikt voor het versleutelen van het wachtwoord als ze afkomstig zijn via de kabel. Deze sleutel bevinden zich alleen in geheime store in de cloud, die sterk is vergrendeld en gecontroleerd, net als elk ander wachtwoord in de map van uw bedrijf.
* **Bedrijfstak standaard Transport Layer Security (TLS)**
  1. Wanneer u een wachtwoord opnieuw instellen of wijzigen bewerking doet zich voor in de cloud, we nemen het leesbare wachtwoord coderen met uw openbare sleutel.
  2. We plaats die in een HTTPS-bericht dat wordt verzonden via een versleuteld kanaal met behulp van Microsoft SSL-certificaten voor uw service bus relay.
  3. Nadat het bericht in de servicebus ontvangt, wordt de agent van uw lokale ontwaakt en geverifieerd aan de servicebus met behulp van de sterke wachtwoorden die eerder is gegenereerd.
  4. De lokale agent het gecodeerde bericht opneemt en ontsleutelt deze met behulp van de persoonlijke sleutel die wordt gegenereerd.
  5. De lokale agent probeert het wachtwoord via de API van AD DS SetPassword in te stellen. Deze stap is wat ons af te dwingen van het beleid van uw Active Directory on-premises wachtwoord (zoals de complexiteit, leeftijd, geschiedenis en filters) is toegestaan in de cloud.
* **Het verloopbeleid bericht** 
  * Als het bericht bevindt zich in de servicebus omdat uw on-premises service niet actief is, een time-out optreedt en wordt verwijderd na enkele minuten. De time-out en het verwijderen van het bericht verhoogt de beveiliging nog verder.

### <a name="password-writeback-encryption-details"></a>Wachtwoord terugschrijven versleuteling details

Nadat een gebruiker een wachtwoord opnieuw instellen verzendt, gaat de aanvraag opnieuw instellen via verschillende stappen voor het versleutelen voordat deze in uw on-premises omgeving binnenkomt. Deze stappen voor het versleutelen Zorg ervoor dat de maximale service betrouwbaarheid en beveiliging. Ze worden als volgt beschreven:

* **Stap 1: Wachtwoordversleuteling met 2048-bits RSA-sleutel**: nadat een gebruiker een wachtwoord worden teruggeschreven naar on-premises verzendt, het opgegeven wachtwoord zelf is versleuteld met een 2048-bits RSA-sleutel.
* **Stap 2: Niveau Package versleuteling met AES-GCM**: het volledige pakket, het wachtwoord, plus de vereiste metagegevens is versleuteld met AES-GCM. Deze versleuteling voorkomt dat iedereen met directe toegang naar de onderliggende service bus-kanaal weergeven of knoeien met de inhoud.
* **Stap 3: Alle communicatie vindt plaats via TLS/SSL**: alle communicatie met Service Bus gebeurt in een SSL/TLS-kanaal. Deze versleuteling beveiligt de inhoud van een niet-geautoriseerde derde partij.
* **Automatische sleutelrollover om de zes maanden**: om de zes maanden of elke keer wachtwoord terugschrijven is uitgeschakeld en vervolgens weer wordt ingeschakeld op Azure AD Connect. We rollover automatisch plaats alle sleutels om te controleren of de maximale service en veiligheid.

### <a name="password-writeback-bandwidth-usage"></a>Wachtwoord terugschrijven bandbreedtegebruik

Wachtwoord terugschrijven is een lage bandbreedte-service die alleen aanvragen naar de lokale agent in de volgende omstandigheden verzendt:

* Twee berichten worden verzonden wanneer de functie is ingeschakeld of uitgeschakeld met Azure AD Connect.
* Een bericht wordt elke vijf minuten als een heartbeat-service voor verzonden als de service wordt uitgevoerd.
* Twee berichten worden verzonden op elk moment een nieuw wachtwoord wordt ingediend:
    * Het eerste bericht is een verzoek om de bewerking niet uitvoeren.
    * Het tweede bericht bevat het resultaat van de bewerking en wordt verzonden in de volgende omstandigheden:
        * Telkens wanneer een nieuw wachtwoord wordt ingediend tijdens een gebruiker zelf uw wachtwoord opnieuw instellen.
        * Telkens wanneer een nieuw wachtwoord wordt ingediend tijdens een wachtwoordwijziging van gebruiker.
        * Telkens wanneer een nieuw wachtwoord wordt ingediend tijdens een beheerder geïnitieerde gebruikerswachtwoord opnieuw instellen (alleen van de portals Azure admin).

#### <a name="message-size-and-bandwidth-considerations"></a>Overwegingen voor het grootte en bandbreedte van het bericht

De grootte van elk van de eerder beschreven bericht is doorgaans dan 1 KB. De service voor het terugschrijven van wachtwoord zelf verbruikt enkele kilobits per seconde van de bandbreedte zelfs onder extreme is geladen. Omdat elk bericht wordt verzonden in realtime, is alleen indien vereist door een updatebewerking wachtwoord, en omdat de berichtgrootte zo klein is het bandbreedtegebruik van de mogelijkheid Write-back te klein voor een merkbare invloed.

## <a name="next-steps"></a>Volgende stappen

* [Hoe kan ik een geslaagde implementatie van SSPR voltooien?](active-directory-passwords-best-practices.md)
* [Uw wachtwoord opnieuw instellen of wijzigen](active-directory-passwords-update-your-own-password.md).
* [Registreer u voor selfservice voor wachtwoordherstel](active-directory-passwords-reset-register.md).
* [Hebt u een vraag over licenties?](active-directory-passwords-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](active-directory-passwords-data.md)
* [Welke verificatiemethoden zijn beschikbaar voor gebruikers?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](active-directory-passwords-policy.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](active-directory-passwords-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](active-directory-passwords-how-it-works.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Writeback]: ./media/active-directory-passwords-writeback/enablepasswordwriteback.png "Wachtwoord terugschrijven in Azure AD Connect inschakelen"
