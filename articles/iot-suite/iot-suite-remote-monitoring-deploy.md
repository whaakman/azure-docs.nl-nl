---
title: De oplossing voor externe controle - Azure implementeren | Microsoft Docs
description: Deze zelfstudie laat zien hoe u de externe controle oplossingsverbetering van azureiotsuite.com inrichten.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: eb78ce91755c43f1c6fedf62a70238df911b940f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-the-remote-monitoring-solution-accelerator"></a>De externe controle oplossingsverbetering implementeren

Deze zelfstudie laat zien hoe de oplossingsverbetering voor externe controle inrichten. U implementeren de oplossing van azureiotsuite.com. U kunt ook de oplossing met behulp van de CLI voor meer informatie over deze optie Zie implementeren [een oplossingsverbetering vanaf de opdrachtregel implementeren](iot-suite-remote-monitoring-deploy-cli.md).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De oplossingsverbetering configureren
> * De accelerator oplossing implementeren
> * Aanmelden bij de oplossingsverbetering

## <a name="prerequisites"></a>Vereisten

U hebt een actief Azure-abonnement nodig om deze zelfstudie te voltooien.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

## <a name="deploy-the-solution-accelerator"></a>De accelerator oplossing implementeren

Voordat u de oplossingsverbetering voor uw Azure-abonnement hebt geïmplementeerd, moet u bepaalde configuratie-opties kiezen:

1. Meld u aan bij [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) met behulp van de referenties van uw Azure-account.

1. Klik op **Probeer nu** op de **externe controle** tegel.

    ![Kies externe controle](media/iot-suite-remote-monitoring-deploy/remotemonitoring.png)

1. Op de **oplossing voor externe controle maken** pagina, voert u een **oplossingsnaam** voor uw externe controle oplossingsverbetering.

1. Selecteer een **Basic** of **standaard** implementatie. Als u de oplossing voor meer informatie over hoe het werkt of een demonstratie uitvoeren, kiest u implementeert de **Basic** optie voor de kosten kunt minimaliseren.

1. Kies een **Java** of **.NET** als de taal. Alle microservices zijn beschikbaar als Java of .NET-implementaties.

1. Controleer de **oplossingsgegevens** Configuratiescherm voor meer informatie over de configuratie-opties.

1. Selecteer het **abonnement** dat en de **regio** die u wilt gebruiken voor het inrichten van de oplossing.

1. Klik op **Oplossing maken** om het inrichtingsproces te starten. Dit proces duurt gewoonlijk enkele minuten om uit te voeren:

    ![Details van de oplossing externe controle](media/iot-suite-remote-monitoring-deploy/createform.png)

Zie voor informatie over probleemoplossing, [wat te doen wanneer een implementatie mislukt](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) in de GitHub-opslagplaats.

## <a name="sign-in-to-the-solution-accelerator"></a>Aanmelden bij de oplossingsverbetering

Wanneer het inrichtingsproces voltooid is, kunt u zich aanmeldt bij uw externe controle oplossingsverbetering.

1. Op de **ingerichte oplossingen** pagina, kiest u uw nieuwe oplossing voor externe controle:

    ![Kies nieuwe oplossing](media/iot-suite-remote-monitoring-deploy/choosenew.png)

1. U kunt informatie weergeven over uw oplossing voor externe controle in het deelvenster die wordt weergegeven. Kies **dashboard van de oplossing** verbinding maken met uw oplossing voor externe controle.

    > [!NOTE]
    > Wanneer u klaar bent, kunt u uw oplossing voor externe controle vanuit dit deelvenster verwijderen.

    ![Oplossing Configuratiescherm](media/iot-suite-remote-monitoring-deploy/solutionpanel.png)

1. Het dashboard externe controle oplossing wordt weergegeven in uw browser.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De oplossingsverbetering configureren
> * De accelerator oplossing implementeren
> * Aanmelden bij de oplossingsverbetering

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het dashboard van de oplossing](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->