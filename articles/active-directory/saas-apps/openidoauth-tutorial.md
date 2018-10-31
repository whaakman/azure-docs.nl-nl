---
title: Een OpenID/OAuth-toepassing van de app-galerie van Azure AD configureren | Microsoft Docs
description: Stappen voor het configureren van een OpenID/OAuth-toepassing uit de galerie met Azure AD-app.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: eedebb76-e78c-428f-9cf0-5891852e79fb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 420ec288b81e0bb12ae9f61a5eaf03880205cc55
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50247977"
---
# <a name="configure-an-openidoauth-application-from-the-azure-ad-app-gallery"></a>Een OpenID/OAuth-toepassing uit de galerie met Azure AD-app configureren

## <a name="process-of-adding-an-openid-application-from-the-gallery"></a>Proces van het toevoegen van een toepassing OpenID uit de galerie

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![Azure Active Directory-knop](./media/openidoauth-tutorial/tutorial_general_01.png)

2. Ga naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![Enterprise applications blade](./media/openidoauth-tutorial/tutorial_general_02.png)

3. Selecteer **nieuwe toepassing** boven aan het dialoogvenster.

    ![Knop nieuwe toepassing](./media/openidoauth-tutorial/tutorial_general_03.png)

4. In het zoekvak, typ de naam van de toepassing. Selecteer de gewenste toepassing vanuit het deelvenster met resultaten en zich registreren voor de toepassing.

    ![Toepassing toevoegen](./media/openidoauth-tutorial/addfromgallery.png)

    > [!NOTE]
    > Voor de OpenID Connect en OAuth-apps, de **toevoegen** knop is standaard uitgeschakeld. Hier selecteert de tenant-beheerder moet de aanmelding knop en geef de toestemming voor de toepassing. De toepassing wordt vervolgens toegevoegd aan de klanttenant, waar u de configuraties kunt doen. Er is niet nodig om toe te voegen van de toepassing expliciet.

    ![Knop Toevoegen](./media/openidoauth-tutorial/addbutton.png)

5. Wanneer u de koppeling voor registratie selecteert, wordt u omgeleid naar de pagina Azure Active Directory (Azure AD) voor de aanmeldingsreferenties.

6. Na een geslaagde authenticatie, gaat u akkoord met de toestemming van de pagina toestemming. Hierna is de startpagina van de toepassing wordt weergegeven.

    > [!NOTE]
    > U kunt slechts één exemplaar van de toepassing kunt toevoegen. Als u hebt al een hebt toegevoegd en toestemming geven om het opnieuw probeert, wordt deze niet opnieuw worden toegevoegd in de tenant. Dus logisch, kunt u slechts één app-exemplaar in de tenant.

## <a name="authentication-flow-using-openid-connect"></a>Stroom voor verificatie met behulp van OpenID Connect

De meest eenvoudige stroom aanmelden bevat de volgende stappen uit:

![Stroom voor verificatie met behulp van OpenID Connect](./media/openidoauth-tutorial/authenticationflow.png)

### <a name="multitenant-application"></a>Multitenant-toepassing 
Een multitenant-toepassing is bedoeld voor gebruik in veel organisaties niet slechts één organisatie. Dit zijn doorgaans software-as-a-service (SaaS)-toepassingen die is geschreven door een onafhankelijke softwareleverancier (ISV). 

Multitenant-toepassingen moeten worden ingericht in elke map waar ze moeten worden gebruikt. Hiervoor gebruiker of beheerder toestemming om u te registreren. Dit proces toestemming wordt gestart wanneer een toepassing in de directory is geregistreerd en toegang tot de Graph API of een andere web-API krijgen wordt. Wanneer een gebruiker of beheerder van een andere organisatie zich registreert voor het gebruik van de toepassing, wordt een dialoogvenster weergegeven met de machtigingen die de toepassing nodig heeft. 

De gebruiker of beheerder kan vervolgens akkoord met de toepassing. De toestemming geeft de toepassing toegang tot de opgegeven gegevens en ten slotte, registreert u de toepassing in de map.

