---
title: Docker compose container recepten
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: 86b6a030af907bd0bf9d33d067ff253fd8601bbe
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295766"
---
# <a name="use-multiple-containers-in-a-private-network-with-docker-compose"></a>Meerdere containers in een particulier netwerk met Docker Compose gebruiken

Geef de juiste _apikey_, _facturering_, en _URI van het eindpunt_ waarden.

```docker-compose
version: '3.3'
services:
  forms:
    image:  "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    environment:
       eula: accept
       billing: "< Your billing URL >"
       apikey: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       FormRecognizer__ComputerVisionApiKey: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       FormRecognizer__ComputerVisionEndpointUri: "< Your computer vision, form recognizer URI >"
    volumes:
       - type: bind
         source: e:\publicpreview\output
         target: /output
       - type: bind
         source: e:\publicpreview\input
         target: /input
    ports:
      - "5010:5000"

  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text"
    environment:
      eula: accept
      apikey: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
      billing: "< Your billing URL >"
    ports:
      - "5021:5000"
```
