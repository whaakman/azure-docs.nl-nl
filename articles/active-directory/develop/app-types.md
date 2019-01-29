---
title: Toepassingstypen in v1.0 | Azure
description: Hierin wordt beschreven welke typen apps en scenario's die worden ondersteund door de Azure Active Directory v2.0-eindpunt.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.openlocfilehash: b4b49cd679ee5d770696b4fa5cc29ebdd908cecf
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55093118"
---
# <a name="application-types-in-v10"></a>Toepassingstypen in v1.0

Azure Active Directory (Azure AD) ondersteunt verificatie voor diverse moderne app-architecturen, allemaal op basis van azurevoor standaardprotocollen OAuth 2.0 of OpenID Connect.

Het volgende diagram illustreert de scenario's en de toepassingstypen, en hoe de verschillende onderdelen kunnen worden toegevoegd:

![App-soorten en scenario’s](./media/authentication-scenarios/application_types_and_scenarios.png)

Dit zijn de vijf primaire toepassing-scenario's ondersteund door Azure AD:

- **[Toepassing met één pagina (SPA)](single-page-application.md)**: Er moet een gebruiker zich aanmeldt bij een toepassing met één pagina die wordt beveiligd door Azure AD.
- **[Web-App de webbrowser](web-app.md)**: Er moet een gebruiker zich aanmeldt bij een webtoepassing die wordt beveiligd door Azure AD.
- **[Systeemeigen toepassing voor de web-API](native-app.md)**: Een systeemeigen toepassing die wordt uitgevoerd op een telefoon, tablet of PC moet een gebruiker voor resources van een web-API die wordt beveiligd door Azure AD verifiëren.
- **[Webtoepassing aan web-API](web-api.md)**: Een web-App nodig heeft om op te halen van resources van een web-API is beveiligd door Azure AD.
- **[Web-API-daemon of server toepassing](service-to-service.md)**: Een daemon-toepassing of een servertoepassing zonder gebruikersinterface web moet resources ophalen uit een web-API die is beveiligd door Azure AD.

Volg de koppelingen voor meer informatie over elk type app en inzicht in geavanceerde scenario's voordat u aan de slag gaat met de code. U kunt ook meer informatie over de verschillen die u nodig hebt bij het schrijven van een bepaalde app die geschikt is voor de v1.0 eindpunt of v2.0-eindpunt.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure AD-scenario's en onderdelen. Om te bepalen of het v2.0-eindpunt moet worden gebruikt, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

U kunt ontwikkelen met een van de apps en scenario's beschreven volgen met behulp van verschillende talen en platforms. Ze worden ondersteund door de volledige codevoorbeelden die beschikbaar zijn in de code voorbeelden guide: [v1.0 codevoorbeelden per scenario](sample-v1-code.md) en [v2.0-codevoorbeelden per scenario](sample-v2-code.md). U kunt ook de codevoorbeelden die rechtstreeks vanuit de bijbehorende downloaden [GitHub-opslagplaatsen voor voorbeeld](https://github.com/Azure-Samples?q=active-directory).

Bovendien, als uw toepassing nodig heeft voor een bepaald of een segment van een end-to-end-scenario, kan in de meeste gevallen deze functionaliteit worden toegevoegd onafhankelijk van elkaar. Als u een systeemeigen toepassing die een web-API-aanroepen hebt, kunt u bijvoorbeeld eenvoudig een webtoepassing die ook de web-API-aanroepen toevoegen.

## <a name="app-registration"></a>App-registratie

### <a name="registering-an-app-that-uses-the-azure-ad-v10-endpoint"></a>Registreren van een app die gebruikmaakt van het eindpunt van de Azure AD v1.0

Alle toepassingen die Azure AD-verificatie heeft, moet zijn geregistreerd in een map. Deze stap omvat het Azure AD te vertellen over uw toepassing, met inbegrip van de URL waar deze zich, de URL voor het verzenden van antwoorden na verificatie van de URI voor het identificeren van uw toepassing, en meer. Deze informatie is vereist voor een aantal belangrijke redenen hebben:

* Azure AD moet communiceren met de toepassing bij het verwerken van aanmelding of uitwisselen-tokens. De gegevens doorgegeven tussen Azure AD en de toepassing omvat het volgende:
  
  * **URI toepassings-ID** -de id voor een toepassing. Deze waarde wordt verzonden naar Azure AD tijdens de verificatie om aan te geven welke toepassing de oproepende functie wil een token. Deze waarde is ook opgenomen in het token zodat de toepassing weet dat dit is het beoogde doel.
  * **Antwoord-URL** en **omleidings-URI** -een web-API of webtoepassing, is de antwoord-URL de locatie waar Azure AD het verificatieantwoord stuurt, met inbegrip van een token als verificatie voltooid is. Voor een systeemeigen toepassing is de omleidings-URI een unieke id waarnaar de gebruikersagent in een OAuth 2.0-aanvraag wordt omgeleid met Azure AD.
  * **Toepassings-ID** -de ID voor een toepassing die wordt gegenereerd door Azure AD wanneer de toepassing is geregistreerd. Bij het aanvragen van een autorisatiecode of het token, worden de toepassings-ID en de sleutel worden verzonden naar Azure AD tijdens de verificatie.
  * **Sleutel** -de sleutel die wordt verzonden, samen met een toepassings-ID bij het verifiëren met Azure AD naar een web-API aanroepen.
