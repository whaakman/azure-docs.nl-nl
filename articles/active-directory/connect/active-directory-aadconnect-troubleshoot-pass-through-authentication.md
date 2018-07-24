---
title: 'Azure AD Connect: Pass through-verificatie oplossen | Microsoft Docs'
description: In dit artikel wordt beschreven hoe u Azure Active Directory (Azure AD) Pass through-verificatie oplossen.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie oplossen, Active Directory installeren, vereiste onderdelen voor Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 6cd6b139699b38a06a8e3f9fce5eb6e24fe24654
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214172"
---
# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Pass through-verificatie voor Azure Active Directory oplossen

In dit artikel helpt u bij het oplossen van problemen informatie over veelvoorkomende problemen met betrekking tot Azure AD Pass-through-verificatie.

>[!IMPORTANT]
>Als u gebruiker aanmelden problemen met Pass through-verificatie ondervindt, niet de functie uitschakelen of verwijderen van Pass through-verificatie-Agents zonder een cloud-only hoofdbeheerdersaccount terugvallen op. Meer informatie over [toevoegen van een alleen-cloud globale beheerdersaccount](../active-directory-users-create-azure-portal.md). U deze stap uitvoert, is van essentieel belang en zorgt ervoor dat u geen toegang meer hebt tot uw tenant.

## <a name="general-issues"></a>Algemene problemen

### <a name="check-status-of-the-feature-and-authentication-agents"></a>Controleer de status van de functie en de verificatie-Agents

Zorg ervoor dat de functie voor Pass through-verificatie nog steeds **ingeschakeld** op uw tenant en de status van verificatie-Agents bevat **Active**, en niet **inactief**. U kunt de status controleren door te gaan naar de **Azure AD Connect** blade in de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/).

![Azure Active Directory-beheercentrum - Azure AD Connect-blade](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory-beheercentrum - Pass through-verificatie-blade](./media/active-directory-aadconnect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Gebruikersgerichte aanmelden foutberichten

Als de gebruiker niet kan zich aanmelden bij het gebruik van Pass through-verificatie, zien zij mogelijk een van de volgende gebruikersgerichte fouten op het aanmeldingsscherm van Azure AD: 

|Fout|Beschrijving|Oplossing
| --- | --- | ---
|AADSTS80001|Kan geen verbinding maken met Active Directory|Zorg dat de agentservers deel uitmaken van hetzelfde AD-forest als de gebruikers waarvan de wachtwoorden moeten worden gevalideerd en ze zijn geen verbinding maken met Active Directory.  
|AADSTS8002|Er is een time-out opgetreden maken van verbinding met Active Directory|Controleren om ervoor te zorgen dat Active Directory beschikbaar is en op aanvragen van de agents reageert.
|AADSTS80004|De gebruikersnaam die is doorgegeven aan de agent is niet geldig|Zorg ervoor dat de gebruiker zich probeert aan te melden met de juiste gebruikersnaam.
|AADSTS80005|Validatie onvoorspelbare WebException opgetreden|Een tijdelijke fout. De aanvraag opnieuw. Als de pogingen mislukken blijven, moet u contact op met Microsoft ondersteuning.
|AADSTS80007|Er is een fout opgetreden communiceert met Active Directory|Controleer de agent-logboeken voor meer informatie en dat Active Directory wordt uitgevoerd zoals verwacht.

### <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Oorzaken voor het aanmelden mislukken op de Azure Active Directory-beheercentrum (Premium-licentie vereist)

Als uw tenant een Azure AD Premium-licentie die is gekoppeld heeft, kunt u ook zoeken op de [rapport van aanmeldingsactiviteiten](../active-directory-reporting-activity-sign-ins.md) op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/).

![Azure Active Directory-beheercentrum - rapport aanmeldingen](./media/active-directory-aadconnect-pass-through-authentication/pta4.png)

Navigeer naar **Azure Active Directory** -> **aanmeldingen** op de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/) en aanmeldingsactiviteiten voor een specifieke gebruiker op. Zoek de **FOUTCODE voor aanmelding door** veld. De waarde van dat veld worden toegewezen aan een reden van fout en een oplossing met behulp van de volgende tabel:

