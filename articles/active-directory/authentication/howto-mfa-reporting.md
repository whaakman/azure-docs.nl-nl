---
title: Toegang en gebruik rapporten voor Azure MFA - Azure Active Directory
description: Dit wordt beschreven hoe u de functie Azure multi-factor Authentication - rapporten.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f78a3135fca290d50370652b33fe0a4d16a6f83
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369043"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporten in de Azure multi-factor Authentication

Azure multi-factor Authentication biedt verschillende rapporten die kunnen worden gebruikt door u en uw organisatie die toegankelijk zijn via Azure portal. De volgende tabel bevat de beschikbare rapporten:

| Rapport | Locatie | Description |
|:--- |:--- |:--- |
| Geschiedenis geblokkeerde gebruikers | Azure AD > MFA-Server > gebruikers blokkeren/deblokkeren | Geeft de geschiedenis van aanvragen voor het blokkeren of deblokkeren van gebruikers. |
| Gebruik en fraude waarschuwingen | Azure AD > Sign-ins | Bevat informatie over algemene gebruik, overzicht van gebruikers en de details van de gebruiker; Als een geschiedenis van Fraudewaarschuwingen die zijn ingediend in het opgegeven datumbereik. |
| Gebruik voor on-premises onderdelen | Azure AD > MFA-Server > activiteitenrapport | Bevat informatie over algemene gebruik voor MFA via de NPS-extensie, ADFS, en de MFA-server. |
| Geschiedenis overgeslagen gebruikers | Azure AD > MFA-Server > eenmalige bypass | Biedt een geschiedenis van aanvragen voor het overslaan van multi-factor Authentication voor een gebruiker. |
| Serverstatus | Azure AD > MFA-Server > status van de Server | Hiermee wordt de status van multi-factor Authentication-Servers die zijn gekoppeld aan uw account. |

## <a name="view-mfa-reports"></a>MFA-rapporten weergeven

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer aan de linkerkant **Azure Active Directory** > **MFA-Server**.
3. Selecteer het rapport dat u wilt weergeven.

   ![Statusrapport van MFA-Server-server in Azure portal](./media/howto-mfa-reporting/report.png)

## <a name="azure-ad-sign-ins-report"></a>Rapport van Azure AD-aanmeldingen

Met de **activiteitenrapport voor aanmeldingen** in de [Azure-portal](https://portal.azure.com), krijgt u informatie die u nodig hebt om te bepalen hoe het gaat met uw omgeving.

Het aanmeldingenrapport kunt u voorzien van informatie over het gebruik van beheerde toepassingen en aanmelding bij activiteiten van gebruikers, waaronder informatie over het gebruik van multi-factor authentication (MFA). De MFA-gegevens geven u inzicht in hoe MFA werkt in uw organisatie. Hiermee kunt u vragen beantwoorden zoals:

- Is de aanmelding gelukt met MFA?
- Hoe heeft de gebruiker MFA voltooid?
- Waarom kan de gebruiker MFA niet voltooien?
- Hoeveel gebruikers gebruiken MFA?
- Hoeveel gebruikers kunnen MFA niet voltooien?
- Wat zijn de meest voorkomende MFA-problemen waarmee eindgebruikers te maken hebben?

Deze gegevens zijn beschikbaar via de [Azure-portal](https://portal.azure.com) en de [rapportage-API](../reports-monitoring/concept-reporting-api.md).

![Azure AD-aanmeldingen rapport in de Azure portal](./media/howto-mfa-reporting/sign-in-report.png)

### <a name="sign-ins-report-structure"></a>Structuur van de rapport-aanmeldingen

De rapporten voor aanmeldingsactiviteiten voor MFA bieden u toegang tot de volgende informatie:

**MFA vereist:** Of MFA is vereist voor de aanmelding of niet. MFA kan worden vereist vanwege MFA per gebruiker, voorwaardelijke toegang of andere redenen. Mogelijke waarden zijn **Ja** of **Nee**.

**MFA-resultaat:** Meer informatie over of MFA is voldaan of geweigerd:

- Als MFA is voldaan, bevat deze kolom meer informatie over hoe dit is gebeurd.
   - Azure Multi-Factor Authentication
      - voltooid in de cloud
      - verlopen als gevolg van beleid dat is geconfigureerd in de tenant
      - registratie gevraagd
      - voldaan door de claim in het token
      - voldaan door de claim geleverd door de externe provider
      - voldaan door geavanceerde verificatie
      - overgeslagen omdat de uitgevoerde stroom een Windows broker-aanmeldingsstroom is
      - overgeslagen vanwege een app-wachtwoord
      - overgeslagen vanwege de locatie
      - overgeslagen vanwege een geregistreerd apparaat
      - overgeslagen vanwege een geregistreerd apparaat
      - voltooid
   - Omgeleid naar externe provider voor Multi-Factor Authentication

- Als MFA is geweigerd, bevat deze kolom de reden van weigering.
   - Azure Multi-Factor Authentication is geweigerd
      - verificatie wordt uitgevoerd
      - dubbele verificatiepoging
      - onjuiste code te vaak ingevoerd
      - ongeldige verificatie
      - ongeldig verificatiecode via mobiele app
      - onjuiste configuratie
      - telefonische oproep verzonden naar voicemail
      - telefoonnummer heeft een ongeldige indeling
      - servicefout
      - kan telefoonnummer van gebruiker niet bereiken
      - kan de mobiele-app-melding niet verzenden naar het apparaat
      - kan de mobiele-app-melding niet verzenden
      - gebruiker heeft verificatie geweigerd
      - gebruiker heeft niet gereageerd op mobiele-app-melding
      - gebruiker heeft geen verificatiemethoden geregistreerd
      - gebruiker heeft onjuiste code ingevoerd
      - gebruiker heeft onjuiste pincode ingevoerd
      - gebruiker heeft opgehangen zonder te verifiëren
      - gebruiker is geblokkeerd
      - gebruiker heeft de verificatiecode niet ingevoerd
      - gebruiker is niet gevonden
      - verificatiecode is al gebruikt

**MFA-verificatiemethode:** De verificatiemethode de gebruiker gebruikt om MFA te voltooien. Mogelijke waarden:

- Sms-bericht
- Meldingen via mobiele app
- Telefonische oproep (verificatie via telefoonnummer)
- Verificatiecode via mobiele app
- Telefonische oproep (telefoon op het werk)
- Telefonische oproep (verificatie via telefoonnummer)

**Detail van MFA-verificatie:** Verwijderde versie van het telefoonnummer, bijvoorbeeld: + X XXXXXXXX64.

**Voorwaardelijke toegang** informatie vinden over beleid voor voorwaardelijke toegang die gevolgen van de aanmeldingspoging, waaronder ondervinden:

- Beleidsnaam
- Besturingselementen toekennen
- Sessiebesturingselementen
- Resultaat

## <a name="powershell-reporting"></a>Melden van PowerShell

Identificeer gebruikers die zich hebben geregistreerd voor MFA met behulp van PowerShell die volgt.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Gebruikers identificeren die niet hebt geregistreerd voor MFA met behulp van PowerShell die volgt.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Volgende stappen

* [Voor gebruikers](../user-help/multi-factor-authentication-end-user.md)
* [Waar u wilt implementeren](concept-mfa-whichversion.md)
