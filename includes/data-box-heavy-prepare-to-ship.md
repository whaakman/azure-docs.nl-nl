---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/28/2019
ms.author: alkohli
ms.openlocfilehash: f5b60d862be0d71f71f770c47d88ad39f2fc6ac7
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66419571"
---
De laatste stap is de voorbereiding van het apparaat voor verzending. In deze stap worden alle shares van apparaten offline geplaatst. De shares zijn niet toegankelijk als u dit proces hebt gestart.

> [!IMPORTANT]
> Voorbereiding voor verzending is vereist omdat gegevens die niet aan de naamgevingsregels van Azure voldoen, worden gemarkeerd. Deze stap overslaan kan leiden tot mogelijke gegevens uploaden storingen als gevolg van niet-conforme gegevens.

1. Ga naar **Voorbereiden voor verzending** en klik op **Voorbereiden starten**. Standaard worden controlesommen worden berekend terwijl gegevens worden gekopieerd. Voorbereiding voor verzending het berekenen van controlesom is voltooid en wordt de lijst met bestanden wordt gemaakt (ook wel bekend als *stuklijst bestanden* of het manifest). Het berekenen van de controlesom kan, afhankelijk van de grootte van uw gegevens, uren tot dagen duren.
   
    ![Voorbereiding voor verzending 1](media/data-box-heavy-prepare-to-ship/prepare-to-ship1.png)

    Als u om een bepaalde reden wilt stoppen met de voorbereiding van het apparaat, klikt u op **Voorbereiding stoppen**. U kunt de voorbereiding voor verzending later hervatten.
        
    ![Voorbereiding voor verzending 2](media/data-box-heavy-prepare-to-ship/prepare-to-ship2.png)
    
2. De voorbereiding voor verzending wordt gestart en de apparaat-shares gaan offline. Er wordt een herinnering om het verzendlabel te downloaden weergegeven zodra het apparaat klaar is.

    ![Herinnering voor downloaden van verzendlabel](media/data-box-heavy-prepare-to-ship/download-shipping-label-reminder.png)

3. De status van het apparaat wordt bijgewerkt naar *Gereed voor verzending* en het apparaat wordt vergrendeld nadat de voorbereiding van het apparaat is voltooid.
        
    ![Voorbereiding voor verzending 3](media/data-box-heavy-prepare-to-ship/prepare-to-ship3.png)

    Als u meer gegevens naar het apparaat wilt kopiëren, kunt u het apparaat ontgrendelen, meer gegevens kopiëren en de voorbereiding voor verzending opnieuw uitvoeren.

    Als er fouten in deze stap, downloadt u het foutenlogboek en los de fouten. Nadat de fouten zijn opgelost, voert u **Voorbereiding voor verzending** uit.

4. Nadat de voorbereiding voor verzending is met succes voltooid (waarbij er geen fouten), downloaden de lijst met bestanden (ook wel bekend als *stuklijst bestanden* of het manifest) in dit proces hebt gekopieerd. 

    ![Lijst van bestanden of stuklijst bestanden downloaden](media/data-box-heavy-prepare-to-ship/download-list-of-files.png)

   U kunt deze lijst later gebruiken om de bestanden te verifiëren die naar Azure zijn geüpload. Zie voor meer informatie, [inspecteren stuklijst bestanden tijdens de voorbereiding voor verzending](../articles/databox/data-box-logs.md#inspect-bom-during-prepare-to-ship).
        
    ![Stuklijst-voorbeeldbestand](media/data-box-heavy-prepare-to-ship/sample-bom-file.png)

5. Schakel het apparaat uit. Ga naar de pagina **Uitschakelen of opnieuw opstarten** en klik op **Afsluiten**. Klik op **OK** om verder te gaan als u om bevestiging wordt gevraagd.

    ![Eerste apparaatknooppunt afsluiten](media/data-box-heavy-prepare-to-ship/shut-down-device-node.png)

6. Herhaal de bovenstaande stappen voor het andere knooppunt.
7. Zodra het apparaat volledig is afgesloten, wordt alle LED's aan het einde van het apparaat hebt uitgeschakeld. De volgende stap is het verwijderen van alle kabels en het apparaat naar Microsoft verzenden.
