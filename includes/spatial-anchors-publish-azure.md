---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 3ee9aca1ee86b04998e357875b98b14782b3e56d
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56963377"
---
### <a name="launch-the-publish-wizard"></a>Start de publicatiewizard

Klik in **Solution Explorer** met de rechtermuisknop op het project **SharingService** en selecteer **Publiceren**.

De publicatiewizard wordt automatisch gestart. Selecteer **App Service** > **Publish** om het dialoogvenster **Create App Service** te openen.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Klik in het dialoogvenster **App Service maken** op **Een account toevoegen** en meld u vervolgens aan bij uw Azure-abonnement. Als u al bent aangemeld, selecteert u het account dat u wilt in de vervolgkeuzelijst.

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

Gebruik in het dialoogvenster **Hostingabonnement configureren** de instellingen uit de tabel.

| Instelling | Voorgestelde waarde | Beschrijving |
|-|-|-|
|App Service-plan| MySharingServicePlan | De naam van het App Service-plan. |
| Locatie | US - west | Het datacenter waar de web-app wordt gehost. |
| Grootte | Gratis | De [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bepaalt de hosting-functies. |

Selecteer **OK**.

### <a name="create-and-publish-the-web-app"></a>De web-app maken en publiceren

Voer bij **App-naam** een unieke appnaam in (geldige tekens zijn `a-z`, `0-9` en `-`) of accepteer de automatisch gegenereerde unieke naam. De URL van de web-app is `https://<app_name>.azurewebsites.net`, waarbij `<app_name>` de naam van uw app is.

Selecteer **Maken** om de Azure-resources te gaan maken.

Zodra de wizard is voltooid, wordt de ASP.NET Core-web-app naar Azure gepubliceerd. Daarna wordt de app gestart in de standaardbrowser.

![Gepubliceerde ASP.NET-web-app in Azure](./media/spatial-anchors-azure/web-app-running-live.png)

De naam van de app die is opgegeven in de [stap voor maken en publiceren](#create-and-publish-the-web-app), wordt gebruikt als URL-voorvoegsel in de indeling `https://<app_name>.azurewebsites.net`. Noteer de URL. U hebt deze later nodig.
