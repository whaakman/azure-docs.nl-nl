---
title: Uw app testen in Azure | Microsoft Docs
description: Leer hoe u een bestandsshare maken in een testomgeving en op uw lokale computer en een virtuele machine in het lab te koppelen en vervolgens desktop/webtoepassingen naar de bestandsshare implementeren en te testen.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.author: spelluru
ms.openlocfilehash: 655159185a6011d79b927fd16d2957e2a59c8376
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54437789"
---
# <a name="test-your-app-in-azure"></a>Uw app in Azure testen 
Dit artikel bevat stappen voor het testen van uw toepassing in Azure met DevTest Labs. Eerst een bestandsshare in een lab instellen en deze koppelen als een station op uw lokale ontwikkelcomputer en een virtuele machine in een testomgeving. Daarna gebruikt u Visual Studio 2017 aan uw app implementeren in de bestandsshare, zodat u de app op de virtuele machine in het lab uitvoeren kunt.  

## <a name="prerequisites"></a>Vereisten 
1. [Maak een Azure-abonnement](https://azure.microsoft.com/free/) als u niet al hebt en meld u aan bij [Azure-portal](https://portal.azure.com).
2. Volg de instructies in [in dit artikel](devtest-lab-create-lab.md) om een lab met Azure DevTest Labs te maken. De testomgeving aan uw dashboard vastmaken zodat u deze gemakkelijk zodra die u zich vinden kunt aanmeldt. Azure DevTest Labs kunt u resources binnen Azure om snel te maken met minimale verspilling en kosten. Zie voor meer informatie over DevTest Labs, [overzicht](devtest-lab-overview.md). 
3. Een Azure Storage-account maken in de resourcegroep van de testomgeving met de instructies in de [een opslagaccount maken](../storage/common/storage-create-storage-account.md) artikel. Op de **storage-account maken** weergeeft, schakelt **Use existing** voor **resourcegroep**, en selecteer de **lab van resourcegroep**. 
4. Een bestandsshare maken in uw Azure-opslag met de instructies in de [een bestandsshare maken in Azure Files](../storage/files/storage-how-to-create-file-share.md) artikel. 

## <a name="mount-the-file-share-on-your-local-machine"></a>De bestandsshare koppelen op uw lokale computer
1. Gebruik het script uit op uw lokale machine [referenties voor opslaan van Azure bestand delen in Windows](../storage/files/storage-how-to-use-files-windows.md#persisting-azure-file-share-credentials-in-windows) sectie van [gebruiken van een Azure-bestandsshare met Windows](../storage/files/storage-how-to-use-files-windows.md) artikel. 
2. Vervolgens gebruikt u `net use` opdracht naar de bestandsshare koppelen op uw computer. Hier volgt de voorbeeldopdracht: Geef uw Azure storage-naam en de naam van bestandsshare voordat u de opdracht uitvoert. 

    `net use Z: \\<YOUR AZURE STORAGE NAME>.file.core.windows.net\<YOUR FILE SHARE NAME> /persistent:yes`

## <a name="create-a-vm-in-the-lab"></a>Een virtuele machine maken in het lab
1. Op de **bestandsshare** weergeeft, schakelt de **resourcegroep** in het koppelingenmenu bovenaan. U ziet de **resourcegroep** pagina. 
    
    ![Selecteer de resourcegroep van de breadcrumb-menu](media/test-app-in-azure/select-resource-group-bread-crump.png)
2. Op de **resourcegroep** weergeeft, schakelt de **lab** in DevTest Labs worden gemaakt.

    ![Het lab selecteren](media/test-app-in-azure/select-devtest-lab-in-resource-group.png)
3. Op de **DevTest Lab** -pagina voor uw lab, selecteer **+ toevoegen** op de werkbalk. 

    ![Knop voor het lab toevoegen](media/test-app-in-azure/add-button-in-lab.png)
4. Op de **kiest u een base** pagina, zoek **smalldisk**, en selecteer **[smalldisk] Windows Server 2016 Datacenter**. 

    ![Kleine schijf Windows server kiezen](media/test-app-in-azure/choose-small-disk-windows-server.png)
5. Op de **virtuele machine** pagina **virtuele-machinenaam**, **gebruikersnaam**, **wachtwoord**, en selecteer **maken** .    
    
    ![Pagina van de virtuele machine maken](media/test-app-in-azure/create-virtual-machine-page.png)    

## <a name="mount-the-file-share-on-your-vm"></a>De bestandsshare koppelen op de virtuele machine
1. Nadat de virtuele machine is gemaakt, selecteert u de **virtuele machine** in de lijst.    

    ![Selecteer in het lab VM](media/test-app-in-azure/select-lab-vm.png)
2. Selecteer **Connect** op de werkbalk om verbinding maken met de virtuele machine. 
3. [Installeer Azure PowerShell](https://azure.microsoft.com/downloads/) met behulp van de **Windows installeren** herstelkoppeling in de **opdrachtregelprogramma's** sectie. Zie voor andere manieren van Azure PowerShell, [in dit artikel](/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-6.8.1).
4. Volg de instructies in de [de bestandsshare koppelen](#mount-the-file-share) sectie. 

## <a name="publish-your-app-from-visual-studio"></a>Publiceer uw app vanuit Visual Studio
In deze sectie maakt publiceren u uw app vanuit Visual Studio om een test-VM in de cloud te.

1. Een bureaublad/web-App maken met behulp van Visual Studio 2017.
2. Uw app te bouwen.
3. Voor het publiceren van uw app, met de rechtermuisknop op uw project in de **Solution Explorer**, en selecteer **publiceren**. 
4. In de **wizard Publiceren**, voer de **station** die toegewezen aan de bestandsshare.

    **Desktop-app:**

    ![Desktop-app](media/test-app-in-azure/desktop-app.png)

    **Web-app:**

    ![Web-app](media/test-app-in-azure/web-app.png)

1. Selecteer **volgende** om te voltooien van de werkstroom publiceren, en selecteer **voltooien**. Als u de stappen in de wizard hebt voltooid, wordt Visual Studio uw toepassing bouwt en publiceert deze naar de bestandsshare. 


## <a name="test-the-app-on-your-test-vm-in-the-lab"></a>De app op uw virtuele testmachine testen in de testomgeving

1. Navigeer naar de pagina van de virtuele machine voor uw virtuele machine in het lab. 
2. Selecteer **Start** op de werkbalk om de virtuele machine starten als deze in staat gestopt is. U kunt automatisch wordt gestart en automatisch afsluiten beleidsregels voor uw virtuele machine te starten en stoppen van elke keer voorkomen instellen. 
3. Selecteer **Verbinden**.

    ![Pagina van de virtuele machine](media/test-app-in-azure/virtual-machine-page.png)
4. In de virtuele machine starten **Verkenner**, en selecteer **deze PC** te vinden van de bestandsshare.

    ![Delen op virtuele machine zoeken](media/test-app-in-azure/find-share-on-vm.png)

    > [!NOTE]
    > Voor een bepaalde reden, als u niet kunt zoeken naar de bestandsshare op uw virtuele machine of op uw lokale computer, u kunt koppelen het door het uitvoeren van de `net use` opdracht. U vindt de `net use` opdracht op de **Connect** Wizard van uw **bestandsshare** in Azure portal.
1. Open de bestandsshare en Bevestig dat u de app die u hebt geïmplementeerd vanuit Visual Studio. 

    ![Open-share op de virtuele machine](media/test-app-in-azure/open-file-share.png)

    U kunt nu toegang tot en test uw app in de test-VM die u hebt gemaakt in Azure.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor informatie over het gebruik van virtuele machines in een testomgeving. 

- [Een virtuele machine aan een lab toevoegen](devtest-lab-add-vm.md)
- [Een lab-virtuele machine opnieuw starten](devtest-lab-restart-vm.md)
- [Formaat van een lab-VM](devtest-lab-resize-vm.md)
