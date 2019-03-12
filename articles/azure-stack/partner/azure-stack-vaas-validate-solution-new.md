---
title: Een nieuwe Azure Stack-oplossing te valideren | Microsoft Docs
description: Leer hoe u een nieuwe Azure Stack-oplossing met validatie als een Service te valideren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: fb7b98a5307606f7335e76ada42d583b975730f5
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57759660"
---
# <a name="validate-a-new-azure-stack-solution"></a>Een nieuwe Azure Stack-oplossing te valideren

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Meer informatie over het gebruik van de **oplossing validatie** werkstroom voor het certificeren van nieuwe oplossingen voor Azure Stack.

Een oplossing voor Azure Stack is een hardware stuklijst (BoM) die gezamenlijk tussen Microsoft en de partner overeengekomen is als u aan de certificeringsvereisten voor Windows Server-logo. Een oplossing moet worden recertified wanneer er een wijziging in de hardware stuklijst is. Voor meer informatie over wanneer opnieuw certificeren oplossingen, neem contact op met het team via [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

Om te certificeren van uw oplossing, de werkstroom van de oplossing validatie twee keer worden uitgevoerd. Voer één keer voor de *minimaal* configuratie ondersteund. Uitvoeren als een tweede keer voor de *bewaartemperatuur* configuratie ondersteund. Microsoft verklaart de oplossing als beide configuraties slagen voor alle tests.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="create-a-solution-validation-workflow"></a>Een werkstroom voor validatie van de oplossing maken

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Selecteer **Start** op de **oplossing validaties** tegel.

    ![Werkstroom van de tegel validaties](media/tile_validation-solution.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Selecteer de **oplossingsconfiguratie**.
    - **Minimale**: de oplossing is geconfigureerd met de minimale ondersteunde aantal knooppunten.
    - **Maximale**: de oplossing is geconfigureerd met de maximale ondersteunde aantal knooppunten.
6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

    ![Oplossing validatie-informatie](media/workflow_validation-solution_info.png)

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Omgeving parameters kunnen niet worden gewijzigd na het maken van een werkstroom.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    U wordt omgeleid naar de pagina overzicht van tests.

## <a name="run-solution-validation-tests"></a>Oplossing validatietests uit te voeren

In de **oplossing validatie test samenvatting** pagina, ziet u een lijst van de tests voor het voltooien van de validatie is vereist.

In de werkstromen validatie **planning** een test maakt gebruik van het niveau van de werkstroom algemene parameters die u hebt opgegeven tijdens het maken van de werkstroom (Zie [werkstroom algemene parameters voor Azure Stack-validatie als een Service](azure-stack-vaas-parameters.md)). Als een van de parameterwaarden test ongeldig geworden, moet u ze resupply volgens de instructies in [wijzigen werkstroomparameters](azure-stack-vaas-monitor-test.md#change-workflow-parameters).

> [!NOTE]
> Een bestaand exemplaar van een validatietest planning maakt een nieuw exemplaar in plaats van het oude-exemplaar in de portal. Logboeken voor het oude exemplaar worden bewaard, maar zijn niet toegankelijk is vanaf de portal.  
Wanneer een test is voltooid en de **planning** actie wordt uitgeschakeld.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](includes/azure-stack-vaas-workflow-step_select-agent.md)]

2. Selecteer de volgende tests uit:
    - Engine voor cloud-simulatie
    - COMPUTE operationele SDK-pakket
    - Test voor schijf-ID
    - KeyVault Extension SDK Operational Suite
    - KeyVault SDK Operational Suite
    - Network SDK Operational Suite
    - Storage-Account SDK operationele Suite

3. Selecteer **planning** in het contextmenu om een prompt voor het plannen van de test-exemplaar te openen.

4. Controleer de testparameters en selecteer vervolgens **indienen** voor het plannen van de test voor de uitvoering.

![Validatietest van schema-oplossing](media/workflow_validation-solution_schedule-test.png)

## <a name="next-steps"></a>Volgende stappen

- [Controleren en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md)