---
title: Een Azure-cloud service-project met Visual Studio configureren | Microsoft Docs
description: Informatie over het configureren van een Azure-cloud service-project in Visual Studio, afhankelijk van uw vereisten voor dat project.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: 799715093bd1a8c8e77e6cdb7168670dc263dfa5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Een Azure-cloud service-project met Visual Studio configureren
U kunt een Azure-cloud service-project, afhankelijk van uw vereisten voor dat project configureren. U kunt eigenschappen instellen voor het project voor de volgende categorieën:

- **Een cloudservice publiceren naar Azure** -u kunt instellen dat een eigenschap om ervoor te zorgen dat een bestaande cloudservice die is geïmplementeerd in Azure niet per ongeluk is verwijderd.
- **Uitvoeren of fouten opsporen in een cloudservice op de lokale computer** -kunt u de configuratie van een service te gebruiken en aangeven of u wilt starten van de Azure-opslagemulator.
- **Valideren van een pakket met cloud-service wanneer deze wordt gemaakt** -u kunt waarschuwingen als fouten behandelen, zodat u ervoor zorgen kunt dat het servicepakket cloud worden geïmplementeerd zonder problemen. 

## <a name="steps-to-configure-an-azure-cloud-service-project"></a>Stappen voor het configureren van een Azure-cloud service-project
1. Open of maak een cloudserviceproject in Visual Studio

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **eigenschappen**.
   
1. Selecteer in de pagina eigenschappen van het project de **ontwikkeling** tabblad.

    ![Menu Project-eigenschappen](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. Stel **vragen voor het verwijderen van een bestaande implementatie** naar **True**. Deze instelling kunt u ervoor zorgen dat u per ongeluk een bestaande implementatie in Azure niet verwijderen

1. Selecteer de gewenste **serviceconfiguratie** om aan te geven welke configuratie van de service die u wilt gebruiken tijdens het uitvoeren of fouten opsporen in uw cloudservice lokaal. Zie voor meer informatie over het wijzigen van de configuratie van een service voor een rol [het configureren van de rollen voor een Azure-cloudservice met Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).

1. Stel **Start Azure-opslagemulator** naar **waar** de Azure-opslagemulator wordt gestart wanneer u uitvoert of fouten opsporen in uw cloudservice lokaal.

1. Stel **waarschuwingen als fouten behandelen** naar **True** om ervoor te zorgen dat u als er validatiefouten pakket niet publiceren.

1. Stel **web project poorten** naar **True** om ervoor te zorgen dat uw Webrol dezelfde poort gebruikt telkens wanneer deze wordt gestart lokaal in IIS Express.

1. Selecteer in de werkbalk Visual Studio **opslaan**.

## <a name="next-steps"></a>Volgende stappen
- [Een Azure-project met behulp van serviceconfiguraties met meerdere configureren](vs-azure-tools-multiple-services-project-configurations.md)

