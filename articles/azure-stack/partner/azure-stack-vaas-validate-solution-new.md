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
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 777609b89bc08cd61489d2c3a3669ec07ccbc372
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647003"
---
# <a name="validate-a-new-azure-stack-solution"></a>Een nieuwe Azure Stack-oplossing te valideren

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Meer informatie over het gebruik van de **oplossing validatie** werkstroom voor het certificeren van nieuwe oplossingen voor Azure Stack.

Een oplossing voor Azure Stack is een hardware stuklijst (BoM) die gezamenlijk tussen Microsoft en de partner overeengekomen is als u aan de certificeringsvereisten voor Windows Server-logo. Een oplossing moet worden recertified wanneer er een wijziging in de hardware stuklijst is. Voor meer informatie over wanneer opnieuw certificeren oplossingen, neem contact op met het team via [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

Als u wilt certificeren van uw oplossing, de werkstroom voor validatie van de oplossing twee keer worden uitgevoerd. Voer één keer voor de *minimaal* configuratie ondersteund. Uitvoeren als een tweede keer voor de *bewaartemperatuur* configuratie ondersteund. Microsoft verklaart de oplossing als beide configuraties slagen voor alle tests.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="create-a-solution-validation-workflow"></a>Een werkstroom voor validatie van de oplossing maken

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]
2. Selecteer **Start** op de **oplossing validaties** tegel.

    ![Werkstroom van de tegel validaties](media/tile_validation-solution.png)

3. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]
4. Selecteer de **oplossingsconfiguratie**.
    - **Minimale**: de oplossing is geconfigureerd met de minimale ondersteunde aantal knooppunten.
    - **Maximale**: de oplossing is geconfigureerd met de maximale ondersteunde aantal knooppunten.
5. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

    ![Oplossing validatie-informatie](media/workflow_validation-solution_info.png)

6. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Omgeving parameters kunnen niet worden gewijzigd na het maken van een werkstroom.

7. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
8. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    U wordt omgeleid naar de pagina overzicht van tests.

## <a name="execute-solution-validation-tests"></a>Oplossing validatietests uitvoeren

In de **oplossing tests Validatieoverzicht** pagina, ziet u een lijst van de tests voor het voltooien van de validatie is vereist.

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

![Validatietest van schema-oplossing](media/workflow_validation-solution_schedule-test.png)

## <a name="next-steps"></a>Volgende stappen

- [Controleren en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md)