---
title: Self-service voor wachtwoord opnieuw instellen van implementatieplan - Azure Active Directory
description: Strategie voor de succesvolle implementatie van Azure AD Self-service voor wachtwoord opnieuw instellen
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b566bfc3f6c49f6cb9fe31f166356f6ae351e38
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440947"
---
# <a name="deploy-azure-ad-self-service-password-reset"></a>Implementeer in Azure AD-selfservice wachtwoord opnieuw instellen

Selfservice voor wachtwoordherstel (SSPR) is een Azure Active Directory-functie waarmee werknemers hun wachtwoord opnieuw instellen zonder contact opnemen met IT-personeel. Werknemers moeten registreren voor of worden geregistreerd voor selfservice voor wachtwoord opnieuw instellen voordat u de service. Tijdens de registratie kiest de werknemer een of meer verificatiemethoden ingeschakeld door hun organisatie.

SSPR kan werknemers snel gedeblokkeerd ophalen en blijven werken, ongeacht waar ze zich bevinden of van de tijd van de dag. Doordat gebruikers zelf deblokkeren, kan uw organisatie verminderen de niet-productieve tijd en de van hoge ondersteuningskosten voor de meest voorkomende problemen met betrekking tot wachtwoord.

Help gebruikers snel door te SSPR implementeren samen met een andere toepassing of service in uw organisatie te registreren. Met deze actie genereert een groot aantal aanmeldingen en de basis van inschrijving.

Voordat u implementeert SSPR, kunnen organisaties wilt om te bepalen hoeveel wachtwoord opnieuw instellen naar de gerelateerde help helpdesk laten gebeuren voor tijd en de gemiddelde kosten van elke aanroep. Ze kunnen deze gegevens na de implementatie gebruiken om weer te geven van de waarde dat SSPR overbrengen van uw organisatie.  

## <a name="how-sspr-works"></a>Hoe SSPR werkt

