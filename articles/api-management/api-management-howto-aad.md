---
title: Autoriseren ontwikkelaarsaccounts met Azure Active Directory - Azure API Management | Microsoft Docs
description: Informatie over het autoriseren van gebruikers met Azure Active Directory in API Management.
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
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: c9d99d6f7c2777c8e68f5db50b402280377d88e9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Hoe ontwikkelaarsaccounts in Azure API Management met behulp van Azure Active Directory autoriseren

Deze handleiding wordt beschreven hoe u toegang inschakelen voor de portal voor ontwikkelaars voor gebruikers van Azure Active Directory. Deze handleiding ook wordt beschreven hoe u groepen met Azure Active Directory-gebruikers beheren door externe groepen de gebruikers van een Azure Active Directory bevatten toe te voegen.

> [!WARNING]
> Azure Active Directory-integratie is beschikbaar in de [Developer, Standard en Premium](https://azure.microsoft.com/pricing/details/api-management/) alleen lagen.

## <a name="prerequisites"></a>Vereisten

- Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).
- Importeren en publiceren van een exemplaar van API Management. Zie voor meer informatie [importeren en publiceren](import-and-publish.md).

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Hoe ontwikkelaarsaccounts met Azure Active Directory autoriseren

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Selecteer ![pijl](./media/api-management-howto-aad/arrow.png).
3. Typ '-api in het zoekvak.
4. Klik op **API Management-services**.
5. Selecteer uw APIM service-exemplaar.
6. Onder **beveiliging**, selecteer **identiteiten**.

    ![Externe identiteit](./media/api-management-howto-aad/api-management-with-aad001.png)
7. Klik op **+ toevoegen** vanaf de bovenkant.

    De **toevoegen identiteitsprovider** venster wordt weergegeven aan de rechterkant.
8. Onder **providertype**, selecteer **Azure Active Directory**.

    Besturingselementen die het mogelijk dat u andere benodigde gegevens weergegeven in het venster. De besturingselementen omvatten: **Client-ID**, **clientgeheim** (krijgt u informatie later in de zelfstudie).
