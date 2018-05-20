---
title: Installeer Visual Studio en maak verbinding met Azure-Stack | Microsoft Docs
description: Meer informatie over de vereiste stappen voor het installeren van Visual Studio en maak verbinding met Azure-Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: mabrigg
ms.openlocfilehash: 3eaefbe011c4d98fe9a76d4f277a76a2f167b191
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Installeer Visual Studio en maak verbinding met Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Visual Studio gebruiken om te ontwerpen en implementeren van Azure Resource Manager [sjablonen](azure-stack-arm-templates.md) in Azure-Stack. U kunt de stappen in dit artikel voor het installeren van Visual Studio vanaf [Azure Stack Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of vanuit een Windows-externe client als u via verbonden bent [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). Deze stappen in dit artikel zijn voor een nieuwe installatie van Visual Studio 2015 Community Edition. Lees meer over [co-existentie](https://msdn.microsoft.com/library/ms246609.aspx) met andere versies van Visual Studio.

## <a name="install-visual-studio"></a>Visual Studio installeren

1. Downloaden en uitvoeren van de [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
2. Zoeken naar **Visual Studio Community 2015 met Microsoft Azure SDK - 2.9.6**, selecteer **toevoegen**, en selecteer vervolgens **installeren**.

    ![Schermopname van WebPI installeren-stappen](./media/azure-stack-install-visual-studio/image1.png)

3. Verwijder de **Microsoft Azure PowerShell** die geïnstalleerd als onderdeel van de Azure SDK.

    ![Schermopname van de interface's toevoegen of verwijderen voor Azure PowerShell](./media/azure-stack-install-visual-studio/image2.png)

4. [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)

5. Het besturingssysteem opnieuw opstarten nadat de installatie is voltooid.

## <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

1. Visual Studio starten.

2. Van de **weergave** selecteert u **Cloud Explorer**.

3. Selecteer in het nieuwe deelvenster **Account toevoegen** en meld u aan met uw Azure Active Directory-referenties.
    ![Schermopname van Cloud Explorer na aanmelding in en is verbonden met de Azure-Stack](./media/azure-stack-install-visual-studio/image6.png)

Wanneer u bent aangemeld, kunt u [implementeren sjablonen](azure-stack-deploy-template-visual-studio.md) of blader beschikbaar resourcetypen en resourcegroepen gebruiken om uw eigen sjablonen te maken.

## <a name="next-steps"></a>Volgende stappen

* [Sjablonen voor Azure-Stack ontwikkelen](azure-stack-develop-templates.md)