1. Wanneer een gebruiker probeert een wachtwoord opnieuw instellen, moeten ze controleren of de eerder geregistreerde verificatiemethode of -methoden om hun identiteit te bewijzen.
1. De gebruiker voert vervolgens een nieuw wachtwoord.
   1. Voor alleen-cloud, gebruikers, wordt het nieuwe wachtwoord opgeslagen in Azure Active Directory. Zie voor meer informatie het artikel [hoe SSPR werkt](concept-sspr-howitworks.md#how-does-the-password-reset-portal-work).
   1. Voor hybride gebruikers, het wachtwoord teruggeschreven naar de on-premises Active Directory via de Azure AD Connect-service. Zie voor meer informatie het artikel [wat is wachtwoord terugschrijven](concept-sspr-writeback.md#how-password-writeback-works).

## <a name="licensing-considerations"></a>Licenties

Azure Active Directory is een licentie per gebruiker betekenis elke gebruiker heeft beschikken over een juiste licentie voor de functies die ze gebruiken.

- Self-service voor wachtwoord opnieuw instellen voor gebruikers met alleen-cloud is beschikbaar met Azure AD Basic of hoger.
- Self-service voor wachtwoord opnieuw instellen met terugschrijven van de on-premises voor hybride omgevingen vereist Azure AD Premium P1 of hoger.

Meer informatie over licentieverlening te vinden in de [Azure Active Directory pagina met prijzen](https://azure.microsoft.com/pricing/details/active-directory/)

## <a name="enable-combined-registration-for-sspr-and-mfa"></a>De gecombineerde registratie inschakelen voor SSPR en MFA

Microsoft raadt aan dat organisaties de gecombineerde registratie-ervaring voor self-service voor Wachtwoordherstel en multi-factor authentication inschakelen. Wanneer u deze gecombineerde registratie-ervaring inschakelt, moeten gebruikers alleen hun registratiegegevens één keer selecteren om in te schakelen van beide functies.

![Registratie van de gecombineerde beveiliging-informatie](./media/howto-sspr-deployment/combined-security-info.png)

De gecombineerde registratie-ervaring is niet vereist voor organisaties om in te schakelen SSPR en Azure multi-factor Authentication te gebruiken. De gecombineerde registratie-ervaring biedt organisaties een betere gebruikerservaring in vergelijking met de traditionele afzonderlijke onderdelen. Meer informatie over gecombineerde registratie, en hoe u inschakelt, kan worden gevonden in het artikel [gecombineerd security informatie registratie (preview)](concept-registration-mfa-sspr-combined.md)

## <a name="plan-the-configuration"></a>De configuratie plannen

De volgende instellingen zijn vereist voor het inschakelen van SSPR, samen met de aanbevolen waarden.

| Onderwerp | Instelling | Value |
| --- | --- | --- |
| **SSPR-eigenschappen** | Self-service voor wachtwoord opnieuw instellen is ingeschakeld | **Geselecteerde** groep voor de pilot / **alle** voor productie |
| **Verificatiemethoden** | Verificatiemethoden die zijn vereist voor de registratie | Altijd 1 meer dan is vereist voor het opnieuw instellen |
|   | Verificatiemethoden die zijn vereist voor het opnieuw instellen | Een of twee |
| **Registratie** | Vereisen dat gebruiker zich bij aanmelding registreren | Ja |
|   | Aantal dagen waarna gebruikers wordt gevraagd hun verificatiegegevens opnieuw te bevestigen | 90, 180 dagen |
| **Meldingen** | Gebruikers een melding tonen over het opnieuw instellen van hun wachtwoord | Ja |
|   | Alle beheerders waarschuwen wanneer andere beheerders hun wachtwoord opnieuw instellen | Ja |
| **Aanpassing** | Helpdeskkoppeling aanpassen | Ja |
|   | Aangepaste helpdesk-e-mailadres of URL | Ondersteuning voor site- of e-mailadres |
| **On-premises integratie** | Wachtwoorden terugschrijven naar on-premises AD | Ja |
|   | Toestaan dat gebruikers voor het account ontgrendelen zonder wachtwoord in te stellen | Ja |

### <a name="sspr-properties-recommendations"></a>Aanbevelingen voor SSPR-eigenschappen

Bij het inschakelen van self-service voor wachtwoord opnieuw instellen, kiest u een beveiligingsgroep moet worden gebruikt tijdens de pilot.

Wanneer u van plan bent om te starten van de service grotere schaal, wordt u aangeraden de optie Alles om af te dwingen van self-service voor Wachtwoordherstel voor iedereen in de organisatie. Als u niet voor alle, selecteer de juiste Azure AD-beveiligingsgroep of AD gesynchroniseerd met Azure AD instellen.

### <a name="authentication-methods"></a>Verificatiemethoden

Verificatiemethoden die zijn vereist voor de registratie naar ten minste één meer dan het getal dat moet opnieuw ingesteld. Zodat meerdere biedt gebruikers flexibiliteit wanneer ze opnieuw wilt instellen.

Stel **aantal methoden dat is vereist om opnieuw in te** naar een niveau dat geschikt is voor uw organisatie. Een vereist de minste ergernis, terwijl er twee uw beveiligingspostuur verhogen.

Zie [wat verificatiemethoden zijn](concept-authentication-methods.md) voor gedetailleerde informatie over welke verificatie methoden zijn beschikbaar voor SSPR, vooraf gedefinieerde beveiligingsvragen en over het maken van aangepaste beveiligingsvragen.

### <a name="registration-settings"></a>Registratie-instellingen

Stel **vereisen dat gebruikers zich bij aanmelding registreren** naar **Ja**. Deze instelling betekent dat de gebruikers worden gedwongen om u te registreren bij het aanmelden, ervoor te zorgen dat alle gebruikers zijn beveiligd.

Instellen **aantal dagen waarna gebruikers wordt gevraagd hun verificatiegegevens opnieuw te bevestigen** aan tussen **90** en **180** dagen, tenzij uw organisatie is in uw bedrijf nodig voor een kortere periode.

### <a name="notifications-settings"></a>Meldingsinstellingen voor

Configureren van beide de **meldingen verzenden naar gebruikers op wachtwoord opnieuw instellen van wachtwoorden** en de **alle beheerders waarschuwen wanneer andere beheerders hun wachtwoord opnieuw instellen** naar **Ja**. Selecteren **Ja** op zowel verhoogt de beveiliging door ervoor te zorgen dat gebruikers zich bewust bent als het wachtwoord is opnieuw ingesteld en dat alle beheerders zijn op de hoogte wanneer een beheerder een wachtwoord wordt gewijzigd. Als gebruikers of beheerders die een melding ontvangen en ze de wijziging niet hebt gestart, kunnen ze onmiddellijk een potentieel beveiligingsprobleem rapporteren.

### <a name="customization"></a>Aanpassing

Het is essentieel om aan te passen de **helpdesk e-mailadres of URL** om te controleren of gebruikers die problemen snel hulp kunnen krijgen. Deze optie instelt op een algemene e-mailadres van de helpdesk of de webpagina die bekend is met uw gebruikers.

### <a name="on-premises-integration"></a>On-premises integratie

Als u een hybride omgeving hebt, zorg ervoor dat **wachtwoorden terugschrijven naar on-premises AD** is ingesteld op **Ja**. Het account ontgrendelen zonder wachtwoord in te stellen op Yes, zoals het biedt hen flexibeler toestaan dat gebruikers ook worden ingesteld.

### <a name="changingresetting-passwords-of-administrators"></a>Wachtwoorden wijzigen/Resetting voor beheerders

Administrator-accounts zijn speciale accounts met verhoogde bevoegdheden. Als u wilt deze beveiligen, gelden de volgende beperkingen voor het wijzigen van wachtwoorden voor beheerders:

- On-premises Ondernemingsadministrators of Domeinadministrators kunnen niet opnieuw instellen van hun wachtwoord via Self-service voor Wachtwoordherstel. Ze kunnen hun wachtwoord in hun on-premises-omgeving alleen wijzigen. Dus, wordt aangeraden niet synchroniseren on-premises AD-beheerder-accounts aan Azure AD.
- Een beheerder niet gebruiken geheime vragen en antwoorden als een methode voor wachtwoord opnieuw instellen.

### <a name="environments-with-multiple-identity-management-systems"></a>Omgevingen met meerdere identity management systems

Als er meerdere identity management-systemen in een omgeving, zoals on-premises identity managers zoals Oracle AM, SiteMinder, of andere systemen en geschreven naar Active Directory-wachtwoorden moet mogelijk worden gesynchroniseerd met andere systemen met behulp van een hulpprogramma zoals de meldingsservice voor wachtwoordwijzigingen (PCNS) met Microsoft Identity Manager (MIM). Ga voor meer informatie over dit complexere scenario, Zie het artikel [implementeren van de MIM meldingsservice voor wachtwoordwijzigingen op een domeincontroller](https://docs.microsoft.com/microsoft-identity-manager/deploying-mim-password-change-notification-service-on-domain-controller).

## <a name="plan-deployment-and-support-for-sspr"></a>Implementatie en ondersteuning voor SSPR plannen

### <a name="engage-the-right-stakeholders"></a>De juiste belanghebbenden betrekken

Wanneer de technologieprojecten niet, doen ze meestal vanwege niet-overeenkomende verwachtingen op impact, resultaten en verantwoordelijkheden. Zorg ervoor dat u de juiste belanghebbenden zijn benaderen en dat belanghebbende rollen in het project goed worden begrepen door het vastleggen van de betrokkenen en hun project invoer en verantwoordelijkheid om te voorkomen dat deze valkuilen.

### <a name="communications-plan"></a>Communicatieplan

Communicatie is essentieel om het succes van elke nieuwe service. Proactief te communiceren met uw gebruikers over het gebruik van de service en wat ze te doen voor hulp als er iets niet werkt zoals verwacht. Controleer de [Self-service voor wachtwoord opnieuw instellen van implementatie materiaal op het Microsoft download center](https://www.microsoft.com/download/details.aspx?id=56768) voor ideeën over het plannen van uw strategie voor de communicatie door eindgebruikers.

### <a name="testing-plan"></a>Plan testen

Om ervoor te zorgen dat uw implementatie werkt zoals verwacht, moet u van plan bent om een set van testscenario's die u gebruiken wilt voor het valideren van de implementatie. De volgende tabel bevat enkele nuttige Testscenario die kunt u in uw organisatie resultaten op basis van uw beleid verwacht document.

| Businesscase | Verwacht resultaat |
| --- | --- |
| SSPR-portal is toegankelijk via binnen het bedrijfsnetwerk. | Bepaald door uw organisatie |
| SSPR-portal is toegankelijk van buiten het bedrijfsnetwerk. | Bepaald door uw organisatie |
| Gebruikerswachtwoord van browser wanneer de gebruiker is niet ingeschakeld voor wachtwoord opnieuw instellen | Gebruiker is geen toegang krijgen tot de stroom van wachtwoord opnieuw instellen |
| Gebruikerswachtwoord van browser wanneer de gebruiker is niet geregistreerd voor wachtwoord opnieuw instellen | Gebruiker is geen toegang krijgen tot de stroom van wachtwoord opnieuw instellen |
| Gebruiker zich aanmeldt als wachtwoord opnieuw instellen van inschrijving wordt afgedwongen | Gebruiker wordt gevraagd om beveiligingsgegevens te registreren |
| Gebruiker zich aanmeldt wanneer de registratie op wachtwoord opnieuw instellen is voltooid | Gebruiker niet gevraagd om beveiligingsgegevens te registreren |
| SSPR-portal is toegankelijk wanneer de gebruiker beschikt niet over een licentie | Toegankelijk is |
| Wachtwoord van de gebruiker van Windows 10 toevoegen aan AAD of H + AADJ vergrendelingsscherm van het apparaat opnieuw instellen nadat de gebruiker is geregistreerd | Gebruiker kan wachtwoord opnieuw instellen |
| SSPR-registratie en het gebruik gegevens zijn beschikbaar voor beheerders in bijna realtime | Is beschikbaar via de logboeken voor controle |

### <a name="support-plan"></a>Ondersteuningsplan

Terwijl SSPR normaal gesproken geen gebruiker problemen maakt, is het belangrijk dat u hebt ondersteuning personeel voorbereid te bekommeren om problemen die zich kunnen voordoen.

Hoewel een beheerder kan wijzigen of het wachtwoord opnieuw voor eindgebruikers via de Azure AD-portal instellen, is het beter om u te helpen bij het oplossen van het probleem via een ondersteuningsproces selfservice.

In de operationele guide-sectie van dit document, maakt u een lijst van kwesties en hun mogelijke oorzaken en een handleiding voor het probleem zou moeten maken.

### <a name="auditing-and-reporting"></a>Controle en rapportage

Na de implementatie in veel organisaties willen weten hoe of als selfservice voor wachtwoordherstel (SSPR) echt wordt gebruikt. De rapportagefunctie die Azure Active Directory (Azure AD) biedt vindt u antwoorden op vragen met behulp van vooraf gedefinieerde rapporten.

Auditlogboeken voor registratie en het wachtwoord opnieuw instellen, zijn beschikbaar voor 30 dagen. Daarom als security auditing gebruiken binnen een onderneming langere periode vereist, de logboeken moeten worden geëxporteerd en gebruikt in een SIEM-hulpprogramma, zoals [Azure Sentinel](../../sentinel/connect-azure-active-directory.md), Splunk of ArcSight.

Document in een tabel, zoals hieronder, de back-upschema, het systeem en de partijen die verantwoordelijk is. U moet mogelijk niet scheiden van controle en rapportage van back-ups, maar er is een afzonderlijke back-up van waaruit u vanaf een probleem herstellen kunt.

|   | Frequentie van downloaden | Doelsysteem | Verantwoordelijke partij |
| --- | --- | --- | --- |
| Controleprogramma back-up |   |   |   |
| Rapportage van back-up |   |   |   |
| Back-up van herstel na noodgevallen |   |   |   |

## <a name="implementation"></a>Implementatie

Uitvoering vindt plaats in drie fasen:

- Gebruikers en licenties configureren
- Azure AD SSPR geconfigureerd voor registratie en selfservice garanderen
- Azure AD Connect voor het terugschrijven van wachtwoorden configureren

### <a name="communicate-the-change"></a>De wijziging communiceren

Beginnen met de uitvoering van het communicatieplan die u in de planningsfase ontwikkeld.

### <a name="ensure-groups-are-created-and-populated"></a>Zorg ervoor dat groepen worden gemaakt en ingevuld

Verwijzen naar de sectie Planning wachtwoord verificatie methoden en zorg ervoor dat de groep(en) voor de pilot of productie-implementatie zijn beschikbaar en alle juiste gebruikers zijn toegevoegd aan de groepen.

### <a name="apply-licenses"></a>Licenties voor toepassen

De groepen die u wilt implementeren moet de Azure AD premium-licentie toegewezen. U kunt licenties toewijzen rechtstreeks aan de groep, of kunt u bestaande licentie beleidsregels zoals via PowerShell of licentieverlening op basis van een groep.

Informatie over het toewijzen van licenties aan groepen met gebruikers kan worden gevonden in het artikel [licenties toewijzen aan gebruikers door groepslidmaatschappen in Azure Active Directory](../users-groups-roles/licensing-groups-assign.md).

### <a name="configure-sspr"></a>SSPR configureren

#### <a name="enable-groups-for-sspr"></a>Groepen voor self-service voor Wachtwoordherstel inschakelen

1. Toegang tot de Azure portal met een administrator-account.
1. Selecteer alle Services en in het filtervak typt u Azure Active Directory en selecteer vervolgens Azure Active Directory.
1. Selecteer op de blade Active Directory voor wachtwoord opnieuw instellen.
1. Selecteer in het eigenschappendeelvenster met voor geselecteerd. Als u wilt dat alle gebruikers ingeschakeld, wordt alles selecteren.
1. In het standaardwachtwoord beleidsblade voor opnieuw instellen, typt u de naam van de eerste groep selecteren en vervolgens klikt u op selecteren aan de onderkant van het scherm en selecteer opslaan boven aan het scherm.
1. Herhaal dit proces voor elke groep.

#### <a name="configure-the-authentication-methods"></a>De authenticatiemethoden configureren

Verwijzen naar de planning van de sectie Planning van wachtwoord-verificatiemethoden van dit document.

1. Selecteer registratie, onder vereisen dat gebruiker zich bij aanmelding registreren, selecteert u Ja en stel vervolgens het aantal dagen voordat het verloopt en selecteer vervolgens opslaan.
1. Melding, selecteren en configureren uw abonnementsgegevens en selecteer vervolgens opslaan.
1. Aanpassing, selecteren en configureren uw abonnementsgegevens en selecteer vervolgens opslaan.
1. On-premises integratie, selecteren en configureren uw abonnementsgegevens en selecteer vervolgens opslaan.

### <a name="enable-sspr-in-windows"></a>SSPR in Windows inschakelen

Windows 10-apparaten met versie 1803 of hoger die beide Azure AD zijn-domein of toegevoegd aan hybrid Azure AD kan hun wachtwoorden op het aanmeldingsscherm van Windows opnieuw instellen. Informatie en stappen voor het configureren van deze mogelijkheid kunnen worden gevonden in het artikel [Azure AD-wachtwoord opnieuw instellen vanuit het aanmeldingsscherm](tutorial-sspr-windows.md)

### <a name="configure-password-writeback"></a>Configureer wachtwoord terugschrijven

Stappen voor het configureren van Write-back van wachtwoord voor uw organisatie kan worden gevonden in het artikel [procedures: Configureer wachtwoord terugschrijven](howto-sspr-writeback.md).

## <a name="manage-sspr"></a>Self-service voor Wachtwoordherstel beheren

Vereiste functies voor het beheren van functies die zijn gekoppeld aan de self-service voor wachtwoord opnieuw instellen.

| Zakelijke rol/persona | Azure AD-rol (indien nodig) |
| :---: | :---: |
| Niveau 1 Helpdesk | Wachtwoordbeheerder |
| Niveau 2 Helpdesk | Gebruikerbeheerder |
| SSPR-beheerder | Globale beheerder |

### <a name="support-scenarios"></a>Ondersteuning van scenario 's

U kunt een veelgestelde vragen over het op basis van u van uw gebruikers ontvangen vragen maken zodat uw team ondersteuning succes. De volgende tabel bevat algemene scenario's voor ondersteuning.

| Scenario's | Description |
| --- | --- |
| Gebruiker beschikt niet over een geregistreerde verificatiemethoden die beschikbaar zijn | Een gebruiker probeert het wachtwoord opnieuw instelt, maar beschikt niet over een van de verificatiemethoden dat ze geregistreerd beschikbaar (voorbeeld: ze hun mobiele telefoon thuis linker- en geen toegang tot e-mail) |
| Gebruiker is niet ontvangen van een SMS-bericht of op hun office- of mobiele telefoon bellen | Een gebruiker probeert om te controleren of hun identiteit via tekst of gesprek, maar is niet ontvangen van een tekst per aanroep. |
| Gebruiker heeft geen toegang tot de portal voor wachtwoord opnieuw instellen | Een gebruiker wil om hun wachtwoord opnieuw instellen, maar is niet ingeschakeld voor wachtwoord opnieuw instellen en daarom geen toegang tot de pagina voor het bijwerken van wachtwoorden. |
| Gebruiker kan niet een nieuw wachtwoord instellen | Een gebruiker verificatie tijdens de stroom van wachtwoord opnieuw instellen is voltooid, maar zijn wel een nieuw wachtwoord kan niet worden ingesteld. |
| Gebruiker ziet een koppeling wachtwoord opnieuw instellen niet in een Windows 10-apparaat | Een gebruiker probeert opnieuw instellen van wachtwoord van het vergrendelingsscherm van Windows 10, maar het apparaat is ofwel niet gekoppeld aan Azure AD of de Intune-apparaatbeleid is niet ingeschakeld |

U kunt ook informatie op, zoals de volgende onderwerpen voor andere problemen oplossen.

- Welke groepen zijn ingeschakeld voor self-service voor Wachtwoordherstel.
- Welke verificatiemethoden zijn geconfigureerd.
- Het toegangsbeleid met betrekking tot op of van het bedrijfsnetwerk.
- Stappen voor probleemoplossing voor algemene scenario's.

U kunt ook verwijzen naar onze online documentatie over het oplossen van self-service voor wachtwoord opnieuw instellen voor meer informatie over de algemene stappen voor probleemoplossing voor de meest voorkomende scenario's voor self-service voor Wachtwoordherstel.

## <a name="next-steps"></a>Volgende stappen

- [Overweeg de implementatie van beveiliging voor Azure AD-wachtwoord](concept-password-ban-bad.md)

- [Overweeg de implementatie van Azure AD Smart Lockout](howto-password-smart-lockout.md)
