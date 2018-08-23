---
title: Een Azure-cloudserviceproject configureren met Visual Studio | Microsoft Docs
description: Informatie over het configureren van een Azure-cloud service-project in Visual Studio, afhankelijk van uw vereisten voor dit project.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/06/2017
ms.author: ghogen
ms.openlocfilehash: 0e2e03f7a492f5018b99145a63bb1b7432d3e0b3
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060622"
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Een Azure-cloudserviceproject configureren met Visual Studio
U kunt een Azure-cloud service-project, configureren, afhankelijk van uw vereisten voor dit project. U kunt instellen voor het project voor de volgende categorieën:

- **Een service in de cloud publiceren naar Azure** -u kunt instellen dat een eigenschap om ervoor te zorgen dat een bestaande cloudservice die is geïmplementeerd in Azure niet per ongeluk is verwijderd.
- **Uitvoeren of fouten opsporen in een cloudservice op de lokale computer** -kunt u de configuratie van een service te gebruiken en aangeven of u wilt starten van de Azure-opslagemulator.
- **Een cloud service-pakket valideren wanneer deze wordt gemaakt** -u kunt waarschuwingen behandelen als fouten, zodat u ervoor zorgen kunt dat het pakket voor cloud-service wordt geïmplementeerd zonder problemen. 

## <a name="steps-to-configure-an-azure-cloud-service-project"></a>Stappen voor het configureren van een Azure-cloud service-project
1. Open of maak een cloud service-project in Visual Studio

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **eigenschappen**.
   
1. Op de pagina van de eigenschappen van het project, selecteer de **ontwikkeling** tabblad.

    ![Menu van de eigenschappen van project](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. Stel **vragen voordat u verwijdert een bestaande implementatie** naar **waar**. Deze instelling kunt u ervoor zorgen dat u per ongeluk verwijdert een bestaande implementatie in Azure

1. Selecteer de gewenste **serviceconfiguratie** om aan te geven welke serviceconfiguratie die u wilt gebruiken wanneer u fouten opsporen in uw cloudservice lokaal of uitvoert. Zie voor meer informatie over het wijzigen van de serviceconfiguratie van een voor een rol [over het configureren van de rollen voor een Azure-cloudservice met Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).

1. Instellen **Start Azure-opslagemulator** naar **waar** starten van de Azure-opslagemulator wanneer u fouten opsporen in uw cloudservice lokaal of uitvoert.

1. Instellen **waarschuwingen als fouten behandelen** naar **waar** om te controleren of u kunt als er validatiefouten pakket niet publiceren.

1. Stel **web project poorten gebruiken** naar **waar** om ervoor te zorgen dat dezelfde poort wordt gebruikt door uw Webrol telkens wanneer deze wordt gestart lokaal in IIS Express.

1. Selecteer in de werkbalk van Visual Studio, **opslaan**.

## <a name="next-steps"></a>Volgende stappen
- [Een Azure-project met behulp van serviceconfiguraties met meerdere configureren](vs-azure-tools-multiple-services-project-configurations.md)

