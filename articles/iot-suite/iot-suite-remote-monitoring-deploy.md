---
title: De oplossing voor externe controle - Azure implementeren | Microsoft Docs
description: Deze zelfstudie laat zien hoe u de vooraf geconfigureerde oplossing voor externe controle van azureiotsuite.com inrichten.
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 12/12/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 768c32e30509c74a292b2355a249ec010af2cd1b
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution"></a>De vooraf geconfigureerde oplossing voor externe bewaking implementeren

In deze zelfstudie leert u hoe de vooraf geconfigureerde oplossing voor externe controle inricht. U implementeren de oplossing van azureiotsuite.com. U kunt ook de oplossing met behulp van de CLI voor meer informatie over deze optie Zie implementeren [implementeren van een vooraf geconfigureerde oplossing vanaf de opdrachtregel](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploy-a-pcs-from-the-command-line).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De vooraf geconfigureerde oplossing configureren
> * De vooraf geconfigureerde oplossing implementeren
> * Aanmelden bij de vooraf geconfigureerde oplossing

## <a name="prerequisites"></a>Vereisten

U hebt een actief Azure-abonnement nodig om deze zelfstudie te voltooien.

Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

## <a name="deploy-the-preconfigured-solution"></a>De vooraf geconfigureerde oplossing implementeren

Voordat u de vooraf geconfigureerde oplossing voor uw Azure-abonnement implementeert, moet u bepaalde configuratie-opties kiezen:

1. Meld u aan bij [azureiotsuite.com](https://www.azureiotsuite.com) met behulp van uw Azure accountreferenties en klikt u op  **+**  om een nieuwe oplossing te maken:

    ![Een nieuwe oplossing maken](media/iot-suite-remote-monitoring-deploy/createnewsolution.png)

1. Klik op **Selecteer** op de **externe controle preview** tegel.

    ![Kies externe controle](media/iot-suite-remote-monitoring-deploy/remotemonitoring.png)

1. Op de **oplossing voor externe controle maken** pagina, voert u een **oplossingsnaam** voor uw externe controle vooraf oplossing geconfigureerde.

1. Selecteer een **Basic** of **standaard** implementatie. Als u de oplossing voor meer informatie over hoe het werkt of een demonstratie uitvoeren, kiest u implementeert de **Basic** optie voor de kosten kunt minimaliseren.

1. Kies een **Java** of **.NET** als de taal. Alle microservices zijn beschikbaar als Java of .NET-implementaties.

1. Controleer de **oplossingsgegevens** Configuratiescherm voor meer informatie over de configuratie-opties.

1. Selecteer het **abonnement** dat en de **regio** die u wilt gebruiken voor het inrichten van de oplossing.

1. Klik op **Oplossing maken** om het inrichtingsproces te starten. Dit proces duurt gewoonlijk enkele minuten om uit te voeren:

    ![De details van oplossing voor externe controle](media/iot-suite-remote-monitoring-deploy/createform.png)

Zie voor informatie over probleemoplossing, [wat te doen wanneer een implementatie mislukt](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) in de GitHub-opslagplaats.

## <a name="sign-in-to-the-preconfigured-solution"></a>Aanmelden bij de vooraf geconfigureerde oplossing

Wanneer het inrichtingsproces voltooid is, kunt u zich aanmeldt bij uw vooraf geconfigureerde oplossing voor externe controle.

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
> * De vooraf geconfigureerde oplossing configureren
> * De vooraf geconfigureerde oplossing implementeren
> * Aanmelden bij de vooraf geconfigureerde oplossing

Nu dat u de oplossing voor externe controle hebt geïmplementeerd, wordt de volgende stap is het [verkennen van de mogelijkheden van het dashboard van de oplossing](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->