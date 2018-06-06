---
title: ZIP-push-implementatie voor Azure Functions | Microsoft Docs
description: De faciliteiten ZIP-bestand implementatie van de Kudu deployment-service gebruiken voor het publiceren van uw Azure-functies.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/29/2018
ms.author: glenga
ms.openlocfilehash: 91c16ad5a6bf8babffc0b83d801626932688631e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699951"
---
# <a name="zip-push-deployment-for-azure-functions"></a>Het ZIP-push-implementatie voor Azure Functions 
In dit artikel wordt beschreven hoe de functie app-project-bestanden implementeren in Azure uit een ZIP-bestand voor (gecomprimeerd). Leert u hoe voor een push-implementatie met behulp van Azure CLI zowel met de REST-API's. 

Azure Functions heeft het volledige bereik van continue implementatie en integratie-instellingen die worden geleverd door Azure App Service. Zie voor meer informatie [continue implementatie voor Azure Functions](functions-continuous-deployment.md). 

Voor snellere iteratie tijdens de ontwikkeling is het vaak eenvoudiger voor het implementeren van de functie app-project-bestanden rechtstreeks vanuit een gecomprimeerde ZIP-bestand. Deze ZIP-bestandsimplementatie gebruikmaakt van dezelfde Kudu service die bevoegdheden continue integratie gebaseerde implementaties, met inbegrip van:

+ Verwijdering van bestanden die zijn overgebleven van eerdere implementaties.
+ Implementatie-aanpassingen, zoals met het uitvoeren van implementatiescripts.
+ Implementatielogboeken van de.
+ Synchroniseren van de functie triggers in een [verbruik plan](functions-scale.md) functie-app.

Zie voor meer informatie de [.zip push implementatie verwijzing](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

## <a name="deployment-zip-file-requirements"></a>Vereisten voor de implementatie ZIP-bestand
Het ZIP-bestand dat u voor de push-implementatie gebruikt moet alle projectbestanden in uw app functie, inclusief uw functiecode bevatten. 

>[!IMPORTANT]
> Wanneer u een ZIP-push-implementatie gebruikt, worden alle bestanden van een bestaande implementatie die niet zijn gevonden in het ZIP-bestand van uw app functie verwijderd.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Een functie-app bevat alle bestanden en mappen in de `wwwroot` directory. De implementatie van een ZIP-bestand omvat de inhoud van de `wwwroot` directory, maar niet de map zelf.  

## <a name="download-your-function-app-files"></a>De functie app-bestanden downloaden

Wanneer u op een lokale computer ontwikkelt, is het eenvoudig te maken van een ZIP-bestand van de functie app-project-map op uw ontwikkelcomputer. 

Echter, u mogelijk hebt gemaakt uw functies met behulp van de editor in Azure portal. U kunt een bestaand functie app-project in een van de volgende manieren downloaden: 

+ **Vanuit de Azure-portal:** 

    1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en gaat u naar de functie-app.

    2. Op de **overzicht** tabblad **app-inhoud downloaden**. Selecteer uw opties voor het downloaden en selecteer vervolgens **downloaden**.     

        ![De functie app-project downloaden](./media/deployment-zip-push/download-project.png)

    Het gedownloade ZIP-bestand is in de juiste indeling opnieuw worden gepubliceerd naar uw app functie via .zip push-implementatie. Het downloaden van de portal kan ook toevoegen aan de benodigde bestanden voor uw app in de functie rechtstreeks openen in Visual Studio.

+ **Met REST API's:** 

    Gebruik de volgende implementatie API ophalen voor het downloaden van de bestanden van uw `<function_app>` project: 

        https://<function_app>.scm.azurewebsites.net/api/zip/site/wwwroot/

    Inclusief `/site/wwwroot/` zorgt ervoor dat het zipbestand bevat alleen de functie app-project-bestanden en niet de gehele site. Als u bent niet al aangemeld bij Azure, wordt u gevraagd om dit te doen. Merk op dat een bericht verzenden aanvragen naar de `api/zip/` API is discoraged voor de zip-implementatiemethode beschreven in dit onderwerp. 

U kunt ook een ZIP-bestand downloaden van een GitHub-opslagplaats. Houd er rekening mee dat wanneer u een GitHub-opslagplaats als ZIP-bestand hebt gedownload, GitHub een extra mapniveau voor de vertakking toegevoegd. Deze extra map niveau betekent dat u het ZIP-bestand als u niet implementeren van GitHub gedownload. Als u een GitHub-opslagplaats voor het onderhouden van de functie-app gebruikt, moet u [continue integratie](functions-continuous-deployment.md) om uw app te implementeren.  

## <a name="cli"></a>Implementeren met behulp van Azure CLI

U kunt Azure CLI gebruiken voor het activeren van een push-implementatie. Push een ZIP-bestand naar uw app functie implementeren met behulp van de [az functionapp implementatie bron config-zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) opdracht. Voor het gebruik van deze opdracht moet u Azure CLI versie 2.0.21 of hoger. Als u wilt zien welke Azure CLI versie die u gebruikt, gebruiken de `az --version` opdracht.

Vervang in de volgende opdracht, de `<zip_file_path>` tijdelijke aanduiding met het pad naar de locatie van het ZIP-bestand. Vervang ook `<app_name>` met de unieke naam van de functie-app. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
Met deze opdracht implementeert projectbestanden van het gedownloade ZIP-bestand naar uw functie-app in Azure. Deze wordt vervolgens de app opnieuw wordt opgestart. De lijst van implementaties voor deze functie-app wilt weergeven, moet u de REST API's gebruiken.

Wanneer u Azure CLI op uw lokale computer `<zip_file_path>` is het pad naar het ZIP-bestand op uw computer. U kunt ook Azure CLI uitvoeren in [Azure Cloud Shell](../cloud-shell/overview.md). Wanneer u de Shell Cloud gebruikt, moet u eerst uw implementatie ZIP-bestand uploaden naar de bestanden van de Azure-account die is gekoppeld aan uw Cloud-Shell. In dat geval `<zip_file_path>` is de locatie voor de opslag die gebruikmaakt van uw Cloud-Shell-account. Zie voor meer informatie [behouden bestanden in de Azure-Cloud-Shell](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Doorlopende implementatie voor Azure Functions](functions-continuous-deployment.md)

[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
