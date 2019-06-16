---
title: Ontwikkelaarsaccounts authoriseren met behulp van Azure Active Directory - Azure API Management | Microsoft Docs
description: Informatie over het autoriseren van gebruikers met behulp van Azure Active Directory in API Management.
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
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: d267ff3a43438d9fe6e4e21f0ac023cfa6675f19
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65956302"
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Ontwikkelaarsaccounts authoriseren met behulp van Azure Active Directory in Azure API Management

Dit artikel leest u hoe de developer-Portal voor gebruikers om toegang te krijgen van Azure Active Directory (Azure AD). Deze handleiding leest u ook hoe voor het beheren van groepen van Azure AD-gebruikers door externe groepen met de gebruikers toe te voegen.

## <a name="prerequisites"></a>Vereisten

- Voltooi de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
- Importeren en publiceren van een Azure API Management-exemplaar. Zie voor meer informatie, [importeren en publiceren](import-and-publish.md).

[!INCLUDE [premium-dev-standard.md](../../includes/api-management-availability-premium-dev-standard.md)]

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Ontwikkelaarsaccounts authoriseren met behulp van Azure AD

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Selecteer ![pijl](./media/api-management-howto-aad/arrow.png).
3. Type **api** in het zoekvak in.
4. Selecteer **API Management-services**.
5. Selecteer uw service-exemplaar van API Management.
6. Onder **Security**, selecteer **identiteiten**.
7. Selecteer **+ toevoegen** vanaf de bovenkant.

    De **id-provider toevoegen** deelvenster wordt weergegeven aan de rechterkant.
8. Onder **providertype**, selecteer **Azure Active Directory**.

    Besturingselementen waarmee u kunt andere benodigde informatie op te geven in het deelvenster worden weergegeven. De besturingselementen zijn **Client-ID** en **clientgeheim**. (U krijgt informatie over deze besturingselementen later in dit artikel.)
9. Maak een notitie van de inhoud van **Omleidings-URL**.
    
   ![Stappen voor het toevoegen van een id-provider in Azure portal](./media/api-management-howto-aad/api-management-with-aad001.png)  
10. Open een ander tabblad in uw browser. 
11. Navigeer naar de [Azure portal - App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) een app registreren in Active Directory.
12. Onder **beheren**, selecteer **App-registraties**.
13. Selecteer **registratie van nieuwe**. Op de **registreren van een toepassing** pagina, stelt u de waarden als volgt:
    
* Stel **naam** naar een beschrijvende naam op. e.g., *developer-portal*
* Stel **ondersteund accounttypen** naar **Accounts in deze organisatie-map alleen**. 
* Stel **omleidings-URI** op de waarde die u hebt verkregen in stap 9. 
* Kies **registreren**. 

14.  Nadat de toepassing is geregistreerd, kopieert u de **(client) toepassings-ID** uit de **overzicht** pagina. 
15. Ga terug naar uw API Management-exemplaar. In de **id-provider toevoegen** venster, plak de **(client) toepassings-ID** waarde in de **Client-ID** vak.
16. Ga terug naar de Azure AD-configuratie, selecteer **certificaten en geheimen** onder **beheren**. Selecteer de **nieuwe clientgeheim** knop. Voer een waarde in **beschrijving**, selecteer een optie voor **verloopt** en kies **toevoegen**. Kopieer de geheime waarde van de client voordat u de pagina verlaat. U hebt deze gegevens nodig in de volgende stap. 
17. Onder **beheren**, selecteer **verificatie** en selecteer vervolgens **ID-tokens** onder **impliciete toekenning**
18. Ga terug naar uw API Management-exemplaar, plakt u het geheim in de **clientgeheim** vak.

    > [!IMPORTANT]
    > Zorg ervoor dat u het bijwerken van de **clientgeheim** voordat de sleutel is verlopen. 
    >  
    >

19. De **id-provider toevoegen** venster bevat ook de **Tenants toegestaan** in het tekstvak. Er, geef de domeinen van de Azure AD-instanties die u wilt toegang verlenen tot de API's van de service-exemplaar van API Management. U kunt meerdere domeinen met vorm, spaties en komma's scheiden.

