---
title: API-vereisten | Microsoft Docs
description: Vereisten voor met behulp van de Cloud Partner Portal-API's.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: e6e25424981118363070d42b2fb057b18fdb91a9
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810169"
---
<a name="api-prerequisites"></a>API-vereisten
================

Er zijn twee vereist programmatische activa die u wilt gebruiken de API's voor Cloud Partner-Portal: een service-principal en een Azure Active Directory (Azure AD)-toegangstoken.


<a name="create-a-service-principal-in-your-azure-active-directory-tenant"></a>Een service-principal maken in uw Azure Active Directory-tenant
----------------------------------------------------------------

Eerst moet u een serviceprincipal maken in uw Azure AD-tenant. Deze tenant krijgt een eigen set machtigingen in de Cloud Partner-Portal. Uw code wordt aangeroepen met behulp van API's als deze tenant in plaats van met uw persoonlijke gegevens.  Zie voor een volledige uitleg over het maken van een service-principal, [portal gebruiken voor het maken van een Azure Active Directory-toepassing en service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).


<a name="add-the-service-principal-to-your-account"></a>De service-principal toevoegen aan uw account
-----------------------------------------

Nu dat u de service principal in uw tenant gemaakt hebt, kunt u deze als een gebruiker toevoegen aan uw account Cloud Partner-Portal. Net als een gebruiker, kan de service-principal zijn een eigenaar of Inzender van de portal.

Gebruik de volgende stappen uit om toe te voegen van de service-principal:

1. Meld u bij de Cloud Partner-Portal. 
2. Klik op **gebruikers** op de menubalk linksboven en kies **gebruiker toevoegen**.

   ![Een gebruiker toevoegen aan de portal](./media/cloud-partner-portal-api-prerequisites/add-user.jpg)

3. Uit de **Type** vervolgkeuzelijst **Service-Principal** en voeg de volgende gegevens toe:

-   Een **beschrijvende naam** voor de service-principal, bijvoorbeeld `spAccount`.
-   De **toepassings-ID**. Als u deze id zoekt, gaat u naar de [Azure Portal](https://portal.azure.com), klikt u op **Azure Active Directory**, kiest u **App-registraties**, en klikt u op uw app.
-   De **Tenant-ID**, ook wel bekend als de **map-ID**, voor uw Azure AD-tenant. U kunt deze id vinden in de Azure Active Directory-pagina in de [Azure-portal](https://portal.azure.com)onder **eigenschappen**.
-   De **Object-ID** voor uw service-principal-object. U kunt deze id ophalen uit de Azure-portal. Ga naar **Azure Active Directory**, kiest u **App-registraties**, klikt u op uw app en klik op de naam van de app onder **beheerde toepassing in lokale map**. Ga vervolgens naar de **eigenschappen** pagina, naar de Object-ID. Zorg ervoor dat u bent niet de gegevens uit de oorspronkelijke Object-ID die zich op uw app, maar in plaats daarvan de Object-ID in de beheerde toepassing formulieren.
-   De **rol** die zijn gekoppeld aan het account dat wordt gebruikt voor RBAC.

     ![Een beheerde app toevoegen aan de portal](./media/cloud-partner-portal-api-prerequisites/managedapp.png)

4. Klik op **toevoegen** de service-principal toevoegen aan uw account.

   ![Een service-principal toevoegen](./media/cloud-partner-portal-api-prerequisites/add-service-principal.jpg)


<a name="get-an-azure-ad-access-token"></a>Een Azure AD-toegangstoken verkrijgen
----------------------------

De Cloud Partner Portal-API's maken gebruik van de volgende elementen en protocollen tijdens de verificatie:

- Een JSON Web Token (JWT) bearer-token voor het aanvragen van toegang tot resources
- De [OpenID Connect](http://openid.net/connect/) (OIDC)-protocol om identiteit te verifiëren
- [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) als de identiteit-instantie

Er zijn twee benaderingen van principe programmatisch ophalen van een JWT-token:

- De Microsoft Authentication Library voor .NET gebruiken ([MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)).  Deze benadering van een hoger niveau wordt aanbevolen voor .NET-ontwikkelaars. 
- Controleer een **HTTP POST** aanvraag naar de Azure AD oauth **token** eindpunt, waarbij het formulier:

``` HTTP
POST https://login.microsoftonline.com/<tenant-id>/oauth2/token
    client_id: <application-id>
    client_secret:<application-secret>
    grant_type: client_credentials
    resource: https://cloudpartner.azure.com
```

Nu kunt u dit token doorgeven als onderdeel van de autorisatie-header voor API-aanvragen.

``` HTTP
GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview 
    Accept: application/json
    Authorization: Bearer <access-token>
```
> [!NOTE]
> Voor alle API's in deze documentatie, wordt de autorisatie-header altijd uitgegaan doorgegeven, zodat deze niet wordt vermeld.

Als u verificatiefouten in uw aanvraag, Zie [verificatiefouten oplossen](./cloud-partner-portal-api-troubleshooting-authentication-errors.md).
