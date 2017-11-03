---
title: Een Azure-cloud service-project maken met Visual Studio | Microsoft Docs
description: Ontdek hoe u een Azure-cloudserviceproject maken met Visual Studio
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 1f6ded87b551f660853ea4eb0548f3d942e28fa8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a>Een Azure-cloud service-project maken met Visual Studio
De Azure-Tools voor Visual Studio biedt een projectsjabloon waarmee u een Azure-cloudservice maken. Zodra het project is gemaakt, wordt Visual Studio kunt u configureren en implementeren van de cloudservice in Azure voor foutopsporing.

## <a name="steps-to-create-an-azure-cloud-service-project-in-visual-studio"></a>Stappen voor het maken van een Azure-cloud service-project in Visual Studio
Deze sectie helpt u bij het maken van een Azure-cloud service-project in Visual Studio met een of meer webrollen.  

1. Start Visual Studio als beheerder.

1. Selecteer in het hoofdmenu **bestand** > **nieuw** > **Project**.

1. Selecteer **Cloud** sjabloon knooppunten van de Visual C# of Visual Basic-project en selecteer **Azure Cloud Service** uit de lijst met sjablonen.

    ![Nieuwe Azure-cloud-service](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. Geef op welke versie van .NET Framework die u gebruiken wilt voor het ontwikkelen van uw project.

1. Voer een naam en locatie voor uw project en een naam voor de oplossing. 

1. Selecteer **OK**.

1. In de **nieuwe Microsoft Azure Cloud Service** dialoogvenster, selecteer de rollen die u wilt toevoegen en kies de knop pijl naar rechts om toe te voegen aan uw oplossing.

    ![Selecteer de nieuwe Azure-cloud service-rollen](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. Wijzig de naam van een rol die u hebt toegevoegd, houd de muisaanwijzer op de rol in de **nieuwe Microsoft Azure Cloud Service** dialoogvenster, en selecteer in het contextmenu **naam**. U kunt ook een rol wijzigen binnen uw oplossing (in de **Solution Explorer**) nadat deze is toegevoegd.

    ![Wijzig de naam van de rol van de Azure-cloud-service](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

Het Azure met Visual Studio-project heeft koppelingen met de functie-projecten in de oplossing. Het project bevat ook de *servicedefinitiebestand* en *serviceconfiguratiebestand*:

- **Servicedefinitiebestand** -definieert de runtime-instellingen voor uw toepassing, met inbegrip van welke rollen zijn vereist, de eindpunten en de grootte van de virtuele machine. 
- **Serviceconfiguratiebestand** -hoeveel exemplaren van een rol worden uitgevoerd en de waarden van de instellingen die zijn gedefinieerd voor een functie configureert. 

Zie voor meer informatie over deze bestanden [de rollen configureert voor een Azure-cloudservice met Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

## <a name="next-steps"></a>Volgende stappen
- [Het beheren van rollen in Azure cloudserviceprojecten met Visual Studio](./vs-azure-tools-cloud-service-project-managing-roles.md)
