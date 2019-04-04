---
title: Plannen en uitvoeren van de implementatie van een Azure multi-factor Authentication - Azure Active Directory
description: Microsoft Azure multi-factor Authentication-implementatie plannen
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 11846ca2a323da5889f444024767df4803a48a51
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58892208"
---
# <a name="planning-a-cloud-based-azure-multi-factor-authentication"></a>Planning van een cloud-gebaseerde Azure multi-factor Authentication

Mensen verbinding maakt met resources van de organisatie in steeds complexer scenario's. Personen verbinden van eigendom van de organisatie, persoonlijke en openbare apparaten in of uit het bedrijfsnetwerk met behulp van Smartphones, tablets, pc's en laptops, vaak op meerdere platforms. In deze wereld altijd verbonden, meerdere apparaten en meerdere platforms zijn de beveiliging van gebruikersaccounts is belangrijker dan ooit. Wachtwoorden, ongeacht hun complexiteit, die wordt gebruikt door apparaten, netwerken en platforms zijn niet meer voldoende zijn voor de beveiliging van het gebruikersaccount, vooral wanneer gebruikers vaak opnieuw gebruiken van wachtwoorden van accounts. Geavanceerde phishing en andere social hacking aanvallen kunnen resulteren in gebruikersnamen en wachtwoorden worden geplaatst en die worden verkocht via de donkere Internet.

[Azure multi-factor Authentication (MFA)](concept-mfa-howitworks.md) om de toegang tot gegevens en toepassingen beveiligen. Het biedt een extra beveiligingslaag met behulp van een tweede vorm van verificatie. Organisaties kunnen gebruiken [voorwaardelijke toegang](../conditional-access/overview.md) aanpassen aan hun specifieke behoeften aan dat de oplossing.

## <a name="prerequisites"></a>Vereisten

Voordat u begint met een implementatie van Azure multi-factor Authentication, zijn er vereiste items die moeten worden overwogen.

