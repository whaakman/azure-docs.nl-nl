---
author: rayne-wiselman
ms.service: backup
ms.topic: include
ms.date: 11/09/2018
ms.author: raynew
ms.openlocfilehash: b589c88e5b5c5991db43a9f3c10003e17094b2e1
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54057370"
---
## <a name="defining-a-backup-policy"></a>Een back-upbeleid definiëren
Een back-upbeleid definieert een matrix met wanneer de momentopnamen van de gegevens zijn gemaakt en hoe lang deze momentopnamen worden bewaard. Bij het definiëren van een beleid voor het maken van een back-up van een virtuele machine, kunt u ervoor zorgen dat er *eenmaal per dag* een back-uptaak wordt uitgevoerd. Wanneer u een nieuw beleid maakt, wordt dit toegepast op de kluis. De interface voor het back-upbeleid ziet er als volgt uit:

![Back-upbeleid](./media/backup-create-policy-for-vms/backup-policy.png)

Ga als volgt te werk om een beleid te maken:

1. Voer een naam in voor de **beleidsnaam**.
2. Momentopnamen van uw gegevens kunnen met tussenpozen van een dag of een week worden gemaakt. Gebruik de vervolgkeuzelijst **Back-upfrequentie** om te kiezen of u wilt dat er dagelijks of wekelijks momentopnamen worden gemaakt.

   * Als u voor een dagelijks interval kiest, moet u met het gemarkeerde besturingselement het tijdstip op de dag kiezen waarop de momentopname moet worden gemaakt. Als u het tijdstip wilt wijzigen, maakt u de selectie voor het huidige tijdstip ongedaan en selecteert u het nieuwe tijdstip.

     ![Dagelijks back-upbeleid](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>
   * Als u voor een wekelijkse interval kiest, moet u met het gemarkeerde besturingselement de dag(en) van de week en het tijdstip op de dag kiezen waarop de momentopname moet worden gemaakt. Selecteer een of meer dagen in het menu met de dagen. Selecteer een tijdstip in het menu met de uren. Als u het tijdstip wilt wijzigen, maakt u de selectie voor het huidige tijdstip ongedaan en selecteert u het nieuwe tijdstip.

     ![Wekelijks back-upbeleid](./media/backup-create-policy-for-vms/backup-policy-weekly.png)
3. Standaard zijn alle opties voor een **bewaartermijn** geselecteerd. Schakel de limieten voor de bewaartermijnen uit die u niet wilt gebruiken. Geef vervolgens een of meer intervallen op die moeten worden gebruikt.

    Door bewaartermijnen van een maand en een jaar te gebruiken, kunt u opgeven dat momentopnamen met tussenstappen van een week of een dag worden gemaakt.

   > [!NOTE]
   >
  - Als u een virtuele machine beveiligt, wordt één keer per dag een back-uptaak uitgevoerd. Het tijdstip waarop de back-up wordt uitgevoerd, is voor elke bewaartermijn hetzelfde.
  - Herstelpunt dat is gegenereerd op de datum en tijd wanneer de back-upmomentopname is voltooid ongeacht wanneer de back-uptaak planning is.
    - Bijvoorbeeld Als de back-upfrequentie is gepland om 11:30 uur en als gevolg van een probleem momentopname om 12:01 uur is voltooid, wordt het herstelpunt worden gemaakt met de volgende datum en 12:01 AM.
  - Een maandelijkse back-up worden als back-up is ingesteld om uit te voeren op de eerste dag van elke maand, en als de momentopname is voltooid op de volgende dag vanwege een probleem opgetreden tijdens vervolgens het herstelpunt voor maandelijkse back-up is gemaakt, gemarkeerd met de volgende dag (zoals) tweede van de maand).
   >
   >


4. Nadat u alle opties voor het beleid hebt ingesteld, klikt u boven in de blade op **Opslaan**.

    Het nieuwe beleid wordt onmiddellijk op de kluis toegepast.
