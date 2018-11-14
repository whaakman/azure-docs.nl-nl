---
title: Ervaring van de eindgebruikers voor toepassingen - Azure Active Directory | Microsoft Docs
description: Azure Active Directory (Azure AD) biedt aanpasbare manieren voor het implementeren van toepassingen voor eindgebruikers in uw organisatie.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: barbkess
ms.reviewer: arvindh
ms.openlocfilehash: b1a4ec4b3a75d1337c011275591af11da8c45a6a
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629015"
---
# <a name="end-user-experiences-for-applications-in-azure-active-directory"></a>Ervaringen van eindgebruikers voor toepassingen in Azure Active Directory
Azure Active Directory (Azure AD) biedt aanpasbare manieren voor het implementeren van toepassingen voor eindgebruikers in uw organisatie:

* Azure AD-Toegangsvenster
* Startprogramma voor Office 365-toepassingen
* Directe aanmelding bij federatieve apps
* Dieptekoppelingen naar federatieve apps, op basis van wachtwoorden, of bestaande apps

Welke methode(n) die u wilt implementeren in uw organisatie is's naar eigen inzicht.

## <a name="azure-ad-access-panel"></a>Azure AD-Toegangsvenster
Het deelvenster toegang https://myapps.microsoft.com is een webportal waarmee een eindgebruiker met een organisatie-account in Azure Active Directory om weer te geven en starten cloud-gebaseerde toepassingen waaraan ze zijn toegang verleend door de Azure AD-beheerder. Als u een eindgebruiker met [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), u kunt ook gebruikmaken van mogelijkheden voor groepsbeheer met Self-service via het toegangsvenster.

![Azure AD-Toegangsvenster](media/what-is-single-sign-on/azure-ad-access-panel.png)

Het toegangsvenster is gescheiden van de Azure-portal en hoeft geen gebruikers hebben een Azure-abonnement of een Office 365-abonnement.

Zie voor meer informatie over de Azure AD-toegangspaneel, de [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="office-365-application-launcher"></a>Startprogramma voor Office 365-toepassingen
Voor organisaties die Office 365 hebt geïmplementeerd, toepassingen die zijn toegewezen aan gebruikers via Azure AD ook worden weergegeven in de Office 365-portal op https://portal.office.com/myapps. Dit maakt het gemakkelijk en handig voor gebruikers in een organisatie hun apps starten zonder gebruik te maken van een tweede portal en is de aanbevolen app starten oplossing voor organisaties met behulp van Office 365.

![](./media/what-is-single-sign-on/officeapphub.png)

Zie voor meer informatie over het startprogramma voor Office 365-toepassingen, [uw App worden weergegeven in het startprogramma voor Office 365](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

## <a name="direct-sign-on-to-federated-apps"></a>Directe aanmelding bij federatieve apps
Meest federatieve toepassingen die ondersteuning bieden voor SAML 2.0, WS-Federation en OpenID connect ook ondersteuning voor de mogelijkheid voor gebruikers om te beginnen bij de toepassing en vervolgens u aangemeld via Azure AD door automatische omleiding of door te klikken op een koppeling aan te melden bij. Dit staat bekend als serviceprovider-aanmelding wordt gestart en meest federatieve toepassingen in de Azure AD-toepassingsgalerie ondersteuning bieden voor deze (Zie de documentatie gekoppeld aan de app configuratie voor eenmalige aanmelding wizard in de Azure-portal voor meer informatie).

![](./media/what-is-single-sign-on/workdaymobile.png)

## <a name="direct-sign-on-links"></a>Directe aanmelding koppelingen
Azure AD biedt ook ondersteuning voor directe eenmalige aanmelding in koppelingen naar afzonderlijke toepassingen die ondersteuning op basis van wachtwoorden eenmalige aanmelding, gekoppelde eenmalige aanmelding en een vorm van federatieve eenmalige aanmelding bieden.

Deze koppelingen zijn speciaal ontworpen URL's die een gebruiker door het proces voor aanmelding bij Azure AD voor een bepaalde toepassing verzenden zonder de gebruiker start ze vanuit de Azure AD toegang tot deelvenster of Office 365. Deze één aanmeldings-URL's kan worden gevonden op het tabblad Dashboard van vooraf geïntegreerde toepassingen in de sectie Active Directory van Azure portal, zoals wordt weergegeven in de onderstaande schermafbeelding.

![](./media/what-is-single-sign-on/deeplink.png)

Deze koppelingen kan worden gekopieerd en geplakt waar die u wilt een koppeling aanmelding aan de geselecteerde toepassing wilt opgeven. Dit wordt mogelijk in een e-mailbericht, of in een aangepaste web-portal op basis van die u hebt ingesteld voor toegang tot de toepassing van gebruiker. Hier volgt een voorbeeld van een Azure AD direct één aanmeldings-URL voor Twitter:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Net als bij de organisatie-specifieke URL's voor het toegangsvenster, u kunt verder aanpassen deze URL door een van de actieve of geverifieerde domeinen voor uw directory nadat de myapps.microsoft.com domein toe te voegen. Dit zorgt ervoor dat eventuele huisstijl van de organisatie wordt geladen onmiddellijk op de pagina aanmelden zonder dat de gebruiker hoeft in te voeren hun gebruikers-ID eerst:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Wanneer een geautoriseerde gebruiker op een van deze koppelingen toepassingsspecifieke klikt, ze eerst zien van hun organisatie-aanmeldingspagina (ervan uitgaande dat ze nog niet bent aangemeld) en na het aanmelden worden omgeleid naar de app zonder eerst op het toegangsvenster stoppen. Als de gebruiker vereisten ontbreken er voor toegang tot de toepassing, zoals de Browseruitbreiding van eenmalige aanmelding op basis van wachtwoorden, wordt klikt u vervolgens de koppeling de gebruiker gevraagd de ontbrekende extensie installeren. URL van de koppeling blijft ook constante als de configuratie voor eenmalige aanmelding voor de toepassing wordt gewijzigd.

Deze koppelingen de mechanismen voor hetzelfde besturingselement gebruiken als het toegangsvenster en de Office 365, en alleen deze gebruikers of groepen die zijn toegewezen aan de toepassing in Azure portal is mogelijk om te verifiëren. Elke gebruiker die niet gemachtigd is zien echter een bericht waarin wordt uitgelegd dat ze geen toegang is verleend en een koppeling naar het laden van het toegangsvenster als u wilt weergeven van beschikbare toepassingen waarvoor ze toegang hebben, krijgen.

## <a name="next-steps"></a>Volgende stappen
Zie voor de implementatieplannen, [Azure Active Directory-implementatieplannen](../fundamentals/active-directory-deployment-plans.md)