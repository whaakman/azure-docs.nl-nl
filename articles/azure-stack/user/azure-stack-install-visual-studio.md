---
title: Installeer Visual Studio en maak verbinding met Azure-Stack | Microsoft Docs
description: Meer informatie over de vereiste stappen voor het installeren van Visual Studio en maak verbinding met Azure-Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2018
ms.author: mabrigg
ms.reviewer: unknown
ms.openlocfilehash: cbd5e5dbcdd2565e8066b0721f45863569bfd90a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295027"
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Installeer Visual Studio en maak verbinding met Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt Visual Studio gebruiken om te schrijven en implementeren van Azure Resource Manager [sjablonen](azure-stack-arm-templates.md) naar Azure-Stack. De stappen in dit artikel helpt u bij de installatie van Visual Studio op de [Azure Stack](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), of op een externe computer als u van plan naar de Stack Azure via bent de [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="install-visual-studio"></a>Visual Studio installeren

1. Downloaden en uitvoeren van de [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).  

2. Open de **Microsoft Web Platform Installer**.

3. Zoeken naar **Visual Studio Community 2015 met Microsoft Azure SDK - 2.9.6**. Klik op **toevoegen**, en **installeren**.

4. Verwijder de **Microsoft Azure PowerShell** die is geïnstalleerd als onderdeel van de Azure SDK.

    ![Schermopname van WebPI installeren-stappen](./media/azure-stack-install-visual-studio/image1.png) 

5. [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)

6. Het besturingssysteem opnieuw opstarten nadat de installatie is voltooid.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Verbinding maken met Azure met Azure AD-Stack

1. Visual Studio starten.

2. Van de **weergave** selecteert u **Cloud Explorer**.

3. Selecteer in het nieuwe deelvenster **Account toevoegen** en meld u aan met de referenties van uw Azure Active Directory (Azure AD).  

    ![Schermopname van Cloud Explorer eenmaal aangemeld en verbonden met de Azure-Stack](./media/azure-stack-install-visual-studio/image2.png)

Wanneer u bent aangemeld, kunt u [implementeren sjablonen](azure-stack-deploy-template-visual-studio.md) of blader beschikbaar resourcetypen en resourcegroepen gebruiken om uw eigen sjablonen te maken.  

## <a name="connect-to-azure-stack-with-ad-fs"></a>Verbinding maken met Azure Stack met AD FS

1. Visual Studio starten.

2. Van **extra**, selecteer **opties**.

3. Vouw **omgeving** in de **navigatiedeelvenster** en selecteer **Accounts**.

4. Selecteer **toevoegen**, en voer de gebruiker Azure Resource Manager-eindpunt.  
  Voor de Azure-Stack Development kit, de URL is: `https://management.local.azurestack/external`.  
  Voor de URL voor systemen Azure-Stack geïntegreerde is: `https://management.[Region}.[External FQDN]`.

    ![X](./media/azure-stack-install-visual-studio/image5.png)

5. Selecteer **Toevoegen**.  

    Visual Studio, roept de Azure Resource Manager en detecteert de eindpunten met inbegrip van het eindpunt voor authenticatie voor Azure Directory Federated Services (AD FS).

    ![Schermopname van Cloud Explorer eenmaal aangemeld en verbonden met de Azure-Stack](./media/azure-stack-install-visual-studio/image6.png)

6. Selecteer **Cloud Explorer** van de **weergave** menu.
7. Selecteer **Account toevoegen** en meld u aan met de referenties van uw AD FS.  

    ![X](./media/azure-stack-install-visual-studio/image7.png)

    Cloud Explorer query uitgevoerd op de beschikbare abonnementen. U kunt een een beschikbare abonnement selecteren om te beheren.

    ![X](./media/azure-stack-install-visual-studio/image8.png)

8. Bladeren door uw bestaande resources, resourcegroepen of sjablonen te implementeren.

## <a name="next-steps"></a>Volgende stappen

 - Lees meer over [co-existentie](https://msdn.microsoft.com/library/ms246609.aspx) met andere versies van Visual Studio.
 - [Sjablonen voor Azure-Stack ontwikkelen](azure-stack-develop-templates.md)