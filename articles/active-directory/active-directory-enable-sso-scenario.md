---
title: Toepassingen beheren met Azure Active Directory | Microsoft Docs
description: Dit artikel de voordelen van Azure Active Directory integreren met uw on-premises, cloud en SaaS-toepassingen.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 95b96f10-2d5c-4b78-8af8-d3657a24140f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: e825e6be79efad5dfb385f96901a2b5682027963
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="managing-applications-with-azure-active-directory"></a>Toepassingen beheren met Azure Active Directory
Afgezien van de werkstroom of inhoud hebben bedrijven twee basisvereisten voor alle toepassingen:

1. Voor productiviteit verhogen, moeten toepassingen gemakkelijk detecteren en gebruiken
2. Voor het inschakelen van beveiliging en beheeracties, moet de organisatie controle en toezicht op wie kan en daadwerkelijk is toegang tot elke toepassing

In de wereld van cloud-toepassingen die deze best kan worden bereikt met behulp van de identiteit van besturingselement '*WIE mag doen wat*'.

In het computing terminologie:

* *Wie* staat bekend als *identiteit* -het beheer van gebruikers en groepen
* *Wat* staat bekend als *toegangsbeheer* – het beheer van toegang tot beveiligde bronnen

Beide onderdelen samen worden aangeduid als *identiteit en toegang Management (IAM)*, die is gedefinieerd door de [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) als een groep '*de beveiliging discipline waarmee de juiste personen toegang krijgen tot de juiste resources aan de rechterkant tijden voor de redenen waarom*'.

OK, wat het probleem is? Als de IAM *niet beheerd* op één locatie met een geïntegreerde oplossing:

* Identiteit beheerders hebben afzonderlijk maken en bijwerken van gebruikersaccounts in alle toepassingen afzonderlijk, een activiteit redundante en tijd in beslag neemt.
* Gebruikers hebben tot meerdere referenties voor toegang tot de toepassingen die ze nodig hebben om te werken met onthouden. Als gevolg hiervan, gaan gebruikers er vaak schrijf hun wachtwoorden of andere beheeroplossingen voor wachtwoord waardoor gegevens beveiligingsrisico's gebruiken.
* Redundante tijdrovend activiteiten Verklein de hoeveelheid tijd gebruikers en beheerders zakelijke activiteiten die verhogen van de onderrand van uw bedrijf werkt.

Ja, wat in het algemeen wordt voorkomen dat organisaties uit gebruik nemen van geïntegreerde IAM-oplossingen?

* Meest technische oplossingen zijn gebaseerd op software-platforms die moeten worden geïmplementeerd en worden aangepast door elke organisatie voor hun eigen toepassingen.
* Cloud-toepassingen zijn vaak met een hogere snelheid dan IT-organisatie kan worden geïntegreerd met bestaande IAM-oplossingen vastgesteld.
* Beveiliging en bewaking tooling vereist aanvullende aanpassingen en integratie te bereiken uitgebreide E2E-scenario's.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory is geïntegreerd met toepassingen
Azure Active Directory is een uitgebreide Identity als een Service (IDaaS) van Microsoft die:

