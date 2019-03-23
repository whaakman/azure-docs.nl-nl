---
title: Snelstartgids - Docker-container implementeren in Azure Container Instances - Portal
description: In deze snelstartgids gebruikt u de Azure portal om snel te implementeren een beperkte web-app die wordt uitgevoerd in een geïsoleerde Azure container-exemplaar
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: f4d232d4d6043ede3979db67e5cd35130d931bef
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369442"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-portal"></a>Quickstart: Een containerinstantie in Azure met behulp van de Azure-portal implementeren

Gebruik Azure Container Instances om uit te voeren zonder server Docker-containers in Azure met de eenvoud en snelheid. Een toepassing implementeren in een container-exemplaar op aanvraag wanneer u niet nodig voor een volledige container-orchestration-platform, zoals Azure Kubernetes Service hebt.

In deze snelstartgids gebruikt u de Azure-portal een geïsoleerde Docker-container implementeren en de toepassing beschikbaar met een volledig gekwalificeerde domeinnaam (FQDN). Na het configureren van een paar instellingen en de implementatie van de container kunt u bladeren naar de toepassing die wordt uitgevoerd:

![App die is geïmplementeerd in Azure Container Instances, weergegeven in de browser][aci-portal-07]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account][azure-free-account] voordat u begint.

## <a name="create-a-container-instance"></a>Een containerinstantie maken

Selecteer **Een nieuwe resource maken** > **Containers** > **Container Instances**.

![Begin met het maken van een nieuwe containerinstantie in Azure Portal][aci-portal-01]

Voer de waarden voor de volgende tekstvakken in: **Containernaam**, **Containerinstallatiekopie** en **Resourcegroep**. Laat de andere waarden op de standaardwaarden staan en selecteer vervolgens **OK**.

* Containernaam: `mycontainer`
* Containerinstallatiekopie: `mcr.microsoft.com/azuredocs/aci-helloworld`
* Resourcegroep: **Nieuwe maken** > `myResourceGroup`

![Basisinstellingen configureren voor een nieuwe containerinstantie in Azure Portal][aci-portal-03]

In deze Quick Start laat de standaardinstelling van **openbare** implementeren van de openbare Microsoft `aci-helloworld` installatiekopie. Deze installatiekopie bevat een kleine web-app die is geschreven in Node.js en die een statische HTML-pagina dient.

Geef onder **configuratie** een **DNS-naamlabel** op voor uw container. De naam moet uniek zijn binnen Azure-regio waarin u de containerinstantie maken. De container zal openbaar bereikbaar zijn op `<dns-name-label>.<region>.azurecontainer.io`. Als u een foutbericht 'DNS-naamlabel niet beschikbaar' ontvangt, probeert u een ander DNS-naamlabel.

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

In deze Quick Start, kunt u een Azure containerexemplaar gemaakt van een installatiekopie van de openbare Microsoft. Als u zelf een containerinstallatiekopie wilt bouwen en deze wilt implementeren met behulp van een privé Azure Container-register, gaat u verder met de zelfstudie voor Azure Container Instances.

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