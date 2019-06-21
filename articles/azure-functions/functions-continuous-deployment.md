---
title: Continue implementatie voor Azure Functions | Microsoft Docs
description: De faciliteiten continue implementatie van Azure App Service gebruiken voor het publiceren van uw functies.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: 2dc5fab0966c2ead0276cd8b23ea764bd4f9ef59
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190482"
---
# <a name="continuous-deployment-for-azure-functions"></a>Doorlopende implementatie voor Azure Functions

Azure Functions kunt u uw code continu via implementeren [integratie van broncodebeheer](functions-deployment-technologies.md#source-control). Hierdoor is een werkstroom waar code trigger updates implementeren naar Azure. Als u geen ervaring met Azure Functions, aan de slag met de [overzicht van Azure Functions](functions-overview.md).

Continue implementatie is een goede optie voor projecten waar u een integratie uitvoert meerdere en regelmatige bijdragen. Ook kunt u een één betrouwbare bron voor uw functiecode te onderhouden. Continue implementatie kunt u configureren in Azure Functions in de volgende locaties van de bron-code:

* [Azure Repos](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

De eenheid van de implementatie voor Azure functions is de functie-app. Dit betekent dat alle functies in een functie-app worden geïmplementeerd op hetzelfde moment. Nadat u continue implementatie is ingeschakeld, toegang tot de functiecode in de Azure-portal is geconfigureerd als *alleen-lezen*, omdat de bron van betrouwbare is ingesteld op het ergens anders zijn.

## <a name="requirements-for-continuous-deployment"></a>Vereisten voor continue implementatie

Voor continue implementatie is voltooid, moet de directorystructuur van uw compatibel zijn met de volgende eenvoudige mapstructuur die Azure Functions wordt verwacht dat:

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="credentials"></a>Continue implementatie instellen

Gebruik deze procedure om continue implementatie voor een bestaande functie-app te configureren. Deze stappen laten zien dat integratie met een GitHub-opslagplaats, maar gelijksoortige stappen gelden voor Azure-opslagplaatsen of andere broncodeopslagplaatsen.

1. In uw functie-app in de [Azure-portal](https://portal.azure.com), selecteer **platformfuncties** > **Implementatiecentrum**.

    ![De implementatiecenter openen](./media/functions-continuous-deployment/platform-features.png)

2. In de **Implementatiecentrum**, selecteer **GitHub**, en selecteer vervolgens **autoriseren**. Of, als u hebt al GitHub gemachtigd, selecteert u **doorgaan**. 

    ![Implementatiecenter](./media/functions-continuous-deployment/github.png)

3. Selecteer in GitHub, **autoriseren AzureAppService**. 

    ![Autoriseren](./media/functions-continuous-deployment/authorize.png)
    
    In de Azure-portal **Implementatiecentrum**, selecteer **doorgaan**.

4. Selecteer een van de volgende build-providers:

    * **App Service-service bouwen** - aanbevolen wanneer u een build of als u een algemene build.
    * **Azure pijplijnen (Preview)** - aanbevolen wanneer u meer controle over de build nodig hebt. Deze provider is momenteel in preview.

    ![Een build-provider selecteren](./media/functions-continuous-deployment/build.png)

5. Gegevens die specifiek zijn voor de optie voor het beheer van bron die u opgegeven configureren. Voor GitHub, moet u de **organisatie**, **opslagplaats**, en **vertakking** waar uw code wordt toegevoegd. Selecteer **doorgaan**.

    ![Configureren van GitHub](./media/functions-continuous-deployment/github-specifics.png)

6. Ten slotte alle gedetailleerde gegevens bekijken en selecteer **voltooien** om de implementatieconfiguratie van uw te voltooien.

    ![Samenvatting](./media/functions-continuous-deployment/summary.png)

Wanneer het proces is voltooid, worden alle code uit de opgegeven bron is geïmplementeerd in uw app. Wijzigingen in de implementatiebron activeren op dat moment een implementatie van deze wijzigingen aan uw functie-app in Azure.

## <a name="deployment-scenarios"></a>Implementatiescenario's

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Verplaatsen van bestaande functies doorlopende implementatie

Als u functies al hebt geschreven de [Azure-portal](https://portal.azure.com) en wilt downloaden van de inhoud van uw app voordat u overschakelt naar continue implementatie, moet u navigeren naar de **overzicht** tabblad van de functie App en klik op de **app-inhoud downloaden** knop.

![App-inhoud downloaden](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> Nadat u continue integratie configureren, kunt u de bronbestanden in de Functions-portal niet meer bewerken.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
