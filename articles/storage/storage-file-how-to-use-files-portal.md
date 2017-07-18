---
title: Azure File Storage beheren vanuit Azure Portal | Microsoft Docs
description: Meer informatie over het gebruik van Azure Portal om Azure File Storage te beheren.
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 58353a8058c2722788a38d2ca596061d14b68caf
ms.contentlocale: nl-nl
ms.lasthandoff: 07/12/2017

---

# <a name="how-to-use-azure-file-storage-from-the-azure-portal"></a>Azure File Storage gebruiken vanuit Azure Portal
[Azure Portal](https://portal.azure.com) biedt een gebruikersinterface voor het beheren van Azure File Storage. U kunt de volgende acties uitvoeren vanuit de webbrowser:

* Een bestandsshare maken
* Bestanden uploaden naar en downloaden van de bestandsshare.
* Het werkelijke gebruik van elke bestandsshare controleren.
* Het quotum van de sharegrootte aanpassen.
* De te gebruiken koppelingsopdrachten kopiëren om uw bestandsshare te koppelen vanuit een Windows- of Linux-client.

## <a name="create-file-share"></a>Bestandsshare maken
1. Meld u aan bij Azure Portal.
2. Klik in het navigatiemenu op **Opslagaccounts** of **Opslagaccounts (klassiek)**.
    
    ![Schermafbeelding van het maken van een bestandsshare in de portal](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. Kies uw opslagaccount.

    ![Schermafbeelding van het maken van een bestandsshare in de portal](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. Kies de service Bestanden.

    ![Schermafbeelding van het maken van een bestandsshare in de portal](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. Klik op Bestandsshares en volg de koppeling om uw eerste bestandsshare te maken.

    ![Schermafbeelding van het maken van een bestandsshare in de portal](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. Vul de naam en de grootte (maximaal 5120 GB) van de bestandsshare in om uw eerste bestandsshare te maken. Wanneer de bestandsshare is gemaakt, kunt u deze koppelen vanuit elk bestandssysteem dat SMB 2.1 of SMB 3.0 ondersteunt. U kunt in de bestandsshare op **Quotum** klikken om de grootte van het bestand van maximaal 5120 GB te wijzigen. Raadpleeg de [Azure-prijscalculator](https://azure.microsoft.com/pricing/calculator/) voor een indicatie van de opslagkosten van het gebruik van Azure File Storage.

    ![Schermafbeelding van het maken van een bestandsshare in de portal](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a>Bestanden uploaden en downloaden
1. Kies een bestandsshare die u al hebt gemaakt.

    ![Schermafbeelding van het uploaden en downloaden van bestanden vanuit de portal](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. Klik op **Uploaden** om de gebruikersinterface voor het uploaden van bestanden te openen.

    ![Schermafbeelding van het uploaden van bestanden vanuit de portal](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-to-file-share"></a>Verbinding maken met de bestandsshare
-  Klik op **Verbinden** om de opdrachtregel op te halen voor het koppelen van de bestandsshare vanuit Windows en Linux. Voor Linux-gebruikers kunt u ook verwijzen naar [Azure File Storage gebruiken met Linux](storage-how-to-use-files-linux.md) voor koppelingsinstructies voor andere Linux-distributies.

    ![Schermafbeelding van het koppelen van de bestandsshare](media/storage-file-how-to-use-files-portal/use-files-portal-connect.png)
-  U kunt de opdrachten voor het koppelen van de bestandsshare in Windows of Linux kopiëren en deze uitvoeren vanaf uw Azure VM of lokale machine.

    ![Schermafbeelding van de koppelingsopdrachten voor Windows en Linux](media/storage-file-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

**Tip:**  
Voor het vinden van de toegangssleutel voor de opslagaccount die u wilt gebruiken voor het koppelen, klikt u op **Toegangssleutels voor dit opslagaccount weergeven** onder aan de pagina waar u verbinding maakt.

## <a name="see-also"></a>Zie ook
Raadpleeg de volgende koppelingen voor meer informatie over Azure File Storage.

* [Veelgestelde vragen](storage-files-faq.md)
* [Problemen oplossen](storage-troubleshoot-file-connection-problems.md)
