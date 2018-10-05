---
title: Azure portal gebruiken voor het implementeren van de catalogus-app service | Microsoft Docs
description: Gebruikers van beheerde toepassingen ziet u hoe u een catalogus-app via Azure portal implementeert.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 4d2e8b442f70ee791fe65a32402e5272eda3f209
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809775"
---
# <a name="deploy-service-catalog-app-through-azure-portal"></a>Catalogus-app service via Azure portal implementeren

In de [voorgaande snelstartgids](publish-managed-app-definition-quickstart.md), u de definitie van een beheerde toepassing hebt gepubliceerd. In deze snelstartgids hebt maken u een catalogus-app van deze definitie.

## <a name="create-service-catalog-app"></a>Catalogus-app maken

Gebruik de volgende stappen uit in de Azure-portal:

1. Selecteer **een resource maken**.

   ![Een resource maken](./media/deploy-service-catalog-quickstart/create-new.png)

1. Zoeken naar **beheerd een Servicecatalogustoepassing** en selecteert u deze uit de beschikbare opties.

   ![Een servicecatalogustoepassing zoeken](./media/deploy-service-catalog-quickstart/select-service-catalog.png)

1. Ziet u een beschrijving van de service beheerde toepassingen. Selecteer **Maken**.

   ![Selecteer Maken](./media/deploy-service-catalog-quickstart/create-service-catalog.png)

1. De portal bevat de definities van beheerde toepassing waartoe u toegang hebt. Selecteer in de beschikbare definities die u wilt implementeren. In deze snelstartgids gebruikt u de **beheerd Opslagaccount** definitie die u in de vorige snelstartgids hebt gemaakt. Selecteer **Maken**.

   ![Selecteer de definitie implementeren](./media/deploy-service-catalog-quickstart/select-definition.png)

1. Waarden opgeven voor de **basisbeginselen** tabblad. Selecteer het Azure-abonnement aan uw app service catalog om te implementeren. Maak een nieuwe resourcegroep met de naam **applicationGroup**. Selecteer een locatie voor uw app. Wanneer u klaar bent, selecteert u **OK**.

   ![Geef waarden op voor basic](./media/deploy-service-catalog-quickstart/provide-basics.png)

1. Geef een voorvoegsel voor de opslagaccountnaam. Selecteer het type van de storage-account te maken. Wanneer u klaar bent, selecteert u **OK**.

   ![Geef waarden op voor opslag](./media/deploy-service-catalog-quickstart/provide-storage.png)

1. Bekijk de samenvatting. Nadat de validatie is geslaagd, schakelt u **OK** om te beginnen met de implementatie.

   ![Overzicht weergeven](./media/deploy-service-catalog-quickstart/view-summary.png)

## <a name="view-results"></a>Resultaten weergeven

Nadat de service catalog-app is geïmplementeerd, hebt u twee nieuwe resourcegroepen. Een resourcegroep bevat de service-app-catalogus. De resourcegroep bevat de resources voor de service-app-catalogus.

1. Weergeven van de resourcegroep met de naam **applicationGroup** om te zien van de service-app-catalogus.

   ![Toepassing weergeven](./media/deploy-service-catalog-quickstart/view-managed-application.png)

1. Weergeven van de resourcegroep met de naam **applicationGroup {hash-tekens}** om te zien van de resources voor de service-app-catalogus.

   ![Resources weergeven](./media/deploy-service-catalog-quickstart/view-resources.png)

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het maken van de definitiebestanden voor een beheerde toepassing, [maken en publiceren van de definitie van een beheerde toepassing](publish-service-catalog-app.md).
* Zie voor de Azure CLI, [implementeren service catalogus-app met Azure CLI](./scripts/managed-application-cli-sample-create-application.md).
* Zie voor PowerShell, [implementeren service catalogus-app met PowerShell](./scripts/managed-application-poweshell-sample-create-application.md).