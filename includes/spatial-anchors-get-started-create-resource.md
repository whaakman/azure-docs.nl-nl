---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 12/13/2018
ms.author: crtreasu
ms.openlocfilehash: 32f4545a45eda8acddd7c93cc4917dbadca9ad4d
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632828"
---
## <a name="create-a-spatial-anchors-resource"></a>Een Spatial Anchors-resource maken

Ga naar de <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

Selecteer in het linkernavigatievenster in Azure portal, **een resource maken**.

Gebruik het zoekvak om te zoeken naar **ruimtelijke ankers**.

   ![Zoeken naar Spatial Anchors](./media/spatial-anchors-get-started-create-resource/portal-search.png)

Selecteer **ruimtelijke ankers**. Selecteer in het dialoogvenster **maken**.

In de **ruimtelijke ankers Account** in het dialoogvenster:

- Voer een unieke resourcenaam, met behulp van reguliere alfanumerieke tekens.
- Selecteer het abonnement dat u wilt dat de resource te koppelen.
- Maak een resourcegroep door te selecteren **nieuw**. Geef het de naam **myResourceGroup** en selecteer **OK**.
      [!INCLUDE [resource group intro text](resource-group.md)]
- Selecteer een locatie (regio) waarin de resource.
- Selecteer **Nieuw** om te beginnen met het maken van de resource.

   ![Een resource maken](./media/spatial-anchors-get-started-create-resource/create-resource-form.png)

Nadat de resource is gemaakt, ziet u Azure Portal dat uw implementatie voltooid is. Klik op **Ga naar resource**.

![Implementatie voltooid](./media/spatial-anchors-get-started-create-resource/deployment-complete.png)

U kunt vervolgens de resource-eigenschappen weergeven. Kopiëren van de resource **Account-ID** waarde in een teksteditor, omdat u hebt deze later nodig.

   ![Resource-eigenschappen](./media/spatial-anchors-get-started-create-resource/view-resource-properties.png)

Onder **instellingen**, selecteer **sleutel**. Kopieer de **primaire sleutel** waarde in een teksteditor. Deze waarde is de `Account Key`. U hebt deze later nodig.

   ![Accountcode](./media/spatial-anchors-get-started-create-resource/view-account-key.png)
