---
title: Ontwikkelaarsaccounts authoriseren met behulp van Azure Active Directory B2C - Azure API Management | Microsoft Docs
description: Informatie over het autoriseren van gebruikers met behulp van Azure Active Directory B2C in API Management.
services: api-management
documentationcenter: API Management
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: bdc479c6cc91c3ebba343a8423a73fb2e0404a73
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992699"
---
# <a name="how-to-authorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a>Hoe ontwikkelaarsaccounts authoriseren met behulp van Azure Active Directory B2C in Azure API Management

## <a name="overview"></a>Overzicht

Azure Active Directory B2C is een cloudoplossing voor het beheer van identiteit voor consumentgerichte web- en mobiele toepassingen. U kunt deze gebruiken voor het beheren van toegang tot uw developer-portal. Deze handleiding geeft u de configuratie die vereist in uw API Management-service om te integreren met Azure Active Directory B2C. Zie voor meer informatie over het inschakelen van toegang tot de portal voor ontwikkelaars met behulp van de klassieke Azure Active Directory [hoe ontwikkelaarsaccounts authoriseren met Azure Active Directory].

> [!NOTE]
> Als u wilt de stappen in deze handleiding hebt voltooid, moet u eerst een Azure Active Directory B2C-tenant te maken van een toepassing in hebben. Bovendien moet u registreren en aanmelden met beleid gereed. Zie voor meer informatie, [overzicht van Azure Active Directory B2C].

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a>Ontwikkelaarsaccounts authoriseren met behulp van Azure Active Directory B2C

1. Als u wilt beginnen, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com) en Ga naar uw API Management-exemplaar.

   > [!NOTE]
   > Als u nog een exemplaar van API Management-service hebt gemaakt, Zie [API Management service-exemplaar maken] [ Create an API Management service instance] in de [aan de slag met Azure API Management-zelfstudie] [Get started with Azure API Management].

2. Onder **identiteiten**. Klik op **+ toevoegen** aan de bovenkant.

   De **id-provider toevoegen** deelvenster wordt weergegeven aan de rechterkant. Choose **Azure Active Directory B2C**.
    
   ![AAD B2C als id-provider toevoegen][api-management-howto-add-b2c-identity-provider]

3. Kopieer de **Omleidings-URL**.

  ![Omleidings-URL voor de provider van de id van de AAD B2C][api-management-howto-copy-b2c-identity-provider-redirect-url]

4. In een nieuw tabblad toegang krijgen tot uw Azure Active Directory B2C-tenant in Azure portal en open de **toepassingen** blade.

  ![Een nieuwe toepassing 1 registreren][api-management-howto-aad-b2c-portal-menu]

5. Klik op de **toevoegen** knop om een nieuwe Azure Active Directory B2C-toepassing te maken.

  ![Een nieuwe toepassing 2 registreren][api-management-howto-aad-b2c-add-button]

6. In de **nieuwe toepassing** blade, voer een naam voor de toepassing. Kies **Ja** onder **Web App of Web-API**, en kies **Ja** onder **impliciete stroom toestaan**. Plak de **Omleidings-URL** gekopieerd in stap 3 in de **antwoord-URL** in het tekstvak.

  ![Een nieuwe toepassing 3 registreren][api-management-howto-aad-b2c-app-details]

7. Klik op de knop **Maken**. Wanneer de toepassing is gemaakt, wordt deze weergegeven de **toepassingen** blade. Klik op de naam van de toepassing om de details te bekijken.

  ![Een nieuwe toepassing 4 registreren][api-management-howto-aad-b2c-app-created]

8. Uit de **eigenschappen** blade, Kopieer de **toepassings-ID** naar het Klembord.

  ![Toepassings-ID 1][api-management-howto-aad-b2c-app-id]

9. Ga terug naar de API Management **id-provider toevoegen** deelvenster en plak de ID in de **Client-Id** in het tekstvak.

  ![Toepassings-ID 2][api-management-howto-aad-b2c-client-id]

10. Ga terug naar de B2C-app-registratie, klikt u op de **sleutels** knop en klik vervolgens op **sleutel genereren**. Klik op **opslaan** aan de configuratie opslaan en weergeven van de **App-sleutel**. De sleutel naar het Klembord kopiëren.

  ![App-sleutel 1][api-management-howto-aad-b2c-app-key]

11. Ga terug naar de API Management **id-provider toevoegen** deelvenster en plak de sleutel in de **Clientgeheim** in het tekstvak.

  ![App-sleutel 2][api-management-howto-aad-b2c-client-secret]

