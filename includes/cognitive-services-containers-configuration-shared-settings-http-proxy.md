---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 9911b1c92bdca6c0cdf064ea484cfb603e659467
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712485"
---
Als u nodig hebt voor het configureren van een HTTP-proxy voor uitgaande aanvragen, gebruikt u deze twee argumenten:

| Name | Gegevenstype | Description |
|--|--|--|
|HTTP_PROXY|string|de proxy moet worden gebruikt, bijvoorbeeld: `http://proxy:8888`<br><proxy-url>|
|HTTP_PROXY_CREDS|string|de referenties die nodig zijn voor verificatie op basis van de proxy, bijvoorbeeld gebruikersnaam.|
|`<proxy-user>`|string|De gebruiker voor de proxy.|
|`proxy-password`|string|Het wachtwoord dat is gekoppeld aan `<proxy-user>` voor de proxy.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<billing-endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```