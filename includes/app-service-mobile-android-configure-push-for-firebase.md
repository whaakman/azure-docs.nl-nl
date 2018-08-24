---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: bb03e4b5b04a0272d8fa9b032da5adb50878b620
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42811625"
---
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **door alles bladeren** > **App Services**, en klik vervolgens op de back-end van uw mobiele Apps. Onder **instellingen**, klikt u op **App Service-Push**, en klik vervolgens op de naam van uw notification hub.
2. Ga naar **Google (GCM)**, voer de **serversleutel** waarde die is verkregen van Firebase in de vorige procedure, en klik vervolgens op **opslaan**.

    ![Instellen van de API-sleutel in de portal](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

De back-end voor mobiele Apps is nu geconfigureerd voor het gebruik van Firebase Cloud Messaging. Hiermee kunt u pushmeldingen verzendt naar uw app wordt uitgevoerd op een Android-apparaat met behulp van de notification hub.
