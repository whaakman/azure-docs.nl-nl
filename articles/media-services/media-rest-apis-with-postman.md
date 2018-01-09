---
title: Postman configureren voor Azure Media Services REST API-aanroepen
description: Informatie over het configureren van Postman voor Media Services REST API-aanroepen.
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
ms.date: 01/04/2017
ms.author: juliako
ms.openlocfilehash: a24b73a93bddbeb5b56ddfdf604fa99faccea442
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Postman configureren voor Media Services REST API-aanroepen

Deze zelfstudie leert u hoe u configureert **Postman** zodat deze kan worden gebruikt om aan te roepen Azure Media Services (AMS) REST-API's. De zelfstudie laat zien hoe u de omgeving en verzameling bestanden importeren in **Postman**. De verzameling bevat gegroepeerde definities van HTTP-aanvragen die REST-API's van Azure Media Services (AMS) aanroepen. Het bestand omgeving bevat variabelen die worden gebruikt door de verzameling.

Deze omgeving en de verzameling wordt gebruikt in de artikelen die laten zien hoe u verschillende taken met Azure Media Services REST API's te bereiken.

## <a name="prerequisites"></a>Vereisten

- Installeer de [Postman](https://www.getpostman.com/) REST-client wilt uitvoeren van de REST-API's in een aantal van de REST van de AMS-zelfstudies wordt weergegeven. 

    We gebruiken **Postman** maar een REST-hulpmiddel zou geschikt zijn. Andere alternatieven zijn: **Visual Studio Code** met de REST-invoegtoepassing of **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>De omgeving configureren 

1. Maak een .json-bestand met de omgevingsvariabelen in AMS zelfstudies gebruikt. Noem het bestand **AzureMediaServices.postman_environment.json**. Open het bestand en plak de code die de Postman-omgeving van definieert [dit codevoorbeeld](postman-environment.md). 
2. Open de **Postman**.
3. Selecteer aan de rechterkant van het scherm de **beheren omgeving** optie.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-create-env.png)
4. Van de **beheren omgeving** dialoogvenster, klikt u op **importeren**.
5. Bladeren en selecteer de **AzureMediaServices.postman_environment.json** bestand.
6. De **AzureMedia** omgeving wordt toegevoegd.
7. Sluit het dialoogvenster.
8. Selecteer de **AzureMedia** omgeving.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Configureer de verzameling

1. Maak een .json-bestand met de **Postman** verzameling met alle bewerkingen die nodig zijn voor het uploaden van een bestand met Media Services. Noem het bestand **AzureMediaServicesOperations.postman_collection.json**. Open het bestand en plak de code die definieert de **Postman** verzameling van [dit codevoorbeeld](postman-collection.md).
2. Klik op **importeren** om de verzameling-bestand te importeren.
3. Kies de **AzureMediaServicesOperations.postman_collection.json** bestand.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Volgende stappen

Bekijk de [uploaden activa](media-services-rest-upload-files.md) artikel.  
