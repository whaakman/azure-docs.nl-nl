---
title: Een Video Indexer-account dat is verbonden met Azure maken | Microsoft Docs
description: Dit artikel leest over het maken van een Video Indexer-account met Azure bent verbonden.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 09/05/2018
ms.author: juliako
ms.openlocfilehash: c598fdae40b4552e1d4dc29b8558d82d0830160a
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43841830"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Maken van een Video Indexer-account dat is verbonden met Azure

Als u een Video Indexer-account maakt, kunt u een gratis proefaccount (waar u aan een bepaald aantal gratis indexering minuten) of een betaalde optie (waarbij u bent niet beperkt door het quotum). Met gratis proefversie Video Indexer biedt maximaal 600 minuten gratis worden geïndexeerd voor gebruikers van de website en maximaal 2400 minuten gratis indexing voor een API-gebruikers. Met de betaalde optie maakt u een Video Indexer-account dat is verbonden met uw Azure-abonnement en een Azure Media Services-account. U betaalt voor minuten geïndexeerd, evenals de Media-Account gekoppeld kosten in rekening gebracht. 

In dit artikel laat zien hoe een Video Indexer-account dat gekoppeld aan een Azure-abonnement en een Azure Media Services-account maken. 

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. 

    Als u een Azure-abonnement nog geen hebt, kunt u zich aanmelden voor [gratis proefversie van Azure](https://azure.microsoft.com/free/).

* Een Azure Active Directory (AD)-domein. 

    Als u geen Azure AD-domein, moet u dit domein maken met uw Azure-abonnement. Zie voor meer informatie, [aangepaste domeinnamen in uw Azure Active Directory beheren](../../active-directory/users-groups-roles/domains-manage.md)

* Een gebruiker en het lid in uw Azure AD-domein. U gebruikt dit lid bij het verbinden van uw Video Indexer-account met Azure.

    Deze gebruiker moet voldoen aan deze criteria:

    * Een Azure AD-gebruiker met een werk- of schoolaccount, niet een persoonlijk account, zoals outlook.com, live.com of hotmail.com worden.
        
        ![alle AAD-gebruikers](./media/create-account/all-aad-users.png)

    *  Een lid in uw Azure-abonnement met de rol van een eigenaar of Inzender-en beheerder van gebruikerstoegang worden. Een gebruiker kan twee keer worden toegevoegd met behulp van 2. Eenmaal met Inzender en anderzijds met Access-beheerder.

        ![Toegangsbeheer](./media/create-account/access-control-iam.png)

* Registreer de resourceprovider voor EventGrid met behulp van de Azure portal.

    In de [Azure-portal](https://portal.azure.com/), gaat u naar **abonnementen** > [abonnement] > **resourceproviders en** > **Microsoft.EventGrid**. Als u niet in de status 'Registered', klikt u op **registreren**. Het duurt enkele minuten om te registreren. 

    ![EventGrid](./media/create-account/event-grid.png)

## <a name="connect-to-azure"></a>Verbinding maken met Azure

1. Aanmelden bij [ https://www.videoindexer.ai/ ](https://www.videoindexer.ai/) en klik op de **verbinding maken met Azure** knop:

    ![Verbinding maken met Azure](./media/create-account/connect-to-azure.png)

2. Wanneer de abonnementenlijst met wordt weergegeven, selecteert u het abonnement dat u wilt gebruiken. 

    ![Video Indexer verbinden met Azure](./media/create-account/connect-vi-to-azure-subscription.png)

3. Selecteer een Azure-regio van de ondersteunde locaties: VS-West 2, Noord-Europa of Oost-Azië.
4. Onder **Azure Media Services-account**, kies een van de volgende opties:

    * Voor het maken van een nieuwe Media Services-account selecteert **nieuwe resourcegroep maken**. Geef een naam voor de resourcegroep.

        Azure maakt uw nieuwe account in uw abonnement, met inbegrip van een nieuw Azure Storage-account. Nieuwe Media Services-account heeft een standaard initiële configuratie met een Streaming-eindpunt en 10 gereserveerde S3-eenheden.
    * Voor het gebruik van een Media Services-account, selecteert u **bestaande resource gebruiken**. Selecteer uw account in de lijst met accounts.

        Media Services-account moet dezelfde regio als uw Video Indexer-account hebben. Om te beperken de duur van indexering en lage doorvoer, het aanpassen van het type en aantal gereserveerde eenheden voor **10 gereserveerde S3-eenheden** in Media Services-account.
    * Voor het handmatig configureren van de verbinding, klikt u op de **overschakelen naar de handmatige configuratie**. 
    
        U wilt uw verbinding als voor een of andere reden de automatische optie kan niet worden voltooid of als de installatie en configuratie is anders dan de algemene aanvragen, of u wilt hebben volledige zichtbaarheid en controle over de instellingen handmatig configureren. 
        
        In de **Video Indexer verbinding maken met een Azure-abonnement**, geef de volgende informatie.

        |Instelling|Beschrijving|
        |---|---|
        |Video Indexer-accountregio|De naam van de regio van de Video Indexer-account. Voor betere prestaties en lagere kosten, is het raadzaam om op te geven van de naam van de regio waar de resource van Azure Media Services en Azure Storage-account zich bevinden. |
        |Azure Active Directory (AAD)-tenant|De naam van de Azure AD-tenant, bijvoorbeeld 'contoso.onmicrosoft.com'. De tenant-gegevens kan worden opgehaald uit de Azure-portal. Plaats de cursor boven de naam van de aangemelde gebruiker in de rechterbovenhoek.|
        |Abonnements-id|De Azure-abonnement waarmee deze verbinding moet worden gemaakt. De abonnements-ID kan worden opgehaald uit de Azure-portal. Klik op **alle services** in het linkerdeelvenster en zoek naar 'abonnementen'. Selecteer, **abonnementen** en kiest u de gewenste-ID in de lijst met uw abonnementen.|
        |Azure Media Services de naam van resourcegroep|De naam voor de resourcegroep waarin u kunt de Media Services-account bestaat.|
        |Media service-resourcenaam|De naam van de Azure Media Services-resource.|
        |Toepassings-id|De toepassings-ID voor Azure AD met machtigingen voor de opgegeven Media Services-account. Zie voor meer informatie, [gebruik service-principal verificatie](../../media-services/previous/media-services-portal-get-started-with-aad.md#service-principal-authentication).|
        |Toepassingssleutel|Zie voor meer informatie, [gebruik service-principal verificatie](../../media-services/previous/media-services-portal-get-started-with-aad.md#service-principal-authentication).|

5. Wanneer u klaar bent, kiest u **Connect**. Met deze bewerking kan enkele minuten duren. 

    Nadat u met Azure verbonden bent, wordt uw nieuwe Video Indexer-account wordt weergegeven in de lijst met accounts:

    ![nieuw account](./media/create-account/new-account.png)

6. Blader naar uw nieuwe account: 

    ![Video Indexer-account](./media/create-account/vi-account.png)

## <a name="considerations"></a>Overwegingen

De volgende Azure Media Services gerelateerde overwegingen zijn van toepassing:

* Als u verbinding met een nieuwe Media Services-account, ziet u een nieuwe resourcegroep, Media Services-account en een opslagaccount in uw Azure-abonnement.
* Als u met een nieuwe Media Services-account verbonden, het medium wordt ingesteld door Video Indexer **gereserveerde eenheden** op 10 S3-eenheden:

    ![Gereserveerde Media Services-eenheden](./media/create-account/ams-reserved-units.png)

* Als u met een Media Services-account verbonden, wordt niet de bestaande mediaset is gewijzigd voor Video Indexer **gereserveerde eenheden** configuratie.

    Mogelijk moet u het type en aantal media aanpassen **gereserveerde eenheden**, op basis van het geplande laden. Houd er rekening mee dat als het laden hoog is en u geen voldoende eenheden of snelheid, video's verwerken in time-fouten resulteren kunnen.

* Als u verbinding met een nieuwe Media Services-account, start Video Indexer automatisch standaard **Streaming-eindpunt** erin:

    ![Media Services streaming-eindpunt](./media/create-account/ams-streaming-endpoint.png)

* Als u met een Media Services-account verbonden, wordt de standaardconfiguratie voor Streaming-eindpunt niet gewijzigd door Video Indexer. Als er geen wordt uitgevoerd **Streaming-eindpunt**, kunt u zich niet Bekijk video's vanaf deze Media Services-account of in Video Indexer.

## <a name="next-steps"></a>Volgende stappen

U kunt via programmacode communiceren met uw proefaccount en/of met uw Video Indexer-accounts die zijn verbonden met azure door de instructies in: [gebruik API's](video-indexer-use-apis.md).

U moet dezelfde Azure AD-gebruiker die u hebt gebruikt bij het verbinden met Azure.


