---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 00cc63f53388ab7bea05a0b55784247f63477684
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704172"
---
## <a name="validate-that-a-container-is-running"></a>Valideren dat een container wordt uitgevoerd 

Er zijn verschillende manieren om te valideren dat de container wordt uitgevoerd. 

|Aanvraag|Doel|
|--|--|
|`http://localhost:5000/`|De container biedt een startpagina.|
|`http://localhost:5000/status`|Aangevraagd met GET, om te valideren dat de container wordt uitgevoerd zonder dat een eindpunt-query. Deze aanvraag kan worden gebruikt voor Kubernetes [liveness en gereedheid voor tests](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).|
|`http://localhost:5000/swagger`|De container biedt een volledige set met documentatie voor de eindpunten en een `Try it now` functie. Met deze functie kunt u uw instellingen invoeren in een web gebaseerde HTML-formulier en de query te maken zonder code te schrijven. Nadat de query retourneert, wordt een voorbeeld van de CURL-opdracht opgegeven om te demonstreren dat de HTTP-headers en hoofdtekst van de indeling die is vereist. |

![Startpagina van de container](./media/cognitive-services-containers-api-documentation/container-webpage.png)
