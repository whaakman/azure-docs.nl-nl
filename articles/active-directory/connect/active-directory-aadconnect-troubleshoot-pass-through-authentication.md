---
title: 'Azure AD Connect: Problemen met Pass through-verificatie | Microsoft Docs'
description: Dit artikel wordt beschreven hoe u problemen met Azure Active Directory (Azure AD) Pass through-verificatie.
services: active-directory
keywords: Problemen met Azure AD Connect Pass through-verificatie, installeert u Active Directory, de vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: billmath
ms.openlocfilehash: b842791be74094c87643528c0b4d3a65be6b3cb1
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Problemen met Azure Active Directory Pass-through-verificatie

Dit artikel helpt u bij het oplossen van problemen informatie over veelvoorkomende problemen met betrekking tot Azure AD Pass-through-verificatie.

>[!IMPORTANT]
>Als u worden geconfronteerd gebruiker aanmeldingsproblemen met Pass through-verificatie, niet de functie uitschakelen of Pass through-verificatie Agents verwijderen zonder een cloudconfiguratie globale beheerdersaccount terugvallen op. Meer informatie over [toevoegen van een cloudconfiguratie globale beheerdersaccount](../active-directory-users-create-azure-portal.md). Deze stap is van essentieel belang en zorgt ervoor dat u geen toegang buiten uw tenant.

## <a name="general-issues"></a>Algemene problemen

### <a name="check-status-of-the-feature-and-authentication-agents"></a>Controleer de status van de functie en de verificatie-Agents

Zorg ervoor dat de functie Pass through-verificatie nog steeds **ingeschakeld** op uw tenant en de status van agenten verificatie toont **Active**, en niet **inactief**. U kunt de status controleren door te gaan naar de **Azure AD Connect** blade in de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/).

![Azure Active Directory-beheercentrum - blade van Azure AD Connect](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory-beheercentrum - blade Pass through-verificatie](./media/active-directory-aadconnect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Gebruikersgerichte aanmelden foutberichten

Als de gebruiker niet kan aanmelden bij het gebruik van Pass through-verificatie, kunnen ze een van de volgende gebruikersgerichte fouten op het Azure AD-aanmeldingsscherm zien: 

|Fout|Beschrijving|Oplossing
| --- | --- | ---
|AADSTS80001|Kan geen verbinding maken met Active Directory|Zorg dat de agentservers deel uitmaken van hetzelfde AD-forest als de gebruikers waarvan de wachtwoorden moeten worden gevalideerd en ze kunnen geen verbinding maken met Active Directory.  
|AADSTS8002|Er is een time-out opgetreden tijdens het maken van verbinding met Active Directory|Controleer of Active Directory beschikbaar is en op aanvragen van de agents reageert.
|AADSTS80004|De gebruikersnaam die is doorgegeven aan de agent is niet geldig|Zorg ervoor dat de gebruiker probeert zich kunnen aanmelden met de juiste gebruikersnaam.
|AADSTS80005|Validatie onvoorspelbare WebException aangetroffen|Een tijdelijke fout. De aanvraag opnieuw proberen. Als dit mislukken blijft, moet u contact op met Microsoft ondersteuning.
|AADSTS80007|Er is een fout opgetreden communiceert met Active Directory|Controleer de logboeken van de agent voor meer informatie en dat Active Directory werkt zoals verwacht.

### <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Aanmelding mislukt redenen op het Azure Active Directory-beheercentrum (Premium-licentie vereist)

Als uw tenant een Azure AD Premium-licentie die is gekoppeld heeft, kunt u ook zoeken op de [aanmeldingsactiviteiten rapport](../active-directory-reporting-activity-sign-ins.md) op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/).

![Azure Active Directory-beheercentrum - aanmeldingen rapport](./media/active-directory-aadconnect-pass-through-authentication/pta4.png)

Navigeer naar **Azure Active Directory** -> **aanmeldingen** op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/) en klik op een specifieke gebruiker aanmelden activiteit. Zoek naar de **SIGN-IN-FOUTCODE** veld. De waarde van dat veld worden toegewezen aan een reden voor fout en de oplossing met behulp van de volgende tabel:

