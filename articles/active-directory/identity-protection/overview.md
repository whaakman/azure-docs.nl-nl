---
title: Azure Active Directory Identity Protection | Microsoft Docs
description: Meer informatie over hoe Azure AD Identity Protection kunt u de mogelijkheid van een aanvaller misbruik te maken van een apparaat of aangetaste identiteit en beveiliging van een identiteit of een apparaat dat eerder is verdacht of bekend is dat inbreuk worden gepleegd beperken.
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2019
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 1e08af577bec463263045a0f0355acffaad5e599
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244161"
---
# <a name="what-is-azure-active-directory-identity-protection"></a>Wat is Azure Active Directory Identity Protection?

Azure Active Directory Identity Protection is een functie van de Premium P2-versie van Azure AD, waarmee u de volgende zaken kunt doen:

- Detecteren van mogelijke beveiligingsproblemen die betrekking hebben op de identiteiten van uw organisatie

- Automatische antwoorden op gedetecteerde verdachte activiteit die betrekking op de identiteiten van uw organisatie hebben configureren  

- Verdachte incidenten onderzoeken en gepaste maatregelen treffen om deze problemen op te lossen   


## <a name="get-started"></a>Aan de slag

Cloud-gebaseerde identiteiten is voor meer dan tien jaar beveiligd door Microsoft. Met Azure Active Directory Identity Protection kunt in uw omgeving, u de dezelfde systemen ter bescherming van die Microsoft gebruikt voor het beveiligen van identiteiten.

De meeste van de beveiliging terugdringen plaatsvinden als aanvallers toegang krijgen tot een omgeving door het stelen van de identiteit van een gebruiker. In de afgelopen jaren, zijn aanvallers steeds meer in gebruik te maken van inbreuken op de derde partij en het gebruik van geavanceerde phishing-aanvallen van kracht geworden. Als een aanvaller toegang heeft om met lage bevoegdheden gebruikersaccounts nog, is het relatief gemakkelijk voor hen toegang te krijgen tot belangrijke bedrijfsbronnen via een laterale verplaatsing.

Als dit moet u naar:

- Beveiligen van alle identiteiten, ongeacht hun niveau van bevoegdheden

- Proactief te voorkomen dat verdachte identiteiten worden misbruikt

Verdachte identiteiten detecteren, is geen eenvoudige taak. Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en methodieken voor het detecteren van afwijkingen en verdachte incidenten die duiden op mogelijk verdachte identiteiten. Met behulp van deze gegevens genereert Identity Protection rapporten en waarschuwingen waarmee u kunt de gedetecteerde problemen evalueren en juiste risicobeperking of herstelacties.

Azure Active Directory Identity Protection is meer dan een controle- en rapportagetool. Ter bescherming van identiteiten in uw organisatie, kunt u risico's gebaseerd beleid die automatisch op gedetecteerde problemen reageren wanneer een opgegeven risiconiveau is bereikt. Deze beleidsregels, naast andere besturingselementen voor voorwaardelijke toegang is geleverd door Azure Active Directory en [Enterprise Mobility + Security](https://docs.microsoft.com/enterprise-mobility-security/) (EMS), kunnen automatisch blokkeren of adaptieve herstelacties met inbegrip van initiëren wachtwoord opnieuw instellen van wachtwoorden en meervoudige verificatie afdwingen.


#### <a name="identity-protection-capabilities"></a>Mogelijkheden voor identiteits-beveiliging

**Detecteren van beveiligingsproblemen en riskante accounts:**  

* Bieden van aangepaste aanbevelingen voor het totale beveiligingspostuur verbeteren door het markeren van beveiligingsproblemen
* Berekenen van de aanmelding risiconiveaus
* Berekenen van de gebruiker risiconiveaus


**Risicogebeurtenissen onderzoeken:**