12. Geef de domeinnaam van de Azure Active Directory B2C-tenant in **toegestane Tenant**.

  ![Toegestane tenant][api-management-howto-aad-b2c-allowed-tenant]

13. Geef de **registratie beleid** en **aanmelding beleid** van de B2C-Tenant voor. (Optioneel) u kunt ook opgeven de **bewerken-Mailprofielbeleid** en **wachtwoord opnieuw instellen van beleid**.

  ![Beleidsregels][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > Zie voor meer informatie over het beleid [Azure Active Directory B2C: Uitbreidbaar beleidsschema].

14. Nadat u de gewenste configuratie hebt opgegeven, klikt u op **opslaan**.

  Nadat de wijzigingen zijn opgeslagen, zich ontwikkelaars op nieuwe accounts maken en aanmelden bij de portal voor ontwikkelaars met behulp van Azure Active Directory B2C.

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a>Aanmelden voor een developer-account met behulp van Azure Active Directory B2C

1. Als u wilt zich registreert voor een developer-account met behulp van Azure Active Directory B2C, open een nieuw browservenster en Ga naar de portal voor ontwikkelaars. Klik op de **aanmelden** knop.

   ![1-portal voor ontwikkelaars][api-management-howto-aad-b2c-dev-portal]

2. Kies aan te melden met **Azure Active Directory B2C**.

   ![2-portal voor ontwikkelaars][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. U bent omgeleid naar de signup-beleid dat u in de vorige sectie hebt geconfigureerd. Kies deze optie in om u te registreren met behulp van uw e-mailadres of een van uw bestaande sociale accounts.

   > [!NOTE]
   > Als Azure Active Directory B2C de enige optie die ingeschakeld is op de **identiteiten** tabblad in de publicatieportal u omgeleid naar het beleid voor aanmelding bij rechtstreeks.

   ![ontwikkelaarsportal][api-management-howto-aad-b2c-dev-portal-b2c-options]

   Wanneer de registratie voltooid is, kunt u bent omgeleid naar de portal voor ontwikkelaars. U bent nu aangemeld bij de portal voor ontwikkelaars voor uw API Management service-exemplaar.

    ![Registratie is voltooid][api-management-registration-complete]

## <a name="next-steps"></a>Volgende stappen

*  [Overzicht van Azure Active Directory B2C]
*  [Azure Active Directory B2C: Uitbreidbaar beleidsschema]
*  [Een Microsoft-account gebruiken als een id-provider in Azure Active Directory B2C]
*  [Een Google-account gebruiken als een id-provider in Azure Active Directory B2C]
*  [Een LinkedIn-account gebruiken als een id-provider in Azure Active Directory B2C]
*  [Een Facebook-account gebruiken als een id-provider in Azure Active Directory B2C]



[api-management-howto-add-b2c-identity-provider]: ./media/api-management-howto-aad-b2c/api-management-add-b2c-identity-provider.PNG
[api-management-howto-copy-b2c-identity-provider-redirect-url]: ./media/api-management-howto-aad-b2c/api-management-b2c-identity-provider-redirect-url.PNG
[api-management-howto-aad-b2c-portal-menu]: ./media/api-management-howto-aad-b2c/api-management-b2c-portal-menu.PNG
[api-management-howto-aad-b2c-add-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-add-button.PNG
[api-management-howto-aad-b2c-app-details]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-details.PNG
[api-management-howto-aad-b2c-app-created]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-created.PNG
[api-management-howto-aad-b2c-app-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-id.PNG
[api-management-howto-aad-b2c-client-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-id.PNG
[api-management-howto-aad-b2c-app-key]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key.PNG
[api-management-howto-aad-b2c-app-key-saved]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key-saved.PNG
[api-management-howto-aad-b2c-client-secret]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-secret.PNG
[api-management-howto-aad-b2c-allowed-tenant]: ./media/api-management-howto-aad-b2c/api-management-b2c-allowed-tenant.PNG
[api-management-howto-aad-b2c-policies]: ./media/api-management-howto-aad-b2c/api-management-b2c-policies.PNG
[api-management-howto-aad-b2c-dev-portal]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-button.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-options]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-options.PNG
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.PNG
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-b2c-security-tab.png
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
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
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
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[https://oauth.net/2/]: https://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: https://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph
[Overzicht van Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview
[Hoe ontwikkelaarsaccounts authoriseren met Azure Active Directory]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad
[Azure Active Directory B2C: Uitbreidbaar beleidsschema]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies
[Een Microsoft-account gebruiken als een id-provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app
[Een Google-account gebruiken als een id-provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app
[Een Facebook-account gebruiken als een id-provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app
[Een LinkedIn-account gebruiken als een id-provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
