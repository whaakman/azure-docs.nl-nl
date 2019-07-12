---
title: Azure App Service-resources verplaatsen naar een nieuw abonnement of de resource-groep
description: Azure Resource Manager gebruiken voor het App Service-resources verplaatsen naar een nieuwe resourcegroep of abonnement.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: c1a09ff4c29a2fedfea2c165a95c042985b3c83a
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723569"
---
# <a name="move-guidance-for-app-service-resources"></a>Richtlijnen voor het App Service-resources verplaatsen

De stappen voor het verplaatsen van resources van App Service verschillen afhankelijk van of u de resources binnen een abonnement of naar een nieuw abonnement verplaatst.

## <a name="move-in-same-subscription"></a>In hetzelfde abonnement verplaatsen

Wanneer u een Web-App verplaatst _binnen hetzelfde abonnement_, u kunt het SSL-certificaten van derden niet verplaatsen. Echter, kunt u een Web-App naar de nieuwe resourcegroep verplaatsen zonder te verplaatsen van het certificaat van derden en de SSL-functionaliteit van uw app nog steeds werkt.

Als u verplaatsen van het SSL-certificaat met de Web-App wilt, volgt u deze stappen:

1. Het certificaat van derden verwijderen van de Web-App, maar Bewaar een kopie van uw certificaat
2. Verplaats de Web-App.
3. Upload het certificaat van derden naar de verplaatste Web-App.

## <a name="move-across-subscriptions"></a>Verplaatsen tussen abonnementen

Wanneer u een Web-App verplaatst _voor abonnementen_, gelden de volgende beperkingen:

- De doelresourcegroep mag geen bestaande App Service-resources. App Service-resources zijn onder andere:
    - Web Apps
    - App Service-abonnementen
    - Geüpload of geïmporteerd SSL-certificaten
    - App Service-omgevingen
- Alle App Service-resources in de resourcegroep, moeten tegelijk worden verplaatst.
- App Service-resources kunnen alleen worden verplaatst uit de resourcegroep waarin ze oorspronkelijk zijn gemaakt. Als een resource App Service is niet meer in de oorspronkelijke resourcegroep, het terug verplaatsen naar de oorspronkelijke resourcegroep. Verplaats de resource voor abonnementen.

Als u niet de oorspronkelijke resourcegroep niet meer weet, kunt u deze kunt vinden via diagnostische gegevens. Voor uw web-app, selecteer **vaststellen en oplossen van problemen met**. Selecteer **configuratie en beheer**.

![Schakel diagnostische gegevens](./media/app-service-move-limitations/select-diagnostics.png)

Selecteer **migratieopties**.

![Selecteer opties voor de migratie](./media/app-service-move-limitations/select-migration.png)

Selecteer de optie voor de aanbevolen stappen voor het verplaatsen van de web-app.

![Selecteer de aanbevolen stappen](./media/app-service-move-limitations/recommended-steps.png)

Ziet u de aanbevolen acties moet uitvoeren voordat het verplaatsen van de resources. De informatie bevat de oorspronkelijke resourcegroep voor de web-app.

![Aanbevelingen](./media/app-service-move-limitations/recommendations.png)

## <a name="move-app-service-certificate"></a>App Service Certificate verplaatsen

U kunt uw App Service Certificate verplaatsen naar een nieuwe resourcegroep of abonnement. Als uw App Service Certificate is gebonden aan een web-app, moet u enkele stappen uitvoeren voordat u de resources verplaatst naar een nieuw abonnement. De SSL-binding en persoonlijk certificaat van de web-app verwijderen voordat u de resources. Het App Service Certificate niet hoeft te worden verwijderd, alleen de persoonlijke certificaat in de web-app.

## <a name="move-support"></a>Ondersteuning verplaatsen

Om te bepalen welke resources van App Service kunnen worden verplaatst, ziet de ondersteuningstatus van de voor verplaatsen:

- [Microsoft.AppService](../move-support-resources.md#microsoftappservice)
- [Microsoft.CertificateRegistration](../move-support-resources.md#microsoftcertificateregistration)
- [Microsoft.DomainRegistration](../move-support-resources.md#microsoftdomainregistration)
- [Microsoft.Web](../move-support-resources.md#microsoftweb)

## <a name="next-steps"></a>Volgende stappen

Zie voor de opdrachten om resources te verplaatsen, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../resource-group-move-resources.md).
