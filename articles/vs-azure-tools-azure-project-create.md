---
title: Het maken van een Azure-cloud service-project met Visual Studio | Microsoft Docs
description: Ontdek hoe u een Azure-cloud service-project maakt met Visual Studio
services: visual-studio-online
author: ghogen
manager: douge
assetId: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 5c59b19d50596ca85deda7e5bfa7bacd0017028b
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969291"
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a>Het maken van een Azure-cloud service-project met Visual Studio
De Azure Tools voor Visual Studio biedt een projectsjabloon waarmee u maakt een [Azure-cloudservice](/azure/cloud-services/cloud-services-choose-me), dit is een eenvoudige algemene Azure-service. Zodra het project is gemaakt, wordt Visual Studio kunt u configureren, fouten opsporen en implementeren van de cloudservice in Azure.

## <a name="steps-to-create-an-azure-cloud-service-project-in-visual-studio"></a>Stappen voor het maken van een Azure-cloud service-project in Visual Studio
In deze sectie helpt u bij het maken van een Azure-cloud service-project in Visual Studio met een of meer webrollen.  

1. Start Visual Studio als beheerder.

1. Selecteer in het hoofdmenu **bestand** > **nieuw** > **Project**.

1. Selecteer **Cloud** sjabloon knooppunten van de Visual C# of Visual Basic-project en selecteer **Azure Cloud Service** uit de lijst met sjablonen.

    ![Nieuwe Azure-cloudservice](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. Geef op welke versie van .NET Framework die u wilt gebruiken voor het ontwikkelen van uw project.

1. Voer een naam en locatie voor uw project en een naam voor de oplossing. 

1. Selecteer **OK**.

1. In de **nieuwe Microsoft Azure Cloud Service** dialoogvenster, selecteer de rollen die u wilt toevoegen en klik op de knop pijl-rechts aan uw oplossing.

    ![Selecteer nieuwe Azure-cloud service-functies](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. Wijzig de naam van een rol die u hebt toegevoegd, Beweeg de muisaanwijzer op de rol in de **nieuwe Microsoft Azure Cloud Service** dialoogvenster, en selecteer in het contextmenu **naam**. U kunt ook een rol wijzigen in uw oplossing (in de **Solution Explorer**) nadat deze is toegevoegd.

    ![Wijzig de naam van de rol van de Azure-cloud-service](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

Het Azure van Visual Studio-project heeft de koppelingen aan de rol-projecten in de oplossing. Het project bevat ook de *servicedefinitiebestand* en *serviceconfiguratiebestand*:

- **Servicedefinitiebestand** -definieert de runtime-instellingen voor uw toepassing, met inbegrip van welke rollen zijn vereist, eindpunten en de grootte van virtuele machine. 
- **Serviceconfiguratiebestand** -configureert u het aantal exemplaren van een rol worden uitgevoerd en de waarden van de instellingen die zijn gedefinieerd voor een rol. 

Zie voor meer informatie over deze bestanden [configureren van de rollen voor een Azure-cloudservice met Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

## <a name="next-steps"></a>Volgende stappen
- [Beheren van rollen in Azure cloud service-projecten met Visual Studio](./vs-azure-tools-cloud-service-project-managing-roles.md)