|Foutcode voor aanmelding|Aanmelding mislukt reden|Oplossing
| --- | --- | ---
| 50144 | Het Active Directory-wachtwoord van de gebruiker is verlopen. | Wachtwoord van de gebruiker in uw lokale Active Directory worden ingesteld.
| 80001 | Er is geen verificatieagent beschikbaar. | Installeren en registreren van een verificatie-Agent.
| 80002 | Er is een time-out opgetreden bij de wachtwoordvalidatie voor de verificatieagent. | Controleer of uw Active Directory bereikbaar is vanaf de verificatie-Agent is.
| 80003 | Ongeldig antwoord ontvangen door de verificatieagent. | Als het probleem consistent reproduceerbare in meerdere gebruikers is, controleert u uw Active Directory-configuratie.
| 80004 | Onjuiste UPN (user principal name) gebruikt voor aanmeldingsaanvraag. | Vraag de gebruiker zich aanmelden met de juiste gebruikersnaam.
| 80005 | Verificatieagent: er is een fout opgetreden. | Tijdelijke fout. Probeer het later opnieuw.
| 80007 | Verificatieagent kan geen verbinding maken met Active Directory. | Controleer of uw Active Directory bereikbaar is vanaf de verificatie-Agent is.
| 80010 | Verificatieagent kan wachtwoord niet ontsleutelen. | Als het probleem consistent reproduceerbare is, installeren en registreren van een nieuwe Agent voor verificatie. En de huidige versie verwijderen. 
| 80011 | Verificatieagent kan ontsleutelingssleutel hier ophalen. | Als het probleem consistent reproduceerbare is, installeren en registreren van een nieuwe Agent voor verificatie. En de huidige versie verwijderen.

## <a name="authentication-agent-installation-issues"></a>Agent-installatie verificatieproblemen

### <a name="an-unexpected-error-occurred"></a>Er is een onverwachte fout opgetreden

