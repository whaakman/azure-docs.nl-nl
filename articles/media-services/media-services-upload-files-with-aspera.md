---
title: Bestanden uploaden naar een Azure Media Services-account met behulp van Aspera | Microsoft Docs
description: Deze zelfstudie wordt u door de stappen voor het uploaden van bestanden in een opslagaccount die is gekoppeld aan een Media Services-account met de ** Aspera Server op aanvraag **-service op Azure.
services: media-services
documentationcenter: 
author: johndeu
manager: cfowler
editor: 
ms.assetid: 8812623a-b425-4a0f-9e05-0ee6c839b6f9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/17/2017
ms.author: juliako
ms.openlocfilehash: e3090da9b2c5b8f99545a1f7f9601bfd8d5221f1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-the-aspera-server-on-demand-service-on-azure"></a>Bestanden uploaden naar een Media Services-account met behulp van de service Aspera Server On Demand in Azure

## <a name="overview"></a>Overzicht

**Aspera** is software die een snelle bestandsoverdracht mogelijk maakt. **Aspera Server On Demand** voor Azure maakt snel uploaden en downloaden van grote bestanden rechtstreeks in Azure Blob-objectopslag mogelijk. Zie de [Aspera Cloud](http://cloud.asperasoft.com/)-site voor informatie over **Aspera On Demand**. 
  
**Aspera Server On Demand** voor Azure is verkrijgbaar in [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/). Om de aankoop van **Aspera Server On Demand** voor Azure te voltooien, meldt u zich aan bij Azure Marketplace met uw Windows Live ID.

In deze zelfstudie leert u stapsgewijs hoe u bestanden uploadt naar een opslagaccount dat is gekoppeld aan een Media Services-account met behulp van de service **Aspera Server On Demand** in Azure. 

[Hier](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest) vindt u een voorbeeld waarin wordt uitgelegd hoe u Azure Functions gebruikt met Aspera en Media Services.

>[!NOTE]
>Er is een maximale bestandsgrootte voor de verwerking met Azure Media Services-mediaprocessors (mp's). Raadpleeg [dit onderwerp](media-services-quotas-and-limitations.md) voor meer informatie over de maximale bestandsgrootte.
>

## <a name="prerequisites"></a>Vereisten 

Voor deze zelfstudie hebt u het volgende nodig:

* Een Windows Live ID
* Een [Azure-account](https://azure.microsoft.com). Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
* Een [Azure Media Services-account](media-services-portal-create-account.md).

## <a name="purchase-aspera-on-demand-for-azure"></a>Aspera On Demand kopen voor Azure

Nadat u zich hebt aangemeld bij Azure Marketplace, volgt u deze eenvoudige stappen om de aankoop van Aspera On Demand voor Azure te voltooien.

1. Zoek naar Aspera en selecteer 'Server On Demand'.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera001.png)

2. Bekijk de abonnementen en klik op 'Aanmelden'

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera002.png)

3. Voer de gegevens voor het Server On Demand-abonnement in.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera003.png)

4. Klik op de **prijscategorie** en selecteer het gewenste maandelijkse volume in het deelvenster. In het deelvenster **Plan details** (Abonnementsgegevens) selecteert u **OK**. Klik in het deelvenster **Uw prijscategorie kiezen** op **Selecteren**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera004.png)

5. Klik op **Juridische voorwaarden** om de juridische voorwaarden in het deelvenster te bekijken en te accepteren. Nadat u de juridische voorwaarden hebt gelezen, klikt u op **Kopen**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera005.png)

6. Voltooi de aankoop door op **Maken** te klikken.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera006.png)

7. Op het Azure-dashboard wordt weergegeven dat de service wordt ingericht.  Wanneer het inrichten is voltooid, kunt u het nieuwe abonnement vinden door op de naam van de service te zoeken in uw resources. Zodra u de service hebt gevonden, dubbelklikt u erop om de beheerportal van de service te openen.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera007.png)

8. Start de Aspera-beheerportal. Zodra u de nieuwe Aspera-service hebt gevonden, klikt u op de service om de beheerportal te openen.  Er wordt een nieuw deelvenster geopend. Klik binnen het nieuwe deelvenster op de **resourcenaam** van uw nieuwe service.  De resourcenaam in de volgende schermafbeelding is 'AsperaTransferDemo'. Zodra u op de resourcenaam klikt, wordt er een nieuw deelvenster geopend. In het nieuwe deelvenster ziet u een koppeling 'Beheren'. Klik op de koppeling 'Beheren' om de beheerportal van Aspera te openen.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera008.png)

9. Door op de koppeling 'Beheren' te klikken, wordt u naar de registratiepagina geleid. Deze is nodig om toegang te krijgen tot de service.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera009.png)

10. U hoort nu toegang te hebben tot de beheerportal van de Aspera-service. Hier kunt u toegangstoetsen maken, Aspera-clients en -licenties downloaden, het gebruik bekijken en meer informatie over de API's krijgen.

    In de volgende schermafbeelding ziet u hoe toegang maken in zijn werk gaat. 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera010.png)

    In de volgende schermafbeelding ziet u de interfaces voor gebruiksrapporten in de portal. 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera011.png)

## <a name="upload-files-with-aspera"></a>Bestanden uploaden met Aspera

1. Download en installeer de clientsoftware van Aspera:
    
    * [Invoegtoepassing browser](http://downloads.asperasoft.com/connect2/)
    * [Rich client](http://downloads.asperasoft.com/en/downloads/2)

2. Voer uw eerste overdracht uit. Voer het volgende uit om de Aspera-client te gebruiken om overdrachten uit te voeren met de Aspera-overdrachtsservice: 

    1. Maak een toegangstoets via de Aspera-portal.  
    2. Download, installeer en licentieer de Aspera-client (de software vindt u in de Aspera-portal).  

    >[!NOTE]
    >Lees de gids van de Aspera-client voor informatie over de configuratie.
    
    3. Haal bepaalde gegevens van uw opslagaccount op die zijn gekoppeld aan uw Azure-media-account met behulp van [Azure Portal](https://portal.azure.com/). Specifiek gaat het om de naam en sleutel, en de naam van de opslag-blob-container waarin u uw inhoud wilt plaatsen. 

        * Opslaggegevens ophalen uit de portal: zoek uw opslagaccount, klik op de toegangstoetsen en kopieer de naam en sleutel van uw account.
        * De containernaam ophalen: zoek uw opslagaccount, selecteer **Blobs**, selecteer de naam van de container waarnaar u de inhoud wilt uploaden. 

    Hieronder ziet u een schermafbeelding van **Verbindingsbeheer** van de Aspera-client waarin u het type Azure-opslag en referenties dient op te geven, evenals de blob-container.

    ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera012.png)

## <a name="resources"></a>Resources

De volgende resources zijn vermeld in dit artikel. 

* [Connect Browser Plugin](http://downloads.asperasoft.com/connect2/)
* [Connect-handleiding](http://downloads.asperasoft.com/en/documentation/8)
* [Aspera-client](http://downloads.asperasoft.com/en/downloads/2)
* [Clienthandleiding](http://downloads.asperasoft.com/en/documentation/2)

## <a name="next-steps"></a>Volgende stappen

U kunt nu [blobs vanuit een opslagaccount kopiëren naar een AMS-account](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

