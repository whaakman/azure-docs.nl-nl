---
title: VHD-bestand uploaden naar Azure DevTest Labs met behulp van Microsoft Azure Storage Explorer | Microsoft Docs
description: VHD-bestand uploaden naar het lab storage-account met behulp van Microsoft Azure Storage Explorer
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: v-craic
ms.openlocfilehash: 25675aae77fbe2610fe416210de9a306c1c09f3d
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="upload-vhd-file-to-labs-storage-account-using-microsoft-azure-storage-explorer"></a>VHD-bestand uploaden naar het lab storage-account met behulp van Microsoft Azure Storage Explorer

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

In Azure DevTest Labs kunnen VHD-bestanden worden gebruikt voor het maken van aangepaste installatiekopieën die worden gebruikt voor het inrichten van virtuele machines. In dit artikel laat zien hoe u [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) een VHD-bestand uploaden naar een lab-opslagaccount. Nadat u uw VHD-bestand hebt geüpload de [Vervolgstappen sectie](#next-steps) geeft een lijst van sommige artikelen die laten zien hoe u een aangepaste installatiekopie van het geüploade VHD-bestand. Zie voor meer informatie over schijven en VHD's in Azure [over schijven en virtuele harde schijven voor virtuele machines](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

De volgende stappen helpt u bij het uploaden van een VHD-bestand voor het gebruik van DevTest Labs [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).

1. [Download en installeer de nieuwste versie van Microsoft Azure Storage Explorer](http://www.storageexplorer.com).

1. Haal de naam van het lab-opslagaccount met de Azure portal:

    1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
    
    1. Selecteer **Meer services** en selecteer in de lijst vervolgens **DevTest Labs**.
    
    1. Selecteer de gewenste testomgeving uit de lijst van labs.  
    
    1. Selecteer op de labblade **configuratie**. 
    
    1. Op de testomgeving **configuratie** blade Selecteer **aangepaste installatiekopieën (VHD's)**.
    
    1. Op de **aangepaste installatiekopieën** blade Selecteer **+ toevoegen**. 
    
    1. Op de **aangepaste installatiekopie** blade Selecteer **VHD**.
    
    1. Op de **VHD** blade Selecteer **een VHD uploaden met PowerShell**.
    
        ![Virtuele harde schijf met behulp van PowerShell geüpload][0]
    
    1. De **een installatiekopie uploaden met PowerShell** blade wordt een aanroep van de **Add-AzureVhd** cmdlet. De eerste parameter (*bestemming*) bevat de naam van het opslagaccount voor de testomgeving in de volgende indeling:
    
        https://<STORAGE-account-name>.BLOB.Core.Windows.NET/uploads/... 

    1. Noteer de opslagaccountnaam omdat deze wordt gebruikt in latere stappen.
    
1. Verbinding maken met een Azure-abonnementsrekening met Opslagverkenner.

    > [!TIP] 
    > 
    > Opslagverkenner ondersteunt verschillende verbindingsopties. Deze sectie ziet u verbinding maakt met een opslagaccount die is gekoppeld aan uw Azure-abonnement. Overzicht van de andere verbindingsopties ondersteund door Opslagverkenner verwijzen naar het artikel [aan de slag met Opslagverkenner](../vs-azure-tools-storage-manage-with-storage-explorer.md).
 
    1. Open Opslagverkenner.
    
    1. Selecteer in Opslagverkenner **Azure Accountinstellingen**. 
    
        ![Azure-accountinstellingen][1]
    
    1. Het linkerdeelvenster geeft de Microsoft-accounts die u hebt aangemeld. Selecteer **Een account toevoegen** om verbinding te maken met een ander account en volg de dialoogvensters om aan te melden met een Microsoft-account dat is gekoppeld aan ten minste één actief Azure-abonnement.
    
        ![Een account toevoegen][2]
    
    1. Wanneer u bent aangemeld met een Microsoft-account, worden in het linkerdeelvenster de Azure-abonnementen weergegeven die aan dat account zijn gekoppeld. Selecteer de Azure-abonnementen waarmee u wilt werken en selecteer vervolgens **Toepassen**. (Selecteren **alle abonnementen** Hiermee wordt de selectie van alle of geen van de vermelde Azure-abonnementen.)
    
        ![Selecteer Azure-abonnementen][3]
    
    1. In het linkerdeelvenster worden de opslagaccounts weergegeven die aan de geselecteerde Azure-abonnementen zijn gekoppeld.
    
        ![Geselecteerde Azure-abonnementen][4]

1. Zoek in het lab-opslagaccount:

    1. Zoek in het linkerdeelvenster van Opslagverkenner en vouw het knooppunt voor de Azure-abonnement dat eigenaar is van de testomgeving.
    
    1. Vouw onder het knooppunt van het abonnement, **Opslagaccounts**.

    1. De testomgeving storage accountknooppunt uitvouwen om te onthullen knooppunten voor **Blob-Containers**, **bestandsshares**, **wachtrijen**, en **tabellen**.
    
    1. Vouw de **Blob-Containers** knooppunt.
    
    1. Selecteer de blob-container uploads aan de inhoud in het rechterdeelvenster.
        
        ![Directory uploaden][5]

1. Het VHD-bestand met Opslagverkenner uploaden:

    1. In het rechterdeelvenster Opslagverkenner ziet u een overzicht van de blobs in de **uploadt** blob-container van het lab-opslagaccount. Selecteer op de werkbalk van de editor blob **uploaden** 
        
        ![Knop Uploaden][6]
    
    1. Van de **uploaden** vervolgkeuzelijst, selecteer **bestanden uploaden...** .
    
    1. Op de **bestanden uploaden** dialoogvenster, selecteer het weglatingsteken.
        
        ![Bestand selecteren][8]  

    1. Op de **Selecteer bestanden uploaden** dialoogvenster, blader naar het gewenste VHD-bestand en selecteer vervolgens selecteert u het **Open**.
    
    1. Wanneer u keert terug naar de **bestanden uploaden** dialoogvenster wijziging **Blob-type** naar **pagina-Blob**.
    
    1. Selecteer **Uploaden**.

        ![Bestand selecteren][9]  
    
    1. Opslagverkenner **activiteitenlogboek** deelvenster toont de downloadstatus (samen met koppelingen naar de upload geannuleerd). Het proces van het uploaden van een VHD-bestand kan worden langdurige, afhankelijk van de grootte van het VHD-bestand en de verbindingssnelheid. 

        ![Uploadbestand status][10]  

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
