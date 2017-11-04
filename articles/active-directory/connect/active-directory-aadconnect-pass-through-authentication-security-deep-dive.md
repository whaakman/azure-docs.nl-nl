---
title: Azure Active Directory-verificatie voor Pass Through-beveiliging dieper | Microsoft Docs
description: Dit artikel wordt beschreven hoe Pass through-verificatie van Azure Active Directory (Azure AD) beschermt uw lokale accounts.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, install Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: femila
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: billmath
ms.openlocfilehash: a5feadd851b166d0a9a77bd1d124cdd599d5d701
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-security-deep-dive"></a>Azure Active Directory-verificatie voor Pass Through-beveiliging dieper

In dit artikel biedt een gedetailleerdere beschrijving over de werking van Pass through-verificatie. Dit artikel gaat meer over de beveiligingsaspecten van de functie. In dit onderwerp is van belang zijn voor beveiliging en IT-beheerders, Chief naleving en beveiliging managers en andere IT-professionals die verantwoordelijk is voor IT-beveiliging en naleving in kleine tot middelgrote organisaties of grote ondernemingen.

Onderwerpen zijn onder andere:
- Gedetailleerde technische informatie over hoe verificatie Agents zijn geïnstalleerd en geregistreerd.
- Gedetailleerde technische informatie over het coderen van wachtwoorden tijdens het aanmelden van gebruikers.
- Beveiliging van de kanalen tussen on-premises verificatie Agents en Azure Active Directory (Azure AD).
- Gedetailleerde technische informatie over hoe verificatie Agents operationeel veilig worden bewaard.
- Andere onderwerpen die betrekking hebben op beveiliging.

## <a name="key-security-capabilities"></a>Belangrijkste beveiligingsmogelijkheden

