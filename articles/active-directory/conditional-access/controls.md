---
title: Wat zijn besturingselementen voor toegang in Azure Active Directory voor voorwaardelijke toegang? | Microsoft Docs
description: Meer informatie over hoe access beheert in Azure Active Directory voor voorwaardelijke toegang tot werk.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: ceaf472f53c48b17701b14fdf4107045c2e43fdc
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521972"
---
# <a name="what-are-access-controls-in-azure-active-directory-conditional-access"></a>Wat zijn besturingselementen voor toegang in Azure Active Directory voor voorwaardelijke toegang?

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), u kunt bepalen hoe gemachtigde gebruikers toegang tot uw cloud-apps. In een beleid voor voorwaardelijke toegang definieert u het antwoord ('Doe dit') met de reden voor het activeren van uw beleid ('als dit gebeurt').

![Beheer](./media/controls/10.png)

In de context van voorwaardelijke toegang

- "**Wanneer dit gebeurt**' heet **voorwaarden**

- "**Voert u deze**' heet **besturingselementen voor toegang**

De combinatie van een voorwaardeninstructie met de besturingselementen vertegenwoordigt een beleid voor voorwaardelijke toegang.

![Beheer](./media/controls/61.png)

Elk besturingselement is een vereiste waaraan moet worden voldaan door de persoon die of systeem aanmelden of een beperking op wat de gebruiker kan doen na het aanmelden.

Er zijn twee typen besturingselementen:

- **Besturingselementen toekennen** - gate-toegang

- **Sessiebesturingselementen** - toegang te beperken in een sessie

Dit onderwerp worden de verschillende besturingselementen die beschikbaar in Azure AD voor voorwaardelijke toegang zijn. 

## <a name="grant-controls"></a>Besturingselementen toekennen

Met besturingselementen voor verlenen, kunt u toegang volledig blokkeren of toegang met aanvullende vereisten toestaan door het selecteren van de gewenste besturingselementen. Voor meerdere besturingselementen, kunt u het volgende nodig:

- Alle geselecteerde besturingselementen om te worden uitgevoerd (*en*)
- Een geselecteerd besturingselement moet worden voldaan (*of*)

![Beheer](./media/controls/17.png)

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Dit besturingselement kunt u multi-factor authentication voor toegang tot de opgegeven cloud-app vereisen. Dit besturingselement ondersteunt de volgende multi-factor Authentication-providers:

- Azure Multi-Factor Authentication

- Een on-premises multi-factor authentication-provider, gecombineerd met Active Directory Federation Services (AD FS).

Met multi-factor authentication beschermt resources toegankelijk zijn via een niet-gemachtigde gebruiker die mogelijk toegang tot de primaire referenties van een geldige gebruiker hebben opgedaan.

### <a name="compliant-device"></a>Compatibel apparaat

U kunt beleid voor voorwaardelijke toegang op basis van apparaat configureren. Het doel van beleid voor voorwaardelijke toegang op basis van het apparaat is alleen toegang verlenen tot de geselecteerde cloud-apps van [beheerde apparaten](require-managed-devices.md). Vereisen van een apparaat moet worden gemarkeerd als compatibel is een optie die u hebt toegang tot beheerde apparaten wilt beperken. Een apparaat kan worden gemarkeerd als compatibel met Intune (voor elk apparaat OS) of met uw externe MDM-systeem voor Windows 10-apparaten. MDM-systemen van derden voor apparaattypen besturingssysteem dan Windows 10 worden niet ondersteund. 

Het apparaat moet worden geregistreerd bij Azure AD voordat deze kan worden gemarkeerd als compatibel. Voor het registreren van een apparaat, hebt u drie opties: 

