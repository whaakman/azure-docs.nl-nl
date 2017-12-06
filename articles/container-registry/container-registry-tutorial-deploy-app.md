---
title: Zelfstudie voor Azure Container register - web-app uit het register van Azure-Container implementeren
description: Implementeer een Linux-gebaseerde web-app met de installatiekopie van een container van een Azure container geogerepliceerde-register. Deel 2 van een reeks drie delen.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: d775a17cb8069a7521788d850d7d52b92cc67526
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-web-app-from-azure-container-registry"></a>Web-app uit het register van Azure-Container implementeren

Dit maakt deel uit twee in een zelfstudie reeks drie delen. In [deel uitmaken van een](container-registry-tutorial-prepare-registry.md), een container privé, geogerepliceerde-register is gemaakt en een installatiekopie van een container is gebouwd op basis van de bron en naar het register gepusht. In dit artikel kunt u de container in twee exemplaren van de Web-App in twee verschillende Azure-regio's om te profiteren van het netwerk wordt gesloten aspect van het register geogerepliceerde implementeert.

In deze zelfstudie maakt deel uit van twee in de reeks:

> [!div class="checklist"]
> * Implementeren van een installatiekopie van een container op twee *Web-Apps voor Containers* exemplaren
> * Controleer of de geïmplementeerde toepassing

Als u een register geogerepliceerde nog niet hebt gemaakt en de installatiekopie van de beperkte voorbeeldtoepassing met het register gepusht terug naar de vorige zelfstudie in de reeks [voorbereiden van een Azure container geogerepliceerde register](container-registry-tutorial-prepare-registry.md).

In het volgende gedeelte van de reeks, kunt u de toepassing bijwerken en vervolgens een nieuwe container installatiekopie push naar het register. Ten slotte bladeren u naar elk uitgevoerd exemplaar van de Web-App om te zien van de wijziging automatisch doorgevoerd in beide wordt weergegeven met Azure Container register geo-replicatie en webhooks in te grijpen.

## <a name="automatic-deployment-to-web-apps-for-containers"></a>Automatische implementatie voor Web-Apps voor Containers

Azure Container register biedt ondersteuning voor het implementeren van beperkte toepassingen rechtstreeks naar [Web-Apps voor Containers](../app-service/containers/index.yml). In deze zelfstudie kunt u de Azure portal gebruiken om de installatiekopie van de container gemaakt in de vorige zelfstudie voor twee web app-serviceabonnementen zich in verschillende Azure-regio's te implementeren.

Wanneer u een web-app uit een container installatiekopie in het register implementeert en hebt u een register geogerepliceerde in dezelfde regio, Azure Container register maakt de implementatie van een installatiekopie [webhook](container-registry-webhook.md) voor u. Wanneer u een nieuwe installatiekopie naar uw opslagplaats container pushen, wordt de webhook neemt over de wijziging en automatisch implementeert de installatiekopie van het nieuwe container in uw web-app.

## <a name="deploy-a-web-app-for-containers-instance"></a>Een Web-App voor exemplaar van de Containers implementeren

In deze stap maakt u een Web-App voor het exemplaar van de Containers in de *VS-West* regio.

Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar het register die u in de vorige zelfstudie hebt gemaakt.

Selecteer **opslagplaatsen** > **acr helloworld**, klik met de rechtermuisknop op de **v1** tag onder **labels** en selecteer **Implementeren in de web-app**.

![Implementeren in app service in de Azure portal][deploy-app-portal-01]

Onder **Web-App voor Containers** die wordt weergegeven, geeft u de volgende waarden voor elke instelling:

| Instelling | Waarde |
|---|---|
| **Sitenaam** | Een globaal unieke naam voor de web-app. In dit voorbeeld gebruiken we de indeling `<acrName>-westus` eenvoudig identificeren van het register en de regio van de web-app is geïmplementeerd. |
| **Resourcegroep** | **Gebruik bestaande** > `myResourceGroup` |
| **App service-plan/locatie** | Maak een nieuw plan met de naam `plan-westus` in de **VS-West** regio. |
| **Afbeelding** | `acr-helloworld:v1`

Selecteer **maken** voor het inrichten van de web-app voor de *VS-West* regio.

![Web-app op Linux-configuratie in de Azure portal][deploy-app-portal-02]

