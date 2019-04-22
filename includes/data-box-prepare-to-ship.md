---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: a8070d25e2606d8ad72ac231a0a208072c612c5c
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59737051"
---
De laatste stap is de voorbereiding van het apparaat voor verzending. In deze stap worden alle shares van apparaten offline geplaatst. De shares zijn niet toegankelijk als u dit proces hebt gestart.

> [!IMPORTANT]
> Voorbereiding voor verzending is vereist omdat gegevens die niet aan de naamgevingsregels van Azure voldoen, worden gemarkeerd. Deze stap overslaan kan leiden tot mogelijke gegevens uploaden storingen als gevolg van niet-conforme gegevens.

1. Ga naar **Voorbereiden voor verzending** en klik op **Voorbereiden starten**. Standaard worden controlesommen inline berekend tijdens de voorbereiding op verzending. Het berekenen van de controlesom kan, afhankelijk van de grootte van uw gegevens, uren tot dagen duren. 
   
    ![Voorbereiding voor verzending 1](media/data-box-prepare-to-ship/prepare-to-ship1.png)

    Als u om een bepaalde reden wilt stoppen met de voorbereiding van het apparaat, klikt u op **Voorbereiding stoppen**. U kunt de voorbereiding voor verzending later hervatten.
        
    ![Voorbereiding voor verzending 2](media/data-box-prepare-to-ship/prepare-to-ship2.png)
    
2. De voorbereiding voor verzending wordt gestart en de apparaat-shares gaan offline. Er wordt een herinnering om het verzendlabel te downloaden weergegeven zodra het apparaat klaar is.

    ![Herinnering voor downloaden van verzendlabel](media/data-box-prepare-to-ship/download-shipping-label-reminder.png)

3. De status van het apparaat wordt bijgewerkt naar *Gereed voor verzending* en het apparaat wordt vergrendeld nadat de voorbereiding van het apparaat is voltooid.
        
    ![Voorbereiding voor verzending 3](media/data-box-prepare-to-ship/prepare-to-ship3.png)

    Als u meer gegevens naar het apparaat wilt kopiëren, kunt u het apparaat ontgrendelen, meer gegevens kopiëren en de voorbereiding voor verzending opnieuw uitvoeren.

    Als deze stap fouten bevat, moet u het foutenlogboek downloaden en de fouten oplossen. Nadat de fouten zijn opgelost, voert u **Voorbereiding voor verzending** uit.

4. Nadat de voorbereiding voor verzending met succes is voltooid (zonder fouten), downloadt u de lijst met bestanden (ook wel bekend als het manifest) die in dit proces zijn gekopieerd. U kunt deze lijst later gebruiken om de bestanden te verifiëren die naar Azure zijn geüpload.
        
    ![Voorbereiding voor verzending 1](media/data-box-prepare-to-ship/prepare-to-ship4.png)

5. Schakel het apparaat uit. Ga naar de pagina **Uitschakelen of opnieuw opstarten** en klik op **Afsluiten**. Klik op **OK** om verder te gaan als u om bevestiging wordt gevraagd.

6. Verwijder de kabels. U moet het apparaat vervolgens naar Microsoft verzenden.
