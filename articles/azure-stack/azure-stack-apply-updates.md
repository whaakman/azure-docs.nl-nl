---
title: Toepassen van updates in Azure Stack | Microsoft Docs
description: Informatie over het importeren en installeer Microsoft update-pakketten voor een geïntegreerde Azure Stack-systeem.
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
ms.date: 02/11/2019
ms.author: mabrigg
ms.reviewer: justini
ms.lastreviewed: 02/11/2019
ms.openlocfilehash: 9eca22f5a594bf3d61b1d68882c4853f4bfc499d
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58100748"
---
# <a name="apply-updates-in-azure-stack"></a>In Azure Stack-updates toepassen

*Van toepassing op: Azure Stack-geïntegreerde systemen*

U kunt de **bijwerken** tegel in de beheerportal om toe te passen van Microsoft of OEM-updatepakketten voor Azure Stack.

Als u van een geïntegreerde systemen versie 1807 gebruikmaakt of eerder gebruikt, moet u het updatepakket downloaden, de pakketbestanden importeren in Azure Stack en installeer vervolgens het updatepakket. Zie voor instructies [Update Azure Stack door het pakket downloaden](#update-azure-stack-by-downloading-the-package)

Deze instructies werken met geïntegreerde Azure Stack-systemen bijwerken. Als u van de ontwikkelomgeving voor Azure Stack gebruikmaakt, moet u het installatiepakket voor de huidige versie te downloaden. Zie voor instructies [Azure Stack Development Kit installeren](./asdk/asdk-install.md)

## <a name="update-azure-stack"></a>Update Azure Stack

### <a name="select-and-apply-an-update-package"></a>Selecteer en toepassen van een updatepakket

1. Open de beheerportal.

2. Selecteer **Dashboard**. Selecteer de **Update** tegel.

    ![Azure Stack is een update beschikbaar](media/azure-stack-apply-updates/azure-stack-updates-1901-dashboard.png)

3. Noteer de huidige versie van uw Azure Stack. U kunt bijwerken naar de volgende volledige versie. Bijvoorbeeld als u met Azure Stack 1811, de volgende versie uitgebracht is 1901.

    ![Azure Stack-update van toepassing](media/azure-stack-apply-updates/azure-stack-updates-1901-updateavailable.png)

4. Selecteer de volgende beschikbare versie in de lijst met Updates. U kunt selecteren **weergave** in de release notes kolom geopend, het onderwerp van de opmerkingen bij de release voor de versie als wilt controleren van wijzigingen in de versie.

5. Selecteer nu bijwerken. De update wordt gestart.

### <a name="review-update-history"></a>Historie van updates controleren

1. Open de beheerportal.

2. Selecteer **Dashboard**. Selecteer de **Update** tegel.

3. Selecteer **bijwerken geschiedenis**.

![Geschiedenis van Azure Stack-update](media/azure-stack-apply-updates/azure-stack-update-history.PNG)

## <a name="update-azure-stack-by-downloading-the-package"></a>Azure Stack door te downloaden van het pakket bijwerken

Als u van een geïntegreerde systemen versie 1807 gebruikmaakt of eerder gebruikt, moet u het updatepakket downloaden, de pakketbestanden importeren in Azure Stack en installeer vervolgens het updatepakket.

## <a name="download-the-update-package"></a>Het updatepakket downloaden

Als een Microsoft- of OEM-updatepakket voor Azure Stack beschikbaar is, het pakket downloaden naar een locatie die bereikbaar is vanaf de Azure Stack en de pakketinhoud controleren. Een updatepakket bestaat gewoonlijk uit de volgende bestanden:

- Een zelfuitpakkende `<PackageName>.exe` bestand. Dit bestand bevat de nettolading voor de update, bijvoorbeeld de meest recente cumulatieve update voor Windows Server.

- Bijbehorende `<PackageName>.bin` bestanden. Deze bestanden vindt u compressie voor de nettolading die gekoppeld aan de *PackageName*.exe-bestand.

- Een `Metadata.xml` bestand. Dit bestand bevat essentiële informatie over de update, bijvoorbeeld de uitgever, de naam, de vereiste, de grootte en het pad-URL voor ondersteuning.

> [!IMPORTANT]  
> Nadat de Azure Stack 1901 update-pakket is toegepast, wordt de verpakkingsindeling voor Azure Stack update pacakges worden verplaatst van .exe, .bin(s), en XML-indeling naar een .zip(s) en XML-indeling. Geen invloed op de Azure Stack-operators die verbinding hebben gemaakt van stempels. Azure Stack-operators die niet zijn verbonden wordt de XML- en ZIP-bestanden te importeren met behulp van dezelfde procedure die hieronder worden beschreven.

## <a name="import-and-install-updates"></a>Importeren en updates installeren

De volgende procedure beschrijft het importeren en -updatepakketten installeren in de beheerdersportal.

> [!IMPORTANT]  
> Wordt aangeraden dat u op de hoogte stellen gebruikers van elke onderhoudsbewerkingen, en dat u plant dat normale onderhoudsvensters tijdens de kantooruren zo veel mogelijk. Onderhoudsbewerkingen mogelijk van invloed op zowel de werkbelastingen van de gebruiker en de portal bewerkingen.

1. Selecteer in de beheerdersportal **alle services**. Klik vervolgens onder de **gegevens en opslag** categorie, selecteer **opslagaccounts**. (Of begint te typen in het filtervak **opslagaccounts**, en selecteer deze.)

    ![Laat zien waar u kunt storage-accounts vinden in de portal](media/azure-stack-apply-updates/ApplyUpdates1.png)

2. Typ in het filtervak **bijwerken**, en selecteer de **updateadminaccount** storage-account.

3. In de opslag account details onder **Services**, selecteer **Blobs**.
 
    ![Laat zien hoe u Blobs voor het opslagaccount ophalen](media/azure-stack-apply-updates/ApplyUpdates3.png) 

4. Onder **Blob-service**, selecteer **+ Container** om een container te maken. Voer een naam (bijvoorbeeld *Update 1811*), en selecteer vervolgens **OK**.
 
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

U kunt updates handmatig verwijderen uit het opslagaccount nadat ze zijn geïnstalleerd in Azure Stack. Azure Stack wordt regelmatig gecontroleerd voor oudere-updatepakketten en worden deze verwijderd uit de opslag. Het Azure Stack duurt twee weken om te verwijderen van de oude pakketten.

## <a name="next-steps"></a>Volgende stappen

- [Updates beheren in Azure Stack-overzicht](azure-stack-updates.md)
- [Azure Stack servicebeleid](azure-stack-servicing-policy.md)
