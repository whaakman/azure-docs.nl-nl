---
title: LinkedIn-voor het delen van gegevens van integratie en toestemming in Azure Active Directory | Microsoft Docs
description: Wordt uitgelegd hoe de gegevens via Microsoft-apps in Azure Active Directory voor het delen van LinkedIn-integratie
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 01/22/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 776c793fb797e9388f0852773315f27cd42dde25
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54471715"
---
# <a name="linkedin-integration-data-sharing-and-consent"></a>Delen van gegevens van LinkedIn integratie en toestemming

Als beheerder van Azure Active Directory (Azure AD) kunt u gebruikers in uw organisatie toe te staan verbinding maken met hun Microsoft-werk- of schoolaccount met hun LinkedIn-account. Wanneer gebruikers hun accounts koppelen, zijn informatie en hoogtepunten van LinkedIn beschikbaar in bepaalde Microsoft-apps en services. Gebruikers kunnen ook hun netwerkervaring verwachten op LinkedIn worden verbeterd en verrijkt met informatie van Microsoft.

LinkedIn-gegevens in Microsoft-apps en -services wilt bekijken, moeten gebruikers toestemming geven voor het verbinding maken met hun eigen Microsoft en LinkedIn-accounts. Gebruikers wordt gevraagd naar hun accounts koppelen de eerste keer dat ze op iemands LinkedIn om informatie te zien op een Profielkaart in Outlook, OneDrive of SharePoint Online. LinkedIn-accountverbindingen zijn niet volledig ingeschakeld voor uw gebruikers totdat ze toestemming geven de ervaring en hun accounts koppelen.

