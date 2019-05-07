---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2019
ms.openlocfilehash: fe1b4699a300831294c26b103d322fb83ad87d3b
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65151103"
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