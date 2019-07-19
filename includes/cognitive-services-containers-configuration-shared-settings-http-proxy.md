---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 84cd8ed79281b005407b5a857398b5669635c072
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320492"
---
Als u een HTTP-proxy wilt configureren voor het maken van uitgaande aanvragen, gebruikt u deze twee argumenten:

| Name | Gegevenstype | Description |
|--|--|--|
|HTTP_PROXY|string|De proxy die moet worden gebruikt, bijvoorbeeld`http://proxy:8888`<br>`<proxy-url>`|
|HTTP_PROXY_CREDS|string|Alle referenties die nodig zijn voor verificatie bij de proxy, bijvoorbeeld gebruikers naam: wacht woord.|
|`<proxy-user>`|string|De gebruiker voor de proxy.|
|`<proxy-password>`|string|Het wacht woord dat `<proxy-user>` is gekoppeld aan de proxy.|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```
