---
title: 'Quickstart: uw eerste Azure Container Instances-container maken met behulp van Azure Portal'
description: In deze snelstart gebruikt u de Azure-portal om een container in Azure Container Instances te implementeren
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: quickstart
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 6aa6fb27b2aa7c8b9614e5812fadc629b1e185f8
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076235"
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Snelstart: Uw eerste container maken in Azure Container Instances

Met Azure Container Instances kunt u gemakkelijk Docker-containers in Azure maken en beheren, zonder virtuele machines te hoeven inrichten of een service op een hoger niveau te moeten gebruiken. In deze snelstart maakt u een container in Azure Portal en publiceert u deze op internet via een FQDN (Fully Qualified Domain Name). Na het configureren van enkele instellingen, ziet u dit in uw browser:

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-portal-07]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account][azure-free-account] voordat u begint.

## <a name="create-a-container-instance"></a>Een containerinstantie maken

Selecteer **Een nieuwe resource maken** > **Containers** > **Container Instances**.

![Begin met het maken van een nieuwe containerinstantie in Azure Portal][aci-portal-01]

Voer de waarden voor de volgende tekstvakken in: **Containernaam**, **Containerinstallatiekopie** en **Resourcegroep**. Laat de andere waarden op de standaardwaarden staan en selecteer vervolgens **OK**.

* Containernaam: `mycontainer`
* Containerinstallatiekopie: `microsoft/aci-helloworld`
* Resourcegroep: `myResourceGroup`

![Basisinstellingen configureren voor een nieuwe containerinstantie in Azure Portal][aci-portal-03]

U kunt zowel Windows- als Linux-containers maken in Azure Container Instances. Behoud voor deze snelstart de standaardinstelling voor **Linux** voor het implementeren van de Linux-gebaseerde `microsoft/aci-helloworld` installatiekopie.

Geef onder **configuratie** een **DNS-naamlabel** op voor uw container. De naam moet uniek zijn voor de Azure-regio waar u de containerinstallatiekopie maakt. De container zal openbaar bereikbaar zijn op `<dns-name-label>.<region>.azurecontainer.io`.

Laat de overige instellingen in **Configuratie** op de standaardwaarden staan. Klik vervolgens op **OK** om de configuratie te valideren.

![Een nieuwe containerinstantie configureren in Azure Portal][aci-portal-04]

Wanneer de validatie is voltooid, ziet u een overzicht van de containerinstellingen. Selecteer **OK** om de aanvraag voor de containerimplementatie te verzenden.

![Overzicht van de instellingen voor een nieuwe containerinstantie in Azure Portal][aci-portal-05]

Wanneer de implementatie is gestart, verschijnt er een tegel op uw portaldashboard waarop de voortgang van de implementatie wordt weergegeven. Zodra deze is geïmplementeerd, wordt uw nieuwe container-instantie in de tegel weergegeven.

![Voortgang van het maken van een nieuwe containerinstantie in Azure Portal][aci-portal-08]

Selecteer de container-instantie **mycontainer** om de eigenschappen ervan weer te geven. Noteer de **FQDN** (de FQDN-naam) van de container-instantie, evenals de **Status**.

![Overzicht van containergroepen in Azure Portal][aci-portal-06]

Wanneer de **Status** eenmaal *Uitvoeren* is, gaat u naar de FQDN van de container in uw browser.

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-portal-07]

Gefeliciteerd. Als u slechts enkele instellingen configureert, hebt u een openbaar toegankelijke toepassing in Azure Container Instances geïmplementeerd.

## <a name="view-container-logs"></a>Containerlogbestanden bekijken

Het weergeven van de logboeken voor een exemplaar van de container is handig bij het oplossen van problemen met de container of de toepassing die wordt uitgevoerd.

Selecteer om de container-logboeken weer te geven onder **INSTELLINGEN** de optie **Containers** en vervolgens **Logboeken**. De HTTP GET-aanvraag wordt als het goed is gegenereerd wanneer u de toepassing in uw browser hebt bekeken.

![Container-logboeken in Azure Portal][aci-portal-11]

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u met de container klaar bent, selecteert u **Overzicht** voor de container-instantie *mycontainer* en vervolgens **Verwijderen**.

![De containerinstantie verwijderen in de Azure-portal][aci-portal-09]

Selecteer **Ja** als het bevestigingsvenster verschijnt.

![Een containerinstantie in de Azure-portal verwijderen][aci-portal-10]

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een Azure-containerinstantie van een installatiekopie gemaakt in een openbare Docker Hub-opslagplaats. Als u zelf een container wilt bouwen en deze wilt implementeren in Azure Container Instances met behulp van Azure Container Registry, gaat u verder met de zelfstudie voor Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png
[aci-portal-11]: ./media/container-instances-quickstart-portal/qs-portal-11.png

<!-- LINKS - External -->
[azure-free-account]: https://azure.microsoft.com/free/