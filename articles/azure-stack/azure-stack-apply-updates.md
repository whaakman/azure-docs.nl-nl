---
title: Toepassen van updates in Azure-Stack | Microsoft Docs
description: "Informatie over het importeren en installeren van Microsoft update-pakketten voor een Azure-Stack geïntegreerd."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: mabrigg
ms.openlocfilehash: 10e82c13dc2bb8461fb628435484a3a6e1877aa8
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="apply-updates-in-azure-stack"></a>Toepassen van updates in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerd systemen*

Als een Azure-Stack-operator, kunt u Microsoft update-pakketten voor Azure-Stack met behulp van de Update-tegel in de beheerdersportal toepassen. U moet het pakket voor Microsoft update te downloaden, het pakketbestanden importeren naar Azure-Stack en installeer het updatepakket. 

## <a name="download-the-update-package"></a>Het updatepakket downloaden

Wanneer een Microsoft update-pakket voor Azure-Stack beschikbaar is, het pakket downloaden naar een locatie die bereikbaar is vanaf de Azure-Stack en bekijkt u de pakketinhoud. Een updatepakket bestaat gewoonlijk uit de volgende bestanden:

- Een zelfuitpakkende *PackageName*.exe-bestand. Dit bestand bevat de nettolading voor de update is bijvoorbeeld de meest recente cumulatieve update voor Windows Server.   
- Bijbehorende *PackageName*.bin-bestanden. Deze bestanden vindt u compressie voor de nettolading dat gekoppeld aan de *PackageName*.exe-bestand. 
- Een Metadata.xml-bestand. Dit bestand bevat essentiële informatie over de update, bijvoorbeeld de uitgever, de naam van vereiste, grootte en pad-URL voor ondersteuning.

## <a name="import-and-install-updates"></a>Importeren en updates installeren

De volgende procedure laat zien hoe importeren en updatepakketten installeren in de beheerdersportal.

> [!IMPORTANT]
> Wordt aangeraden dat u gebruikers van elke onderhoudsbewerkingen waarschuwen en dat u plant dat normale onderhoudsvensters tijdens buiten kantooruren zo veel mogelijk. Onderhoud kunnen zowel de werkbelastingen van de gebruiker en de portal bewerkingen beïnvloeden.

1. Selecteer in de beheerdersportal **meer services**. Klik vervolgens onder de **gegevens en opslag** categorie, selecteer **opslagaccounts**. (Of begint te typen in het filtervak **opslagaccounts**, en selecteer deze.)

    ![Waar vind ik storage-accounts in de portal bevat](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Typ in het filtervak **bijwerken**, en selecteer de **updateadminaccount** storage-account.

    ![Laat zien hoe om te zoeken naar updateadminaccount](media/azure-stack-apply-updates/ApplyUpdates2.png)

3. In de opslag account details onder **Services**, selecteer **Blobs**.
 
    ![Toont hoe u naar BLOB's voor het opslagaccount](media/azure-stack-apply-updates/ApplyUpdates3.png) 
 
4. Onder **Blob-service**, selecteer **+ Container** en een container te maken. Voer een naam (bijvoorbeeld *Update 1709*), en selecteer vervolgens **OK**.
 
     ![Laat zien hoe een container in de storage-account toevoegen](media/azure-stack-apply-updates/ApplyUpdates4.png)

5. Nadat de container is gemaakt, klikt u op de containernaam van de en klik vervolgens op **uploaden** het pakketbestanden uploaden naar de container.
 
    ![Laat zien hoe de pakketbestanden uploaden](media/azure-stack-apply-updates/ApplyUpdates5.png)

6. Onder **blob uploaden**, klik op het mappictogram, bladert u naar het updatepakket .exe-bestand en klik vervolgens op **Open** in het bestand explorer-venster.
  
7. Onder **blob uploaden**, klikt u op **uploaden**. 
 
    ![Laat zien waar elk pakketbestand uploaden](media/azure-stack-apply-updates/ApplyUpdates6.png)

8. Herhaal stap 6 en 7 voor de *PackageName*.bin en Metadata.xml-bestanden. 
9. Wanneer u klaar bent, kunt u de meldingen (belpictogram in de rechterbovenhoek van de portal) bekijken. De meldingen zou moeten aangeven dat het uploaden is voltooid. 
10. Ga terug naar de tegel Update op het dashboard. De tegel zou moeten aangeven dat er een update beschikbaar is. Klik op de tegel om te controleren van het zojuist toegevoegde updatepakket.
11. De update wilt installeren, selecteer het pakket dat gemarkeerd als **gereed** en met de rechtermuisknop op het pakket en selecteert u **nu bijwerken**, of klik op de **nu bijwerken** actie boven .
12. Als u op het updatepakket installeren, vindt u de status in de **Update-uitvoering details** gebied. Hier kunt u kunt ook klikken op **volledige logboeken downloaden** voor het downloaden van de logboekbestanden.
13. Wanneer de update is voltooid, ziet u de Update-tegel de bijgewerkte versie van de Azure-Stack.

## <a name="next-steps"></a>Volgende stappen

- [Beheren van updates in de Azure-Stack-overzicht](azure-stack-updates.md)
- [Azure Stack onderhoud van beleid](azure-stack-servicing-policy.md)
