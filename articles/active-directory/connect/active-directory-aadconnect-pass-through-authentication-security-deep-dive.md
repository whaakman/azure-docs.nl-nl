---
title: Azure Active Directory-verificatie voor Pass Through-beveiliging diepgaand | Microsoft Docs
description: Dit artikel wordt beschreven hoe uw lokale accounts in Azure Active Directory (Azure AD) Pass through-verificatie worden beveiligd
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, install Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: billmath
ms.openlocfilehash: 84a5ef23739635ba4d2f0adc688c1b506f643a36
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-pass-through-authentication-security-deep-dive"></a>Azure Active Directory-verificatie voor Pass Through-beveiliging diepgaand

In dit artikel biedt een gedetailleerdere beschrijving van de werking van Azure Active Directory (Azure AD) Pass through-verificatie. Dit artikel gaat over de beveiligingsaspecten van de functie. In dit artikel is bedoeld voor beveiliging en IT-beheerders, chief naleving en beveiliging managers en andere IT-professionals die verantwoordelijk voor IT-beveiliging en naleving in kleine tot middelgrote zijn organisaties grootte of grote ondernemingen.

De onderwerpen zijn onder andere:
- Gedetailleerde technische informatie over het installeren en registreren van de verificatie-Agents.
- Gedetailleerde technische informatie over de versleuteling van wachtwoorden tijdens het aanmelden van gebruikers.
- De beveiliging van de kanalen tussen on-premises verificatie Agents en Azure AD.
- Gedetailleerde technische informatie over hoe u de verificatie-Agents operationeel beveiligen.
- Andere onderwerpen die betrekking hebben op beveiliging.

## <a name="key-security-capabilities"></a>Belangrijkste beveiligingsmogelijkheden

