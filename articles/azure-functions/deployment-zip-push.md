---
title: ZIP-push-implementatie voor Azure Functions | Microsoft Docs
description: De ZIP-bestand implementatie faciliteiten van de Kudu-deployment-service gebruiken voor het publiceren van uw Azure-functies.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
editor: ''
tags: ''
ms.service: functions
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2018
ms.author: glenga
ms.openlocfilehash: 06124a0a0db47d76552ddf2172a7f656c6d869cf
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818464"
---
# <a name="zip-deployment-for-azure-functions"></a>ZIP-implementatie voor Azure Functions

In dit artikel wordt beschreven hoe u de projectbestanden van uw functie-app implementeren in Azure uit (gecomprimeerd) ZIP-bestand. Leert u hoe u voor een push-implementatie zowel met behulp van Azure CLI en met behulp van de REST API's. [Azure Functions Core Tools](functions-run-local.md) ook deze implementatie van API's worden gebruikt bij het publiceren van een lokale project naar Azure.

Azure Functions heeft het volledige aanbod van continue integratie en implementatie-opties die worden geleverd door Azure App Service. Zie voor meer informatie, [continue implementatie voor Azure Functions](functions-continuous-deployment.md).

Als u wilt sneller ontwikkelen, wellicht vindt u het eenvoudiger te implementeren van uw functie app-project-bestanden rechtstreeks vanuit een ZIP-bestand. De implementatie van het ZIP-API wordt de inhoud van een ZIP-bestand en de inhoud in de `wwwroot` map van uw functie-app. De implementatie van dit ZIP-bestand gebruikt dezelfde Kudu-service die bevoegdheden continue integratie-implementaties op basis van, met inbegrip van:

+ Verwijdering van bestanden die zijn overgebleven van eerdere implementaties.
+ Implementatieaanpassing, inclusief het uitvoeren van implementatiescripts.
+ Logboeken van de implementatie.
+ Synchroniseren van de functie-triggers in een [verbruiksabonnement](functions-scale.md) functie-app.

