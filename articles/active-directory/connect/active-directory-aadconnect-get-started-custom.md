---
title: 'Azure AD Connect: Aangepaste installatie | Microsoft Docs'
description: In dit document worden de opties voor een aangepaste installatie van Azure AD Connect beschreven. Volg deze instructies om Active Directory met Azure AD Connect te installeren.
services: active-directory
keywords: wat is Azure AD Connect, Active Directory installeren, vereiste onderdelen voor Azure AD
documentationcenter: 
author: billmath
manager: mtillman
ms.assetid: 6d42fb79-d9cf-48da-8445-f482c4c536af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/02/2018
ms.author: billmath
ms.openlocfilehash: a12bd2ec296acfb810c8805c92941e5bf70c6ccb
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="custom-installation-of-azure-ad-connect"></a>Custom installation of Azure AD Connect (Engelstalig)
Voor meer opties voor de installatie gaat u naar **Aangepaste instellingen**. Deze instellingen gebruikt u wanneer u meerdere forests hebt of als u optionele functies wilt configureren die niet in de snelle installatie voorkomen. De aangepaste instellingen worden gebruikt in alle gevallen waarin de optie [**snelle installatie**](active-directory-aadconnect-get-started-express.md) niet aan uw implementatie of topologie voldoet.

Zorg ervoor dat u, voordat u begint met de installatie van Azure AD Connect, [Azure AD Connect downloadt](http://go.microsoft.com/fwlink/?LinkId=615771) en de vereiste stappen in [Azure AD Connect: Hardware and prerequisites](active-directory-aadconnect-prerequisites.md) voltooit. Zorg er ook voor dat de benodigde accounts beschikbaar zijn, zoals beschreven in [Azure AD Connect accounts and permissions](active-directory-aadconnect-accounts-permissions.md).

Als de aangepaste instellingen niet met uw topologie overeenkomen, bijvoorbeeld om DirSync te upgraden, bekijk dan de [verwante documentatie](#related-documentation) voor andere scenario's.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Installatie van Azure AD Connect met aangepaste instellingen
### <a name="express-settings"></a>Snelle instellingen
Klik op deze pagina op **Aanpassen** om met een installatie met aangepaste instellingen te beginnen.

### <a name="install-required-components"></a>Vereiste onderdelen installeren
Wanneer u de synchronisatieservices installeert, kunt de optie voor optionele configuratie uitschakelen: Azure AD Connect stelt alles dan automatisch in. Het programma stelt een exemplaar van SQL Server 2012 Express LocalDB in, maakt de juiste groepen aan en wijst machtigingen toe. Als u de standaardinstellingen wilt wijzigen, kunt u in de volgende tabel zien welke opties voor optionele configuratie beschikbaar zijn.

![Vereiste onderdelen](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

| Optionele configuratie | Beschrijving |
| --- | --- |
| Een bestaande SQL Server gebruiken |Hiermee kunt u de naam van de SQL Server en de exemplaarnaam opgeven. Kies deze optie als u al een databaseserver heeft die u wilt gebruiken. Voer de exemplaarnaam, gevolgd door een komma en poortnummer, in bij **Exemplaarnaam** als bladeren niet is ingeschakeld voor uw SQL Server. |
| Een bestaand serviceaccount gebruiken |Azure AD Connect maakt standaard gebruik van een virtueel serviceaccount voor de synchronisatieservices. Als u een externe SQL-server gebruikt of een proxyserver waarvoor verificatie is vereist, hebt u een **beheerd serviceaccount** of een serviceaccount in het domein nodig en moet u het wachtwoord kennen. Voer in dat geval het te gebruiken account in. Zorg dat de gebruiker die de installatie uitvoert een SA in SQL is, zodat een aanmelding voor het serviceaccount kan worden aangemaakt. Zie [Azure AD Connect accounts and permissions](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) |
| Aangepaste synchronisatiegroepen opgeven |Azure AD Connect maakt standaard vier groepen lokaal op de server aan wanneer de synchronisatieservices worden geïnstalleerd. Deze groepen zijn: Beheerders, Operators, Bladeren en Wachtwoord opnieuw instellen. U kunt hier uw eigen groepen opgeven. De groepen moeten lokaal op de server zijn en mogen zich niet in het domein bevinden. |

### <a name="user-sign-in"></a>Gebruikersaanmelding
Nadat de vereiste onderdelen zijn geïnstalleerd, wordt u gevraagd een eenmalige aanmeldmethode voor uw gebruikers te selecteren. In de volgende tabel staan de beschikbare opties kort beschreven. Zie voor een volledige beschrijving van de aanmeldmethodes [User sign-in](active-directory-aadconnect-user-signin.md).

![Aanmelding door een gebruiker](./media/active-directory-aadconnect-get-started-custom/usersignin2.png)

| Optie voor eenmalige aanmelding | Beschrijving |
| --- | --- |
| Wachtwoordhashsynchronisatie |Gebruikers kunnen zich bij Microsoft-cloudservices, zoals Office 365, aanmelden met hetzelfde wachtwoord als ze in hun on-premises netwerk gebruiken. De wachtwoorden van gebruikers worden gesynchroniseerd naar Azure AD als een wachtwoord-hash en verificatie vindt plaats in de cloud. Zie [Wachtwoordhashsynchronisatie](active-directory-aadconnectsync-implement-password-synchronization.md) voor meer informatie. |
|Pass-through-verificatie|Gebruikers kunnen zich bij Microsoft-cloudservices, zoals Office 365, aanmelden met hetzelfde wachtwoord als ze in hun on-premises netwerk gebruiken.  Het wachtwoord van de gebruiker wordt doorgegeven aan de on-premises Active Directory-domeincontroller voor validatie.
| Federatie met AD FS |Gebruikers kunnen zich bij Microsoft-cloudservices, zoals Office 365, aanmelden met hetzelfde wachtwoord als ze in hun on-premises netwerk gebruiken.  De gebruikers worden omgeleid naar hun on-premises AD FS-exemplaar om zich aan te melden en de verificatie vindt plaats on-premises. |
| Niet configureren |Er is geen functie voor gebruikersaanmelding geïnstalleerd en geconfigureerd. Kies deze optie als u al een federatieserver van derden of een andere bestaande oplossing heeft. |
|Eenmalige aanmelding inschakelen|Deze optie is beschikbaar bij zowel wachtwoordsynchronisatie als Pass-through-verificatie en biedt een eenmalige aanmelding voor desktopgebruikers binnen het bedrijfsnetwerk. Zie [Eenmalige aanmelding](active-directory-aadconnect-sso.md) voor meer informatie. </br>Deze optie is niet beschikbaar voor AD FS-klanten omdat AD FS hetzelfde niveau van eenmalige aanmelding biedt.</br>

### <a name="connect-to-azure-ad"></a>Verbinding maken met Azure AD
Voer op het scherm Verbinding maken met Azure AD het account en wachtwoord van een globale beheerder in. Als u op de vorige pagina **Federatie met AD FS** hebt geselecteerd, meld u dan niet aan met een account in een domein waarvoor u federatie wilt inschakelen. Het is aan te raden om een account te gebruiken uit het standaarddomein **onmicrosoft.com**, dat bij uw Azure AD-directory wordt geleverd.

Dit account wordt alleen gebruikt om een serviceaccount in Azure AD aan te maken en wordt niet gebruikt wanneer de wizard is voltooid.  
![Aanmelding door een gebruiker](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Als MFA voor het account van de globale beheerder is ingeschakeld, dan moet u het wachtwoord opnieuw in het aanmeldpopupvenster invoeren en de MFA-controle voltooien. De controle kan bestaan uit het invoeren van een verificatiecode of uit een telefonische oproep.  
![Aanmelding door een gebruiker bij MFA](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

Voor het account van de globale beheerder kan ook [Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md) zijn ingeschakeld.

Zie [Connectiviteitsproblemen oplossen](active-directory-aadconnect-troubleshoot-connectivity.md) als u een foutbericht krijgt en u problemen hebt met de connectiviteit.

## <a name="pages-under-the-sync-section"></a>Pagina's in de sectie Synchronisatie

### <a name="connect-your-directories"></a>Verbinding maken met uw directory’s
Azure AD Connect heeft de forestnaam en de referenties van een account met de juiste machtigingen nodig om verbinding met uw Active Directory-domeinservice te maken.

![Verbinding maken met Directory](./media/active-directory-aadconnect-get-started-custom/connectdir01.png)

Nadat u de forestnaam hebt ingevoerd en op **Map toevoegen** hebt geklikt, wordt een pop-upvenster met de volgende opties weergegeven:

| Optie | Beschrijving |
| --- | --- |
| Nieuw account maken | Selecteer deze optie als u wilt dat de Azure AD Connect-wizard het AD DS-account maakt dat Azure AD Connect nodig heeft om verbinding te maken met de AD-forest tijdens directorysynchronisatie. Voer wanneer deze optie is geselecteerd de gebruikersnaam en het wachtwoord in voor een enterprisebeheerdersaccount. Het opgegeven enterprisebeheerdersaccount wordt door de Azure AD Connect-wizard gebruikt om het vereiste AD DS-account te maken. U kunt het domeingedeelte in NetBios- of FQDN-indeling invoeren, dat wil zeggen FABRIKAM\administrator of fabrikam.com\administrator. |
| Bestaand account gebruiken | Selecteer deze optie als u wilt toestaan dat een bestaand AD DS-account wordt gebruikt door Azure AD Connect om verbinding te maken met de AD-forest tijdens directorysynchronisatie. U kunt het domeingedeelte in NetBios- of FQDN-indeling invoeren, dat wil zeggen FABRIKAM\syncuser of fabrikam.com\syncuser. Dit account mag een normaal gebruikersaccount zijn, omdat alleen de standaard leesmachtigingen nodig zijn. Afhankelijk van uw scenario heeft u echter mogelijk meer machtigingen nodig. Zie [Azure AD Connect Accounts and permissions](active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account) voor meer informatie. |

![Verbinding maken met Directory](./media/active-directory-aadconnect-get-started-custom/connectdir02.png)

### <a name="azure-ad-sign-in-configuration"></a>Aanmeldconfiguratie Azure AD
Op deze pagina kunt u bekijken welke UPN-domeinen zich in de on-premises AD DS bevinden en in Azure AD zijn geverifieerd. Daarnaast kunt u op deze pagina het kenmerk configureren dat voor de userPrincipalName moet worden gebruikt.

![Niet-geverifieerde domeinen](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Bekijk elk domein waarbij **Niet toegevoegd** en **Niet geverifieerd** staat. Zorg ervoor dat de domeinen die u gebruikt in Azure AD zijn geverifieerd. Klik op het symbool Vernieuwen wanneer u uw domeinen hebt geverifieerd. Zie voor meer informatie [add and verify the domain](../active-directory-domains-add-azure-portal.md)

**UserPrincipalName** - Met het kenmerk userPrincipalName melden gebruikers zich aan bij Azure AD en Office 365. De gebruikte domeinen, ook wel het UPN-achtervoegsel genoemd, moeten worden geverifieerd in Azure AD voordat de gebruikers worden gesynchroniseerd. Het wordt door Microsoft aangeraden om het standaardkenmerk userPrincipalName te behouden. Als dit kenmerk niet-routeerbaar is en niet kan worden geverifieerd, dan kunt u een ander kenmerk selecteren. U kunt bijvoorbeeld e-mail selecteren als het kenmerk met het aanmeldings-id. Het gebruik van een ander kenmerk dan userPrincipalName wordt **alternatieve id** genoemd. De waarde van het alternatieve-id-kenmerk moet aan de standaard RFC822 voldoen. Een alternatieve id kan worden gebruikt met wachtwoordsynchronisatie en federatie. Het kenmerk mag in Active Directory niet met meerdere waarden worden gedefinieerd, zelfs als het slechts één waarde heeft.

>[!NOTE]
> Als u Pass-through-verificatie inschakelt, moet u ten minste één geverifieerd domein hebben om de wizard te voltooien.

> [!WARNING]
> Het gebruik van een alternatieve id is niet met alle Office 365-werkbelastingen compatibel. Zie [Alternatieve aanmeldings-id configureren](https://technet.microsoft.com/library/dn659436.aspx).
>
>

### <a name="domain-and-ou-filtering"></a>Domein- en OE-filters
Standaard worden alle domeinen en OE's gesynchroniseerd. Van domeinen of OE’s die u niet wilt synchroniseren naar Azure AD kunt u de selectie opheffen.  
![Domein- en OE-filters](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png)  
Op deze pagina in de wizard configureert u filteren op basis van een domein en OE. Als u wijzigingen wilt aanbrengen, gaat u naar [domain-based filtering](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) en [OE filteren](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) voordat u deze wijzigingen aanbrengt. Sommige OE's zijn essentieel voor de functionaliteit en mogen niet worden uitgeschakeld.

Als u OE-filters gebruikt met een versie van Azure AD Connect lager dan 1.1.524.0, worden nieuwe OE's die later zijn toegevoegd, standaard gesynchroniseerd. Als u wilt dat nieuwe OE's niet worden gesynchroniseerd, kunt u dit met [OE filteren](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) instellen nadat de wizard is voltooid. Voor Azure AD Connect versie 1.1.524.0 en hoger kunt u aangeven of u wilt dat nieuwe OE's moeten worden gesynchroniseerd of niet.

Als u [Groep filteren](#sync-filtering-based-on-groups) wilt gebruiken, zorgt u ervoor dat de OE met de groep is opgenomen en dat u niet filtert met OE filteren. OE filteren wordt geëvalueerd vóór groep filteren.

Sommige domeinen kunnen ook onbereikbaar zijn door firewallbeperkingen. Deze domeinen zijn standaard uitgeschakeld en hebben een waarschuwing.  
![Onbereikbare domeinen](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Als u deze waarschuwing ziet, verzeker uzelf er dan van dat deze domeinen inderdaad onbereikbaar zijn en de waarschuwing wordt verwacht.

### <a name="uniquely-identifying-your-users"></a>Uw gebruikers een unieke id geven

#### <a name="select-how-users-should-be-identified-in-your-on-premises-directories"></a>Selecteren hoe gebruikers moeten worden aangeduid in uw on-premises directory’s
Met de functie Overeenkomend in forests kunt u definiëren hoe gebruikers van uw AD DS-forests worden weergegeven in Azure AD. Een gebruiker kan ofwel slechts één keer in alle forests worden weergegeven of een combinatie van ingeschakelde en uitgeschakelde accounts hebben. De gebruiker kan in sommige forests ook worden weergegeven als een contactpersoon.

![Uniek](./media/active-directory-aadconnect-get-started-custom/unique.png)

| Instelling | Beschrijving |
| --- | --- |
| [Gebruikers worden slechts één keer weergegeven in alle forests](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Alle gebruikers worden als afzonderlijke objecten in Azure AD aangemaakt. De objecten zijn niet gekoppeld in de metaverse. |
| [E-mailkenmerk](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Deze optie koppelt gebruikers en contactpersonen als het e-mailkenmerk in verschillende forests dezelfde waarde heeft. Gebruik deze optie wanneer uw contactpersonen met behulp van GALSync zijn aangemaakt. Als u deze optie kiest, worden gebruikersobjecten waarvan het e-mailkenmerk niet is ingevuld, niet naar Azure AD gesynchroniseerd. |
| [ObjectSID en msExchangeMasterAccountSID / msRTCSIP-OriginatorSid](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Deze optie koppelt een ingeschakelde gebruiker in een account-forest met een uitgeschakelde gebruiker in een bron-forest. In Exchange wordt deze configuratie een gekoppeld postvak genoemd. Deze optie kan ook worden gebruikt als u alleen Lync gebruikt en Exchange niet in de bron-forest aanwezig is. |
| sAMAccountName en MailNickName |Deze optie koppelt kenmerken waarbij wordt verwacht dat de aanmeldings-id van de gebruiker gevonden kan worden. |
| Een specifiek kenmerk |Met deze optie kunt u uw eigen kenmerk selecteren. Als u deze optie kiest, worden gebruikersobjecten waarvan het (geselecteerde) kenmerk niet is ingevuld, niet naar Azure AD gesynchroniseerd. **Beperking:** Zorg dat u een kenmerk kiest dat al in de metaverse te vinden is. Als u een aangepast kenmerk kiest dat niet in de metaverse staat, kan de wizard niet voltooid worden. |

#### <a name="select-how-users-should-be-identified-with-azure-ad---source-anchor"></a>Selecteren hoe gebruikers moeten worden aangeduid met Azure AD - bronanker
Het kenmerk sourceAnchor is onveranderbaar tijdens de levensduur van een gebruikersobject. Het is de primaire sleutel die de on-premises gebruiker aan de gebruiker in Azure AD koppelt.

| Instelling | Beschrijving |
| --- | --- |
| Azure het bronanker voor mij laten beheren | Selecteer deze optie als u wilt dat Azure AD het kenmerk voor u selecteert. Als u deze optie selecteert, wordt in de wizard Azure AD Connect de selectielogica voor het kenmerk sourceAnchor toegepast die wordt beschreven in [Azure AD Connect: Design concepts - Using msDS-ConsistencyGuid as sourceAnchor](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor) (Azure AD Connect: Ontwerpconcepten - msDS-ConsistencyGuid gebruiken als sourceAnchor). De wizard geeft aan welk kenmerk is geselecteerd als het kenmerk SourceAnchor nadat de aangepaste installatie is voltooid. |
| Een specifiek kenmerk | Selecteer deze optie als u een bestaand AD-kenmerk opgeeft als het kenmerk sourceAnchor. |

Omdat het kenmerk niet kan worden gewijzigd, moet u een goed kenmerk kiezen. Een goede kandidaat is objectGUID. Dit kenmerk wordt niet gewijzigd, tenzij het gebruikersaccount worden verplaatst tussen forests/domeinen. In een omgeving met meerdere forests waarin u accounts tussen forests verplaatst, moet een ander kenmerk worden gebruikt, zoals een kenmerk met de id van de werknemer. Vermijd kenmerken die wijzigen wanneer iemand trouwt of een andere taak krijgt. U kunt geen kenmerken met een @-sign gebruiken, dus het e-mailadres en userPrincipalName kunnen niet worden gebruikt. Het kenmerk is ook hoofdlettergevoelig, dus als u een object tussen forests verplaatst, zorg dan dat de hoofdletters en kleine letters hetzelfde blijven. Binaire kenmerken krijgen base64-codering, maar andere kenmerktypen blijven ongecodeerd. In scenario's met federatie en in sommige Azure AD-interfaces wordt dit kenmerk ook wel onveranderbare id genoemd. Meer informatie over het bronanker vindt u in de [ontwerpconcepten](active-directory-aadconnect-design-concepts.md#sourceanchor).

### <a name="sync-filtering-based-on-groups"></a>Synchronisatiefilters op basis van groepen
Met de functie Filteren op groep kunt u bij wijze van proef een kleine subset van objecten synchroniseren. Om deze functie te gebruiken maakt u voor dit doel een groep aan in uw on-premises Active Directory. Voeg vervolgens gebruikers en groepen toe die naar Azure AD moeten worden gesynchroniseerd als directe leden. U kunt later gebruikers aan deze groep toevoegen en eruit verwijderen om de lijst te onderhouden met objecten die in Azure AD aanwezig moeten zijn. Alle objecten die u wilt synchroniseren moet een direct lid van de groep zijn. Gebruikers, groepen, contactpersonen en computers/apparaten moeten allemaal directe leden zijn. Genest groepslidmaatschap is niet opgelost. Wanneer u een groep als lid toevoegt, wordt alleen de groep zelf toegevoegd en niet de leden ervan.

![Synchronisatiefilters](./media/active-directory-aadconnect-get-started-custom/filter2.png)

> [!WARNING]
> Deze functie is alleen bedoeld ter ondersteuning van een proef-implementatie. Gebruik deze functie niet bij een volledige productie-implementatie.
>
>

Bij een volledige productie-implementatie wordt het door alle objecten die gesynchroniseerd moeten worden, moeilijk om één groep te behouden. Gebruik in plaats daarvan een van de methodes in [Configure filtering](active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Optionele functies
In dit scherm kunt u de optionele functies voor uw specifieke scenario's selecteren.

![Optionele functies](./media/active-directory-aadconnect-get-started-custom/optional.png)

> [!WARNING]
> Als DirSync of Azure AD Sync momenteel actief zijn, activeer dan geen terugschrijffuncties in Azure AD Connect.
>
>

| Optionele functies | Beschrijving |
| --- | --- |
| Hybride implementatie voor Exchange |Met de functie Hybride implementatie voor Exchange kunnen on-premises en in Office 365 meerdere Exchange-postbussen naast elkaar bestaan. Azure AD Connect synchroniseert een specifieke set [kenmerken](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) vanuit Azure AD naar uw on-premises directory. |
| Openbare e-mailmappen van Exchange | Met de functie Openbare e-mailmappen van Exchange kunt u de voor e-mail ingeschakelde openbare mapobjecten van Active Directory synchroniseren naar Azure AD. |
| Azure AD-app- en -kenmerkfilters |Door de Azure AD-app- en -kenmerkfilters in te schakelen kan de set gesynchroniseerde kenmerken worden aangepast. Door deze optie worden twee extra configuratiepagina’s aan de wizard toegevoegd. Zie voor meer informatie [Azure AD app and attribute filtering](#azure-ad-app-and-attribute-filtering). |
| Wachtwoordsynchronisatie |Als u federatie als de oplossing voor aanmelden hebt geselecteerd, dan kunt u deze optie inschakelen. Wachtwoordsynchronisatie kan vervolgens als een backupoptie worden gebruikt. Zie [Password synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) voor meer informatie. </br></br>Als u Pass-through-verificatie hebt geselecteerd, kan deze optie ook worden ingeschakeld als ondersteuning voor verouderde clients en als back-upoptie. Zie [Password synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) voor meer informatie.|
| Wachtwoord terugschrijven |Door wachtwoord terugschrijven in te schakelen worden wachtwoordwijzigingen in Azure AD teruggeschreven naar uw on-premises directory. Zie voor meer informatie [Getting started with password management](../active-directory-passwords-getting-started.md). |
| Groep terugschrijven |Als u de functie **Office 365-groepen** gebruikt, dan kunnen deze groepen in uw on-premises Active Directory worden weergegeven. Deze optie is alleen beschikbaar als Exchange in uw on-premises Active Directory aanwezig is. Zie voor meer informatie [Group writeback](active-directory-aadconnect-feature-preview.md#group-writeback). |
| Apparaat terugschrijven |Hiermee kunt u apparaatobjecten in Azure AD terugschrijven naar uw on-premises Active Directory voor scenario's voor voorwaardelijke toegang. Zie voor meer informatie [Enabling device writeback in Azure AD Connect](active-directory-aadconnect-feature-device-writeback.md). |
| Synchronisatie van directory-extensiekenmerken |Door synchronisatie van directory-extensiekenmerken in te schakelen worden de opgegeven kenmerken gesynchroniseerd naar Azure AD. Zie voor meer informatie [Directory extensions](active-directory-aadconnectsync-feature-directory-extensions.md). |

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD-app- en -kenmerkfilters
Als u wilt beperken welke kenmerken met Azure AD moeten worden gesynchroniseerd, begin dan met te selecteren welke services u gebruikt. Als u de configuratie op deze pagina wijzigt, moet u expliciet een nieuwe service selecteren door de installatiewizard opnieuw uit te voeren.

![Optionele functies apps](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Op deze pagina worden alle kenmerken die worden gesynchroniseerd weergegeven op basis van de services die u in de vorige stap heeft geselecteerd. In deze lijst staan alle objecttypen die worden gesynchroniseerd. Als er een aantal specifieke kenmerken zijn die niet moeten worden gesynchroniseerd, dan kunt u de selectie van deze kenmerken opheffen.

![Optionele functies kenmerken](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

> [!WARNING]
> Het verwijderen van kenmerken kan invloed hebben op de functionaliteit. Zie voor aanbevolen procedures en andere aanbevelingen [attributes synchronized](active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).
>
>

### <a name="directory-extension-attribute-sync"></a>Synchronisatie van directory-extensiekenmerken
U kunt het schema in Azure AD uitbreiden met aangepaste kenmerken die door uw organisatie zijn toegevoegd of met andere kenmerken in Active Directory. Om deze functie te gebruiken, selecteert u **Synchronisatie van directory-extensiekenmerken** op de pagina **optionele functies**. U kunt op deze pagina meer kenmerken selecteren om te synchroniseren.

![Uitbreidingen van de directory](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Zie voor meer informatie [Directory extensions](active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="enabling-single-sign-on-sso"></a>Eenmalige aanmelding (SSO) inschakelen
Het configureren van eenmalige aanmelding voor gebruik met wachtwoordsynchronisatie of passthrough-verificatie is een eenvoudig proces dat u slechts één keer hoeft te doorlopen voor elk forest dat wordt gesynchroniseerd met Azure AD. De configuratie omvat de volgende twee stappen:

1.  Maak het benodigde computeraccount in uw on-premises Active Directory.
2.  Configureer de intranetzone van de clientcomputers voor de ondersteuning van eenmalige aanmelding.

#### <a name="create-the-computer-account-in-active-directory"></a>Het computeraccount maken in Active Directory
Voor elk forest in Azure AD Connect dat is toegevoegd, moet u referenties van de domeinbeheerder opgeven, zodat het computeraccount in elk forest kan worden gemaakt. De referenties worden alleen gebruikt om het account te maken. Ze worden niet opgeslagen of voor andere zaken gebruikt. Voeg de referenties toe op de pagina **Eenmalige aanmelding inschakelen** van de wizard Azure AD Connect, zoals hieronder wordt weergegeven:

![Eenmalige aanmelding inschakelen](./media/active-directory-aadconnect-get-started-custom/enablesso.png)

>[!NOTE]
>Forests waarvoor u eenmalige aanmelding niet wilt gebruiken, kunt u overslaan.

#### <a name="configure-the-intranet-zone-for-client-machines"></a>De intranetzone voor clientcomputers configureren
Als u ervoor wilt zorgen dat de client zich automatisch aanmeldt in de intranetzone, moeten de URL's deel uitmaken van de intranetzone. De desktopcomputers die via het domein zijn gekoppeld, verzenden dan automatisch een Kerberos-ticket wanneer ze zijn verbonden met het bedrijfsnetwerk.
Op een computer met de hulpprogramma's voor Groepsbeleidsbeheer.

1.  Open de hulpprogramma's voor Groepsbeleidsbeheer
2.  Bewerk het groepsbeleid dat op alle gebruikers wordt toegepast. Bijvoorbeeld het standaard domeinbeleid.
3.  Ga naar **Gebruikersconfiguratie\Beheersjablonen\Windows-onderdelen\Internet Explorer\Onderdeel Internetopties van het Configuratiescherm\Het tabblad Beveiliging** en selecteer **Lijst van zonetoewijzingen voor websites**, zoals in de afbeelding hieronder.
4.  Schakel het beleid in en geef de volgende twee items op in het dialoogvenster.

        Value: `https://autologon.microsoftazuread-sso.com`  
        Data: 1  
        Value: `https://aadg.windows.net.nsatc.net`  
        Data: 1

5.  Het moet er ongeveer als volgt uitzien:  
![Intranetzones](./media/active-directory-aadconnect-get-started-custom/sitezone.png)

6.  Klik twee keer op **OK**.

## <a name="configuring-federation-with-ad-fs"></a>Federatie configureren met AD FS
AD FS is heel eenvoudig met een paar muisklikken met Azure AD Connect te configureren. Voorafgaand aan de configuratie is het volgende vereist.

* Een Windows Server 2012 R2-server of later als de federatieserver, waarop extern beheer is ingeschakeld
* Een Windows Server 2012 R2-server of later als de webtoepassingsproxyserver, waarop extern beheer is ingeschakeld
* Een SSL-certificaat voor de Federation Service-naam die u wilt gebruiken (bijvoorbeeld sts.contoso.com)

>[!NOTE]
>U kunt het SSL-certificaat van de AD FS-farm bijwerken met behulp van Azure AD Connect, zelfs als u Azure AD Connect niet gebruikt om de federatieve vertrouwensrelatie te beheren.

### <a name="ad-fs-configuration-pre-requisites"></a>Vereisten voor de AD FS-configuratie
Zorg dat WinRM is ingeschakeld op de externe servers om uw AD FS-farm met behulp van Azure AD Connect te configureren. Zorg ervoor dat u de andere taken in [federatievereisten](active-directory-aadconnect-prerequisites.md#prerequisites-for-federation-installation-and-configuration) hebt uitgevoerd. Neem daarnaast de vereisten voor poorten door die vermeld staan in [Tabel 3 - Azure AD Connect en federatieve servers/WAP](active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-ad-fs-federation-serverswap).

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Maak een nieuwe AD FS-farm aan of gebruik een bestaande AD FS-farm
U kunt een bestaande AD FS-farm gebruiken of u kunt ervoor kiezen een nieuwe AD FS-farm aan te maken. Als u ervoor kiest om een nieuwe aan te maken, dan dient u het SSL-certificaat op te geven. Als het SSL-certificaat met een wachtwoord is beveiligd, dan wordt u gevraagd het wachtwoord op te geven.

![AD FS-Farm](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Als u een bestaande AD FS-farm gebruikt, wordt u meteen doorgestuurd naar het scherm De vertrouwensrelatie tussen AD FS en Azure AD configureren.

>[!NOTE]
>Azure AD Connect kan worden gebruikt om slechts één AD FS-farm te beheren. Als op de geselecteerde AD FS-farm al een federatieve vertrouwensrelatie is geconfigureerd, wordt deze relatie helemaal opnieuw gemaakt met Azure AD Connect.

### <a name="specify-the-ad-fs-servers"></a>Geef de AD FS-servers op
Voer de servers in waarop u AD FS wilt installeren. U kunt naar gelang de behoeften van uw capaciteitsplanning een of meer servers toevoegen. Koppel alle AD FS-servers (niet vereist voor de WAP-servers) aan Active Directory voordat u deze configuratie uitvoert. Het wordt door Microsoft aangeraden om voor proefimplementaties één AD FS-server te installeren. Voeg vervolgens meer servers toe en implementeer deze om aan uw schaalbehoeften te voldoen door Azure AD Connect na de eerste configuratie opnieuw uit te voeren.

> [!NOTE]
> Zorg ervoor dat alle servers aan een AD-domein zijn gekoppeld voordat u deze configuratie uitvoert.
>
>

![AD FS-Servers](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Geef de webtoepassingsproxyservers op
Voer de servers in die u als uw webtoepassingsproxyservers wilt gebruiken. De webtoepassingsproxyserver wordt in uw DMS (extranetgericht) geïmplementeerd en ondersteunt verificatieaanvragen van het extranet. U kunt naar gelang de behoeften van uw capaciteitsplanning een of meer servers toevoegen. Het wordt door Microsoft aangeraden om voor proefimplementaties één webtoepassingsproxyserver te installeren. Voeg vervolgens meer servers toe en implementeer deze om aan uw schaalbehoeften te voldoen door Azure AD Connect na de eerste configuratie opnieuw uit te voeren. Het is aan te raden een gelijk aantal proxyservers te hebben om aan de verificatie van het intranet te voldoen.

> [!NOTE]
> <li> Als het account dat u gebruikt geen lokale beheerder op de WAP-servers is, wordt u gevraagd om beheerreferenties.</li>
> <li> Zorg ervoor dat er een HTTP/HTTPS-verbinding tussen de Azure AD Connect-server en de webtoepassingsproxyserver is voordat u deze stap uitvoert.</li>
> <li> Zorg ervoor dat er een HTTP/HTTPS-verbinding tussen de webtoepassingsserver en de AD FS-server is, zodat verificatieaanvragen door kunnen stromen.</li>
>

![Web-app](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

U wordt gevraagd referenties op te geven zodat de webtoepassingsserver een beveiligde verbinding met de AD FS-server kan maken. Deze referenties moeten van een lokale beheerder op de AD FS-server zijn.

![Proxy](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Geef het serviceaccount voor de AD FS-service op
De AD FS-service vereist een domeinserviceaccount om gebruikers te verifiëren en gebruikersinformatie in Active Directory op te zoeken. De service kan twee soorten serviceaccounts ondersteunen:

* **Beheerd serviceaccount voor groepen** - Geïntroduceerd in Active Directory-domainservices met WindowsServer 2012. Dit type account biedt services, zoals AD FS, één account zonder dat het accountwachtwoord regelmatig bijgewerkt hoeft te worden. Gebruik deze optie als u al Windows Server 2012-domeincontrollers hebt in het domein waarbij uw AD FS-servers horen.
* **Domeingebruikersaccount** - Voor dit type account moet u een wachtwoord opgeven en het wachtwoord regelmatig bijwerken wanneer het wachtwoord wordt gewijzigd of verloopt. Gebruik deze optie alleen als u geen Windows Server 2012-domeincontrollers heeft in het domein waarbij uw AD FS-servers horen.

Als u Beheerd serviceaccount voor groepen heeft geselecteerd en deze functie nog nooit in Active Directory is gebruikt, wordt u gevraagd om referenties van de ondernemingsbeheerder. Deze referenties worden gebruikt om de sleutelopslagplaats te beginnen en de functie in Active Directory in te schakelen.

> [!NOTE]
> Azure AD Connect controleert of de AD FS-service al is geregistreerd als een SPN in het domein.  Het is in AD DS niet toegestaan om in één bewerking dubbele SPN's te registreren.  Als er een dubbele SPN wordt gevonden, kunt u pas verdergaan nadat de SPN is verwijderd.

![AD FS-serviceaccount](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Selecteer het Azure AD-domein dat u wilt federeren
Deze configuratie wordt gebruikt voor het instellen van de federatieverbinding tussen AD FS en Azure AD. AD FS wordt hiermee geconfigureerd om beveiligingstokens aan Azure AD te verstrekken en Azure AD wordt geconfigureerd om de tokens van deze specifieke AD FS-instantie te vertrouwen. Op deze pagina kunt u tijdens de eerste installatie slechts één domein configureren. U kunt later meer domeinen configureren door Azure AD Connect nogmaals uit te voeren.

![Azure AD-domein](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Controleer het Azure AD-domein dat voor federatie is geselecteerd
Wanneer u het te federeren domein selecteert, geeft Azure AD Connect u de informatie die u nodig heeft om een niet-geverifieerd domein te verifiëren. Zie [Add and verify the domain](../active-directory-domains-add-azure-portal.md) om uit te vinden hoe deze informatie gebruikt moet worden.

![Azure AD-domein](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

> [!NOTE]
> AD Connect probeert het domein tijdens de configuratiefase te verifiëren. Als u doorgaat met de configuratie zonder de vereiste DNS-records toe te voegen, dan kan de wizard de configuratie niet voltooien.
>
>

## <a name="configure-and-verify-pages"></a>Configureer en verifieer pagina 's
De configuratie wordt op deze pagina uitgevoerd.

> [!NOTE]
> Als u de federatie heeft geconfigureerd, zorg dan dat u, voordat u doorgaat met de installatie, de [Naamomzetting voor federatieservers](active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers) heeft geconfigureerd.
>
>

![Klaar om te configureren](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Faseringsmodus
Het is mogelijk om een nieuwe synchronisatieserver parallel met de faseringsmodus in te stellen. Het is alleen ondersteund als u één synchronisatieserver naar één map in de cloud laat exporteren. Als u echter vanaf een andere server wilt verplaatsen, bijvoorbeeld een server met DirSync, dan kunt u Azure AD Connect in de faseringsmodus inschakelen. Wanneer dit is ingeschakeld, importeert en synchroniseert de synchronisatie-engine zoals gewoonlijk, maar hij exporteert niets naar Azure AD of AD. De functies Wachtwoordsynchronisatie en Wachtwoord terugschrijven zijn uitgeschakeld in de faseringsmodus.

![Faseringsmodus](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

In de faseringsmodus kunt u vereiste wijzigingen in de synchronisatie-engine aanbrengen en controleren wat geëxporteerd gaat worden. Voer de installatiewizard opnieuw uit en schakel de faseringsmodus uit wanneer de configuratie er goed uitziet. De gegevens zijn nu van deze server naar Azure AD geëxporteerd. Schakel de andere server op hetzelfde moment uit, zodat slechts één server actief aan het exporteren is.

Zie voor meer informatie [Staging mode](active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Controleer uw federatieconfiguratie
Azure AD Connect verifieert de DNS-instellingen voor u wanneer u op Verifiëren klikt.

**Controles voor connectiviteit intranet**

* Federatie-FQDN omzetten: Azure AD Connect controleert of de FQDN van de federatie via DNS kan worden omgezet om de connectiviteit te controleren. Als de FQDN kan niet worden omgezet in Azure AD Connect, mislukt de verificatie. Zorg ervoor dat er een DNS-record aanwezig is voor de FQDN van de federatieservice om de verificatie voltooien.
* DNS A-record: Azure AD Connect controleert of er een A-record is voor uw federatieservice. Als het A-record ontbreekt, mislukt de verificatie. Maak een A-record voor de FQDN van uw federatie, in plaats van een CNAME-record, om de verificatie voltooien.

**Controles voor connectiviteit extranet**

* Federatie-FQDN omzetten: Azure AD Connect controleert of de FQDN van de federatie via DNS kan worden omgezet om de connectiviteit te controleren.

![Voltooien](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Verifiëren](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Voer daarnaast de volgende verificatiestappen uit:

* Controleer of u zich kunt aanmelden met een browser op een computer op het intranet die aan het domein is gekoppeld: Maak verbinding met https://myapps.microsoft.com en controleer de aanmelding met uw aangemelde account. Het ingebouwde beheerdersaccount van AD DS wordt niet gesynchroniseerd en kan niet worden gebruikt voor verificatie.
* Controleer of u zich kunt aanmelden met een apparaat vanaf het extranet. Maak op een computer thuis of op een mobiel apparaat verbinding met https://myapps.microsoft.com en voer uw referenties in.
* Aanmelding uitgebreide client controleren. Maak verbinding met https://testconnectivity.microsoft.com, kies het tabblad **Office 365** en vervolgens de **Office 365 Test Eenmalige aanmelding**.

## <a name="next-steps"></a>Volgende stappen
Nadat de installatie is voltooid, dient u zich af te melden en weer aan te melden bij Windows voordat u de Synchronization Service Manager of Synchronization Rule Editor gaat gebruiken.

Nu u Azure AD Connect geïnstalleerd hebt kunt u [de installatie verifiëren en licenties toewijzen](active-directory-aadconnect-whats-next.md).

Meer informatie over deze functies, die tijdens de installatie zijn ingeschakeld: [Prevent accidental deletes](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) en [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Meer informatie over deze algemene onderwerpen: [scheduler and how to trigger sync](active-directory-aadconnectsync-feature-scheduler.md).

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