* Verzenden van meldingen voor risicogebeurtenissen
* Het onderzoeken van risico's met behulp van relevante en contextuele informatie
* Biedt eenvoudige werkstromen voor het volgen van onderzoek
* Eenvoudige toegang tot herstelacties, zoals wachtwoord opnieuw instellen

**Beleid voor voorwaardelijke toegang op basis van risico's:**

* Beleid voor riskante aanmeldingen beperken door het blokkeren van aanmeldingen of het vereisen van multi-factor authentication-uitdagingen
* Beleid op blokkeren of accounts voor beveiligde riskante gebruikers
* Beleid om te vereisen dat gebruikers zich registreren voor meervoudige verificatie



## <a name="identity-protection-roles"></a>Identity Protection-functies

Om taken te verdelen de beheeractiviteiten rond uw Identity Protection-implementatie, kunt u verschillende rollen toewijzen. Azure AD Identity Protection ondersteunt 3 directory-rollen:

| Rol                         | Kan doen                          | Niet mogelijk is
| :--                          | ---                                |  ---   |
| Globale beheerder         | Volledige toegang tot Identity Protection, onboarding Identity Protection| |
| Beveiligingsbeheerder       | Volledige toegang tot Identity Protection | Onboarding Identity Protection, wachtwoorden opnieuw instellen voor een gebruiker |
| Beveiligingslezer              | Alleen-lezen toegang tot Identity Protection | Onboarding Identity Protection, gebruikers herstellen, configureren van beleid, wachtwoorden opnieuw instellen |




Zie voor meer informatie, [beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)



## <a name="detection"></a>Detectie

### <a name="vulnerabilities"></a>Beveiligingsproblemen

Azure Active Directory Identity Protection analyseert uw configuratie en beveiligingsproblemen die invloed op van uw gebruikers-id's hebben kunnen detecteert. Zie voor meer informatie, [beveiligingsproblemen die worden gedetecteerd door Azure Active Directory Identity Protection](vulnerabilities.md).

### <a name="risk-events"></a>Risicogebeurtenissen

Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en methodieken voor het detecteren van verdachte activiteit die betrekking op de identiteiten van uw gebruikers hebben. Maakt een record voor elke gedetecteerde verdachte actie van het systeem. Deze records worden ook wel bekend als risicogebeurtenissen.  
Zie [Risicogebeurtenissen in Azure Active Directory](../active-directory-identity-protection-risk-events.md) voor meer informatie.


## <a name="investigation"></a>Onderzoek

Uw reis via Identity Protection wordt doorgaans begint met het dashboard Identity Protection.

![Herstel](./media/overview/1000.png "herstel")

Het dashboard biedt u toegang tot:

* Rapporten, zoals **gebruikers die zijn gemarkeerd voor risico's**, **Risicogebeurtenissen** en **beveiligingsproblemen**
* Instellingen zoals de configuratie van uw **beveiligingsbeleid**, **meldingen** en **registratie voor meervoudige verificatie**

Het is doorgaans het startpunt voor onderzoek, is het proces van het controleren van de activiteiten, logboeken en andere relevante informatie met betrekking tot een risicogebeurtenis om te bepalen of de updateherstel- of risicobeperking stappen nodig zijn, en hoe de identiteit is aangetast en te begrijpen hoe de identiteit van de aangetaste is gebruikt.

U kunt uw onderzoeksactiviteiten om te verbinden, de [meldingen](notifications.md) Azure Active Directory Protection per e-mail verzendt.



## <a name="policies"></a>Beleidsregels

Voor het implementeren van automatische antwoorden, biedt Azure Active Directory Identity Protection drie beleidsregels:

- [Registratiebeleid voor meervoudige verificatie](howto-mfa-policy.md)

- [Beleid voor gebruikersrisico 's](howto-user-risk-policy.md)

- [Beleid voor aanmeldingsrisico](howto-sign-in-risk-policy.md)


## <a name="next-steps"></a>Volgende stappen

- [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

- [Azure Active Directory Identity Protection inschakelen](enable.md)

