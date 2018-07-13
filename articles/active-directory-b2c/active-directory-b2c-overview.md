---
title: Wat is Azure Active Directory B2C? | Microsoft Docs
description: Lees hoe u een aanmeldingservaring voor uw toepassing kunt maken en beheren met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 04/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6949ab89cf806818783c86199e6df334e263b046
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440878"
---
# <a name="what-is-azure-active-directory-b2c"></a>Wat is Azure Active Directory B2C?

Azure Active Directory (Azure AD) B2C is een identiteitsbeheerservice waarmee u kunt aanpassen en controleren hoe uw klanten zich registreren en aanmelden en hoe zijn hun profielen beheren wanneer ze uw toepassingen gebruiken. Dit omvat ook toepassingen die onder meer zijn ontwikkeld voor iOS, Android en .NET. Met Azure AD B2C kunt u deze acties mogelijk maken en tegelijkertijd de identiteit van uw klanten beschermen.

U kunt een toepassing die is geregistreerd bij Azure AD B2C configureren voor het uitvoeren van een aantal acties voor identiteitsbeheer. Een aantal voorbeelden:

- Een klant in staat stellen om zich te registreren voor gebruik van uw geregistreerde toepassing
- Een geregistreerde klant in staat stellen om zich aan te melden en uw toepassing te gebruiken
- Een geregistreerde klant in staat stellen om hun profiel te bewerken
- Meervoudige verificatie inschakelen in uw toepassing
- Een klant in staat stellen om zich te registreren en aan te melden met specifieke id-providers
- Vanuit uw toepassing toegang verlenen tot API's die u bouwt
- Het uiterlijk en de werking van de registratie- en aanmeldingservaring aanpassen
- Sessies met eenmalige aanmelding voor uw toepassing beheren

## <a name="what-do-i-need-to-think-about-before-using-azure-ad-b2c"></a>Waar moet ik aan nadenken voordat ik Azure AD B2C ga gebruiken?

- Hoe wil ik dat klant interactie heeft met mijn toepassing?
- Hoe moet de gebruikersinterface voor het aanmelden eruit zien?
- Welke id-providers kunnen klanten kiezen in mijn toepassing?
- Vereist de aanmeldingsprocedure extra API's?

### <a name="customer-interaction"></a>Klantinteractie

