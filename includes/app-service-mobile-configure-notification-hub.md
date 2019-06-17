---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: da3793c428c624ce3a224cbd7606ab26c4a50803
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66140272"
---
De functie Mobile Apps van Azure App Service maakt gebruik van [Azure Notification Hubs] pushes, verzenden, zodat u een notification hub voor uw mobiele app configureren.

1. In de [Azure Portal], gaat u naar **App Services**, en selecteer vervolgens de back-end van uw app. Onder **instellingen**, selecteer **Push**.
2. Selecteer een resource voor notification hub toevoegen aan de app, **Connect**. U kunt maken van een hub of verbinding maken met een bestaande resourcegroep.

    ![Een hub configureren](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

U hebt nu een notification hub verbonden met uw Mobile Apps-back-end-project. Later configureert u deze notification hub verbinden met een platform notification system (PNS) om naar apparaten te pushen.

[Azure Portal]: https://portal.azure.com/
[Azure Notification Hubs]: https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/