|Foutcode voor aanmelding|Reden van fout-aanmelding|Oplossing
| --- | --- | ---
| 50144 | Het Active Directory-wachtwoord van de gebruiker is verlopen. | Opnieuw instellen van wachtwoord van de gebruiker in uw on-premises Active Directory.
| 80001 | Er is geen verificatieagent beschikbaar. | Installeer en registreer een verificatie-Agent.
| 80002 | Er is een time-out opgetreden bij de wachtwoordvalidatie voor de verificatieagent. | Controleer of uw Active Directory bereikbaar is vanaf de verificatie-Agent is.
| 80003 | Ongeldig antwoord ontvangen door de verificatieagent. | Als het probleem consistent reproduceerbare voor meerdere gebruikers is, controleert u de configuratie van uw Active Directory.
| 80004 | Onjuiste UPN (user principal name) gebruikt voor aanmeldingsaanvraag. | Vraag de gebruiker zich aanmelden met de juiste gebruikersnaam.
| 80005 | Verificatieagent: er is een fout opgetreden. | Tijdelijke fout. Probeer het later opnieuw.
| 80007 | Verificatieagent kan geen verbinding maken met Active Directory. | Controleer of uw Active Directory bereikbaar is vanaf de verificatie-Agent is.
| 80010 | Verificatieagent kan wachtwoord niet ontsleutelen. | Als het probleem consistent reproduceerbare is, installeren en registreren van een nieuwe verificatie-Agent. En de huidige versie verwijderen. 
| 80011 | Verificatieagent kan ontsleutelingssleutel hier ophalen. | Als het probleem consistent reproduceerbare is, installeren en registreren van een nieuwe verificatie-Agent. En de huidige versie verwijderen.

## <a name="authentication-agent-installation-issues"></a>Verificatie-Agent installatieproblemen

### <a name="an-unexpected-error-occurred"></a>Er is een onverwachte fout opgetreden

