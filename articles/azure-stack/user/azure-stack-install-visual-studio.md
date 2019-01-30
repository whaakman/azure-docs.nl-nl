---
title: Visual Studio installeren en verbinding maken met Azure Stack | Microsoft Docs
description: Meer informatie over de stappen die nodig zijn voor Visual Studio installeren en verbinding maken met Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 274240aab54f27f36734516026e9feebf64ae4b5
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55248109"
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Visual Studio installeren en verbinding maken met Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt Visual Studio gebruiken om te schrijven en implementeren van Azure Resource Manager [sjablonen](azure-stack-arm-templates.md) naar Azure Stack. De stappen in dit artikel helpen u bij het installeren van Visual Studio op [Azure Stack](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of op een externe computer als u van plan bent te gebruiken van Azure Stack via [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="install-visual-studio"></a>Visual Studio installeren

1. Downloaden en uitvoeren van de [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).  

2. Open de **Microsoft Web Platform Installer**.

3. Zoeken naar **Visual Studio Community 2015 met Microsoft Azure SDK - 2.9.6**. Klik op **toevoegen**, en **installeren**.

4. Verwijder de **Microsoft Azure PowerShell** die is geïnstalleerd als onderdeel van de Azure SDK.

    ![Schermafbeelding van WebPI-installatiestappen](./media/azure-stack-install-visual-studio/image1.png) 

5. [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)

6. Het besturingssysteem opnieuw opstarten nadat de installatie is voltooid.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Verbinding maken met Azure Stack met Azure AD

1. Launch Visual Studio.

2. Uit de **weergave** in het menu **Cloud Explorer**.

3. Selecteer in het nieuwe deelvenster **-Account toevoegen** en meld u aan met de referenties van uw Azure Active Directory (Azure AD).  

    ![Schermopname van Cloud Explorer één keer aangemeld en verbonden met Azure Stack](./media/azure-stack-install-visual-studio/image2.png)

Zodra u bent aangemeld, kunt u [sjablonen implementeren](azure-stack-deploy-template-visual-studio.md) typen beschikbare resources en resourcegroepen voor het maken van uw eigen sjablonen te bladeren.  

## <a name="connect-to-azure-stack-with-ad-fs"></a>Verbinding maken met Azure Stack met AD FS

1. Launch Visual Studio.

2. Van **extra**, selecteer **opties**.

3. Vouw **omgeving** in de **navigatiedeelvenster** en selecteer **Accounts**.

4. Selecteer **toevoegen**, en voer de gebruiker Azure Resource Manager-eindpunt.  
  Voor de Azure Stack Development Kit, de URL is: `https://management.local.azurestack/external`.  
  De URL is voor geïntegreerde Azure Stack-systemen: `https://management.[Region}.[External FQDN]`.

    ![X](./media/azure-stack-install-visual-studio/image5.png)

5. Selecteer **Toevoegen**.  

    Visual Studio Azure Resource Manager-aanroepen en detecteert de eindpunten, met inbegrip van de verificatie-eindpunt voor Azure Directory Federated Services (AD FS).

    ![Schermopname van Cloud Explorer één keer aangemeld en verbonden met Azure Stack](./media/azure-stack-install-visual-studio/image6.png)

6. Selecteer **Cloud Explorer** uit de **weergave** menu.

1. Selecteer **-Account toevoegen** en meld u aan met uw AD FS-referenties.  

    ![Cloud Explorer](./media/azure-stack-install-visual-studio/image7.png)

    Cloud Explorer vraagt de beschikbare abonnementen. U kunt een selecteren een abonnement beschikbaar om te beheren.

    ![Cloud Explorer](./media/azure-stack-install-visual-studio/image8.png)

8. Bladeren door uw bestaande resources, resourcegroepen of sjablonen implementeren.

## <a name="next-steps"></a>Volgende stappen

 - Meer informatie over Visual Studio [naast elkaar](https://msdn.microsoft.com/library/ms246609.aspx) met andere versies van Visual Studio.
 - [Sjablonen ontwikkelen voor Azure Stack](azure-stack-develop-templates.md).