- [Azure AD ingeschreven apparaten](../devices/overview.md#azure-ad-registered-devices)
- [Azure AD gekoppelde apparaten](../devices/overview.md#azure-ad-joined-devices)  
- [Hybride Azure AD gekoppelde apparaten](../devices/overview.md#hybrid-azure-ad-joined-devices)

Zie voor meer informatie, [hoe u beheerde apparaten vereist voor toegang tot cloud-Apps met voorwaardelijke toegang](require-managed-devices.md).

### <a name="hybrid-azure-ad-joined-device"></a>Hybrid Azure AD gekoppelde apparaat

Vereist dat een hybride Azure AD gekoppelde apparaat is een andere optie die u moet configureren van beleid voor voorwaardelijke toegang op basis van het apparaat. Deze vereiste verwijst naar de Windows-desktops, laptops en tablets enterprise die zijn gekoppeld aan een on-premises Active Directory. Als deze optie is geselecteerd, geeft uw beleid voor voorwaardelijke toegang op toegang aanmeldpogingen met apparaten die zijn gekoppeld aan uw on-premises Active Directory en uw Azure Active Directory.  

Zie voor meer informatie, [instellen van beleid voor het apparaat gebaseerde voorwaardelijke toegang van Azure Active Directory](require-managed-devices.md).

### <a name="approved-client-app"></a>Goedgekeurde client-app

Omdat uw werknemers mobiele apparaten voor zowel privé- en werktaken gebruiken, kunt u de mogelijkheid om bedrijfsgegevens toegankelijk zijn via apparaten, zelfs in het geval wanneer ze niet worden beheerd door u te beschermen.
U kunt [Intune beveiligingsbeleid voor apps](https://docs.microsoft.com/intune/app-protection-policy) ter bescherming van gegevens van uw bedrijf onafhankelijk van een oplossing voor mobile device management (MDM).

Met goedgekeurde client-apps, u kunt vereisen dat een client-app die u probeert te krijgen tot uw cloud-apps voor de ondersteuning van [Intune beveiligingsbeleid voor apps](https://docs.microsoft.com/intune/app-protection-policy). U kunt bijvoorbeeld toegang tot Exchange Online beperken voor de Outlook-app. Beleid voor voorwaardelijke toegang waarvoor goedgekeurde client-apps wordt ook wel bekend als [beleid voor voorwaardelijke toegang op basis van app](app-based-conditional-access.md). Zie voor een lijst van ondersteunde goedgekeurde client-apps, [goedgekeurde client-app vereiste](technical-reference.md#approved-client-app-requirement).

### <a name="terms-of-use"></a>Gebruiksvoorwaarden

U kunt vereisen dat een gebruiker in uw tenant akkoord gaan met de gebruiksvoorwaarden voordat toegang tot een resource wordt verleend. U kunt als beheerder, configureren en aanpassen van gebruiksvoorwaarden door te uploaden, een PDF-document. Als een gebruiker valt wordt bereik van dit besturingselement voor toegang tot een toepassing alleen verleend als de gebruiksvoorwaarden zijn vastgesteld.

### <a name="custom-controls-preview"></a>Aangepaste besturingselementen (preview-versie)

U kunt aangepaste besturingselementen maken voor de voorwaardelijke toegang die uw gebruikers omleiden naar een compatibele service voor verdere buiten Azure Active Directory-vereisten. Hiermee kunt u bepaalde externe verificatie met meerdere factoren en verificatie providers gebruiken om af te dwingen van regels voor voorwaardelijke toegang of om uw eigen aangepaste service. Vereist verificatie of validatie-activiteiten wordt uitgevoerd en wordt vervolgens keert u terug naar Azure Active Directory om te voldoen aan dit besturingselement, de browser van een gebruiker wordt omgeleid naar de externe service. Als de gebruiker is geverifieerd of gevalideerd, wordt de gebruiker blijft in de stroom voor voorwaardelijke toegang. 

## <a name="custom-controls"></a>Aangepaste besturingselementen

Aangepaste besturingselementen zijn een functie van de Azure Active Directory Premium P1-editie. Wanneer u aangepaste besturingselementen, worden uw gebruikers worden omgeleid naar een compatibele service voor verdere buiten Azure Active Directory-vereisten. Vereist verificatie of validatie-activiteiten wordt uitgevoerd en wordt vervolgens keert u terug naar Azure Active Directory om te voldoen aan dit besturingselement, de browser van een gebruiker wordt omgeleid naar de externe service. Controleert de reactie van Azure Active Directory en, als de gebruiker is geverifieerd of gevalideerd, wordt de gebruiker blijft in de stroom voor voorwaardelijke toegang.

Deze besturingselementen kunnen het gebruik van bepaalde externe of aangepaste services als besturingselementen voor voorwaardelijke toegang en in het algemeen van de mogelijkheden van voorwaardelijke toegang.

Biedt momenteel een compatibele service providers zijn onder andere:

- [Duo beveiliging](https://duo.com/docs/azure-ca)
- [Entrust Datacard](https://www.entrustdatacard.com/products/authentication/intellitrust)
- [Ping Identity](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_AzureADIntegration.html)
- RSA
- [SecureAuth](https://docs.secureauth.com/pages/viewpage.action?pageId=47238992#)
- [Silverfort](https://www.silverfort.io/company/using-silverfort-mfa-with-azure-active-directory/)
- [Symantec VIP](https://help.symantec.com/home/VIP_Integrate_with_Azure_AD)
- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

Voor meer informatie over deze services rechtstreeks contact op met de providers.

### <a name="creating-custom-controls"></a>Het maken van aangepaste besturingselementen

Voor het maken van een aangepast besturingselement, moet u eerst contact op met de provider die u wilt gebruiken. Elke niet-Microsoft-provider heeft een eigen proces en de vereisten om te registreren, abonnement of anders wordt een onderdeel van de service, en om aan te geven dat u wilt integreren met voorwaardelijke toegang. Op dat moment geeft de provider u een blok van gegevens in JSON-indeling. Deze gegevens kunnen de provider en voorwaardelijke toegang om samen te werken voor uw tenant, wordt het nieuwe besturingselement gemaakt en wordt gedefinieerd hoe voorwaardelijke toegang kan zien als uw gebruikers verifiëren met de provider hebt uitgevoerd.

Kopieer de JSON-gegevens en plak deze in het bijbehorende tekstvak. Geen eventuele wijzigingen aanbrengen in de JSON tenzij u expliciet op de hoogte zijn van de wijziging die u aanbrengt. Elke wijziging kan verbreekt de verbinding tussen de provider en de Microsoft en u en uw gebruikers geen toegang tot uw accounts mogelijk te vergrendelen.

De optie voor het maken van een aangepast besturingselement is in de **beheren** sectie van de **voorwaardelijke toegang** pagina.

![Beheer](./media/controls/82.png)

Te klikken op **nieuw aangepast besturingselement**, wordt een blade geopend met een tekstvak voor de JSON-gegevens van het besturingselement.  

![Beheer](./media/controls/81.png)

### <a name="deleting-custom-controls"></a>Verwijderen van aangepaste besturingselementen

Als u wilt verwijderen van een aangepast besturingselement, moet u eerst ervoor zorgen dat deze niet wordt gebruikt in een beleid voor voorwaardelijke toegang. Wanneer u klaar bent:

1. Ga naar de lijst met aangepaste besturingselementen

2. Klik op...  

3. Selecteer **Verwijderen**.

### <a name="editing-custom-controls"></a>Aangepaste besturingselementen bewerken

Als u wilt een aangepast besturingselement bewerken, moet u het huidige besturingselement verwijderen en maak een nieuw besturingselement met de bijgewerkte gegevens.

## <a name="session-controls"></a>Sessiebesturingselementen

Met de sessiebesturingselementen wordt de beperkte ervaring in een cloud-app ingeschakeld. De sessiebesturingselementen worden afgedwongen door cloud-apps en zijn gebaseerd op aanvullende informatie geleverd door Azure AD aan de app over de sessie.

![Beheer](./media/controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Door apps gehandhaafde beperkingen gebruiken

U kunt dit besturingselement gebruiken om te vereisen van Azure AD apparaatgegevens doorgeven aan de geselecteerde cloud-apps. De gegevens van een apparaat kunt de cloud-apps om te weten of een verbinding is geïnitieerd vanaf een apparaat compatibel of domein. Dit besturingselement ondersteunt alleen SharePoint Online en Exchange Online als de geselecteerde cloud-apps. Als u selecteert, de cloud-app maakt gebruik van de apparaatgegevens om gebruikers, afhankelijk van de apparaatstatus, met een beperkt of volledig ervaring te bieden.

Voor meer informatie zie:

- [Beperkte toegang met SharePoint Online inschakelen](https://aka.ms/spolimitedaccessdocs)

- [Beperkte toegang met Exchange Online inschakelen](https://aka.ms/owalimitedaccess)

## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).

- Zie [Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md) als u klaar bent om beleid voor voorwaardelijke toegang in Azure Active Directory te configureren.
