---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 25a70d314ecb690a34e89a3dff7d2c4fad8b7a00
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42820156"
---
Toegang tot het eindpunt voorspelling wordt geleverd met een eindpuntsleutel. Gebruik de gratis starter-sleutel die is gekoppeld aan uw LUIS-account voor de doeleinden van deze quickstart. 
 
1. Aanmelden met uw LUIS-account. 

    [![Schermafbeelding van Language Understanding (LUIS) app-lijst](media/cognitive-services-luis/app-list.png "Screenshot van Language Understanding (LUIS) app-lijst")](media/cognitive-services-luis/app-list.png)

2. Selecteer uw naam in het menu rechtsboven en selecteer vervolgens **instellingen**.

    ![Instellingen van LUIS-menu gebruikerstoegang](media/cognitive-services-luis/get-user-settings-in-luis.png)

3. Kopieer de waarde van de **ontwerpen sleutel**. U gebruikt dit later in deze quickstart. 

    [![Schermafbeelding van Language Understanding (LUIS) gebruikersinstellingen](media/cognitive-services-luis/get-user-authoring-key.png "gebruikersinstellingen Screenshot van Language Understanding (LUIS)")](media/cognitive-services-luis/get-user-authoring-key.png)

    De ontwerphandleiding sleutel kan gratis onbeperkte aanvragen naar de API ontwerpen en tot wel 1000 query's naar het eindpunt van de voorspellings-API per maand voor al uw LUIS-apps. <!--Once the prediction endpoint quota from the authoring key is used for the month, you need to create a **Language Understanding** key from the Azure portal. The key created in the portal is known as the endpoint key. The endpoint key is used _only_ for prediction endpoint queries.-->