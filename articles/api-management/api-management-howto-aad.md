---
title: Ontwikkelaarsaccounts autoriseren met behulp van Azure Active Directory - Azure API Management | Microsoft Docs
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
ms.openlocfilehash: 9f3669d205ab4bd24ccba53ffb532fe1d88131ac
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="authorize-developer-accounts-by-using-azure-active-directory-in-azure-api-management"></a>Ontwikkelaarsaccounts autoriseren met behulp van Azure Active Directory in Azure API Management

In dit artikel laat zien hoe toegang inschakelen voor de portal voor ontwikkelaars voor gebruikers van Azure Active Directory (Azure AD). Deze handleiding ook wordt beschreven hoe u groepen van Azure AD-gebruikers beheren door externe groepen de gebruikers bevatten toe te voegen.

> [!NOTE]
> Azure AD-integratie is beschikbaar in de [Developer, Standard en Premium](https://azure.microsoft.com/pricing/details/api-management/) alleen lagen.

## <a name="prerequisites"></a>Vereisten

- Lees de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
- Importeren en publiceren van een exemplaar van Azure API Management. Zie voor meer informatie [importeren en publiceren](import-and-publish.md).

## <a name="authorize-developer-accounts-by-using-azure-ad"></a>Ontwikkelaarsaccounts autoriseren met Azure AD

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Selecteer ![pijl](./media/api-management-howto-aad/arrow.png).
3. Type **api** in het zoekvak.
4. Selecteer **API Management-services**.
5. Selecteer uw API Management-service-exemplaar.
6. Onder **beveiliging**, selecteer **identiteiten**.

7. Selecteer **+ toevoegen** vanaf de bovenkant.

    De **toevoegen identiteitsprovider** deelvenster wordt weergegeven aan de rechterkant.
8. Onder **providertype**, selecteer **Azure Active Directory**.

    Besturingselementen die het mogelijk dat u andere benodigde gegevens weergegeven in het deelvenster. De besturingselementen omvatten **Client-ID** en **clientgeheim**. (U informatie ophalen over deze beveiligingsmaatregelen later in dit artikel.)
9. Noteer de inhoud van **Omleidings-URL**.
    
   ![Stappen voor het toevoegen van een id-provider in de Azure portal](./media/api-management-howto-aad/api-management-with-aad001.png)  
10. Open in uw browser een ander tabblad. 
11. Ga naar de [Azure Portal](https://portal.azure.com).
12. Selecteer ![pijl](./media/api-management-howto-aad/arrow.png).
13. Type **active**. De **Azure Active Directory** deelvenster wordt weergegeven.
14. Selecteer **Azure Active Directory**.
15. Onder **beheren**, selecteer **App registraties**.
16. Selecteer **Nieuwe toepassing registreren**.

    ![De selecties voor het maken van een nieuwe app-registratie](./media/api-management-howto-aad/api-management-with-aad002.png)

    De **maken** deelvenster wordt weergegeven aan de rechterkant. Dat is waar u de Azure AD app relevante informatie invoeren.
17. Voer een naam voor de toepassing.
18. Selecteer voor het toepassingstype **Web-app/API**.
19. Voer de URL aanmeldingspagina van uw ontwikkelaarsportal voor de URL aanmelden. In dit voorbeeld wordt de URL van de aanmeldingspagina is https://apimwithaad.portal.azure-api.net/signin.
20. Selecteer **maken** om de toepassing te maken.
21. Als u uw app zoekt, selecteert u **App registraties** en zoeken op naam.

    ![Vak waar u naar een app zoeken](./media/api-management-howto-aad/find-your-app.png)
22. Nadat de toepassing is geregistreerd, gaat u naar **antwoord-URL** en zorg ervoor dat **Omleidings-URL** is ingesteld op de waarde die u hebt verkregen in stap 9. 
23. Als u wilt uw toepassing configureren (bijvoorbeeld wijzigen **App-ID-URL**), selecteer **eigenschappen**.

    ![Het deelvenster 'Eigenschappen' te openen](./media/api-management-howto-aad/api-management-with-aad004.png)

    Als meerdere exemplaren van Azure AD wordt gebruikt voor deze toepassing, selecteert u **Ja** voor **Multi-verpachte**. De standaardwaarde is **Nee**.
24. Toepassing worden machtigingen ingesteld door het selecteren van **vereist machtigingen**.
25. Selecteer uw toepassing en selecteer vervolgens de **mapgegevens lezen** en **aanmelden en gebruikersprofiel lezen** selectievakjes uit.

    ![Selectievakjes voor machtigingen](./media/api-management-howto-aad/api-management-with-aad005.png)

    Zie voor meer informatie over de toepassing en gedelegeerde machtigingen, [toegang tot de Graph API][Accessing the Graph API].
26. Kopieer in het linkerdeelvenster de **toepassings-ID** waarde.

    ![De waarde 'toepassings-ID](./media/api-management-howto-aad/application-id.png)
27. Ga terug naar uw API Management-toepassing. 

    In de **toevoegen identiteitsprovider** venster plakken de **toepassings-ID** waarde in de **Client-ID** vak.
28. Ga terug naar de configuratie van Azure AD en selecteer **sleutels**.
29. Maak een nieuwe sleutel door te geven van een naam en duur. 
30. Selecteer **Opslaan**. De sleutel wordt gegenereerd.

    De sleutel naar het Klembord kopiëren.

    ![De selecties voor het maken van een sleutel](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Noteer deze sleutel. Nadat u het deelvenster Azure AD-configuratie sluit, wordt de sleutel kan niet opnieuw weergegeven.
    > 
    > 
31. Ga terug naar uw API Management-toepassing. 

    In de **toevoegen identiteitsprovider** venster plakt u de sleutel in de **clientgeheim** in het tekstvak.
32. De **toevoegen identiteitsprovider** venster bevat ook de **Tenants toegestaan** in het tekstvak. Er, geef de domeinen van de Azure AD-exemplaren die u wilt toegang verlenen tot de API's van het exemplaar van API Management-service. U kunt meerdere domeinen scheiden met nieuwe regels, spaties of komma's.

    U kunt opgeven dat meerdere domeinen in de **Tenants toegestaan** sectie. Voordat een gebruiker met een ander domein dan het oorspronkelijke domein waar de toepassing is geregistreerd aanmelden kunt, moet een globale beheerder van het andere domein machtiging voor de toepassing directorygegevens toegang verlenen. Als u wilt machtigen, moet de globale beheerder van het volgende doen:
    
    a. Ga naar `https://<URL of your developer portal>/aadadminconsent` (bijvoorbeeld https://contoso.portal.azure-api.net/aadadminconsent).
    
    b. Typ in het domein van de Azure AD-tenant die ze willen krijgen.
    
    c. Selecteer **indienen**. 
    
    In het volgende voorbeeld wordt probeert een globale beheerder van miaoaad.onmicrosoft.com te machtigen voor deze specifieke developer-portal. 

33. Nadat u de gewenste configuratie opgeeft, selecteert u **toevoegen**.

    !['Add' knop in deelvenster 'Identiteitsprovider toevoegen'](./media/api-management-howto-aad/api-management-with-aad007.png)

Nadat de wijzigingen zijn opgeslagen, worden gebruikers in de opgegeven Azure AD exemplaar kan aanmelden bij de portal voor ontwikkelaars door de stappen in [aanmelden bij de portal voor ontwikkelaars met behulp van een Azure AD-account](#log_in_to_dev_portal).

![De naam van een Azure AD-tenant invoeren](./media/api-management-howto-aad/api-management-aad-consent.png)

De globale beheerder is op het volgende scherm gevraagd te bevestigen geeft de toestemming. 

![Bevestiging van het toekennen van machtigingen](./media/api-management-howto-aad/api-management-permissions-form.png)

Als een niet-globale beheerder probeert aan te melden voordat een globale beheerder machtigingen verleent, de aanmeldingspoging mislukt en een fout wordt weergegeven.

## <a name="add-an-external-azure-ad-group"></a>Toevoegen van een externe Azure AD-groep

Nadat u de toegang voor gebruikers in een Azure AD-exemplaar inschakelt, kunt u Azure AD-groepen toevoegen in API Management. Vervolgens kunt u de koppeling van de ontwikkelaars in de groep met de gewenste producten eenvoudiger beheren.

Voor het configureren van een externe Azure AD-groep, moet u eerst de Azure AD-exemplaar op de **identiteiten** tabblad via de volgende procedure in de vorige sectie. 

Toevoegen van externe Azure AD-groepen van de **groepen** tabblad van uw exemplaar van API Management.

1. Selecteer de tab **Groepen**.
2. Selecteer de **toevoegen AAD-groep** knop.
   ![Knop 'AAD-groep toevoegen'](./media/api-management-howto-aad/api-management-with-aad008.png)
3. Selecteer de groep die u wilt toevoegen.
4. Druk op de **Selecteer** knop.

Na het toevoegen van een externe Azure AD kunt u bekijken en configureer de eigenschappen van groep. Selecteer de naam van de groep uit de **groepen** tabblad. Hier kunt u kunt bewerken **naam** en **beschrijving** gegevens voor de groep.
 
Gebruikers van de geconfigureerde Azure AD-exemplaar nu kan aanmelden bij de portal voor ontwikkelaars. Ze kunnen bekijken en zich abonneren op alle groepen waarvan ze inzicht hebben.

## <a name="a-idlogintodevportalsign-in-to-the-developer-portal-by-using-an-azure-ad-account"></a><a id="log_in_to_dev_portal"/>Aanmelden bij de portal voor ontwikkelaars met behulp van een Azure AD-account

Aan te melden bij de portal voor ontwikkelaars met een Azure AD-account die u hebt geconfigureerd in de vorige secties:

1. Open een nieuw browservenster met de URL aanmelden van de configuratie van de Active Directory-toepassing en selecteer **Azure Active Directory**.

   ![Aanmeldingspagina][api-management-dev-portal-signin]

2. Voer de referenties van een van de gebruikers in Azure AD en selecteer **aanmelden**.

   ![Aanmelden met gebruikersnaam en wachtwoord][api-management-aad-signin]

3. Mogelijk moet u met een registratieformulier als eventuele aanvullende informatie vereist is. Vul het registratieformulier en selecteer **aanmelden**.

   ![Knop 'Aanmelden' op registratieformulier][api-management-complete-registration]

De gebruiker is nu aangemeld bij de portal voor ontwikkelaars voor uw API Management-service-exemplaar.

![Portal voor ontwikkelaars nadat de registratie is voltooid][api-management-registration-complete]

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
