---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 02/21/2019
ms.openlocfilehash: e80feac7dbf16652cc2e2a6176ed8b2c8c48e35b
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753636"
---
### <a name="running-multiple-containers-on-the-same-host"></a>Meerdere containers uitvoeren op dezelfde host

Als u van plan bent om uit te voeren van meerdere containers met blootgestelde poorten, zorg ervoor dat elke container uitvoeren met een andere poort. Bijvoorbeeld: de eerste container op poort 5000- en de tweede container uitvoeren op poort 5001.

Vervang de `<container-registry>` en `<container-name>` met de waarden van de containers die u gebruikt. Deze hoeven niet te zijn van dezelfde container. U kunt de Face-container en de LUIS-container die samen worden uitgevoerd op de HOST hebben of u kunt meerdere Face containers die worden uitgevoerd. 

De eerste container worden uitgevoerd op poort 5000. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

De tweede container op poort 5001 worden uitgevoerd.


```bash 
docker run --rm -it -p 5001:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Elke volgende container moet zich op een andere poort. 
