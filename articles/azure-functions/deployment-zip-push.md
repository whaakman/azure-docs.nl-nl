---
title: ZIP-push-implementatie voor Azure Functions | Microsoft Docs
description: De faciliteiten ZIP-bestand implementatie van de Kudu deployment-service gebruiken voor het publiceren van uw Azure-functies.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/06/2017
ms.author: glenga
ms.openlocfilehash: 5c8c608126f20aa5f5dc52bb8e24cfec14fd00f5
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="zip-push-deployment-for-azure-functions"></a>Het ZIP-push-implementatie voor Azure Functions 
In dit onderwerp wordt beschreven hoe de functie app-project-bestanden implementeren in Azure uit een ZIP-bestand voor (gecomprimeerd). Leert u hoe voor een push-implementatie met behulp van de Azure CLI en de REST API's. Azure Functions heeft het volledige bereik van continue implementatie en integratie-opties die zijn opgegeven door de Azure App Service voor meer informatie Zie [continue implementatie voor Azure Functions](functions-continuous-deployment.md). Voor snellere iteratie tijdens de ontwikkeling is het echter vaak eenvoudiger voor het implementeren van de functie app-project-bestanden rechtstreeks vanuit een gecomprimeerde ZIP-bestand.

Deze implementatie ZIP-bestand maakt gebruik van dezelfde Kudu service die bevoegdheden continue integratie gebaseerde implementaties, waaronder de volgende functionaliteit:

+ Verwijdering van bestanden vanuit een eerdere implementatie overgebleven.
+ Implementatie-aanpassingen, zoals met het uitvoeren van implementatiescripts.
+ Implementatielogboeken van de.
+ Synchroniseren van de functie triggers in een [verbruik plan](functions-scale.md) functie-app.

Zie voor meer informatie de [.zip push implementatie naslagonderwerp]. 

## <a name="requirements-of-the-deployment-zip-file"></a>Vereisten van het ZIP-bestand voor implementatie 
Het ZIP-bestand voor push-implementatie gebruikt u moet alle projectbestanden in uw app functie, inclusief uw functiecode bevat. 

>[!IMPORTANT]
> Wanneer u een ZIP-push-implementatie gebruikt, worden alle bestanden van een bestaande implementatie is niet gevonden in het ZIP-bestand van uw app functie verwijderd.  

### <a name="function-app-folder-structure"></a>De functie app-mapstructuur

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

### <a name="downloading-your-function-app-project"></a>De functie app-project downloaden

Wanneer u lokale ontwikkeling doet, is het eenvoudig te maken van een ZIP-bestand van de functie app-project-map op uw ontwikkelcomputer. Echter, u hebt ook gemaakt uw functies met behulp van de editor in de portal. De functie app-project downloaden via de portal: 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) en navigeer naar de functie-app.

2. In de **overzicht** tabblad **app-inhoud downloaden**, kies uw opties voor het downloaden en op **downloaden**.     

    ![De functie app-project downloaden](./media/deployment-zip-push/download-project.png)

Het gedownloade ZIP-bestand is in de juiste indeling opnieuw worden gepubliceerd naar uw functie-app met ZIP-push-implementatie.

GitHub kunt u ook een ZIP-bestand downloaden van een opslagplaats. Houd er rekening mee dat wanneer u een GitHub-opslagplaats als ZIP-bestand hebt gedownload, GitHub een extra map voegt niveau voor de vertakking, wat betekent dat u kunt geen implementeren het ZIP-bestand gedownload van GitHub is. Als u een GitHub-opslagplaats voor het onderhouden van uw app in de functie gebruikt, moet u [continue integratie](functions-continuous-deployment.md) om uw app te implementeren.  

## <a name="cli"></a>Implementeren met behulp van de Azure CLI

U kunt de Azure CLI gebruiken voor het activeren van een push-implementatie. Push een ZIP-bestand naar uw app functie implementeren met behulp van de [az functionapp implementatie bron config-zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) opdracht. Moet u Azure CLI versie 2.0.21 of hoger. Gebruik de `az --version` opdracht om te zien welke versie die u gebruikt.

Vervang in de volgende opdracht, de `<zip_file_path>` tijdelijke aanduiding met het pad naar de locatie van het ZIP-bestand. Vervang ook `<app_name>` met de unieke naam van de functie-app. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
Met deze opdracht projectbestanden van het gedownloade ZIP-bestand naar uw functie-app in Azure implementeert en de app opnieuw wordt opgestart. De lijst van implementaties voor deze functie-app wilt weergeven, moet u de REST API's gebruiken.

Wanneer u de Azure CLI op uw lokale computer `<zip_file_path>` is het pad naar het ZIP-bestand op uw computer. U kunt ook Azure CLI uitvoeren in de [Azure Cloud Shell](../cloud-shell/overview.md). Wanneer u Cloud-Shell, moet u eerst uw implementatie ZIP-bestand uploaden naar de Azure File-account die is gekoppeld aan uw Cloud-Shell. In dat geval `<zip_file_path>` de opslaglocatie die wordt gebruikt door uw Cloud-Shell-account is. Zie voor meer informatie [behouden bestanden in de Azure-Cloud-Shell](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Doorlopende implementatie voor Azure Functions](functions-continuous-deployment.md)

[.zip push implementatie naslagonderwerp]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
