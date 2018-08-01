---
title: Toepassingen beheren met Azure Active Directory | Microsoft Docs
description: Dit artikel de voordelen van Azure Active Directory integreren met uw on-premises, cloud en SaaS-toepassingen.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: bf53829a2d2578132f9a3595c0bac5e8eb588916
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366768"
---
# <a name="managing-applications-with-azure-active-directory"></a>Toepassingen beheren met Azure Active Directory
Bedrijven hebben dan de feitelijke werkstroom of de inhoud, twee basisvereisten voor alle toepassingen:

1. Als u wilt de productiviteit verhogen, moeten toepassingen gemakkelijk om te detecteren en toegang tot
2. Als u wilt inschakelen van beveiliging en governance, moet de organisatie controle en het overzicht over wie toegang heeft tot en wie daadwerkelijk elke toepassing

In de wereld van cloud-toepassingen is dit best kan worden bereikt met behulp van de identiteit van besturingselement "*die is toegestaan om te doen wat*."

In het computergebruik terminologie:

* *Wie* staat bekend als *identiteit* -het beheer van gebruikers en groepen
* *Wat* staat bekend als *toegangsbeheer* : het beheer van toegang tot beveiligde bronnen

Beide onderdelen samen worden aangeduid als *Identity and Access Management (IAM)*, die is gedefinieerd door de [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) als een groep '*de discipline beveiliging die kan de juiste personen toegang tot de juiste resources aan de rechterkant tijden voor de redenen waarom*'.

Oké, dus wat is het probleem? Als IAM *niet worden beheerd* op één locatie met een geïntegreerde oplossing:

* Identiteitsbeheerders moeten afzonderlijk maken en bijwerken van gebruikersaccounts in alle toepassingen afzonderlijk, een activiteit redundante en tijd in beslag nemen.
* Gebruikers moeten kunnen meerdere referenties voor toegang tot de toepassingen die ze nodig hebben om te werken met onthouden. Als gevolg hiervan vaak gebruikers Schrijf op hun wachtwoorden of andere oplossingen voor wachtwoordbeheer gebruiken. Alternatieve mogelijkheden introduceren gegevens beveiligingsrisico's.
* Redundante tijd in beslag nemen activiteiten verminderen de gebruikers en beheerders besteden aan zakelijke activiteiten waarmee u de winstgevendheid van uw bedrijf te verhogen.

Wat in het algemeen wordt voorkomen dat de organisaties van het goedkeuren van geïntegreerde IAM-oplossingen?

* Meest technische oplossingen zijn gebaseerd op software-platforms die moeten worden geïmplementeerd en worden aangepast door elke organisatie voor hun eigen toepassingen.
* Cloud-toepassingen zijn vaak vastgesteld met een hogere snelheid dan IT-organisatie kan worden geïntegreerd met bestaande IAM-oplossingen.
* Beveiliging en bewaking van hulpprogramma's moeten extra aanpassingen en integratie om uitgebreide E2E-scenario's.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory is geïntegreerd met toepassingen
Azure Active Directory is een uitgebreide Identity & van Microsoft als een Service (IDaaS)-oplossing die:

* Hiermee kunt u IAM als een service in de cloud 
* Biedt centraal toegangsbeleid, eenmalige aanmelding (SSO), en rapportage 
* Biedt ondersteuning voor geïntegreerde toegangsbeheer voor [duizenden toepassingen](https://azure.microsoft.com/marketplace/active-directory/) in de galerie met toepassingen, waaronder Salesforce, Google Apps, Box, Concur en meer. 

Met Azure Active Directory, alle toepassingen die u publiceert voor uw partners en klanten (bedrijven of consumenten) hebben dezelfde identiteit en toegang tot de mogelijkheden voor beheer.<br> Hiermee kunt u uw operationele kosten aanzienlijk verlagen.

Wat gebeurt er als u nodig hebt voor het implementeren van een toepassing die nog niet wordt vermeld in de toepassingsgalerie? Dit is iets meer tijd in beslag dan het configureren van eenmalige aanmelding voor toepassingen uit de galerie, biedt Azure AD u een wizard waarmee u met de configuratie.

De waarde van Azure AD gaat dan 'net' cloud-toepassingen. U kunt deze ook gebruiken met on-premises toepassingen via een beveiligde externe toegang. Beveiligde externe toegang, kunt u elimineert de noodzaak van VPN's of andere implementaties van traditionele RAS-beheer.

Door het centrale toegangsbeheer en eenmalige aanmelding (SSO) bieden voor al uw toepassingen, biedt Azure AD de oplossing voor de belangrijkste gegevensbeveiliging en productiviteit van problemen.

* Gebruikers kunnen toegang krijgen tot meerdere toepassingen met één aanmelding waardoor meer tijd om inkomsten te genereren of zakelijke operationele activiteiten uitgevoerd.
* Identiteitsbeheerders kunnen toegang tot toepassingen op één locatie beheren.

Het voordeel voor de gebruiker en voor uw bedrijf is duidelijk. Eens nader bekijken op de voordelen voor de beheerder van de identiteit en de organisatie.

## <a name="integrated-application-benefits"></a>Voordelen van de geïntegreerde toepassing
Het proces voor eenmalige aanmelding bestaat uit twee stappen:

* Verificatie, het proces identiteit van de gebruiker te valideren.
* Autorisatie, de beslissing om inschakelen of blokkeren toegang tot een resource met een toegangsbeleid.

Als u Azure AD gebruikt voor het beheren van toepassingen en eenmalige aanmelding inschakelen:

* Verificatie is uitgevoerd op de on-premises (b.v. AD) of de Azure AD-account van de gebruiker.
* Autorisatie wordt uitgevoerd op het Azure AD-toewijzing en beveiliging beleid ervoor te zorgen dat consistente gebruikerservaring en u toewijzing, locaties en MFA voorwaarden toevoegen op elke toepassing, ongeacht de interne mogelijkheden in te schakelen.

Het is belangrijk om te begrijpen die de manier waarop die de autorisatie is ingesteld op de doeltoepassing is afhankelijk van hoe de toepassing is geïntegreerd met Azure AD.

* **Toepassingen die vooraf zijn geïntegreerd door serviceprovider** , zoals Office 365 en Azure, zijn deze toepassingen ontwikkeld rechtstreeks met Azure AD en afhankelijk van de uitgebreide mogelijkheden voor identiteits- en toegangsbeheer. Toegang tot deze toepassingen is door middel van directory-informatie en token-uitgifte ingeschakeld.
* **Toepassingen die vooraf zijn geïntegreerd door Microsoft en aangepaste toepassingen** zijn onafhankelijke cloudtoepassingen die afhankelijk zijn van een interne toepassingsmap en onafhankelijk van Azure AD kunnen werken. Toegang tot deze toepassingen is met behulp van een toepassingsspecifieke referenties toegewezen aan een account van de toepassing ingeschakeld. Afhankelijk van de mogelijkheden van toepassingen mogelijk de referentie op die een federation-token of gebruikersnaam en wachtwoord voor een account dat eerder is ingericht in de toepassing.
* **On-premises toepassingen** die zijn gepubliceerd via de Azure AD-toepassingsproxy voornamelijk toegang tot on-premises toepassingen inschakelen. Deze toepassingen zijn afhankelijk van een centrale on-premises map, zoals Windows Server Active Directory. Toegang tot deze toepassingen is ingeschakeld door het activeren van de proxy voor het leveren van inhoud van de toepassing aan de eindgebruiker bij het naleven van de on-premises aanmeldings-vereiste.

Als een gebruiker lid wordt van uw organisatie, moet u bijvoorbeeld een account voor de gebruiker maken in Azure AD voor de primaire bewerkingen van het type aanmelding. Als deze gebruiker toegang hebben tot een beheerde toepassing zoals Salesforce moet, moet u ook een account voor deze gebruiker in Salesforce maken en koppelen aan het Azure-account te maken van eenmalige aanmelding werkt. Wanneer de gebruiker uw organisatie verlaat, is het raadzaam om de Azure AD-account te verwijderen en alle bijbehorende accounts in de IAM opgeslagen van de gebruiker heeft toegang tot toepassingen.

## <a name="access-detection"></a>Detectie van toegang
In moderne ondernemingen zijn IT-afdelingen vaak niet op de hoogte van alle cloudtoepassingen die worden gebruikt. In combinatie met Cloud App Discovery biedt Azure AD u een oplossing voor het detecteren van deze toepassingen.

## <a name="account-management"></a>Accountbeheer
Beheren van accounts in de verschillende toepassingen is traditioneel een handmatig proces uitgevoerd door IT of ondersteuningspersoneel in de organisatie. Azure AD automatiseert accountbeheer volledig verschillende toepassingen die zijn geïntegreerd door serviceproviders en toepassingen vooraf geïntegreerd met Microsoft ondersteuning bieden voor geautomatiseerde gebruikersinrichting of SAML JIT-inrichting.

## <a name="automated-user-provisioning"></a>Automatisch gebruikers inrichten
Sommige toepassingen bieden interfaces van automatisering voor het maken en verwijderen (of deactivering) van accounts. Als een provider een interface biedt, wordt gebruikt door Azure AD. Dit vermindert uw operationele kosten omdat administratieve taken vinden automatisch plaats en verbetert de beveiliging van uw omgeving, omdat het verkleint de kans op onbevoegde toegang.

## <a name="access-management"></a>Toegangsbeheer
U kunt toegang tot toepassingen met behulp van afzonderlijke of regel gestuurde toewijzingen beheren met Azure AD. U kunt ook overdragen toegangsbeheer de juiste personen in de organisatie ervoor te zorgen dat het beste toezicht en de belasting van de helpdesk verminderen.

## <a name="on-premises-applications"></a>On-premises toepassingen
De ingebouwde toepassingsproxy kunt u uw on-premises toepassingen naar uw gebruikers leidt tot publiceren zowel consistente ervaring met moderne cloud-App en de voordelen van Azure AD-mogelijkheden voor bewaking, rapportage en beveiliging van toegang.

## <a name="reporting-and-monitoring"></a>Rapportage- en bewakingsdoeleinden
Azure AD biedt u vooraf geïntegreerde rapportage- en bewakingsdoeleinden mogelijkheden waarmee u te weten wie toegang tot toepassingen en heeft wanneer ze deze daadwerkelijk gebruikt.

## <a name="related-capabilities"></a>Aanverwante mogelijkheden
U kunt uw toepassingen met gedetailleerde toegangsbeleid en vooraf geïntegreerde MFA beveiligen met Azure AD. Voor meer informatie over Azure MFA Zie [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Aan de slag
Kijk eens aan de slag toepassingen integreren met Azure AD de [integratie van Azure Active Directory met toepassingen aan de slag](plan-an-application-integration.md).

## <a name="see-also"></a>Zie ook
* [Article Index for Application Management in Azure Active Directory](../active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Stapsgewijze implementatie plannen voor eenmalige aanmelding in een SaaS-toepassing](http://aka.ms/ssodeploymentplan)

