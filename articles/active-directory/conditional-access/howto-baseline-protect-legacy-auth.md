---
title: Basislijn beleid blok verouderde verificatie (preview) - Azure Active Directory
description: Beleid voor voorwaardelijke toegang voor de verouderde verificatieprotocollen blokkeren
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7eebc68ae8a55d636f3bc85e179bd7d6813be8d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235560"
---
# <a name="baseline-policy-block-legacy-authentication-preview"></a>Beleid: Verouderde blok-verificatie (preview)

Als u wilt uw gebruikers eenvoudig toegang geven tot uw cloud-apps, Azure Active Directory (Azure AD) biedt ondersteuning voor tal van verificatieprotocollen, waaronder oudere authentication. Verouderde verificatie is een term die naar een verificatieaanvraag gemaakt verwijst door:

* Oudere Office-clients die geen gebruik maken van moderne verificatie (bijvoorbeeld Office 2010-client)
* Een willekeurige client die gebruikmaakt van verouderde e-mailprotocollen zoals IMAP/SMTP/POP3

Meerderheid van alle leveren op het gebied aanmeldingspogingen is vandaag, afkomstig van verouderde verificatie. Verouderde verificatie biedt geen ondersteuning voor multi-factor authentication (MFA). Zelfs als u een MFA-beleid dat is ingeschakeld op uw directory hebt, kunt een actor slecht verifiëren met behulp van een verouderde protocol en MFA overslaan.

De beste manier om uw account beschermen tegen schadelijke verificatieaanvragen door verouderde protocollen is deze pogingen alles bij elkaar blokkeren. Als u wilt het eenvoudiger voor u alle aanmeldingsaanvragen via verouderde protocollen te blokkeren, we een basislijn-beleid die precies dat doet gemaakt.

![Verouderde verificatie met voorwaardelijke toegang blokkeren](./media/howto-baseline-protect-legacy-auth/baseline-policy-block-legacy-authentication.png)

**Verouderde verificatie blok** is [Basisbeleid](concept-baseline-protection.md) die alle verificatieaanvragen van verouderde protocollen worden geblokkeerd. Moderne verificatie moet worden gebruikt om het aanmelden voor alle gebruikers. Gebruikt in combinatie met de basislijn-beleid, worden alle aanvragen die afkomstig zijn van verouderde protocollen geblokkeerd en alle gebruikers is vereist voor MFA wanneer vereist. Dit beleid worden niet geblokkeerd voor Exchange ActiveSync.

## <a name="identify-legacy-authentication-use"></a>Identificeren van verouderde verificatie gebruiken

Voordat u verouderde verificatie in uw directory blokkeren kunt, moet u eerst te begrijpen als uw gebruikers de apps die gebruikmaken van verouderde en hoe dit van invloed op uw algehele directory hebben. Azure AD aanmelden logboeken kunnen worden gebruikt om te begrijpen als u verouderde verificatie.

1. Navigeer naar de **Azure-portal** > **Azure Active Directory** > **aanmeldingen**.
1. De Client-App-kolom niet toevoegen als deze niet wordt weergegeven door te klikken op **kolommen** > **Client-App**.
1. Filteren op **Client-App** > **andere Clients** en klikt u op **toepassen**.

Filteren wordt alleen weergegeven u zich aanmeldt, probeert die zijn aangebracht door verouderde verificatieprotocollen. Te klikken op elke afzonderlijke aanmeldingspoging leert u meer informatie. De **Client-App** veld onder de **basisgegevens** tabblad wordt aangegeven welke verouderde verificatieprotocol is gebruikt.

Deze logboeken wordt aangegeven welke gebruikers zijn nog steeds afhankelijk van verouderde en welke toepassingen verouderde protocollen gebruiken om aan te verificatieaanvragen. Implementeren van beleid voor voorwaardelijke toegang voor gebruikers die niet in deze logboeken weergegeven en is bevestigd verouderde verificatie niet worden gebruikt, of schakel de **Basisbeleid: block verouderde verificatie** voor deze gebruikers alleen.

## <a name="moving-away-from-legacy-authentication"></a>Laten de legacy-verificatie

Zodra u een beter idee van wie verouderde verificatie gebruikt in uw directory en welke toepassingen afhankelijk zijn van deze hebt, is de volgende stap upgraden van uw gebruikers voor het gebruik van moderne verificatie. Moderne verificatie is een methode voor identiteitsbeheer die veiliger verificatie en autorisatie biedt. Als u een MFA-beleid in plaats van uw directory hebt, moderne verificatie zorgt ervoor dat de gebruiker wordt gevraagd voor MFA indien nodig. Dit is het veiliger alternatief voor verouderde verificatieprotocollen.

