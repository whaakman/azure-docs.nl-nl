---
title: 'Azure AD Connect: Migreren van Federatie naar wachtwoord-hashsynchronisatie voor Azure AD | Microsoft Docs'
description: In dit artikel bevat informatie over het verplaatsen van uw hybride identiteit omgeving van Federatie naar wachtwoord-hashsynchronisatie.
services: active-directory
author: billmath
manager: daveba
ms.reviewer: martincoetzer
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/13/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a2effd6c067a1378d9f774f282f6cea69a50596c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56204437"
---
# <a name="migrate-from-federation-to-password-hash-synchronization-for-azure-active-directory"></a>Migreren van Federatie naar wachtwoord-hashsynchronisatie voor Azure Active Directory

In dit artikel wordt beschreven hoe uw organisatie-domeinen van Active Directory Federation Services (AD FS) naar wachtwoord-hashsynchronisatie verplaatsen.

U kunt [downloaden van dit artikel](https://aka.ms/ADFSTOPHSDPDownload).

## <a name="prerequisites-for-migrating-to-password-hash-synchronization"></a>Vereisten voor het migreren naar wachtwoord-hashsynchronisatie

De volgende vereisten zijn vereist voor de migratie van AD FS voor het gebruik van wachtwoord-hashsynchronisatie.

### <a name="update-azure-ad-connect"></a>Update van Azure AD Connect

Minimaal uit te voeren de stappen voor het migreren naar wachtwoord-hashsynchronisatie, moeten er [Azure AD connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0. Deze versie bevat belangrijke wijzigingen aangebracht in de manier waarop aanmelden conversie wordt uitgevoerd en vermindert de totale tijd voor het migreren van Federatie naar Cloud-verificatie van mogelijk uren en minuten.


> [!IMPORTANT]
> U kunt lezen in verouderde documentatie, hulpprogramma's en blogs gebruikersconversie is vereist wanneer u domeinen van federatieve identiteiten naar beheerde identiteit converteren. *Converteren van gebruikers* is niet langer vereist. Microsoft werkt aan voor het bijwerken van documentatie en hulpprogramma's om deze wijziging weer te geven.

Voor het bijwerken van Azure AD Connect u de stappen in [Azure AD Connect: Een upgrade uitvoeren naar de meest recente versie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

### <a name="password-hash-synchronization-required-permissions"></a>Wachtwoord-hash-synchronisatie vereist machtigingen

U kunt Azure AD Connect configureren met behulp van de express-instellingen of een aangepaste installatie. Als u de optie aangepaste installatie gebruikt de [vereiste machtigingen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-accounts-permissions) voor wachtwoord-hash synchronisatie kan niet worden voldaan.

De Azure AD verbinding maken met Active Directory Domain Services (AD DS)-serviceaccount moet de volgende machtigingen voor het synchroniseren van wachtwoord-hashes:

* Directorywijzigingen repliceren
* Alle repliceren Directory gewijzigd

Nu is een goed moment om te controleren of dat deze machtigingen ingesteld voor alle domeinen in het forest zijn.

### <a name="plan-the-migration-method"></a>De migratiemethode plannen

U kunt kiezen uit twee methoden voor het migreren van een federatieve identiteitsbeheer naar wachtwoord-hashsynchronisatie en naadloze eenmalige aanmelding (SSO). De methode die u gebruikt, is afhankelijk van hoe uw exemplaar van AD FS voor het oorspronkelijk is geconfigureerd.

* **Azure AD Connect**. Als u AD FS oorspronkelijk is geconfigureerd met behulp van Azure AD Connect u *moet* wijzigen naar wachtwoord-hashsynchronisatie met behulp van de Azure AD Connect-wizard.

   Azure AD Connect voert automatisch de **Set MsolDomainAuthentication** cmdlet als u de gebruiker aanmelden methode wijzigen. Azure AD Connect unfederates automatisch alle gecontroleerde federatieve domeinen in uw Azure AD-tenant.

   > [!NOTE]
   > Op dit moment als u Azure AD Connect oorspronkelijk hebt gebruikt voor het configureren van AD FS, kan niet worden vermeden unfederating alle domeinen in uw tenant wanneer u de aanmelding voor gebruiker naar de wachtwoord-hashsynchronisatie overschakelt. ‎
* **Azure AD Connect met PowerShell**. U kunt deze methode alleen gebruiken als u AD FS oorspronkelijk niet configureren met behulp van Azure AD Connect. Voor deze optie, moet u nog steeds de gebruiker aanmelden methode via de Azure AD Connect-wizard wijzigen. Het belangrijkste verschil met deze optie is dat de wizard automatisch wordt niet uitgevoerd de **Set MsolDomainAuthentication** cmdlet. Met deze optie is hebt u volledige controle over welke domeinen worden geconverteerd en in welke volgorde.

Als u wilt weten welke methode u moet gebruiken, de stappen in de volgende secties.

#### <a name="verify-current-user-sign-in-settings"></a>Controleer of de huidige gebruiker aanmelden instellingen

Om te controleren of de huidige gebruiker aanmelden instellingen:

1. Aanmelden bij de [Azure AD-portal](https://aad.portal.azure.com/) met behulp van een globale beheerdersaccount.
2. In de **aanmelden van gebruikers** sectie, controleert u of de volgende instellingen:
   * **Federatieve** is ingesteld op **ingeschakeld**.
   * **Naadloze eenmalige aanmelding** is ingesteld op **uitgeschakelde**.
   * **Pass through-verificatie** is ingesteld op **uitgeschakelde**.

   ![Schermafbeelding van de instellingen in de sectie van de aanmelding bij Azure AD Connect-gebruiker](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image1.png)

#### <a name="verify-the-azure-ad-connect-configuration"></a>Controleer of de Azure AD Connect-configuratie

1. Open op uw Azure AD Connect-server, Azure AD Connect. Selecteer **configureren**.
2. Op de **extra taken** weergeeft, schakelt **de huidige configuratie weergeven**, en selecteer vervolgens **volgende**.<br />

   ![Schermafbeelding van de huidige configuratie-optie van weergave, geselecteerd op de pagina extra taken](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image2.png)<br />
3. Op de **uw oplossing controleren** pagina, houd er rekening mee de **wachtwoord-hashsynchronisatie** status.<br /> 

   * Als **wachtwoord-hashsynchronisatie** is ingesteld op **uitgeschakelde**, de stappen in dit artikel om te schakelen.
   * Als **wachtwoord-hashsynchronisatie** is ingesteld op **ingeschakeld**, kunt u de sectie overslaan **stap 1: Synchronisatie van wachtwoordhashes inschakelen** in dit artikel.
4. Op de **uw oplossing controleren** pagina, bladert u naar **Active Directory Federation Services (AD FS)**.<br />

   * Als de AD FS-configuratie wordt weergegeven in deze sectie, kunt u veilig ervan uitgaan dat AD FS oorspronkelijk is geconfigureerd met behulp van Azure AD Connect. U kunt uw domeinen van federatieve identiteiten converteren naar beheerde identiteit met behulp van de Azure AD Connect **aanmelden van gebruikers wijzigen** optie. Het proces wordt beschreven in de sectie **optie A: Overschakelen van Federatie naar wachtwoord-hashsynchronisatie met behulp van Azure AD Connect**.
   * Als AD FS niet wordt weergegeven in de huidige instellingen, moet u handmatig converteren uw domeinen van federatieve identiteiten naar beheerde identiteit met behulp van PowerShell. Zie de sectie voor meer informatie over dit proces **optie B: Overschakelen van Federatie naar wachtwoord-hashsynchronisatie met behulp van Azure AD Connect en PowerShell**.

### <a name="document-current-federation-settings"></a>Document huidige federatie-instellingen

Als wilt uw huidige federatie-instellingen weten, voert de **Get-MsolDomainFederationSettings** cmdlet:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName YourDomain.extention | fl *
```

Voorbeeld:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName Contoso.com | fl *
```

Controleer de instellingen die mogelijk zijn aangepast voor uw federation-documentatie voor ontwerp en implementatie. Specifiek, zoekt u aanpassingen in **PreferredAuthenticationProtocol**, **SupportsMfa**, en **PromptLoginBehavior**.

Raadpleeg voor meer informatie de volgende artikelen:

* [AD FS-prompt = aanmelding parameter ondersteuning](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)
* [Set-MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> Als **SupportsMfa** is ingesteld op **waar**, u een on-premises multi-factor authentication-oplossing gebruikt voor het invoeren van een tweede factor-uitdaging in de gebruikersstroom voor verificatie. Deze instelling werkt niet meer voor scenario's Azure AD-verificatie. 
>
> Gebruik in plaats daarvan de cloud gebaseerde service van Azure multi-factor Authentication om uit te voeren dezelfde functie. Evalueer zorgvuldig de vereisten voor meervoudige verificatie voordat u doorgaat. Voordat u uw domeinen converteert, zorg ervoor dat u weet hoe u Azure multi-factor Authentication, de gevolgen voor de licenties en het registratieproces voor de gebruiker.

#### <a name="back-up-federation-settings"></a>Back-up van federatie-instellingen

Hoewel tijdens de processen die worden beschreven in dit artikel geen wijzigingen aan andere Relying Party's in uw AD FS-farm aangebracht worden, raden wij aan dat u een huidige geldige back-up van uw AD FS-farm die u hebt vanuit herstellen kunt. U kunt een huidige geldige back-up maken met behulp van de gratis Microsoft [AD FS Rapid Restore-hulpprogramma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool). U kunt het hulpprogramma gebruiken om back-up van AD FS te herstellen van een bestaande farm of maak een nieuwe farm.

Als u ervoor kiest om niet te gebruiken van de AD FS Rapid Restore-hulpprogramma, ten minste, moet u de Microsoft Office 365 Identity Platform relying partyvertrouwensrelatie en bijbehorende aangepaste claimregels u toegevoegd exporteren. U kunt de vertrouwensrelatie van Relying Party en de bijbehorende claimregels exporteren met behulp van de volgende PowerShell-voorbeeld:

``` PowerShell
(Get-AdfsRelyingPartyTrust -Name "Microsoft Office 365 Identity Platform") | Export-CliXML "C:\temp\O365-RelyingPartyTrust.xml"
```

## <a name="deployment-considerations-and-using-ad-fs"></a>Overwegingen voor de implementatie en het gebruik van AD FS

Deze sectie beschrijft overwegingen voor de implementatie en meer informatie over het gebruik van AD FS.

### <a name="current-ad-fs-use"></a>Huidige AD FS gebruiken

Voordat u van federatieve identiteiten naar beheerde identiteit converteren, nauw kijken naar hoe u momenteel AD FS gebruiken voor Azure AD, Office 365, en andere toepassingen (relying party's). Specifiek, houd rekening met de scenario's die worden beschreven in de volgende tabel:

| Als | Then |
|-|-|
| U wilt AD FS met andere toepassingen blijven gebruiken (anders dan Azure AD en Office 365). | Nadat u uw domeinen converteren, gebruikt u zowel AD FS en Azure AD. Houd rekening met de gebruikerservaring. In sommige scenario's mogelijk gebruikers worden gevraagd zich twee keer verifiëren: één keer naar Azure AD (waarbij een gebruiker opgehaald voor toegang via eenmalige aanmelding voor andere toepassingen, zoals Office 365) en voor alle toepassingen die nog steeds zijn gebonden aan de AD FS als een relying party. |
| Uw exemplaar van AD FS kan sterk worden aangepast en is afhankelijk van aanpassingsinstellingen voor specifieke in het bestand OnLoad.js (bijvoorbeeld, als u de aanmeldingsprocedure hebt gewijzigd, zodat gebruikers met alleen een **SamAccountName** indeling voor hun gebruikersnaam in plaats van een gebruiker heeft Principal Name (UPN) of uw organisatie sterk merkproducten van de aanmeldingservaring). Het bestand OnLoad.js kan niet worden gedupliceerd in Azure AD. | Voordat u doorgaat, moet u controleren of Azure AD kunt voldoen aan de aanpassingsvereisten van uw huidige. Voor meer informatie en richtlijnen, Zie de secties van de huisstijl van de AD FS en AD FS-aanpassing.|
| AD FS kunt u eerdere versies van verificatieclients blokkeren.| Houd rekening met het vervangen van AD FS-besturingselementen die blokkeren van eerdere versies van verificatieclients op basis van een combinatie van [besturingselementen voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) en [toegangsregels voor Exchange Online clients](http://aka.ms/EXOCAR). |
| U vereisen dat gebruikers multi-factor authentication tegen een on-premises multi-factor authentication server-oplossing uitvoeren wanneer gebruikers bij AD FS authenticeren.| In een domein beheerde identiteit kan niet u een vraag met meervoudige verificatie via de on-premises multi-factor authentication-oplossing invoeren in de verificatie-stroom. U kunt echter de Azure multi-factor Authentication-service voor meervoudige verificatie gebruiken nadat het domein is geconverteerd.<br /><br /> Als uw gebruikers geen momenteel van Azure multi-factor Authentication gebruikmaakt, is een eenmalige gebruiker registratiestap vereist. U moet voorbereiden en de registratie van de geplande aan uw gebruikers communiceren. |
| U gebruikt momenteel beleid voor toegangsbeheer (AuthZ-regels) in AD FS voor het beheren van toegang tot Office 365.| Houd rekening met het beleid vervangen door de equivalente Azure AD [beleid voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) en [toegangsregels voor Exchange Online clients](http://aka.ms/EXOCAR).|

### <a name="common-ad-fs-customizations"></a>Algemene AD FS-aanpassingen

Deze sectie beschrijft de algemene AD FS-aanpassingen.

#### <a name="insidecorporatenetwork-claim"></a>InsideCorporateNetwork claim

Problemen met AD FS de **InsideCorporateNetwork** claim als de gebruiker die is geverifieerd binnen het bedrijfsnetwerk is. Deze claim kan vervolgens worden doorgegeven aan Azure AD. De claim wordt gebruikt om multi-factor authentication op basis van de netwerklocatie van de gebruiker overslaan. Zie voor informatie over het bepalen of deze functionaliteit momenteel is ingeschakeld in AD FS, [goedgekeurde IP-adressen voor federatieve gebruikers](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud).

De **InsideCorporateNetwork** claim is niet beschikbaar nadat u uw domeinen worden geconverteerd naar wachtwoord-hashsynchronisatie. U kunt [benoemde locaties in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) ter vervanging van deze functionaliteit.

Nadat u benoemde locaties hebt geconfigureerd, moet u alle beleidsregels voor voorwaardelijke toegang die zijn geconfigureerd als u wilt opnemen of uitsluiten van het netwerk bijwerken **alle vertrouwde locaties** of **MFA goedgekeurde IP-adressen** moeten worden zijn de nieuwe benoemde locaties.

Voor meer informatie over de **locatie** voorwaarde in voor voorwaardelijke toegang, Zie [Active Directory-locaties voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).

#### <a name="hybrid-azure-ad-joined-devices"></a>Hybride Azure AD join-apparaten

Wanneer u een apparaat aan Azure AD toevoegen, kunt u regels voor voorwaardelijke toegang die afdwingen dat apparaten voldoen aan uw standaarden voor beveiliging en naleving maken. Ook kunnen gebruikers zich aanmelden met een apparaat met behulp van een organisatie-werk- of schoolaccount in plaats van een persoonlijk account. Bij het gebruik van hybride Azure AD join-apparaten, kunt u uw Active Directory-domein gekoppelde apparaten toevoegen aan Azure AD. Uw federatieve omgeving mogelijk zijn ingesteld om deze functie te gebruiken.

Om ervoor te zorgen dat join hybride blijft werken voor alle apparaten die zijn gekoppeld aan het domein nadat uw domeinen worden geconverteerd naar wachtwoord-hashsynchronisatie voor Windows 10-clients, moet u Azure AD Connect gebruiken om te synchroniseren van Active Directory-computeraccounts naar Azure AD. 

Voor Windows 8 en Windows 7-computeraccounts, hybride join naadloze eenmalige aanmelding gebruikt voor het registreren van de computer in Azure AD. U hoeft niet te synchroniseren Windows 8 en Windows 7-computeraccounts, zoals u ook voor Windows 10-apparaten. U moet echter een bijgewerkte workplacejoin.exe-bestand (via een MSI-bestand) op Windows 8 en Windows 7-clients implementeren, zodat ze zich inschrijven kunnen met behulp van naadloze eenmalige aanmelding. [Download het MSI-bestand](https://www.microsoft.com/download/details.aspx?id=53554).

Zie voor meer informatie, [configureren hybride Azure AD gekoppelde apparaten](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup).

#### <a name="branding"></a>Huisstijl

Als uw organisatie [aangepast van uw AD FS sign-in's](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) als u wilt weergeven van informatie die meer relevant is voor de organisatie, kunt u soortgelijke [aanpassingen van de Azure AD-aanmeldingspagina](https://docs.microsoft.com/azure/active-directory/customize-branding).

Hoewel vergelijkbaar aanpassingen die beschikbaar zijn, worden sommige visuele element wordt aangepast op aanmelden pagina's na de conversie verwacht. Mogelijk wilt u meer informatie over de verwachte wijzigingen in uw communicatie bieden aan gebruikers.

> [!NOTE]
> Huisstijl van uw organisatie is beschikbaar, alleen als u de Premium- of Basic-licentie voor Azure Active Directory kopen of als u een Office 365-licentie hebt.

## <a name="plan-deployment-and-support"></a>Implementatie en ondersteuning plannen

Voltooi de taken die worden beschreven in deze sectie om te plannen voor implementatie en ondersteuning.

### <a name="plan-the-maintenance-window"></a>Het onderhoudsvenster te plannen

Hoewel de conversie van het domein relatief snel is, blijven Azure AD kan sommige verificatieaanvragen verzenden naar uw AD FS-servers voor maximaal vier uur nadat de domein-conversie is voltooid. Tijdens deze vier uur, en afhankelijk van verschillende caches op de service, kan deze verificaties niet geaccepteerd door Azure AD. Gebruikers mogelijk een foutbericht weergegeven. De gebruiker kan nog steeds is verificatie op basis van AD FS, maar Azure AD accepteert geen van de gebruiker token krijgen omdat die federatieve vertrouwensrelatie is nu verwijderd.

Alleen gebruikers die toegang hebben tot de services via een webbrowser tijdens deze periode na de conversie voordat de cache op de service is uitgeschakeld, worden beïnvloed. Verouderde clients (Exchange ActiveSync, Outlook 2010-2013) worden niet verwacht worden beïnvloed, omdat Exchange Online een cache van hun referenties voor een bepaalde periode houdt. De cache wordt gebruikt om de gebruiker op de achtergrond verifiëren. De gebruiker beschikt niet over om terug te keren naar AD FS. Referenties die zijn opgeslagen op het apparaat voor deze clients worden gebruikt om zelf op de achtergrond verifiëren nadat deze opgeslagen in de cache is uitgeschakeld. Gebruikers worden niet verwacht voor het ontvangen van een wachtwoord wordt gevraagd als gevolg van het proces voor conversie. 

Moderne verificatieclients (Office 2016 en Office 2013, iOS en Android-apps) gebruik een geldige vernieuwingstoken om nieuwe toegangstokens voor blijvende toegang tot resources in plaats van retourneren aan de AD FS. Deze clients is immuun voor een wachtwoord wordt gevraagd die voortvloeien uit de conversie van het domein. De clients zullen blijven werken zonder aanvullende configuratie.

> [!IMPORTANT]
> Niet afsluiten van uw AD FS-omgeving of verwijderen van de Office 365 relying party trust totdat u hebt geverifieerd dat alle gebruikers verifiëren kunnen met behulp van cloudverificatie.

### <a name="plan-for-rollback"></a>Plan voor terugdraaien

Als er een groot probleem die snel kan niet worden omgezet, moet u besluiten terugdraaien van de oplossing voor Federatie. Het is belangrijk om te plannen wat te doen als uw distributie niet uitgerold zoals bedoeld. Als de conversie van het domein of de gebruikers is mislukt tijdens de implementatie, of als u wilt terugkeren naar federation, moet u het beperken van een storing voordoet en het effect op uw gebruikers te begrijpen.

#### <a name="to-roll-back"></a>Om terug te draaien

Om te plannen voor ongedaan maken, raadpleegt u de federation-ontwerp en implementatie van documentatie voor uw specifieke implementatie-informatie. Het proces moet deze taken omvatten:

* Beheerde domeinen converteren naar federatieve domeinen met behulp van de **Convert-MSOLDomainToFederated** cmdlet.
* Indien nodig, worden er regels van aanvullende configureren.

### <a name="plan-communications"></a>De communicatie plannen

Een belangrijk onderdeel van de planning, implementatie en ondersteuning is ervoor te zorgen dat uw gebruikers proactief op de hoogte van toekomstige wijzigingen zijn. Gebruikers moeten van tevoren weten wat ze zich kunnen voordoen en wat zijn de vereisten van beide. 

Nadat de beide wachtwoord-hashsynchronisatie en naadloze eenmalige aanmelding zijn geïmplementeerd, de gebruiker aanmelden voor toegang tot Office 365 en andere resources die worden geverifieerd via wijzigingen in de Azure AD. Gebruikers die zich buiten het netwerk zien alleen de Azure AD-aanmeldingspagina. Deze gebruikers worden niet omgeleid naar de pagina op basis van formulieren, dat wordt aangeboden door extern gerichte web application proxy-servers.

Neem de volgende elementen in uw Communicatiestrategie voor:

* Melding over toekomstige en uitgebrachte functies met behulp van:
   * E-mail en andere interne communicatiekanalen.
   * Visuele elementen, zoals posters.
   * Executive, live of andere communicatie.
* Bepalen wie de communicatie wordt aangepast en die de communicatie wordt verzonden en wanneer.

## <a name="implement-your-solution"></a>Uw oplossing implementeren

U uw oplossing gepland. Nu kunt u nu deze implementeren. Implementatie bestaat uit de volgende onderdelen:

* Wachtwoord-hashsynchronisatie inschakelen.
* Voorbereiden voor naadloze eenmalige aanmelding.
* De aanmeldingsmethode wordt gewijzigd in de wachtwoord-hashsynchronisatie en naadloze eenmalige aanmelding in te schakelen.

### <a name="step-1-enable-password-hash-synchronization"></a>Stap 1: Synchronisatie van wachtwoordhashes inschakelen

De eerste stap bij het implementeren van deze oplossing is wachtwoord-hashsynchronisatie inschakelen met behulp van de Azure AD Connect-wizard. Wachtwoord-hashsynchronisatie is een optionele functie die u in omgevingen die gebruikmaken van federatieve kunt inschakelen. Er is geen invloed op de verificatie-stroom. In dit geval wordt Azure AD Connect synchronisatie van wachtwoord-hashes zonder gevolgen voor gebruikers die zich aanmelden met behulp van de federation starten.

Daarom is het raadzaam deze stap als een voorbereidingstaak goed te voltooien voordat u de aanmeldingsmethode van uw domein wijzigt. Vervolgens hebt u voldoende tijd om te controleren of wachtwoord-hashsynchronisatie werkt goed.

Wachtwoord-hashsynchronisatie inschakelen:

1. Open de Azure AD Connect-wizard op de Azure AD Connect-server en selecteer vervolgens **configureren**.
2. Selecteer **aanpassen Synchronisatieopties**, en selecteer vervolgens **volgende**.
3. Op de **verbinding maken met Azure AD** pagina, voer de gebruikersnaam en wachtwoord van een globale beheerdersaccount.
4. Op de **verbinding maken met uw mappen** weergeeft, schakelt **volgende**.
5. Op de **domein en OE filteren** weergeeft, schakelt **volgende**.
6. Op de **optionele functies** weergeeft, schakelt **Wachtwoordsynchronisatie**, en selecteer vervolgens **volgende**.
 
   ![Schermafbeelding van de optie voor het synchroniseren van wachtwoord hebt geselecteerd op de pagina optionele functies](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image6.png)<br />
7. Selecteer **volgende** op de overige pagina's. Selecteer op de laatste pagina **configureren**.
8. Azure AD Connect begint met het synchroniseren van wachtwoord-hashes in de volgende synchronisatie.

Nadat de wachtwoord-hashsynchronisatie is ingeschakeld, de wachtwoord-hashes voor alle gebruikers in de Azure AD Connect-synchronisatie bereik worden nieuwe en geschreven naar Azure AD. Afhankelijk van het aantal gebruikers, kan deze bewerking minuten of enkele uren duren.

Voor de planning, moet u schat dat ongeveer 20.000 gebruikers, worden verwerkt binnen 1 uur.

Om te controleren dat wachtwoord-hashsynchronisatie goed werkt, voltooi de **probleemoplossing** taak in de Azure AD Connect-wizard:

1. Open een nieuwe Windows PowerShell-sessie op uw Azure AD Connect-server met behulp van de uitvoeren als beheerder.
2. Voer `Set-ExecutionPolicy RemoteSigned` of `Set-ExecutionPolicy Unrestricted`.
3. Start de Azure AD Connect-wizard.
4. Ga naar de **extra taken** weergeeft, schakelt **oplossen**, en selecteer vervolgens **volgende**.
5. Op de **probleemoplossing** weergeeft, schakelt **starten** voor het oplossen van problemen met het menu start in PowerShell.
6. Selecteer in het hoofdmenu **wachtwoord-hashsynchronisatie oplossen**.
7. Selecteer in het submenu **wachtwoord-hashsynchronisatie werkt niet helemaal**.

Zie voor het oplossen van problemen, [wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie oplossen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization).

### <a name="step-2-prepare-for-seamless-sso"></a>Stap 2: Voorbereiden voor naadloze eenmalige aanmelding

Voor uw apparaten naadloze eenmalige aanmelding gebruiken, moet u een Azure AD-URL toevoegen aan zone-instellingen van gebruikers intranet met behulp van Groepsbeleid in Active Directory.

Standaard berekenen webbrowsers automatisch de juiste zone internet- of intranethosts vanuit een URL. Bijvoorbeeld, **http:\/\/contoso /** wordt toegewezen aan de zone Lokaal intranet en **http:\/\/intranet.contoso.com** toegewezen aan de zone internet omdat ( de URL bevat een periode). Alleen als u de URL expliciet aan de intranetzone van de browser toevoegen, browsers Kerberos-tickets verzenden naar een cloudeindpunt, zoals de Azure AD-URL.

Voltooi de stappen voor het [uitgerold](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) de benodigde wijzigingen aan uw apparaten.

> [!IMPORTANT]
> Deze wijziging aanbrengt, niet de manier waarop uw gebruikers zich aanmelden bij Azure AD wijzigen. Het is echter belangrijk dat u deze configuratie op al uw apparaten, toepassen voordat u doorgaat. Gebruikers die zich aanmelden op apparaten die nog niet hebt ontvangen van deze configuratie zijn gewoon vereist om in te voeren van een gebruikersnaam en wachtwoord aanmelden bij Azure AD.

### <a name="step-3-change-the-sign-in-method-to-password-hash-synchronization-and-enable-seamless-sso"></a>Stap 3: De aanmeldingsmethode naar wachtwoord-hashsynchronisatie en naadloze eenmalige aanmelding inschakelen

U hebt twee opties voor het wijzigen van de aanmeldingsmethode naar wachtwoord-hashsynchronisatie en naadloze eenmalige aanmelding inschakelen.

#### <a name="option-a-switch-from-federation-to-password-hash-synchronization-by-using-azure-ad-connect"></a>Optie A: Overschakelen van Federatie naar wachtwoord-hashsynchronisatie met behulp van Azure AD Connect

Gebruik deze methode als u uw AD FS-omgeving in eerste instantie geconfigureerd met behulp van Azure AD Connect. U kunt deze methode niet gebruiken als u *niet* oorspronkelijk uw AD FS-omgeving configureren met behulp van Azure AD Connect.

Wijzig eerst de aanmeldingsmethode:

1. Open op de Azure AD Connect-server, de Azure AD Connect-wizard.
2. Selecteer **aanmelden van gebruikers wijzigen**, en selecteer vervolgens **volgende**. 

   ![Schermafbeelding van de wijziging gebruiker aanmelden optie op de pagina extra taken](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image7.png)<br />
3. Op de **verbinding maken met Azure AD** pagina, voer de gebruikersnaam en wachtwoord van een globale beheerdersaccount.
4. Op de **aanmelden van gebruikers** weergeeft, schakelt de **knop van wachtwoord-hash-synchronisatie**. Zorg ervoor dat u selecteert de **gebruikersaccounts niet converteren** selectievakje. De optie is afgeschaft. Selecteer **eenmalige aanmelding inschakelen**, en selecteer vervolgens **volgende**.

   ![Schermafbeelding van de inschakelen-pagina eenmalige aanmelding](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image8.png)<br />

   > [!NOTE]
   > Beginnen met Azure AD Connect versie 1.1.880.0, de **naadloze eenmalige aanmelding** selectievakje is standaard geselecteerd.

   > [!IMPORTANT]
   > U kunt de waarschuwingen die dat gebruikersconversie aangeven veilig negeren en volledige synchronisatie van wachtwoordhashes zijn vereiste stappen voor het converteren van Federatie naar cloud-verificatie. Houd er rekening mee dat deze stappen zijn niet vereist meer. Als er nog steeds deze waarschuwingen, zorg ervoor dat u de meest recente versie van Azure AD Connect en voert u de nieuwste versie van deze handleiding. Zie voor meer informatie de sectie [Update van Azure AD Connect](#update-azure-ad-connect).

5. Op de **eenmalige aanmelding inschakelen** pagina, voert u de referenties van domein Administrator-account en selecteer vervolgens **volgende**.

   ![Schermafbeelding van de inschakelen-pagina eenmalige aanmelding](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image9.png)<br />

   > [!NOTE]
   > Domeinbeheerder-accountreferenties zijn vereist om in te schakelen naadloze eenmalige aanmelding. Het proces is voltooid de volgende acties uit, waarbij deze verhoogde machtigingen. De referenties van het domein Administrator-account worden niet opgeslagen in Azure AD Connect of in Azure AD. De referenties van het domein Administrator-account worden gebruikt alleen voor de functie inschakelen. De referenties worden verwijderd wanneer het proces met succes is voltooid.
   >
   > 1. Een account met de naam AZUREADSSOACC (die staat voor Azure AD) is gemaakt in uw on-premises Active Directory-exemplaar.
   > 2. Het computeraccount Kerberos ontsleutelingssleutel wordt veilig worden gedeeld met Azure AD.
   > 3. Er zijn twee Kerberos-SPN-namen (SPN's) gemaakt voor twee URL's die worden gebruikt tijdens de aanmelding bij Azure AD.

6. Op de **klaar om te configureren** pagina, zorg ervoor dat de **Start het synchronisatieproces wanneer de configuratie is voltooid** selectievakje is ingeschakeld. Selecteer **configureren**.

      ![Schermafbeelding van de pagina gereed voor configuratie](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image10.png)<br />

   > [!IMPORTANT]
   > Op dit moment wordt uw federatieve domeinen gewijzigd in beheerde verificatie. Wachtwoord-hashsynchronisatie is de nieuwe methode voor verificatie.

7. Selecteer in de Azure AD-portal, **Azure Active Directory** > **Azure AD Connect**.
8. Controleer of deze instellingen:
   * **Federatieve** is ingesteld op **uitgeschakelde**.
   * **Naadloze eenmalige aanmelding** is ingesteld op **ingeschakeld**.
   * **Wachtwoordsynchronisatie** is ingesteld op **ingeschakeld**.<br /> 

   ![Schermafbeelding van de instellingen in de sectie van de aanmelding gebruiker ](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image11.png)<br />

Gaat u naar [testen en de volgende stappen](#testing-and-next-steps).

   > [!IMPORTANT]
   > De sectie overslaan **optie B: Overschakelen van Federatie naar wachtwoord-hashsynchronisatie met behulp van Azure AD Connect en PowerShell**. De stappen in deze sectie niet van toepassing als u een naadloze eenmalige aanmelding te wijzigen in de aanmeldingsmethode wachtwoord-hashsynchronisatie optie hebt gekozen.

#### <a name="option-b-switch-from-federation-to-password-hash-synchronization-using-azure-ad-connect-and-powershell"></a>Optie B: Overschakelen van Federatie naar wachtwoord-hashsynchronisatie met Azure AD Connect en PowerShell

Gebruik deze optie als u niet uw federatieve domeinen in eerste instantie geconfigureerd met behulp van Azure AD Connect. Tijdens dit proces inschakelen u naadloze eenmalige aanmelding en uw domeinen uit die zijn gefedereerd met beheerde-switch.

1. Open op de Azure AD Connect-server, de Azure AD Connect-wizard.
2. Selecteer **aanmelden van gebruikers wijzigen**, en selecteer vervolgens **volgende**.
3. Op de **verbinding maken met Azure AD** pagina, de gebruikersnaam en wachtwoord invoeren voor een globale beheerdersaccount.
4. Op de **aanmelden van gebruikers** weergeeft, schakelt de **wachtwoord-hashsynchronisatie** knop. Selecteer **eenmalige aanmelding inschakelen**, en selecteer vervolgens **volgende**.

   Voordat u wachtwoord-hashsynchronisatie inschakelen: ![Schermafbeelding van de komen optie op de aanmeldingspagina van gebruiker niet configureren](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image12.png)<br />

   Nadat u synchronisatie van wachtwoordhashes inschakelen: ![Schermafbeelding van nieuwe opties op de aanmeldingspagina van gebruiker ](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image13.png)<br />
   
   > [!NOTE]
   > Beginnen met Azure AD Connect versie 1.1.880.0, de **naadloze eenmalige aanmelding** selectievakje is standaard geselecteerd.

5. Op de **eenmalige aanmelding inschakelen** pagina, typ de referenties voor een domein Administrator-account en selecteer vervolgens **volgende**.

   > [!NOTE]
   > Domeinbeheerder-accountreferenties zijn vereist om in te schakelen naadloze eenmalige aanmelding. Het proces is voltooid de volgende acties uit, waarbij deze verhoogde machtigingen. De referenties van het domein Administrator-account worden niet opgeslagen in Azure AD Connect of in Azure AD. De referenties van het domein Administrator-account worden gebruikt alleen voor de functie inschakelen. De referenties worden verwijderd wanneer het proces met succes is voltooid.
   >
   > 1. Een account met de naam AZUREADSSOACC (die staat voor Azure AD) is gemaakt in uw on-premises Active Directory-exemplaar.
   > 2. Het computeraccount Kerberos ontsleutelingssleutel wordt veilig worden gedeeld met Azure AD.
   > 3. Er zijn twee Kerberos-SPN-namen (SPN's) gemaakt voor twee URL's die worden gebruikt tijdens de aanmelding bij Azure AD.

6. Op de **klaar om te configureren** pagina, zorg ervoor dat de **Start het synchronisatieproces wanneer de configuratie is voltooid** selectievakje is ingeschakeld. Selecteer **configureren**.

   ![Schermafbeelding met de knop configureren op de pagina gereed voor configuratie](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image15.png)<br />
   Wanneer u selecteert de **configureren** knop, naadloze eenmalige aanmelding is geconfigureerd zoals wordt aangegeven in de vorige stap. Configuratie van wachtwoord-hash-synchronisatie wordt niet gewijzigd omdat deze eerder is ingeschakeld.

   > [!IMPORTANT]
   > Er worden geen wijzigingen aangebracht in de manier waarop gebruikers zich aanmelden op dit moment.

7. Controleer of deze instellingen in de Azure AD-portal:
   * **Federatieve** is ingesteld op **ingeschakeld**.
   * **Naadloze eenmalige aanmelding** is ingesteld op **ingeschakeld**.
   * **Wachtwoordsynchronisatie** is ingesteld op **ingeschakeld**.

   ![Schermafbeelding van de instellingen in de sectie van de aanmelding gebruiker](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image16.png)

#### <a name="convert-domains-from-federated-to-managed"></a>Converteren van domeinen uit die zijn gefedereerd met het beheerd

Federatie is op dit moment nog steeds ingeschakeld en voor uw domeinen. Als u wilt doorgaan met de implementatie, moet elk domein moet worden geconverteerd van die zijn gefedereerd met beheerde om af te dwingen van gebruikersverificatie via wachtwoord-hashsynchronisatie.

> [!IMPORTANT]
> U hoeft niet te converteren van alle domeinen op hetzelfde moment. U kunt selecteren om te beginnen met een testdomein op uw productietenant of beginnen met het domein met het laagste aantal gebruikers.

De conversie is voltooid met behulp van de Azure AD PowerShell-module:

1. In PowerShell, aanmelden bij Azure AD met behulp van een globale beheerdersaccount.
2. Als u wilt converteren van het eerste domein, moet u de volgende opdracht uitvoeren:

   ``` PowerShell
   Set-MsolDomainAuthentication -Authentication Managed -DomainName <domain name>
   ```

3. Selecteer in de Azure AD-portal, **Azure Active Directory** > **Azure AD Connect**.
4. Controleer of dat het domein is geconverteerd naar beheerd door de volgende opdracht uit:

   ``` PowerShell
   Get-MsolDomain -DomainName <domain name>
   ```

## <a name="testing-and-next-steps"></a>Testen en de volgende stappen

Voer de volgende taken om te controleren of de wachtwoord-hashsynchronisatie en om te voltooien tijdens de conversie.

### <a name="test-authentication-by-using-password-hash-synchronization"></a>Testen van authenticatie met behulp van wachtwoord-hashsynchronisatie 

Wanneer u uw tenant federatieve identiteit gebruikt, zijn gebruikers van de Azure AD-aanmeldingspagina omgeleid naar uw AD FS-omgeving. Nu dat de tenant is geconfigureerd voor het gebruik van wachtwoord-hashsynchronisatie in plaats van federatieve verificatie, worden gebruikers worden niet omgeleid naar AD FS. In plaats daarvan zich gebruikers aanmelden rechtstreeks op de aanmeldingspagina van Azure AD.

Wachtwoord-hashsynchronisatie testen:

1. Open Internet Explorer in InPrivate-modus zodat naadloze eenmalige aanmelding niet je automatisch aanmelden.
2. Ga naar de aanmeldingspagina van Office 365 ([http://portal.office.com](http://portal.office.com/)).
3. Voer de UPN van een gebruiker en selecteer vervolgens **volgende**. Zorg ervoor dat u de UPN van een hybride-gebruiker die is gesynchroniseerd vanuit uw on-premises Active Directory-exemplaar en die eerder federatieve verificatie gebruikt. Er verschijnt een pagina waarop u de gebruikersnaam en wachtwoord invoeren:

   ![Schermafbeelding van de aanmeldingspagina-opgeven waarin u een gebruikersnaam invoeren](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image18.png)

   ![Schermafbeelding van de aanmeldingspagina-opgeven waarin u een wachtwoord invoeren](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image19.png)

4. Nadat u het wachtwoord invoeren en selecteer **aanmelden**, wordt u doorgestuurd naar de Office 365-portal.

   ![Schermafbeelding van de Office 365-portal](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image20.png)


### <a name="test-seamless-sso"></a>Naadloze eenmalige aanmelding testen

1. Meld u aan een domein machine die is verbonden met het bedrijfsnetwerk bevinden.
2. In Internet Explorer of Chrome, gaat u naar een van de volgende URL's (Vervang ' contoso' met uw domein):

   * https:\/\/myapps.microsoft.com/contoso.com
   * https:\/\/myapps.microsoft.com/contoso.onmicrosoft.com

   De gebruiker wordt kort omgeleid naar de Azure AD-aanmeldingspagina, waarin het bericht "Het u aanmelden." De gebruiker wordt niet gevraagd een gebruikersnaam of wachtwoord.<br />

   ![Schermafbeelding van de aanmeldingspagina van Azure AD en het bericht](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image21.png)<br />
3. De gebruiker wordt omgeleid en is aangemeld bij het toegangsvenster:

   > [!NOTE]
   > Naadloze eenmalige aanmelding werkt op Office 365-services die ondersteuning bieden voor geheugensteun voor het domein (bijvoorbeeld myapps.microsoft.com/contoso.com). De Office 365-beheerportal (portal.office.com) ondersteunt op dit moment geen domein hints. Gebruikers moeten een UPN invoeren. Nadat een UPN die is opgegeven, wordt het Kerberos-ticket namens de gebruiker opgehaald in naadloze eenmalige aanmelding. De gebruiker is aangemeld zonder een wachtwoord in te voeren.

   > [!TIP]
   > Overweeg de implementatie van [hybride Azure AD join op Windows 10](https://docs.microsoft.com/azure/active-directory/device-management-introduction) voor een verbeterde ervaring voor eenmalige aanmelding.

### <a name="remove-the-relying-party-trust"></a>Verwijderen van de relying party trust

Nadat u valideert dat alle gebruikers en -clients is tijdens de verificatie via Azure AD, is het veilig verwijderen van de Office 365 relying party trust.

Als u AD FS niet voor andere doeleinden gebruikt (dat wil zeggen, voor andere vertrouwensrelaties met relying party's), is het veilig om op dit moment uit bedrijf nemen AD FS.

### <a name="rollback"></a>Ongedaan maken

Als u een groot probleem detecteren en deze snel niet kan oplossen, kunt u de oplossing terugkeren naar de federation.

Raadpleeg de federation-ontwerp en implementatie-documentatie voor de details van uw specifieke implementatie. Het proces moet deze taken omvatten:

* Beheerde domeinen converteren naar federatieve verificatie met behulp van de **Convert-MSOLDomainToFederated** cmdlet.
* Configureer, indien nodig aanvullende claimregels.

### <a name="sync-userprincipalname-updates"></a>UserPrincipalName-updates synchroniseren

In het verleden zijn bijgewerkt naar de **UserPrincipalName** kenmerk, die gebruikmaakt van de synchronisatieservice uit de on-premises-omgeving, worden geblokkeerd, tenzij beide volgende voorwaarden wordt voldaan:

* De gebruiker zich in een beheerde identiteit (niet-gefedereerde)-domein.
* De gebruiker is niet een licentie toegewezen.

Zie voor informatie over het controleren of deze functie inschakelt, [userPrincipalName updates synchroniseren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsyncservice-features).

### <a name="troubleshooting"></a>Problemen oplossen

Het ondersteuningsteam moet weten hoe u het oplossen van verificatieproblemen die zich voordoen tijdens of na de wijziging van Federatie beheerd. Gebruik de volgende documentatie voor het oplossen van problemen voor het ondersteuningsteam raken met de algemene stappen voor probleemoplossing en de bijbehorende acties die helpen kunnen om te isoleren en los het probleem.

[Azure Active Directory-wachtwoord-hashsynchronisatie oplossen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization)

[Azure Active Directory naadloze eenmalige aanmelding oplossen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sso)

## <a name="roll-over-the-seamless-sso-kerberos-decryption-key"></a>Beweeg de muis over de naadloze eenmalige aanmelding Kerberos-ontsleutelingssleutel

Het is belangrijk om u te vaak plaats de muisaanwijzer op de ontsleutelingssleutel Kerberos van het account van de AZUREADSSOACC computer (die staat voor Azure AD). Het computeraccount AZUREADSSOACC wordt gemaakt in uw on-premises Active Directory-forest. Het is raadzaam dat u de ontsleutelingssleutel Kerberos ten minste elke 30 dagen meegenomen om te worden uitgelijnd met de manier waarop dat leden van een domein Active Directory wachtwoord te wijzigen indienen. Er is geen gekoppelde apparaat dat is gekoppeld aan het computeraccountobject AZUREADSSOACC, dus moet u handmatig de rollover uitvoeren.

Start de rollover van de naadloze eenmalige aanmelding Kerberos ontsleutelingssleutel op de on-premises server waarop Azure AD Connect wordt uitgevoerd.

Zie voor meer informatie, [hoe ik de muisaanwijzer op de ontsleutelingssleutel Kerberos van het account van de computer AZUREADSSOACC?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [ontwerpconcepten Azure AD Connect](plan-connect-design-concepts.md).
* Kies de [juiste verificatie](https://docs.microsoft.com/azure/security/azure-ad-choose-authn).
* Meer informatie over [topologieën ondersteund](plan-connect-design-concepts.md).
