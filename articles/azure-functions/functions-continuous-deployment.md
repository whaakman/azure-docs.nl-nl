---
title: Continue implementatie voor Azure Functions | Microsoft Docs
description: De functies voor continue implementatie van Azure App Service gebruiken voor het publiceren van uw functies.
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
ms.openlocfilehash: dd1605aa2f5fc9e0b4bc790bae2a1c20cb3ea048
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594540"
---
# <a name="continuous-deployment-for-azure-functions"></a>Doorlopende implementatie voor Azure Functions

U kunt Azure Functions gebruiken voor uw code continu implementeren met behulp van [integratie van broncodebeheer](functions-deployment-technologies.md#source-control). Integratie van broncodebeheer kunt een werkstroom waarin een code-update-implementatie naar Azure activeert. Als u geen ervaring met Azure Functions, aan de slag aan de hand van de [overzicht van Azure Functions](functions-overview.md).

Continue implementatie is een goede optie voor projecten waar het integreren van meerdere en regelmatige bijdragen. Wanneer u continue implementatie, kunt u een één betrouwbare bron behouden voor uw code, waarmee teams kunnen samenwerken. Continue implementatie kunt u configureren in Azure Functions in de volgende locaties van de bron-code:

* [Azure Repos](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

De eenheid van de implementatie voor functies in Azure is de functie-app. Alle functies in een functie-app worden geïmplementeerd op hetzelfde moment. Nadat u continue implementatie ingeschakeld, toegang tot de functiecode in de Azure-portal is geconfigureerd als *alleen-lezen* omdat de bron van betrouwbare is ingesteld op het ergens anders zijn.

## <a name="requirements-for-continuous-deployment"></a>Vereisten voor continue implementatie

Voor continue implementatie is voltooid, moet de directorystructuur van uw compatibel zijn met de basic mapstructuur die Azure Functions wordt verwacht.

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="credentials"></a>Continue implementatie instellen

Deze stappen voor het configureren van continue implementatie voor een bestaande functie-app. De stappen laten zien dat integratie met een GitHub-opslagplaats, maar gelijksoortige stappen gelden voor Azure-opslagplaatsen of andere broncodeopslagplaatsen.

1. In uw functie-app in de [Azure-portal](https://portal.azure.com), selecteer **platformfuncties** > **Implementatiecentrum**.

    ![Implementatie-Center openen](./media/functions-continuous-deployment/platform-features.png)

2. In **Implementatiecentrum**, selecteer **GitHub**, en selecteer vervolgens **autoriseren**. Als u hebt al GitHub gemachtigd, selecteert u **doorgaan**. 

    ![Azure App Service-implementatie Center](./media/functions-continuous-deployment/github.png)

3. Selecteer in GitHub, de **AzureAppService autoriseren** knop. 

    ![Azure App Service autoriseren](./media/functions-continuous-deployment/authorize.png)
    
    In **Implementatiecentrum** in Azure portal, selecteert u **doorgaan**.

4. Selecteer een van de volgende build-providers:

    * **App Service-service bouwen**: Aanbevolen wanneer u een build of als u een algemene build.
    * **Azure pijplijnen (Preview)** : Beste wordt weergegeven wanneer u moet meer controle over de build. Deze provider is momenteel in preview.

    ![Selecteer een build-provider](./media/functions-continuous-deployment/build.png)

5. Gegevens die specifiek zijn voor de optie voor het beheer van bron die u opgegeven configureren. Voor GitHub, moet u opgeven of Selecteer waarden voor **organisatie**, **opslagplaats**, en **vertakking**. De waarden zijn gebaseerd op de locatie van uw code. Selecteer **doorgaan**.

    ![Configureren van GitHub](./media/functions-continuous-deployment/github-specifics.png)

6. Alle gedetailleerde gegevens bekijken en selecteer vervolgens **voltooien** om de implementatieconfiguratie van uw te voltooien.

    ![Samenvatting](./media/functions-continuous-deployment/summary.png)

Wanneer het proces is voltooid, worden alle code uit de opgegeven bron is geïmplementeerd in uw app. Wijzigingen in de implementatiebron activeren op dat moment een implementatie van deze wijzigingen aan uw functie-app in Azure.

## <a name="deployment-scenarios"></a>Implementatiescenario's

<a name="existing"></a>

### <a name="move-existing-functions-to-continuous-deployment"></a>Verplaatsen van bestaande functies doorlopende implementatie

Als u functies al hebt geschreven de [Azure-portal](https://portal.azure.com) en u wilt de inhoud van uw app te downloaden voordat u naar continue implementatie overschakelt gaat u naar de **overzicht** tabblad van de functie-app. Selecteer de **app-inhoud downloaden** knop.

![App-inhoud downloaden](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> Nadat u continue integratie configureren, kunt u de bronbestanden in de Functions-portal niet meer bewerken.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