> [!NOTE]
> Kunt u meerdere domeinen in de **Tenants toegestaan** sectie. Voordat elke gebruiker kan zich aanmelden vanaf een ander domein dan het oorspronkelijke domein waar de toepassing is geregistreerd, moet een globale beheerder van het andere domein machtiging voor de toepassing toegang heeft tot directory gegevens verlenen. Als u wilt machtigen, de globale beheerder moet: een. Ga naar `https://<URL of your developer portal>/aadadminconsent` (bijvoorbeeld https://contoso.portal.azure-api.net/aadadminconsent).
> b. Typ in het domein van de Azure AD-tenant die ze willen om toegang aan te geven.
> c. Selecteer **Indienen**. 

20.  Nadat u de gewenste configuratie opgeeft, selecteert u **toevoegen**.

Nadat de wijzigingen zijn opgeslagen, worden gebruikers in de opgegeven Azure AD exemplaar kan zich aanmelden bij de portal voor ontwikkelaars met de volgende stappen in [aanmelden bij de portal voor ontwikkelaars met behulp van een Azure AD-account](#log_in_to_dev_portal).

## <a name="add-an-external-azure-ad-group"></a>Toevoegen van een externe Azure AD-groep

Nadat u de toegang voor gebruikers in een Azure AD-exemplaar hebt ingeschakeld, kunt u Azure AD-groepen toevoegen in API Management. Vervolgens kunt u de koppeling van de ontwikkelaars in de groep met de gewenste producten eenvoudiger beheren.

 > [!IMPORTANT]
 > Toevoegen van een externe Azure AD-groep, moet u eerst de Azure AD-exemplaar op de **identiteiten** tabblad via de volgende procedure in de vorige sectie. Bovendien de toepassing, moet toegangsrechten hebben voor Azure AD Graph API met `Directory.Read.All` machtiging. 

Toevoegen van externe Azure AD-groepen van de **groepen** tabblad van uw exemplaar van API Management.

1. Selecteer de tab **Groepen**.
2. Selecteer de **toevoegen AAD-groep** knop.
   ![Knop 'AAD-groep toevoegen'](./media/api-management-howto-aad/api-management-with-aad008.png)
3. Selecteer de groep die u wilt toevoegen.
4. Druk op de **Selecteer** knop.

Nadat u een externe Azure AD hebt toegevoegd groep, kunt u bekijken en de eigenschappen ervan configureren. Selecteer de naam van de groep van de **groepen** tabblad. Hier kunt u **naam** en **beschrijving** gegevens voor de groep.
 
Gebruikers van de geconfigureerde Azure AD-exemplaar nu kan aanmelden bij de portal voor ontwikkelaars. Ze kunnen bekijken en zich abonneren op alle groepen waarvan ze inzicht hebben.

## <a name="a-idlogintodevportalsign-in-to-the-developer-portal-by-using-an-azure-ad-account"></a><a id="log_in_to_dev_portal"/>Aanmelden bij de portal voor ontwikkelaars met behulp van een Azure AD-account

Zich aanmeldt bij de portal voor ontwikkelaars met behulp van een Azure AD-account die u hebt geconfigureerd in de vorige secties:

1. Open een nieuw browservenster geopend met behulp van de aanmelding-URL van de configuratie van de Active Directory-toepassing en selecteer **Azure Active Directory**.

   ![Aanmeldingspagina opgeven][api-management-dev-portal-signin]

1. Voer de referenties van een van de gebruikers in Azure AD en selecteer **aanmelden**.

   ![Aanmelden met gebruikersnaam en wachtwoord][api-management-aad-signin]

1. U mogelijk gevraagd met een registratieformulier als eventuele aanvullende informatie vereist is. Vul het registratieformulier in en selecteer **aanmelden**.

   ![Knop 'Aanmelden' op het registratieformulier][api-management-complete-registration]

De gebruiker is nu aangemeld bij de portal voor ontwikkelaars voor uw API Management service-exemplaar.

![Developer-portal nadat de registratie is voltooid][api-management-registration-complete]

[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

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

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
