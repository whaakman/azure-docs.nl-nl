---
title: Autoriseren ontwikkelaarsaccounts met Azure Active Directory - Azure API Management | Microsoft Docs
description: Informatie over het autoriseren van gebruikers met Azure Active Directory in API Management.
services: api-management
documentationcenter: API Management
author: vladvino
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 2e8bc33ddf38657fe2d0d84a7bf64d4177d51572
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
> [!WARNING]
> Azure Active Directory-integratie is beschikbaar in de [Developer- en Premium](https://azure.microsoft.com/en-us/pricing/details/api-management/) alleen lagen.

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Hoe ontwikkelaarsaccounts in Azure API Management met behulp van Azure Active Directory autoriseren
## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u toegang inschakelen voor de portal voor ontwikkelaars voor gebruikers van Azure Active Directory. Deze handleiding ook wordt beschreven hoe u groepen met Azure Active Directory-gebruikers beheren door externe groepen de gebruikers van een Azure Active Directory bevatten toe te voegen.

> U moet een Azure Active Directory in te maken van een toepassing hebben voor het voltooien van de stappen in deze handleiding.
> 

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Hoe ontwikkelaarsaccounts met Azure Active Directory autoriseren
Om te beginnen, klikt u op **publicatieportal** in de Azure portal voor uw API Management-service. Hiermee gaat u naar de publicatieportal van API Management.

![Publicatieportal][api-management-management-console]

> Als u nog geen service-exemplaar van API Management hebt gemaakt, raadpleegt u [Service-exemplaar van API Management maken][Create an API Management service instance] in de zelfstudie [Aan de slag met Azure API Management][Get started with Azure API Management].
> 
> 

Klik op **beveiliging** van de **API Management** menu aan de linkerkant en klik op **externe identiteiten**.

![Externe identiteit][api-management-security-external-identities]

Klik op **Azure Active Directory**. Noteer de **Omleidings-URL** en schakel naar uw Azure Active Directory in de klassieke Azure-Portal.

![Externe identiteit][api-management-security-aad-new]

Klik op de **toevoegen** klikken om een nieuwe Azure Active Directory-toepassing maken en kies **mijn organisatie ontwikkelt toepassing toevoegen**.

![Nieuwe Azure Active Directory-toepassing toevoegen][api-management-new-aad-application-menu]

Voer een naam voor de toepassing, selecteer **Web-toepassing en/of Web-API**, en klik op de knop Volgende.

![Nieuwe Azure Active Directory-toepassing][api-management-new-aad-application-1]

Voor **aanmeldings-URL**, voer de aanmeldings-URL van uw ontwikkelaarsportal. In dit voorbeeld wordt de **aanmeldings-URL** is `https://aad03.portal.current.int-azure-api.net/signin`. 

Voor de **App-ID-URL**, voer het standaarddomein of een aangepast domein voor de Azure Active Directory en een unieke tekenreeks toegevoegd aan. In dit voorbeeld wordt het standaarddomein van **https://contoso5api.onmicrosoft.com** wordt gebruikt met het achtervoegsel van **/api** opgegeven.

![Eigenschappen van nieuwe Azure Active Directory-toepassing][api-management-new-aad-application-2]

Klik op de knop met het vinkje om op te slaan en de toepassing maken overschakelen naar de **configureren** tabblad configureren van de nieuwe toepassing.

![Nieuwe Azure Active Directory-toepassing gemaakt][api-management-new-aad-app-created]

Als meerdere Azure Active Directory's worden gebruikt voor deze toepassing gaat, klikt u op **Ja** voor **toepassing is multitenant**. De standaardwaarde is **Nee**.

![Toepassing is multitenant][api-management-aad-app-multi-tenant]

Kopieer de **Omleidings-URL** van de **Azure Active Directory** sectie van de **externe identiteiten** tabblad in de publicatieportal en plak deze in de **antwoord-URL** in het tekstvak. 

![Antwoord-URL][api-management-aad-reply-url]

Ga naar de onderkant van het tabblad configureren, selecteer de **Toepassingsmachtigingen** vervolgkeuzelijst en Controleer **mapgegevens lezen**.

![Toepassingsmachtigingen][api-management-aad-app-permissions]

Selecteer de **machtigingen delegeren** vervolgkeuzelijst en Controleer **aanmelding inschakelen en gebruikersprofiel lezen**.

![Gedelegeerde machtigingen][api-management-aad-delegated-permissions]

> Zie voor meer informatie over de toepassing en gedelegeerde machtigingen, [toegang tot de Graph API][Accessing the Graph API].
> 
> 

Kopieer de **Client-Id** naar het Klembord.

![Client-Id][api-management-aad-app-client-id]

Ga terug naar de publicatieportal en plak in het **Client-Id** gekopieerd van de configuratie van de Azure Active Directory-toepassing.

![Client-Id][api-management-client-id]

Ga terug naar de Azure Active Directory-configuratie en klikt u op de **Selecteer duur** omlaag in de **sleutels** sectie en geeft u een interval. In dit voorbeeld **1 jaar** wordt gebruikt.

![Sleutel][api-management-aad-key-before-save]

Klik op **opslaan** de configuratie op te slaan en de sleutel wordt weergegeven. De sleutel naar het Klembord kopiëren.

> Noteer deze sleutel. Wanneer u het venster Azure Active Directory-configuratie hebt gesloten, wordt de sleutel kan niet opnieuw weergegeven.
> 
> 

![Sleutel][api-management-aad-key-after-save]

Ga terug naar de publicatieportal en plak de sleutel in de **Clientgeheim** in het tekstvak.

![Clientgeheim][api-management-client-secret]

**Tenants toegestaan** geeft aan welke mappen zijn voor toegang tot de API's van het exemplaar van API Management-service. Geef de domeinen van de Azure Active Directory-exemplaren waartoe u toegang wilt verlenen. U kunt meerdere domeinen scheiden met nieuwe regels, spaties of komma's.

![Toegestane tenants][api-management-client-allowed-tenants]


Nadat de gewenste configuratie wordt opgegeven, klikt u op **opslaan**.

![Opslaan][api-management-client-allowed-tenants-save]

Wanneer de wijzigingen zijn opgeslagen, de gebruikers in de opgegeven Azure Active Directory kunnen aanmelden bij de portal voor ontwikkelaars door de stappen in [aanmelden bij de portal voor ontwikkelaars met een Azure Active Directory-account][Log in to the Developer portal using an Azure Active Directory account].

Meerdere domeinen kunnen worden opgegeven in de **Tenants toegestaan** sectie. Voordat een gebruiker zich vanaf een ander domein dan het oorspronkelijke domein waar de toepassing is geregistreerd aanmelden kan, moet een globale beheerder van het andere domein machtiging voor de toepassing directorygegevens toegang verlenen. Als u wilt machtigen, de globale beheerder moet gaat u naar `https://<URL of your developer portal>/aadadminconsent` (bijvoorbeeld https://contoso.portal.azure-api.net/aadadminconsent), typt u de domeinnaam van de Active Directory-tenant die ze willen krijgen en klik op verzenden. In het volgende voorbeeld wordt een globale beheerder van `miaoaad.onmicrosoft.com` wilt machtigen voor deze specifieke developer-portal. 

![Machtigingen][api-management-aad-consent]

In het volgende scherm wordt de globale beheerder wordt gevraagd om te bevestigen dat de toestemming geven. 

![Machtigingen][api-management-permissions-form]

> Als een niet-globale beheerder probeert aan te melden voordat een globale beheerder machtigingen worden verleend, wordt de aanmeldingspoging mislukt en wordt een fout wordt weergegeven.
> 
> 

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Het toevoegen van een externe Azure Active Directory-groep
Nadat de toegang voor gebruikers in een Azure Active Directory is ingeschakeld, kunt u Azure Active Directory-groepen toevoegen in API Management koppeling van de ontwikkelaars in de groep met de gewenste producten eenvoudiger te beheren.

> Voor het configureren van een externe Azure Active Directory-groep moet de Azure Active Directory eerst worden geconfigureerd op het tabblad identiteiten via de volgende procedure in de vorige sectie. 
> 
> 

Externe Azure Active Directory-groepen worden toegevoegd vanuit de **zichtbaarheid** tabblad van het product waarvoor u toegang wilt verlenen aan de groep. Klik op **producten**, en klik vervolgens op de naam van de gewenste product.

![Product configureren][api-management-configure-product]

Overschakelen naar de **zichtbaarheid** tabblad en klik op **groepen toevoegen van Azure Active Directory**.

![Groepen toevoegen][api-management-add-groups]

Selecteer de **Azure Active Directory-Tenant** in de vervolgkeuzelijst lijst en typ vervolgens de naam van de gewenste groep in de **groepen** moet worden toegevoegd in het tekstvak.

![Groep selecteren][api-management-select-group]

De naam van deze groep kan worden gevonden in de **groepen** lijst voor uw Azure Active Directory, zoals wordt weergegeven in het volgende voorbeeld.

![Lijst van Azure Active Directory-groepen][api-management-aad-groups-list]

Klik op **toevoegen** valideren van de groepsnaam en voeg de groep. In dit voorbeeld wordt de **Contoso 5 ontwikkelaars** externe groep wordt toegevoegd. 

![Groep toegevoegd][api-management-aad-group-added]

Klik op **opslaan** om op te slaan van de selectie van de nieuwe groep.

Zodra een Azure Active Directory-groep van een product is geconfigureerd, is het beschikbaar moet worden gecontroleerd op de **zichtbaarheid** tabblad voor de andere producten in de service-exemplaar van API Management.

Om te bekijken en configureren van de eigenschappen voor externe groepen nadat ze zijn toegevoegd, klikt u op de naam van de groep uit de **groepen** tabblad.

![Groepen beheren][api-management-groups]

Hier kunt u de **naam** en de **beschrijving** van de groep.

![Groep bewerken][api-management-edit-group]

Gebruikers van de geconfigureerde Azure Active Directory kunnen aanmelden bij de portal voor ontwikkelaars en de weergave en zich abonneren op alle groepen waarvan ze inzicht hebben door de instructies in de volgende sectie.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Hoe u zich aanmelden bij de portal voor ontwikkelaars met een Azure Active Directory-account
Als u wilt zich aanmelden bij de portal voor ontwikkelaars met een Azure Active Directory-account geconfigureerd in de vorige secties, open een nieuw browser venster met de **aanmeldings-URL** in de configuratie van Active Directory-toepassing en klik op **Azure Active Directory**.

![Portal voor ontwikkelaars][api-management-dev-portal-signin]

Voer de referenties van een van de gebruikers in uw Azure Active Directory en klikt u op **aanmelden**.

![Aanmelden][api-management-aad-signin]

U wordt mogelijk gevraagd met een registratieformulier als eventuele aanvullende informatie vereist is. Vul het registratieformulier en klik op **aanmelden**.

![Registratie][api-management-complete-registration]

De gebruiker is nu aangemeld bij de portal voor ontwikkelaars voor uw API Management-service-exemplaar.

![Inschrijving voltooid][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-aad-consent]: ./media/api-management-howto-aad/api-management-aad-consent.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