[!INCLUDE [active-directory-gdpr-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="benefits-of-sharing-linkedin-information"></a>Voordelen van het delen van gegevens van LinkedIn

Toegang tot de gegevens van LinkedIn binnen Microsoft-apps en services maakt het gemakkelijker voor uw gebruikers te verbinden, betrekken en professionele relaties bouwen met collega's, klanten en partners binnen en buiten uw organisatie. Nieuwe gebruikers kunnen krijgen op weg te helpen sneller door verbinding te maken met collega's, meer informatie over deze en eenvoudige toegang tot meer informatie. Hier volgt een voorbeeld van hoe LinkedIn-informatie wordt weergegeven op de Profielkaart in Microsoft-apps:

![LinkedIn-integratie inschakelen](./media/linkedin-user-consent/display-example.png)

## <a name="enable-and-announce-linkedin-integration"></a>Inschakelen en LinkedIn-integratie aankondigen

U moet een Azure Active Directory-beheerder voor het beheren van de instelling voor uw organisatie. U kunt deze inschakelen voor alle gebruikers, of voor een specifieke set gebruikers.

1. Als u wilt in- of uitschakelen van de integratie, volg de stappen in [LinkedIn-integratie](linkedin-integration.md).
2. Wanneer u de LinkedIn-integratie in uw organisatie aankondigen, wijst u uw gebruikers de veelgestelde vragen over [LinkedIn-gegevens in Microsoft-apps en services](https://support.office.com/article/about-linkedin-information-and-features-in-microsoft-apps-and-services-dc81cc70-4d64-4755-9f1c-b9536e34d381). Het artikel vindt u informatie over waar LinkedIn-informatie wordt weergegeven, hoe u verbinding maakt accounts en meer.

## <a name="user-consent-for-data-access-in-microsoft-and-linkedin"></a>Gebruiker toestemming geven voor toegang tot gegevens in Microsoft en LinkedIn

Gegevens die toegankelijk is vanuit LinkedIn is niet permanent opgeslagen in Microsoft-services. Gegevens die toegankelijk is vanuit Microsoft is niet permanent opgeslagen met LinkedIn, met uitzondering van de contactpersonen. Microsoft Contacts worden opgeslagen op LinkedIn totdat gebruikers verwijderen, zoals beschreven in [geïmporteerde contactpersonen verwijderen uit de LinkedIn](https://www.linkedin.com/help/linkedin/answer/43377).

Wanneer gebruikers hun accounts koppelen, zijn informatie en inzichten op basis van LinkedIn beschikbaar in bepaalde Microsoft-apps, zoals de Profielkaart. Gebruikers kunnen ook hun netwerkervaring verwachten op LinkedIn worden verbeterd en verrijkt met informatie van Microsoft.
Wanneer gebruikers in uw organisatie verbinding maken met hun Microsoft- en LinkedIn werk of school-accounts, zijn deze twee opties:

* Machtigen voor gegevens worden geopend vanaf beide accounts. Dit betekent dat ze voor hun Microsoft machtigen- of werkaccount voor toegang tot gegevens van hun LinkedIn-account en voor [hun LinkedIn-account om toegang te krijgen tot gegevens uit hun Microsoft-werk- of schoolaccount](https://www.linkedin.com/help/linkedin/answer/84077).
* Toestemming geven om alleen de LinkedIn-gegevens worden geopend door hun Microsoft werken en schoolaccount.

Gebruikers kunnen verbinding verbreken accounts en machtigingen voor gegevenstoegang op elk gewenst moment verwijderen en [gebruikers kunnen bepalen hoe hun eigen LinkedIn-profiel wordt weergegeven](https://www.linkedin.com/help/linkedin/answer/83), met inbegrip van of het profiel kan worden weergegeven in de Microsoft-apps.

### <a name="linkedin-account-data"></a>Gegevens van LinkedIn-account

Als u verbinding maakt met uw Microsoft en LinkedIn-accounts, kunt u LinkedIn om te zorgen dat de volgende gegevens naar Microsoft toestaan:

* Profielgegevens: geldt voor LinkedIn-identiteit, contactgegevens en de informatie die u met anderen delen op uw [LinkedIn-profiel](https://www.linkedin.com/help/linkedin/answer/15493).
* Gegevens interesseert - interesses op LinkedIn, bevat, zoals mensen en -onderwerpen die u volgt, cursussen groepen, en u inhoud, zoals en delen.
* Abonnementen data - bevat abonnementen op LinkedIn-toepassingen en services samen met de bijbehorende gegevens. 
* Gegevens van de verbindingen: bevat de [LinkedIn-netwerk](https://www.linkedin.com/help/linkedin/answer/110) met inbegrip van profielen en contactgegevens van uw verbindingen 1e graden.

Gegevens die toegankelijk is vanuit LinkedIn is niet permanent opgeslagen in Microsoft-services. Zie voor meer informatie over de Microsoft-gebruik van persoonlijke gegevens, de [privacyverklaring van Microsoft](https://privacy.microsoft.com/privacystatement/).

### <a name="microsoft-work-or-school-account-data"></a>Microsoft werk of school-accountgegevens

Als u verbinding maakt met uw Microsoft en LinkedIn-accounts, kunt u Microsoft voor de volgende gegevens voor LinkedIn toestaan:

* Profielgegevens: bevat informatie zoals uw voornaam, laatste naam, profielfoto, e-mailadres, beheer en mensen die u beheert.
* Agenda-gegevens - vergaderingen bevat in je agenda's, hun tijden, locaties en contactgegevens van deelnemers. Informatie over de vergadering, zoals agenda, inhoud of aan de wensen van titel is niet opgenomen in de agendagegevens.
* Gegevens interesseert: bevat het belang dat is gekoppeld aan uw account, op basis van uw gebruik van Microsoft-services, zoals Cortana en Bing voor bedrijven.
* Abonnementen data - omvat abonnementen die worden geleverd door uw organisatie naar Microsoft-apps en services, zoals Office 365.
* Contactpersonen van gegevens: bevat lijsten met contactpersonen in Outlook, Skype en andere Microsoft-account-services, met inbegrip van de contactgegevens voor mensen die u vaak communiceren of samenwerken met. Contactpersonen periodiek geïmporteerd, worden opgeslagen, en die worden gebruikt door LinkedIn, bijvoorbeeld verbindingen voorstellen, helpen bij het organiseren van contactpersonen en updates over contactpersonen weergeven.

Gegevens die toegankelijk is vanuit Microsoft is niet permanent opgeslagen met LinkedIn, met uitzondering van de contactpersonen. Microsoft Contacts worden opgeslagen op LinkedIn totdat deze gebruikers verwijderen. Meer informatie over [geïmporteerde contactpersonen verwijderen uit de LinkedIn](https://www.linkedin.com/help/linkedin/answer/43377).

Zie voor meer informatie over de LinkedIn-gebruik van persoonlijke gegevens, de [LinkedIn-privacybeleid](https://www.linkedin.com/legal/privacy-policy). Voor LinkedIn-services, overdracht van gegevens en opslag, gegevens uit de Europese Unie naar de Verenigde Staten en weer kan stromen en uw privacy is beveiligd als die in [gegevensoverdracht van de Europese Unie](https://www.linkedin.com/help/linkedin/answer/62533).

## <a name="next-steps"></a>Volgende stappen

* [LinkedIn in Microsoft-toepassingen met uw werk of school-account](https://www.linkedin.com/help/linkedin/answer/84077)