[Agent-logboeken te verzamelen](#collecting-pass-through-authentication-agent-logs) van de server en neem contact op met Microsoft Support met uw probleem.

## <a name="authentication-agent-registration-issues"></a>Problemen met de registratie van de verificatie-Agent

### <a name="registration-of-the-authentication-agent-failed-due-to-blocked-ports"></a>Registratie van de verificatie-Agent is mislukt vanwege een geblokkeerde poorten

Zorg ervoor dat de server waarop de verificatie-Agent is geïnstalleerd met onze service communiceren kan-URL's en poorten vermeld [hier](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-the-prerequisites).

### <a name="registration-of-the-authentication-agent-failed-due-to-token-or-account-authorization-errors"></a>Registratie van de verificatie-Agent is mislukt vanwege een token of account-verificatiefouten

Zorg ervoor dat u een account van de globale beheerder alleen in de cloud voor alle Azure AD Connect of zelfstandige verificatie-Agent-installatie en registratiebewerkingen gebruiken. Er is een bekend probleem met accounts van globale beheerder MFA is ingeschakeld; MFA (alleen voor de bewerkingen worden voltooid) tijdelijk uitschakelen als tijdelijke oplossing.

### <a name="an-unexpected-error-occurred"></a>Er is een onverwachte fout opgetreden

[Agent-logboeken te verzamelen](#collecting-pass-through-authentication-agent-logs) van de server en neem contact op met Microsoft Support met uw probleem.

## <a name="authentication-agent-uninstallation-issues"></a>Problemen met verificatie-Agent verwijderen

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Waarschuwingsbericht wordt weergegeven tijdens het verwijderen van Azure AD Connect

Als u Pass-through-verificatie ingeschakeld in uw tenant hebt en u probeert te verwijderen van Azure AD Connect, wordt deze weergegeven u de volgende waarschuwing weergegeven: "gebruikers zich niet aanmelden bij Azure AD, tenzij u andere Pass through-verificatie-agents geïnstalleerd op hebt andere servers."

Zorg ervoor dat uw setup [hoog beschikbare](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability) voordat u Azure AD Connect om te voorkomen dat belangrijke aanmelden van gebruikers verwijderen.

## <a name="issues-with-enabling-the-feature"></a>Problemen bij het inschakelen van de functie

### <a name="enabling-the-feature-failed-because-there-were-no-authentication-agents-available"></a>Inschakelen van de functie is mislukt omdat er geen verificatie-Agents beschikbaar

U moet ten minste één active Authentication-Agent voor Pass through-verificatie inschakelen in uw tenant hebben. U kunt een verificatie-Agent te installeren Azure AD Connect, of een zelfstandige verificatie-Agent installeren.

### <a name="enabling-the-feature-failed-due-to-blocked-ports"></a>Inschakelen van de functie is mislukt vanwege een geblokkeerde poorten

Zorg ervoor dat de server waarop Azure AD Connect is geïnstalleerd met onze service communiceren kan-URL's en poorten vermeld [hier](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-the-prerequisites).

### <a name="enabling-the-feature-failed-due-to-token-or-account-authorization-errors"></a>Inschakelen van de functie is mislukt vanwege een token of account-verificatiefouten

Zorg ervoor dat u een account van de globale beheerder alleen in de cloud gebruiken bij het inschakelen van de functie. Er is een bekend probleem met multi-factor authentication (MFA)-accounts van globale beheerder; ingeschakeld MFA (alleen voor de bewerking is voltooid) tijdelijk uitschakelen als tijdelijke oplossing.

## <a name="collecting-pass-through-authentication-agent-logs"></a>Verzamelen Pass through-verificatie-Agent-logboeken

Afhankelijk van het type probleem dat u hebt, moet u zoeken op verschillende plaatsen voor Pass through-verificatie-Agent-logboeken.

### <a name="azure-ad-connect-logs"></a>Logboeken in Azure AD Connect

Voor fouten met betrekking tot installatie, controleert u de Azure AD Connect-logboeken op **%ProgramData%\AADConnect\trace-\*.log**.

### <a name="authentication-agent-event-logs"></a>Verificatie-Agent-logboeken

Voor fouten met betrekking tot de verificatie-Agent, opent u de toepassing Logboeken op de server en controleer onder **toepassing en Service Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**.

Voor gedetailleerde analyse inschakelen ' sessielogboek '. De verificatie-Agent niet uitvoeren met dit logboek tijdens normale bewerkingen; ingeschakeld Gebruik alleen voor het oplossen van problemen. Inhoud van het logboek zijn alleen zichtbaar nadat het logboek opnieuw is uitgeschakeld.

### <a name="detailed-trace-logs"></a>Gedetailleerde traceerlogboeken

Om op te lossen gebruiker aanmeldingen dat is toegestaan, zoek naar Logboeken met traceringen op **%ProgramData%\Microsoft\Azure AD verbinding maken met verificatie Agent\Trace\\**. Deze logboeken bevatten de redenen waarom een specifieke gebruiker aanmelden is mislukt met de functie voor Pass through-verificatie. Deze fouten worden ook is toegewezen aan de aanmelding mislukt redenen die wordt weergegeven in de voorgaande [tabel](#sign-in-failure-reasons-on-the-Azure-portal). Hieronder volgt een voorbeeld van invoer voor logboekbestand:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

U kunt beschrijvende informatie van de fout ('1328' in het voorgaande voorbeeld) krijgen door de opdrachtprompt te openen en de volgende opdracht uit (Opmerking: '1328' vervangen door de werkelijke foutnummer die u in uw logboeken ziet):

`Net helpmsg 1328`

![Pass-through-verificatie](./media/active-directory-aadconnect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Domeincontroller registreert

Als logboekregistratie is ingeschakeld, kunt u meer informatie vinden in de beveiligingslogboeken van uw domeincontrollers. Een eenvoudige manier om op te vragen van aanmeldingsaanvragen verzonden door Pass through-verificatie-Agents is als volgt:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

## <a name="performance-monitor-counters"></a>Prestatiemeteritems

Er is een andere manier voor het bewaken van verificatie-Agents voor het bijhouden van specifieke prestatiemeteritems op elke server waarop de verificatie-Agent is geïnstalleerd. Gebruik de volgende algemene items (**# PTA verificaties**, **#PTA mislukt verificaties** en **#PTA geslaagde verificaties**) en de foutentellers (**# PTA verificatiefouten**):

![Pass through-verificatie prestatiemeters](./media/active-directory-aadconnect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>Pass through-verificatie biedt hoge beschikbaarheid met behulp van meerdere verificatie-Agents en _niet_ taakverdeling. Afhankelijk van uw configuratie _niet_ alle verificatie-Agents ongeveer ontvangen _gelijk_ aantal aanvragen. Het is mogelijk dat een specifieke Verificatieagent helemaal geen verkeer ontvangt.