Azure AD B2C biedt ondersteuning voor [OpenID Connect](https://openid.net/connect/) voor alle klantervaringen. In de implementatie van OpenID Connect in Azure AD B2C initieert uw web-app deze gebruikerservaring door verificatieaanvragen te versturen naar Azure AD B2C. Het resultaat van de aanvraag is een `id_token`. Dit beveiligingstoken vertegenwoordigt de identiteit van de gebruiker.

Elke toepassing die gebruikmaakt van Azure AD B2C moet via Azure Portal zijn geregistreerd in een Azure AD B2C-tenant. Tijdens het registratieproces worden er waarden verzameld en toegewezen aan uw toepassing. Het betreft waarden zoals een toepassings-id die een unieke identificatie vormt van de toepassing en een omleidings-URI die kan worden gebruikt om antwoorden terug te leiden naar de toepassing.

De interactie van elke toepassing volgt in grote lijnen hetzelfde patroon:

1. De toepassing zorgt ervoor dat de klant een beleid uitvoert.
2. De gebruiker voltooit het beleid volgens de beleidsdefinitie.
3. De toepassing ontvangt een beveiligingstoken.
4. De toepassing gebruikt het beveiligingstoken om toegang te krijgen tot een beveiligde resource.
5. De resource-server valideert het beveiligingstoken om te controleren of toegang kan worden verleend.
6. Het beveiligingstoken wordt regelmatig vernieuwd door de toepassing.

Deze stappen kunnen iets verschillen, afhankelijk van het type toepassing dat u maakt.

Azure AD B2C communiceert in een bepaalde volgorde met id-providers, klanten, andere systemen en de lokale adreslijst om een identiteitstaak te voltooien, bijvoorbeeld om een klant aan te melden, een nieuwe klant te registreren of een wachtwoord opnieuw in te stellen. Identity Experience Framework is het onderliggende platform dat een vertrouwensrelatie opzet tussen meerdere partijen en deze stappen uitvoert. Dit framework bepaalt samen met een beleid (ook wel een gebruikersbeleving of een Trust Framework-beleid genoemd) expliciet de actoren, de acties, de protocollen en de volgorde van stappen die moeten worden voltooid.

Azure AD B2C beschermt uw toepassingen op verschillende manieren tegen denial-of-service- en wachtwoordaanvallen. Azure AD B2C maakt gebruik van detectie- en risicobeperkingstechnieken zoals SYN-cookies en limieten voor snelheid en verbinding om resources te beschermen tegen denial-of-service-aanvallen. Risicobeperking wordt ook toegepast voor brute-force-wachtwoordaanvallen en dictionary-wachtwoordaanvallen.

#### <a name="built-in-policies"></a>Ingebouwd beleid

Elke aanvraag die naar Azure AD B2C wordt verzonden, bevat een beleid. Een beleid bepaalt hoe uw toepassing communiceert met Azure AD B2C. Er zijn vooraf gedefinieerde ingebouwde beleidsregels voor de meest algemene identiteitstaken, zoals registratie, aanmelding en het bewerken van profielen.  Zo kunt u bijvoorbeeld een registratiebeleid definiëren door het configureren van de volgende instellingen:

- Social media-accounts die de klant kan gebruiken om zich te registreren voor de toepassing
- Gegevens van de klant die worden gevraagd, zoals voornaam of postcode
- Multi-Factor Authentication
- Werking en uiterlijk van alle aanmeldingspagina's
- Gegevens die worden geretourneerd naar de toepassing

#### <a name="custom-policies"></a>Aangepast beleid 

[Aangepaste beleidsregels](active-directory-b2c-overview-custom.md) zijn configuratiebestanden die het gedrag van het Identity Experience Framework in uw Azure AD B2C-tenant definiëren. Aangepaste beleidsregels kunnen volledig worden aangepast voor het voltooien van allerlei taken. Een aangepast beleid bestaat uit een of meer XML-bestanden die in een hiërarchische keten naar elkaar verwijzen. 

U kunt meerdere aangepaste beleidsregels van verschillende typen gebruiken in uw Azure AD B2C-tenant en deze kunnen ook worden hergebruikt in toepassingen. Deze flexibiliteit zorgt ervoor dat u klantidentiteitservaringen kunt definiëren en aanpassen met geen of minimale wijzigingen van uw code. Beleidsregels kunnen worden gebruikt door een speciale queryparameter toe te voegen aan HTTP-verificatieaanvragen.

Aangepaste beleidsregels kunnen op deze manieren worden gebruikt om gebruikerservaringen te beheren:

- Interactie met API's definiëren om aanvullende gegevens vast te leggen, door de klant geleverde claims te controleren of externe processen te activeren.
- Gedrag wijzigen op basis van claims van API's of van claims in de adreslijst, zoals *migrationStatus*.
- Een werkstroom die niet wordt gedekt door de ingebouwde beleidsregels, zoals het opvragen van extra gegevens bij een klant tijdens een aanmeldingservaring en het uitvoeren van een verificatiecontrole voor toegang tot een resource.

### <a name="identity-providers"></a>Id-providers

Een id-provider is een service die klantidentiteiten verifieert en beveiligingstokens verstrekt. In Azure AD B2C kunt u een aantal id-providers configureren in uw tenant, zoals een Microsoft-account, Facebook of Amazon. 

Als u een id-provider wilt configureren in uw Azure AD B2C-tenant, moet u de toepassings-id of client-id vastleggen, evenals het wachtwoord of het clientgeheim van de id-providertoepassing die u maakt. Deze id en wachtwoord worden vervolgens gebruikt om uw toepassing te configureren.

### <a name="user-interface-experience"></a>Gebruikersinterface

De meeste HTML- en CSS-inhoud die wordt weergegeven aan klanten kan worden ingesteld. Met behulp van de functie voor het aanpassen van de gebruikersinterface kunt u de werking en het uiterlijk van het beleid aanpassen. U kunt er ook voor zorgen dat er visuele en merkconsistentie is tussen uw toepassing en Azure AD B2C.

Azure AD B2C voert code uit in de browser van de klant en maakt gebruik van een moderne aanpak die Cross-Origin Resource Sharing (CORS) genoemd. U geeft eerst een URL op in een beleid met aangepaste HTML-inhoud. Azure AD B2C combineert elementen van de gebruikersinterface met de HTML-inhoud die vanaf de URL wordt geladen en geeft vervolgens de pagina weer aan de klant.

U kunt parameters verzenden naar Azure AD B2C in een queryreeks. Door de parameter door te geven aan uw HTML-eindpunt, kunt u de pagina-inhoud dynamisch wijzigen. U kunt bijvoorbeeld de achtergrondafbeelding op de registratie- of aanmeldingspagina van Azure AD B2C wijzigen, op basis van een parameter die u doorgeeft vanuit uw web- of mobiele toepassing.

## <a name="how-do-i-get-started-with-azure-ad-b2c"></a>Aan de slag met Azure AD B2C

In Azure AD B2C bestaat een tenant uit een adreslijst of directory met gebruikers en vertegenwoordigt deze zo uw organisatie. Elke Azure AD B2C-tenant is uniek en staat volledig los van andere Azure AD B2C-tenants. Een tenant bevat gegevens van de klanten die zich hebben geregistreerd om uw toepassing te gebruiken. Denk hierbij aan gegevens zoals wachtwoorden, profielgegevens en machtigingen.

U moet uw Azure AD B2C-tenant koppelen aan uw Azure-abonnement om te beschikken over alle functionaliteit en te betalen voor gebruikskosten. Als u Azure AD B2C-klanten in staat wilt stellen zich aan te melden bij uw toepassing, moet u uw toepassing registreren in een Azure AD B2C-tenant.

Voordat u uw toepassing configureert voor het gebruik van Azure AD B2C, moet u eerst een Azure AD B2C-tenant maken en uw toepassing registreren. Om uw toepassing te registreren, voert u de zelfstudie [Een Azure Active Directory B2C-tenant maken](tutorial-register-applications.md) uit.
  
Als u ASP.NET webtoepassingen ontwikkelt, configureert u uw toepassing om accounts te verifiëren met behulp van de stappen in de zelfstudie [Een webtoepassing inschakelen om te verifiëren met accounts met behulp van Azure Active Directory B2C](active-directory-b2c-tutorials-web-app.md).

Als u desktoptoepassingen ontwikkelt, configureert u uw toepassing om accounts te verifiëren met behulp van de stappen in de zelfstudie [Verificatie van desktop-apps inschakelen met accounts met behulp van Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app.md).

Als u toepassingen met één pagina ontwikkelt, configureert u uw toepassing om accounts te verifiëren met behulp van de stappen in de zelfstudie [Verificatie van apps met één pagina inschakelen met behulp van Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md).

## <a name="next-steps"></a>Volgende stappen

Ga verder met de volgende zelfstudie om uw toepassing te configureren voor de registratie- en aanmeldingservaring.

> [!div class="nextstepaction"]
> [Zelfstudie: Een Azure Active Directory B2C-tenant maken](tutorial-register-applications.md)