Dit zijn de sleutelbeveiliging aspecten van deze functie:
- Dit is gebaseerd op een veilige multi-verpachte architectuur die isolatie van aanmelden aanvragen tussen de tenants biedt.
- On-premises wachtwoorden worden nooit opgeslagen in de cloud in een formulier.
- Lokale verificatie Agents die luisteren naar en reageren op aanvragen voor validatie van wachtwoord, worden alleen uitgaande verbindingen van binnen uw netwerk maken. Er is geen vereiste voor het installeren van deze verificatie-Agents in een perimeternetwerk (DMZ genoemd).
- Alleen standaard poorten (80 en 443) worden gebruikt voor uitgaande communicatie van de verificatie-Agents naar Azure AD. Er zijn geen poorten voor inkomend verkeer moeten worden geopend op uw firewall. 
  - Poort 443 wordt gebruikt voor alle geverifieerde uitgaande communicatie
  - Poort 80 wordt alleen gebruikt voor het downloaden van de certificaatintrekkingslijsten (CRL's) om te controleren of geen van de certificaten die worden gebruikt door de functie zijn ingetrokken.
  - Zie [hier](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites) voor de volledige lijst met netwerkvereisten.
- Wachtwoorden die zijn opgegeven door de gebruiker tijdens het aanmelden worden gecodeerd in de cloud voordat wordt geaccepteerd door on-premises verificatie Agents voor validatie op basis van uw Active Directory.
- Het HTTPS-kanaal tussen Azure AD en een lokale verificatie-Agent wordt beveiligd door wederzijdse verificatie.
- Naadloos geïntegreerd met Azure AD cloud beveiliging mogelijkheden zoals beleidsregels voor voorwaardelijke toegang (met inbegrip van multi-factor Authentication) Identity Protection en de smartcard vergrendelingsduur.

## <a name="components-involved"></a>Onderdelen betrokken

Raadpleeg voor algemene informatie over operationele, Azure AD-service en de beveiliging van gegevens, de [Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/). De volgende onderdelen betrokken zijn bij Pass through-verificatie wordt gebruikt voor de gebruiker zich aanmelden:
- **Azure AD-STS**: een stateless Security Token Service (STS) die aanmelden verzoeken verwerkt en verstrekt beveiligingstokens aan gebruikers browsers, clients of services zoals vereist.
- **Azure Service Bus**: biedt ingeschakeld voor de cloud communicatie met berichtenverzending voor bedrijven en relays communicatie waarmee u oplossingen on-premises-verbinding met de cloud.
- **Azure AD Connect verificatie-Agent (verificatie-Agent)**: een onderdeel van de lokale die luistert naar en reageert op aanvragen van wachtwoord validatie.
- **Azure SQL Database**: bevat informatie over uw tenant verificatie Agents, met inbegrip van de sleutels metagegevens en versleuteling.
- **Active Directory (AD)**: uw lokale Active Directory, waar de gebruikersaccounts (en hun wachtwoorden) worden opgeslagen.

## <a name="installation-and-registration-of-authentication-agents"></a>Installatie en registratie van verificatie Agents

Verificatie-Agents zijn geïnstalleerd en geregistreerd met Azure AD wanneer u [Pass through-verificatie met Azure AD Connect inschakelen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-2-enable-the-feature) of wanneer u [toevoegen om ervoor te zorgen hoge beschikbaarheid van extra verificatie-Agents aanmeldingsaanvragen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-4-ensure-high-availability). Ophalen van een verificatie-Agent werkt bestaat uit drie belangrijkste fasen:

- De installatie van de verificatie-Agent
- Agentregistratie verificatie
- De initialisatie van de verificatie-Agent

Elk van deze wordt beschreven in de volgende onderwerpen worden besproken.

### <a name="authentication-agent-installation"></a>De installatie van de verificatie-Agent

Alleen globale beheerders kunnen een verificatie-Agent (met behulp van Azure AD Connect of zelfstandig) installeren op een on-premises server. Installatie voegt twee nieuwe vermeldingen voor de **Configuratiescherm -> programma's -> programma's en onderdelen** lijst:
- De verificatie-Agent-toepassing zelf. Dit wordt uitgevoerd met [netwerkservice](https://msdn.microsoft.com/library/windows/desktop/ms684272.aspx) bevoegdheden.
- De Updater toepassing gebruikt voor het automatisch bijwerken van de verificatie-Agent. Dit wordt uitgevoerd met [lokaal systeem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) bevoegdheden.


### <a name="authentication-agent-registration"></a>Agentregistratie verificatie

Nadat u de verificatie-Agent hebt geïnstalleerd, moet deze zelf registreren met Azure AD. Azure AD doet dit door het toewijzen van elke Agent verificatie een unieke id van de digitale certificaat die kan worden gebruikt voor veilige communicatie met Azure AD.

De registratieprocedure ook de verificatie-Agent met uw tenant wordt gebonden zodat die Azure AD weet dat deze specifieke verificatie-Agent is de enige gemachtigd wachtwoord validatie om aanvragen te verwerken voor uw tenant. Deze procedure wordt herhaald voor elke nieuwe verificatie-Agent registreren.

Hier ziet u hoe verificatie Agents registreren met Azure AD:

![Het registreren van agent](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta1.png)

1. Azure AD-eerst een globale beheerder aan te melden bij Azure AD met hun referenties aanvragen. Tijdens het aanmelden verkrijgt de verificatie-Agent een Access Token die namens de hoofdbeheerder kan worden gebruikt.
2. De verificatie-Agent wordt gegenereerd zodra een sleutelpaar – een openbare sleutel en een persoonlijke sleutel.
    - De combinatie van deze sleutel wordt gegenereerd met behulp van standaard **RSA 2048-bits** versleuteling.
    - De persoonlijke sleutel wordt nooit verlaten van de lokale server waarop de verificatie-Agent is geïnstalleerd.
3.  De verificatie-Agent maakt een registratieaanvraag voor "" naar Azure AD via HTTPS, met de volgende onderdelen opgenomen in de aanvraag:
    - De Access Token verkregen in stap 1.
    - De openbare sleutel die wordt gegenereerd in stap 2.
    - Een **ondertekening van de aanvraag van het certificaat** (CSR of certificaataanvraag). Dit is een certificaat digitale identiteiten met Azure AD als de certificeringsinstantie.
4. Azure AD Access Token in de registratieaanvraag valideert en wordt gecontroleerd of de aanvraag afkomstig van een globale beheerder is.
5. Azure AD en vervolgens ondertekent en problemen met een certificaat van de digitale identiteit weer naar de verificatie-Agent.
    - Het certificaat is ondertekend met **Azure AD Root certificeringsinstantie (CA)**. Opmerking: deze CA is _niet_ in het Windows **vertrouwde basiscertificeringsinstanties** opslaan.
    - Deze Certificeringsinstantie wordt alleen door de functie Pass through-verificatie gebruikt. Alleen wordt gebruikt voor ondertekening CSR's tijdens de registratie van de verificatie-Agent.
    - Deze Certificeringsinstantie wordt niet gebruikt door een andere service in Azure AD.
    - De certificaathouder (**DN-naam of DN**) is ingesteld op uw **Tenant-ID**. Dit is een GUID die een unieke identificatie van uw tenant. Hiermee begrenst u het certificaat voor gebruik met alleen uw tenant.
6. Azure AD slaat de openbare sleutel van de verificatie-Agent in een Azure SQL-database, die alleen Azure AD toegang tot heeft.
7. Het certificaat (uitgegeven in stap 5) wordt opgeslagen op de lokale server in de **Windows-certificaatarchief** (specifiek in de  **[CERT_SYSTEM_STORE_LOCAL_MACHINE](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_LOCAL_MACHINE)**  locatie), en wordt gebruikt door zowel de verificatie-Agent en de Updater-toepassingen.

### <a name="authentication-agent-initialization"></a>De initialisatie van de verificatie-Agent

Wanneer de verificatie-Agent wordt gestart, voor de eerste keer na de registratie of na een server opnieuw opstarten, moet een manier om veilig communiceren met Azure AD-service en start wachtwoord validatie aanvragen te accepteren.

![De initialisatie van agent](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta2.png)

Hier ziet u hoe verificatie Agents ophalen geïnitialiseerd:



1. De verificatie-Agent wordt gestart door het maken van een uitgaande 'bootstrap'-aanvraag naar Azure AD. 
    - Deze aanvraag bootstrap via poort 443 wordt gemaakt en wordt via een wederzijds verifieerbaar HTTPS-kanaal (met behulp van hetzelfde certificaat uitgegeven tijdens de registratie van de verificatie-Agent).
2. Azure AD reageert op deze bootstrap aanvraag door een **toegangssleutel** naar een Azure Service Bus-wachtrij die uniek is voor uw tenant (aangeduid door uw Tenant-ID).
3. De verificatie-Agent maakt een permanente uitgaande HTTPS-verbinding (via poort 443) naar de wachtrij. 
    - Het is nu gereed om te halen en het wachtwoord validatie van aanvragen.

Als u hebt meerdere verificatie-Agents op uw tenant is geregistreerd, wordt de van de initialisatieprocedure zorgt ervoor dat elk criterium verbinding met dezelfde Azure Service Bus-wachtrij maakt. 

## <a name="processing-sign-in-requests"></a>Verwerking van aanvragen voor aanmelden 

Het onderstaande diagram ziet hoe Pass through-verificatie gebruiker aanmelden aanvragen verwerkt.

![Verwerking van aanmeldingspagina](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta3.png)

Pass through-verificatie verwerkt een gebruikersaanvraag aanmelden als volgt: 

1. Een gebruiker probeert te krijgen tot een toepassing (bijvoorbeeld de Outlook Web App - [https://outlook.office365.com/owa](https://outlook.office365.com/owa).)
2. Als de gebruiker niet is al aangemeld, wordt de browser in de toepassing omgeleid naar de aanmeldingspagina van Azure AD.
3. De Azure AD-STS-service reageert op de aanmeldingspagina van gebruiker.
4. De gebruiker voert hun gebruikersnaam en wachtwoord in de aanmeldingspagina van Azure AD en klikt op de knop 'Aanmelden'.
5. De gebruikersnaam en wachtwoord worden verzonden naar Azure AD-STS in een HTTPS-POST-aanvraag.
6. Azure AD STS haalt van openbare sleutels voor alle verificatie-Agents op uw tenant in Azure SQL-database geregistreerd en het wachtwoord dat ze versleutelt. 
    - Het versleutelde wachtwoord "n" genereert waarden voor "n" verificatie Agents op uw tenant is geregistreerd.
7. Azure AD STS wordt de wachtwoord-validatieaanvraag (de gebruikersnaam en waarden van het versleutelde wachtwoord) naar de Azure Service Bus-wachtrij geplaatst die specifiek zijn voor uw tenant.
8. Aangezien de geïnitialiseerde verificatie Agents permanent met de Azure Service Bus-wachtrij verbonden zijn, een van de beschikbare Agents voor verificatie de wachtwoord-validatieaanvraag opgehaald.
9. De verificatie-Agent wordt gezocht naar de waarde van het versleutelde wachtwoord die specifiek is voor de openbare sleutel (met een id) en ontsleutelt deze met behulp van de persoonlijke sleutel.
10. De verificatie-Agent probeert te valideren van de gebruikersnaam en het wachtwoord op basis van uw lokale Active Directory met het  **[Win32 LogonUser API](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx)**  (met de **dwLogonType** parameter ingesteld op **LOGON32_LOGON_NETWORK**). 
    - Dit is de dezelfde API gebruikt door Active Directory Federation Services (AD FS) aan te melden gebruikers in een federatieve aanmelding scenario.
    - Dit is afhankelijk van het proces standaardresolutie in Windows Server naar de domein-Contoller vinden.
11. De verificatie-Agent ontvangt het resultaat van Active Directory (geslaagd, gebruikersnaam of wachtwoord is onjuist, het wachtwoord is verlopen, is vergrendeld door gebruiker, enzovoort).
12. De verificatie-Agent verzendt het resultaat terug naar Azure AD-STS via een uitgaande wederzijds verifieerbaar HTTPS-kanaal via poort 443. Eerder uitgegeven aan de verificatie-Agent tijdens de registratie van hetzelfde certificaat maakt gebruik van wederzijdse verificatie.
13. Azure AD STS controleert of dat dit resultaat met de aanvraag voor specifieke aanmelden op uw tenant correleert.
14. Azure AD STS gaat door met de procedure aanmelden zoals geconfigureerd. Bijvoorbeeld, als de wachtwoordvalidatie van het gelukt is, de gebruiker kan worden gecontroleerd voor multi-factor Authentication of terug naar de toepassing omgeleid.

## <a name="operational-security-of-authentication-agents"></a>Operationele beveiliging van de verificatie-Agents

Om ervoor te zorgen dat Pass through-verificatie operationeel veilige blijft, vernieuwt Azure AD regelmatig hun certificaten. Deze vernieuwing van Azure AD worden geactiveerd en niet worden geregeld door de verificatie-Agents zelf.

![Operationele beveiliging](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta4.png)

Hier ziet u hoe een verificatie-Agent wordt verlengd de vertrouwensrelatie met Azure AD:

1. De verificatie-Agent regelmatig pingt Azure AD (om de paar uur) om te controleren of het tijd om het certificaat te vernieuwen. 
    - Deze controle wordt uitgevoerd via een wederzijds verifieerbaar HTTPS-kanaal (met behulp van hetzelfde certificaat uitgegeven tijdens de registratie).
2. Als de service is het tijd om te vernieuwen aangeeft, de verificatie-Agent genereert een nieuw sleutelpaar: een openbare sleutel en een persoonlijke sleutel.
    - Deze sleutels worden gegenereerd met de standaard **RSA 2048-bits** versleuteling.
    - De persoonlijke sleutel wordt nooit verlaten van de on-premises server.
3. De verificatie-Agent maakt vervolgens een 'certificaatvernieuwing'-aanvraag naar Azure AD via HTTPS, met de volgende onderdelen opgenomen in de aanvraag:
    - Het bestaande certificaat (opgehaald uit **CERT_SYSTEM_STORE_LOCAL_MACHINE** locatie op het Windows-certificaatarchief). Er is geen algemeen beheerder die zijn betrokken bij deze procedure, zodat er geen toegang-Token namens de hoofdbeheerder nodig is.
    - De openbare sleutel die wordt gegenereerd in stap 2.
    - Een **ondertekening van de aanvraag van het certificaat** (CSR of certificaataanvraag). Dit is om toe te passen voor een nieuw certificaat digitale identiteiten met Azure AD als de certificeringsinstantie.
4. Azure AD valideert het bestaande certificaat in de aanvraag voor certificaatvernieuwing. Vervolgens wordt gecontroleerd of de aanvraag afkomstig van een verificatie-Agent op uw tenant is geregistreerd is.
5. Als het bestaande certificaat nog geldig is, Azure AD vervolgens ondertekent het certificaat van een nieuwe digitale identiteit en problemen met het nieuwe certificaat terug naar de verificatie-Agent. 
6. Als het bestaande certificaat is verlopen, verwijderen Azure AD de verificatie-Agent van uw tenant-lijst van geregistreerde verificatie Agents. Een globale beheerder moet handmatig installeren en registreren van een nieuwe Agent voor verificatie.
    - Het certificaat is ondertekend met **Azure AD Root certificeringsinstantie (CA)**.
    - De certificaathouder (**DN-naam of DN**) is ingesteld op uw **Tenant-ID** een GUID die een unieke identificatie van uw tenant. Dat wil zeggen, is het certificaat afgestemd op uw tenant alleen.
6. Azure AD slaat de "new" openbare sleutel van de verificatie-Agent in een Azure SQL-database die alleen toegang heeft. En de 'oude' openbare sleutel die is gekoppeld aan de verificatie-Agent wordt ongeldig gemaakt.
7. Het nieuwe certificaat (dat is uitgegeven in stap 5) wordt vervolgens opgeslagen op de server in de **Windows-certificaatarchief** (specifiek in de  **[CERT_SYSTEM_STORE_CURRENT_USER](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_CURRENT_USER)**  locatie).
    - Omdat de vertrouwensrelatie vernieuwing procedure gebeurt niet-interactief (zonder de aanwezigheid van de globale beheerder), de verificatie-Agent niet langer toegang heeft tot bijwerken van het bestaande certificaat in de **CERT_SYSTEM_STORE_LOCAL_MACHINE** locatie. Houd er rekening mee dat het certificaat zelf in de **CERT_SYSTEM_STORE_LOCAL_MACHINE** locatie wordt niet verwijderd tijdens deze procedure.
8. Het nieuwe certificaat wordt gebruikt voor verificatie vanaf dit punt op. Elke latere verlenging van het certificaat vervangt het certificaat in de **CERT_SYSTEM_STORE_LOCAL_MACHINE** locatie.

## <a name="auto-update-of-authentication-agents"></a>Automatisch bijwerken van Agents voor verificatie

De verificatie-Agent de toepassing Updater automatisch bijgewerkt wanneer er een nieuwe versie wordt uitgebracht. Dit verwerkt wachtwoord validatie verzoeken voor uw tenant niet. 

Azure AD als host fungeert voor de nieuwe versie van de software als een ondertekende **Windows Installer-pakket (MSI)**. Het MSI-bestand is ondertekend met [Microsoft Authenticode](https://msdn.microsoft.com/library/ms537359.aspx) met **SHA256** als digest-algoritme. 

![Automatisch bijwerken](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta5.png)

Hier ziet u hoe een verificatie-Agent wordt automatisch bijgewerkt:

1. De Updater pingt periodiek Azure AD (elk uur) om te controleren of er een nieuwe versie van de verificatie-Agent beschikbaar. 
    - Deze controle wordt uitgevoerd via een wederzijds verifieerbaar HTTPS-kanaal (met behulp van hetzelfde certificaat uitgegeven tijdens de registratie). De verificatie-Agent en de Updater delen het certificaat dat is opgeslagen op de server.
2. Als een nieuwe versie beschikbaar is, retourneert Azure AD het ondertekende MSI-bestand naar de Updater.
3. De Updater verifieert dat het MSI-bestand is ondertekend door Microsoft.
4. De Updater voert het MSI-bestand. Deze actie wordt de volgende stappen uit (Let op: de Updater wordt uitgevoerd met [lokaal systeem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) bevoegdheden):
    - De verificatie-Agent-service gestopt.
    - Installeert de nieuwe versie van de verificatie-Agent op de server.
    - De verificatie-Agent-service wordt opnieuw gestart.

>[!NOTE]
>Als er meerdere verificatie-Agents op uw tenant is geregistreerd, Azure AD niet hun certificaten vernieuwen of deze op hetzelfde moment bijwerken. In plaats daarvan biedt Azure AD dus geleidelijk om ervoor te zorgen hoge beschikbaarheid van de aanvragen aanmelden.


## <a name="next-steps"></a>Volgende stappen
- [**Huidige beperkingen** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -welke scenario's worden ondersteund en welke worden niet meer.
- [**Snel starten** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - laten en Azure AD Pass-through-verificatie wordt uitgevoerd.
- [**Vergrendelen van smartcard** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -mogelijkheid slimme accountvergrendeling configureren op uw tenant gebruikersaccounts te beveiligen.
- [**Hoe het werkt** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -Leer de basisbeginselen van de werking van Azure AD Pass-through-verificatie.
- [**Veelgestelde vragen** ](active-directory-aadconnect-pass-through-authentication-faq.md) -antwoorden op veelgestelde vragen.
- [**Problemen met** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -informatie over het oplossen van veelvoorkomende problemen met de functie.
- [**Azure AD naadloze eenmalige aanmelding** ](active-directory-aadconnect-sso.md) -meer informatie over deze aanvullende functie.

