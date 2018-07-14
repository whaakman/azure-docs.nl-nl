---
title: Toepassen van updates in Azure Stack | Microsoft Docs
description: Informatie over het importeren en installeer Microsoft update-pakketten voor een geïntegreerde Azure Stack-systeem.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: mabrigg
ms.openlocfilehash: da8261d27ae7fad3c5ff30e4e1cce3f1bca2b70a
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035330"
---
# <a name="apply-updates-in-azure-stack"></a>In Azure Stack-updates toepassen

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Als Azure Stack-operators, kunt u Microsoft toepassen of OEM-updatepakketten voor Azure Stack met behulp van de Update-tegel in de beheerdersportal. U moet het updatepakket downloaden, het pakketbestanden importeren in Azure Stack en vervolgens het updatepakket te installeren. 

## <a name="download-the-update-package"></a>Het updatepakket downloaden

Als een Microsoft- of OEM-updatepakket voor Azure Stack beschikbaar is, het pakket downloaden naar een locatie die bereikbaar is vanaf de Azure Stack en de pakketinhoud controleren. Een updatepakket bestaat gewoonlijk uit de volgende bestanden:

- Een zelfuitpakkende *PackageName*.exe-bestand. Dit bestand bevat de nettolading voor de update, bijvoorbeeld de meest recente cumulatieve update voor Windows Server.   
- Bijbehorende *PackageName*.bin-bestanden. Deze bestanden vindt u compressie voor de nettolading die gekoppeld aan de *PackageName*.exe-bestand. 
- Een Metadata.xml-bestand. Dit bestand bevat essentiële informatie over de update, bijvoorbeeld de uitgever, de naam, de vereiste, de grootte en het pad-URL voor ondersteuning.

## <a name="import-and-install-updates"></a>Importeren en updates installeren

De volgende procedure beschrijft het importeren en -updatepakketten installeren in de beheerdersportal.

> [!IMPORTANT]
> Wordt aangeraden dat u op de hoogte stellen gebruikers van elke onderhoudsbewerkingen, en dat u plant dat normale onderhoudsvensters tijdens de kantooruren zo veel mogelijk. Onderhoudsbewerkingen mogelijk van invloed op zowel de werkbelastingen van de gebruiker en de portal bewerkingen.

1. Selecteer in de beheerdersportal **meer services**. Klik vervolgens onder de **gegevens en opslag** categorie, selecteer **opslagaccounts**. (Of begint te typen in het filtervak **opslagaccounts**, en selecteer deze.)

    ![Laat zien waar u kunt storage-accounts vinden in de portal](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Typ in het filtervak **bijwerken**, en selecteer de **updateadminaccount** storage-account.

    ![Laat zien hoe u om te zoeken naar updateadminaccount](media/azure-stack-apply-updates/ApplyUpdates2.png)

3. In de opslag account details onder **Services**, selecteer **Blobs**.
 
    ![Laat zien hoe u Blobs voor het opslagaccount ophalen](media/azure-stack-apply-updates/ApplyUpdates3.png) 
 
4. Onder **Blob-service**, selecteer **+ Container** om een container te maken. Voer een naam (bijvoorbeeld *Update 1709*), en selecteer vervolgens **OK**.
 
     ![Laat zien hoe u een container in de storage-account toevoegen](media/azure-stack-apply-updates/ApplyUpdates4.png)

5. Nadat de container is gemaakt, klikt u op de containernaam van de en klik vervolgens op **uploaden** de pakketbestanden uploaden naar de container.
 
    ![Laat zien hoe u de pakketbestanden uploaden](media/azure-stack-apply-updates/ApplyUpdates5.png)

6. Onder **blob uploaden**, klik op het mappictogram, blader naar het updatepakket .exe-bestand en klik vervolgens op **Open** in het bestand explorer-venster.
  
7. Onder **blob uploaden**, klikt u op **uploaden**. 
 
    ![Laat zien waar elke pakketbestand uploaden](media/azure-stack-apply-updates/ApplyUpdates6.png)

8. Herhaal stap 6 en 7 dagen voor de *PackageName*.BIN-bestand en Metadata.xml-bestanden. Importeer het bestand aanvullende Notice.txt niet als opgenomen.
9. Wanneer u klaar bent, kunt u de meldingen (belpictogram in de rechterbovenhoek van de portal) bekijken. De meldingen moeten aangeven dat het uploaden is voltooid. 
10. Ga terug naar de tegel van de Update op het dashboard. De tegel aangeven dat een update beschikbaar is. Klik op de tegel om te controleren van het zojuist toegevoegde updatepakket.
11. Als u wilt de update installeert, selecteert u het pakket dat gemarkeerd als **gereed** en met de rechtermuisknop op het pakket en selecteert u **nu bijwerken**, of klik op de **nu bijwerken** actie aan de bovenkant .
12. Als u het updatepakket installeren klikt, ziet u de status in de **Update-uitvoering details** gebied. Hier kunt u kunt ook klikken op **volledige logboeken downloaden die u** voor het downloaden van de logboekbestanden.
13. Wanneer de update is voltooid, wordt de tegel Update de bijgewerkte versie van Azure Stack.

## <a name="next-steps"></a>Volgende stappen

- [Updates beheren in Azure Stack-overzicht](azure-stack-updates.md)
- [Azure Stack servicebeleid](azure-stack-servicing-policy.md)