Zie voor meer informatie de [.zip implementatie verwijzing](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

## <a name="deployment-zip-file-requirements"></a>Vereisten voor implementatie van ZIP-bestand

Het ZIP-bestand dat u voor de push-implementatie gebruikt moet alle bestanden die nodig zijn om uit te voeren van uw functie bevatten.

>[!IMPORTANT]
> Wanneer u een ZIP-implementatie gebruikt, worden alle bestanden van een bestaande implementatie die niet zijn gevonden in het ZIP-bestand verwijderd uit uw functie-app.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Een functie-app bevat alle bestanden en mappen in de `wwwroot` directory. De implementatie van een ZIP-bestand bevat de inhoud van de `wwwroot` directory, maar niet aan de map zelf. Wanneer u een C#-klassebibliotheekproject implementeert, moet u de gecompileerde dll-bestanden en afhankelijkheden in opnemen een `bin` submap in uw ZIP-pakket.

## <a name="download-your-function-app-files"></a>Uw functie-app-bestanden downloaden

Wanneer u op een lokale computer ontwikkelt, is het eenvoudig te maken van een ZIP-bestand van de projectmap van de functie-app op uw ontwikkelcomputer.

Echter, u mogelijk hebt gemaakt uw functies met behulp van de editor in Azure portal. U kunt een bestaande functie-app-project in een van de volgende manieren downloaden:

+ **Vanuit de Azure-portal:**

    1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en ga vervolgens naar uw functie-app.

    2. Op de **overzicht** tabblad **app-inhoud downloaden**. Selecteer de gewenste downloadopties en selecteer vervolgens **downloaden**.

        ![De functie-app-project downloaden](./media/deployment-zip-push/download-project.png)

    Het gedownloade ZIP-bestand is in de juiste indeling opnieuw worden gepubliceerd naar uw functie-app met behulp van ZIP-push-implementatie. Het downloaden van de portal kan ook de bestanden die nodig zijn voor uw functie-app rechtstreeks in Visual Studio openen toevoegen.

+ **Met behulp van REST-API's:**

    Gebruik de volgende implementatie API ophalen voor het downloaden van de bestanden van uw `<function_app>` project: 

        https://<function_app>.scm.azurewebsites.net/api/zip/site/wwwroot/

    Inclusief `/site/wwwroot/` zorgt ervoor dat uw zip-bestand bevat alleen de projectbestanden voor functie-app en niet de gehele site. Als u niet al bent aangemeld naar Azure, wordt u gevraagd om dit te doen.  

U kunt ook een ZIP-bestand downloaden vanuit een GitHub-opslagplaats. Wanneer u een GitHub-opslagplaats als ZIP-bestand hebt gedownload, wordt het mapniveau van een extra voor de vertakking toegevoegd in GitHub. Dit niveau betekent dat u het ZIP-bestand rechtstreeks als u niet implementeren van extra map gedownload vanuit GitHub. Als u een GitHub-opslagplaats te houden van uw functie-app gebruikt, moet u [continue integratie](functions-continuous-deployment.md) om uw app te implementeren.  

## <a name="cli"></a>Implementeren met behulp van Azure CLI

U kunt Azure CLI gebruiken voor het activeren van een push-implementatie. Push een ZIP-bestand naar uw functie-app implementeren met behulp van de [az functionapp deployment source config-zip](/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config-zip) opdracht. Voor het gebruik van deze opdracht moet u Azure CLI versie 2.0.21 of hoger. Als u wilt zien welke Azure CLI-versie die u gebruikt, gebruikt u de `az --version` opdracht.

In de volgende opdracht, vervangt u de `<zip_file_path>` tijdelijke aanduiding door het pad naar de locatie van het ZIP-bestand. Vervang ook `<app_name>` met de unieke naam van uw functie-app. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```

Deze opdracht wordt de projectbestanden van het gedownloade ZIP-bestand naar uw functie-app in Azure geïmplementeerd. Er wordt vervolgens de app opnieuw wordt opgestart. De lijst van de implementaties voor deze functie-app wilt weergeven, moet u de REST API's gebruiken.

Wanneer u met behulp van Azure CLI op uw lokale computer, `<zip_file_path>` is het pad naar het ZIP-bestand op uw computer. U kunt ook Azure CLI uitvoeren in [Azure Cloud Shell](../cloud-shell/overview.md). Als u Cloud Shell gebruikt, moet u eerst uw implementatie ZIP-bestand uploaden aan de Azure Files-account dat is gekoppeld aan uw Cloud Shell. In dat geval `<zip_file_path>` is van de opslaglocatie die gebruikmaakt van uw Cloud Shell-account. Zie voor meer informatie, [behouden bestanden in Azure Cloud Shell](../cloud-shell/persisting-shell-storage.md).

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

## <a name="run-functions-from-the-deployment-package"></a>Functies van het implementatiepakket uitvoeren

U kunt ook uw functies direct vanuit het pakketbestand van de implementatie uitgevoerd. Deze methode slaat de implementatiestap van het kopiëren van bestanden van het pakket naar de `wwwroot` map van uw functie-app. In plaats daarvan het pakketbestand is gekoppeld met de Functions-runtime en de inhoud van de `wwwroot` directory alleen-lezen.  

> [!NOTE]
> De mogelijkheid om uit te voeren van uw functie-app van het implementatiepakket is in preview.

ZIP-implementatie kan worden geïntegreerd met deze functie, die u inschakelen kunt door in te stellen van de functie app-instelling `WEBSITE_RUN_FROM_ZIP` op een waarde van `1`. Zie voor meer informatie, [uw functies worden uitgevoerd vanuit een pakketbestand implementatie](run-functions-from-deployment-package.md).

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Doorlopende implementatie voor Azure Functions](functions-continuous-deployment.md)

[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
