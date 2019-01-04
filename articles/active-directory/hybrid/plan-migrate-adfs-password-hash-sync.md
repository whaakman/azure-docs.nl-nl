---
title: 'Azure AD Connect: Migreren van Federatie naar wachtwoord-hashsynchronisatie voor Azure AD | Microsoft Docs'
description: Informatie over het verplaatsen van uw hybride identiteit omgeving van Federatie naar wachtwoord-hashsynchronisatie.
services: active-directory
author: billmath
manager: mtillman
ms.reviewer: martincoetzer
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/13/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: cad368cb968b94d1327cc99ed4dfa6df0aedd2cd
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555095"
---
# <a name="migrate-from-federation-to-password-hash-synchronization-for-azure-ad"></a>Migreren van Federatie naar wachtwoord-hashsynchronisatie voor Azure AD
Het volgende document bevat richtlijnen over het verplaatsen van AD FS naar wachtwoord-hashsynchronisatie.

>[!NOTE]
>Een downloadbare een kopie van dit document vindt u [hier](https://aka.ms/ADFSTOPHSDPDownload).


## <a name="prerequisites-for-the-migration"></a>Vereisten voor de migratie 
De volgende vereisten zijn vereist voordat u kunt migreren.
### <a name="update-azure-ad-connect"></a>Update van Azure AD Connect

Als een minimum uit te voeren de stappen voor het migreren naar pass-through-verificatie, moet u hebben [Azure AD connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0. Deze versie bevat belangrijke wijzigingen aangebracht in de manier waarop aanmelden conversie wordt uitgevoerd en vermindert de totale tijd voor het migreren van Federatie naar Cloud-verificatie van mogelijk uren en minuten.

> [!IMPORTANT]
> Verouderde documentatie, hulpprogramma's en blogs aangeven dat de gebruikersconversie een vereiste stap is bij het converteren van domeinen van federatieve naar beheerde. Houd er rekening mee dat gebruikers converteren niet meer hoeft en Microsoft is bezig met bijwerken van de documentatie en hulpprogramma's om dit weer te geven.

Azure AD Connect bijwerken naar de nieuwste versie, volg deze [instructies update](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

### <a name="password-hash-synchronization-required-permissions"></a>Wachtwoord-hash-synchronisatie vereist machtigingen

Azure AD Connect kan worden geconfigureerd met behulp van de Express-instellingen of aangepaste installatie. Als u de optie aangepaste installatie gebruikt de [vereiste machtigingen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-accounts-permissions) voor wachtwoord-Hashsynchronisatie mogelijk niet aanwezig.

De Azure AD Connect AD DS-service account moet de volgende machtigingen voor het synchroniseren van wachtwoord-hashes.

* Directorywijzigingen repliceren

* Alle repliceren Directory gewijzigd

Nu is een goed moment om te valideren dat deze machtigingen zijn ingesteld voor alle domeinen in het forest.

### <a name="plan-migration-method"></a>Migratiemethode plannen

Er zijn twee methoden voor het migreren van federatieve verificatie naar wachtwoord-Hashsynchronisatie en naadloze eenmalige aanmelding. De methode die u gebruikt afhankelijk van hoe uw AD FS voor het oorspronkelijk is geconfigureerd. 



- **Optie A: Azure AD Connect gebruiken**. Als u AD FS oorspronkelijk is geconfigureerd met behulp van Azure AD Connect, moet de wijziging van synchronisatie van Wachtwoordhashes als de gebruiker aanmelden methode worden uitgevoerd via de Azure AD Connect-wizard.   
Wanneer u Azure AD Connect gebruikt, wordt de cmdlet Set-MsolDomainAuthentication voor u wordt automatisch uitgevoerd wanneer u de gebruiker aanmelden methode wijzigen en daarom hebt u geen controle over het unfederating alle van de gecontroleerde federatieve domeinen in uw Azure AD-tenant.

> [!NOTE]
> Op dit moment kan niet worden vermeden unfederating alle domeinen in uw tenant wanneer u wijzigt het aanmelden van gebruikers in de wachtwoord-Hashsynchronisatie wanneer AAD Connect oorspronkelijk werd gebruikt voor het configureren van AD FS voor u.  



- **Optie B: Azure AD Connect gebruiken met PowerShell**. Deze methode kan worden gebruikt alleen wanneer de AD FS oorspronkelijk niet is geconfigureerd met Azure AD Connect. U moet nog steeds wijzigen van de gebruiker aanmelden methode via de Azure AD Connect-wizard, maar het belangrijkste verschil is dat deze wordt niet automatisch uitgevoerd de cmdlet Set-MsolDomainAuthentication voor u als er geen kennis van uw AD FS-farm, en daarom hebt u volledige controle over welke domeinen worden geconverteerd en in welke volgorde.

Om te begrijpen welke methode u moet gebruiken, moet u de stappen uitvoeren op de volgende sectie.

#### <a name="verify-current-user-sign-in-settings"></a>Controleer of de huidige gebruiker aanmelden instellingen

Controleer of de huidige gebruiker aanmelden instellingen door aan te melden bij de Azure AD-beheerportal [ https://aad.portal.azure.com ](https://aad.portal.azure.com/) met een globale beheerdersaccount.

Controleer of dat Federation is ingeschakeld en dat naadloze eenmalige aanmelding en Pass through-verificatie zijn uitgeschakeld in de sectie gebruiker aanmelden. Controleer ook of deze staat van Wachtwoordsynchronisatie die moet worden weergegeven als uitgeschakeld, tenzij dit eerder is ingeschakeld.

![Afbeelding 5](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image1.png)

#### <a name="verify-azure-ad-connect-configuration"></a>Azure AD Connect-configuratie controleren

   1. Ga naar uw Azure AD Connect-server en Azure AD Connect starten, en selecteer vervolgens configureren. 
   2. Selecteer de huidige configuratie weergeven op het scherm Extra taken en selecteer volgende.</br>
   ![Afbeelding 31](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image2.png)</br>
   
   3. In het scherm van uw oplossing controleren, noteer de synchronisatie van wachtwoord-status.</br> 

   Als u wachtwoord-Hashsynchronisatie is momenteel ingesteld op uitgeschakeld, moet u de stappen in deze handleiding te kunnen. Als u wachtwoord-Hashsynchronisatie is momenteel ingesteld op ingeschakeld, kunt u veilig de sectie overslaan [stap 1: synchronisatie van Wachtwoordhashes inschakelen](#step-1--enable-password-hash-synchronization) in deze handleiding.
   4. Schuif omlaag naar Active Directory Federation Services (AD FS) in het scherm voor uw oplossing controleren.</br>
 
   Als u ziet dat de AD FS-configuratie in deze sectie is en u veilig wordt ervan uitgegaan dat kunt AD FS oorspronkelijk is geconfigureerd met Azure AD Connect en kan daarom de conversie van uw domeinen uit die zijn gefedereerd met beheerde worden bepaald via de Azure AD Connect 'wijziging gebruiker aanmelden -in ' optie, dit proces wordt beschreven in de sectie **optie A - Switch van Federatie naar wachtwoord-Hashsynchronisatie met behulp van Azure AD Connect**.
   5. Als u Active Directory Federation Services die worden vermeld in de huidige instellingen niet ziet, dan u handmatig omzetten in de domeinen die zijn gefedereerd moet met beheerd via PowerShell, waarmee wordt beschreven in de sectie **optie B - Switch van Federatie naar Wachtwoord-Hashsynchronisatie met Azure AD Connect en PowerShell**.

### <a name="document-current-federation-settings"></a>Document huidige federatie-instellingen

U kunt de huidige Federatie-instelling vinden door de cmdlet Get-MsolDomainFederationSettings.

De opdracht is:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName YourDomain.extention | fl *
```

Bijvoorbeeld:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName Contoso.com | fl *
```
Alle instellingen die mogelijk zijn aangepast aan de Federation-documentatie voor ontwerp en implementatie, specifiek PreferredAuthenticationProtocol, SupportsMfa en PromptLoginBehavior valideren.

Meer informatie over wat deze instellingen u hieronder vindt.

[Active Directory Federation Services vragen = aanmelding parameter ondersteuning](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)  
‎[Set-MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> Als de waarde SupportsMfa is momenteel ingesteld op 'True' vervolgens betekent dit dat u een On-Premises MFA-oplossing gebruikt voor het invoeren van een 2e factor uitdaging in de gebruikersstroom voor verificatie. Dit werkt niet meer voor scenario's Azure AD-verificatie, en u moet in plaats daarvan gebruikmaken van de service Azure MFA (cloud-indeling) om uit te voeren dezelfde functie. Zorgvuldig uw MFA-vereisten evalueren voordat u verder gaat en zorg ervoor dat u begrijpt hoe u Azure MFA, de gevolgen voor de licenties en het registratieproces voor de eindgebruiker voordat u converteert uw domeinen. Onze Implementatiehandleiding voor Azure MFA die dieper kan worden gevonden op [ https://aka.ms/deploymentplans ](https://aka.ms/deploymentplans).

#### <a name="backup-federation-settings"></a>Back-federatie-instellingen

Hoewel er geen wijzigingen worden aangebracht aan andere Relying Party's op uw AD FS-farm tijdens dit proces, is het raadzaam om ervoor te zorgen dat u hebt een huidige geldige back-up van uw AD FS-farm die kan worden hersteld. U kunt dit doen met behulp van de gratis Microsoft [AD FS Rapid Restore-hulpprogramma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool). Dit hulpprogramma kan worden gebruikt voor back-up en herstellen van de AD FS, toekennen aan een bestaande farm of een nieuwe farm.

Als u ervoor kiest om niet te gebruiken van de AD FS Rapid Restore-hulpprogramma, moet klikt u vervolgens ten minste, u exporteren 'Microsoft Office 365 Identity Platform"relying party trust en de bijbehorende aangepaste claimregels u hebt toegevoegd. U kunt dit doen via de volgende PowerShell-voorbeeld

``` PowerShell
(Get-AdfsRelyingPartyTrust -Name "Microsoft Office 365 Identity Platform") | Export-CliXML "C:\temp\O365-RelyingPartyTrust.xml"
```

## <a name="deployment-considerations-and-ad-fs-usage"></a>Overwegingen voor de implementatie en het gebruik van de AD FS

### <a name="validate-your-current-ad-fs-usage"></a>Valideren van het gebruik van uw huidige AD FS

Voordat u federatieve converteert naar beheerd, moet u nauw zoeken op hoe u AD FS momenteel voor Azure AD/Office 365 en andere toepassingen gebruiken (relying party's). Specifiek, moet u overwegen de volgende tabel:

| Als| vervolgens |
|-|-|
| U wilt behouden van de AD FS voor deze andere toepassingen.| U gaat gebruiken zowel AD FS en Azure AD en moet rekening houden met de eindgebruikerservaring als gevolg hiervan. Gebruikers mogelijk om te verifiëren tweemaal in sommige scenario's, één keer naar Azure AD (waar ze krijgen eenmalige aanmelding en hoger voor andere toepassingen zoals Office 365) en opnieuw voor toepassingen nog steeds gekoppeld aan de AD FS als een relying party. |
| AD FS is sterk gepersonaliseerde en vertrouwen op specifieke aanpassingsinstellingen in het bestand OnLoad.js die in Azure AD kan niet worden gedupliceerd (bijvoorbeeld, u hebt gewijzigd de aanmeldingsprocedure zodat gebruikers alleen SamAccountName-indeling voor hun gebruikersnaam invoeren dus aan een UPN of hebben een sterk merknaam de aanmeldingsprocedure)| U moet controleren dat de aanpassingsvereisten van uw huidige kunnen worden voldaan door Azure AD voordat u doorgaat. Raadpleeg de huisstijl van AD FS en AD FS-aanpassing secties voor meer informatie over en richtlijnen.|
| U blokkeren oudere verificatieclients via AD FS.| Houd rekening met het vervangen van de besturingselementen voor het blokkeren van oudere verificatieclients die momenteel aanwezig zijn op de AD FS met een combinatie van [voorwaardelijke toegang voor oudere verificatie besturingselementen](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) en [Exchange Online Client Access Regels](http://aka.ms/EXOCAR).|
| U vereisen dat gebruikers MFA tegen een on-premises MFA-server-oplossing uitvoeren bij het verifiëren van de AD FS.| Kunt u zich niet aan het invoeren van een MFA-controle via de on-premises MFA-oplossing in de authenticatiestroom voor een beheerd domein, maar u de Azure MFA-service gebruiken kunt om dit te doen voortaan zodra het domein wordt geconverteerd. Als gebruikers Azure MFA niet momenteel gebruiken, wordt dit een eenmalige eindgebruikers registratiestap die u moet voorbereiden en communiceren aan uw eindgebruikers inhouden.|
| U beleid voor toegangsbeheer (AuthZ-regels) momenteel gebruikt in AD FS voor het beheren van toegang tot Office 365.| Houd rekening met het vervangen van deze met de equivalente Azure AD [beleid voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) en [toegangsregels voor Exchange Online clients](http://aka.ms/EXOCAR).|

### <a name="considerations-for-common-ad-fs-customizations"></a>Overwegingen voor de algemene AD FS-aanpassingen

#### <a name="inside-corporate-network-claim"></a>Binnen bedrijfsnetwerk

De claim InsideCorporateNetwork is uitgegeven door AD FS als de gebruiker die heeft geverifieerd binnen het bedrijfsnetwerk is. Deze claim kan vervolgens worden doorgegeven aan Azure AD en gebruikt om meervoudige verificatie op basis van netwerklocatie van de gebruikers over te slaan. Zie [goedgekeurde IP-adressen voor federatieve gebruikers](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud) voor meer informatie over het bepalen of u dit is momenteel ingeschakeld in AD FS hebt.

De claim InsideCorporateNetwork zijn niet beschikbaar meer zodra uw domeinen worden geconverteerd naar wachtwoord-Hashsynchronisatie. [Benoemde locaties in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) kan worden gebruikt ter vervanging van deze functionaliteit.

Als met de naam locaties zijn geconfigureerd, alle beleidsregels voor voorwaardelijke toegang geconfigureerd wilt opnemen of uitsluiten van de netwerklocaties 'Alle vertrouwde locaties' of 'MFA goedgekeurde IP-adressen' moet worden bijgewerkt om de zojuist gemaakte benoemde locaties weer te geven.

Zie [Active Directory voor voorwaardelijke toegang tot locaties](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations) voor meer informatie over de locatie-voorwaarde voor de voorwaardelijke toegang.

#### <a name="hybrid-azure-ad-joined-devices"></a>Hybride Azure AD verbonden apparaten

Een apparaat toevoegen aan Azure AD kunt u regels voor voorwaardelijke toegang die afdwingen van apparaten die voldoen aan uw standaarden voor beveiliging en naleving maken en Hiermee kunnen gebruikers zich aanmeldt bij een apparaat met behulp van een organisatie-werk- of schoolaccount in plaats van een persoonlijke -account. Hybride Azure AD met toegevoegde apparaten kunt u uw AD-domein gekoppelde apparaten toevoegen aan Azure AD. Uw federatieve omgeving is mogelijk geconfigureerd met deze functie.

Om ervoor te zorgen dat voor nieuwe apparaten toegevoegd aan het domein zodra uw domeinen zijn geconverteerd naar wachtwoord-Hashsynchronisatie hybride Join blijft, moet Azure AD Connect worden geconfigureerd voor Active Directory-computeraccounts voor Windows 10-clients om te synchroniseren Azure AD. Naadloze eenmalige aanmelding hybride Join wordt gebruikt voor het registreren van de computer in Azure AD voor Windows 7 en Windows 8-computeraccounts, en u hoeft niet te synchroniseren ze zoals u dat wel voor Windows 10-apparaten doet. U wordt wel om een bijgewerkte workplacejoin.exe-bestand (via een MSI-bestand) te implementeren op deze clients met oudere versies, zodat ze zelf via naadloze eenmalige aanmelding kunnen registreren. [Download het MSI-bestand](https://www.microsoft.com/download/details.aspx?id=53554). 

Raadpleeg voor meer informatie, [hoe het configureren van hybride Azure Active Directory apparaten gekoppelde](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup).

#### <a name="branding"></a>Huisstijl

Uw organisatie mogelijk [aangepast van uw AD FS sign-in's](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) om weer te geven informatie meer relevant zijn voor de organisatie. Als dit het geval is, kunt u soortgelijke [aanpassingen van de Azure AD-aanmeldingspagina](https://docs.microsoft.com/azure/active-directory/customize-branding).

Terwijl vergelijkbare aanpassingen die beschikbaar zijn, worden sommige visuele wijzigingen verwacht. Mogelijk wilt de verwachte wijzigingen in uw communicatie met eindgebruikers opnemen.

> [!NOTE]
> Aangepaste huisstijl is alleen beschikbaar als u de Premium- of Basic-licentie worden aangeschaft voor Azure AD of Office 365-licentie hebt.

## <a name="planning-deployment-and-support"></a>Planning, implementatie en ondersteuning

### <a name="plan-the-maintenance-window"></a>Het onderhoudsvenster te plannen

Tijdens de conversie van het domein zelf relatief snel is, Azure AD kan nog steeds bepaalde verificatieaanvragen verzenden naar uw AD FS-servers gedurende een periode van vier uur nadat de domein-conversie is voltooid. Tijdens deze vier uur, en afhankelijk van verschillende caches op de service, deze verificaties niet kunnen worden geaccepteerd door Azure AD en gebruikers, ontvangt een foutbericht wanneer ze zich op basis van AD FS nog steeds worden geverifieerd, maar Azure AD worden niet meer geaccepteerd een token van de gebruiker krijgen die federatieve vertrouwensrelatie is nu verwijderd.

> [!NOTE]
> Dit wordt alleen invloed-gebruikers die toegang hebben tot de services via een browser tijdens deze conversie post totdat de cache op de service is uitgeschakeld. Verouderde clients (Exchange ActiveSync, Outlook 2010-2013) moeten niet worden beïnvloed als Exchange Online een cache van hun referenties voor een bepaalde periode die wordt gebruikt houdt voor het opnieuw verifiëren van de gebruiker op de achtergrond zonder te hoeven gaat u terug naar AD FS. Referenties die zijn opgeslagen op het apparaat voor deze clients worden gebruikt om zich opnieuw verifiëren zelf op de achtergrond zodra deze opgeslagen in de cache is uitgeschakeld en kan daarom gebruikers een wachtwoord wordt gevraagd als gevolg van het conversieproces domein niet krijgen. Daarentegen voor moderne verificatieclients (Office 2013/2016, IOS en Android-Apps) deze een geldige Refresh Token gebruiken om te verkrijgen van nieuwe toegangstokens voor blijvende toegang tot resources in plaats van de terug te gaan naar AD FS, en die dus immuun voor een wachtwoord wordt gevraagd als een resultaat van de domein-conversie verwerken en blijft functioneren zonder extra configuratie vereist.

> [!IMPORTANT]
> Niet afsluiten van uw AD FS-omgeving of verwijderen van de Office 365-vertrouwensrelatie van relying party totdat u alle gebruikers met succes worden geverifieerd met behulp van cloud-verificatie hebt geverifieerd.

### <a name="plan-for-rollback"></a>Plan voor terugdraaien

Als er een groot probleem is gevonden en kan niet snel worden opgelost, kunt u besluiten om terug te draaien back-de oplossing op Federation. Het is belangrijk om te plannen wat te doen als uw distributie niet gaat u als gepland. Als de conversie van het domein of de gebruikers is mislukt tijdens de implementatie of als u terugdraaien naar federation wilt, moet u het beperken van een storing voordoet en de de gevolgen voor uw gebruikers begrijpen.

#### <a name="rolling-back"></a>Terugdraaien

Raadpleeg de Federation-documentatie voor ontwerp en implementatie voor uw specifieke implementatie-informatie. Het beste overleggen met het proces:

* Beheerde domeinen wilt federeren met behulp van Convert-MSOLDomainToFederated converteren 

* Indien nodig, worden er regels van aanvullende configureren.

### <a name="plan-change-communications"></a>Wijzig de communicatie plannen

Een belangrijk onderdeel van de planning, implementatie en ondersteuning is ervoor te zorgen dat uw eindgebruikers zijn proactief op de hoogte van de wijzigingen en wat ze optreden kunnen of moeten doen. 

Nadat de beide wachtwoord-Hashsynchronisatie en naadloze eenmalige aanmelding zijn geïmplementeerd, verandert de eindgebruikerservaring voor aanmelden bij het verkrijgen van toegang tot Office 365 en andere gekoppelde resources via Azure AD zijn geverifieerd. Gebruikers van buiten het netwerk ziet nu de Azure AD-aanmelding op de pagina, in plaats van wordt omgeleid naar de pagina op basis van formulieren gepresenteerd door de externe tegenoverliggende Web Application Proxy-servers.

Er zijn meerdere elementen op de planning van uw Communicatiestrategie voor. Deze omvatten:

* Gebruikers van de toekomstige en vrijgegeven functionaliteit via de hoogte te brengen
  * E-mail en andere interne communicatiekanalen
  * Visuele elementen zoals posters
  * Live of andere communicatie
* Bepalen wie gaat aanpassen en die de communicatie wordt verzonden en wanneer.

## <a name="implementing-your-solution"></a>Uw oplossing implementeren

Nu dat u uw oplossing hebt gepland, bent u klaar om dit te implementeren. Implementatie omvat de volgende onderdelen:

1. Wachtwoord-hashsynchronisatie inschakelen

2. Voorbereiden voor naadloze eenmalige aanmelding op

3. Aanmeldingsmethode wijzigen naar wachtwoord-Hashsynchronisatie en naadloze eenmalige aanmelding inschakelen

### <a name="step-1--enable-password-hash-synchronization"></a>Stap 1: synchronisatie van wachtwoordhashes inschakelen

De eerste stap bij het implementeren van deze oplossing is het wachtwoord-Hashsynchronisatie inschakelen in de Azure AD Connect-wizard. Wachtwoord-Hashsynchronisatie is een optionele functie die kan worden ingeschakeld voor omgevingen met behulp van federatieve zonder enige gevolgen voor de verificatie-stroom. In dit geval Azure AD Connect wordt gestart voor het synchroniseren van wachtwoord-hashes zonder gevolgen voor gebruikers aanmelden met behulp van de Federatie.

Om deze reden is het raadzaam deze stap uitvoeren als een voorbereidingstaak goed voordat u wijzigt de aanmeldingsmethode domeinen. Hiermee beschikt u over voldoende tijd voor het valideren van wachtwoord-Hashsynchronisatie correct werkt.

Wachtwoord-Hashsynchronisatie inschakelen:

   1. Op de Azure AD Connect-Server, opent u de wizard en selecteert u configureren.
   2. Selecteer de synchronisatieopties aanpassen en selecteer volgende.
   3. Geef de gebruikersnaam en het wachtwoord van een globale beheerder in het scherm verbinding maken met Azure AD.
   4. Klik op volgende bij het verbinden van uw scherm mappen.
   5. Klik op volgende in het domein en OE filteren scherm.
   6. In het scherm optionele functies Wachtwoordsynchronisatie selecteren en selecteer je volgende.
   ![Afbeelding 21](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image6.png)</br>
   7. Selecteer vervolgens in alle resterende schermen en configureren op het laatste scherm.
   8. Azure AD Connect wordt gestart met het synchroniseren van wachtwoord-hashes in de volgende synchronisatie.

Zodra de wachtwoord-Hashsynchronisatie is ingeschakeld, de wachtwoord-hashes voor alle gebruikers in Azure AD Connect synchronisatie bereik worden nieuwe en geschreven naar Azure AD. Afhankelijk van het aantal gebruikers kunnen deze bewerking van minuten tot enkele uren.

Voor de planning, moet u schat dat ongeveer 20.000 gebruikers, kunnen worden verwerkt binnen 1 uur.

Voor het valideren van wachtwoord-Hashsynchronisatie correct werkt, gebruikt u de taak voor het oplossen van problemen op de Azure AD Connect-wizard.

   1. Open een nieuwe Windows PowerShell-sessie op uw Azure AD Connect-server met het uitvoeren als beheerder optie.
   2. Set-ExecutionPolicy RemoteSigned of Set-ExecutionPolicy Unrestricted uitvoeren.
   3. Start de Azure AD Connect-wizard.
   4. Navigeer naar de pagina extra taken, selecteert u problemen oplossen en klik op volgende.
   5. Klik op starten voor het oplossen van problemen met het menu start in PowerShell op de pagina voor problemen oplossen.
   6. Selecteer in het hoofdmenu van problemen met wachtwoord-hashsynchronisatie.
   7. Selecteer in het submenu wachtwoord-hashsynchronisatie niet helemaal werkt.

Als u problemen ondervindt, gebruikt u de informatie op [in dit artikel](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization) om op te lossen.

### <a name="step-2--prepare-for-seamless-sso"></a>Stap 2: voorbereiden voor naadloze eenmalige aanmelding

Op uw apparaten naadloze eenmalige aanmelding gebruiken, moet u een Azure AD-URL toevoegen aan zone-instellingen van de gebruikers Intranet met behulp van Groepsbeleid in Active Directory.

Standaard berekent de browser automatisch de juiste zone, Internet- of intranethosts van een specifieke URL. Bijvoorbeeld, "http://contoso/'wordt toegewezen aan de intranetzone, terwijl'http://intranet.contoso.com/' wordt toegewezen aan de zone Internet (omdat de URL een periode bevat). Browsers wordt Kerberos-tickets niet verzenden naar een cloudeindpunt, zoals de URL van de Azure AD, tenzij u expliciet de URL aan de intranetzone van de browser toevoegen.

Ga als volgt de [stappen om te worden uitgerold](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) de benodigde wijzigingen aan uw apparaten.

> [!IMPORTANT]
> Deze wijziging niet de manier waarop uw gebruikers zich aanmelden bij Azure AD worden gewijzigd. Het is echter belangrijk dat deze configuratie wordt toegepast op al uw apparaten voordat u met de stap 3 doorgaat. Let ook op gebruikers die zich op apparaten die deze configuratie niet ontvangen moeten gewoon gebruikersnaam en wachtwoord aanmelden bij Azure AD in te voeren.

### <a name="step-3--change-sign-in-method-to-phs-and-enable-seamless-sso"></a>Stap 3: aanmelden methode wijzigen in WHS en naadloze eenmalige aanmelding inschakelen

#### <a name="option-a---switch-from-federation-to-phs-by-using-azure-ad-connect"></a>Optie A - Switch van Federatie naar WHS met behulp van Azure AD Connect

Gebruik deze methode als uw AD FS is in eerste instantie geconfigureerd met behulp van Azure AD Connect. U kunt deze methode niet gebruiken als uw AD FS oorspronkelijk niet is geconfigureerd met behulp van Azure AD Connect. Eerste **gebruiker aanmelden methode wijzigen**

   1. Op de Azure AD Connect-Server de wizard te openen.
   2. Selecteer aanmelden van gebruikers wijzigen en selecteer volgende.
   ![Afbeelding 27](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image7.png)</br>
   3. In de **verbinding maken met Azure AD** scherm Geef de gebruikersnaam en wachtwoord van een **hoofdbeheerder**.
   4. In de **aanmelden van gebruikers** scherm wijzigen van het keuzerondje van Federatie met AD FS in Hashsynchronisatie doorgeven en zorg ervoor dat u het selectievakje gebruikersaccounts niet converteren als dit een stap is afgeschaft en wordt verwijderd uit toekomstige versies van AAD Connect. Ook eenmalige aanmelding inschakelen selecteren en selecteer vervolgens **volgende**.
   ![Afbeelding 29](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image8.png)</br>
   
   > [!NOTE]
   > Beginnen met Azure AD Connect versie 1.1.880.0, is de naadloze eenmalige aanmelding selectievakje standaard ingeschakeld.
   
   > [!IMPORTANT]
   > U kunt de waarschuwingen die aangeven dat gebruikersconversie veilig negeren en volledige synchronisatie van wachtwoordhashes zijn vereiste stappen voor het converteren van Federatie naar cloud-verificatie. Houd er rekening mee dat deze stappen zijn niet vereist meer, toekomstige versies van Azure AD Connect heeft geen een optie voor het converteren van gebruikers. Als u deze waarschuwingen nog steeds ziet, zijn selectievakje dat u de nieuwste versie van Azure AD Connect en die u uitvoert met behulp van de meest recente versie van deze handleiding. Zie voor meer informatie de [Update van Azure AD Connect-sectie](#_Update_Azure_AD).
   
   5. In het scherm inschakelen Single Sign-on, voert u de referenties van domein Administrator-account en selecteer je volgende.
   ![Afbeelding 35](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image9.png)</br>
   
   > [!NOTE]
   > Domeinbeheerder-referenties zijn vereist voor het inschakelen van naadloze eenmalige aanmelding als het proces voert de volgende acties waarvoor deze verhoogde machtigingen. De referenties voor de domeinbeheerder zijn niet opgeslagen in Azure AD Connect of in Azure AD. Ze alleen gebruikt voor het inschakelen van de functie en vervolgens verwijderd na de bewerking is voltooid
   >  * Een account met de naam AZUREADSSOACC (die staat voor Azure AD) is gemaakt in uw on-premises Active Directory (AD).
   >  * Het computeraccount Kerberos ontsleutelingssleutel wordt veilig worden gedeeld met Azure AD.
   >  * Bovendien zijn twee Kerberos-SPN-namen (SPN's) gemaakt voor twee URL's die worden gebruikt tijdens de aanmelding bij Azure AD.
   >  * De referenties voor de domeinbeheerder zijn niet opgeslagen in Azure AD Connect of in Azure AD. Ze alleen gebruikt voor het inschakelen van de functie en vervolgens verwijderd na de bewerking is voltooid
   
   6. Zorg ervoor dat in de Ready to Configure scherm, 'Start synchronisatieproces wanneer de configuratie is voltooid' selectievakje is ingeschakeld. Selecteer vervolgens configureren.
   ![Afbeelding 36](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image10.png)</br>
   
   > [!IMPORTANT]
   > Alle federatieve domeinen wordt op dit moment worden gewijzigd in beheerde verificatie zal nu gebruikmaken van wachtwoord-Hashsynchronisatie als de methode voor verificatie.
       
   7. Open de Azure AD-portal, selecteert u Azure Active Directory en selecteer vervolgens Azure AD Connect.
   8. Controleer of u Federation tijdens naadloze eenmalige aanmelding is uitgeschakeld en Password Sync zijn ingeschakeld.  
  ![Afbeelding 37](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image11.png)</br>
   9. Ga naar [testen en de volgende stappen](#testing-and-next-steps).
   
   > [!IMPORTANT]
   > Overslaan de sectie optie B - Switch van Federatie naar wachtwoord-Hashsynchronisatie met Azure AD Connect en PowerShell als de stappen in deze sectie niet van toepassing.  

#### <a name="option-b---switch-from-federation-to-phs-using-azure-ad-connect-and-powershell"></a>Optie B - Switch van Federatie naar WHS met behulp van Azure AD Connect en PowerShell

Gebruik deze optie als uw federatieserver is niet in eerste instantie geconfigureerd met behulp van Azure AD Connect.

Als onderdeel van dit proces kunt u naadloze eenmalige aanmelding inschakelen en overschakelen van uw domeinen van federatieve beheerd.

   1. Op de Azure AD Connect-Server de wizard te openen.
   2. Selecteer aanmelden van gebruikers wijzigen en selecteer volgende. 
   3. Geef de gebruikersnaam en het wachtwoord van een globale beheerder in het scherm verbinding maken met Azure AD.
   4. Selecteer Schakel eenmalige aanmelding vervolgens selecteert u vervolgens op het aanmelden van gebruikers-scherm, het keuzerondje uit niet is geconfigureerd om wachtwoord-Hashsynchronisatie, te wijzigen.
   
   Voordat u de wijziging: ![Afbeelding 20](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image12.png)</br>

   Na de wijziging:  
   ![Afbeelding 22](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image13.png)</br>
   
   > [!NOTE]
   > Beginnen met Azure AD Connect versie 1.1.880.0, is de naadloze eenmalige aanmelding selectievakje standaard ingeschakeld.
   
   5. In het scherm inschakelen Single Sign-on, voert u de referenties van domein Administrator-account en selecteer je volgende.
   
   > [!NOTE]
   > Domeinbeheerder-referenties zijn vereist voor het inschakelen van naadloze eenmalige aanmelding als het proces voert de volgende acties waarvoor deze verhoogde machtigingen. De referenties voor de domeinbeheerder zijn niet opgeslagen in Azure AD Connect of in Azure AD. Ze alleen gebruikt voor het inschakelen van de functie en vervolgens verwijderd na de bewerking is voltooid.
   > * Een account met de naam AZUREADSSOACC (die staat voor Azure AD) is gemaakt in uw on-premises Active Directory (AD).
   > * Het computeraccount Kerberos ontsleutelingssleutel wordt veilig worden gedeeld met Azure AD.
   > * Bovendien zijn twee Kerberos-SPN-namen (SPN's) gemaakt voor twee URL's die worden gebruikt tijdens de aanmelding bij Azure AD.
   
   6. Zorg ervoor dat in de Ready to Configure scherm, 'Start synchronisatieproces wanneer de configuratie is voltooid' selectievakje is ingeschakeld. Selecteer vervolgens configureren.
   ![Afbeelding 41](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image15.png)</br>
   Wanneer u configureert, wordt aan de hand van de stap previews naadloze eenmalige aanmelding geconfigureerd. Configuratie van wachtwoord-Hashsynchronisatie wordt niet worden gewijzigd als deze eerder is ingeschakeld.
   
   > [!IMPORTANT]
   > Geen wijzigingen worden aangebracht in de manier waarop gebruikers zich aanmelden op dit moment.  
   
   7. Controleer of Federation blijft worden ingeschakeld en nu dat naadloze eenmalige aanmelding is ingeschakeld op de Azure AD-Portal.
   ![Afbeelding 42](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image16.png)

#### <a name="convert-domains-from-federated-to-managed"></a>Converteren van domeinen uit die zijn gefedereerd met het beheerd

Federatie is op dit moment nog steeds ingeschakeld en voor uw domeinen. Elk domein moet worden geconverteerd van federatieve naar beheerde om af te dwingen van gebruikersverificatie via wachtwoord-Hashsynchronisatie om door te gaan met de implementatie.

> [!IMPORTANT]
> Niet alle domeinen moeten het worden geconverteerd op hetzelfde moment, u kunt beginnen met een testdomein op uw productietenant of het domein met de minste aantal gebruikers.

De conversie wordt uitgevoerd met behulp van de Azure AD PowerShell-Module.

   1. Open PowerShell en meld u aan bij Azure AD met behulp van een globale beheerdersaccount.  
   2. Als u wilt converteren van het eerste domein, moet u de volgende opdracht uitvoeren:  
   
   ``` PowerShell
   Set-MsolDomainAuthentication -Authentication Managed -DomainName <domainname>
   ```
   
   3. Open de Azure AD-portal, selecteert u Azure Active Directory en selecteer vervolgens Azure AD Connect.
   4. Controleer of dat het domein is geconverteerd naar beheerd door de volgende opdracht uit:
   
   ``` PowerShell
   Get-MsolDomain -DomainName <domainname>
   ```

## <a name="testing-and-next-steps"></a>Testen en de volgende stappen

### <a name="test-authentication-with-phs"></a>Testen van authenticatie met WHS

Als uw tenant is gebruikmaakt van Federatie, zijn gebruikers van de aanmeldingspagina van Azure AD ophalen omgeleid naar uw AD FS-omgeving. Nu dat de tenant is geconfigureerd voor het gebruik van wachtwoord-Hashsynchronisatie in plaats van de Federatie, worden gebruikers niet wordt u omgeleid naar AD FS en in plaats daarvan wordt aan rechtstreeks via de Azure AD-aanmeldingspagina.

Open Internet Explorer in InPrivate-modus om te voorkomen dat naadloze eenmalige aanmelding automatisch aanmelden en Ga naar de aanmeldingspagina van Office 365 ([http://portal.office.com](http://portal.office.com/)). Typ de UPN van de gebruiker en klik op volgende. Zorg ervoor dat u de UPN van een hybride-gebruiker die is gesynchroniseerd vanuit uw on-premises Active Directory en die eerder is gefedereerd typt. De gebruiker ziet het scherm te typen in hun gebruikersnaam en wachtwoord.

![Afbeelding 9](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image18.png)

![Afbeelding 12](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image19.png)

Nadat u het wachtwoord typt, moet u ophalen omgeleid naar de Office 365-portal.

![Afbeelding 17](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image20.png)

### <a name="test-seamless-single-sign-on"></a>Naadloze eenmalige aanmelding testen

Meld u aan een domein gekoppelde machine die is verbonden met het bedrijfsnetwerk bevinden. Open Internet Explorer en Ga naar een van de volgende URL's:  
  
[https://myapps.microsoft.com/contoso.com](https://myapps.microsoft.com/contoso.com) [https://myapps.microsoft.com/contoso.onmicrosoft.com](https://myapps.microsoft.com/contoso.onmicrosoft.com) (vervangen door Contoso uw domein).

De gebruiker wordt kort worden omgeleid naar de aanmeldingspagina van Azure AD en het bericht 'Het aanmelden ' en niet moet worden gevraagd om een gebruikersnaam of wachtwoord.

![Afbeelding 24 uur per dag](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image21.png)

Vervolgens wordt de gebruiker ophalen omgeleid en bent aangemeld bij het toegangsvenster is:

> [!NOTE]
> Naadloze eenmalige aanmelding werkt op Office 365-services die ondersteuning biedt voor geheugensteun voor het domein (bijvoorbeeld myapps.microsoft.com/contoso.com). De Office 365-portal (portal.office.com) op dit moment biedt geen ondersteuning voor domeinhint en daarom is het waarschijnlijk dat gebruikers moeten hun UPN typt. Zodra een UPN die is opgegeven, naadloze eenmalige aanmelding op het Kerberos-ticket namens de gebruiker ophalen en melden ze in zonder een wachtwoord te typen. 

> [!TIP]
> Overweeg de implementatie van [hybride van Azure AD Join op Windows 10](https://docs.microsoft.com/azure/active-directory/device-management-introduction) voor een verbeterde eenmalige aanmelding mogelijk.

### <a name="removal-of-the-relying-party-trust"></a>Het verwijderen van de relying party trust

Nadat u hebt geverifieerd dat alle gebruikers en -clients is tijdens de verificatie via Azure AD, kan deze veilig verwijderen van de Office 365-vertrouwensrelatie van relying party worden beschouwd.

Als AD FS niet wordt gebruikt voor andere doeleinden (andere Relying Party-vertrouwensrelaties zijn geconfigureerd), is het veilig om op te nemen van AD FS nu.

### <a name="rollback"></a>Ongedaan maken

Als er een groot probleem is gevonden en kan niet snel worden opgelost, kunt u besluiten om terug te draaien back-de oplossing op Federation.

Raadpleeg de Federation-documentatie voor ontwerp en implementatie voor uw specifieke implementatie-informatie. Het beste overleggen met het proces:

* Beheerde domeinen wilt federeren met behulp van Convert-MSOLDomainToFederated converteren

* Indien nodig, worden er regels van aanvullende configureren.

### <a name="enable-synchronization-of-userprincipalname-updates"></a>Synchronisatie van userPrincipalName updates inschakelen

In het verleden updates voor het kenmerk UserPrincipalName met behulp van de synchronisatieservice van on-premises is geblokkeerd, tenzij beide volgende voorwaarden wordt voldaan:

* De gebruiker wordt beheerd (niet-gefedereerde).

* De gebruiker heeft geen licentie is toegewezen.

Voor instructies over het controleren of deze functie inschakelen, raadpleegt u het volgende artikel:

[UserPrincipalName-updates synchroniseren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsyncservice-features).

### <a name="troubleshooting"></a>Problemen oplossen

Het ondersteuningsteam moet weten hoe u het oplossen van verificatieproblemen die zich voordoen tijdens of na de wijziging van Federatie beheerd. Gebruik de volgende documentatie voor het oplossen van problemen voor het ondersteuningsteam raken met de algemene stappen voor probleemoplossing en de bijbehorende acties die helpen kunnen om te isoleren en los het probleem.

[Azure Active Directory-wachtwoord-Hashsynchronisatie oplossen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization)

[Azure Active Directory naadloze eenmalige aanmelding oplossen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sso)  

## <a name="roll-over-the-seamless-sso-kerberos-decryption"></a>Beweeg de muis over de ontsleuteling van Kerberos met naadloze eenmalige aanmelding

Het is belangrijk dat u vaak plaats de muisaanwijzer op de Kerberos ontsleutelingssleutel van het account van de AZUREADSSOACC computer (die staat voor Azure AD) hebt gemaakt in uw on-premises AD-forest. Het is raadzaam dat u via de ontsleutelingssleutel Kerberos ten minste elke 30 dagen implementeert zodat deze overeenkomt met hoe de wachtwoordwijzigingen voor het verzenden van leden van een domein Active Directory. Omdat er niet gekoppeld is moet apparaat dat is gekoppeld aan het computeraccountobject AZUREADSSOACC de meegenomen handmatig worden uitgevoerd.

Volg deze stappen op de on-premises server waarop Azure AD Connect de rollover van de ontsleutelingssleutel Kerberos initiëren.

[Hoe kan ik de muisaanwijzer op de ontsleutelingssleutel Kerberos van het account van de computer AZUREADSSOACC](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq)?

## <a name="next-steps"></a>Volgende stappen

- [Ontwerpconcepten van Azure AD Connect](plan-connect-design-concepts.md)
- [Kies de juiste verificatie](https://docs.microsoft.com/azure/security/azure-ad-choose-authn)
- [Ondersteunde topologieën](plan-connect-design-concepts.md)
