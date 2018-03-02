---
title: 'Quickstart: uw eerste Azure Container Instances-container maken met behulp van Azure Portal'
description: Azure Container Instances implementeren en ermee aan de slag gaan
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 01/02/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 63f22544276da07ec98e779cc524879603655db6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Uw eerste container maken in Azure Container Instances

Met Azure Container Instances kunt u eenvoudig containers maken en beheren in Azure. In deze quickstart maakt u een container in Azure en geeft u deze op internet weer via een openbaar IP-adres. Dit doet u met behulp van Azure Portal. Na slechts een paar muisklikken ziet u het volgende in de browser:

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-portal-07]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-container-instance"></a>Een containerinstantie maken

Selecteer **Een nieuwe resource maken** > **Containers** > **Azure Container Instances (preview)**.

![Begin met het maken van een nieuwe containerinstantie in Azure Portal][aci-portal-01]

Voer de waarden voor de volgende tekstvakken in: **Containernaam**, **Containerinstallatiekopie** en **Resourcegroep**. Laat de andere waarden op de standaardwaarden staan, en klik vervolgens op **OK**.

* Containernaam: `mycontainer`
* Containerinstallatiekopie: `microsoft/aci-helloworld`
* Resourcegroep: `myResourceGroup`

![Basisinstellingen configureren voor een nieuwe containerinstantie in Azure Portal][aci-portal-03]

U kunt zowel Windows- als Linux-containers maken in Azure Container Instances. In deze quickstart laten we de standaardinstelling van **Linux** staan, aangezien we in de vorige stap een Linux-container (`microsoft/aci-helloworld`) hebben opgegeven.

Laat voor de overige instellingen in **Configuratie** de standaardwaarden staan. Klik vervolgens op **OK** om de configuratie te valideren.

![Een nieuwe containerinstantie configureren in Azure Portal][aci-portal-04]

Wanneer de validatie is voltooid, ziet u een overzicht van de containerinstellingen. Selecteer **OK** om de aanvraag voor de containerimplementatie te verzenden.

![Overzicht van de instellingen voor een nieuwe containerinstantie in Azure Portal][aci-portal-05]

Wanneer de implementatie is gestart, verschijnt er een tegel op uw portaldashboard waarop de voortgang van de implementatie wordt weergegeven. Wanneer de implementatie is voltooid, wordt de tegel bijgewerkt met de nieuwe containergroep **mycontainer myc1**.

![Voortgang van het maken van een nieuwe containerinstantie in Azure Portal][aci-portal-08]

Selecteer de containergroep **mycontainer myc1** om de eigenschappen van de container weer te geven. Noteer het **IP-adres** van de containergroep, evenals de **STATUS** van de container.

![Overzicht van containergroepen in Azure Portal][aci-portal-06]

Zodra de container de status **Actief** heeft, gaat u naar het IP-adres dat u in de vorige stap hebt genoteerd, om de toepassing weer te geven die wordt gehost in de nieuwe container.

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-portal-07]

## <a name="delete-the-container"></a>De container verwijderen
Wanneer u klaar bent met de container, selecteert u de containergroep **mycontainer-myc1** en klikt u vervolgens op **Verwijderen**.

![De containerinstantie verwijderen in de Azure-portal][aci-portal-09]

Dit start een bevestigingsdialoogvenster, selecteer **Ja** wanneer hierom wordt gevraagd.

![Een containerinstantie in de Azure-portal verwijderen][aci-portal-10]

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-02]: ./media/container-instances-quickstart-portal/qs-portal-02.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een Azure-containerinstantie van een installatiekopie gemaakt in een openbare Docker Hub-opslagplaats. Als u zelf een container wilt bouwen en deze wilt implementeren in Azure Container Instances met behulp van Azure Container Registry, gaat u verder met de zelfstudie voor Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)