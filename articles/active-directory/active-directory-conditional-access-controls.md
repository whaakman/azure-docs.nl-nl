---
title: Besturingselementen in voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: Meer informatie over de werking van besturingselementen in voorwaardelijke toegang van Azure Active Directory.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4cb225266a45808e5fda271e901749bf03c636e2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="controls-in-azure-active-directory-conditional-access"></a>Besturingselementen in Azure Active Directory voorwaardelijke toegang 

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), u kunt beheren hoe gemachtigde gebruikers toegang tot uw cloud-apps. In een beleid voor voorwaardelijke toegang definieert u het antwoord ('dit doen') naar een bepaalde voorwaarde ('als dit gebeurt'). In de context van voorwaardelijke toegang 

- "**Wanneer dit gebeurt**' heet **voorwaarde-instructie**

- "**Voert u deze**' heet **besturingselementen**

![Besturingselement](./media/active-directory-conditional-access-controls/11.png)

De combinatie van een voorwaardeninstructie met de besturingselementen vertegenwoordigt een beleid voor voorwaardelijke toegang.

![Besturingselement](./media/active-directory-conditional-access-controls/12.png)

Elk besturingselement is een vereiste waaraan moet worden voldaan door de persoon die of systeem aanmelden of een beperking van wat de gebruiker kunt doen na het aanmelden. 

Er zijn twee typen besturingselementen: 

- **Hiermee bepaalt u GRANT** - gate toegang

- **Sessie-besturingselementen** - toegang te beperken in een sessie

Dit onderwerp worden de verschillende besturingselementen die beschikbaar in voorwaardelijke toegang van Azure AD zijn. 

## <a name="grant-controls"></a>Verleen besturingselementen

Met grant-besturingselementen, kunt u helemaal toegang blokkeren of toestaan van toegang met aanvullende vereisten als u selecteert de gewenste besturingselementen. Voor meerdere besturingselementen kunt u het volgende nodig:

- Alle geselecteerde besturingselementen moet worden voldaan (*en*) 
- Een besturingselement moet worden voldaan geselecteerd (*of*)

![Besturingselement](./media/active-directory-conditional-access-controls/17.png)



### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

U kunt dit besturingselement meervoudige authenticatie voor toegang tot de opgegeven cloud-app. Dit besturingselement ondersteunt de volgende multi-factor Authentication-providers: 

- Azure Multi-Factor Authentication 

- Een lokale multi-factor authentication-provider, gecombineerd met Active Directory Federation Services (AD FS).
 
Met multi-factor authentication voorkomt bronnen worden geopend door onbevoegde gebruikers die toegang tot de primaire referenties van een geldige gebruiker kan hebben.



### <a name="compliant-device"></a>Compatibel apparaat

U kunt beleid voor voorwaardelijke toegang op basis van het apparaat configureren. Het doel van een beleid voor voorwaardelijke toegang op basis van apparaten is om toegang te verlenen tot de geconfigureerde resources alleen vanaf vertrouwde apparaten. Vereisen van een compatibel apparaat is een optie die u hebt om te definiëren welke een vertrouwd apparaat is. Zie voor meer informatie [Azure Active Directory-beleid voor voorwaardelijke toegang op basis van apparaten instellen](active-directory-conditional-access-policy-connected-applications.md).

### <a name="domain-joined-device"></a>Domein-apparaat

U hebt vereisen van dat een domein-apparaat is een andere optie voor het configureren van beleidsregels voor voorwaardelijke toegang op basis van apparaten. Deze vereiste verwijst naar de Windows-desktops, laptops en tablets enterprise die zijn gekoppeld aan een lokale Active Directory. Zie voor meer informatie [Azure Active Directory-beleid voor voorwaardelijke toegang op basis van apparaten instellen](active-directory-conditional-access-policy-connected-applications.md).





### <a name="approved-client-app"></a>Goedgekeurde client-app

