---
title: Beveiligde score in Azure Security Center | Microsoft Docs
description: " Uw aanbevelingen voor beveiliging met behulp van de beveiligde score in Azure Security Center een prioriteit geven. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: rkarlin
ms.openlocfilehash: fc521db9ad753c4162b65abfd2f9f23c318fa994
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46131320"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Verbetering van uw beveiligde score in Azure Security Center


Met zo veel services biedt de voordelen van beveiliging, is het vaak moeilijk te weten welke stappen voor het eerst duren om te beveiligen en beveiliging van uw workload. De beveiligde score van Azure Security Center controleert uw aanbevelingen voor beveiliging en bepaalt de volgorde van deze voor u, zodat u welke aanbevelingen weet voor het eerst uitvoeren zodat u het meest ernstige beveiligingsproblemen zoeken, zodat u prioriteiten onderzoek aanbrengen kunt. Beveiligde score is een meting hulpprogramma waarmee u de beveiliging van uw beveiliging voor het bereiken van een beveiligde werkbelasting.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]


![Score dashboard beveiligen](./media/security-center-secure-score/secure-score-dashboard.png)

## <a name="secure-score-calculation"></a>Berekening van de risicoscore beveiligen

Security Center lijkt op het werk van de beveiligingsanalist, uw aanbevelingen voor beveiliging controleren en geavanceerde algoritmen om te bepalen hoe essentieel is voor elke aanbeveling toe te passen.
Azure Security center voortdurend beoordeelt u actieve aanbevelingen en uw beveiligde score op basis van deze berekent, de score van een aanbeveling is afgeleid van de ernst en security best practices die gevolgen voor de beveiliging van uw workload de meeste hebben.

De **score secure** is een berekening op basis van de verhouding tussen uw resources in orde is en uw totale resources. Als het aantal resources in orde gelijk aan het totale aantal resources is, krijgt u de maximaal veilige score van 50. Om te proberen op te halen van uw beveiligde score dichter naar de maximale score, corrigeer de resources niet in orde door de aanbevelingen te volgen.

Security Center biedt u ook met een algemene beveiligde score. 

**Totale score secure** is een opeenstapeling van al uw aanbevelingen. Uw algemene beveiligde score vindt u in uw abonnementen of beheergroepen, afhankelijk van wat u hebt geselecteerd. De score varieert op basis van het geselecteerde abonnement en de actieve aanbevelingen in deze abonnementen.

 

U kunt de meeste impact hebben top 3-aanbevelingen weergeven in het dashboard van Security Center om te controleren welke aanbevelingen zijn de meeste invloed hebben op uw beveiligde score, of kunt u de aanbevelingen voor het gebruik van blade aanbevelingen lijst sorteren de **beveiligde score impact** kolom.


Uw algemene beveiligde score weergeven:

1. Klik in het Azure-dashboard op **Security Center** en klik vervolgens op **aanbevelingen**.
2. Bovenaan ziet u de beveiligde score, die staat voor de score per beleid per geselecteerde abonnement. 
2. In de onderstaande tabel met een lijst met de aanbevelingen, kunt u zien dat voor elke aanbeveling er is een kolom die staat voor de **score impact Secure**. Dit getal staat hoeveel uw algehele beveiligde score als u Volg de aanbevelingen wordt verbeterd. Bijvoorbeeld, in het scherm hieronder als u **beveiligingsproblemen in container-beveiligingsconfiguraties herstellen**, uw beveiligde score worden verhoogd met 35 punten.

   ![beveiligen van score](./media/security-center-secure-score/security-center-secure-score1.png)

## <a name="individual-secure-score"></a>Afzonderlijke beveiligde score

Bovendien, als u wilt weergeven van afzonderlijke beveiligde scores, kunt u deze in de blade afzonderlijke aanbeveling vinden.  

De **aanbeveling secure score** is een berekening op basis van de verhouding tussen uw resources in orde is en uw totale resources. Als het aantal resources in orde gelijk aan het totale aantal resources is, krijgt u de maximaal veilige score van de aanbeveling. Als u wilt proberen op te halen van uw beveiligde score dichter naar de maximale score, corrigeer de resources niet in orde door de herstelstappen te volgen.

De **aanbeveling impact** kunt u hoeveel uw beveiligde score wordt verbeterd weten als u de stappen van de aanbeveling toepast. Bijvoorbeeld als uw beveiligde score 42 is en de **aanbeveling impact** + 3, als u de stappen die worden beschreven in de aanbeveling uitvoeren uw beveiligde score om te worden 45 wordt verbeterd.

De aanbeveling ziet u welke bedreigingen voor uw workload wordt blootgesteld aan als de stappen voor herstel zijn niet in gebruik.

![afzonderlijke aanbeveling beveiligde score](./media/security-center-secure-score/indiv-recommendation-secure-score.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over het verbeteren van uw beveiliging verbeteren met **beveiligde score** in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.


