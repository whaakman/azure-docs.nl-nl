---
title: Azure Active Directory Pass through-verificatie grondig onderzoek van beveiliging | Microsoft Docs
description: Dit artikel wordt beschreven hoe uw on-premises-accounts worden beveiligd met Azure Active Directory (Azure AD) Pass through-verificatie
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, installatie van Active Directory, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 48710bc10a57854fcbd4ffbe44bc426333baddc0
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159226"
---
# <a name="azure-active-directory-pass-through-authentication-security-deep-dive"></a>Azure Active Directory Pass through-verificatie grondig onderzoek van beveiliging

Dit artikel bevat een gedetailleerde beschrijving van de werking van Azure Active Directory (Azure AD) Pass through-verificatie. Dit artikel gaat over de beveiligingsaspecten van de functie. In dit artikel is bedoeld voor beveiliging en IT-beheerders, chief naleving en security officers, en andere IT-professionals die verantwoordelijk voor IT-beveiliging en naleving in kleine tot middelgrote zijn organisaties grootte of grote ondernemingen.

De onderwerpen zijn onder andere:
- Gedetailleerde technische informatie over het installeren en registreren van de verificatie-Agents.
- Gedetailleerde technische informatie over de versleuteling van wachtwoorden tijdens het aanmelden van gebruikers.
- De beveiliging van de kanalen tussen on-premises verificatie-Agents en Azure AD.
- Gedetailleerde technische informatie over hoe u de verificatie-Agents operationeel om veilig te houden.
- Andere onderwerpen met betrekking tot beveiliging.

## <a name="key-security-capabilities"></a>Belangrijke beveiligingsmogelijkheden

