---
title: Azure Security Center-snelstartgids - onboarding uw virtuele machines van Azure Stack met Security Center | Microsoft Docs
description: Deze quickstart laat zien hoe u de extensie voor de Azure Monitor-, Update- en Configuratiebeheer-virtuele machine op een virtuele machines van Azure Stack inricht.
services: security-center
documentationcenter: na
author: monhaber
manager: dsavage
editor: ''
ms.assetid: 8982348a-0624-40c7-8a1e-642a523c7f6b
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: monhaber
ms.openlocfilehash: 9efd6514b722168f8ecb1235159e7463ce318118
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904012"
---
# <a name="quickstart--onboard-your-azure-stack-virtual-machines-to-security-center"></a>Quickstart:  Onboarding uw virtuele machines van Azure Stack met Security Center
Nadat u onboarding uw Azure-abonnement, u kunt Security Center inschakelen voor het beveiligen van uw virtuele machines die worden uitgevoerd op Azure Stack door toe te voegen de **Azure Monitor-, Update- en Configuratiebeheer** -extensie voor virtuele machine van de Azure Stack marketplace.

Deze snelstartgids leest u hoe om toe te voegen de **Azure Monitor-, Update- en Configuratiebeheer** extensie van de virtuele machine op een virtuele machine (Linux en Windows worden beide ondersteund) die worden uitgevoerd op Azure Stack.

## <a name="prerequisites"></a>Vereisten
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

U moet een Azure-abonnement hebben op Security Center Standard-laag voordat u begint. Zie [Onboard your Azure subscription to Security Center Standard](security-center-get-started.md) (Uw Azure-abonnement registreren voor Security Center Standard) voor upgrade-instructies. U kunt Security Center Standard-laag gratis gedurende 30 dagen. Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.

## <a name="add-an-azure-stack-virtual-machine"></a>Toevoegen van een virtuele machine van Azure Stack

1. Meld u aan bij de [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. Ga naar het **Microsoft Azure**-menu en selecteer **Security Center**. **Security Center - Overzicht** wordt geopend. 

   ![Overzicht van Security Center][2]

3. Selecteer in het hoofdmenu van Security Center de optie **Aan de slag**.
4. Selecteer het tabblad **Aan de slag**.

   ![Aan de slag][3]

5. Klik op **Configureren** onder **Nieuwe niet-Azure-computers toevoegen**. Een lijst met uw Log Analytics-werkruimten wordt weergegeven. De lijst bevat, indien van toepassing, de standaardwerkruimte die is gemaakt door Security Center toen automatisch inrichten werd ingeschakeld. Deze werkruimte of een andere werkruimte die u wilt dat de Azure Stack-VM naar rapport beveiligingsgegevens te selecteren.

    ![Niet-Azure-computer toevoegen](./media/quick-onboard-windows-computer/non-azure.png)

   De **Direct Agent** blade geopend met een koppeling voor het downloaden van de agent en sleutels voor uw werkruimte-ID te gebruiken bij het configureren van de agent.

   >[!NOTE]
   > U hoeft niet de agent handmatig downloadt. De agent wordt geïnstalleerd als een VM-extensie in de onderstaande stappen.

6. Selecteer rechts van **Werkruimte-id** het kopieerpictogram en plak de id in Kladblok.

7. Selecteer rechts van **Primaire sleutel** het kopieerpictogram en plak de sleutel in Kladblok.

## <a name="add-the-virtual-machine-extension-to-your-existing-azure-stack-virtual-machines"></a>De virtuele machine-extensie toevoegen aan uw bestaande virtuele machines van Azure Stack
U moet toevoegen de **Azure Monitor-, Update- en Configuratiebeheer** virtuele machine-extensie voor de virtuele machines die worden uitgevoerd op uw Azure Stack.

1. In een nieuw browsertabblad, meld u aan bij uw **Azure Stack** portal.
2. Ga naar de **virtuele machines** pagina, selecteert u de virtuele machine die u wilt beveiligen met Security Center. Zie voor meer informatie over het maken van een virtuele machine in Azure Stack [deze Quick Start voor Windows-machines](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-windows-portal) of [deze Quick Start voor virtuele Linux-machines](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-linux-portal).
3. Selecteer **Extensies**. De lijst met extensies voor virtuele machines geïnstalleerd op deze virtuele machine wordt weergegeven.
4. Klik op de **toevoegen** tabblad. De **nieuwe Resource** menu-blade wordt geopend en ziet u de lijst met beschikbare virtuele machine-extensies. 
5. Selecteer de **Azure Monitor-, Update- en Configuratiebeheer** -extensie en klikt u op **maken**. De **-extensie installeren** configuratieblade wordt geopend.
6. Op de **-extensie installeren** configuratieblade, plak de **werkruimte-ID** en **Werkruimtesleutel (primaire sleutel)** die u in Kladblok hebt gekopieerd in de vorige procedure.
7.  Wanneer u de vereiste configuratie-instellingen voltooien hebt, klikt u op **OK**.
8. Nadat de installatie van de extensie is voltooid, de status wordt weergegeven als **inrichting geslaagd**. Het duurt maximaal één uur voor de virtuele machine wordt weergegeven in de portal van Security Center.

Zie voor meer informatie over het installeren en configureren van de agent voor Windows [Connect Windows computers](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard).

Voor Linux het oplossen van problemen met de agentstatus, Zie [oplossen Azure Log Analytics Linux Agent](../azure-monitor/platform/agent-linux-troubleshoot.md).

U kunt nu uw Azure-VM's en niet-Azure-computers op één plek bewaken. In de Security Center-portal op Azure, onder **Compute**, hebt u een overzicht van alle virtuele machines en computers, plus de aanbevelingen. Security Center geeft ook de detectie voor deze computers in beveiligingswaarschuwingen.

  ![Blade Compute][6]

Er worden twee soorten pictogrammen weergegeven op de blade **Compute**:

![pictogram1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Niet-Azure-computer 

![pictogram2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Azure-VM (virtuele machines van Azure Stack wordt in deze groep weergeven)

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u niet meer nodig hebt, kunt u de extensie verwijderen uit de virtuele machine via de Azure Stack-portal.

De uitbreiding te verwijderen:

1. Open de **Azure Stack-Portal**.
2. Ga naar **virtuele machines** pagina, selecteert u de virtuele machine die u wilt verwijderen van de extensie.
3. Selecteer **extensies**, selecteert u de extensie **Microsoft.EnterpriseCloud.Monitoring**.
4. Klik op **verwijderen**, en Bevestig de selectie van door te klikken op **Ja**.

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u de Azure Monitor-, Update- en Configuratiebeheer-extensie op een virtuele machine op Azure Stack ingericht. Voor meer informatie over het gebruik van Security Center gaat u verder met de zelfstudie voor het configureren van een beveiligingsbeleid en het beoordelen van de beveiliging van uw resources.

> [!div class="nextstepaction"]
> [Zelfstudie: Opstellen en beoordelen van beveiligingsbeleid](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/get-started.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png
