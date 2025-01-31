---
title: Postman configureren voor Azure Media Services REST API-aanroepen
description: Informatie over het configureren van Postman voor Media Services REST API-aanroepen.
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
ms.date: 04/11/2019
ms.author: juliako
ms.openlocfilehash: a2171ff8a4354a59ec2f790f9bf38b7a687419ca
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60322546"
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Postman configureren voor Media Services REST API-aanroepen

Dit artikel leest u hoe het configureren van **Postman** zodat deze kan worden gebruikt voor het aanroepen van Azure Media Services (AMS) REST-API's. Het artikel wordt beschreven hoe u omgeving en verzameling bestanden importeren in **Postman**. De verzameling bevat gegroepeerde definities van HTTP-aanvragen die Azure Media Services (AMS) REST-API's aanroepen. Het omgevingsbestand bevat variabelen die worden gebruikt door de verzameling.

Controleer voordat u begint met het ontwikkelen, [ontwikkelen met Media Services v3 API's](media-services-apis-overview.md).

## <a name="prerequisites"></a>Vereisten

- [Een Azure Media Services-account maken](create-account-cli-how-to.md). Zorg ervoor dat u de naam van de resourcegroep en de naam van de Media Services-account. 
- Informatie die nodig zijn voor [toegang tot API's](access-api-cli-how-to.md)
- Installeer de [Postman](https://www.getpostman.com/) REST-client als u de REST-API's wilt uitvoeren die in een aantal AMS REST-zelfstudies worden weergegeven. 

    We gebruiken **Postman** maar elk ander REST-hulpprogramma is hiervoor geschikt. Enkele andere alternatieven: **Visual Studio Code** met de REST-invoegtoepassing of **Telerik Fiddler**. 

## <a name="download-postman-files"></a>Postman-bestanden downloaden

Kloon een GitHub-opslagplaats die de Postman verzameling en -omgevingsbestanden bevat.

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-rest-postman.git
 ```

## <a name="configure-postman"></a>Postman configureren

In deze sectie ziet u hoe u de Postman configureert.

### <a name="configure-the-environment"></a>De omgeving configureren 

1. Open de **Postman**.
2. Selecteer rechts van het scherm de optie **Manage environment**.

    ![Omgeving beheren](./media/develop-with-postman/postman-import-env.png)
4. Klik in het dialoogvenster **Manage environment** op **Import**.
2. Blader naar het bestand `Azure Media Service v3 Environment.postman_environment.json` dat is gedownload toen u `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kloonde.
6. De omgeving **Azure Media Service v3 Environment** is toegevoegd.

    > [!Note]
    > Werk de toegangsvariabelen bij met waarden die u hebt gekregen in de sectie **Toegang krijgen tot de Media Services API** hierboven.

7. Dubbelklik op het geselecteerde bestand en voer waarden in die u hebt verkregen door de toegang tot API-stappen te volgen.
8. Sluit het dialoogvenster.
9. Selecteer de omgeving **Azure Media Service v3 Environment** in de vervolgkeuzelijst.

    ![Omgeving kiezen](./media/develop-with-postman/choose-env.png)
   
### <a name="configure-the-collection"></a>De collectie configureren

1. Klik op **Import** om het verzamelingbestand te importeren.
1. Blader naar het bestand `Media Services v3.postman_collection.json` dat is gedownload toen u `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kloonde
3. Kies het bestand **Media Services v3.postman_collection.json**.

    ![Een bestand importeren](./media/develop-with-postman/postman-import-collection.png)

## <a name="get-azure-ad-token"></a>Azure AD-token verkrijgen 

Voordat u begint met het bewerken van AMS v3 resources die u wilt ophalen en instellen van Azure AD-Token voor verificatie van Service-Principal.

1. Selecteer in het linkervenster van Postman 'Step 1: Get AAD Auth token'.
2. Selecteer vervolgens Get Azure AD Token for Service Principal Authentication.
3. Druk op **Verzenden**.

    De volgende **POST**-bewerking wordt verzonden.

    ```
    https://login.microsoftonline.com/:tenantId/oauth2/token
    ```

4. Het antwoord bevat de token en stelt de omgevingsvariabele AccessToken in op de tokenwaarde.  

    ![AAD-token verkrijgen](./media/develop-with-postman/postman-get-aad-auth-token.png)

## <a name="see-also"></a>Zie ook

- [Bestanden uploaden naar een Media Services-account - REST](upload-files-rest-how-to.md)
- [Filters maken met Media Services - REST](filters-dynamic-manifest-rest-howto.md)
- [REST API op basis van Azure Resource Manager](https://github.com/Azure-Samples/media-services-v3-arm-templates)

## <a name="next-steps"></a>Volgende stappen

- [Bestanden met de REST Stream](stream-files-tutorial-with-rest.md).  
- [Zelfstudie: Extern bestand coderen op basis van URL en video streamen - REST](stream-files-tutorial-with-rest.md)
