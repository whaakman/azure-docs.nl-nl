---
title: Aan de slag met Azure AD-verificatie met behulp van de Azure portal | Microsoft Docs
description: Informatie over het gebruiken van de Azure-portal voor toegang tot Azure Active Directory (Azure AD)-verificatie voor het verbruik van de API van Azure Media Services.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 829e0759f8aeb8758f6b8895b88b60b66ffb22ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-the-azure-portal"></a>Aan de slag met Azure AD-verificatie met behulp van de Azure-portal

Informatie over het gebruiken van de Azure-portal voor toegang tot Azure Active Directory (Azure AD)-verificatie voor toegang tot de API van Azure Media Services.

## <a name="prerequisites"></a>Vereisten

- Een Azure-account. Als u geen account hebt, beginnen met een [gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Een Media Services-account. Zie voor meer informatie [een Azure Media Services-account maken met behulp van de Azure-portal](media-services-portal-create-account.md).
- Zorg ervoor dat u de [toegang tot Azure Media Services-API met Azure AD authentication overzicht](media-services-use-aad-auth-to-access-ams-api.md). 

Wanneer u Azure AD-verificatie met Azure Media Services gebruikt, hebt u twee opties voor verificatie:

- **Gebruikersverificatie**. Verifiëren van een persoon die de app wordt gebruikt om te communiceren met Media Services-resources. De interactieve toepassing moet eerst de gebruiker om referenties gevraagd. Een voorbeeld is een management console-app gebruikt door gemachtigde gebruikers codering taken bewaken of live streamen. 
- **Verificatie van de service-principal**. Een service verifiëren. Toepassingen die gebruikmaken van deze verificatiemethode vaak zijn apps die daemon-services, middelste laag services of geplande taken uitvoeren: web-apps, apps van de functie, logische apps, API's of een microservice.

> [!IMPORTANT]
> Media Services ondersteunt momenteel, het model van Azure Access Control service-verificatie. Access Control-autorisatie wordt echter op 1 juni 2018 afgeschaft. Het is raadzaam om te migreren naar het model van Azure AD authentication zo snel mogelijk.

## <a name="select-the-authentication-method"></a>Selecteer de verificatiemethode

1. In de [Azure-portal](https://portal.azure.com/), selecteert u uw Media Services-account.
2. Selecteer hoe u verbinding maken met het Media Services-API.

    ![Selecteer op de pagina methode verbinding](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a>Gebruikersverificatie

Voor verbinding met het Media Services-API met behulp van de optie voor verificatie, moet de client-app voor het aanvragen van een Azure AD-token met de volgende parameters:  

* Azure AD-tenant-eindpunt
* Media Services resource-URI
* Media Services (systeemeigen) toepassing client-ID 
* Media Services (systeemeigen)-toepassing omleidings-URI 
* Resource-URI voor de REST mediaservices

U kunt de waarden ophalen voor deze parameters op de **Media Services-API met gebruikersverificatie** pagina. 

![Verbinding maken met verificatie op de gebruikerspagina](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

Als u verbinding met het Media Services-API met behulp van de Microsoft van Media Services .NET SDK, is de vereiste waarden zijn beschikbaar voor u als onderdeel van de SDK. Zie voor meer informatie [gebruik Azure AD-verificatie voor toegang tot de API van Azure Media Services met .NET](media-services-dotnet-get-started-with-aad.md).

Als u geen van de client-Media Services .NET SDK gebruikmaakt, moet u handmatig een Azure AD-tokenaanvraag maken met behulp van de parameters die eerder is besproken. Zie voor meer informatie [hoe de Azure AD-token ophalen met de Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="service-principal-authentication"></a>Verificatie van service-principal

Voor verbinding met het Media Services-API met behulp van de service principal-optie, moet uw app middelste laag (web-API of web-App) om aan te vragen van een Azure AD-token met de volgende parameters:  

* Azure AD-tenant-eindpunt
* Media Services resource-URI 
* Resource-URI voor de REST mediaservices
* Waarden van Azure AD-toepassing: de **client-ID** en **clientgeheim**

U kunt de waarden ophalen voor deze parameters op de **verbinding maken met Media Services-API met service-principal** pagina. Gebruik deze pagina om een nieuwe Azure AD-toepassing of een bestaande set selecteren. Nadat u de Azure AD-app hebt geselecteerd, kunt u ophalen van de client-ID (toepassings-ID) en de waarden van client geheim (sleutel) gegenereerd. 

![Verbinding maken met de pagina van de service-principal](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

Wanneer de **Service-Principal** blade wordt geopend, wordt de eerste Azure AD-toepassing die voldoet aan de volgende criteria geselecteerd:

- Het is een geregistreerde Azure AD-toepassing.
- Het heeft Inzender of Owner Role-Based Access Control-machtigingen op het account.

Nadat u maakt of een Azure AD-app selecteert, kunt u maken en een clientgeheim (sleutel) en de client-ID (toepassings-ID) kopiëren. Het clientgeheim en de client-ID moet het toegangstoken ophalen in dit scenario.

Als u geen machtigingen voor het maken van Azure AD-apps in uw domein, de Azure AD-app-besturingselementen van de blade worden niet weergegeven en een waarschuwing wordt weergegeven.

Als u verbinding met het Media Services-API met behulp van de Media Services .NET SDK, Zie [gebruik Azure AD-verificatie voor toegang tot de API van Azure Media Services met .NET](media-services-dotnet-get-started-with-aad.md).

Als u geen van de client-Media Services .NET SDK gebruikmaakt, moet u handmatig een tokenaanvraag van Azure AD met de parameters die eerder is besproken. Zie voor meer informatie [hoe de Azure AD-token ophalen met de Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="get-the-client-id-and-client-secret"></a>De client-ID en clientgeheim ophalen

Nadat u een bestaande Azure AD-app selecteren of de optie voor het maken van een nieuwe, wordt de volgende knoppen weergegeven:

![Knop machtigingen en beheren toepassing knop beheren](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

Open de blade van Azure AD-toepassing, te klikken op **beheren toepassing**. Op de **beheren toepassing** blade, kun je de app client-ID (toepassings-ID). Als een clientgeheim (sleutel) gegenereerd, selecteer **sleutels**.

![Optie voor toepassing blade sleutels beheren](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-the-application"></a>Machtigingen en de toepassing beheren

Nadat u de Azure AD-toepassing hebt geselecteerd, kunt u de toepassing en machtigingen beheren. Als u uw Azure AD-toepassing toegang krijgen tot andere toepassingen instelt, klikt u op **machtigingen beheren**. Voor beheertaken, zoals het wijzigen van sleutels en antwoord-URL's of bewerken van het toepassingsmanifest, klikt u op **beheren toepassing**.

### <a name="edit-the-apps-settings-or-manifest"></a>De app-instellingen bewerken of manifest

De instellingen van de app of het manifest bewerken, klikt u op **beheren toepassing**.

![Toepassingspagina beheren](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a>Volgende stappen

Aan de slag met [bestanden uploaden naar uw account](media-services-portal-upload-files.md).
