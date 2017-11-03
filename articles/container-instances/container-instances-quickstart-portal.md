---
title: Quick Start - maken van uw eerste Azure-Containerexemplaren-container met de Azure portal
description: Azure Container Instances implementeren en ermee aan de slag gaan
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 0179107ece1e150246ab40836783d810425be3ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Uw eerste container maken in Azure Container Instances

Met Azure Container Instances kunt u eenvoudig containers maken en beheren in Azure. In deze snelstartgids een container maken in Azure en tijdens het blootgesteld aan internet met een openbaar IP-adres. Deze bewerking is voltooid met behulp van de Azure-portal. Met een paar muisklikken ziet u dit in uw browser:

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-app-browser]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-container-instance"></a>Maak een exemplaar van de container

Selecteer de **nieuw** > **Containers** > **exemplaren van Azure-Container (preview)**.

![Begin met het maken van een nieuw exemplaar van de container in Azure portal][aci-portal-01]

Voer de volgende waarden in de **containernaam**, **Container installatiekopie**, en **resourcegroep** tekstvakken. Laat de andere waarden op de standaardwaarden en klik vervolgens op **OK**.

* De containernaam van de:`mycontainer`
* Afbeelding van de container:`microsoft/aci-helloworld`
* Resourcegroep:`myResourceGroup`

![Basisinstellingen voor een nieuw exemplaar van de container configureren in Azure portal][aci-portal-03]

U kunt Windows- en Linux-containers maken in Azure Containerexemplaren. In deze snelstartgids laten we de standaardinstelling van **Linux** omdat we een container op basis van Linux opgegeven (`microsoft/aci-helloworld`) in de vorige stap.

Laat de overige instellingen in **configuratie** de standaardinstellingen, klikt u vervolgens op **OK** om de configuratie te valideren.

![Een nieuw exemplaar van de container configureren in Azure portal][aci-portal-04]

Wanneer de validatie is voltooid, kunt u een overzicht van de container-instellingen worden weergegeven. Selecteer **OK** uw implementatie container aanvraag indienen.

![Samenvatting van instellingen voor een nieuw exemplaar van de container in Azure portal][aci-portal-05]

Wanneer implementatie start, wordt een tegel op uw voortgang van de implementatie die aangeeft portaldashboard geplaatst. Wanneer de implementatie is voltooid, de tegel is bijgewerkt zodat uw nieuwe **mycontainer myc1** containergroep.

![Voortgang van het maken van een nieuw exemplaar van de container in Azure portal][aci-portal-08]

Selecteer de **mycontainer myc1** containergroep om de eigenschappen van de container weer te geven. Noteer de **IP-adres** van de containergroep, evenals de **status** van de container.

![Overzicht van de container in Azure portal][aci-portal-06]

Nadat de container wordt verplaatst naar de **met** status, gaat u naar het IP-adres dat u hebt genoteerd in de vorige stap om de toepassing die wordt gehost in uw nieuwe container weer te geven.

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-app-browser]

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-02]: ./media/container-instances-quickstart-portal/qs-portal-02.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-app-browser]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids kunt u een Azure Container exemplaar gemaakt van een installatiekopie in een openbare opslagplaats, Docker-Hub. Als u probeert het bouwen van een container en deze implementeren in Azure Container-exemplaren die gebruikmaken van het register van de Container Azure wilt, blijven de zelfstudie exemplaren van Azure-Container.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)