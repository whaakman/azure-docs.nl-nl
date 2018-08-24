---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 0a74079677a084b2d4e8221cf5a74b356126811d
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42817445"
---
De functie Mobile Apps van Azure App Service maakt gebruik van [Azure Notification Hubs] pushes, verzenden, zodat u een notification hub voor uw mobiele app configureren.

1. In de [Azure Portal], gaat u naar **App Services**, en selecteer vervolgens de back-end van uw app. Onder **instellingen**, selecteer **Push**.
2. Selecteer een resource voor notification hub toevoegen aan de app, **Connect**. U kunt maken van een hub of verbinding maken met een bestaande resourcegroep.

    ![Een hub configureren](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

U hebt nu een notification hub verbonden met uw Mobile Apps-back-end-project. Later configureert u deze notification hub verbinden met een platform notification system (PNS) om naar apparaten te pushen.

[Azure Portal]: https://portal.azure.com/
[Azure Notification Hubs]: https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/