Dit zijn de sleutelbeveiliging aspecten van deze functie:
- Dit gebaseerd op een veilige multi-verpachte architectuur die isolatie van aanmelden aanvragen tussen de tenants biedt.
- On-premises wachtwoorden worden nooit opgeslagen in de cloud in een formulier.
- Lokale verificatie-Agents die luisteren naar en reageren op, wachtwoord validatie aanvragen alleen uitgaande verbindingen van binnen uw netwerk maken. Er is geen vereiste voor het installeren van deze verificatie-Agents in een perimeternetwerk (DMZ genoemd).
- Alleen standaard poorten (80 en 443) worden gebruikt voor uitgaande communicatie van de verificatie-Agents naar Azure AD. U hoeft niet te openen van poorten voor inkomend verkeer op uw firewall. 
  - Poort 443 wordt gebruikt voor alle geverifieerde uitgaande communicatie.
  - Poort 80 wordt alleen voor het downloaden van de certificaatintrekkingslijsten (CRL's) gebruikt om ervoor te zorgen dat geen van de certificaten die door deze functie is ingetrokken.
  - Zie voor de volledige lijst van de netwerkvereisten [Azure Active Directory Pass-through-verificatie: snel starten](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-the-prerequisites).
- Wachtwoorden die gebruikers tijdens het aanmelden opgeven worden gecodeerd in de cloud voordat de Agents van lokale verificatie ermee akkoord bent gegaan voor validatie op basis van Active Directory.
- Het HTTPS-kanaal tussen Azure AD en de lokale verificatie-Agent is beveiligd met behulp van wederzijdse verificatie.
- De functie integreert naadloos met Azure AD cloud beveiliging mogelijkheden, zoals beleidsregels voor voorwaardelijke toegang (met inbegrip van Azure multi-factor Authentication), bescherming van identiteiten en slimme vergrendeling.

## <a name="components-involved"></a>Onderdelen betrokken

Raadpleeg voor algemene informatie over operationele, Azure AD-service en de beveiliging van gegevens, de [Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/). De volgende onderdelen betrokken zijn bij u Pass-through-verificatie voor aanmelding bij de gebruiker gebruiken:
- **Azure AD-STS**: een stateless beveiligingstokenservice (STS) die aanmelden verzoeken verwerkt en verstrekt beveiligingstokens aan gebruikers browsers, clients of services zoals vereist.
- **Azure Service Bus**: biedt ingeschakeld voor de cloud communicatie met berichtenverzending voor bedrijven en relays communicatie waarmee u oplossingen on-premises-verbinding met de cloud.
- **Azure AD Connect verificatie Agent**: een onderdeel van de lokale die luistert naar en reageert op aanvragen van wachtwoord validatie.
- **Azure SQL Database**: bevat informatie over uw tenant verificatie Agents, inclusief de codes metagegevens en versleuteling.
- **Active Directory**: On-premises Active Directory, waar de gebruikersaccounts en hun wachtwoorden worden opgeslagen.

## <a name="installation-and-registration-of-the-authentication-agents"></a>Installatie en registratie van de verificatie-Agents

Verificatie-Agents zijn geïnstalleerd en geregistreerd met Azure AD wanneer u beide:
   - [Inschakelen van Pass through-verificatie via Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-2-enable-the-feature)
   - [Meer verificatie Agents om te controleren of de hoge beschikbaarheid van aanmeldingsaanvragen toevoegen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-4-ensure-high-availability) 
   
Ophalen van een verificatie-Agent werkt bestaat uit drie belangrijkste fasen:

1. De installatie van de verificatie-Agent
2. Agentregistratie verificatie
3. De initialisatie van de verificatie-Agent

De volgende secties worden deze fasen in detail behandeld.

### <a name="authentication-agent-installation"></a>De installatie van de verificatie-Agent

Alleen globale beheerders kunnen een verificatie-Agent (met behulp van Azure AD Connect of zelfstandig) installeren op een on-premises server. Installatie voegt twee nieuwe vermeldingen voor de **Configuratiescherm** > **programma's** > **programma's en onderdelen** lijst:
- De verificatie-Agent-toepassing zelf. Deze toepassing wordt uitgevoerd met [NetworkService](https://msdn.microsoft.com/library/windows/desktop/ms684272.aspx) bevoegdheden.
- De Updater-toepassing die wordt gebruikt voor de verificatie-Agent automatisch bijwerken. Deze toepassing wordt uitgevoerd met [LocalSystem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) bevoegdheden.

### <a name="authentication-agent-registration"></a>Agentregistratie verificatie

Nadat u de verificatie-Agent hebt geïnstalleerd, moet deze zelf registreren met Azure AD. Azure AD wijst elke Agent verificatie een unieke, digitale identiteit certificaat die kan worden gebruikt voor veilige communicatie met Azure AD.

De registratieprocedure koppelt u ook de verificatie-Agent met uw tenant. Dit zorgt ervoor dat Azure AD weet dat deze specifieke verificatie-Agent de enige is geautoriseerd voor het afhandelen van wachtwoord validatie aanvragen voor uw tenant. Deze procedure wordt herhaald voor elke nieuwe verificatie-Agent registreren.

De verificatie-Agents gebruik de volgende stappen kunnen worden geregistreerd met Azure AD:

![Het registreren van agent](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta1.png)

1. Azure AD-eerst aanvragen dat een globale beheerder aanmelden bij Azure AD met hun referenties. Tijdens het aanmelden verkrijgt de verificatie-Agent een toegangstoken die namens de hoofdbeheerder kan worden gebruikt.
2. De verificatie-Agent genereert vervolgens een sleutelpaar: een openbare sleutel en een persoonlijke sleutel.
    - Het sleutelpaar wordt gegenereerd via standaard RSA 2048-bits codering.
    - De persoonlijke sleutel blijft op de lokale server waarop de verificatie-Agent zich bevindt.
3. De verificatie-Agent maakt een registratieaanvraag voor "" naar Azure AD via HTTPS, met de volgende onderdelen opgenomen in de aanvraag:
    - Het toegangstoken is verkregen in stap 1.
    - De openbare sleutel die wordt gegenereerd in stap 2.
    - Een aanvraag voor Certificaatondertekening (CSR of certificaataanvraag). Deze aanvraag geldt voor een certificaat digitale identiteiten met Azure AD als de certificeringsinstantie (CA).
4. Azure AD valideert het toegangstoken in de aanvraag voor functieregistratie en wordt gecontroleerd of de aanvraag afkomstig van een globale beheerder is.
5. Azure AD vervolgens ondertekent en een certificaat van de digitale identiteit wordt teruggestuurd naar de verificatie-Agent.
    - De basis-CA in Azure AD wordt gebruikt voor het certificaat ondertekent. 

     >[!NOTE]
     > Deze CA is _niet_ opslaan in de Windows vertrouwde basiscertificeringsinstanties.
    - De CA wordt alleen door de functie Pass through-verificatie gebruikt. De CA wordt alleen aan te melden CSR's tijdens de registratie van de verificatie-Agent gebruikt.
    -  Geen van de Azure AD-services gebruiken deze CA.
    - De certificaathouder (DN-naam of DN-naam) is ingesteld op uw tenant-ID. Deze DN-naam is een GUID die een unieke identificatie van uw tenant. Deze DN begrenst u het certificaat alleen voor gebruik met uw tenant.
6. Azure AD slaat de openbare sleutel van de verificatie-Agent in een Azure SQL-database, die alleen Azure AD toegang tot heeft.
7. Het certificaat (uitgegeven in stap 5) wordt opgeslagen op de lokale server in het Windows-certificaatarchief (specifiek in de [CERT_SYSTEM_STORE_LOCAL_MACHINE](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_LOCAL_MACHINE) locatie). Deze wordt gebruikt door zowel de verificatie-Agent en de Updater-toepassingen.

### <a name="authentication-agent-initialization"></a>De initialisatie van de verificatie-Agent

Wanneer de verificatie-Agent wordt gestart, voor de eerste keer na de registratie of na een server opnieuw opstarten, moet een manier om veilig te communiceren met de Azure AD-service en start wachtwoord validatie aanvragen te accepteren.

![De initialisatie van agent](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta2.png)

Hier ziet u hoe verificatie Agents zijn geïnitialiseerd:

1. De verificatie-Agent maakt een uitgaande bootstrap-aanvraag naar Azure AD. 
    - Deze aanvraag wordt via poort 443 en wordt via een wederzijds verifieerbaar HTTPS-kanaal. De aanvraag gebruikt hetzelfde certificaat dat is uitgegeven tijdens de registratie van de verificatie-Agent.
2. Azure AD reageert op de aanvraag door een toegangssleutel naar een Azure Service Bus-wachtrij die uniek is voor uw tenant en die wordt geïdentificeerd door uw tenant-ID.
3. De verificatie-Agent maakt een permanente uitgaande HTTPS-verbinding (via poort 443) naar de wachtrij. 
    - De verificatie-Agent is nu gereed om te halen en het valideren van het wachtwoord van aanvragen.

Als u hebt meerdere verificatie-Agents op uw tenant is geregistreerd, wordt de van de initialisatieprocedure zorgt ervoor dat elk criterium verbinding met dezelfde Service Bus-wachtrij maakt. 

## <a name="process-sign-in-requests"></a>Aanvragen voor processen aanmelden 

Het volgende diagram toont hoe Pass through-verificatie gebruiker aanmelden aanvragen verwerkt.

![Proces van aanmeldingspagina](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta3.png)

Pass through-verificatie verwerkt een gebruikersaanvraag aanmelden als volgt: 

1. Een gebruiker probeert te krijgen tot een toepassing bijvoorbeeld [Outlook Web App](https://outlook.office365.com/owa).
2. Als de gebruiker niet is al aangemeld, wordt de browser in de toepassing omgeleid naar de aanmeldingspagina van Azure AD.
3. De service Azure AD-STS reageert back maken met de **gebruikersaanmelding** pagina.
4. De gebruiker invoert hun gebruikersnaam en wachtwoord in de **gebruikersaanmelding** pagina en selecteert de **aanmelden** knop.
5. De gebruikersnaam en wachtwoord worden verzonden naar Azure AD-STS in een HTTPS-POST-aanvraag.
6. Azure AD STS opgehaald van openbare sleutels voor alle verificatie-Agents die worden geregistreerd op de tenant van de Azure SQL database en het wachtwoord versleutelt met behulp van deze. 
    - De waarden "N" versleuteld wachtwoord produceert voor "N" verificatie-Agents op uw tenant is geregistreerd.
7. Azure AD STS plaatst de wachtwoord-validatieaanvraag uit de gebruikersnaam en de waarden van het versleutelde wachtwoord, naar de Service Bus-wachtrij die specifiek zijn voor uw tenant bestaat.
8. Omdat de geïnitialiseerde verificatie Agents permanent met de Service Bus-wachtrij verbonden zijn, een van de beschikbare Agents voor verificatie de wachtwoord-validatieaanvraag opgehaald.
9. De verificatie-Agent wordt gezocht naar de waarde van het versleutelde wachtwoord die specifiek is voor de openbare sleutel met behulp van een id en ontsleuteld met behulp van de persoonlijke sleutel.
10. De verificatie-Agent probeert te valideren van de gebruikersnaam en het wachtwoord op basis van de lokale Active Directory met behulp van de [Win32 LogonUser API](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx) met de **dwLogonType** parameter ingesteld op **LOGON32_LOGON_NETWORK**. 
    - Deze API is de dezelfde API die wordt gebruikt door Active Directory Federation Services (AD FS) aan te melden gebruikers in een scenario voor federatieve aanmelding.
    - Deze API is afhankelijk van het proces standaardresolutie in Windows Server om de domeincontroller te zoeken.
11. De verificatie-Agent ontvangt het resultaat van Active Directory, zoals geslaagd, gebruikersnaam of wachtwoord is onjuist of het wachtwoord verlopen.
12. De verificatie-Agent verzendt het resultaat terug naar Azure AD-STS via een uitgaande wederzijds verifieerbaar HTTPS-kanaal via poort 443. Het certificaat dat aan de verificatie-Agent tijdens de registratie eerder uitgegeven maakt gebruik van wederzijdse verificatie.
13. Azure AD STS controleert of dat dit resultaat met de aanvraag voor specifieke aanmelden op uw tenant correleert.
14. Azure AD STS gaat door met de procedure aanmelden zoals geconfigureerd. Bijvoorbeeld, als de wachtwoordvalidatie van het gelukt is, de gebruiker mogelijk gevraagd om multi-factor Authentication of terug naar de toepassing omgeleid.

## <a name="operational-security-of-the-authentication-agents"></a>Operationele beveiliging van de verificatie-Agents

Om ervoor te zorgen dat Pass through-verificatie operationeel veilige blijft, vernieuwt periodiek Azure AD Authentication Agents certificaten. Azure AD activeert de vernieuwingen. De vernieuwing niet worden geregeld door de verificatie-Agents zelf.

![Operationele beveiliging](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta4.png)

Om te verlengen vertrouwensrelatie van een verificatie-Agent met Azure AD:

1. De verificatie-Agent regelmatig pingt Azure AD om de paar uur om te controleren of het tijd om het certificaat te vernieuwen. 
    - Deze controle wordt uitgevoerd via een wederzijds verifieerbaar HTTPS-kanaal en maakt gebruik van hetzelfde certificaat dat is uitgegeven tijdens de registratie.
2. Als de service is het tijd om te vernieuwen aangeeft, de verificatie-Agent genereert een nieuw sleutelpaar: een openbare sleutel en een persoonlijke sleutel.
    - Deze sleutels worden gegenereerd door middel van standaard RSA 2048-bits codering.
    - De persoonlijke sleutel wordt nooit verlaten van de on-premises server.
3. De verificatie-Agent maakt vervolgens een 'certificaatvernieuwing'-aanvraag naar Azure AD via HTTPS, met de volgende onderdelen opgenomen in de aanvraag:
    - Het bestaande certificaat dat opgehaald uit de CERT_SYSTEM_STORE_LOCAL_MACHINE-locatie op het Windows-certificaatarchief. Er is geen hoofdbeheerder die zijn betrokken bij deze procedure, zodat er geen toegangstoken namens de hoofdbeheerder nodig is.
    - De openbare sleutel die wordt gegenereerd in stap 2.
    - Een aanvraag voor Certificaatondertekening (CSR of certificaataanvraag). Deze aanvraag is van toepassing voor een nieuw certificaat digitale identiteiten met Azure AD als de certificeringsinstantie.
4. Azure AD valideert het bestaande certificaat in de aanvraag voor certificaatvernieuwing. Vervolgens wordt gecontroleerd of de aanvraag afkomstig van een verificatie-Agent op uw tenant is geregistreerd is.
5. Als het bestaande certificaat nog geldig is, Azure AD vervolgens ondertekent het certificaat van een nieuwe digitale identiteit en problemen met het nieuwe certificaat terug naar de verificatie-Agent. 
6. Als het bestaande certificaat is verlopen, verwijderen Azure AD de verificatie-Agent van uw tenant-lijst van geregistreerde verificatie Agents. Een globale beheerder moet handmatig installeren en registreren van een nieuwe Agent voor verificatie.
    - De basis-CA van de Azure AD gebruiken voor het ondertekenen van het certificaat.
    - De certificaathouder (DN-naam of DN-naam) ingesteld op uw tenant-ID, een GUID die een unieke identificatie van uw tenant. Het certificaat in uw tenant, alleen de DN-bereiken.
6. Azure AD slaat de nieuwe openbare sleutel van de verificatie-Agent in een Azure SQL-database die alleen toegang heeft. Het wordt ook de oude openbare sleutel die is gekoppeld aan de verificatie-Agent wordt ongeldig gemaakt.
7. Het nieuwe certificaat (uitgegeven in stap 5) wordt vervolgens opgeslagen op de server in de Windows-certificaatarchief (specifiek in de [CERT_SYSTEM_STORE_CURRENT_USER](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_CURRENT_USER) locatie).
    - Omdat de vertrouwensrelatie vernieuwing procedure gebeurt niet-interactief (zonder de aanwezigheid van de globale beheerder), heeft de verificatie-Agent niet langer toegang tot het bestaande certificaat op de locatie CERT_SYSTEM_STORE_LOCAL_MACHINE bijwerken. 
    
   > [!NOTE]
   > Deze procedure verwijdert het certificaat zelf niet vanaf de locatie CERT_SYSTEM_STORE_LOCAL_MACHINE.
8. Het nieuwe certificaat wordt gebruikt voor verificatie vanaf dit punt op. Elke latere verlenging van het certificaat vervangt het certificaat op de locatie CERT_SYSTEM_STORE_LOCAL_MACHINE.

## <a name="auto-update-of-the-authentication-agents"></a>Automatisch bijwerken van de verificatie-Agents

De verificatie-Agent de toepassing Updater automatisch bijgewerkt wanneer er een nieuwe versie wordt uitgebracht. De toepassing verwerkt geen verzoeken validatie wachtwoord voor uw tenant. 

Azure AD als host fungeert voor de nieuwe versie van de software als een ondertekende **Windows Installer-pakket (MSI)**. Het MSI-bestand is ondertekend met behulp van [Microsoft Authenticode](https://msdn.microsoft.com/library/ms537359.aspx) met SHA256 als digest-algoritme. 

![Automatisch bijwerken](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta5.png)

Voor automatische updates een verificatie-Agent:

1. De Updater toepassing pings Azure AD elk uur wilt controleren of er een nieuwe versie van de verificatie-Agent beschikbaar. 
    - Deze controle wordt uitgevoerd via een wederzijds verifieerbaar HTTPS-kanaal met behulp van hetzelfde certificaat dat is uitgegeven tijdens de registratie. De verificatie-Agent en de Updater delen het certificaat dat is opgeslagen op de server.
2. Als een nieuwe versie beschikbaar is, retourneert Azure AD het ondertekende MSI-bestand naar de Updater.
3. De Updater verifieert dat het MSI-bestand is ondertekend door Microsoft.
4. De Updater voert het MSI-bestand. Deze actie omvat de volgende stappen:

 > [!NOTE]
 > De Updater wordt uitgevoerd met [lokaal systeem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) bevoegdheden.

    - De verificatie-Agent-service gestopt
    - Installeert de nieuwe versie van de verificatie-Agent op de server
    - De verificatie-Agent-service wordt opnieuw gestart

>[!NOTE]
>Als er meerdere verificatie-Agents op uw tenant is geregistreerd, Azure AD niet hun certificaten vernieuwen of deze op hetzelfde moment bijwerken. In plaats daarvan biedt Azure AD dus geleidelijk om te controleren of de hoge beschikbaarheid van aanmeldingsaanvragen.
>


## <a name="next-steps"></a>Volgende stappen
- [Huidige beperkingen](active-directory-aadconnect-pass-through-authentication-current-limitations.md): meer informatie over welke scenario's worden ondersteund en welke worden niet.
- [Snel starten](active-directory-aadconnect-pass-through-authentication-quick-start.md): actief en werkend krijgen voor Azure AD Pass-through-verificatie.
- [Vergrendelen van smartcard](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): de mogelijkheid slimme accountvergrendeling configureren op uw tenant gebruikersaccounts te beveiligen.
- [Hoe het werkt](active-directory-aadconnect-pass-through-authentication-how-it-works.md): Leer de basisbeginselen van de werking van Azure AD Pass-through-verificatie.
- [Veelgestelde vragen over](active-directory-aadconnect-pass-through-authentication-faq.md): Hier vindt u antwoorden op veelgestelde vragen.
- [Problemen met](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): informatie over het oplossen van veelvoorkomende problemen met de functie Pass through-verificatie.
- [Azure AD naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md): meer informatie over deze aanvullende functie.
