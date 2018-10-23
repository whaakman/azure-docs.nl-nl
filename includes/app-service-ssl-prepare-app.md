---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 10/15/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 858d098f18604d1360af8509eb8a0cec6fa7d0c7
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49354526"
---
## <a name="prepare-your-web-app"></a>Uw web-app voorbereiden

Als u een aangepast SSL-certificaat (een certificaat van een derde of een App Service-certificaat) met uw web-app wilt verbinden, moet uw [App Service-plan](https://azure.microsoft.com/pricing/details/app-service/) op de prijscategorie **Basic**, **Standard** **Premium** of **Isolated** zijn ingesteld. In deze stap zorgt u ervoor dat de web-app zich in de ondersteunde prijscategorie bevindt.

### <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Open de [Azure Portal](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Navigeer naar uw web-app

Klik in het linkermenu op **App Services** en klik op de naam van uw web-app.

![Selecteer de web-app](./media/app-service-ssl-prepare-app/select-app.png)

U bevindt zich op de beheerpagina van uw web-app.  

### <a name="check-the-pricing-tier"></a>Controleer de prijscategorie

Scrol in de navigatiebalk links van de web-app-pagina naar het gedeelte **Instellingen** en selecteer **Opschalen (App Service-plan)**.

![Menu Opschalen](./media/app-service-ssl-prepare-app/scale-up-menu.png)

Controleer of de web-app zich niet in de laag **F1** of **D1** bevindt. De huidige categorie van de web-app is gemarkeerd met een donkerblauw vak.

![Controleer prijscategorie](./media/app-service-ssl-prepare-app/check-pricing-tier.png)

Aangepaste SSL wordt niet ondersteund in de laag **F1** of **D1**. Als u omhoog moet schalen, volgt u de stappen in het volgende gedeelte. Anders sluit u de pagina **Scale up** (Omhoog schalen) en gaat u door naar [Upload and bind your SSL certificate](#upload) (Uw SSL-certificaat uploaden en binden).

### <a name="scale-up-your-app-service-plan"></a>Uw App Service-plan omhoog schalen

Selecteer een van de lagen die niet gratis zijn (**B1**, **B2**, **B3** of een laag in de categorie **Productie**). Klik op **Aanvullende opties bekijken** voor aanvullende opties.

Klik op **Toepassen**.

![Prijscategorie kiezen](./media/app-service-ssl-prepare-app/choose-pricing-tier.png)

Wanneer u de volgende melding ziet, is de schaalbewerking voltooid.

![Melding voor omhoog schalen](./media/app-service-ssl-prepare-app/scale-notification.png)

