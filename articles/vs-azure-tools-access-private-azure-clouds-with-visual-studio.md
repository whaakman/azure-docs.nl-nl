---
title: Toegang tot persoonlijke Azure-clouds met Visual Studio | Microsoft Docs
description: Leer hoe u toegang tot persoonlijke cloud-bronnen met behulp van Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/13/2017
ms.author: ghogen
ms.openlocfilehash: 669d8fe2141016924432f8128256c3deb7c44db3
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42059444"
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Toegang tot persoonlijke Azure-clouds met Visual Studio

Visual Studio biedt standaard ondersteuning voor REST-eindpunten voor Azure-cloud. In dit artikel leert u hoe u uw persoonlijke cloud-certificaat gebruiken voor toegang tot en interactie met de privécloud vanuit Visual Studio.

1. Download het bestand publicatie-instellingen in de Azure-portal voor de privécloud, of neem contact op met uw beheerder voor een bestand met publicatie-instellingen. (Het bestand heeft de extensie `.publishsettings`.)

1. In Visual Studio **Server Explorer**, met de rechtermuisknop op de **Azure** knooppunt en selecteert u **beheren en Filter abonnementen**.

    ![Opdracht abonnementen beheren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. In de **Microsoft Azure-abonnementen beheren** dialoogvenster, selecteer de **certificaten** tabblad, en selecteer vervolgens **importeren**.

    ![Azure-certificaten importeren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. In de **importeren van Microsoft Azure-abonnementen** dialoogvenster, selecteer **Bladeren**.

    ![Knop in het dialoogvenster voor het importeren van Microsoft Azure-abonnementen bladeren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. In de **Open** dialoogvenster, blader naar de map waar u het bestand publicatie-instellingen hebt opgeslagen, selecteert u het bestand en selecteer vervolgens **Open**.

    ![Selecteer het bestand met publicatie-instellingen](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. Wanneer geretourneerd naar de **importeren van Microsoft Azure-abonnementen** dialoogvenster, selecteer **importeren**.

    ![Het bestand met publicatie-instellingen importeren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    De certificaten zijn geïmporteerd uit het bestand met publicatie-instellingen in Visual Studio en u kunt nu interactief werken met uw persoonlijke cloudresources.

