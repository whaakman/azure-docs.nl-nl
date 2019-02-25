---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 02/21/2019
ms.author: rgarcia
ms.openlocfilehash: 0e4c0b6d622fe0d8d20692ec6c0c889779e4601a
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753035"
---
### <a name="access-tokens"></a>Toegangstokens

Toegangstokens zijn een meer robuuste methode voor verificatie met Azure ruimtelijke ankers. Met name bij het voorbereiden van uw toepassing voor een productie-implementatie. De samenvatting van deze benadering is voor het instellen van een back-end-service die uw clienttoepassing kan veilig worden geverifieerd met. Uw back-end-service-interfaces met AAD tijdens runtime en de service Azure ruimtelijke ankers STS om aan te vragen een toegangstoken. Dit token wordt vervolgens geleverd aan de clienttoepassing en in de SDK gebruikt voor verificatie met Azure ruimtelijke ankers.
