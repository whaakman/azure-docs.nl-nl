---
title: Bestanden uploaden naar een Azure Media Services-account met behulp van REST | Microsoft Docs
description: Informatie over het verkrijgen van media-inhoud in Media Services door te maken en uploaden van activa.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: a241f66adecbab1d0b1462f379d3765d6c1de252
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55995387"
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>Bestanden uploaden naar een Media Services-account met behulp van REST

In Media Services uploadt u uw digitale bestanden naar een blob-container die zijn gekoppeld aan een asset. De [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entiteit kan video, audio, afbeeldingen, verzamelingen van miniaturen, tekst tekstsporen en ondertitelingsbestanden bestanden (en de metagegevens over deze bestanden) bevatten. Zodra de bestanden zijn geüpload naar de container van de asset, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming.

In dit artikel wordt beschreven hoe u een lokaal bestand suing REST te uploaden.

## <a name="prerequisites"></a>Vereisten

Als u wilt de in dit onderwerp beschreven stappen hebt voltooid, hebt u naar:

- Beoordeling [Asset concept](assets-concept.md).
- [Postman configureren voor Azure Media Services REST API-aanroepen](media-rest-apis-with-postman.md).
    
    Zorg ervoor dat u de laatste stap in het onderwerp [Azure AD Token ophalen](media-rest-apis-with-postman.md#get-azure-ad-token). 

## <a name="create-an-asset"></a>Maak een asset

Deze sectie wordt beschreven hoe u een nieuwe Asset maken.

1. Selecteer **activa** -> **maken of bijwerken van een Asset**.
2. Druk op **Verzenden**.

    ![Maak een asset](./media/upload-files/postman-create-asset.png)

U ziet de **antwoord** met de informatie over de zojuist gemaakte asset.

## <a name="get-a-sas-url-with-read-write-permissions"></a>Ophalen van een SAS-URL met machtigingen voor lezen / schrijven 

Deze sectie wordt beschreven hoe u een SAS-URL die is gegenereerd voor de gemaakte asset. De SAS-URL is gemaakt met machtigingen voor lezen / schrijven en kan worden gebruikt om de digitale bestanden uploaden naar de container Asset.

1. Selecteer **activa** -> **de URL van de Asset**.
2. Druk op **Verzenden**.

    ![Bestand uploaden](./media/upload-files/postman-create-sas-locator.png)

U ziet de **antwoord** met de informatie over asset van URL's. De eerste URL kopiëren en deze gebruiken om uw bestand te uploaden.

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Een bestand uploaden naar blob-opslag met de upload-URL

Gebruik de Azure Storage-API's of de SDK's (bijvoorbeeld de [REST API voor Storage](../../storage/common/storage-rest-api-auth.md), [JAVA SDK](../../storage/blobs/storage-quickstart-blobs-java-v10.md), of [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md).

## <a name="next-steps"></a>Volgende stappen

[Zelfstudie: Een extern bestand op basis van URL coderen en streamen van de video - REST](stream-files-tutorial-with-rest.md)
