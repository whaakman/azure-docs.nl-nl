---
title: Bedreigingen in Azure Security Center | Microsoft Docs
description: " Detectie van bedreigingen en schadelijke door de Microsoft Cloud App Security integratie met Azure Security Center. "
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: rkarlin
ms.openlocfilehash: af7896ec4afaeefda7261542bf593a89a7bb9ae8
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551788"
---
# <a name="ueba-for-azure-resources-and-users"></a>UEBA voor Azure-resources en gebruikers 

Azure Security Center-partners met Microsoft Cloud App Security om u waarschuwingen op basis van gebruiker and entity Behavior analytics (UEBA) voor uw Azure-resources en gebruikers (Azure-activiteit). Deze waarschuwingen detecteren afwijkingen in het gedrag van gebruikers en zijn gebaseerd op UEBA en machine learning (ML), zodat u onmiddellijk geavanceerde dreigingsdetectie kunt uitvoeren op de activiteiten van uw abonnementen. Omdat ze automatisch worden ingeschakeld, bieden de nieuwe detecties van afwijkend gedrag onmiddellijk resultaten doordat ze meteen detecties aanleveren, op basis van verschillende gedragsafwijkingen van de gebruikers en resources die aan uw abonnement zijn gekoppeld. Bovendien maken deze waarschuwingen gebruik van aanvullende gegevens die al aanwezig zijn in de detectie-engine van Microsoft Cloud App Security, waardoor het onderzoeksproces nog verder wordt versneld en u altijd op de hoogte bent van doorlopende dreigingen. 

> [!NOTE]
> Azure Security Center, die mogelijk Sla een kopie van gegevens van de klant met betrekking tot beveiliging, die worden verzameld uit of die zijn gekoppeld aan een klant-resource (bijvoorbeeld virtuele machine of Azure Active Directory-tenant): (a) in hetzelfde geografische gebied als de resource met uitzondering van in deze geografische gebieden waar Microsoft nog niet is implementeren van Azure Security Center, in dat geval een kopie van dergelijke gegevens worden opgeslagen in de Verenigde Staten; en (b) waar Azure Security Center maakt gebruik van een andere Microsoft Online Service voor het verwerken van dergelijke gegevens, die gegevens in overeenstemming met de geolocatie van de regels van die andere Online Service kunnen worden opgeslagen.
>

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="prerequisites"></a>Vereisten

- Een geldige geactiveerd [Microsoft Cloud App Security-licentie](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security)
- [Security Center Standard-laag](https://azure.microsoft.com/pricing/details/security-center/)
 
## <a name="threat-detection-alerts"></a>Meldingen voor geconstateerde bedreigingen

Security Center ondersteunt afwijkingsdetectiewaarschuwingen van Cloud App Security, zoals:

**Onmogelijk traject**
-  Deze detectie identificeert twee gebruikersactiviteiten (is één of meerdere sessies) die afkomstig zijn van geografisch verafgelegen locaties binnen een periode die korter is dan de tijd die het zou hebben genomen de gebruiker voor een van de eerste locatie reis naar de tweede, waarmee wordt aangegeven dat een andere gebruiker dezelfde referenties gebruikt is. Deze detectie maakt gebruik van een machine learning-algoritme dat wordt genegeerd voor de hand liggende 'valse positieven' bijdragen aan de voorwaarde onmogelijk traject, zoals VPN-verbindingen en de locaties die regelmatig worden gebruikt door andere gebruikers in de organisatie. De detectie is een eerste leerperiode van zeven dagen gedurende welke het activiteitenpatroon van een nieuwe gebruiker hoort.

**Activiteit vanuit niet-gangbaar land**
- Deze detectie rekening gehouden met het verleden activiteit locaties om te bepalen van de nieuwe en onregelmatige locaties. De anomaliedetectie-engine bevat informatie over de voorgaande locaties die worden gebruikt door gebruikers in de organisatie. Een waarschuwing wordt geactiveerd wanneer een activiteit plaatsvindt op een locatie die niet onlangs of nooit door een gebruiker in de organisatie bezocht is. 

**Activiteit vanaf anonieme IP-adressen**
- Deze detectie identificeert dat gebruikers actief vanaf een IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres zijn. Deze proxy's worden gebruikt door mensen die u wilt verbergen, IP-adres van hun apparaat en kunnen voor kwade bedoelingen worden gebruikt. Deze detectie maakt gebruik van een machine learning-algoritme dat vermindert 'valse positieven', zoals onjuiste label IP-adressen die veel door gebruikers in de organisatie worden gebruikt.
 
  ![Threat detection waarschuwing](./media/security-center-ueba-mcas/security-center-mcas-alert.png)

## <a name="disabling-threat-detection-alerts"></a>Meldingen voor geconstateerde bedreigingen uitschakelen

Deze waarschuwingen zijn standaard ingeschakeld, maar u kunt ze uitschakelen:

1. Selecteer in de blade Security Center **prijzen & stellingen** en selecteer het abonnement van toepassing.
2. Klik op **detectie van bedreigingen**.
3. Onder **inschakelen integraties**, schakel het selectievakje **toestaan dat Microsoft Cloud App Security voor toegang tot mijn gegevens**, en klikt u op **opslaan**.

   ![Threat detection waarschuwing](./media/security-center-ueba-mcas/security-center-mcas-optout.png)

> [!NOTE]
> Er is een eerste leerperiode van zeven dagen gedurende welke niet alle anomaly detectiewaarschuwingen worden gegenereerd. Na deze periode wordt elke sessie vergeleken met de activiteiten, de tijd dat gebruikers actief waren, de IP-adressen, de apparaten, enzovoort die de afgelopen maand zijn gedetecteerd en wordt naar de risicoscore van deze activiteiten gekeken. Deze detecties maken deel uit van de machine learning-anomaliedetectie-engine die profielen voor uw omgeving en activeert waarschuwingen met betrekking tot een basislijn die is geleerd over de activiteit van uw organisatie. Deze detecties maken ook gebruik van machine learning-algoritmen is ontworpen voor het profiel van de gebruikers en het aanmelden met een patroon voor fout-positieven.
>
  
## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd u werken met detectie van bedreigingen in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.



<!--Image references-->
[1]: ./media/security-center-confidence-score/confidence-score.png
[2]: ./media/security-center-confidence-score/suspicious-confidence-score.png