> [!NOTE]
> Als u uw toepassing beschikbaar voor gebruikers in meerdere mappen maken bent, moet u een mechanisme om te bepalen welke tenant waarin ze zijn opgeslagen. Een toepassing met één tenant moet alleen in een eigen map voor een gebruiker te zoeken. Er moet een multitenant-toepassing voor het identificeren van een specifieke gebruiker vanuit alle mappen in Azure AD.
> 
> Als u wilt deze taak wordt uitgevoerd, biedt Azure AD een algemene verificatie-eindpunt waar aanmeldingsaanvragen, in plaats van een tenant-specifieke eindpunt dat een multitenant-toepassing instellen kunt. Dit eindpunt is [ https://login.microsoftonline.com/common ](https://login.microsoftonline.com/common) voor alle mappen in Azure AD. Een tenant-specifieke eindpunt mogelijk [ https://login.microsoftonline.com/contoso.onmicrosoft.com ](https://login.microsoftonline.com/contoso.onmicrosoft.com). 
>
> Het eindpunt van de algemene is het belangrijk om te overwegen wanneer u uw toepassing ontwikkelt. U moet de benodigde logica voor het afhandelen van meerdere tenants tijdens het aanmelden, afmelden, en validatie van tokens.

Standaard wordt Azure AD multitenant-toepassingen. Ze gemakkelijk toegankelijk zijn voor organisaties en ze zijn eenvoudig te gebruiken nadat u de toestemming hebt geaccepteerd.

## <a name="consent-framework"></a>Toestemmingsframework

Het toestemmingsframework Azure AD kunt u meerdere tenants web- en native clienttoepassingen ontwikkelen. Deze toepassingen kunnen aanmelden door gebruikersaccounts van een Azure AD-tenant, dan het adres waar de toepassing is geregistreerd. Ze moeten mogelijk ook toegang krijgen tot web-API's zoals:
- De Microsoft Graph API, toegang hebben tot Azure AD en Intune in Office 365-services. 
- Andere Microsoft-services API's.
- Uw eigen web-API's. 

Het framework is gebaseerd op een gebruiker of beheerder toestemming verlenen tot een toepassing die wordt gevraagd om te worden geregistreerd in de directory. De registratie kan betrekking hebben op toegang tot directorygegevens. Nadat toestemming is opgegeven, kan de clienttoepassing aanroepen van de Microsoft Graph API namens de gebruiker en de gegevens indien nodig kunnen gebruiken.

De [Microsoft Graph API](https://graph.microsoft.io/) biedt toegang tot gegevens in Office 365, zoals:

- Agenda's en berichten van Exchange.
- Sites en -lijsten van SharePoint.
- Als u documenten uit OneDrive.
- Notitieblokken van OneNote.
- Taken van de Planner.
- Vanuit Excel-werkmappen.

De Graph-API biedt ook toegang tot gebruikers en groepen van Azure AD en andere objecten van meer Microsoft-cloudservices.

De volgende stappen laten zien hoe de toestemming ervaren geschikt is voor de ontwikkelaar van toepassingen en de gebruiker:

1. Stel dat u hebt een web-clienttoepassing die nodig zijn om aan te vragen van specifieke machtigingen voor toegang tot een resource of API. De Azure-portal wordt gebruikt om te declareren machtigingsaanvragen tijdens de configuratie. Net als andere configuratie-instellingen worden ze onderdeel van Azure AD-registratie van de toepassing:

    ![Graph API](./media/openidoauth-tutorial/graphapi.png)

2. Houd rekening met dat van uw toepassing machtigingen zijn bijgewerkt. De toepassing wordt uitgevoerd en een gebruiker is gebruikt voor de eerste keer. De toepassing moet eerst een autorisatiecode ophalen van de Azure AD / eindpunt voor autorisatie. De autorisatiecode kan vervolgens worden gebruikt bij het aanschaffen van een nieuwe toegang en vernieuwen van het token.

3. Als de gebruiker niet al is geverifieerd, de Azure AD eindpunt wordt u gevraagd om de aanmelding voor autorisatie.

    ![Verificatie](./media/openidoauth-tutorial/authentication.png)

4. Nadat de gebruiker is aangemeld, wordt in Azure AD bepaalt of de gebruiker moet een instemmingspagina weergegeven. Deze beslissing is gebaseerd op of de gebruiker (of de beheerder van hun organisatie) heeft al verleend de toestemming van de toepassing.

   Als geen toestemming gekregen heeft, wordt Azure AD de gebruiker om toestemming wordt gevraagd en geeft de vereiste machtigingen die nodig is om te werken. De machtigingen die worden weergegeven in het dialoogvenster voor toestemming overeenkomen met de die u hebt geselecteerd in de gedelegeerde machtigingen in Azure portal.

    ![Instemmingspagina](./media/openidoauth-tutorial/consentpage.png)

Een gewone gebruiker kan toestemming geven voor bepaalde machtigingen. Andere machtigingen vereisen van een tenantbeheerder toestemming.

## <a name="difference-between-admin-consent-and-user-consent"></a>Verschil tussen de toestemming van een beheerder en toestemming van de gebruiker

Als beheerder, kunt u ook toestemming voor gedelegeerde machtigingen namens alle gebruikers van een toepassing in uw tenant. Toestemming van een beheerder voorkomt dat het dialoogvenster voor toestemming wordt weergegeven voor elke gebruiker in de tenant. Gebruikers die beschikken over de beheerdersrol kunnen toestemming in Azure portal bieden. Uit de **instellingen** -pagina voor uw toepassing, selecteert **vereiste machtigingen** > **machtigingen verlenen**.

![Knop machtigingen verlenen](./media/openidoauth-tutorial/grantpermission.png)

> [!NOTE]
> Expliciete toestemming verlenen met behulp van de **machtigingen verlenen** knop is nu vereist voor één pagina toepassingen (kuuroorden) die gebruikmaken van ADAL.js. Anders mislukt de toepassing wanneer het toegangstoken wordt aangevraagd.

Altijd een tenantbeheerder toestemming om alleen App-machtigingen. Als uw toepassing een alleen-app-machtigingen worden aangevraagd en een gebruiker wil zich aanmelden bij de toepassing, wordt een foutbericht weergegeven. Het bericht dat de gebruiker kan geen tot toestemming geven.

Als uw toepassing gebruikmaakt van machtigingen voor toestemming van een beheerder, moet u een beweging, zoals een knop of koppeling hebben waar de beheerder de actie kan starten. De aanvraag die uw toepassing voor deze actie verzendt is de gebruikelijke autorisatieaanvraag voor OAuth2/OpenID Connect. Dit verzoek bevat de *prompt = admin_consent* query-tekenreeksparameter. 

Nadat de beheerder heeft ingestemd, en de service-principal is gemaakt in de tenant van de klant, hoger aanmeldingsaanvragen hoeft niet de *prompt = admin_consent* parameter. Omdat de beheerder heeft besloten dat de aangevraagde machtigingen geaccepteerd worden, wordt er geen andere gebruikers in de tenant wordt gevraagd om toestemming vanaf dat moment.

Een tenantbeheerder kan de mogelijkheid voor normale gebruikers instemmen met toepassingen uitschakelen. Als deze mogelijkheid is uitgeschakeld, is toestemming van een beheerder altijd vereist voor de toepassing moet worden gebruikt in de tenant. Als u testen van uw toepassing met eindgebruikers toestemming geven uitgeschakeld wilt, vindt u de configuratie-switch in de [Azure-portal](https://portal.azure.com/). Het is in de [gebruikersinstellingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) sectie onder **bedrijfstoepassingen**.

De *prompt = admin_consent* parameter kan ook worden gebruikt door toepassingen die geen machtigingen voor toestemming van een beheerder vragen. Een voorbeeld is een toepassing waarbij een ervaring waar de tenant-beheerder "zich aanmeldt" tijdstip en wordt er geen andere gebruikers wordt gevraagd om toestemming vanaf dat moment op.

Stel dat een toepassing beheerderstoestemming vereist en een beheerder zich aanmeldt zonder de *prompt = admin_consent* parameter worden verzonden. Als de beheerder is is toestemming voor de toepassing heeft, wordt het geldt alleen voor hun gebruikersaccount. Normale gebruikers nog steeds zich niet aanmelden of toestemming geven voor de toepassing. Deze functie is handig als u de tenantbeheerder bieden de mogelijkheid om te verkennen van uw toepassing wilt voorafgaand aan andere gebruikers toegang toe te staan.