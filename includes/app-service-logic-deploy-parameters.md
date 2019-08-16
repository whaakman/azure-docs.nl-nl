---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: f188f2c7bea511f1109d37ef49563e0f745a770e
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "68385756"
---
Met Azure Resource Manager kunt u para meters definiëren voor de waarden die moeten worden gebruikt bij het implementeren van de sjabloon. De sjabloon bevat een `parameters` sectie die alle parameter waarden bevat. Elke parameter waarde wordt door de sjabloon gebruikt voor het definiëren van de resources die u wilt implementeren.

> [!NOTE]
> Definieer geen parameters voor waarden die altijd hetzelfde blijven. Definieer alleen para meters voor waarden die variëren, op basis van het project dat u implementeert of op basis van de omgeving waarin u implementeert.

Wanneer u para meters definieert:

* Als u de toegestane waarden wilt opgeven die een gebruiker kan opgeven tijdens de implementatie, gebruikt u het veld **allowedValues** .

* Als u standaard waarden wilt toewijzen aan een para meter wanneer er geen waarden worden gegeven tijdens de implementatie, gebruikt u het veld **DefaultValue** . 
