---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2019
ms.openlocfilehash: 0745e4987ca602a41f2d93ddc0a93cb061ce127d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58052102"
---
Als u nodig hebt voor het configureren van een HTTP-proxy voor uitgaande aanvragen, gebruikt u deze twee argumenten:

| Name | Gegevenstype | Description |
|--|--|--|
|HTTP_PROXY|string|de proxy moet worden gebruikt, bijvoorbeeld: `http://proxy:8888`|
|HTTP_PROXY_CREDS|string|de referenties die nodig zijn voor verificatie op basis van de proxy, bijvoorbeeld gebruikersnaam.|

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=http://190.169.1.6:3128 \
HTTP_PROXY_CREDS=jerry:123456 \
Logging:Disk:LogLevel=Debug Logging:Disk:Format=json
```