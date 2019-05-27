---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 51a75ee7bf87c38e3916bdbc8d85abcfb14dca8b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140334"
---
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **door alles bladeren** > **App Services**, en klik vervolgens op de back-end van uw mobiele Apps. Onder **instellingen**, klikt u op **App Service-Push**, en klik vervolgens op de naam van uw notification hub.
2. Ga naar **Google (GCM)**, voer de **serversleutel** waarde die is verkregen van Firebase in de vorige procedure, en klik vervolgens op **opslaan**.

    ![Instellen van de API-sleutel in de portal](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

De back-end voor mobiele Apps is nu geconfigureerd voor het gebruik van Firebase Cloud Messaging. Hiermee kunt u pushmeldingen verzendt naar uw app wordt uitgevoerd op een Android-apparaat met behulp van de notification hub.
