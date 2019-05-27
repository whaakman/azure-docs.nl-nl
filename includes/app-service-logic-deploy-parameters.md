---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: f188f2c7bea511f1109d37ef49563e0f745a770e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66167422"
---
Met Azure Resource Manager kunt u parameters voor de waarden voor gebruik bij het implementeren van de sjabloon definiëren. De sjabloon bevat een `parameters` sectie die alle parameterwaarden bevat. De waarde van elke parameter wordt gebruikt door de sjabloon voor het definiëren van de resources die u wilt implementeren.

> [!NOTE]
> Definieer geen parameters voor waarden die altijd hetzelfde blijven. Definieer parameters alleen voor waarden die variëren, gebaseerd op het project dat u wilt implementeren of op basis van de omgeving waar u implementeert.

Wanneer u parameters definiëren:

* De toegestane waarden die een gebruiker tijdens de implementatie opgeven kunt wilt opgeven, gebruikt u de **allowedValues** veld.

* Als u wilt toewijzen aan parameter standaardwaarden wanneer er geen waarden zijn opgegeven tijdens de implementatie, gebruikt u de **defaultValue** veld. 
