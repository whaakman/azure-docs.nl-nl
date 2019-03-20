---
title: VHD-bestand uploaden naar Azure DevTest Labs met Microsoft Azure Storage Explorer | Microsoft Docs
description: VHD-bestand uploaden naar het lab storage-account met behulp van Microsoft Azure Storage Explorer
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 12a21f4fdfb279df154b4995b6191aa0ed54ab43
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57894475"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-microsoft-azure-storage-explorer"></a>VHD-bestand uploaden naar het lab storage-account met behulp van Microsoft Azure Storage Explorer

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

In Azure DevTest Labs, kunnen de VHD-bestanden worden gebruikt om aangepaste installatiekopieën, die worden gebruikt voor het inrichten van virtuele machines te maken. In dit artikel laat zien hoe u [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) een VHD-bestand uploaden naar storage-account van een lab. Als u uw VHD-bestand hebt geüpload de [sectie volgende stappen](#next-steps) geeft een lijst van sommige artikelen die laten zien hoe u een aangepaste installatiekopie van het geüploade VHD-bestand maken. Zie voor meer informatie over schijven en VHD's in Azure, [Inleiding tot beheerde schijven](../virtual-machines/linux/managed-disks-overview.md)

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

De volgende stappen helpen u bij het uploaden van een VHD-bestand voor het gebruik van DevTest Labs [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).

1. [Download en installeer de nieuwste versie van de Microsoft Azure Storage Explorer](https://www.storageexplorer.com).

1. Haal de naam van de storage-account van de testomgeving met behulp van de Azure-portal:

    1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
    
    1. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** in de lijst.
    
    1. Selecteer de gewenste lab in de lijst met labs.  
    
    1. Selecteer op de blade van de testomgeving **configuratie**. 
    
    1. In het lab **configuratie** Selecteer **aangepaste installatiekopieën (VHD's)**.
    
    1. Op de **aangepaste installatiekopieën** blade, selecteer **+ toevoegen**. 
    
    1. Op de **aangepaste installatiekopie** Selecteer **VHD**.
    
    1. Op de **VHD** Selecteer **uploaden van een VHD met behulp van PowerShell**.
    
        ![Met behulp van PowerShell VHD uploaden][0]
    
    1. De **Upload een afbeelding met behulp van PowerShell** blade geeft een aanroep naar de **Add-AzureVhd** cmdlet. De eerste parameter (*bestemming*) bevat de naam van het opslagaccount voor de testomgeving in de volgende indeling:
    
        https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/... 

    1. Noteer de naam van opslagaccount als deze wordt gebruikt in latere stappen.
    
1. Verbinding maken met een Azure-abonnement-account met behulp van Storage Explorer.

    > [!TIP] 
    > 
    > Storage Explorer biedt ondersteuning voor verschillende verbindingsopties. In deze sectie ziet u verbinding maken met een opslagaccount dat is gekoppeld aan uw Azure-abonnement. Als u wilt zien van de andere verbindingsopties die worden ondersteund door de Storage Explorer, raadpleegt u het artikel [aan de slag met Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).
 
    1. Open Storage Explorer.
    
    1. Selecteer in Storage Explorer **Azure-Accountinstellingen**. 
    
        ![Azure-accountinstellingen][1]
    
    1. In het linkerdeelvenster ziet u zich hebt aangemeld bij Microsoft-accounts. Selecteer **Een account toevoegen** om verbinding te maken met een ander account en volg de dialoogvensters om aan te melden met een Microsoft-account dat is gekoppeld aan ten minste één actief Azure-abonnement.
    
        ![Een account toevoegen][2]
    
    1. Wanneer u bent aangemeld met een Microsoft-account, worden in het linkerdeelvenster de Azure-abonnementen weergegeven die aan dat account zijn gekoppeld. Selecteer de Azure-abonnementen waarmee u wilt werken en selecteer vervolgens **Toepassen**. (Selecteren **alle abonnementen** Hiermee wordt de selectie van alle of geen van de Azure-abonnementen.)
    
        ![Selecteer Azure-abonnementen][3]
    
    1. In het linkerdeelvenster worden de opslagaccounts weergegeven die aan de geselecteerde Azure-abonnementen zijn gekoppeld.
    
        ![Geselecteerde Azure-abonnementen][4]

1. Zoek de storage-account van de testomgeving:

    1. Zoek in het linkerdeelvenster van Opslagverkenner en vouw het knooppunt voor de Azure-abonnement dat eigenaar is van het lab.
    
    1. Vouw onder het knooppunt van het abonnement, **Opslagaccounts**.

    1. Vouw van het lab storage-account om weer te geven van de knooppunten voor **Blobcontainers**, **bestandsshares**, **wachtrijen**, en **tabellen**.
    
    1. Vouw de **Blobcontainers** knooppunt.
    
    1. Selecteer het uploaden van blob-container om de inhoud ervan in het rechterdeelvenster weer te geven.
        
        ![Map uploaden][5]

1. De VHD-bestand uploaden met Storage Explorer:

    1. In het rechter deelvenster van de Storage Explorer, ziet u een overzicht van de blobs in de **uploadt** blob-container van het opslagaccount van het lab. Selecteer op de werkbalk van de editor blob **uploaden** 
        
        ![Knop Uploaden][6]
    
    1. Uit de **uploaden** vervolgkeuzelijst in het menu **bestanden uploaden...** .
    
    1. Op de **bestanden uploaden** dialoogvenster, selecteer het weglatingsteken.
        
        ![Bestand selecteren][8]  

    1. Op de **bestanden selecteren voor uploaden** dialoogvenster, blader naar de gewenste VHD-bestand, selecteert u deze en selecteer vervolgens **Open**.
    
    1. Wanneer geretourneerd naar de **bestanden uploaden** dialoogvenster wijziging **Blobtype** naar **pagina-Blob**.
    
    1. Selecteer **Uploaden**.

        ![Bestand selecteren][9]  
    
    1. Opslagverkenner **activiteitenlogboek** deelvenster toont de downloadstatus (samen met koppelingen naar de upload annuleren). Het proces van het uploaden van een VHD-bestand mag langdurige, afhankelijk van de grootte van het VHD-bestand en de verbindingssnelheid. 

        ![Status van het bestand uploaden][10]  

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste installatiekopie maken in Azure DevTest Labs van een VHD-bestand met de Azure portal](devtest-lab-create-template.md)
- [Een aangepaste installatiekopie maken in Azure DevTest Labs van een VHD-bestand met behulp van PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

[0]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-image-using-psh.png
[1]: ./media/devtest-lab-upload-vhd-using-storage-explorer/settings-icon.png
[2]: ./media/devtest-lab-upload-vhd-using-storage-explorer/add-account-link.png
[3]: ./media/devtest-lab-upload-vhd-using-storage-explorer/subscriptions-list.png
[4]: ./media/devtest-lab-upload-vhd-using-storage-explorer/storage-accounts-list.png
[5]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-dir.png
[6]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-button.png
[7]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-files.png
[8]: ./media/devtest-lab-upload-vhd-using-storage-explorer/select-file.png
[9]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-file.png
[10]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-status.png
