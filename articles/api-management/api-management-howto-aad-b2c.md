---
title: Ontwikkelaarsaccounts autoriseren met behulp van Azure Active Directory B2C - Azure API Management | Microsoft Docs
description: Informatie over het autoriseren van gebruikers met behulp van Azure Active Directory B2C in API Management.
services: api-management
documentationcenter: API Management
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: d843757c2ed63c4f8cff09d809c2de382b2aeb59
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
> [!WARNING]
> Integratie van Azure Active Directory B2C is beschikbaar in de [Developer- en Premium](https://azure.microsoft.com/en-us/pricing/details/api-management/) alleen lagen.

# <a name="how-to-authorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a>Hoe ontwikkelaarsaccounts autoriseren met behulp van Azure Active Directory B2C in Azure API Management
## <a name="overview"></a>Overzicht
Azure Active Directory B2C is een oplossing voor identiteitsbeheer voor consumentgerichte webtoepassingen en mobiele toepassingen cloud. U kunt deze gebruiken voor toegang tot uw ontwikkelaarsportal beheren. Deze handleiding beschrijft u de configuratie die vereist is in uw API Management-service om te integreren met Azure Active Directory B2C. Zie voor meer informatie over het inschakelen van toegang tot de portal voor ontwikkelaars via de klassieke Azure Active Directory [hoe ontwikkelaarsaccounts met Azure Active Directory autoriseren].

> [!NOTE]
> U moet een Azure Active Directory B2C-tenant maken van een toepassing in hebben voor het voltooien van de stappen in deze handleiding. U moet ook, aanmelding en aanmelding beleid gereed. Zie voor meer informatie [overzicht van Azure Active Directory B2C].

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a>Ontwikkelaarsaccounts autoriseren met behulp van Azure Active Directory B2C

1. Om te beginnen, klikt u op **publicatieportal** in de Azure portal voor uw API Management-service. Hiermee gaat u naar de publicatieportal van API Management.

   ![Publicatieportal][api-management-management-console]

   > [!NOTE]
   > Als u nog een exemplaar van API Management-service hebt gemaakt, Zie [API Management service-exemplaar maken] [ Create an API Management service instance] in de [aan de slag met Azure API Management-zelfstudie] [Get started with Azure API Management].

2. Op de **API Management** menu, klikt u op **beveiliging**. Op de **identiteiten** Kies **Azure Active Directory B2C**.

  ![Externe identiteiten 1][api-management-howto-aad-b2c-security-tab]

3. Noteer de **Omleidings-URL** en schakel naar Azure Active Directory B2C in de Azure portal.

  ![Externe identiteiten 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. Klik op de **toepassingen** knop.

  ![Een nieuwe toepassing 1 registreren][api-management-howto-aad-b2c-portal-menu]

5. Klik op de **toevoegen** knop een nieuwe Azure Active Directory B2C-toepassing maken.

  ![Een nieuwe toepassing 2 registreren][api-management-howto-aad-b2c-add-button]

6. In de **nieuwe toepassing** blade een naam voor de toepassing. Kies **Ja** onder **Web App of Web-API**, en kies **Ja** onder **toestaan impliciete stroom**. Kopieer de **Omleidings-URL** van de **Azure Active Directory B2C** sectie van de **identiteiten** tabblad in de publicatieportal en plak deze in de **antwoord URL** in het tekstvak.

  ![Registreren van een nieuwe toepassing 3][api-management-howto-aad-b2c-app-details]

7. Klik op de knop **Maken**. Wanneer de toepassing is gemaakt, wordt deze weergegeven de **toepassingen** blade. Klik op de naam van de toepassing om de details te bekijken.

  ![Een nieuwe toepassing 4 registreren][api-management-howto-aad-b2c-app-created]

8. Van de **eigenschappen** blade-, Kopieer de **toepassings-ID** naar het Klembord.

  ![Toepassings-ID 1][api-management-howto-aad-b2c-app-id]

9. Ga terug naar de publicatieportal en plakt u de ID in de **Client-Id** in het tekstvak.

  ![Toepassings-ID 2][api-management-howto-aad-b2c-client-id]

10. Ga terug naar de Azure-portal, klikt u op de **sleutels** knop en klik vervolgens op **sleutel genereren**. Klik op **opslaan** de configuratie op te slaan en wordt weergegeven de **App-sleutel**. De sleutel naar het Klembord kopiëren.

  ![App-sleutel 1][api-management-howto-aad-b2c-app-key]

11. Ga terug naar de publicatieportal en plak de sleutel in de **Clientgeheim** in het tekstvak.

  ![App-sleutel 2][api-management-howto-aad-b2c-client-secret]

12. Geef de domeinnaam van de Azure Active Directory B2C-tenant in **Tenant toegestaan**.

  ![Toegestane tenant][api-management-howto-aad-b2c-allowed-tenant]

13. Geef de **aanmelding beleid** en **aanmelding beleid**. Desgewenst kunt u ook opgeven de **profiel bewerken beleid** en **opnieuw instellen wachtwoordbeleid**.

  ![Beleidsregels][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > Zie voor meer informatie over beleidsregels [Azure Active Directory B2C: uitbreidbaar beleidsframework].

14. Nadat u de gewenste configuratie hebt opgegeven, klikt u op **opslaan**.

  Nadat de wijzigingen zijn opgeslagen, zich ontwikkelaars voor het maken van nieuwe accounts en aanmelden bij de portal voor ontwikkelaars met behulp van Azure Active Directory B2C.

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a>Aanmelden voor een ontwikkelaarsaccount met behulp van Azure Active Directory B2C

1. Aanmelden voor een ontwikkelaarsaccount met behulp van Azure Active Directory B2C, open een nieuw browservenster en gaat u naar de portal voor ontwikkelaars. Klik op de **aanmelden** knop.

   ![1-portal voor ontwikkelaars][api-management-howto-aad-b2c-dev-portal]

2. Wilt u zich aanmeldt met **Azure Active Directory B2C**.

   ![2-portal voor ontwikkelaars][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. U wordt omgeleid naar het registratie-beleid die u hebt geconfigureerd in de vorige sectie. Kies aan te melden met behulp van uw e-mailadres of een van uw bestaande sociale accounts.

   > [!NOTE]
   > Als u Azure Active Directory B2C is de enige optie die ingeschakeld op de **identiteiten** tabblad in de publicatieportal u zult omgeleid naar het registratie-beleid rechtstreeks.

   ![ontwikkelaarsportal][api-management-howto-aad-b2c-dev-portal-b2c-options]

   Wanneer de aanmelding voltooid is, bent u omgeleid naar de portal voor ontwikkelaars. U bent nu aangemeld bij de portal voor ontwikkelaars voor uw API Management-service-exemplaar.

    ![Inschrijving voltooid][api-management-registration-complete]

## <a name="next-steps"></a>Volgende stappen

*  [overzicht van Azure Active Directory B2C]
*  [Azure Active Directory B2C: uitbreidbaar beleidsframework]
*  [Een Microsoft-account gebruiken als een id-provider in Azure Active Directory B2C]
*  [Een Google-account gebruiken als een id-provider in Azure Active Directory B2C]
*  [Een LinkedIn-account gebruiken als een id-provider in Azure Active Directory B2C]
*  [Een Facebook-account gebruiken als een id-provider in Azure Active Directory B2C]




[api-management-howto-aad-b2c-security-tab]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab.PNG
[api-management-howto-aad-b2c-security-tab-reply-url]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab-reply-url.PNG
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

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph
[overzicht van Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview
[hoe ontwikkelaarsaccounts met Azure Active Directory autoriseren]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad
[Azure Active Directory B2C: uitbreidbaar beleidsframework]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies
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