[Verzamelen van Logboeken van de agent](#collecting-pass-through-authentication-agent-logs) naar de server en neem contact op met Microsoft Support met het probleem.

## <a name="authentication-agent-registration-issues"></a>Problemen met de registratie van de verificatie-Agent

### <a name="registration-of-the-authentication-agent-failed-due-to-blocked-ports"></a>Registratie van de verificatie-Agent is mislukt vanwege geblokkeerde poorten

Zorg ervoor dat de server waarop de verificatie-Agent is geïnstalleerd met onze service communiceren kan-URL's en poorten vermeld [hier](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-the-prerequisites).

### <a name="registration-of-the-authentication-agent-failed-due-to-token-or-account-authorization-errors"></a>Registratie van de verificatie-Agent is mislukt vanwege een token of account autorisatie-fouten

Zorg ervoor dat u een cloudconfiguratie globale beheerdersaccount voor Azure AD Connect of zelfstandige verificatie-Agent-installatie en registratie-bewerkingen. Er is een bekend probleem met MFA ingeschakeld globale beheerdersaccounts; MFA tijdelijk (alleen voor de bewerkingen) uitschakelen als tijdelijke oplossing.

### <a name="an-unexpected-error-occurred"></a>Er is een onverwachte fout opgetreden

[Verzamelen van Logboeken van de agent](#collecting-pass-through-authentication-agent-logs) naar de server en neem contact op met Microsoft Support met het probleem.

## <a name="authentication-agent-uninstallation-issues"></a>Verificatieproblemen Agent verwijderen

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Waarschuwingsbericht tijdens het verwijderen van Azure AD Connect

Als u Pass-through-verificatie ingeschakeld op uw tenant zijn en u probeert te verwijderen van Azure AD Connect, u ziet de volgende waarschuwing weergegeven: "Gebruikers zich niet kunt aanmelden bij Azure AD tenzij u andere Pass through-verificatie agents zijn geïnstalleerd op andere servers hebt."

Zorg ervoor dat de instellingen van uw [hoge beschikbaar](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) voordat u Azure AD Connect om te voorkomen dat gebruikers zich aanmelden verwijderen.

## <a name="issues-with-enabling-the-feature"></a>Problemen met het inschakelen van de functie

### <a name="enabling-the-feature-failed-because-there-were-no-authentication-agents-available"></a>Inschakelen van de functie is mislukt, omdat er geen Agents verificatie beschikbaar zijn

U moet ten minste één actieve verificatie-Agent in te schakelen van Pass through-verificatie voor uw tenant hebben. U kunt een verificatie-Agent installeren Azure AD Connect of een zelfstandige verificatie-Agent installeren.

### <a name="enabling-the-feature-failed-due-to-blocked-ports"></a>Inschakelen van de functie is mislukt vanwege een geblokkeerde poorten

Zorg ervoor dat de server waarop Azure AD Connect is geïnstalleerd met onze service communiceren kan-URL's en poorten vermeld [hier](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-the-prerequisites).

### <a name="enabling-the-feature-failed-due-to-token-or-account-authorization-errors"></a>Inschakelen van de functie is mislukt vanwege een token of account autorisatie-fouten

Zorg ervoor dat u een cloudconfiguratie globale beheerdersaccount gebruiken wanneer u deze functie inschakelt. Er is een bekend probleem met multi-factor authentication (MFA)-globale beheerdersaccounts; ingeschakeld MFA (alleen voor de bewerking niet voltooien) tijdelijk uitschakelen als tijdelijke oplossing.

## <a name="exchange-activesync-configuration-issues"></a>Problemen met Exchange ActiveSync-configuratie

Dit zijn veelvoorkomende problemen bij het configureren van Exchange ActiveSync-ondersteuning voor verificatie met Pass-through.

### <a name="exchange-powershell-issue"></a>Exchange PowerShell probleem

Als u ziet de '**een parameter kan niet worden gevonden die overeenkomt met de naam van de 'PerTenantSwitchToESTSEnabled'\.**' Fout tijdens het uitvoeren van de `Set-OrganizationConfig` Exchange PowerShell opdracht, neem contact op met Microsoft Support.

### <a name="exchange-activesync-not-working"></a>Exchange ActiveSync werkt niet

De configuratie neemt enige tijd in te voeren: de periode is afhankelijk van uw omgeving. Als de situatie zich blijft gedurende een lange periode voordoen, neem dan contact op met Microsoft Support.

## <a name="collecting-pass-through-authentication-agent-logs"></a>Logboeken verzamelen Pass through-verificatie-Agent

Afhankelijk van het type van het probleem wellicht moet u op verschillende plaatsen voor Pass-through-verificatie-Agent logboeken bekijken.

### <a name="azure-ad-connect-logs"></a>Azure AD Connect-Logboeken

Voor fouten met betrekking tot de installatie, Controleer de logboeken van de Azure AD Connect op **%ProgramData%\AADConnect\trace-\*.log**.

### <a name="authentication-agent-event-logs"></a>De gebeurtenislogboeken van de verificatie-Agent

Voor fouten met betrekking tot de verificatie-Agent, opent u de toepassing Logboeken op de server en controleer onder **toepassing en Service Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**.

Voor gedetailleerde analytics inschakelen ' sessielogboek '. De verificatie-Agent niet uitvoeren met dit logboek ingeschakeld tijdens normale bewerkingen. alleen gebruiken voor het oplossen van problemen. Inhoud van het logboek zijn alleen zichtbaar nadat het logboek opnieuw is uitgeschakeld.

### <a name="detailed-trace-logs"></a>Gedetailleerde traceerlogboeken

Voor het oplossen van de gebruiker, mislukte aanmeldingen zoekt traceringslogboeken op **%ProgramData%\Microsoft\Azure AD Authentication Agent\Trace verbinding\\**. Deze logboeken bevatten redenen waarom een specifieke gebruiker aanmelden is mislukt met de functie Pass through-verificatie. Deze fouten ook is toegewezen aan de aanmelding mislukt redenen weergegeven in de voorgaande [tabel](#sign-in-failure-reasons-on-the-Azure-portal). Hier volgt een voorbeeld van invoer voor logboek:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

U kunt beschrijvende details van de fout ('1328' in het voorgaande voorbeeld) ophalen door het openen van de opdrachtprompt en voer de volgende opdracht (Opmerking: '1328' vervangen door de werkelijke foutnummer die u in uw logboeken ziet):

`Net helpmsg 1328`

![Pass-through-verificatie](./media/active-directory-aadconnect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Registreert domeincontroller

Als logboekregistratie is ingeschakeld, kunt u meer informatie vinden in de beveiligingslogboeken van uw domeincontrollers. Een eenvoudige manier om te zoeken aanmelden aanvragen verzonden door de Agents van Pass through-verificatie is als volgt:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

## <a name="performance-monitor-counters"></a>Prestatiemeteritems

Verificatie-Agents bewaken op een andere manier is om bij te houden van specifieke prestatiemeteritems op elke server waarop de verificatie-Agent is geïnstalleerd. Gebruik de volgende algemene items (**# PTA verificaties**, **#PTA mislukt verificaties** en **#PTA geslaagde verificaties**) en fout-prestatiemeteritems (**verificatiefouten # PTA**):

![Pass through-verificatie Prestatiemeter](./media/active-directory-aadconnect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>Pass through-verificatie biedt hoge beschikbaarheid met behulp van meerdere verificatie Agents en _niet_ taakverdeling. Afhankelijk van uw configuratie _niet_ alle verificatie-Agents ongeveer ontvangen _gelijk_ aantal aanvragen. Het is mogelijk dat een specifieke verificatie-Agent helemaal geen verkeer ontvangt.
