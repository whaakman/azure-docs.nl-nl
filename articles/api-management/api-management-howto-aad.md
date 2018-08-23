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
ms.openlocfilehash: d4eb8f3e805d96a276851052b74cac90465d1185
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42056871"
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Ontwikkelaarsaccounts authoriseren met behulp van Azure Active Directory in Azure API Management

Dit artikel leest u hoe de developer-Portal voor gebruikers om toegang te krijgen van Azure Active Directory (Azure AD). Deze handleiding leest u ook hoe voor het beheren van groepen van Azure AD-gebruikers door externe groepen met de gebruikers toe te voegen.

> [!NOTE]
> Azure AD-integratie is beschikbaar in de [Developer, Standard en Premium](https://azure.microsoft.com/pricing/details/api-management/) alleen lagen.

## <a name="prerequisites"></a>Vereisten

- Lees de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
- Importeren en publiceren van een Azure API Management-exemplaar. Zie voor meer informatie, [importeren en publiceren](import-and-publish.md).

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Ontwikkelaarsaccounts authoriseren met behulp van Azure AD

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
1. Selecteer ![pijl](./media/api-management-howto-aad/arrow.png).
1. Type **api** in het zoekvak in.
1. Selecteer **API Management-services**.
1. Selecteer uw service-exemplaar van API Management.
1. Onder **SECURITY**, selecteer **identiteiten**.

1. Selecteer **+ toevoegen** vanaf de bovenkant.

    De **id-provider toevoegen** deelvenster wordt weergegeven aan de rechterkant.
1. Onder **providertype**, selecteer **Azure Active Directory**.

    Besturingselementen waarmee u kunt andere benodigde informatie op te geven in het deelvenster worden weergegeven. De besturingselementen zijn **Client-ID** en **clientgeheim**. (U krijgt informatie over deze besturingselementen later in dit artikel.)
1. Maak een notitie van de inhoud van **Omleidings-URL**.
    
   ![Stappen voor het toevoegen van een id-provider in Azure portal](./media/api-management-howto-aad/api-management-with-aad001.png)  
1. Open een ander tabblad in uw browser. 
1. Ga naar de [Azure Portal](https://portal.azure.com).
1. Selecteer ![pijl](./media/api-management-howto-aad/arrow.png).
1. Type **active**. De **Azure Active Directory** deelvenster wordt weergegeven.
1. Selecteer **Azure Active Directory**.
1. Onder **beheren**, selecteer **App-registraties**.
1. Selecteer **Nieuwe toepassing registreren**.

    ![Selecties voor het maken van een nieuwe app-registratie](./media/api-management-howto-aad/api-management-with-aad002.png)

    De **maken** deelvenster wordt weergegeven aan de rechterkant. Dat is waar u de Azure AD app relevante informatie invoeren.
1. Voer een naam voor de toepassing.
1. Selecteer voor het toepassingstype **Web-app/API**.
1. Voer de URL aanmelden van uw developer-portal voor de URL aanmelden. In dit voorbeeld wordt de URL van de aanmelding is `https://apimwithaad.portal.azure-api.net/signin`.
1. Selecteer **maken** om de toepassing te maken.
1. Selecteer uw app zoekt, **App-registraties** en zoeken op naam.

    ![Het vak waarin u voor een app zoeken](./media/api-management-howto-aad/find-your-app.png)
1. Nadat de toepassing is geregistreerd, gaat u naar **antwoord-URL** en zorg ervoor dat **Omleidings-URL** is ingesteld op de waarde die u hebt verkregen in stap 9. 
1. Als u wilt dat uw toepassing configureren (bijvoorbeeld wijzigen **URL van App-ID**), selecteer **eigenschappen**.

    ![Het deelvenster 'Eigenschappen' te openen](./media/api-management-howto-aad/api-management-with-aad004.png)

    Als meerdere exemplaren van Azure AD wordt gebruikt voor deze toepassing, selecteert u **Ja** voor **multitenant**. De standaardwaarde is **Nee**.
1. Stel de machtigingen van de toepassing door te selecteren **vereiste machtigingen**.
1. Selecteer uw toepassing en selecteer vervolgens de **mapgegevens lezen** en **aanmelden en gebruikersprofiel lezen** selectievakjes.

    ![Selectievakjes voor machtigingen](./media/api-management-howto-aad/api-management-with-aad005.png)

    Zie voor meer informatie over de machtigingen van de toepassing en gedelegeerde machtigingen [toegang tot de Graph API][Accessing the Graph API].
1. Kopieer in het linkerdeelvenster de **toepassings-ID** waarde.

    !["Toepassings-ID-waarde](./media/api-management-howto-aad/application-id.png)
1. Ga terug naar uw API Management-toepassing. 

    In de **id-provider toevoegen** venster, plak de **toepassings-ID** waarde in de **Client-ID** vak.
1. Ga terug naar de Azure AD-configuratie en selecteer **sleutels**.
1. Maak een nieuwe sleutel met een naam en de duur op te geven. 
1. Selecteer **Opslaan**. De sleutel wordt gegenereerd.

    De sleutel naar het Klembord kopiëren.

    ![Selecties voor het maken van een sleutel](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Maak een notitie van deze sleutel. Nadat u het deelvenster van de configuratie van Azure AD hebt gesloten, wordt de sleutel niet opnieuw weergeven.
    > 
    > 
1. Ga terug naar uw API Management-toepassing. 

    In de **id-provider toevoegen** venster, plak de sleutel in de **clientgeheim** in het tekstvak.

    > [!IMPORTANT]
    > Zorg ervoor dat u het bijwerken van de **clientgeheim** voordat de sleutel is verlopen. 
    >  
    >
1. De **id-provider toevoegen** venster bevat ook de **Tenants toegestaan** in het tekstvak. Er, geef de domeinen van de Azure AD-instanties die u wilt toegang verlenen tot de API's van de service-exemplaar van API Management. U kunt meerdere domeinen met vorm, spaties en komma's scheiden.

    Kunt u meerdere domeinen in de **Tenants toegestaan** sectie. Voordat elke gebruiker kan zich aanmelden vanaf een ander domein dan het oorspronkelijke domein waar de toepassing is geregistreerd, moet een globale beheerder van het andere domein machtiging voor de toepassing toegang heeft tot directory gegevens verlenen. Als u wilt machtigen, moet u de globale beheerder:
    
    a. Ga naar `https://<URL of your developer portal>/aadadminconsent` (bijvoorbeeld https://contoso.portal.azure-api.net/aadadminconsent).
    
    b. Typ in het domein van de Azure AD-tenant die ze willen om toegang aan te geven.
    
    c. Selecteer **indienen**. 
    
    In het volgende voorbeeld wordt probeert een globale beheerder van miaoaad.onmicrosoft.com te machtigen voor deze specifieke developer-portal. 

1. Nadat u de gewenste configuratie opgeeft, selecteert u **toevoegen**.

    !["Toevoegen" knop in het deelvenster 'Id-provider toevoegen'](./media/api-management-howto-aad/api-management-with-aad007.png)

Nadat de wijzigingen zijn opgeslagen, worden gebruikers in de opgegeven Azure AD exemplaar kan zich aanmelden bij de portal voor ontwikkelaars met de volgende stappen in [aanmelden bij de portal voor ontwikkelaars met behulp van een Azure AD-account](#log_in_to_dev_portal).

![De naam van een Azure AD-tenant](./media/api-management-howto-aad/api-management-aad-consent.png)

De globale beheerder is op het volgende scherm gevraagd te bevestigen de toestemming geven. 

![Bevestiging van het toewijzen van machtigingen](./media/api-management-howto-aad/api-management-permissions-form.png)

Als een niet-globale beheerder wil zich aanmelden voordat u een globale beheerder machtigingen verleent, de aanmeldingspoging is mislukt en een foutbericht wordt weergegeven.

## <a name="add-an-external-azure-ad-group"></a>Toevoegen van een externe Azure AD-groep

Nadat u de toegang voor gebruikers in een Azure AD-exemplaar hebt ingeschakeld, kunt u Azure AD-groepen toevoegen in API Management. Vervolgens kunt u de koppeling van de ontwikkelaars in de groep met de gewenste producten eenvoudiger beheren.

Het configureren van een externe Azure AD-groep, moet u eerst de Azure AD-exemplaar op de **identiteiten** tabblad via de volgende procedure in de vorige sectie. 

Toevoegen van externe Azure AD-groepen van de **groepen** tabblad van uw exemplaar van API Management.

1. Selecteer de tab **Groepen**.
1. Selecteer de **toevoegen AAD-groep** knop.
   ![Knop 'AAD-groep toevoegen'](./media/api-management-howto-aad/api-management-with-aad008.png)
1. Selecteer de groep die u wilt toevoegen.
1. Druk op de **Selecteer** knop.

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

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Sign in to the developer portal by using an Azure AD account]: #Sign-in-to-the-developer-portal-by-using-an-Azure-AD-account