Omdat uw werknemers mobiele apparaten voor zowel persoonlijke en werktaken gebruiken, is het raadzaam de mogelijkheid om bedrijfsgegevens toegankelijk zijn via apparaten zelfs in het geval waar ze worden niet beheerd door u te beschermen.
U kunt [beveiligingsbeleid voor Intune app](https://docs.microsoft.com/intune/app-protection-policy) bescherming van gegevens van uw bedrijf onafhankelijk van een beheeroplossing voor mobiele apparaten (MDM)-oplossing.


Met goedgekeurde ClientApps, kunt u een client-app die u probeert te krijgen tot uw cloud-apps voor de ondersteuning van vereisen [beveiligingsbeleid voor Intune app](https://docs.microsoft.com/intune/app-protection-policy). U kunt bijvoorbeeld toegang tot Exchange Online beperken tot de Outlook-app. Beleid voor voorwaardelijke toegang waarvoor goedgekeurde client-apps wordt ook wel [beleid voor voorwaardelijke toegang op basis van een app](active-directory-conditional-access-mam.md). Zie voor een lijst met ondersteunde goedgekeurde client-apps, [client app vereiste goedgekeurd](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement).


### <a name="terms-of-use"></a>Gebruiksvoorwaarden

U kunt een gebruiker vereisen in uw tenant toe te staan de gebruiksvoorwaarden voordat toegang tot een resource wordt verleend. Als beheerder, kunt u configureren en aanpassen van gebruiksvoorwaarden door het uploaden van een PDF-document. Als een gebruiker valt verleend bereik van dit besturingselement toegang tot een toepassing alleen als de gebruiksvoorwaarden hebt is gesloten. 


### <a name="custom-controls"></a>Aangepaste besturingselementen 

U kunt aangepaste besturingselementen kunt maken van voorwaardelijke toegang die uw gebruikers omleiden naar een compatibele service voor verdere buiten Azure Active Directory-vereisten. Hiermee kunt u bepaalde externe multi-factor authentication-server en de verificatie-providers gebruiken om af te dwingen van regels voor voorwaardelijke toegang of voor het bouwen van uw eigen aangepaste service. Voert de vereiste verificatie of validatie van activiteiten en wordt vervolgens omgeleid naar Azure Active Directory om te voldoen aan dit besturingselement, browser van een gebruiker wordt omgeleid naar de externe service. Als de gebruiker is geverifieerd of gevalideerd, wordt de gebruiker blijft in de stroom voor voorwaardelijke toegang. 

## <a name="custom-controls"></a>Aangepaste besturingselementen

Aangepaste besturingselementen zijn een functie van de Azure Active Directory Premium P2-editie. Wanneer u aangepaste besturingselementen, worden gebruikers omgeleid naar een compatibele service voor verdere buiten Azure Active Directory-vereisten. Voert de vereiste verificatie of validatie van activiteiten en wordt vervolgens omgeleid naar Azure Active Directory om te voldoen aan dit besturingselement, browser van een gebruiker wordt omgeleid naar de externe service. Controleert de reactie van Azure Active Directory en als de gebruiker is geverifieerd of gevalideerd, wordt de gebruiker blijft in de stroom voor voorwaardelijke toegang.

Deze besturingselementen toestaan van het gebruik van bepaalde diensten externe of aangepaste als besturingselementen voor voorwaardelijke toegang en in het algemeen van de mogelijkheden van voorwaardelijke toegang.

Biedt momenteel een compatibele service providers zijn onder andere:

- Duo beveiliging

- RSA

- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

Voor meer informatie over deze services rechtstreeks contact op met de providers.

### <a name="creating-custom-controls"></a>Aangepaste besturingselementen te maken

Voor het maken van een aangepast besturingselement, moet u eerst contact op met de provider die u wilt gebruiken. Elke niet-Microsoft-provider heeft een eigen proces en de vereisten voor aanmelden, abonneren of anders een deel uitmaken van de service en om aan te geven die u wilt integreren met voorwaardelijke toegang. Op dat moment zal de provider bieden u een blok gegevens in JSON-indeling. Deze gegevens kunnen de provider en voorwaardelijke toegang om samen te werken voor uw tenant, het nieuwe besturingselement gemaakt en wordt gedefinieerd hoe voorwaardelijke toegang kan zien als uw gebruikers verificatie met de provider hebt uitgevoerd.

De JSON-gegevens kopieer en plak deze in het bijbehorende tekstvak. Breng eventuele wijzigingen niet in de JSON, tenzij u expliciet op de hoogte zijn van de wijziging die u wilt aanbrengen. Door een wijziging kan de verbinding verbreken tussen de provider en Microsoft en mogelijk vergrendelen u en uw gebruikers buiten uw accounts.

De optie voor het maken van een aangepast besturingselement bevindt zich in de **beheren** sectie van de **voorwaardelijke toegang** pagina.

![Besturingselement](./media/active-directory-conditional-access-controls/82.png)

Te klikken op **nieuwe aangepaste besturingselement**, wordt een blade geopend met een tekstvak voor de JSON-gegevens van het besturingselement.  


![Besturingselement](./media/active-directory-conditional-access-controls/81.png)


### <a name="deleting-custom-controls"></a>Aangepaste besturingselementen verwijderen

Als u wilt een aangepast besturingselement verwijderen, moet u eerst controleren of deze niet wordt gebruikt in een beleid voor voorwaardelijke toegang. Wanneer u klaar bent:

1. Ga naar de lijst met aangepaste besturingselementen

2. Klik op...  

3. Selecteer **Verwijderen**.

### <a name="editing-custom-controls"></a>Aangepaste besturingselementen bewerken

Een aangepast besturingselement bewerken, moet u het huidige besturingselement verwijderen en een nieuw besturingselement met de bijgewerkte gegevens te maken.




## <a name="session-controls"></a>Sessie-besturingselementen

Sessie-besturingselementen inschakelen beperkte ervaring in een cloud-app. De sessie-besturingselementen worden afgedwongen door de cloud-apps en zijn gebaseerd op aanvullende informatie verstrekt door Azure AD naar de app over de sessie.

![Besturingselement](./media/active-directory-conditional-access-controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Afgedwongen app-beperkingen gebruiken

Dit besturingselement kunt u Azure AD de apparaatgegevens doorgeven aan de cloud-app vereisen. Dit helpt de cloud-app als de gebruiker afkomstig is van een compatibel apparaat of een domein zijn toegevoegd apparaat weten. Dit besturingselement momenteel wordt alleen ondersteund met SharePoint als de cloud-app. De apparaatgegevens SharePoint gebruikt om gebruikers een ervaring beperkt of volledig afhankelijk van de apparaatstatus.
Zie voor meer informatie over het vereisen van beperkte toegang met SharePoint, [toegang beheren vanuit niet-beheerde apparaten](https://aka.ms/spolimitedaccessdocs).



## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
