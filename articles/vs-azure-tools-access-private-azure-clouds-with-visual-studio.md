---
title: Toegang tot persoonlijke Azure-clouds met Visual Studio | Microsoft Docs
description: Informatie over het toegang krijgen tot persoonlijke cloud-bronnen met behulp van Visual Studio.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.openlocfilehash: b2578c837732ab05d538e9b896ed3a3035075a70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Toegang tot persoonlijke Azure-clouds met Visual Studio
Visual Studio ondersteunt standaard openbare Azure-cloud REST-eindpunten. In dit onderwerp leert u hoe uw persoonlijke cloud-certificaat gebruiken voor toegang tot - en interactie met - de privécloud vanuit Visual Studio.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Voor toegang tot een persoonlijke Azure cloud in Visual Studio
1. In de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885) download het bestand met publicatie-instellingen voor de privécloud, of neem contact op met uw beheerder voor een bestand met publicatie-instellingen. Op de openbare versie van Azure, de koppeling voor het downloaden van dit is [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Het gedownloade bestand moet een extensie van `.publishsettings`)

1. Open Visual Studio

1. In **Server Explorer**, met de rechtermuisknop op de **Azure** knooppunt en selecteer in het contextmenu **beheren en Filter abonnementen**.
   
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
   
## <a name="next-steps"></a>Volgende stappen
- [Publiceren naar een Azure Cloudservice vanuit Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- [How to: downloaden en importeren van instellingen en abonnementsgegevens publiceren](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)
