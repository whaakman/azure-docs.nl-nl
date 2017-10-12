---
title: Foutcodes voor aanmeldactiviteitenrapporten in Azure Active Directory Portal | Microsoft Docs
description: Naslaginformatie over foutcodes voor aanmeldactiviteitenrapporten.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 2a1b7b87df2cd8fa2e98f217480b46f5f6334297
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="sign-in-activity-report-error-codes-in-the-azure-active-directory-portal"></a>Foutcodes voor aanmeldactiviteitenrapporten in Azure Active Directory Portal

In de informatie die wordt aangeboden in het rapport over aanmeldactiviteiten van gebruikers, vindt u antwoord op vragen zoals:

- Wie heeft zich aangemeld met Azure Active Directory?
- Bij welke apps hebben gebruikers zich aangemeld?
- Welke aanmeldingen zijn mislukt en waarom?

In dit onderwerp worden de foutcodes en de bijbehorende beschrijvingen genoemd. 

## <a name="how-can-i-display-failed-sign-ins"></a>Hoe kan ik mislukte aanmeldingen weergeven? 

Uw startpunt voor alle aanmeldingsactiviteitgegevens is **[Aanmeldingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)** in het gedeelte **Activiteit** van **Azure Active**.


![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins-errors/61.png "Aanmeldingsactiviteit")


In het aanmeldingenrapport kunt u alle mislukte aanmeldingen weergeven door **Mislukt** te selecteren als **Aanmeldstatus**.


![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins-errors/06.png "Aanmeldingsactiviteit")

Wanneer u op een item in de weergegeven lijst klikt, wordt de blade **Activiteitendetails: aanmeldingen** geopend. Deze weergave biedt u alle details die Azure Active Directory bijhoudt voor aanmeldingen, met inbegrip van de **Foutcode voor aanmelding** en een **Reden van fout**.

![Aanmeldingsactiviteit](./media/active-directory-reporting-activity-sign-ins-errors/05.png "Aanmeldingsactiviteit")


In plaats van Azure Portal kunt u ook de [rapportage-API](active-directory-reporting-api-getting-started-azure-portal.md) gebruiken voor toegang tot aanmeldingsgegevens.


De volgende sectie biedt een volledig overzicht van alle mogelijke fouten en de bijbehorende beschrijvingen. 

## <a name="error-codes"></a>Foutcodes

| Fout| Beschrijving |
| --- | --- |
| 50001| De service-principal X is niet gevonden in de tenant Y. Dit kan gebeuren als de toepassing niet is geïnstalleerd door de beheerder van de tenant. Of de resource-principal is niet gevonden in de map of is ongeldig|
| 50008| SAML-verklaring ontbreekt of is niet juist geconfigureerd in het token.|
| 50011| Het antwoordadres ontbreekt, is onjuist geconfigureerd of komt niet overeen met de antwoordadressen die voor de toepassing zijn geconfigureerd.|
| 50053| Uw account is vergrendeld omdat u zich te vaak hebt aangemeld met een onjuiste gebruikers-id of een onjuist wachtwoord.|
| 50054| Er is een oud wachtwoord gebruikt voor verificatie.|
| 50055| Het opgegeven wachtwoord is ongeldig of verlopen.|
| 50057| Het gebruikersaccount is uitgeschakeld.|
| 50058| Er is in de opgegeven referenties geen informatie over de identiteit van de gebruiker gevonden, de gebruiker kan niet worden gevonden in de tenant of er is een aanmeldverzoek op de achtergrond verzonden, maar er is geen gebruiker aangemeld of de service kan de gebruiker niet verifiëren.|
| 50074| Sterke verificatie (tweede factor) is vereist|
| 50079| Gebruiker moet zich registreren voor verificatie met een tweede factor|
| 50126| Gebruikersnaam of wachtwoord is ongeldig of on-premises gebruikersnaam of wachtwoord is ongeldig.|
| 50131| Wordt gebruikt voor verschillende fouten voor voorwaardelijke toegang. Bijvoorbeeld bij slechte Windows-apparaatstatus, geblokkeerde aanvraag vanwege verdachte activiteit, beslissingen voor toegangsbeleid en beveiligingsbeleid.|
| 50133| Sessie is ongeldig omdat het wachtwoord is verlopen of recent is gewijzigd.|
| 50144| Het Active Directory-wachtwoord van de gebruiker is verlopen.|
| 65001| Toepassing X heeft geen toegangsmachtiging voor toepassing Y of de machtiging is ingetrokken. Of de gebruiker of beheerder heeft niet toegestaan dat de toepassing wordt gebruikt met id X. Stuur een interactieve autorisatieaanvraag voor deze gebruiker en resource. Of de gebruiker of beheerder heeft niet toegestaan dat de toepassing wordt gebruikt met id X. Stuur een autorisatieaanvraag naar uw tenant-beheerder uit naam van toepassing Y voor resource Z.|
| 65005| De voor de toepassing vereiste resourcetoegangslijst bevat geen toepassingen die kunnen worden gedetecteerd door de resource, de clienttoepassing heeft toegang aangevraagd tot een resource die niet is opgegeven in de vereiste resourcetoegangslijst, de Graph-service heeft een onjuiste aanvraag geretourneerd of de resource is niet gevonden.|
| 70001| De toepassing X is niet gevonden in de tenant Y. Dit kan gebeuren als de toepassing niet is geïnstalleerd door de beheerder van de tenant of er geen toestemming voor is verleend door een gebruiker in de tenant. Mogelijk hebt u de verificatieaanvraag naar de verkeerde tenant verzonden.|
| 80001| Er is geen verificatieagent beschikbaar.|
| 80002| Er is een time-out opgetreden bij de wachtwoordvalidatie voor de verificatieagent.|
| 80003| Ongeldig antwoord ontvangen door de verificatieagent.|
| 80004| Onjuiste UPN (user principal name) gebruikt voor aanmeldingsaanvraag.|
| 80005| Verificatieagent: er is een fout opgetreden.|
| 80007| Verificatieagent kan geen verbinding maken met Active Directory.|
| 80010| Verificatieagent kan wachtwoord niet ontsleutelen.|
| 81001| Kerberos-ticket van de gebruiker is te groot.|
| 81002| Kan Kerberos-ticket van de gebruiker niet valideren.|
| 81003| Kan Kerberos-ticket van de gebruiker niet valideren.|
| 81004| Poging tot Kerberos-verificatie is mislukt.|
| 81008| Kan Kerberos-ticket van de gebruiker niet valideren.|
| 81009| Kan Kerberos-ticket van de gebruiker niet valideren.|
| 81010| Naadloze eenmalige aanmelding is mislukt omdat het Kerberos-ticket van de gebruiker is verlopen of ongeldig is.|
| 81011| Kan gebruikersobject niet vinden op basis van de informatie in het Kerberos-ticket van de gebruiker.|
| 81012| De gebruiker die zich probeert aan te melden bij Azure AD, komt niet overeen met de gebruiker die is aangemeld bij het apparaat.|
| 81013| Kan gebruikersobject niet vinden op basis van de informatie in het Kerberos-ticket van de gebruiker.|
| 90014| Wordt gebruikt in verschillende gevallen waarbij een verwacht veld niet aanwezig is in de referentie.|
| 90093| De grafiek is geretourneerd met een niet-toegestane foutcode voor de aanvraag.|



## <a name="next-steps"></a>Volgende stappen

Raadpleeg [Aanmeldactiviteitenrapporten in Azure Active Directory Portal](active-directory-reporting-activity-sign-ins.md) voor meer informatie.