* Vereist voor Azure AD om te controleren of dat de toepassing heeft de vereiste machtigingen om toegang te krijgen tot de directorygegevens van uw, andere toepassingen in uw organisatie, enzovoort.

Informatie voor meer informatie over hoe u [een app registreren bij de Azure AD-eindpunt v1.0](quickstart-v1-add-azure-ad-app.md).

## <a name="single-tenant-and-multi-tenant-apps"></a>Apps met één tenant en meerdere tenants

Inrichting wordt duidelijker als u weet wat er zijn twee soorten toepassingen die kunnen worden ontwikkeld en geïntegreerd met Azure AD:

* **Eén tenant toepassing** -een toepassing met één tenant is bedoeld voor gebruik in een organisatie. Dit zijn doorgaans line-of-business (LoB)-toepassingen die is geschreven door de ontwikkelaar in een organisatie. Een toepassing met één tenant moet alleen worden geopend door gebruikers in een map en als gevolg hiervan alleen moet worden ingericht in een map. Deze toepassingen zijn meestal geregistreerd door een ontwikkelaar in de organisatie.
* **Toepassing met meerdere tenants** -een toepassing met meerdere tenants is bedoeld voor gebruik in veel organisaties niet slechts één organisatie. Dit zijn normaliter Software-as-a-Service-toepassingen (SaaS), geschreven door een onafhankelijke softwareleverancier (ISV). Toepassingen met meerdere tenants moeten worden ingericht in elke map waar ze worden gebruikt, welke gebruiker of beheerder toestemming om u te registreren. Het toestemmingsproces begint wanneer een toepassing in de map is geregistreerd en toegang verkrijgt tot de Graph-API of wellicht zelfs een andere web-API. Wanneer een gebruiker of beheerder van een andere organisatie zich registreert voor het gebruik van de toepassing, worden ze weergegeven met een dialoogvenster waarin de machtigingen die voor de toepassing moet worden weergegeven. De gebruiker of beheerder kan vervolgens akkoord met de toepassing, die de toepassing toegang geeft tot de opgegeven gegevens en tot slot, registreert u de toepassing in de directory. Zie voor meer informatie, [overzicht van het Framework toestemming geven](consent-framework.md).

### <a name="additional-considerations-when-developing-single-tenant-or-multi-tenant-apps"></a>Aanvullende overwegingen bij het ontwikkelen van één of meerdere tenant-apps

Enkele aanvullende overwegingen zich voordoen bij het ontwikkelen van een toepassing met meerdere tenants in plaats van een toepassing met één tenant. Bijvoorbeeld, als u uw toepassing beschikbaar voor gebruikers in meerdere mappen, moet u een mechanisme om te bepalen welke tenant waarin ze zijn opgeslagen. Een toepassing met één tenant is alleen nodig heeft om te zoeken in een eigen map voor een gebruiker, terwijl een toepassing met meerdere tenants moet een specifieke gebruiker vanuit alle mappen in Azure AD te identificeren. Als u wilt deze taak wordt uitgevoerd, biedt Azure AD een algemene verificatie-eindpunt waar aanmeldingsaanvragen, in plaats van een tenant-specifieke eindpunt dat een toepassing met meerdere tenants instellen kunt. Dit eindpunt is https://login.microsoftonline.com/common voor alle mappen in Azure AD, dat een tenant-specifieke eindpunt mogelijk https://login.microsoftonline.com/contoso.onmicrosoft.com. Het eindpunt van de algemene is vooral belangrijk om te overwegen bij het ontwikkelen van uw toepassing omdat u de benodigde logica moet, voor het afhandelen van meerdere tenants tijdens het aanmelden, afmelden, en validatie van tokens.

Als u momenteel het ontwikkelen van een toepassing met één tenant, maar u wilt het toegankelijk maken voor veel organisaties, kunt u eenvoudig wijzigingen aanbrengen aan de toepassing en de configuratie ervan in Azure AD te maken met meerdere tenants kan worden. Azure AD gebruikt bovendien de dezelfde ondertekeningssleutel voor alle tokens in alle mappen, of u verificatie in een enkele tenant of een toepassing met meerdere tenants opgeeft.

Elk scenario vermeld in dit document bevat een subsectie die de vereisten voor inrichting beschrijft. Zie voor meer gedetailleerde informatie over het inrichten van een toepassing in Azure AD en de verschillen tussen één en multitenant-toepassingen [toepassingen integreren met Azure Active Directory](quickstart-v1-integrate-apps-with-azure-ad.md) voor meer informatie. Doorgaan met lezen voor meer informatie over de veelvoorkomende toepassingsscenario's in Azure AD.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over andere Azure AD [de basisbeginselen van verificatie](authentication-scenarios.md)
