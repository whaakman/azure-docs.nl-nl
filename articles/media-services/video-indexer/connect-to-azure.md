---
title: Een Video Indexer-account maken in Azure portal
titlesuffix: Azure Media Services
description: In dit artikel laat zien hoe een Video Indexer-account maken in Azure portal.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: f220aee3fa0d9a79723383fc31fec0eed2554bb4
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52833683"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Maken van een Video Indexer-account dat is verbonden met Azure

Wanneer u een Video Indexer-account maakt, kunt u kiezen uit een gratis proefversie (waarmee u een bepaald aantal gratis minuten indexering krijgt) of een betaalde optie (zonder quotumlimiet). Bij de gratis proefversie biedt Video Indexer websitegebruikers maximaal 600 minuten aan gratis indexering en API-gebruikers maximaal 2400 minuten gratis indexering. Met de betaalde optie maakt u een Video Indexer-account dat is verbonden met uw Azure-abonnement en een Azure Media Services-account. U betaalt zowel voor de geïndexeerde minuten als voor kosten verbonden aan het Media-account. 

In dit artikel laat zien hoe een Video Indexer-account dat gekoppeld aan een Azure-abonnement en een Azure Media Services-account maken. Het onderwerp bevat stappen voor het verbinden met Azure met behulp van de stroom automatisch (standaard). Ook ziet u hoe handmatig verbinding maken met Azure (Geavanceerd).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement.

    Als u een Azure-abonnement nog geen hebt, kunt u zich aanmelden voor [gratis proefversie van Azure](https://azure.microsoft.com/free/).

* Een Azure Active Directory (AD)-domein.

    Als u geen Azure AD-domein, moet u dit domein maken met uw Azure-abonnement. Zie voor meer informatie, [aangepaste domeinnamen in uw Azure Active Directory beheren](../../active-directory/users-groups-roles/domains-manage.md)

* Een gebruiker en het lid in uw Azure AD-domein. U gebruikt dit lid bij het verbinden van uw Video Indexer-account met Azure.

    Deze gebruiker moet een Azure AD-gebruiker met een werk- of schoolaccount, niet een persoonlijk account, zoals outlook.com, live.com of hotmail.com.

    ![alle AAD-gebruikers](./media/create-account/all-aad-users.png)

### <a name="additional-prerequisites-for-automatic-flow"></a>Aanvullende vereisten voor automatische doorstroming

Een gebruiker en het lid in uw Azure AD-domein. U gebruikt dit lid bij het verbinden van uw Video Indexer-account met Azure.

Deze gebruiker moet een lid in uw Azure-abonnement met ofwel een **eigenaar** rol of beide **Inzender** en **Administrator voor gebruikerstoegang** rollen. Een gebruiker kan twee keer worden toegevoegd met behulp van 2. Eenmaal met Inzender en anderzijds met Access-beheerder.

![Toegangsbeheer](./media/create-account/access-control-iam.png)

### <a name="additional-prerequisites-for-manual-flow"></a>Aanvullende vereisten voor handmatige flow

Registreer de resourceprovider voor EventGrid met behulp van de Azure portal.

In de [Azure-portal](https://portal.azure.com/), gaat u naar **abonnementen**-> [abonnement] ->**resourceproviders en**. 

Zoeken naar **Microsoft.Media** en **Microsoft.EventGrid**. Als u niet in de status 'Registered', klikt u op **registreren**. Het duurt enkele minuten om te registreren.

![EventGrid](./media/create-account/event-grid.png)

## <a name="connect-to-azure"></a>Verbinding maken met Azure

1. Ga naar de [Video Indexer](https://www.videoindexer.ai/)-website en meld u aan.

2. Klik op de **verbinding maken met Azure** knop:

    ![Verbinding maken met Azure](./media/create-account/connect-to-azure.png)

3. Wanneer de abonnementenlijst met wordt weergegeven, selecteert u het abonnement dat u wilt gebruiken.

    ![Video Indexer verbinden met Azure](./media/create-account/connect-vi-to-azure-subscription.png)

4. Selecteer een Azure-regio van de ondersteunde locaties: VS-West 2, Noord-Europa of Oost-Azië.
5. Onder **Azure Media Services-account**, kies een van de volgende opties:

    * Voor het maken van een nieuwe Media Services-account selecteert **nieuwe resourcegroep maken**. Geef een naam voor de resourcegroep.

        Azure maakt uw nieuwe account in uw abonnement, met inbegrip van een nieuw Azure Storage-account. Nieuwe Media Services-account heeft een standaard initiële configuratie met een Streaming-eindpunt en 10 gereserveerde S3-eenheden.
    * Voor het gebruik van een Media Services-account, selecteert u **bestaande resource gebruiken**. Selecteer uw account in de lijst met accounts.

        Media Services-account moet dezelfde regio als uw Video Indexer-account hebben. 

        > [!NOTE]
        > Om te beperken de duur van indexering en lage doorvoer, het is raadzaam om aan te passen van het type en aantal [gereserveerde eenheden](../previous/media-services-scale-media-processing-overview.md ) naar **10 gereserveerde S3-eenheden** in Media Services-account. Zie [portal gebruiken om te wijzigen van de gereserveerde eenheden](../previous/media-services-portal-scale-media-processing.md).

    * Voor het handmatig configureren van de verbinding, klikt u op de **overschakelen naar de handmatige configuratie** koppeling.

        Zie voor gedetailleerde informatie, de [handmatig verbinding maken met Azure](#connect-to-azure-manually-advanced-option) de volgende sectie (geavanceerde optie).
6. Wanneer u klaar bent, kiest u **Connect**. Met deze bewerking kan enkele minuten duren. 

    Nadat u met Azure verbonden bent, wordt uw nieuwe Video Indexer-account wordt weergegeven in de lijst met accounts:

    ![nieuw account](./media/create-account/new-account.png)

7. Blader naar uw nieuwe account:

    ![Video Indexer-account](./media/create-account/vi-account.png)

## <a name="connect-to-azure-manually-advanced-option"></a>Handmatig verbinding maken met Azure (geavanceerde optie)

Als de verbinding met Azure is mislukt, kunt u proberen het probleem oplossen door handmatig verbinding te maken.

### <a name="create-and-configure-a-media-services-account"></a>Een Media Services-account maken en configureren

1. Gebruik de [Azure](https://portal.azure.com/) portal om te maken van een Azure Media Services-account, zoals beschreven in [maken van een account](../previous/media-services-portal-create-account.md).

    Bij het maken van een opslagaccount voor uw Media Services-account, selecteert u **StorageV2** voor soort account en **Geo-redundant (RGS)** voor replicatie velden.

    ![nieuwe AMS-account](./media/create-account/create-ams-account1.png)

    > [!NOTE]
    > Zorg ervoor dat u Noteer de namen van de Media Services-resource en -account. U moet deze voor de stappen in de volgende sectie.

2. Het type en aantal aanpassen [gereserveerde eenheden](../previous/media-services-scale-media-processing-overview.md ) naar **10 gereserveerde S3-eenheden** in het Media Services-account dat u hebt gemaakt. Zie [portal gebruiken om te wijzigen van de gereserveerde eenheden](../previous/media-services-portal-scale-media-processing.md).
3. Voordat u uw video's in de Video Indexer-webtoepassing afspelen kunt, moet u eerst de standaard **Streaming-eindpunt** van de nieuwe Media Services-account.

    Klik in de nieuwe Media Services-account op **Streaming-eindpunten**. Selecteer de Streaming-eindpunt en druk op start.

    ![nieuwe AMS-account](./media/create-account/create-ams-account2.png)

4. Voor Video Indexer om te verifiëren met Media Services-API, moet een AD-toepassing worden gemaakt. De volgende stappen handleiding die u via het verificatieproces van Azure AD dat wordt beschreven in [aan de slag met Azure AD-verificatie met behulp van de Azure-portal](../previous/media-services-portal-get-started-with-aad.md):

    1. Selecteer in het nieuwe Media Services-account, **API-toegang**.
    2. Selecteer [Service-principal verificatiemethode](../previous/media-services-portal-get-started-with-aad.md#service-principal-authentication).
    3. Ophalen van de client-ID en clientgeheim, zoals beschreven in de [ophalen van de client-ID en clientgeheim](../previous/media-services-portal-get-started-with-aad.md#get-the-client-id-and-client-secret) sectie.

        Nadat u hebt geselecteerd **instellingen**->**sleutels**, toevoegen **beschrijving**, drukt u op **opslaan**, waarde van de sleutel wordt gevuld.

        Als de sleutel is verlopen moet eigenaar van het account contact opnemen met ondersteuning voor Video Indexer om te vernieuwen van de sleutel.

        > [!NOTE]
        > Zorg ervoor dat u Noteer de sleutelwaarde en de toepassings-ID. U moet deze voor de stappen in de volgende sectie.

### <a name="connect-manually"></a>Handmatig verbinding maken

In de **Video Indexer verbinding maken met een Azure-abonnement** dialoogvenster van uw [Video Indexer](https://www.videoindexer.ai/) weergeeft, schakelt de **overschakelen naar de handmatige configuratie** koppeling.

Geef in het dialoogvenster de volgende informatie:

|Instelling|Beschrijving|
|---|---|
|Video Indexer-accountregio|De naam van de regio van de Video Indexer-account. Voor betere prestaties en lagere kosten, is het raadzaam om op te geven van de naam van de regio waar de resource van Azure Media Services en Azure Storage-account zich bevinden. |
|Azure Active Directory (AAD)-tenant|De naam van de Azure AD-tenant, bijvoorbeeld 'contoso.onmicrosoft.com'. De tenant-gegevens kan worden opgehaald uit de Azure-portal. Plaats de cursor boven de naam van de aangemelde gebruiker in de rechterbovenhoek. Zoek de naam aan de rechterkant van **domein**.|
|Abonnements-id|De Azure-abonnement waarmee deze verbinding moet worden gemaakt. De abonnements-ID kan worden opgehaald uit de Azure-portal. Klik op **alle services** in het linkerdeelvenster en zoek naar 'abonnementen'. Selecteer **abonnementen** en kiest u de gewenste-ID in de lijst met uw abonnementen.|
|Azure Media Services de naam van resourcegroep|De naam voor de resourcegroep waarin u de Media Services-account hebt gemaakt.|
|Media service-resourcenaam|De naam van het Azure Media Services-account dat u in de vorige sectie hebt gemaakt.|
|Toepassings-id|De Azure AD-ID (met machtigingen voor de opgegeven Media Services-account) die u in de vorige sectie hebt gemaakt.|
|Toepassingssleutel|De sleutel van de Azure AD-toepassing die u hebt gemaakt in de vorige sectie. |

## <a name="considerations"></a>Overwegingen

De volgende Azure Media Services gerelateerde overwegingen zijn van toepassing:

* Als u automatisch verbinding maken, ziet u een nieuwe resourcegroep, Media Services-account en een opslagaccount in uw Azure-abonnement.
* Als u automatisch verbinding maken, Video Indexer wordt ingesteld op de media **gereserveerde eenheden** op 10 S3-eenheden:

    ![Gereserveerde Media Services-eenheden](./media/create-account/ams-reserved-units.png)

* Als u verbinding met een Media Services-account maakt, wordt niet de bestaande mediaset is gewijzigd voor Video Indexer **gereserveerde eenheden** configuratie.

   Mogelijk moet u het type en aantal gereserveerde Media-eenheden aanpassen op basis van het geplande laden. Houd er rekening mee dat als het laden hoog is en u geen voldoende eenheden of snelheid, video's verwerken in time-fouten resulteren kunnen.

* Als u verbinding met een nieuwe Media Services-account maakt, start Video Indexer automatisch standaard **Streaming-eindpunt** erin:

    ![Media Services streaming-eindpunt](./media/create-account/ams-streaming-endpoint.png)

    Streaming-eindpunten hebben een aanzienlijke hoeveelheid opstarttijd. Daarom kan het enkele minuten vanaf het moment dat u uw account met Azure bent verbonden, totdat uw video's kunnen worden gestreamd en in de Video Indexer-web-App bekeken duren.

* Als u verbinding met een Media Services-account maakt, wordt de standaardconfiguratie voor Streaming-eindpunt niet gewijzigd door Video Indexer. Als er geen wordt uitgevoerd **Streaming-eindpunt**, kunt u zich niet Bekijk video's vanaf deze Media Services-account of in Video Indexer.

## <a name="next-steps"></a>Volgende stappen

U kunt via programmacode communiceren met uw proefaccount en/of met uw Video Indexer-accounts die zijn verbonden met azure door de instructies in: [gebruik API's](video-indexer-use-apis.md).

U moet dezelfde Azure AD-gebruiker die u hebt gebruikt bij het verbinden met Azure.


