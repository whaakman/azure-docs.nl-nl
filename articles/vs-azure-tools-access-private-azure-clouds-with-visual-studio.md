---
title: Toegang tot persoonlijke Azure-clouds met Visual Studio | Microsoft Docs
description: Informatie over het toegang krijgen tot persoonlijke cloud-bronnen met behulp van Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 11/13/2017
ms.author: ghogen
ms.openlocfilehash: 0160ac6db2b92d5a30a19dd444d01a8558b3eed3
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31792987"
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Toegang tot persoonlijke Azure-clouds met Visual Studio

Visual Studio ondersteunt standaard Azure-cloud REST-eindpunten. In dit artikel leert u hoe u uw persoonlijke cloud-certificaat gebruiken voor toegang tot en communiceren met de privécloud vanuit Visual Studio.

1. Download het bestand met publicatie-instellingen in de Azure portal voor de privécloud, of neem contact op met uw beheerder voor een bestand met publicatie-instellingen. (Het bestand de extensie heeft `.publishsettings`.)

1. In Visual Studio **Server Explorer**, met de rechtermuisknop op de **Azure** uit en selecteer **beheren en Filter abonnementen**.

    ![Opdracht voor abonnementen beheren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. In de **Microsoft Azure-abonnementen beheren** dialoogvenster, selecteer de **certificaten** tabblad en selecteer vervolgens **importeren**.

    ![Azure certificaten importeren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. In de **importeren Microsoft Azure-abonnementen** dialoogvenster Selecteer **Bladeren**.

    ![Knop in het dialoogvenster importeren Microsoft Azure-abonnementen bladeren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. In de **Open** dialoogvenster, blader naar de map waar u het bestand publicatie-instellingen hebt opgeslagen, selecteert u het bestand en selecteer vervolgens **Open**.

    ![Selecteer het bestand met publicatie-instellingen](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. Wanneer u keert terug naar de **importeren Microsoft Azure-abonnementen** dialoogvenster Selecteer **importeren**.

    ![Het bestand met publicatie-instellingen importeren](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    De certificaten zijn geïmporteerd uit het bestand met publicatie-instellingen in Visual Studio en u kunt nu werken met de resources van uw privécloud.

