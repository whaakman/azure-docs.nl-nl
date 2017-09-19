---
title: Bedreigingsinformatie in Azure Security Center | Microsoft Docs
description: In dit document wordt uitgelegd hoe u de mogelijkheden van bedreigingsinformatie in Azure Security Center gebruikt voor het identificeren van mogelijke bedreigingen in uw virtuele machines en computers.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: c492662aa3ee444347c55d9771790573ad969150
ms.contentlocale: nl-nl
ms.lasthandoff: 09/14/2017

---
# <a name="threat-intelligence-in-azure-security-center"></a>Bedreigingsinformatie in Azure Security Center
In dit document wordt uitgelegd hoe u bedreigingsinformatie van Azure Security Center gebruikt voor het aanpakken van beveiligingsproblemen.

## <a name="what-is-threat-intelligence"></a>Wat is bedreigingsinformatie?
Met de optie Bedreigingsinformatie van Security Center kunnen IT-beheerders beveiligingsbedreigingen voor de omgeving identificeren. Zo kunnen ze bijvoorbeeld bepalen of een bepaalde computer deel uitmaakt van een botnet. Computers kunnen knooppunten worden in een botnet wanneer aanvallers malware installeren waarmee de computer wordt verbonden met de bedieningscentrale. Er kunnen ook potentiële bedreigingen mee worden geïdentificeerd die afkomstig zijn van underground communicatiekanalen zoals darknet.

Voor het samenstellen van deze bedreigingsinformatie gebruikt Security Center gegevens die afkomstig zijn uit meerdere bronnen binnen Microsoft. Security Center gebruikt deze gegevens voor het identificeren van mogelijke bedreigingen voor uw omgeving. Het deelvenster Bedreigingsinformatie bestaat uit drie belangrijke onderdelen:

- Gedetecteerde bedreigingstypen
- Bedreigingsoorsprong
- Bedreigingsinformatiekaart


## <a name="when-should-i-use-threat-intelligence"></a>Wanneer moet ik bedreigingsinformatie gebruiken?
Een van de stappen van een [beveiligingsincidentresponsproces](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) is het identificeren van de ernst van het gecompromitteerde systeem. In deze fase moet u de volgende taken uitvoeren:

- Bepaal de aard van de aanval
- Bepaal de oorsprong van de aanval
- Bepaal de bedoeling van de aanval. Was de aanval specifiek gericht op uw organisatie om specifieke gegevens te verkrijgen, of was het een willekeurige aanval?
- Identificeren welke systemen zijn gecompromitteerd
- Identificeer de bestanden waartoe toegang is gekregen en bepaal de gevoeligheid van deze bestanden. U kunt Bedreigingsinformatie van Security Center gebruiken als hulpmiddel bij deze taken. 

## <a name="how-to-access-the-threat-intelligence"></a>Hoe open ik de bedreigingsinformatie?
Als u de huidige bedreigingsinformatie voor uw omgeving wilt visualiseren, moet u eerst de werkruimte selecteren waarin uw gegevens zich bevinden. Als u maar één werkruimte hebt, wordt de werkruimteselector niet weergegeven en gaat u rechtstreeks naar het dashboard **Bedreigingsinformatie**. Volg onderstaande stappen om het dashboard Bedreigingsinformatie te openen:

1.  Open het dashboard **Security Center**.
2.  Klik in het linkerdeelvenster onder **Detectie** op **Bedreigingsinformatie**. Het dashboard **Bedreigingsinformatie** wordt weergegeven.

    ![Bedreigingsinformatie](./media/security-center-threat-intel/security-center-threat-intel-fig1.png)

    > [!NOTE]
    > Als in de laatste kolom **UPGRADE PLAN** wordt weergegeven, betekent dit dat deze werkruimte gebruikmaakt van het gratis abonnement en u een upgrade naar Standaard moet uitvoeren als u deze functie wilt gebruiken. Als er REQUIRES UPDATE wordt weergegeven, dan moet u [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bijwerken om deze functie te gebruiken. Lees Prijzen van Azure Security Center voor meer informatie over de prijzen. 
    > 
3. Als er meer dan één werkruimte moet worden onderzocht, kunt u een onderzoeksprioriteit aangeven volgens de kolom **MALICIOUS IP**. In deze kolom wordt het huidige aantal kwaadaardige IP’s in deze werkruimte weergegeven. Selecteer de werkruimte die u wilt gebruiken. Daarna wordt het dashboard **Bedreigingsinformatie** weergegeven.

    ![Bedreigingsinformatie](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

4. Dit dashboard is onderverdeeld in vier tegels:
    * **Bedreigingstypen**: geeft een overzicht van het type bedreigingen dat in de geselecteerde werkruimte is geselecteerd.
    * **Land van oorsprong**: combineert de hoeveelheid verkeer op basis van de bronlocatie.
    * **Bedreigingslocatie**: helpt u bij het identificeren van de huidige locaties die wereldwijd met uw omgeving communiceren. Er zijn oranje (inkomende) en rode (uitgaande) pijlen op deze kaart die de richting van het verkeer aangeven. Als u op een van deze pijlen klikt, wordt het type bedreiging en de richting van het verkeer weergegeven.
    * **Bedreigingsdetails**: toont meer informatie over de bedreiging die u op de kaart hebt geselecteerd.

Ongeacht de geselecteerde optietegel is het dashboard dat wordt weergegeven, gebaseerd op de query [Zoeken in logboeken](https://docs.microsoft.com/azure/security-center/security-center-search). Het enige verschil is het type query en het resultaat.

### <a name="threat-types"></a>Bedreigingstypen
Nadat u op de tegel **Bedreigingstypen** hebt geklikt, wordt het dashboard **Zoeken in logboeken** weergegeven. Daar ziet u links de filteropties en rechts de queryresultaten.

![Zoeken in bedreigingsinformatie](./media/security-center-threat-intel/security-center-threat-intel-fig3.png)

Het queryresultaat toont combinaties van bedreigingen op naam. In het linkerdeelvenster kunt u het kenmerk selecteren waarop u wilt filteren. Als u bijvoorbeeld alleen de bedreigingen wilt zien die op dat moment zijn verbonden met computers, selecteert u **Verbonden** in de **SESSIONSTATE** en klikt u op de knop **Toepassen**.

![sessiestatus](./media/security-center-threat-intel/security-center-threat-intel-fig4.png)

Voor Azure-VM’s worden alleen de netwerkgegevens die via de agent lopen, weergegeven in het dashboard Bedreigingsinformatie. De volgende gegevenstypen worden ook gebruikt door bedreigingsinformatie:

- CEF-gegevens (Type=CommonSecurityLog)
- WireData (Type=WireData)
- IIS-logboeken (Type=W3CIISLog)
- Windows Firewall (Type=WindowsFirewall)
- DNS-gebeurtenissen (Type=DnsEvents)


## <a name="see-also"></a>Zie ook
In dit document hebt u geleerd hoe u bedreigingsinformatie in Security Center gebruikt om u te helpen bij het identificeren van verdachte activiteiten. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center. 
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.