| Scenario | Vereiste |
| --- | --- |
| **Alleen in de cloud** identity-omgeving met moderne verificatie | **Er is geen aanvullende vereiste taken** |
| **Hybride** identiteit scenario's | [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) is geïmplementeerd en gebruikers-id's zijn gesynchroniseerd of Verenigd met de on-premises Active Directory Domain Services met Azure Active Directory. |
| On-premises oudere toepassingen die zijn gepubliceerd voor cloudtoegang | Azure AD [Application Proxy](../manage-apps/application-proxy.md) wordt geïmplementeerd. |
| Met behulp van Azure MFA met RADIUS-verificatie | Een [Network Policy Server (NPS)](howto-mfa-nps-extension.md) wordt geïmplementeerd. |
| Gebruikers hebben Microsoft Office 2010 of ouder of Apple Mail voor iOS 11 of eerder | Een upgrade uitvoeren naar [Microsoft Office 2013 of later](https://support.microsoft.com/help/4041439/modern-authentication-configuration-requirements-for-transition-from-o) en Apple mail voor iOS 12 of hoger. Voorwaardelijke toegang wordt niet ondersteund door verouderde verificatieprotocollen. |

## <a name="plan-user-rollout"></a>Gebruiker-implementatie plannen

Uw MFA-rollout-plan moet bevatten een testimplementatie gevolgd door de implementatie-blokken die zich binnen de capaciteit van de ondersteuning. Begin uw implementatie door het beleid voor voorwaardelijke toegang toepassen op een kleine groep testgebruikers. Nadat u het effect op de testgebruikers, proces dat wordt gebruikt en gedrag van de registratie, kunt u meer groepen toevoegen aan het beleid of meer gebruikers toevoegen aan de bestaande groepen.

### <a name="user-communications"></a>Communicatie van de gebruiker

Het is essentieel om gebruikers te informeren, in de geplande communicatie over toekomstige wijzigingen, vereisten voor Azure MFA-registratie en eventuele benodigde gebruikersacties. Het is raadzaam om de communicatie zijn ontwikkeld samen met vertegenwoordigers van binnen uw organisatie, zoals een afdelingen communicatie, wijzigingsbeheer en Human Resources.

Microsoft biedt [communicatiesjablonen](https://aka.ms/mfatemplates) en [documentatie voor eindgebruikers](../user-help/security-info-setup-signin.md) om u te helpen bij het ontwerp van uw communicatie. U kunt gebruikers verzenden [ https://myprofile.microsoft.com ](https://myprofile.microsoft.com) registreren rechtstreeks door het selecteren van de **beveiligingsgegevens** koppelingen op deze pagina.

## <a name="deployment-considerations"></a>Overwegingen voor implementatie

Azure multi-factor Authentication wordt geïmplementeerd door het afdwingen van beleid met voorwaardelijke toegang. Een [beleid voor voorwaardelijke toegang](../conditional-access/overview.md) kunnen vereisen dat gebruikers multi-factor authentication uitvoeren wanneer aan bepaalde criteria wordt voldaan, zoals:

* Alle gebruikers, een specifieke gebruiker, lid zijn van een groep of toegewezen rol
* Specifieke cloudtoepassing waartoe toegang wordt verkregen
* Apparaatplatform
* Status van apparaat
* Netwerklocatie of zich geo IP-adres
* Clienttoepassingen
* Aanmeldingsrisico (Identity Protection is vereist)
* Compatibel apparaat
* Hybrid Azure AD gekoppelde apparaat
* Goedgekeurde client-toepassing

Beleid voor voorwaardelijke toegang afdwingen registratie, niet-geregistreerde gebruikers te verlangen dat registratie op de eerste aanmelding wordt een belangrijke veiligheidsoverweging voltooien.

[Azure AD Identity Protection](../identity-protection/howto-configure-risk-policies.md) deel uitmaakt van een registratiebeleid voor zowel geautomatiseerde risico's detecteren en herstellen van beleid voor het verhaal van de Azure multi-factor Authentication. Beleidsregels kunnen worden gemaakt om af te dwingen wachtwoord te wijzigen wanneer er een bedreiging van identiteit waarmee is geknoeid of MFA is vereist wanneer een aanmelding wordt beschouwd als riskant door de volgende [gebeurtenissen](../reports-monitoring/concept-risk-events.md):

* Gelekte referenties
* Aanmeldingen vanaf anonieme IP-adressen
* Onmogelijke reis naar ongewone locaties
* Aanmeldingen vanaf onbekende locaties
* Aanmeldingen vanaf geïnfecteerde apparaten
* Aanmeldingen vanaf IP-adressen met verdachte activiteiten

Sommige van de risicogebeurtenissen die worden gedetecteerd door Azure Active Directory Identity Protection in realtime plaatsvinden en sommige offline verwerking vereist. Beheerders kunnen kiezen om gebruikers die riskant gedrag vertonen en handmatig herstellen, een wachtwoordwijziging vereisen of een multi-factor authentication vereist als onderdeel van hun beleid voor voorwaardelijke toegang te blokkeren.

## <a name="define-network-locations"></a>Netwerklocaties definiëren

Het wordt aangeraden dat organisaties gebruik van voorwaardelijke toegang voor het definiëren van hun netwerk met [benoemde locaties](../conditional-access/location-condition.md#named-locations). Als uw organisatie Identity Protection gebruikt, kunt u overwegen risico's gebaseerd beleid in plaats van benoemde locaties.

### <a name="configuring-a-named-location"></a>Een benoemde locatie configureren

1. Open **Azure Active Directory** in Azure portal
2. Klik op **voorwaardelijke toegang**
3. Klik op **benoemde locaties**
4. Klik op **nieuwe locatie**
5. In de **naam** veld, Geef een beschrijvende naam
6. Selecteer of u de locatie met IP-bereiken of de landen/regio's definieert
   1. Als u IP-bereiken
      1. Bepaal of u de locatie als vertrouwde markeren. Aanmelden van een vertrouwde benoemde locaties verlaagt het aanmeldingsrisico van een gebruiker. Deze locatie als vertrouwde alleen markeren als u weet dat de IP-bereiken tot stand gebrachte en betrouwbaar zijn in uw organisatie.
      2. Geef de IP-adresbereiken
   2. Als u met behulp van de landen/regio 's
      1. Vouw de vervolgkeuzelijst en selecteer de landen of regio's die u wilt definiëren voor deze locatie met de naam.
      2. Bepaal of onbekende gebieden opnemen. Onbekende gebieden zijn IP-adressen die niet kunnen worden toegewezen aan een land/regio.
7. Klik op **Maken**.

## <a name="plan-authentication-methods"></a>Verificatiemethoden plannen

Beheerders kunnen kiezen om de [verificatiemethoden](../authentication/concept-authentication-methods.md) dat ze beschikbaar wilt maken voor gebruikers. Het is belangrijk om toe te staan meer dan een methode voor verificatie met eenmalige zodat gebruikers beschikken over een back-upmethode beschikbaar als hun primaire methode niet beschikbaar is. De volgende methoden zijn beschikbaar voor beheerders om in te schakelen:

### <a name="notification-through-mobile-app"></a>Melding via mobiele app

Een pushmelding wordt verzonden naar de Microsoft Authenticator-app op uw mobiele apparaat. De gebruiker de melding weergaven en **goedkeuren** om verificatie te voltooien. Pushmeldingen verzenden via een mobiele app bieden de minste firewallverbinding mogelijkheid voor gebruikers. Ze zijn ook de meest betrouwbare en veilige optie omdat deze een gegevensverbinding in plaats van telefonie gebruiken.

### <a name="verification-code-from-mobile-app"></a>Verificatiecode via mobiele app

Een mobiele app, zoals de Microsoft Authenticator-app genereert een nieuwe OATH-verificatiecode elke 30 seconden. De gebruiker voert de verificatiecode in de interface van aanmelding. De optie mobiele app kan worden gebruikt of de telefoon wel een gegevens- of mobiele signaal of niet.

### <a name="call-to-phone"></a>Bellen naar telefoon

Een geautomatiseerd telefoongesprek wordt de gebruiker geplaatst. De gebruiker beantwoordt het gesprek en persen **#** op de toetsenblok van de telefoon om goed te keuren van de verificatie. De aanroep naar telefoon is een goede back-upmethode voor melding of verificatie via een mobiele app.

### <a name="text-message-to-phone"></a>Sms-bericht naar telefoon

Een SMS-bericht met een verificatiecode naar de gebruiker wordt verzonden, de gebruiker wordt gevraagd de verificatiecode invoeren in de interface van aanmelding.

### <a name="choose-verification-options"></a>Verificatie-opties kiezen

1. Blader naar **Azure Active Directory**, **gebruikers**, **multi-Factor Authentication**.

   ![Toegang tot de multi-factor Authentication-portal van Azure AD-gebruikers-blade in Azure portal](media/howto-mfa-getstarted/users-mfa.png)

1. Blader in het nieuwe tabblad dat geopend naar **service-instellingen**.
1. Onder **verificatieopties**, schakel alle selectievakjes in voor de methoden die beschikbaar zijn voor gebruikers.

   ![Verificatiemethoden configureren in het tabblad instellingen van multi-factor Authentication-service](media/howto-mfa-getstarted/mfa-servicesettings-verificationoptions.png)

1. Klik op **Opslaan**.
1. Sluit de **service-instellingen** tabblad.

## <a name="plan-registration-policy"></a>Registratiebeleid plannen

Beheerders moeten bepalen hoe gebruikers hun methoden worden geregistreerd. Organisaties moeten [inschakelen van de nieuwe ervaring voor de registratie van de gecombineerde](howto-registration-mfa-sspr-combined.md) voor Azure MFA en Self-service voor wachtwoord opnieuw instellen (SSPR). SSPR kan gebruikers hun wachtwoord opnieuw instellen op een veilige manier met behulp van de methodes die ze voor meervoudige verificatie gebruiken. We raden dat deze registratie, momenteel in openbare preview, gecombineerd, omdat het is een fantastische ervaring voor gebruikers met de mogelijkheid om één keer registreren voor beide services. Inschakelen van dezelfde methoden voor SSPR en Azure MFA, kunnen uw gebruikers worden geregistreerd voor beide functies gebruiken.

### <a name="registration-with-identity-protection"></a>Registratie met Identity Protection

Als uw organisatie van Azure Active Directory Identity Protection gebruikmaakt, [configureren van de MFA-registratiebeleid](../identity-protection/howto-mfa-policy.md) dat uw gebruikers zich voor het registreren van de volgende keer dat ze zich interactief aanmelden.

### <a name="registration-without-identity-protection"></a>Inschrijving zonder identity Protection

Als uw organisatie heeft geen licenties met Identity Protection inschakelen, wordt gebruikers gevraagd om u te registreren van de volgende keer dat MFA vereist bij het aanmelden is. Gebruikers kunnen niet worden geregistreerd voor MFA als ze de toepassingen die zijn beveiligd met MFA niet gebruikt. Het is belangrijk om alle gebruikers geregistreerd, zodat beveiligingsrisico kan het wachtwoord van een gebruiker raden en zich registreren voor MFA namens hen effectief nodig de controle van het account.

#### <a name="enforcing-registration"></a>Afdwingen van inschrijving

Met behulp van de volgende stappen uit een beleid voor voorwaardelijke toegang kunt afdwingen dat gebruikers zich registreren voor meervoudige verificatie

1. Maak een groep, voeg alle gebruikers die momenteel niet geregistreerd.
2. Door middel van voorwaardelijke toegang, multi-factor authentication voor deze groep voor toegang tot alle resources afdwingen.
3. Periodiek, Evalueer het groepslidmaatschap en gebruikers die zich hebben geregistreerd verwijderen uit de groep.

Kan voor het identificeren van geregistreerde en niet-geregistreerde gebruikers van Azure MFA met PowerShell-opdrachten die afhankelijk van zijn de [MSOnline PowerShell-module](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0).

#### <a name="identify-registered-users"></a>Geregistreerde gebruikers identificeren

```PowerShell
Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName | Sort-Object userprincipalname 
```

#### <a name="identify-non-registered-users"></a>Niet-geregistreerde gebruikers identificeren

```PowerShell
Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName | Sort-Object userprincipalname 
```

## <a name="plan-conditional-access-policies"></a>Beleid voor voorwaardelijke toegang plannen

Plan de strategie voor het beleid van voorwaardelijke toegang, zodat u wanneer MFA en andere besturingselementen vereist bepalen kunt zijn, als u wilt verwijzen naar [wat is voorwaardelijke toegang in Azure Active Directory?](../conditional-access/overview.md).

Het is belangrijk dat u voorkomen dat per ongeluk toegang tot uw Azure AD-tenant wordt geblokkeerd. U kunt beperken de gevolgen van deze onbedoeld gebrek aan beheerderstoegang door [twee of meer accounts voor Noodtoegang maken in uw tenant](../users-groups-roles/directory-emergency-access.md) en worden deze uitgesloten van uw beleid voor voorwaardelijke toegang.

### <a name="create-conditional-access-policy"></a>Beleid voor voorwaardelijke toegang maken

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een globale beheerdersaccount.
1. Blader naar **Azure Active Directory**, **voorwaardelijke toegang**.
1. Selecteer **nieuw beleid**.
1. Geef een beschrijvende naam voor uw beleid.
1. Onder **gebruikers en groepen**:
   * Op de **opnemen** tabblad de **alle gebruikers** keuzerondje
   * Op de **uitsluiten** tabblad, schakel het selectievakje voor **gebruikers en groepen** en kiest u uw accounts voor toegang in noodgevallen.
   * Klik op **Gereed**.
1. Onder **Cloud-apps**, selecteer de **alle cloud-apps** keuzerondje.
   * (OPTIONEEL): Op de **uitsluiten** tabblad, kiest u cloud-apps die uw organisatie geen MFA voor vereist.
   * Klik op **Gereed**.
1. Onder **voorwaarden** sectie:
   * (OPTIONEEL): Als u Azure Identity Protection hebt ingeschakeld, kunt u aanmeldingsrisico beoordelen als onderdeel van het beleid.
   * (OPTIONEEL): Als u de vertrouwde locaties hebt geconfigureerd of benoemde locaties, u opgeven kunt als u wilt opnemen of uitsluiten van deze locaties van het beleid.
1. Onder **verlenen**, zorg ervoor dat de **toegang verlenen** keuzerondje is geselecteerd.
    * Schakel het selectievakje voor **meervoudige verificatie vereisen**.
    * Klik op **Selecteren**.
1. Overslaan de **sessie** sectie.
1. Stel de **beleid inschakelen** overzet naar **op**.
1. Klik op **Create**.

![Maken van een beleid voor voorwaardelijke toegang voor MFA inschakelen voor gebruikers van de Azure portal in de testfasegroep](media/howto-mfa-getstarted/conditionalaccess-newpolicy.png)

## <a name="plan-integration-with-on-premises-systems"></a>Integratie met on-premises systemen plannen

Sommige oudere en on-premises toepassingen die niet rechtstreeks aan de hand Azure AD verifiëren vereisen voor gebruik met inbegrip van MFA extra stappen uitvoeren:

* Verouderde on-premises toepassingen, die moet Application proxy gebruiken.
* On-premises RADIUS-toepassingen, die u moeten het gebruik van MFA-adapter met NPS-server.
* On-premises AD FS toepassingen, die het gebruik van MFA-adapter met AD FS 2016 moet.

Toepassingen die rechtstreeks met Azure AD verifiëren en moderne verificatie (WS-Federation, SAML, OAuth, OpenID Connect) kunnen u rechtstreeks van beleid voor voorwaardelijke toegang gebruiken.

### <a name="use-azure-mfa-with-azure-ad-application-proxy"></a>Azure MFA gebruiken met Azure AD-toepassingsproxy

Toepassingen die on-premises kunnen worden gepubliceerd naar uw Azure AD-tenant via [Azure AD-toepassingsproxy](../manage-apps/application-proxy.md) en kunnen profiteren van Azure multi-factor Authentication als ze zijn geconfigureerd voor het gebruik van Azure AD verificatie vooraf.

Deze toepassingen zijn onderworpen aan beleid voor voorwaardelijke toegang waarmee Azure multi-factor Authentication, net als elke andere Azure AD geïntegreerde toepassing worden afgedwongen.

Op dezelfde manier als Azure multi-factor Authentication wordt afgedwongen voor alle gebruikers zich aangemeld, on-premises toepassingen die zijn gepubliceerd met Azure AD Application Proxy worden beveiligd.

### <a name="integrating-azure-multi-factor-authentication-with-network-policy-server"></a>Integratie van Azure multi-factor Authentication met Network Policy Server

De Network Policy Server (NPS)-extensie voor Azure MFA voegt cloud-gebaseerde MFA functies aan uw infrastructuur voor verificatie met behulp van uw bestaande servers. Met de NPS-extensie kunt u telefonische oproepen, SMS-bericht of verificatie via de telefoon-app kunt toevoegen aan uw bestaande verificatiestroom. Deze integratie heeft de volgende beperkingen:

* Met het protocol CHAPv2 worden alleen verificator-app pushmeldingen te verzenden en telefoongesprek ondersteund.
* Beleid voor voorwaardelijke toegang kunnen niet worden toegepast.

De NPS-extensie fungeert als een adapter tussen RADIUS- en cloud-gebaseerde Azure MFA voor een tweede factor van de verificatie te beschermen [VPN](howto-mfa-nps-extension-vpn.md), [verbindingen van extern bureaublad-Gateway](howto-mfa-nps-extension-rdg.md), of andere RADIUS-staat toepassingen. Gebruikers die voor alle verificatiepogingen registreren voor Azure MFA in deze omgeving wordt gevraagd, het ontbreken van voorwaardelijk beleid gemiddelde MFA is altijd vereist.

#### <a name="implementing-your-nps-server"></a>Implementatie van de NPS-Server

Als u een NPS-exemplaar geïmplementeerd en wordt gebruikt, verwijzen naar [uw bestaande NPS-infrastructuur integreren met Azure multi-factor Authentication](howto-mfa-nps-extension.md). Als u NPS voor het eerst instelt, raadpleegt u [Network Policy Server (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) voor instructies. Richtlijnen voor probleemoplossing vindt u in het artikel [foutberichten van de NPS-extensie voor Azure multi-factor Authentication oplossen](howto-mfa-nps-extension-errors.md).

#### <a name="prepare-nps-for-users-that-arent-enrolled-for-mfa"></a>NPS voorbereiden voor gebruikers die niet zijn geregistreerd voor MFA

Kies wat er gebeurt als gebruikers die niet zijn geregistreerd met MFA probeert te verifiëren. Gebruik de registerinstelling `REQUIRE_USER_MATCH` in het registerpad `HKLM\Software\Microsoft\AzureMFA` om te bepalen het gedrag van de functie. Deze instelling heeft een configuratie voor één optie.

| Sleutel | Value | Standaard |
| --- | --- | --- |
| `REQUIRE_USER_MATCH` | DE WAARDE TRUE / FALSE | Niet ingesteld (gelijk aan TRUE) |

Het doel van deze instelling is om te bepalen wat er gebeurt wanneer een gebruiker is niet geregistreerd voor MFA. De gevolgen van het wijzigen van deze instelling worden weergegeven in de onderstaande tabel.

| Instellingen | Gebruiker MFA-Status | Effecten |
| --- | --- | --- |
| Sleutel bestaat niet | Niet geregistreerd | MFA-controle is mislukt |
| De waarde ingesteld op True / niet ingesteld | Niet geregistreerd | MFA-controle is mislukt |
| Sleutel ingesteld op False | Niet geregistreerd | Verificatie zonder MFA |
| Sleutel ingesteld op False of True | Ingeschreven | Moet worden geverifieerd met MFA |

### <a name="integrate-with-active-directory-federation-services"></a>Integreren met Active Directory Federatieservices

Als uw organisatie is gefedereerd met Azure AD, kunt u [Azure multi-factor Authentication AD FS-serviceresources beveiligen](multi-factor-authentication-get-started-adfs.md), zowel on-premises en in de cloud. Azure MFA kunt u beperken van wachtwoorden en bieden een veiliger manier om te verifiëren. Beginnen met Windows Server 2016, kunt u Azure MFA configureren voor primaire verificatie.

In tegenstelling tot met AD FS in Windows Server 2012 R2, de AD FS 2016 Azure MFA-adapter wordt rechtstreeks geïntegreerd met Azure AD en een on-premises Azure MFA-server is niet vereist. De Azure MFA-adapter is ingebouwd in Windows Server 2016 en is niet nodig voor een aanvullende informatie.

Wanneer met behulp van Azure MFA met AD FS 2016 en de doeltoepassing onderworpen aan beleid voor voorwaardelijke toegang is, zijn er aanvullende overwegingen:

* Voorwaardelijke toegang is beschikbaar wanneer de toepassing een relying party naar Azure AD is, gefedereerd met AD FS 2016.
* Voorwaardelijke toegang is niet beschikbaar wanneer de toepassing is van een relying party met AD FS 2016 en wordt beheerd of gefedereerd met AD FS 2016.
* Voorwaardelijke toegang is ook niet beschikbaar wanneer AD FS 2016 is geconfigureerd voor gebruik van Azure MFA als primaire verificatiemethode.

#### <a name="ad-fs-logging"></a>AD FS-logboekregistratie

Standaard AD FS 2016 logboekregistratie in zowel het beveiligingslogboek van Windows en het logboek voor AD FS-beheerder, bevat informatie over aanvragen voor authenticatie en hun slagen of mislukken. Logboekgegevens over gebeurtenissen in de volgende gebeurtenissen wordt aangegeven of de Azure MFA is gebruikt. Bijvoorbeeld, kan AD FS-controle-gebeurtenis-ID 1200 bevatten:

```
<MfaPerformed>true</MfaPerformed>
<MfaMethod>MFA</MfaMethod>
```

#### <a name="renew-and-manage-certificates"></a>Vernieuwen en certificaten beheren

Op elke AD FS-server in de lokale computer mijn Store, zal er een zelf-ondertekend Azure MFA-certificaat met de titel organisatie-eenheid = Microsoft AD FS Azure MFA, die de vervaldatum van het certificaat bevat. Controleer de geldigheidsperiode van dit certificaat op elke AD FS-server om te bepalen van de vervaldatum.

Als de geldigheidsperiode van de certificaten is bijna verlopen, [genereren en te verifiëren van een nieuwe MFA-certificaat op elke AD FS-server](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa#configure-the-ad-fs-servers).

De volgende richtlijnen om details over het beheren van de Azure MFA-certificaten op uw AD FS-servers. Wanneer u AD FS met Azure MFA configureren, de certificaten die is gegenereerd via de `New-AdfsAzureMfaTenantCertificate` PowerShell-cmdlet gedurende twee jaar geldig zijn. Vernieuwen en installeer het verlengde certificaat vóór de vervaldatum op ovoid onderbrekingen in de MFA-service.

## <a name="implement-your-plan"></a>Uw abonnement implementeren

Nu dat u uw oplossing hebt gepland, kunt u implementeren door de onderstaande stappen te volgen:

1. Voldoen aan alle benodigde voorwaarden
   1. Implementeer [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) voor een hybride scenario's
   1. Implementeren [Azure AD-toepassingsproxy](../manage-apps/application-proxy.md) voor op een on-premises toepassingen gepubliceerd voor cloudtoegang
   1. Implementeer [NPS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top) voor de RADIUS-verificatie
   1. Zorg ervoor dat gebruikers een upgrade hebt uitgevoerd op ondersteunde versies van Microsoft Office met moderne verificatie ingeschakeld
1. Configureren van de door u gekozen [verificatiemethoden](#choose-verification-options)
1. Definieer uw [met de naam netwerklocaties](../conditional-access/location-condition.md#named-locations)
1. Selecteer groepen om te beginnen met het uitrollen van MFA.
1. Configureer uw [beleid voor voorwaardelijke toegang](#create-conditional-access-policy)
1. Configureren van uw MFA-registratiebeleid
   1. [Gecombineerde MFA en Self-service voor Wachtwoordherstel](howto-registration-mfa-sspr-combined.md)
   1. Met [Identity Protection](../identity-protection/howto-mfa-policy.md)
1. Communicatie van de gebruiker te verzenden en gebruikers te registreren door naar [https://aka.ms/mfasetup](https://aka.ms/mfasetup)
1. [Bijhouden van die geregistreerd](#identify-non-registered-users)

## <a name="manage-your-solution"></a>Uw oplossing beheren

Rapporten voor Azure MFA

Azure multi-factor Authentication biedt rapporten via de Azure-portal:

| Rapport | Locatie | Description |
| --- | --- | --- |
| Gebruik en fraude waarschuwingen | Azure AD > Sign-ins | Bevat informatie over algemene gebruik, overzicht van gebruikers en de details van de gebruiker; Als een geschiedenis van Fraudewaarschuwingen die zijn ingediend in het opgegeven datumbereik. |

## <a name="troubleshoot-mfa-issues"></a>Problemen met MFA

Zoek oplossingen voor veelvoorkomende problemen met Azure MFA op de [artikel voor het oplossen van Azure multi-factor Authentication](https://support.microsoft.com/help/2937344/troubleshooting-azure-multi-factor-authentication-issues) op het Microsoft Support Center.

## <a name="next-steps"></a>Volgende stappen

* [Wat zijn verificatiemethoden?](concept-authentication-methods.md)
* [Geconvergeerde registratie voor meervoudige verificatie en Azure AD Self-service voor wachtwoord opnieuw instellen inschakelen](concept-registration-mfa-sspr-converged.md)
* Waarom is een gebruiker wordt gevraagd of niet wordt gevraagd om uit te voeren van MFA? Zie de sectie [rapport van Azure AD-aanmeldingen in de rapporten in de Azure multi-factor Authentication-document](howto-mfa-reporting.md#azure-ad-sign-ins-report).