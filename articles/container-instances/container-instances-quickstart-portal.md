---
title: 'Snelstartgids: Een toepassing uitvoeren in Azure Container Instances - portal'
description: In deze snelstartgids gebruikt u Azure Portal om een Docker-containertoepassing te implementeren die moet worden uitgevoerd in een geïsoleerde container in Azure Container Instances
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 8f547977e544854e281e1c6be442607d55149e5e
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53190285"
---
# <a name="quickstart-run-a-container-application-in-azure-container-instances-in-the-azure-portal"></a>Snelstartgids: Een containertoepassing in Azure Container Instances uitvoeren in Azure Portal

Gebruik Azure Container Instances om Docker-containers in Azure snel en eenvoudig uit te voeren. U hoeft geen virtuele machines te implementeren en u hoeft geen volledig container-indelingsplatform zoals Kubernetes te gebruiken. In deze snelstart gebruikt u Azure Portal om een container te maken in Azure en maakt u de bijbehorende toepassing beschikbaar met een FQDN (Fully Qualified Domain Name). Na het configureren van een paar instellingen en de implementatie van de container kunt u bladeren naar de toepassing die wordt uitgevoerd:

![App die is geïmplementeerd in Azure Container Instances, weergegeven in de browser][aci-portal-07]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account][azure-free-account] voordat u begint.

## <a name="create-a-container-instance"></a>Een containerinstantie maken

Selecteer **Een nieuwe resource maken** > **Containers** > **Container Instances**.

![Begin met het maken van een nieuwe containerinstantie in Azure Portal][aci-portal-01]

Voer de waarden voor de volgende tekstvakken in: **Containernaam**, **Containerinstallatiekopie** en **Resourcegroep**. Laat de andere waarden op de standaardwaarden staan en selecteer vervolgens **OK**.

* Containernaam: `mycontainer`
* Containerinstallatiekopie: `microsoft/aci-helloworld`
* Resourcegroep: **Nieuwe maken** > `myResourceGroup`

![Basisinstellingen configureren voor een nieuwe containerinstantie in Azure Portal][aci-portal-03]

U kunt zowel Windows- als Linux-containers maken in Azure Container Instances. Behoud voor deze snelstart de standaardinstelling voor **Linux** voor het implementeren van de Linux-gebaseerde `microsoft/aci-helloworld` installatiekopie.

Geef onder **configuratie** een **DNS-naamlabel** op voor uw container. De naam moet uniek zijn voor de Azure-regio waar u de containerinstallatiekopie maakt. De container zal openbaar bereikbaar zijn op `<dns-name-label>.<region>.azurecontainer.io`.

Laat de overige instellingen in **Configuratie** op de standaardwaarden staan. Klik vervolgens op **OK** om de configuratie te valideren.

![Een nieuwe containerinstantie configureren in Azure Portal][aci-portal-04]

Wanneer de validatie is voltooid, ziet u een overzicht van de containerinstellingen. Selecteer **OK** om de aanvraag voor de containerimplementatie te verzenden.

![Overzicht van de instellingen voor een nieuwe containerinstantie in Azure Portal][aci-portal-05]

Wanneer de implementatie is gestart, wordt een melding weergegeven dat de implementatie wordt uitgevoerd. Er wordt nog een melding weergegeven wanneer de containergroep is geïmplementeerd.

![Voortgang van het maken van een nieuwe containerinstantie in Azure Portal][aci-portal-08]

Open het overzicht voor de containergroep door te navigeren naar **Resourcegroepen** > **myResourceGroup** > **mycontainer**. Noteer de **FQDN** (de FQDN-naam) van de container-instantie, evenals de **Status**.

![Overzicht van containergroepen in Azure Portal][aci-portal-06]

Wanneer de **Status** eenmaal *Uitvoeren* is, gaat u naar de FQDN van de container in uw browser.

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-portal-07]

Gefeliciteerd! Als u slechts enkele instellingen configureert, hebt u een openbaar toegankelijke toepassing in Azure Container Instances geïmplementeerd.

## <a name="view-container-logs"></a>Containerlogbestanden bekijken

Het weergeven van de logboeken voor een exemplaar van de container is handig bij het oplossen van problemen met de container of de toepassing die wordt uitgevoerd.

Selecteer om de container-logboeken weer te geven onder **Instellingen** de optie **Containers** en vervolgens **Logboeken**. De HTTP GET-aanvraag wordt als het goed is gegenereerd wanneer u de toepassing in uw browser hebt bekeken.

![Container-logboeken in Azure Portal][aci-portal-11]

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u met de container klaar bent, selecteert u **Overzicht** voor de container-instantie *mycontainer* en vervolgens **Verwijderen**.

![De containerinstantie verwijderen in de Azure-portal][aci-portal-09]

Selecteer **Ja** als het bevestigingsvenster verschijnt.

![Een containerinstantie in de Azure-portal verwijderen][aci-portal-10]

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een Azure-containerinstantie van een installatiekopie gemaakt in een openbare Docker Hub-opslagplaats. Als u zelf een containerinstallatiekopie wilt bouwen en deze wilt implementeren met behulp van een privé Azure Container-register, gaat u verder met de zelfstudie voor Azure Container Instances.

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