---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 0dab71b6d169e26a3d7dc208dd09efe1143fbe13
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305237"
---
### <a name="open-the-publish-wizard"></a>Open de wizard Publiceren

In **Solution Explorer**, met de rechtermuisknop op de **SharingService** project en selecteer **publiceren**.

De Wizard publiceren wordt gestart. Selecteer **App Service** > **publiceren** openen de **Create App Service** in het dialoogvenster.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

In de **Create App Service** in het dialoogvenster, selecteer **een account toevoegen** en aanmelden bij uw Azure-abonnement. Als u al bent aangemeld, selecteert u het gewenste account uit de vervolgkeuzelijst.

> [!NOTE]
> Als u al bent aangemeld, selecteert u **Maken** nog niet.
>

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [resource group intro text](resource-group.md)]

Selecteer **Nieuw** naast **Resourcegroep**.

Geef de resourcegroep de naam **myResourceGroup** en selecteer **OK**.

### <a name="create-an-app-service-plan"></a>Een App Service-plan maken

[!INCLUDE [app-service-plan](app-service-plan.md)]

Selecteer bij **Hostingabonnement** **Nieuw**.

In de **configureren abonnement voor webhosting** dialoogvenster vak, gebruikt u deze instellingen:

| Instelling | Voorgestelde waarde | Description |
|-|-|-|
|App Service-plan| MySharingServicePlan | De naam van het App Service-plan. |
| Locatie | US - west | Het datacenter waar de web-app wordt gehost. |
| Grootte | Gratis | De [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) waarmee wordt bepaald hosting-functies. |

Selecteer **OK**.

### <a name="create-and-publish-the-web-app"></a>De web-app maken en publiceren

In **Appnaam**, voer een unieke app-naam (geldige tekens zijn `a-z`, `0-9`, en `-`), of accepteer de automatisch gegenereerde unieke naam. De URL van de web-app is `https://<app_name>.azurewebsites.net`, waarbij `<app_name>` de naam van uw app is.

Selecteer **Maken** om de Azure-resources te gaan maken.

Nadat de wizard is voltooid, wordt de ASP.NET Core web-app gepubliceerd naar Azure en vervolgens de app geopend in de standaardbrowser.

![Gepubliceerde ASP.NET-web-app in Azure](./media/spatial-anchors-azure/web-app-running-live.png)

De naam van de app die u hebt gebruikt in deze sectie wordt gebruikt als de URL-voorvoegsel in de indeling `https://<app_name>.azurewebsites.net`. Noteer deze URL omdat u hebt deze nodig.
