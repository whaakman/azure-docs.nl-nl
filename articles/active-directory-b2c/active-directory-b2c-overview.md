---
title: Wat is Azure Active Directory B2C? | Microsoft Docs
description: Meer informatie over hoe u kunt maken en beheren van uw toepassing aanmeldingservaring met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 878d4da715308abf6dbe536ea0d1df2a68225179
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711311"
---
# <a name="what-is-azure-active-directory-b2c"></a>Wat is Azure Active Directory B2C?

Azure Active Directory (Azure AD) B2C is een identiteitsbeheerservice waarmee u kunt aanpassen en controleren hoe uw klanten zich registreren en aanmelden en hoe zijn hun profielen beheren wanneer ze uw toepassingen gebruiken. Dit omvat ook toepassingen die onder meer zijn ontwikkeld voor iOS, Android en .NET. Azure AD B2C kunt deze acties terwijl de identiteiten van uw klant beveiligd op hetzelfde moment.

U kunt een toepassing die is geregistreerd bij Azure AD B2C tal van identiteit beheeracties uitvoeren. Een aantal voorbeelden:

- Een klant aanmelden voor de geregistreerde toepassing inschakelen
- Een klant geregistreerd aanmelden en aan de slag met uw toepassing inschakelen
- Inschakelen van een klant geregistreerd hun profiel te bewerken
- Multi-factor authentication in uw toepassing inschakelen
- Kan de klant registreren en aanmelden met specifieke id-providers
- Toegang verlenen vanuit uw App naar API's die u bouwen
- Aanpassen van het uiterlijk van de ervaring zich kunnen registreren en aanmelden
- Beheren van sessies voor eenmalige aanmelding voor uw toepassing

## <a name="what-do-i-need-to-think-about-before-using-azure-ad-b2c"></a>Wat moet ik denken dat voordat u Azure AD B2C?

- Hoe wil ik de klant om te communiceren met mijn toepassing?
- Wat is de gebruikerservaring van de gebruikersinterface (UI) die ik wil bieden aan klanten?
- Welke id-providers wilt ik laat klanten kiezen uit in mijn toepassing?
- Vereist mijn proces aanmelden aanvullende API's om uit te voeren?

### <a name="customer-interaction"></a>Interactie met de klant