## <a name="view-the-deployed-web-app"></a>De geïmplementeerde web-app weergeven

Wanneer de implementatie is voltooid, kunt u de actieve toepassing weergeven door te navigeren naar de URL in uw browser.

Selecteer in de portal **App Services**, en vervolgens de web-app die u hebt ingericht in de vorige stap. In dit voorbeeld wordt de web-app heet *uniqueregistryname westus*.

Selecteer de hyperlink URL van de web-app in de rechterbovenhoek van de **App Service** overzicht weergeven van de actieve toepassing in uw browser.

![Web-app op Linux-configuratie in de Azure portal][deploy-app-portal-04]

Zodra de Docker-installatiekopie uit het register geogerepliceerde container is geïmplementeerd, wordt een installatiekopie van de Azure-regio die als host fungeert voor het register van de container die in de site weergegeven.

![Geïmplementeerde webtoepassing weergegeven in een browser][deployed-app-westus]

## <a name="deploy-second-web-app-for-containers-instance"></a>Tweede Web-App voor exemplaar van de Containers implementeren

Gebruik de procedure die in de vorige sectie zijn beschreven voor het implementeren van een tweede web-app voor de *VS-Oost* regio. Onder **Web-App voor Containers**, geef de volgende waarden:

| Instelling | Waarde |
|---|---|
| **Sitenaam** | Een globaal unieke naam voor de web-app. In dit voorbeeld gebruiken we de indeling `<acrName>-eastus` eenvoudig identificeren van het register en de regio van de web-app is geïmplementeerd. |
| **Resourcegroep** | **Gebruik bestaande** > `myResourceGroup` |
| **App service-plan/locatie** | Maak een nieuw plan met de naam `plan-eastus` in de **VS-Oost** regio. |
| **Afbeelding** | `acr-helloworld:v1`

Selecteer **maken** voor het inrichten van de web-app voor de *VS-Oost* regio.

![Web-app op Linux-configuratie in de Azure portal][deploy-app-portal-06]

## <a name="view-the-deployed-web-app"></a>De geïmplementeerde web-app weergeven

Als voordat, u kunt de actieve toepassing weergeven door te navigeren naar de URL in uw browser.

Selecteer in de portal **App Services**, en vervolgens de web-app die u hebt ingericht in de vorige stap. In dit voorbeeld wordt de web-app heet *uniqueregistryname eastus*.

Selecteer de hyperlink URL van de web-app in de rechterbovenhoek van de **App Service-overzicht** om weer te geven van de actieve toepassing in uw browser.

![Web-app op Linux-configuratie in de Azure portal][deploy-app-portal-07]

Zodra de Docker-installatiekopie uit het register geogerepliceerde container is geïmplementeerd, wordt een installatiekopie van de Azure-regio die als host fungeert voor het register van de container die in de site weergegeven.

![Geïmplementeerde webtoepassing weergegeven in een browser][deployed-app-eastus]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u twee Web-App voor exemplaren van de Containers uit het register van een Azure container geogerepliceerde geïmplementeerd. Met de volgende stappen in deze zelfstudie maakt u:

> [!div class="checklist"]
> * Een installatiekopie van een container geïmplementeerd op twee *Web-Apps voor Containers* exemplaren
> * De geïmplementeerde toepassing geverifieerd

Ga naar de volgende zelfstudie bijwerken en vervolgens de installatiekopie van een nieuwe container in het register van de container te implementeren en klik vervolgens controleren of de web-apps met in beide regio's zijn automatisch bijgewerkt.

> [!div class="nextstepaction"]
> [Een update implementeert op de installatiekopie van de container geogerepliceerde](./container-registry-tutorial-deploy-update.md)

<!-- IMAGES -->
[deploy-app-portal-01]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-01.png
[deploy-app-portal-02]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-02.png
[deploy-app-portal-03]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-03.png
[deploy-app-portal-04]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-04.png
[deploy-app-portal-05]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-05.png
[deploy-app-portal-06]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-06.png
[deploy-app-portal-07]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-07.png
[deployed-app-westus]: ./media/container-registry-tutorial-deploy-app/deployed-app-westus.png
[deployed-app-eastus]: ./media/container-registry-tutorial-deploy-app/deployed-app-eastus.png