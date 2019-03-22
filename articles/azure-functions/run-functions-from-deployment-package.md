---
title: Voer uw Azure-functies uit een pakket | Microsoft Docs
description: De Azure Functions-runtime die uw functies worden uitgevoerd door het koppelen van een implementatie van pakket-bestand met de projectbestanden van uw functie-app hebben.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: glenga
ms.openlocfilehash: 57126c87879da9f99d224457433bbbd5f95ef021
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58336725"
---
# <a name="run-your-azure-functions-from-a-package-file"></a>Uw Azure Functions uitvoeren vanuit een pakketbestand

> [!NOTE]
> De functionaliteit die worden beschreven in dit artikel is niet beschikbaar voor functie-apps op Linux in een [App Service-plan](functions-scale.md#app-service-plan).

In Azure, kunt u uw functies direct vanuit een pakketbestand implementeren in uw functie-app uitvoeren. De andere mogelijkheid is het implementeren van uw bestanden in de `d:\home\site\wwwroot` map van uw functie-app.

Dit artikel beschrijft de voordelen van het uitvoeren van uw functies uit een pakket. U ziet ook hoe u deze functionaliteit in uw functie-app in te schakelen.

## <a name="benefits-of-running-from-a-package-file"></a>Voordelen van het uitvoeren van een pakketbestand
  
Er zijn verschillende voordelen bij het uitvoeren van een pakketbestand:

+ Vermindert het risico van kopiëren van bestanden het vergrendelen van problemen.
+ Kan worden geïmplementeerd voor een productie-app (met opnieuw opstarten).
+ U kunt er zeker van de bestanden die worden uitgevoerd in uw app.
+ Verbetert de prestaties van [Azure Resource Manager-implementaties](functions-infrastructure-as-code.md).
+ Kan de tijdstippen koude start, met name voor JavaScript-functies met grote npm-pakket structuren verminderen.

Zie voor meer informatie, [deze aankondiging](https://github.com/Azure/app-service-announcements/issues/84).

## <a name="enabling-functions-to-run-from-a-package"></a>Uitvoeren van een pakket-functies inschakelen

Als u wilt inschakelen voor de functie-app om uit te voeren van een pakket, toe te voegen een `WEBSITE_RUN_FROM_PACKAGE` instellen op de instellingen van uw functie-app. De `WEBSITE_RUN_FROM_PACKAGE` instelling kan een van de volgende waarden hebben:

| Value  | Description  |
|---------|---------|
| **`1`**  | Aanbevolen voor functie-apps die worden uitgevoerd op Windows. Uitvoeren van een pakketbestand in de `d:\home\data\SitePackages` map van uw functie-app. Als dit niet het [implementeren met zip implementeren](#integration-with-zip-deployment), deze optie is vereist voor de map hebben ook een bestand met de naam `packagename.txt`. Dit bestand bevat alleen de naam van het pakketbestand in de map, zonder een spatie. |
|**`<url>`**  | Locatie van een specifiek pakket-bestand dat u wilt uitvoeren. Wanneer u Blob-opslag gebruikt, moet u een privé-container met een [Shared Access Signature (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#attach-a-storage-account-by-using-a-shared-access-signature-sas) om in te schakelen de Functions-runtime voor de toegang tot het pakket. U kunt de [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) pakketbestanden uploaden naar uw Blob storage-account.         |

> [!CAUTION]
> Bij het uitvoeren van een functie-app voor Windows, de optie van de externe URL-slechter koude start prestaties levert. Wanneer uw functie-app implementeren op Windows, moet u instellen `WEBSITE_RUN_FROM_PACKAGE` naar `1` en publiceert met zip-implementatie.

Hieronder ziet u een functie-app die is geconfigureerd voor het uitvoeren van een ZIP-bestand die wordt gehost in Azure Blob-opslag:

![WEBSITE_RUN_FROM_ZIP app-instelling](./media/run-functions-from-deployment-package/run-from-zip-app-setting-portal.png)

> [!NOTE]
> Op dit moment worden alleen ZIP-pakketbestanden ondersteund.

## <a name="integration-with-zip-deployment"></a>Integratie met zip-implementatie

[ZIP-implementatie] [ Zip deployment for Azure Functions] is een functie van Azure App Service waarmee u uw functie-app-project te implementeren de `wwwroot` directory. Het project wordt geleverd als een ZIP-bestand voor implementatie. Dezelfde API's kunnen worden gebruikt voor het implementeren van uw pakket naar de `d:\home\data\SitePackages` map. Met de `WEBSITE_RUN_FROM_PACKAGE` waarde van de app-instelling van `1`, wordt het zip-implementatie API's Kopieer uw pakket naar de `d:\home\data\SitePackages` map in plaats van het uitpakken van de bestanden naar `d:\home\site\wwwroot`. Dit leidt ook tot de `packagename.txt` bestand. De functie-app vervolgens uit het pakket wordt uitgevoerd na het opnieuw opstarten, en `wwwroot` alleen-lezen. Zie voor meer informatie over de implementatie van zip [Zip-implementatie voor Azure Functions](deployment-zip-push.md).

## <a name="adding-the-websiterunfrompackage-setting"></a>De instelling WEBSITE_RUN_FROM_PACKAGE toe te voegen

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Doorlopende implementatie voor Azure Functions](functions-continuous-deployment.md)

[Zip deployment for Azure Functions]: deployment-zip-push.md
