---
title: 'Azure AD Connect: Migreren van Federatie naar pass-through-verificatie voor Azure AD | Microsoft Docs'
description: Informatie over het verplaatsen van uw hybride identiteit omgeving van Federatie naar Pass-through-verificatie.
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
ms.openlocfilehash: dcb2d1741a8e62bd317881d3f224d3358cad8778
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53557203"
---
# <a name="migrate-from-federation-to-pass-through-authentication-for-azure-ad"></a>Migreren van Federatie naar pass-through-verificatie voor Azure AD
Het volgende document bevat richtlijnen over het verplaatsen van AD FS naar pass-through-verificatie.

>[!NOTE]
>Een downloadbare een kopie van dit document vindt u [hier](https://aka.ms/ADFSTOPTADPDownload).

## <a name="prerequisites-for-pass-through-authentication"></a>Vereisten voor Pass through-verificatie
De volgende vereisten zijn vereist voordat u kunt migreren.
### <a name="update-azure-ad-connect"></a>Update van Azure AD Connect

Als een minimum uit te voeren de stappen voor het migreren naar pass-through-verificatie, moet u hebben [Azure AD connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0. Deze versie bevat belangrijke wijzigingen aangebracht in de manier waarop aanmelden conversie wordt uitgevoerd en vermindert de totale tijd voor het migreren van Federatie naar Cloud-verificatie van mogelijk uren en minuten.

> [!IMPORTANT]
> Verouderde documentatie, hulpprogramma's en blogs aangeven dat de gebruikersconversie een vereiste stap is bij het converteren van domeinen van federatieve naar beheerde. **Converteren van gebruikers** is niet meer vereist en Microsoft is bezig met bijwerken van de documentatie en hulpprogramma's om dit weer te geven.

Azure AD Connect bijwerken naar de nieuwste versie, volg deze [instructies update](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

### <a name="plan-authentication-agent-number-and-placement"></a>Verificatie-agent aantal en de plaatsing plannen

Pass through-verificatie wordt bereikt door het lichte agents op de Azure AD Connect-Server en op uw on-premises Windows-Servers implementeren. Installeer de agents zo dicht mogelijk bij uw Active Directory-domeincontrollers te verminderen latentie.

Voor de meeste klanten twee of drie verificatie-agents zijn voldoende voor hoge beschikbaarheid en capaciteit kan, en een tenant niet meer dan 12 agents die zijn geregistreerd. De eerste agent is geïnstalleerd op de AAD Connect-server zelf. Raadpleeg de [informatie over het netwerkverkeer schattingen en prestatierichtlijnen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-current-limitations) om te begrijpen van de beperkingen van de agent en de opties voor implementatie van de agent.

### <a name="plan-migration-method"></a>Migratiemethode plannen

Er zijn twee methoden voor het migreren van federatieve verificatie PTA en naadloze eenmalige aanmelding. De methode die u gebruikt afhankelijk van hoe uw AD FS voor het oorspronkelijk is geconfigureerd.

- **Azure AD Connect gebruiken**. Als u AD FS oorspronkelijk is geconfigureerd met Azure AD Connect, wordt de wijziging van Pass through-verificatie *moet* worden uitgevoerd via de Azure AD Connect-wizard.

Wanneer u Azure AD Connect gebruikt, wordt de cmdlet Set-MsolDomainAuthentication voor u wordt automatisch uitgevoerd wanneer u de gebruiker aanmelden methode wijzigen en daarom hebt u geen controle over het unfederating alle van de gecontroleerde federatieve domeinen in uw Azure AD-tenant.  
‎  
> [!NOTE]
> Op dit moment niet federating alle kan niet worden vermeden domeinen in uw tenant wanneer u de gebruiker aanmelden bij Pass through-verificatie wanneer AAD Connect het oorspronkelijk is gebruikt voor het configureren van AD FS voor u.  
‎
- **Azure AD Connect gebruiken met PowerShell**. Deze methode kan worden gebruikt alleen wanneer de AD FS oorspronkelijk niet is geconfigureerd met Azure AD Connect. U moet nog steeds wijzigen van de gebruiker aanmelden methode via de Azure AD Connect-wizard, maar het belangrijkste verschil is dat deze wordt niet automatisch uitgevoerd de cmdlet Set-MsolDomainAuthentication voor u als er geen kennis van uw AD FS-farm, en daarom hebt u volledige controle over welke domeinen worden geconverteerd en in welke volgorde.

Om te begrijpen welke methode u moet gebruiken, moet u de stappen uitvoeren op de volgende sectie.

#### <a name="verify-current-user-sign-in-settings"></a>Controleer of de huidige gebruiker aanmelden instellingen

Controleer of de huidige gebruiker aanmelden instellingen door aan te melden bij de Azure AD-beheerportal [ https://aad.portal.azure.com ](https://aad.portal.azure.com/) met een globale beheerdersaccount. 

In de **gebruiker aanmelden** sectie, Controleer **Federation** is **ingeschakeld** en **naadloze eenmalige aanmelding** en  **Pass through-verificatie** zijn **uitgeschakelde**. 

![Afbeelding 5](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image1.png)

#### <a name="verify-how-federation-was-configured"></a>Controleer of het federation is geconfigureerd

   1. Ga naar uw Azure AD Connect-server en start Azure AD Connect en selecteer vervolgens **configureren**.
   2. Op de **extra taken** scherm, selecteer **de huidige configuratie weergeven** en selecteer vervolgens **volgende**.</br>
   ![Afbeelding 31](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image2.png)</br>
   3. In de **uw oplossing controleren** scherm, schuif omlaag naar **Active Directory Federation Services (AD FS)**.</br>
   Als u ziet dat de AD FS-configuratie in deze sectie is en u veilig wordt ervan uitgegaan dat kunt AD FS oorspronkelijk is geconfigureerd met Azure AD Connect en kan daarom de conversie van uw domeinen uit die zijn gefedereerd met het beheerd via de Azure AD Connect worden bepaald  **Aanmelden van gebruikers wijzigen** optie, dit proces wordt beschreven in de sectie **optie 1: Pass through-verificatie configureren met behulp van Azure AD Connect**.  
‎
   4. Als u Active Directory Federation Services die worden vermeld in de huidige instellingen niet ziet, dan u handmatig omzetten in de domeinen die zijn gefedereerd moet met beheerd via PowerShell die wordt beschreven in de sectie **optie 2 - Switch van Federatie naar PTA met behulp van Azure AD Connect en PowerShell**.

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


Alle instellingen die mogelijk zijn aangepast aan de Federation-documentatie voor ontwerp en implementatie, specifiek valideren **PreferredAuthenticationProtocol**, **SupportsMfa**, en  **PromptLoginBehavior**.

Meer informatie over wat deze instellingen u hieronder vindt.

[Active Directory Federation Services vragen = aanmelding parameter ondersteuning](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)  
‎[Set-MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> Als de waarde SupportsMfa is momenteel ingesteld op 'True' vervolgens betekent dit dat u een On-Premises MFA-oplossing gebruikt voor het invoeren van een 2e factor uitdaging in de gebruikersstroom voor verificatie. Dit werkt niet meer voor scenario's Azure AD-verificatie, en u moet in plaats daarvan gebruikmaken van de service Azure MFA (cloud-indeling) om uit te voeren dezelfde functie. Zorgvuldig uw MFA-vereisten evalueren voordat u verder gaat en zorg ervoor dat u begrijpt hoe u Azure MFA, de gevolgen voor de licenties en het registratieproces voor de eindgebruiker voordat u converteert uw domeinen.

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
| AD FS is sterk gepersonaliseerde en vertrouwen op specifieke aanpassingsinstellingen in het bestand OnLoad.js die in Azure AD kan niet worden gedupliceerd (bijvoorbeeld, u hebt gewijzigd de aanmeldingsprocedure zodat gebruikers alleen SamAccountName-indeling voor hun gebruikersnaam invoeren dus aan een UPN of hebben een sterk merknaam de aanmeldingservaring)| U moet controleren dat de aanpassingsvereisten van uw huidige kunnen worden voldaan door Azure AD voordat u doorgaat. Raadpleeg de huisstijl van AD FS en AD FS-aanpassing secties voor meer informatie over en richtlijnen.|
| U blokkeren oudere verificatieclients via AD FS.| Houd rekening met het vervangen van de besturingselementen voor het blokkeren van oudere verificatieclients die momenteel aanwezig zijn op de AD FS met een combinatie van [voorwaardelijke toegang voor oudere verificatie besturingselementen](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) en [Exchange Online Client Access Regels](http://aka.ms/EXOCAR). |
| U vereisen dat gebruikers MFA tegen een on-premises MFA-server-oplossing uitvoeren bij het verifiëren van de AD FS.| Kunt u zich niet aan het invoeren van een MFA-controle via de on-premises MFA-oplossing in de authenticatiestroom voor een beheerd domein, maar u de Azure MFA-service gebruiken kunt om dit te doen voortaan zodra het domein wordt geconverteerd. Als gebruikers Azure MFA niet momenteel gebruiken, wordt dit een eenmalige eindgebruikers registratiestap die u moet voorbereiden en communiceren aan uw eindgebruikers inhouden. |
| U beleid voor toegangsbeheer (AuthZ-regels) momenteel gebruikt in AD FS voor het beheren van toegang tot Office 365.| Houd rekening met het vervangen van deze met de equivalente Azure AD [beleid voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) en [toegangsregels voor Exchange Online clients](http://aka.ms/EXOCAR).|

### <a name="considerations-for-common-ad-fs-customizations"></a>Overwegingen voor de algemene AD FS-aanpassingen

#### <a name="inside-corporate-network-claim"></a>Binnen bedrijfsnetwerk

De claim InsideCorporateNetwork is uitgegeven door AD FS als de gebruiker die heeft geverifieerd binnen het bedrijfsnetwerk is. Deze claim kan vervolgens worden doorgegeven aan Azure AD en gebruikt om meervoudige verificatie op basis van netwerklocatie van de gebruikers over te slaan. Zie [goedgekeurde IP-adressen voor federatieve gebruikers](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud) voor meer informatie over het bepalen of u dit is momenteel ingeschakeld in AD FS hebt.

De claim InsideCorporateNetwork zijn niet beschikbaar meer zodra uw domeinen worden geconverteerd naar Pass-through-verificatie. [Benoemde locaties in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) kan worden gebruikt ter vervanging van deze functionaliteit.

Als met de naam locaties zijn geconfigureerd, alle beleidsregels voor voorwaardelijke toegang geconfigureerd wilt opnemen of uitsluiten van de netwerklocaties 'Alle vertrouwde locaties' of 'MFA goedgekeurde IP-adressen' moet worden bijgewerkt om de zojuist gemaakte benoemde locaties weer te geven.

Zie [Active Directory voor voorwaardelijke toegang tot locaties](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations) voor meer informatie over de locatie-voorwaarde voor de voorwaardelijke toegang.

#### <a name="hybrid-azure-ad-joined-devices"></a>Hybride Azure AD verbonden apparaten

Een apparaat toevoegen aan Azure AD kunt u regels voor voorwaardelijke toegang die afdwingen van apparaten die voldoen aan uw standaarden voor beveiliging en naleving maken en Hiermee kunnen gebruikers zich aanmelden bij een apparaat met behulp van een organisatie-werk- of schoolaccount in plaats van een persoonlijke -account. Hybride Azure AD met toegevoegde apparaten kunt u uw AD-domein gekoppelde apparaten toevoegen aan Azure AD. Uw federatieve omgeving is mogelijk geconfigureerd met deze functie.

Om ervoor te zorgen dat hybride Join blijft werken voor nieuwe apparaten toegevoegd aan het domein zodra uw domeinen zijn geconverteerd naar Pass-through-verificatie, moet Azure AD Connect worden geconfigureerd voor Active Directory-computeraccounts voor Windows 10-clients om te synchroniseren Azure AD. Naadloze eenmalige aanmelding hybride Join wordt gebruikt voor het registreren van de computer in Azure AD voor Windows 7 en Windows 8-computeraccounts, en u hoeft niet te synchroniseren ze zoals u dat wel voor Windows 10-apparaten doet. U wordt wel om een bijgewerkte workplacejoin.exe-bestand (via een MSI-bestand) te implementeren op deze clients met oudere versies, zodat ze zelf via naadloze eenmalige aanmelding kunnen registreren. [Download het MSI-bestand](https://www.microsoft.com/download/details.aspx?id=53554).

Zie voor meer informatie over deze vereiste [hoe het configureren van hybride Azure Active Directory apparaten gekoppelde](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup)

#### <a name="branding"></a>Huisstijl

Uw organisatie mogelijk [aangepast van uw AD FS sign-in's](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) om weer te geven informatie meer relevant zijn voor de organisatie. Als dit het geval is, kunt u soortgelijke [aanpassingen van de Azure AD-aanmeldingspagina](https://docs.microsoft.com/azure/active-directory/customize-branding).

Terwijl vergelijkbare aanpassingen die beschikbaar zijn, worden sommige visuele wijzigingen verwacht. Mogelijk wilt de verwachte wijzigingen in uw communicatie met eindgebruikers opnemen.

> [!NOTE]
> Aangepaste huisstijl is alleen beschikbaar als u de Premium- of Basic-licentie worden aangeschaft voor Azure AD of Office 365-licentie hebt.

## <a name="planning-for-smart-lockout"></a>Planning voor slimme accountvergrendeling

Azure AD slimme accountvergrendeling worden beschermd tegen aanvallen met brute-force-wachtwoord en voorkomt u dat de on-premises Active Directory-account wordt vergrendeld als Pass through-verificatie wordt gebruikt en een groep voor accountvergrendelingsbeleid is ingesteld in Active Directory. 

Voor meer informatie over [slimme accountvergrendeling en het bewerken van de configuratie ervan](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout).

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

Nadat de beide Pass through-verificatie en naadloze eenmalige aanmelding zijn geïmplementeerd, verandert de eindgebruikerservaring voor aanmelden bij het verkrijgen van toegang tot Office 365 en andere gekoppelde resources via Azure AD zijn geverifieerd. Gebruikers van buiten het netwerk ziet nu de Azure AD-aanmeldingspagina, in plaats van wordt omgeleid naar de pagina op basis van formulieren gepresenteerd door de externe tegenoverliggende Web Application Proxy-servers.

Er zijn meerdere elementen op de planning van uw Communicatiestrategie voor. Deze omvatten:

* Gebruikers van de toekomstige en vrijgegeven functionaliteit via de hoogte te brengen
  * E-mail en andere interne communicatiekanalen
  * Visuele elementen zoals posters
  * Live of andere communicatie
* Bepalen wie gaat aanpassen en die de communicatie wordt verzonden en wanneer.

## <a name="implementing-your-solution"></a>Uw oplossing implementeren

Nu dat u uw oplossing hebt gepland, bent u klaar om dit te implementeren. Implementatie omvat de volgende onderdelen:

   1. Voorbereiden voor naadloze eenmalige aanmelding op
   2. Wijzigen van aanmeldingsmethode Pass through-verificatie en naadloze eenmalige aanmelding inschakelen

## <a name="step-1--prepare-for-seamless-sso"></a>Stap 1: voorbereiden voor naadloze eenmalige aanmelding

Op uw apparaten naadloze eenmalige aanmelding gebruiken, moet u een Azure AD-URL toevoegen aan zone-instellingen van de gebruikers Intranet met behulp van Groepsbeleid in Active Directory.

Standaard berekent de browser automatisch de juiste zone, Internet- of intranethosts van een specifieke URL. Bijvoorbeeld, "http://contoso/'wordt toegewezen aan de intranetzone, terwijl'http://intranet.contoso.com/' wordt toegewezen aan de zone Internet (omdat de URL een periode bevat). Browsers wordt Kerberos-tickets niet verzenden naar een cloudeindpunt, zoals de URL van de Azure AD, tenzij u expliciet de URL aan de intranetzone van de browser toevoegen.

Ga als volgt de [stappen om te worden uitgerold](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) de benodigde wijzigingen aan uw apparaten.

> [!IMPORTANT]
> Deze wijziging niet de manier waarop uw gebruikers zich aanmelden bij Azure AD worden gewijzigd. Het is echter belangrijk dat deze configuratie wordt toegepast op al uw apparaten voordat u met de stap 3 ook Houd er rekening mee dat gebruikers die zich op apparaten die deze configuratie niet ontvangen gewoon moet invoeren doorgaat, gebruikersnaam en wachtwoord aanmelden bij Azure AD.

## <a name="step-2--change-sign-in-method-to-pta-and-enable-seamless-sso"></a>Stap 2: aanmelden methode wijzigen in PTA en naadloze eenmalige aanmelding inschakelen

### <a name="option-a-configuring-pta-by-using-azure-ad-connect"></a>Optie A: PTA configureren met behulp van Azure AD Connect

Gebruik deze methode als uw AD FS is in eerste instantie geconfigureerd met behulp van Azure AD Connect. U kunt deze methode niet gebruiken als uw AD FS oorspronkelijk niet is geconfigureerd met behulp van Azure AD Connect.

> [!IMPORTANT]
> Houd er rekening mee dat met de volgende stappen hieronder alle domeinen worden geconverteerd van federatieve naar beheerd. Raadpleeg de sectie migratiemethode plannen voor meer informatie.[](#_Plan_Migration_Method)

U moet eerst aanmeldingsmethode wijzigen:

   1. Op de Azure AD Connect-Server de wizard te openen.
   2. Selecteer **wijziging gebruiker aanmelden** en selecteer vervolgens **volgende**. 
   3. In de **verbinding maken met Azure AD** scherm Geef de gebruikersnaam en wachtwoord van een globale beheerder.
   4. De **aanmelden van gebruikers** scherm, wijzigt u het keuzerondje van **Federatie met AD FS** naar **Pass through-verificatie**, selecteer **eenmalige aanmelding inschakelen**  Selecteer **volgende**.
   5. In het scherm inschakelen Single Sign-on, voert u de referenties van domein Administrator-account en selecteer je volgende.  

   > [!NOTE]
   > Domeinbeheerder-referenties zijn vereist voor het inschakelen van naadloze eenmalige aanmelding als het proces voert de volgende acties waarvoor deze verhoogde machtigingen. De referenties voor de domeinbeheerder zijn niet opgeslagen in Azure AD Connect of in Azure AD. Ze alleen gebruikt voor het inschakelen van de functie en vervolgens verwijderd na de bewerking is voltooid
   >
   > * Een account met de naam AZUREADSSOACC (die staat voor Azure AD) is gemaakt in uw on-premises Active Directory (AD).
   > * Het computeraccount Kerberos ontsleutelingssleutel wordt veilig worden gedeeld met Azure AD.
   > * Bovendien zijn twee Kerberos-SPN-namen (SPN's) gemaakt voor twee URL's die worden gebruikt tijdens de aanmelding bij Azure AD.
   > * De referenties voor de domeinbeheerder zijn niet opgeslagen in Azure AD Connect of in Azure AD. Ze alleen gebruikt voor het inschakelen van de functie en vervolgens verwijderd na de bewerking is voltooid

   6. In de **klaar om te configureren** scherm, controleert u of **Start synchronisatieproces wanneer de configuratie is voltooid** selectievakje is ingeschakeld. Selecteer vervolgens **configureren**.</br>
   ![afbeelding](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image8.png)</br>
   7. Open de **Azure AD-portal**, selecteer **Azure Active Directory**, en selecteer vervolgens **Azure AD Connect**.
   8. Controleer **Federation is uitgeschakeld** terwijl **naadloze eenmalige aanmelding** en **Pass grondige verificatie** zijn **ingeschakeld**.</br>
   ![afbeelding](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image9.png)</br>

Vervolgens moet u aanvullende verificatiemethoden implementeren. Open de **Azure-portal**, blader naar **Azure Active Directory, Azure AD Connect** en klikt u op **Pass through-verificatie**.

Uit de **Pass through-verificatie** pagina, klikt u op de knop downloaden. Uit de **downloaden** Agent scherm, klikt u op **voorwaarden accepteren en downloaden**.

Het downloaden van aanvullende verificatie-agents wordt gestart. De secundaire verificatie-Agent installeren op een server voor het domein. 

> [!NOTE]
> De eerste agent is altijd op de Azure AD Connect-server zelf als onderdeel van de configuratiewijzigingen in de sectie gebruiker aanmelden van het hulpprogramma Azure AD Connect geïnstalleerd. Eventuele aanvullende verificatie-Agents moet worden geïnstalleerd op een afzonderlijke server. Het wordt aanbevolen dat tussen 2 en 3 aanvullende verificatie-Agents beschikbaar. 

De verificatie-Agent-installatie uitvoeren. Tijdens de installatie, moet u voor referenties van een **hoofdbeheerder** account.

![Afbeelding 1243067202](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image11.png)

![Afbeelding 1243067210](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image12.png)

Nadat de verificatie-Agent is geïnstalleerd, kunt u terugkeren naar de health-pagina van de Agent voor Pass through-verificatie om te controleren of de status van de extra agents.


Ga naar testen en de volgende stappen.

> [!IMPORTANT]
> De sectie optie B: overslaan Overschakelen van Federatie naar PTA met behulp van Azure AD Connect en PowerShell als de stappen in die sectie niet van toepassing.  

### <a name="option-b---switch-from-federation-to-pta-using-azure-ad-connect-and-powershell"></a>Optie B - Switch van Federatie naar PTA met behulp van Azure AD Connect en PowerShell

Gebruik deze optie als uw federatieserver is niet in eerste instantie geconfigureerd met behulp van Azure AD Connect. Eerst moet u Pass-through-verificatie inschakelen:

   1. Op de Azure AD Connect-Server de wizard te openen.
   2. Selecteer **wijziging gebruiker aanmelden** en selecteer vervolgens **volgende**.
   3. In de **verbinding maken met Azure AD** scherm Geef de gebruikersnaam en wachtwoord van een globale beheerder.
   4. Op de **aanmelden van gebruikers** scherm, wijzigt u het keuzerondje van **niet configureert** naar **Pass through-verificatie**, selecteer **eenmalige aanmeldinginschakelen** Selecteer **volgende**.
   5. In **inschakelen Single Sign-on** scherm, voert u de referenties van domein Administrator-account en selecteer vervolgens **volgende**.

   > [!NOTE]
   > Domeinbeheerder-referenties zijn vereist voor het inschakelen van naadloze eenmalige aanmelding als het proces voert de volgende acties waarvoor deze verhoogde machtigingen. De referenties voor de domeinbeheerder zijn niet opgeslagen in Azure AD Connect of in Azure AD. Ze alleen gebruikt voor het inschakelen van de functie en vervolgens verwijderd na de bewerking is voltooid.
   >
   > * Een account met de naam AZUREADSSOACC (die staat voor Azure AD) is gemaakt in uw on-premises Active Directory (AD).
   > * Het computeraccount Kerberos ontsleutelingssleutel wordt veilig worden gedeeld met Azure AD.
   > * Bovendien zijn twee Kerberos-SPN-namen (SPN's) gemaakt voor twee URL's die worden gebruikt tijdens de aanmelding bij Azure AD.

   6. In de **klaar om te configureren** scherm, controleert u of **Start synchronisatieproces wanneer de configuratie is voltooid** selectievakje is ingeschakeld. Selecteer vervolgens **configureren**.</br>
   ‎![afbeelding](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image18.png)</br>
   De volgende stappen uitgevoerd bij het selecteren van configureren:
   * De eerste Pass through-verificatie-Agent is geïnstalleerd
   * De Pass Through-functie is ingeschakeld
   * Naadloze eenmalige aanmelding is ingeschakeld.  
   7. Controleer **Federation** is nog steeds **ingeschakeld** en **naadloze eenmalige aanmelding** en **Pass grondige verificatie** zijn nu beschikbaar.
   ![Afbeelding 2](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image19.png)
   8. Selecteer **Pass through-verificatie** en controleer of de status **Active**.</br>
   
   Als de verificatie-Agent niet actief is, voert u de [deze stappen voor probleemoplossing](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication) voordat u doorgaat met het proces van de conversatie domein in de volgende stap. U risico waardoor verificatie als u uw domeinen vóór het valideren van converteert uw PTA agents hebt geïnstalleerd en dat hun status wordt weergegeven als 'Active' in de Azure-portal.  
   9. Implementeer vervolgens aanvullende verificatie-agents. Open de **Azure-portal**, blader naar **Azure Active Directory**, **Azure AD Connect** en klikt u op **Pass through-verificatie**.
   10. Uit de **Pass through-verificatie** pagina, klikt u op de **downloaden** knop. Uit de **-Agent downloaden** scherm, klikt u op **voorwaarden accepteren en downloaden**.
   
   Het downloaden van aanvullende verificatie-agents wordt gestart. De secundaire verificatie-Agent installeren op een server voor het domein.

  > [!NOTE]
  > De eerste agent is altijd op de Azure AD Connect-server zelf als onderdeel van de configuratiewijzigingen in de sectie gebruiker aanmelden van het hulpprogramma Azure AD Connect geïnstalleerd. Eventuele aanvullende verificatie-Agents moet worden geïnstalleerd op een afzonderlijke server. Het wordt aanbevolen dat tussen 2 en 3 aanvullende verificatie-Agents beschikbaar.
  
   11. De verificatie-Agent-installatie uitvoeren. Tijdens de installatie, moet u voor referenties van een **hoofdbeheerder** account.</br>
   ![Afbeelding 1243067215](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image23.png)</br>
   ![Afbeelding 1243067216](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image24.png)</br>
   12. Nadat de verificatie-Agent is geïnstalleerd, kunt u terugkeren naar de health-pagina van de Agent voor Pass through-verificatie om te controleren of de status van de extra agents.

Federatie is op dit moment nog steeds ingeschakeld en voor uw domeinen. Als u wilt doorgaan met de implementatie, moet elk domein uit federatieve worden geconverteerd naar beheerde zodat Pass through-verificatie wordt gestart voor de verificatie-aanvragen voor het domein.

Niet alle domeinen moeten het worden geconverteerd op hetzelfde moment, u kunt beginnen met een testdomein op uw productietenant of het domein met de minste aantal gebruikers.

De conversie wordt uitgevoerd met behulp van de Azure AD PowerShell-Module.

   1. Open **PowerShell** en meld u aan bij Azure AD via een **hoofdbeheerder** account.
   2. Als u wilt converteren van het eerste domein, moet u de volgende opdracht uitvoeren:
 
 ``` PowerShell
 Set-MsolDomainAuthentication -Authentication Managed -DomainName <domainname>
 ```
 
   3. Open de **Azure AD-portal**, selecteer **Azure Active Directory**, en selecteer vervolgens **Azure AD Connect**.  
   4. Nadat u hebt uw federatieve domeinen geconverteerd, Controleer **Federation is uitgeschakeld** terwijl **naadloze eenmalige aanmelding** en **Pass through-verificatie** zijn **Ingeschakeld**.</br>
   ![afbeelding](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image26.png)</br>

## <a name="testing-and-next-steps"></a>Testen en de volgende stappen

### <a name="test-pass-through-authentication"></a>Test Pass through-verificatie 

Als uw tenant is gebruikmaakt van Federatie, zijn gebruikers van de aanmeldingspagina van Azure AD ophalen omgeleid naar uw AD FS-omgeving. Nu dat de tenant is geconfigureerd voor het gebruik van Pass through-verificatie in plaats van de Federatie, worden gebruikers niet wordt u omgeleid naar AD FS en in plaats daarvan wordt aan rechtstreeks via de Azure AD-aanmeldingspagina.

Open Internet Explorer in InPrivate-modus om te voorkomen dat naadloze eenmalige aanmelding automatisch aanmelden en Ga naar de aanmeldingspagina van Office 365 ([http://portal.office.com](http://portal.office.com/)). Type de **UPN** van uw gebruiker en klik op **volgende**. Zorg ervoor dat u de UPN van een hybride-gebruiker die is gesynchroniseerd vanuit uw on-premises Active Directory en die eerder is gefedereerd typt. De gebruiker ziet het scherm te typen in hun gebruikersnaam en wachtwoord.

![Afbeelding 18](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image27.png)

![Afbeelding 19](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image28.png)

Nadat u het wachtwoord typt, moet u ophalen omgeleid naar de Office 365-portal.

![Afbeelding 23](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image29.png)

### <a name="test-seamless-single-sign-on"></a>Naadloze eenmalige aanmelding testen

   1. Meld u aan bij een domein lid zijn van de computer die is verbonden met het bedrijfsnetwerk. 
   2. Open **Internet Explorer** of **Chrome** en gaat u naar een van de volgende URL's:   
      ‎  
      ‎[https://myapps.microsoft.com/contoso.com](https://myapps.microsoft.com/contoso.com) [https://myapps.microsoft.com/contoso.onmicrosoft.com](https://myapps.microsoft.com/contoso.onmicrosoft.com) (vervangen door Contoso uw domein).

      De gebruiker wordt kort worden omgeleid naar de aanmeldingspagina van Azure AD en het bericht 'Het aanmelden ' en niet moet worden gevraagd om een gebruikersnaam of wachtwoord.</br>
   ![Afbeelding 24 uur per dag](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image30.png)</br>
   3. Vervolgens wordt de gebruiker ophalen omgeleid en bent aangemeld bij het toegangsvenster is:

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

## <a name="roll-over-the-seamless-sso-kerberos-decryption-key"></a>Beweeg de muis over de naadloze eenmalige aanmelding Kerberos-ontsleutelingssleutel

Het is belangrijk dat u vaak plaats de muisaanwijzer op de Kerberos ontsleutelingssleutel van het account van de AZUREADSSOACC computer (die staat voor Azure AD) hebt gemaakt in uw on-premises AD-forest. Het is raadzaam dat u via de ontsleutelingssleutel Kerberos ten minste elke 30 dagen implementeert zodat deze overeenkomt met hoe de wachtwoordwijzigingen voor het verzenden van leden van een domein Active Directory. Als er geen gekoppelde apparaat dat is gekoppeld aan het computeraccountobject AZUREADSSOACC is moet de rollover handmatig worden uitgevoerd.

Volg deze stappen op de on-premises server waarop Azure AD Connect worden de meegenomen van de ontsleutelingssleutel Kerberos initiëren.

[Hoe kan ik de muisaanwijzer op de ontsleutelingssleutel Kerberos van het account van de computer AZUREADSSOACC](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq)?

## <a name="monitoring-and-logging"></a>Bewaking en registratie

De servers met de verificatie-Agents moeten worden gecontroleerd om de beschikbaarheid van de oplossing. De verificatie-Agents blootstellen naast algemene server-prestatiemeters, prestatie-objecten die kunnen worden gebruikt om te begrijpen van verificatie-statistieken en fouten.

Verificatie-Agents Meld u aan operations Windows-gebeurtenislogboeken onder '-toepassing en Service Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin'.

Problemen met Logboeken oplossen kan worden ingeschakeld indien nodig.

Zie voor meer informatie over [controle en logboekregistratie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-Pass-through-authentication).

## <a name="next-steps"></a>Volgende stappen

- [Ontwerpconcepten van Azure AD Connect](plan-connect-design-concepts.md)
- [Kies de juiste verificatie](https://docs.microsoft.com/azure/security/azure-ad-choose-authn)
- [Ondersteunde topologieën](plan-connect-design-concepts.md)
