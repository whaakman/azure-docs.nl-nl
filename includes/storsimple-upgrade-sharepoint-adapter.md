---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 1ebbce89c7a977dd9620bd8be67da647c107eadc
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55888548"
---
### <a name="upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-storsomple-adapter-for-sharepoint"></a>Upgrade van SharePoint 2010 voor SharePoint 2013 en installeer vervolgens de StorSomple-Adapter voor SharePoint
> [!IMPORTANT]
> Alle bestanden die eerder zijn verplaatst naar de externe opslag, via de Resourcestructuur zijn is niet beschikbaar totdat de upgrade is voltooid en de functie Resourcestructuur opnieuw is ingeschakeld. Als u wilt beperken impact op gebruikers, uitvoeren een upgrade of installatie tijdens gepland onderhoud.
> 
> 

#### <a name="to-upgrade-sharepoint-2010-to-sharepoint-2013-and-then-install-the-adapter"></a>Om te upgraden van SharePoint 2010 voor SharePoint 2013 en installeer vervolgens de adapter
1. Houd er rekening mee de BLOB store-pad voor de externalized BLOBs en de inhoudsdatabases waarvoor Resourcestructuur is ingeschakeld in de farm SharePoint 2010. 
2. Installeer en configureer het nieuwe SharePoint 2013-farm. 
3. Databases, toepassingen en siteverzamelingen verplaatsen uit de SharePoint 2010-farm aan de nieuwe SharePoint 2013-farm. Ga voor instructies naar [overzicht van het upgradeproces naar SharePoint 2013](https://technet.microsoft.com/library/cc262483.aspx).
4. De StorSimple-Adapter voor SharePoint op de nieuwe farm installeren. Ga naar [de StorSimple-Adapter voor SharePoint installeren](#install-the-storsimple-adapter-for-sharepoint) voor procedures.
5. Met de informatie die u in stap 1 hebt genoteerd, Resourcestructuur inschakelen voor dezelfde set inhoudsdatabases en geef het pad met dezelfde BLOB store die is gebruikt in de SharePoint 2010-installatie. Ga naar [configureren Resourcestructuur](#configure-rbs) voor procedures. Nadat u deze stap hebt voltooid, is eerder externalized bestanden moeten toegankelijk is vanaf de nieuwe farm. 

### <a name="upgrade-the-storsimple-adapter-for-sharepoint"></a>De StorSimple-Adapter voor SharePoint bijwerken
> [!IMPORTANT]
> U moet plannen dat deze upgrade worden uitgevoerd tijdens een geplande onderhoudsvenster om de volgende redenen:
> 
> * Eerder zich externalized inhoud niet beschikbaar totdat de adapter is geïnstalleerd.
> * Alle inhoud die geüpload naar de site nadat u de vorige versie van de StorSimple-Adapter voor SharePoint hebt verwijderd, maar voordat u de nieuwe versie installeert worden, opgeslagen in de inhoud van de database. U moet overstappen van die inhoud op het StorSimple-apparaat nadat u de nieuwe-adapter installeren. U kunt de Microsoft` RBS Migrate()` PowerShell-cmdlet die deel uitmaakt van SharePoint voor het migreren van de inhoud. Zie voor meer informatie, [inhoud migreren naar of uit Resourcestructuur](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="to-upgrade-the-storsimple-adapter-for-sharepoint"></a>De StorSimple-Adapter voor SharePoint bijwerken
1. De vorige versie van StorSimple-Adapter voor SharePoint verwijderen.
   
   > [!NOTE]
   > Hiermee wordt automatisch Resourcestructuur uitgeschakeld op de inhoudsdatabases. Bestaande BLOBs blijft echter op het StorSimple-apparaat. Omdat Resourcestructuur is uitgeschakeld en de BLOBs niet zijn gemigreerd naar de inhoudsdatabases, mislukken alle aanvragen voor deze BLOBs. 
   > 
   > 
2. De nieuwe StorSimple-Adapter voor SharePoint installeren. De nieuwe adapter automatisch de inhoudsdatabases die eerder zijn ingeschakeld of uitgeschakeld voor Resourcestructuur herkent en gebruikt de vorige instellingen.

