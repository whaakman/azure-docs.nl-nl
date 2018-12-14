---
title: Azure-portal gebruiken om een beheerde toepassing te bewaken | Microsoft Docs
description: Laat zien hoe de Azure-portal gebruiken voor het bewaken van beschikbaarheid en waarschuwingen voor een beheerde toepassing.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 754d2a246a86585e9f05f8a070c51e158f73affd
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53342592"
---
# <a name="monitor-a-deployed-instance-of-a-managed-application"></a>Een geïmplementeerd exemplaar van een beheerde toepassing bewaken

Nadat u een beheerde toepassing naar uw Azure-abonnement hebt geïmplementeerd, kunt u de status van de toepassing wilt controleren. In dit artikel ziet u opties in de Azure-portal voor het controleren van de status. U kunt de beschikbaarheid van de resources in uw beheerde toepassing bewaken. U kunt ook instellen en waarschuwingen weergeven.

## <a name="view-resource-health"></a>Resourcestatus weergeven

1. Selecteer uw exemplaar van de beheerde toepassing.

   ![Selecteer de beheerde toepassing](./media/monitor-managed-application-portal/select-managed-application.png)

1. Selecteer **resourcestatus**.

   ![Selecteer resource health](./media/monitor-managed-application-portal/select-resource-health.png)

1. De beschikbaarheid van de resources in uw beheerde toepassing weergeven.

   ![Resourcestatus weergeven](./media/monitor-managed-application-portal/view-health.png)

## <a name="view-alerts"></a>Waarschuwingen weergeven

1. Selecteer **waarschuwingen**.

   ![Selecteer waarschuwingen](./media/monitor-managed-application-portal/select-alerts.png)

1. Als u regels voor waarschuwingen geconfigureerd hebt, ziet u informatie over waarschuwingen die zijn opgetreden.

   ![Waarschuwingen weergeven](./media/monitor-managed-application-portal/view-alerts.png)

1. Waarschuwingsregels toevoegen, selecteert u **+ nieuwe waarschuwingsregel**.

   ![Waarschuwing maken](./media/monitor-managed-application-portal/create-new-alert.png)

U kunt waarschuwingen maken voor uw exemplaar van de beheerde toepassing of de resources in de beheerde toepassing. Zie voor meer informatie over het maken van waarschuwingen [overzicht van waarschuwingen in Microsoft Azure](../azure-monitor/platform/alerts-overview.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor voorbeelden van beheerde toepassing [voorbeeldprojecten voor Azure beheerde toepassingen](sample-projects.md).
* Zie voor het implementeren van een beheerde toepassing [implementeren service catalogus-app via Azure portal](deploy-service-catalog-quickstart.md).