Azure AD B2C ondersteunt [OpenID Connect](https://openid.net/connect/) voor alle klant optreedt. In de Azure AD B2C-implementatie van OpenID Connect initieert uw toepassing deze gebruiker reis door verificatieaanvragen naar Azure AD B2C. Het resultaat van de aanvraag is een `id_token`. Dit beveiligingstoken vertegenwoordigt de identiteit van de klant.

Elke toepassing die gebruikmaakt van Azure AD B2C moet zijn geregistreerd in een Azure AD B2C-tenant met behulp van de Azure-portal. Het registratieproces verzamelt en waarden worden toegewezen aan uw toepassing. Deze waarden bevatten een toepassings-ID die een unieke identificatie van de toepassing en een omleidings-URI die kan worden gebruikt om te antwoorden terug naar verwijzen.

De interactie van elke toepassing volgt eenzelfde globaal patroon:

1. De toepassing zorgt ervoor dat de klant om uit te voeren van een beleid.
2. De klant voltooit het beleid volgens de beleidsdefinitie.
3. De toepassing ontvangt een beveiligingstoken.
4. De toepassing gebruikt het beveiligingstoken om toegang tot een beveiligde bron.
5. De resource-server valideert het beveiligingstoken om te controleren of toegang kan worden verleend.
6. Het beveiligingstoken wordt regelmatig vernieuwd door de toepassing.

Deze stappen kunnen verschillen enigszins verschillen op basis van het type van de toepassing die u wilt maken.

Azure AD B2C communiceert met identiteitsproviders, klanten, andere systemen en met de lokale map in de juiste volgorde om een identity-taak te voltooien. Bijvoorbeeld, meld u aan een klant, een nieuwe klant registreren of een wachtwoord opnieuw instellen. Het onderliggende platform die meerdere partijen vertrouwensrelatie te worden ingesteld en deze stappen is voltooid, wordt de identiteit ervaring Framework genoemd. Dit framework en een beleid (ook wel een reis gebruiker of een beleid vertrouwen Framework) definieert u expliciet de actoren, de acties, de protocollen en de volgorde van stappen te voltooien.

Azure AD B2C worden beschermd tegen denial-of-service en het wachtwoord aanvallen op uw toepassingen op verschillende manieren. Azure AD B2C maakt gebruik van detectie en risicobeperking technieken zoals SYN cookies en snelheid en verbinding beperkingen voor het beveiligen van bronnen tegen denial of service-aanvallen. Risicobeperking is ook opgenomen voor aanvallen met brute kracht wachtwoord en het wachtwoord woordenboekaanvallen.

#### <a name="built-in-policies"></a>Ingebouwd beleid

Elke aanvraag die naar Azure AD B2C wordt verzonden Hiermee geeft u een beleid. Een beleid regelt het gedrag van de interactie van uw toepassing met Azure AD B2C. Ingebouwde beleid zijn vooraf gedefinieerd voor de meest algemene taken voor identiteit, zoals registreren, aanmelden, en profiel bewerken.  Bijvoorbeeld, kunt een registratiebeleid u bepalen van gedrag door de volgende instellingen configureren:

- Sociale accounts die de klant gebruiken kunt om aan te melden voor de toepassing
- Gegevens verzameld van de klant, zoals de voornaam of postcode
- Multi-Factor Authentication
- Uiterlijk van alle aanmeldingspagina 's
- Informatie die wordt geretourneerd naar de toepassing

#### <a name="custom-policies"></a>Aangepast beleid 

[Aangepast beleid](active-directory-b2c-overview-custom.md) configuratiebestanden die het gedrag van het kader van de gebruikerservaring identiteit in uw Azure AD B2C-tenant definiëren zijn. Aangepaste beleidsregels kunnen volledig worden bewerkt om veel taken uitvoeren. Een aangepast beleid wordt weergegeven als een of meer XML-bestanden die in een hiërarchische keten naar elkaar verwijzen. 

Meerdere aangepaste beleidsregels met verschillende typen kunnen worden gebruikt in uw Azure AD B2C-tenant, indien nodig en opnieuw kunnen worden gebruikt in toepassingen. Deze flexibiliteit kunt u definiëren en gebruikerservaringen-identiteit met minimale of geen wijzigingen aangebracht in uw code te wijzigen. Beleid kan worden gebruikt door een speciale queryparameter toe te voegen op HTTP-aanvragen voor verificatie.

Aangepast beleid kunnen worden gebruikt om gebruiker trajecten op de volgende manieren beheren:

- Voor het definiëren van interactie met API's voor het vastleggen van aanvullende informatie controleren door de klant geleverde claims of externe processen activeren.
- Gedrag wijzigen op basis van claims van API's of van claims in de map zoals *migrationStatus*.
- De workflow niet wordt gedekt door ingebouwde beleid. Bijvoorbeeld controleren informatie verzamelen van een klant tijdens een aanmeldingservaring en het uitvoeren van een vergunning voor toegang tot een resource.

### <a name="identity-providers"></a>Id-providers

Een id-provider is een service die wordt geverifieerd klant identiteiten en verstrekt beveiligingstokens. In Azure AD B2C, kunt u een aantal id-providers configureren in uw tenant, zoals een Microsoft-account Facebook of Amazon onder andere. 

Voor het configureren van een id-provider in uw Azure AD B2C-tenant, moet u de toepassings-id of id van de client en het wachtwoord of client geheim van de identiteit provider-toepassing die u maakt registreren. Deze id en wachtwoord worden vervolgens gebruikt om uw toepassing te configureren.

### <a name="user-interface-experience"></a>-Gebruikersinterface

De meeste van de HTML- en CSS-inhoud wordt gesteld aan klanten kan worden beheerd. Met behulp van de functie pagina UI aanpassen, moet u het uiterlijk van het beleid aanpassen. U kunt ook behouden merk en visuele consistentie tussen uw toepassing en de Azure AD B2C.

Azure AD B2C code wordt uitgevoerd in de browser van de klant en gebruikt een benadering van moderne Cross-Origin-Resource delen (CORS) genoemd. Eerst, geeft u een URL in een beleid met aangepaste HTML-inhoud. Azure AD B2C samenvoegingen UI-elementen met de HTML-inhoud die is geladen vanaf uw URL en wordt de pagina weergegeven in de klant.

U kunt parameters verzenden naar Azure AD B2C in een querytekenreeks. Door de parameter wordt doorgegeven aan uw HTML-eindpunt, kunt u de pagina-inhoud dynamisch wijzigen. U kunt bijvoorbeeld de achtergrondafbeelding op de Azure AD B2C registreren of aanmelden pagina op basis van een parameter die u van uw web- of mobiele toepassing doorgeeft wijzigen.

## <a name="how-do-i-get-started-with-azure-ad-b2c"></a>Hoe ga ik aan de slag met Azure AD B2C?

In Azure AD B2C, een tenant vertegenwoordigt de organisatie en een map van gebruikers. Elke Azure AD B2C-tenant is uniek en werkt afzonderlijk van andere tenants Azure AD B2C. Een tenant bevat informatie over de klanten die zich hebben geregistreerd om uw toepassing te gebruiken. Bijvoorbeeld, wachtwoorden, profielgegevens en machtigingen.

U moet uw Azure AD B2C-tenant koppelen aan uw Azure-abonnement inschakelen van alle functionaliteit en betaalt voor gebruikskosten. Als u Azure AD B2C-klanten aan te melden bij uw toepassing, moet u uw toepassing registreren in een Azure AD B2C-tenant.

Voordat u uw toepassing configureren voor Azure AD B2C gebruiken, moet u eerst een Azure AD B2C-tenant maken en registreren van uw toepassing. Voer de stappen in voor het registreren van uw toepassing [zelfstudie: een toepassing registreren en aanmelden met Azure AD B2C inschakelen registreren](tutorial-register-applications.md).
  
Als u een ontwikkelaar van ASP.NET webtoepassingen, instellen van uw toepassing om te verifiëren met behulp van de stappen in accounts [zelfstudie: een webtoepassing om te verifiëren met behulp van Azure AD B2C-accounts in staat stellen](active-directory-b2c-tutorials-web-app.md).

Als u een ontwikkelaar bureaubladtoepassing instellen van uw toepassing om te verifiëren met behulp van de stappen in accounts [zelfstudie: een bureaubladtoepassing om te verifiëren met behulp van Azure AD B2C-accounts inschakelen](active-directory-b2c-tutorials-desktop-app.md).

Als u een single-page application ' ontwikkelaar met behulp van Node.js, instellen van uw toepassing om te verifiëren met behulp van de stappen in accounts [zelfstudie: een single-page application ' om te verifiëren met behulp van Azure AD B2C-accounts inschakelen](active-directory-b2c-tutorials-spa.md).

## <a name="next-steps"></a>Volgende stappen

Start uw toepassing configureren voor de ervaring zich kunnen registreren en aanmelden voort te zetten naar de zelfstudie.

> [!div class="nextstepaction"]
> [Zelfstudie: U registreert een toepassing registreren en aanmelden met Azure AD B2C inschakelen](tutorial-register-applications.md)