9. Noteer de **Omleidings-URL**.  
10. Open in uw browser een ander tabblad. 
11. Open [Azure Portal](https://portal.azure.com).
12. Selecteer ![pijl](./media/api-management-howto-aad/arrow.png).
13. Het type 'actief' de **Azure Active Directory** wordt weergegeven.
14. Selecteer **Azure Active Directory**.
15. Onder **beheren**, selecteer **App-registratie**.

    ![App-registratie](./media/api-management-howto-aad/api-management-with-aad002.png)
16. Klik op **Nieuwe toepassing registreren**.

    De **maken** venster wordt weergegeven aan de rechterkant. Dat is waar u de AAD-app relavent informatie invoeren.
17. Eneter een naam voor de toepassing.
18. Selecteer voor het toepassingstype **Web-app/API**.
19. Voer de aanmeldings-URL van uw ontwikkelaarsportal voor eenmalige aanmelding URL. In dit voorbeeld wordt de URL van de aanmelding is: https://apimwithaad.portal.azure-api.net/signin.
20. Klik op **maken** om de toepassing te maken.
21. Als u uw app zoekt, selecteert u **App registraties** en zoeken op naam.

    ![App-registratie](./media/api-management-howto-aad/find-your-app.png)
22. Nadat de toepassing is geregistreerd, gaat u naar **antwoord-URL** en zorg ervoor dat de 'Omleidings-URL' is ingesteld op de waarde die u hebt verkregen in stap 9. 
23. Als u wilt uw toepassing configureren (bijvoorbeeld wijzigen **App-ID-URL**) Selecteer **eigenschappen**.

    ![App-registratie](./media/api-management-howto-aad/api-management-with-aad004.png)

    Als meerdere Azure Active Directory's worden gebruikt voor deze toepassing gaat, klikt u op **Ja** voor **toepassing is multitenant**. De standaardwaarde is **Nee**.
24. Toepassing worden machtigingen ingesteld door het selecteren van **vereist machtigingen**.
25. Selecteer de uw toepassing en Vink **mapgegevens lezen** en **aanmelden en gebruikersprofiel lezen**.

    ![App-registratie](./media/api-management-howto-aad/api-management-with-aad005.png)

    Zie voor meer informatie over de toepassing en gedelegeerde machtigingen, [toegang tot de Graph API][Accessing the Graph API].
26. Kopieer in het linkerdeelvenster de **toepassings-ID** waarde.

    ![App-registratie](./media/api-management-howto-aad/application-id.png)
27. Ga terug naar uw API Management-toepassing. De **toevoegen identiteitsprovider** venster moet worden weergegeven. <br/>Plak de **toepassings-ID** waarde in de **Client-Id** vak.
28. Ga terug naar de Azure Active Directory-configuratie en klikt u op **sleutels**.
29. Maak een nieuwe sleutel door speicifying een naam en duur. 
30. Klik op **Opslaan**. De sleutel wordt gegenereerd.

    De sleutel naar het Klembord kopiëren.

    ![App-registratie](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Noteer deze sleutel. Wanneer u het venster Azure Active Directory-configuratie hebt gesloten, wordt de sleutel kan niet opnieuw weergegeven.
    > 
    > 
31. Ga terug naar uw API Management-toepassing. <br/>In de **toevoegen identiteitsprovider** venster plakt u de sleutel in de **clientgeheim** in het tekstvak.
32. De **toevoegen identiteitsprovider** venster bevat de **Tenants toegestaan** tekstvak in u opgeven welke mappen toegang hebben tot de API's van het exemplaar van API Management-service. <br/>Geef de domeinen van de Azure Active Directory-exemplaren waartoe u toegang wilt verlenen. U kunt meerdere domeinen scheiden met nieuwe regels, spaties of komma's.

    Meerdere domeinen kunnen worden opgegeven in de **Tenants toegestaan** sectie. Voordat een gebruiker zich vanaf een ander domein dan het oorspronkelijke domein waar de toepassing is geregistreerd aanmelden kan, moet een globale beheerder van het andere domein machtiging voor de toepassing directorygegevens toegang verlenen. Als u wilt machtigen, de globale beheerder moet gaat u naar `https://<URL of your developer portal>/aadadminconsent` (bijvoorbeeld https://contoso.portal.azure-api.net/aadadminconsent), typt u de domeinnaam van de Active Directory-tenant die ze willen krijgen en klik op verzenden. In het volgende voorbeeld wordt een globale beheerder van `miaoaad.onmicrosoft.com` wilt machtigen voor deze specifieke developer-portal. 

33. Nadat de gewenste configuratie wordt opgegeven, klikt u op **toevoegen**.

    ![App-registratie](./media/api-management-howto-aad/api-management-with-aad007.png)

Wanneer de wijzigingen zijn opgeslagen, de gebruikers in de opgegeven Azure Active Directory kunnen aanmelden bij de portal voor ontwikkelaars door de stappen in [aanmelden bij de portal voor ontwikkelaars met een Azure Active Directory-account](#log_in_to_dev_portal).

![Machtigingen](./media/api-management-howto-aad/api-management-aad-consent.png)

In het volgende scherm wordt de globale beheerder wordt gevraagd om te bevestigen dat de toestemming geven. 

![Machtigingen](./media/api-management-howto-aad/api-management-permissions-form.png)

Als een niet-globale beheerder probeert aan te melden voordat een globale beheerder machtigingen worden verleend, wordt de aanmeldingspoging mislukt en wordt een fout wordt weergegeven.

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Het toevoegen van een externe Azure Active Directory-groep

Nadat de toegang voor gebruikers in een Azure Active Directory is ingeschakeld, kunt u Azure Active Directory-groepen toevoegen in API Management koppeling van de ontwikkelaars in de groep met de gewenste producten eenvoudiger te beheren.

Voor het configureren van een externe Azure Active Directory-groep moet de Azure Active Directory eerst worden geconfigureerd op het tabblad identiteiten via de volgende procedure in de vorige sectie. 

Externe Azure Active Directory-groepen worden toegevoegd vanuit de **groepen** tabblad van uw exemplaar van API Management.

![Groepen](./media/api-management-howto-aad/api-management-with-aad008.png)

1. Selecteer de tab **Groepen**.
2. Klik op de **toevoegen AAD-groep** knop.
3. Selecteer de groep die u wilt toevoegen.
4. Druk op **Selecteer** knop.

Zodra een Azure Active Directory-groep is gemaakt, kunt u bekijken en configureren van de eigenschappen voor externe groepen zodra ze zijn toegevoegd, klikt u op de naam van de groep uit de **groepen** tabblad.

Hier kunt u de **naam** en de **beschrijving** van de groep.
 
Gebruikers van de geconfigureerde Azure Active Directory kunnen aanmelden bij de portal voor ontwikkelaars en de weergave en zich abonneren op alle groepen waarvan ze inzicht hebben door de instructies in de volgende sectie.

## <a name="a-idlogintodevportalhow-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a><a id="log_in_to_dev_portal"/>Hoe u zich aanmelden bij de portal voor ontwikkelaars met een Azure Active Directory-account

Als u wilt zich aanmelden bij de portal voor ontwikkelaars met een Azure Active Directory-account geconfigureerd in de vorige secties, open een nieuw browser venster met de **aanmeldings-URL** in de configuratie van Active Directory-toepassing en klik op **Azure Active Directory**.

![Portal voor ontwikkelaars][api-management-dev-portal-signin]

Voer de referenties van een van de gebruikers in uw Azure Active Directory en klikt u op **aanmelden**.

![Aanmelden][api-management-aad-signin]

U wordt mogelijk gevraagd met een registratieformulier als eventuele aanvullende informatie vereist is. Vul het registratieformulier en klik op **aanmelden**.

![Registratie][api-management-complete-registration]

De gebruiker is nu aangemeld bij de portal voor ontwikkelaars voor uw API Management-service-exemplaar.

![Registratie is voltooid][api-management-registration-complete]

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

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
