---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2019
ms.openlocfilehash: f4925401235aedb341a7e29ca36b079126647f7b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176309"
---
## <a name="validate-that-a-container-is-running"></a>Valideren dat een container wordt uitgevoerd 

Er zijn verschillende manieren om te valideren dat de container wordt uitgevoerd. 

|Aanvraag|Doel|
|--|--|
|`http://localhost:5000/`|De container biedt een startpagina.|
|`http://localhost:5000/status`|Aangevraagd met GET, om te valideren dat de container wordt uitgevoerd zonder dat een eindpunt-query. Deze aanvraag kan worden gebruikt voor Kubernetes [liveness en gereedheid voor tests](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).|
|`http://localhost:5000/swagger`|De container biedt een volledige set met documentatie voor de eindpunten en een `Try it now` functie. Met deze functie kunt u uw instellingen invoeren in een web gebaseerde HTML-formulier en de query te maken zonder code te schrijven. Nadat de query retourneert, wordt een voorbeeld van de CURL-opdracht opgegeven om te demonstreren dat de HTTP-headers en hoofdtekst van de indeling die is vereist. |

![Startpagina van de container](./media/cognitive-services-containers-api-documentation/container-webpage.png)