In deze sectie geeft een stapsgewijze overzicht over het bijwerken van uw omgeving met moderne verificatie. Lees de onderstaande stappen voordat u een oudere verificatie blokkeren van beleid in uw organisatie inschakelt.

### <a name="step-1-enable-modern-authentication-in-your-directory"></a>Stap 1: Moderne verificatie in uw directory inschakelen

De eerste stap bij het inschakelen van moderne verificatie wordt zorgen dat uw directory biedt ondersteuning voor moderne verificatie. Moderne verificatie is standaard ingeschakeld voor mappen die zijn gemaakt op of na 1 augustus 2017. Als uw directory is gemaakt vóór deze datum, moet u handmatig moderne verificatie inschakelen voor uw directory via de volgende stappen uit:

1. Controleer of de map al moderne verificatie door uit te voeren ondersteunt `Get-CsOAuthConfiguration` uit de [Skype voor bedrijven Online PowerShell-module](https://docs.microsoft.com/office365/enterprise/powershell/manage-skype-for-business-online-with-office-365-powershell).
1. Als uw opdracht een lege retourneert `OAuthServers` eigenschap en klik vervolgens moderne verificatie is uitgeschakeld. Werk de instelling om in te schakelen met behulp van moderne verificatie `Set-CsOAuthConfiguration`. Als uw `OAuthServers` eigenschap bevat een vermelding is, bent u aan de slag.

Zorg ervoor dat deze stap hebt voltooid voordat u verder gaat. Het is essentieel dat de configuraties van uw directory eerst worden gewijzigd omdat ze bepalen welk protocol wordt gebruikt door alle Office-clients. Zelfs als u van Office-clients die ondersteuning bieden voor moderne verificatie gebruikmaakt, standaard voor het gebruik van verouderde protocollen als moderne verificatie is uitgeschakeld in uw directory.

### <a name="step-2-office-applications"></a>Stap 2: Office-toepassingen

Als u moderne verificatie hebt ingeschakeld in uw directory, kunt u beginnen met het bijwerken van toepassingen door het inschakelen van moderne authenticatie voor Office-clients. Office 2016 of later clients ondersteuning voor moderne verificatie standaard. Er zijn geen extra stappen vereist.

Als u van Office 2013 Windows-clients gebruikmaakt of ouder, wordt aangeraden een upgrade naar Office 2016 of later. Zelfs na het voltooien van de vorige stap van het inschakelen van moderne verificatie in uw directory, de oudere Office-toepassingen wordt echter ook doorgaan met verouderde verificatieprotocollen. Als u van Office 2013 clients gebruikmaakt en kan niet direct een upgrade uitvoert naar Office 2016 of hoger, volgt u de stappen in het volgende artikel voor [moderne verificatie inschakelen voor Office 2013 op Windows-apparaten](https://docs.microsoft.com/office365/admin/security-and-compliance/enable-modern-authentication). Ter bescherming van uw account terwijl u verouderde verificatie, wordt u aangeraden sterke wachtwoorden in uw directory. Bekijk [Azure AD-wachtwoordbeveiliging](../authentication/concept-password-ban-bad.md) te bannen zwakke wachtwoorden in uw directory.

Office 2010 biedt geen ondersteuning voor moderne verificatie. U moet alle gebruikers met Office 2010 upgraden naar een recentere versie van Office. Wordt aangeraden een upgrade naar Office 2016 of hoger, omdat deze verouderde verificatie standaard geblokkeerd.

Als u Mac OS, wordt u aangeraden een upgrade naar Office voor Mac 2016 of hoger. Als u van de systeemeigen e-mailclient gebruikmaakt, moet u MacOS version 10.14 of later op alle apparaten.

### <a name="step-3-exchange-and-sharepoint"></a>Stap 3: Exchange en SharePoint

Voor Outlook op basis van een Windows-clients gebruiken moderne verificatie, moet Exchange Online moderne verificatie ook ingeschakeld. Als de moderne verificatie is uitgeschakeld voor Exchange Online, Outlook op basis van een Windows-clients die ondersteuning bieden voor moderne verificatie (Outlook 2013 of later) basisverificatie verbinding maken met de postvakken van Exchange Online gebruikt.

SharePoint Online is ingeschakeld voor moderne verificatie standaard. Voor de mappen die zijn gemaakt na 1 augustus 2017, is moderne verificatie standaard ingeschakeld in Exchange Online. Echter, als u moderne verificatie eerder was uitgeschakeld of u een map die zijn gemaakt vóór deze datum gebruikt, volgt u de stappen in het volgende artikel voor [moderne verificatie inschakelen in Exchange Online](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online).

### <a name="step-4-skype-for-business"></a>Stap 4: Skype voor Bedrijven

Om te voorkomen dat een verouderde verificatieaanvragen door Skype voor bedrijven, is het nodig om moderne verificatie inschakelen voor Skype voor bedrijven Online. Moderne verificatie voor Skype voor bedrijven is standaard ingeschakeld voor de mappen die zijn gemaakt na 1 augustus 2017.

We raden u overgang naar Microsoft Teams, die ondersteuning biedt voor moderne verificatie standaard zodat moderne verificatie in Skype voor bedrijven. Echter, als u zich niet tr op dit moment, u moet moderne verificatie voor Skype voor bedrijven Online inschakelen zodat Skype voor bedrijven-clients wordt gestart die moderne authenticatie gebruiken. Volg deze stappen in het artikel [Skype voor bedrijven-topologieën met moderne verificatie ondersteund](https://docs.microsoft.com/skypeforbusiness/plan-your-deployment/modern-authentication/topologies-supported), voor de stappen voor het moderne verificatie inschakelen voor Skype voor bedrijven.

Naast het inschakelen van moderne verificatie voor Skype voor bedrijven Online, wordt aangeraden moderne verificatie worden ingeschakeld voor Exchange Online bij het inschakelen van moderne verificatie voor Skype voor bedrijven. Dit proces wordt te synchroniseren van de status van moderne verificatie in Exchange Online en Skype voor bedrijven online en wordt voorkomen dat meerdere aanmeldingsprompts voor Skype voor bedrijven-clients.

### <a name="step-5-using-mobile-devices"></a>Stap 5: Met behulp van mobiele apparaten

Toepassingen op uw mobiele apparaat moeten ook verouderde verificatie blokkeren. Het is raadzaam om met behulp van Outlook voor mobiele apparaten. Outlook Mobile biedt standaard ondersteuning voor moderne verificatie en, voldoen aan de andere MFA basislijn beveiligingsbeleid voor apps.

Als u wilt de systeemeigen iOS-e-mailclient gebruiken, moet u 11.0 of hoger om te controleren of dat de e-mailclient is bijgewerkt voor het blokkeren van verouderde iOS-versie worden uitgevoerd.

### <a name="step-6-on-premises-clients"></a>Stap 6: On-premises clients

Als u een Exchange Server on-premises en Skype voor bedrijven on-premises met hybride-klant bent, moet beide services worden bijgewerkt om moderne verificatie inschakelen. Bij het gebruik van moderne verificatie in een hybride omgeving, bent u nog steeds verificatie van gebruikers on-premises. Het verhaal van het verlenen van hun toegang tot resources (bestanden of e-mailberichten) wijzigingen.

Voordat u met het inschakelen van moderne verificatie beginnen kunt on-premises, moet u voldoen aan de theIf u voldoet aan de vereisten, u bent nu klaar om in te schakelen van moderne verificatie op locatie.

Stappen voor het inschakelen van moderne verificatie vindt u in de volgende artikelen:

* [Het configureren van Exchange Server on-premises naar hybride moderne verificatie gebruiken](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
* [Moderne verificatie (ADAL) gebruiken met Skype voor bedrijven](https://docs.microsoft.com/skypeforbusiness/manage/authentication/use-adal)

## <a name="enable-the-baseline-policy"></a>De basislijn-beleid inschakelen

Het beleid **Basisbeleid: Verouderde blok-verificatie (preview)** wordt al geconfigureerd geleverd en worden weergegeven aan de bovenkant wanneer u gaat u naar de blade voor voorwaardelijke toegang in Azure portal.

Dit beleid inschakelt en bescherming van uw organisatie:

1. Aanmelden bij de **Azure-portal** als hoofdbeheerder, beveiligingsbeheerder of beheerder van voorwaardelijke toegang.
1. Blader naar **Azure Active Directory** > **voorwaardelijke toegang**.
1. Selecteer in de lijst met beleidsregels **Basisbeleid: Verouderde blok-verificatie (preview)** .
1. Stel **beleid inschakelen** naar **beleid direct gebruiken**.
1. Gebruiker uitsluitingen toevoegen door te klikken op **gebruikers** > **uitgesloten gebruikers selecteren** en het kiezen van de gebruikers die moeten worden uitgesloten. Klik op **Selecteer** vervolgens **gedaan**.
1. Klik op **opslaan**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie:

* [Basislijn protection-beleid voor voorwaardelijke toegang](concept-baseline-protection.md)
* [Vijf stappen voor het beveiligen van uw infrastructuur voor identiteiten](../../security/azure-ad-secure-steps.md)
* [Wat is voorwaardelijke toegang in Azure Active Directory?](overview.md)
