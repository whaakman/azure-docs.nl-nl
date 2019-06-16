---
title: Beveiligde score in Azure Security Center | Microsoft Docs
description: " Uw aanbevelingen voor beveiliging met behulp van de beveiligde score in Azure Security Center een prioriteit geven. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/15/2019
ms.author: v-mohabe
ms.openlocfilehash: c23c9a2b9af1947c397b96431ae3c3bcd0e30aaa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65968298"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Verbetering van uw beveiligde score in Azure Security Center


Met zo veel services biedt de voordelen van beveiliging, is het vaak moeilijk te weten welke stappen voor het eerst duren om te beveiligen en beveiliging van uw workload. De Azure beveiligde score controleert uw aanbevelingen voor beveiliging en bepaalt de volgorde van deze voor u, zodat u welke aanbevelingen weet voor het eerst uitvoert. Zo kunt u het meest ernstige beveiligingsproblemen zoeken, zodat u prioriteiten onderzoek aanbrengen kunt. Beveiligde score is een hulpprogramma waarmee u bij het bepalen van de beveiligingsstatus van uw workload.

## <a name="secure-score-calculation"></a>Berekening van de risicoscore beveiligen

Security Center lijkt op het werk van een beveiligingsanalist, controleren van uw aanbevelingen voor beveiliging en het toepassen van geavanceerde algoritmen om te bepalen hoe essentieel is voor elke aanbeveling.
Azure Security center voortdurend beoordeelt u actieve aanbevelingen en uw beveiligde score op basis van deze berekent, de score van een aanbeveling is afgeleid van de ernst en best practices voor beveiliging die gevolgen voor de beveiliging van uw workload de meeste hebben.

Security Center biedt u ook een **secure totale score**. 

**Totale score secure** een opeenstapeling van alle scores van de aanbeveling is. Uw algemene beveiligde score vindt u in uw abonnementen of beheergroepen, afhankelijk van wat u hebt geselecteerd. De score varieert op basis van het geselecteerde abonnement en de actieve aanbevelingen in deze abonnementen.

 
Als u wilt controleren welke aanbevelingen van invloed zijn op uw beveiligde score meest, kunt u de drie belangrijkste impact aanbevelingen bekijken in het dashboard van Security Center of kunt u de aanbevelingen voor het gebruik van blade aanbevelingen lijst sorteren de **beveiligde score impact** kolom.


Uw algemene beveiligde score weergeven:

1. Klik in het Azure-dashboard op **Security Center** en klik vervolgens op **beveiligde score**.
2. Bovenaan ziet u Secure score belangrijke functies:
   - De **Algehele veiligheidsscore** vertegenwoordigt de score per beleid, per geselecteerd abonnement
   - **Score beveiligen op categorie** ziet u welke resources de meeste aandacht nodig
   - **Belangrijkste aanbevelingen beveiligen door score impact** biedt u een lijst met aanbevelingen die uw beveiligde score verbeteren de meeste als u ze implementeert.
 
   ![beveiligen van score](./media/security-center-secure-score/secure-score-dashboard.png)

3. In de onderstaande tabel ziet u al uw abonnementen en de algemene beveiligde score voor elke.

   > [!NOTE]
   > De som van de veiligheidsscore van elk abonnement staat niet gelijk aan de algehele veiligheidsscore. De veiligheidsscore is een berekening op basis van de ratio tussen uw gezonde resources en uw totale resources per aanbeveling, niet een som van veiligheidsscores in uw abonnementen. 
   >
4. Klik op **aanbevelingen weergeven** om te zien van de aanbevelingen voor het abonnement die u ter verbetering van uw beveiligde score kunt herstellen.
4. In de lijst met aanbevelingen, kunt u zien dat voor elke aanbeveling er is een kolom die staat voor de **score impact Secure**. Dit getal staat hoeveel uw algehele beveiligde score als u Volg de aanbevelingen wordt verbeterd. Bijvoorbeeld, in het scherm hieronder als u **beveiligingsproblemen in container-beveiligingsconfiguraties herstellen**, uw beveiligde score worden verhoogd met 35 punten.

   ![beveiligen van score](./media/security-center-secure-score/security-center-secure-score1.png)



## <a name="individual-secure-score"></a>Afzonderlijke beveiligde score

Bovendien, als u wilt weergeven van afzonderlijke beveiligde scores, kunt u deze in de blade afzonderlijke aanbeveling vinden.  

De **Aanbevelingsveiligheidsscore** is een berekening op basis van de ratio tussen uw gezonde resources en uw totale resources per aanbeveling. Als het aantal gezonde resources gelijk staat aan het aantal resources, krijgt u de maximale aanbevelingsscore van 50. Verbeter de slechte resources door de aanbevelingen over te nemen en verbeter zo uw veiligheidsscore.

Met de **Gevolgen van de aanbeveling** weet u hoeveel u uw veiligheidsscore moet verbeteren als u de aanbevelingsstappen toepast. Als u veiligheidsscore bijvoorbeeld 42 is, en de **Gevolgen van de aanbeveling** is +3, verbetert u uw score tot 45 door de stappen in de aanbeveling te volgen.

De aanbeveling ziet u welke bedreigingen voor uw workload wordt blootgesteld aan als de stappen voor herstel zijn niet in gebruik.

![afzonderlijke aanbeveling beveiligde score](./media/security-center-secure-score/indiv-recommendation-secure-score.png)







## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over het verbeteren van uw beveiliging verbeteren met **beveiligde score** in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.


