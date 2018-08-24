---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 25bb5cfb0073f7533faddec43b38fb5031b82a43
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42811598"
---
1. Start op uw Mac **toegang tot sleutelhanger**. Op de linkernavigatiebalk onder **categorie**Open **mijn certificaten**. Zoek het SSL-certificaat dat u hebt gedownload in de vorige sectie en vervolgens de inhoud ervan te vermelden. Selecteer alleen het certificaat (niet de persoonlijke sleutel selecteren). Vervolgens [exporteren](https://support.apple.com/kb/PH20122?locale=en_US).
2. In de [Azure-portal](https://portal.azure.com/), selecteer **door alles bladeren** > **App Services**. Vervolgens Selecteer uw Mobile Apps-back-end. 
3. Onder **instellingen**, selecteer **App Service-Push**. Selecteer vervolgens de naam van uw notification hub. 
4. Ga naar **Apple Push Notification Services** > **-certificaat uploaden**. Upload het .p12-bestand selecteren van de juiste **modus** (afhankelijk van of uw SSL-clientcertificaat uit eerder is productie- of sandbox). Wijzigingen opslaan.

Uw service is nu geconfigureerd om te werken met pushmeldingen in iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