* Hiermee kunt u IAM als een cloudservice 
* Voorziet in centraal toegangsbeleid beheer, eenmalige aanmelding (SSO) en rapportage 
* Ondersteunt de geïntegreerde access management voor [duizenden toepassingen](https://azure.microsoft.com/marketplace/active-directory/) in de galerie met toepassingen, inclusief Salesforce, Google Apps, Box, Concur en meer. 

Met Azure Active Directory, alle toepassingen die u publiceert voor uw partners en klanten (werk of consumer) hebben dezelfde identiteit en toegang tot beheermogelijkheden.<br> Hiermee kunt u de operationele kosten aanzienlijk te verlagen.

Wat gebeurt er als u wilt een toepassing die nog niet wordt vermeld in de galerie met toepassingen implementeren? Dit is wat tijd in beslag dan eenmalige aanmelding configureren voor toepassingen uit de galerie met toepassingen, biedt Azure AD u een wizard waarmee u met de configuratie.

De waarde van Azure AD zich verder uitstrekt dan 'net' cloud-toepassingen. U kunt deze ook gebruiken met on-premises toepassingen via een beveiligde externe toegang. Met veilige externe toegang, kunt u elimineren het de noodzaak voor VPN-verbindingen of andere implementaties van traditionele RAS-beheer.

Dankzij de centraal toegangsbeleid beheer en eenmalige aanmelding (SSO) voor alle toepassingen, levert Azure AD de oplossing voor de belangrijkste gegevens beveiliging en productiviteit problemen.

* Gebruikers hebben toegang tot meerdere toepassingen met één aanmelding met een meer tijd voor inkomsten genereren of zakelijke operationele activiteiten uitgevoerd.
* Identity-beheerders kunnen toegang tot toepassingen op één plek beheren.

Het voordeel voor de gebruiker en voor uw bedrijf is duidelijk. We nemen nader bekeken de voordelen voor de beheerder van de identiteit en de organisatie.

## <a name="integrated-application-benefits"></a>Voordelen van de geïntegreerde toepassing
De SSO-proces bestaat uit twee stappen:

* Verificatie, het proces van validatie van de identiteit van de gebruiker.
* Autorisatie, de beslissing voor inschakelen of blokkeren van toegang tot een resource met een toegangsbeleid.

Als u Azure AD-toepassingen beheren en eenmalige aanmelding inschakelen:

* Verificatie is uitgevoerd op de on-premises-(bijvoorbeeld AD) of Azure AD-account van de gebruiker.
* Autorisatie wordt uitgevoerd op het Azure AD-toewijzing en beveiliging beleid consistent eindgebruiker gezorgd en het inschakelen van u toewijzing, locaties en MFA voorwaarden toevoegen op elke toepassing, ongeacht de interne mogelijkheden.

Het belangrijk om te begrijpen dat de manier waarop de autorisatie is ingesteld op de doeltoepassing varieert, afhankelijk van hoe de toepassing is geïntegreerd met Azure AD.

* **Toepassingen die vooraf zijn geïntegreerd door serviceprovider** zoals Office 365 en Azure, dit zijn toepassingen rechtstreeks op Azure AD is gebouwd en afhankelijk van de uitgebreide mogelijkheden voor identiteits- en toegangsbeheer. Toegang tot deze toepassingen is ingeschakeld door middel van directorygegevens en uitgifte van tokens.
* **Toepassingen die vooraf zijn geïntegreerd door Microsoft en aangepaste toepassingen** dit onafhankelijke cloudtoepassingen die afhankelijk zijn van een interne applicatie directory en kunnen werken onafhankelijk van Azure AD zijn. Toegang tot deze toepassingen is door uitgifte van een specifieke referentie toegewezen aan een account voor groep van toepassing ingeschakeld. Afhankelijk van de toepassingsmogelijkheden, de referentie mogelijk een federation-token of gebruikersnaam en wachtwoord voor een account dat u eerder in de toepassing is ingericht.
* **On-premises toepassingen** toepassingen zijn gepubliceerd via de Azure AD-toepassingsproxy voornamelijk toegang tot on-premises toepassingen inschakelen. Deze toepassingen afhankelijk zijn van een centrale on-premises adreslijst zoals Windows Server Active Directory. Toegang tot deze toepassingen wordt ingeschakeld door de activering van de proxy voor het leveren van inhoud van de toepassing aan de eindgebruiker bij het naleven van de lokale aanmelding vereiste.

Als een gebruiker lid wordt van uw organisatie, moet u bijvoorbeeld een account voor de gebruiker maken in Azure AD voor de primaire bewerkingen voor eenmalige aanmelding. Als deze gebruiker toegang tot een beheerde toepassing zoals Salesforce vereist, moet u ook een account voor deze gebruiker maken in Salesforce en koppel deze aan het Azure-account te maken van eenmalige aanmelding werkt. Wanneer de gebruiker uw organisatie verlaat, verdient het aanbeveling om de Azure AD-account te verwijderen en alle bijbehorende equivalent accounts in de IAM slaat de gebruiker heeft toegang tot toepassingen.

## <a name="access-detection"></a>Detectie toegang
In moderne ondernemingen zijn IT-afdelingen vaak niet op de hoogte van alle cloud-toepassingen die worden gebruikt. In combinatie met Cloud App Discovery biedt Azure AD u een oplossing voor het detecteren van deze toepassingen.

## <a name="account-management"></a>Accountbeheer
Het beheren van accounts in verschillende toepassingen is traditioneel een handmatig proces uitgevoerd door IT of ondersteuning voor persoonlijke in de organisatie. Accountbeheer Azure AD volledig geautomatiseerd tussen alle serviceprovider geïntegreerde toepassingen en die vooraf zijn geïntegreerd met Microsoft ondersteuning van geautomatiseerde gebruikersinrichting of SAML JIT toepassingen.

## <a name="automated-user-provisioning"></a>Automatisch gebruikers inrichten
Sommige toepassingen bieden automatiseringsinterfaces voor het maken en verwijderen (of deactivering) van accounts. Als een provider een interface biedt, wordt gebruikt door Azure AD. Dit vermindert de operationele kosten omdat administratieve taken automatisch gebeurt en verbetert de beveiliging van uw omgeving, omdat dit de kans op onbevoegde toegang verkleint.

## <a name="access-management"></a>Toegangsbeheer
U kunt toegang tot toepassingen die gebruikmaken van afzonderlijke of regel toewijzingen aangestuurd met behulp van Azure AD beheren. U kunt ook overdragen toegang tot beheer van de juiste mensen in de organisatie zorgen voor de beste toezicht en de belasting van de helpdesk verminderen.

## <a name="on-premises-applications"></a>On-premises toepassingen
De ingebouwde application proxy kunt u voor het publiceren van uw on-premises toepassingen aan uw gebruikers waardoor zowel consistente ervaring met moderne cloudtoepassing en de voordelen van Azure AD-mogelijkheden voor bewaking, rapportage en beveiliging van toegang.

## <a name="reporting-and-monitoring"></a>Rapportage- en controle
Azure AD biedt u vooraf geïntegreerde rapportage en monitoring mogelijkheden waarmee u te weten wie toegang tot toepassingen en heeft wanneer deze ze daadwerkelijk gebruikt.

## <a name="related-capabilities"></a>Verwante mogelijkheden
U kunt uw toepassingen met gedetailleerde toegangsbeleid en vooraf geïntegreerde MFA beveiligen met Azure AD. Voor meer informatie over Azure MFA-Zie [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Aan de slag
Om te beginnen toepassingen integreren met Azure AD te kijken hoe de [integratie van Azure Active Directory met toepassingen aan de slag](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Zie ook
[Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)