Dit zijn de sleutelbeveiliging aspecten van deze functie:
- Het gebouwd op een veilige, multitenant architectuur die isolatie van aanmeldingsaanvragen tussen tenants biedt.
- On-premises wachtwoorden worden nooit opgeslagen in de cloud in welke vorm.
- On-premises verificatie-Agents die luisteren naar en reageren op, wachtwoord validatie aanvragen alleen uitgaande verbindingen in uw netwerk maken. Er is geen vereiste voor het installeren van deze verificatie-Agents in een perimeternetwerk (DMZ).
- Alleen standaard poorten (80 en 443) worden gebruikt voor uitgaande communicatie tussen de verificatie-Agents en Azure AD. U hoeft niet te openen van poorten voor inkomend verkeer op uw firewall. 
  - Poort 443 wordt gebruikt voor alle geverifieerde uitgaande communicatie.
  - Poort 80 wordt alleen voor het downloaden van de certificaatintrekkingslijsten (CRL's) gebruikt om ervoor te zorgen dat geen van de certificaten die worden gebruikt door deze functie is ingetrokken.
  - Zie voor de volledige lijst van de netwerkvereisten [Azure Active Directory Pass through-verificatie: snel starten](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-the-prerequisites).
- Wachtwoorden die gebruikers tijdens het aanmelden opgeven worden versleuteld in de cloud voordat de verificatie-Agents voor on-premises ermee akkoord bent gegaan voor validatie op basis van Active Directory.
- Het HTTPS-kanaal tussen Azure AD en de on-premises Authentication-Agent is beveiligd met behulp van wederzijdse verificatie.
- De functie wordt naadloos geïntegreerd met Azure AD-cloudbeveiliging mogelijkheden, zoals beleidsregels voor voorwaardelijke toegang (met inbegrip van Azure multi-factor Authentication), identity protection en Smart Lockout.

## <a name="components-involved"></a>Onderdelen betrokken

Raadpleeg voor algemene informatie over operationele, Azure AD-service en de beveiliging van gegevens, de [Trust Center](https://azure.microsoft.com/support/trust-center/). De volgende onderdelen betrokken zijn wanneer u Pass-through-verificatie voor aanmelding bij de gebruiker gebruikt:
- **Azure AD-STS**: een stateless beveiligingstokenservice (STS) waarmee aanmeldingsaanvragen worden verwerkt en verstrekt beveiligingstokens aan gebruikers browsers, clients of services zoals vereist.
- **Azure Service Bus**: biedt cloudcommunicatie met berichtenverzending en relays communicatie waarmee u verbinding maken met on-premises oplossingen met de cloud.
- **Azure AD Connect Authentication-Agent**: een on-premises-component die luistert naar en reageert op aanvragen van wachtwoord-validatie.
- **Azure SQL Database**: bevat informatie over van uw tenant verificatie-Agents, inclusief de codes metagegevens en versleuteling.
- **Active Directory**: On-premises Active Directory, waar de accounts van uw gebruikers en hun wachtwoorden worden opgeslagen.

## <a name="installation-and-registration-of-the-authentication-agents"></a>Installatie en registratie van de verificatie-Agents

Verificatie-Agents zijn geïnstalleerd en geregistreerd bij Azure AD wanneer u een van beide:
   - [Inschakelen van Pass-through-verificatie via Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-2-enable-the-feature)
   - [Meer verificatie-Agents om te controleren of de hoge beschikbaarheid van aanmeldingsaanvragen toevoegen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-4-ensure-high-availability) 
   
Ophalen van een verificatie-Agent werkt, bestaat uit drie belangrijke fasen:

1. De installatie van de verificatie-Agent
2. Registratie van verificatie-Agent
3. De initialisatie van de verificatie-Agent

De volgende secties worden de fasen in detail besproken.

### <a name="authentication-agent-installation"></a>De installatie van de verificatie-Agent

Alleen globale beheerders kunnen een verificatie-Agent (met behulp van Azure AD Connect of zelfstandig) installeren op een on-premises server. Installatie voegt twee nieuwe items voor de **Configuratiescherm** > **programma's** > **programma's en onderdelen** lijst:
- De verificatie-Agent-toepassing zelf. Deze toepassing wordt uitgevoerd met [NetworkService](https://msdn.microsoft.com/library/windows/desktop/ms684272.aspx) bevoegdheden.
- De Updater-toepassing die wordt gebruikt voor de verificatie-Agent automatisch bijwerken. Deze toepassing wordt uitgevoerd met [LocalSystem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) bevoegdheden.

### <a name="authentication-agent-registration"></a>Registratie van verificatie-Agent

Nadat u de verificatie-Agent hebt geïnstalleerd, moet deze zelf te registreren bij Azure AD. Azure AD wijst elke verificatie-Agent een unieke, digitale identiteit certificaat die kan worden gebruikt voor veilige communicatie met Azure AD toe.

De registratieprocedure koppelt u ook de verificatie-Agent aan uw tenant. Dit zorgt ervoor dat Azure AD weet dat deze specifieke verificatie-Agent is alleen toegestaan voor het afhandelen van aanvragen voor validatie van wachtwoord voor uw tenant. Deze procedure wordt herhaald voor elke nieuwe verificatie-Agent dat u zich registreren.

De verificatie-Agents gebruiken de volgende stappen uit om u te registreren met Azure AD:

![Agentregistratie](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta1.png)

1. Azure AD de eerste keer aanvraagt dat een globale beheerder aanmelden bij Azure AD met hun referenties. Tijdens het aanmelden verkrijgt de verificatie-Agent een toegangstoken die namens de hoofdbeheerder kan worden gebruikt.
2. De verificatie-Agent wordt vervolgens een sleutelpaar gegenereerd: een openbare sleutel en een persoonlijke sleutel.
    - Het sleutelpaar wordt gegenereerd door middel van standaard RSA 2048-bits codering.
    - De persoonlijke sleutel blijft op de on-premises server waarop de verificatie-Agent zich bevindt.
3. De verificatie-Agent maakt een "registratie"-aanvraag naar Azure AD via HTTPS, met de volgende onderdelen opgenomen in de aanvraag:
    - Het toegangstoken is verkregen in stap 1.
    - De openbare sleutel die in stap 2.
    - Een aanvraag voor Certificaatondertekening (CSR of certificaataanvraag). Deze aanvraag geldt voor het certificaat van een digitale identiteit met Azure AD als de certificeringsinstantie (CA).
4. Azure AD het toegangstoken in de aanvraag voor functieregistratie valideert en verifieert u of de aanvraag afkomstig van een globale beheerder is.
5. Vervolgens Azure AD zich aanmeldt en verzendt het certificaat van een digitale identiteit weer in de verificatie-Agent.
    - De basis-CA in Azure AD wordt gebruikt om het certificaat ondertekent. 

     >[!NOTE]
     > Deze CA is _niet_ opslaan in de Windows vertrouwde basiscertificeringsinstanties.
    - De CA wordt alleen gebruikt door de functie voor Pass through-verificatie. De CA wordt alleen gebruikt om zich CSR's tijdens de registratie van verificatie-Agent.
    -  Geen van de andere Azure AD-services gebruik van deze CA.
    - De certificaathouder (DN-naam of DN-naam) is ingesteld op uw tenant-ID. Deze DN-naam is een GUID die de unieke identificatie van uw tenant. Deze DN het bereik van het certificaat voor gebruik met uw tenant.
6. Azure AD slaat de openbare sleutel van de verificatie-Agent in een Azure SQL-database, die alleen Azure AD toegang tot heeft.
7. Het certificaat (in stap 5) is opgeslagen op de lokale server in het Windows-certificaatarchief (met name de [CERT_SYSTEM_STORE_LOCAL_MACHINE](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_LOCAL_MACHINE) locatie). Deze wordt gebruikt door zowel de verificatie-Agent en de toepassingen die Updater.

### <a name="authentication-agent-initialization"></a>De initialisatie van de verificatie-Agent

Wanneer de verificatie-Agent wordt gestart, voor het eerst na de registratie of na een server opnieuw opstarten, moet een manier om te communiceren veilig met de Azure AD-service en start wachtwoord validatie aanvragen te accepteren.

![De initialisatie van agent](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta2.png)

Hier ziet u hoe de verificatie-Agents zijn geïnitialiseerd:

1. De verificatie-Agent maakt een uitgaande bootstrap-aanvraag voor Azure AD. 
    - Deze aanvraag wordt gedaan via poort 443 en is via een wederzijds HTTPS-kanaal. De aanvraag gebruikt hetzelfde certificaat dat is uitgegeven tijdens de registratie van verificatie-Agent.
2. Azure AD reageert op de aanvraag door te geven van een toegangssleutel in een Azure Service Bus-wachtrij die uniek is voor uw tenant en die wordt geïdentificeerd door uw tenant-ID.
3. De verificatie-Agent maakt een permanente uitgaande HTTPS-verbinding (via poort 443) naar de wachtrij. 
    - De verificatie-Agent is nu gereed om te halen en validatie van het wachtwoord aanvragen verwerken.

Als u hebt meerdere verificatie-Agents op uw tenant is geregistreerd, wordt de initialisatieprocedure zorgt ervoor dat elke verbinding met dezelfde Service Bus-wachtrij maakt. 

## <a name="process-sign-in-requests"></a>Proces aanmeldingsaanvragen 

Het volgende diagram toont hoe Pass through-verificatie gebruiker aanmeldingsaanvragen verwerkt.

![Aanmelden van proces](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta3.png)

Pass through-verificatie verwerkt een gebruikersaanvraag aanmelden als volgt: 

1. Een gebruiker probeert te krijgen tot een toepassing, bijvoorbeeld [Outlook Web App](https://outlook.office365.com/owa).
2. Als de gebruiker is niet al aangemeld, wordt de browser in de toepassing omgeleid naar de aanmeldingspagina van Azure AD.
3. De Azure AD-STS-service reageert back-ups met de **aanmelden van gebruikers** pagina.
4. De gebruiker voert de gebruikersnaam in de **aanmelden van gebruikers** pagina en selecteert de **volgende** knop.
5. De gebruiker voert zijn wachtwoord in de **aanmelden van gebruikers** pagina en selecteert de **aanmelden** knop.
6. De gebruikersnaam en wachtwoord worden verzonden naar Azure AD-STS in een HTTPS-POST-aanvraag.
7. Azure AD STS opgehaald van openbare sleutels voor alle verificatie-Agents die worden geregistreerd op uw tenant van de Azure SQL-database en het wachtwoord versleutelt met behulp van deze. 
    - Het genereert is "N" versleuteld wachtwoord waarden voor "N" verificatie-Agents op uw tenant is geregistreerd.
8. Azure AD STS plaatst de wachtwoordvalidatie voor, die uit de gebruikersnaam en het versleutelde wachtwoord waarden, naar de Service Bus-wachtrij die specifiek zijn voor uw tenant bestaat.
9. Omdat de geïnitialiseerd verificatie-Agents permanent met de Service Bus-wachtrij verbonden zijn, een van de beschikbare verificatie-Agents de wachtwoordvalidatie opgehaald.
10. De verificatie-Agent wordt gezocht naar het versleutelde wachtwoordwaarde die is specifiek voor de openbare sleutel met behulp van een id en ontsleutelt deze met behulp van de persoonlijke sleutel.
11. De verificatie-Agent probeert te valideren van de gebruikersnaam en het wachtwoord op basis van on-premises Active Directory met behulp van de [Win32 LogonUser API](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx) met de **dwLogonType** parameter ingesteld op **LOGON32_LOGON_NETWORK**. 
    - Deze API is dezelfde API die wordt gebruikt door Active Directory Federation Services (AD FS) aan te melden bij gebruikers in een scenario voor federatieve aanmelding.
    - Deze API is afhankelijk van het proces standaardresolutie in Windows Server om de domeincontroller te zoeken.
12. De verificatie-Agent ontvangt het resultaat van Active Directory, zoals geslaagde pogingen, gebruikersnaam of wachtwoord is onjuist of het wachtwoord is verlopen.
13. De verificatie-Agent verzendt het resultaat terug naar Azure AD-STS via een uitgaande HTTPS-kanaal sluiten elkaar wederzijds geverifieerd via poort 443. Het certificaat die eerder zijn verleend aan de verificatie-Agent tijdens de registratie maakt gebruik van wederzijdse verificatie.
14. Azure AD STS controleert of dat dit resultaat komt met de aanvraag voor specifieke aanmelden op uw tenant overeen.
15. Azure AD STS gaat verder met de procedure aanmelden zoals geconfigureerd. Bijvoorbeeld als de validatie van het wachtwoord voltooid is, de gebruiker kan worden gecontroleerd voor multi-factor Authentication of keert u terug naar de toepassing.

## <a name="operational-security-of-the-authentication-agents"></a>Operationele beveiliging van de verificatie-Agents

Om ervoor te zorgen dat de Pass through-verificatie operationeel beveiligd blijft, wordt Azure AD regelmatig vernieuwd certificaten voor verificatie-Agents. Azure AD wordt de vernieuwingen geactiveerd. De vernieuwing niet worden geregeld door de verificatie-Agents zelf.

![Operationele beveiliging](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta4.png)

Vernieuwen van een verificatie-Agent-vertrouwensrelatie met Azure AD:

1. De verificatie-Agent regelmatig pingt Azure AD elke paar uur om te controleren als het is tijd om het certificaat te vernieuwen. Het certificaat is 30 dagen vóór de vervaldatum van de vernieuwd.
    - Deze controle wordt uitgevoerd via een wederzijds HTTPS-kanaal en maakt gebruik van hetzelfde certificaat dat is uitgegeven tijdens de registratie.
2. Als de service wordt aangegeven dat is het tijd om te vernieuwen, de verificatie-Agent wordt een nieuw sleutelpaar gegenereerd: een openbare sleutel en een persoonlijke sleutel.
    - Deze sleutels worden gegenereerd door middel van standaard RSA 2048-bits codering.
    - De persoonlijke sleutel wordt nooit verlaat de on-premises server.
3. De verificatie-Agent maakt vervolgens een 'certificaatvernieuwing'-aanvraag naar Azure AD via HTTPS, met de volgende onderdelen opgenomen in de aanvraag:
    - Het bestaande certificaat dat wordt opgehaald uit de CERT_SYSTEM_STORE_LOCAL_MACHINE-locatie op het Windows-certificaatarchief. Er is geen globale beheerder die betrokken zijn bij deze procedure, zodat er geen toegangstoken namens de hoofdbeheerder nodig is.
    - De openbare sleutel die in stap 2.
    - Een aanvraag voor Certificaatondertekening (CSR of certificaataanvraag). Deze aanvraag geldt voor een nieuw certificaat digitale identiteit met Azure AD als de certificeringsinstantie.
4. Azure AD valideert het bestaande certificaat in de aanvraag voor certificaatvernieuwing. Vervolgens wordt gecontroleerd of de aanvraag afkomstig van een verificatie-Agent op uw tenant is geregistreerd is.
5. Als het bestaande certificaat nog geldig is, Azure AD vervolgens ondertekent het certificaat van een nieuwe digitale identiteit en problemen met het nieuwe certificaat weer in de verificatie-Agent. 
6. Als het bestaande certificaat is verlopen, wordt in Azure AD de verificatie-Agent verwijderen uit van uw tenant-lijst met geregistreerde verificatie-Agents. Vervolgens moet een globale beheerder handmatig installeren en registreren van een nieuwe verificatie-Agent.
    - Gebruik de Azure AD basis-CA het certificaat te ondertekenen.
    - Instellen van het certificaat onderwerp (DN-naam of DN-naam) aan uw tenant-ID, een GUID die de unieke identificatie van uw tenant. Het certificaat voor uw tenant alleen het bereik van de DN-naam.
6. Azure AD slaat de nieuwe openbare sleutel van de verificatie-Agent in een Azure SQL-database die alleen het toegang tot heeft. Dit wordt ook ongeldig de oude openbare sleutel die is gekoppeld aan de verificatie-Agent.
7. Het nieuwe certificaat (uitgegeven in stap 5) wordt vervolgens opgeslagen in het Windows-certificaatarchief op de server (met name de [CERT_SYSTEM_STORE_CURRENT_USER](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_CURRENT_USER) locatie).
    - Omdat de vertrouwensrelatie vernieuwing procedure gebeurt niet-interactief (zonder de aanwezigheid van de globale beheerder), heeft de verificatie-Agent niet langer toegang tot het bijwerken van het bestaande certificaat op de locatie CERT_SYSTEM_STORE_LOCAL_MACHINE. 
    
   > [!NOTE]
   > Deze procedure wordt het certificaat zelf niet verwijderd van de locatie CERT_SYSTEM_STORE_LOCAL_MACHINE.
8. Het nieuwe certificaat wordt gebruikt voor verificatie vanaf dit punt op. Elke volgende verlenging van het certificaat wordt vervangen door het certificaat op de locatie CERT_SYSTEM_STORE_LOCAL_MACHINE.

## <a name="auto-update-of-the-authentication-agents"></a>Automatisch bijwerken van de verificatie-Agents

De verificatie-Agent de Updater-toepassing automatisch bijgewerkt wanneer er een nieuwe versie wordt uitgebracht. De toepassing verwerkt niet alle aanvragen van de validatie van wachtwoord voor uw tenant. 

Azure AD als host fungeert voor de nieuwe versie van de software als een ondertekende **Windows Installer-pakket (MSI)**. Het MSI-bestand is ondertekend met behulp van [Microsoft Authenticode](https://msdn.microsoft.com/library/ms537359.aspx) met SHA256 als de digest-algoritme. 

![Automatisch bijwerken](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta5.png)

Voor automatisch bijwerken een verificatie-Agent:

1. De Updater toepassing pings Azure AD elk uur om te controleren of er een nieuwe versie van de verificatie-Agent beschikbaar. 
    - Deze controle wordt via een HTTPS-kanaal sluiten elkaar wederzijds geverifieerd met behulp van hetzelfde certificaat dat is uitgegeven tijdens de registratie uitgevoerd. De verificatie-Agent en de Updater deelt het certificaat dat is opgeslagen op de server.
2. Als een nieuwe versie beschikbaar is, retourneert Azure AD het ondertekende MSI-bestand naar de Updater.
3. De Updater wordt gecontroleerd of het MSI-bestand is ondertekend door Microsoft.
4. De Updater voert het MSI-bestand. Deze actie omvat de volgende stappen uit:

 > [!NOTE]
 > De Updater wordt uitgevoerd met [lokaal systeem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) bevoegdheden.

    - De verificatie-Agent-service gestopt
    - De nieuwe versie van de verificatie-Agent geïnstalleerd op de server
    - De verificatie-Agent-service wordt opnieuw gestart

>[!NOTE]
>Als u meerdere verificatie-Agents op uw tenant is geregistreerd hebt, Azure AD niet hun certificaten vernieuwen of ze op hetzelfde moment bijwerken. In plaats daarvan biedt Azure AD daarom geleidelijk om te controleren of de hoge beschikbaarheid van aanmeldingsaanvragen.
>


## <a name="next-steps"></a>Volgende stappen
- [Huidige beperkingen](active-directory-aadconnect-pass-through-authentication-current-limitations.md): informatie over welke scenario's worden ondersteund en welke niet.
- [Quick start-](active-directory-aadconnect-pass-through-authentication-quick-start.md): aan de slag op Azure AD Pass-through-verificatie.
- [Vergrendeling van het smart](../authentication/howto-password-smart-lockout.md): de mogelijkheid Smart Lockout configureren op uw tenant om te beveiligen van gebruikersaccounts.
- [Hoe het werkt](active-directory-aadconnect-pass-through-authentication-how-it-works.md): Leer de basisprincipes van de werking van Azure AD Pass-through-verificatie.
- [Veelgestelde vragen over](active-directory-aadconnect-pass-through-authentication-faq.md): vind antwoorden op veelgestelde vragen.
- [Problemen oplossen](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): informatie over het oplossen van veelvoorkomende problemen met de functie voor Pass through-verificatie.
- [Azure AD naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md): meer informatie over deze aanvullende functie.
