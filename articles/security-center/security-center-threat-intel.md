---
title: Bedreigingsinformatie in Azure Security Center | Microsoft Docs
description: In dit document wordt uitgelegd hoe u de mogelijkheden van bedreigingsinformatie in Azure Security Center gebruikt voor het identificeren van mogelijke bedreigingen op uw virtuele machines en computers.
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
ms.date: 09/12/2017
ms.author: yurid
ms.openlocfilehash: b5bdeb1c70893c2477413df02c7b1a4a07999e93
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="threat-intelligence-in-azure-security-center"></a>Bedreigingsinformatie in Azure Security Center
In dit artikel wordt uitgelegd hoe u bedreigingsinformatie van Azure Security Center gebruikt voor het aanpakken van beveiligingsproblemen.

## <a name="what-is-threat-intelligence"></a>Wat is bedreigingsinformatie?
Met de optie Bedreigingsinformatie van Security Center kunnen IT-beheerders beveiligingsbedreigingen voor de omgeving identificeren. Zo kunnen ze bijvoorbeeld bepalen of een bepaalde computer deel uitmaakt van een botnet. Computers kunnen knooppunten worden in een botnet wanneer aanvallers malware installeren waarmee de computer wordt verbonden met een computer van de aanvallers. Bedreigingsinformatie kan ook worden gebruikt om potentiële bedreigingen te identificeren die afkomstig zijn van underground communicatiekanalen, zoals het 'dark web'.

Voor het samenstellen van deze bedreigingsinformatie gebruikt Security Center gegevens die afkomstig zijn uit meerdere bronnen binnen Microsoft. Security Center gebruikt deze gegevens voor het identificeren van mogelijke bedreigingen voor uw omgeving. Het deelvenster **Bedreigingsinformatie** bestaat uit drie belangrijke onderdelen:

- Gedetecteerde bedreigingstypen
- Bedreigingsoorsprong
- Bedreigingsinformatiekaart


## <a name="when-should-you-use-threat-intelligence"></a>Wanneer moet ik bedreigingsinformatie gebruiken?
Een van de stappen van de [procedure om te reageren op een beveiligingsincident](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) is het identificeren van de ernst van het aangevallen systeem. In deze fase moet u de volgende taken uitvoeren:

- Bepaal de aard van de aanval.
- Bepaal de oorsprong van de aanval.
- Bepaal de bedoeling van de aanval. Was de aanval gericht op uw organisatie om specifieke gegevens te verkrijgen, of was het een willekeurige aanval?
- Stel vast welke systemen zijn aangevallen.
- Stel vast welke bestanden zijn geopend en bepaal de gevoeligheid van die bestanden.

U kunt bedreigingsinformatie in Security Center gebruiken om te helpen bij deze taken. 

## <a name="access-the-threat-intelligence"></a>Hoe open ik de bedreigingsinformatie?
Als u de huidige bedreigingsinformatie voor uw omgeving wilt visualiseren, moet u eerst de werkruimte selecteren waarin uw gegevens zich bevinden. Als u maar één werkruimte hebt, wordt de werkruimteselector niet weergegeven en gaat u rechtstreeks naar het dashboard **Bedreigingsinformatie**. Het dashboard openen:

1. Open het dashboard van **Security Center**.

2. Selecteer **Bedreigingsinformatie** in het linkerdeelvenster onder **Detectie**. Het dashboard **Bedreigingsinformatie** wordt weergegeven.

    ![Het dashboard Bedreigingsinformatie](./media/security-center-threat-intel/security-center-threat-intel-fig1.png)

    > [!NOTE]
    > Als in de meest rechtse kolom de tekst **UPGRADE ABONNEMENT** staat, is deze werkruimte gekoppeld aan het gratis abonnement. Voer een upgrade naar de Standard-versie uit om deze functie te gebruiken. Als in de meest rechtse kolom **VEREIST UPDATE** staat, moet u [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bijwerken om deze functie te gebruiken. Lees Prijzen van Azure Security Center voor meer informatie over de prijzen. 
    > 
3. Als er meer dan één werkruimte is om te onderzoeken, bepaalt u de prioriteit van het onderzoek via de kolom **Schadelijk IP**. Hier ziet u het huidige aantal schadelijke IP-adressen in deze werkruimte. Selecteer de werkruimte die u wilt gebruiken. Het dashboard **Bedreigingsinformatie** wordt nu weergegeven.

    ![Bedreigingsinformatie](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

4. Het dashboard is onderverdeeld in vier tegels:

    a.  **Bedreigingstypen**. Hier ziet u een overzicht van het type bedreigingen dat in de geselecteerde werkruimte is gedetecteerd.

    b.  **Land van oorsprong**. Hier wordt de hoeveelheid verkeer gecombineerd op basis van de bronlocatie.

    c.  **Locatie van de bedreiging**. Helpt u bij het identificeren van de huidige locaties die wereldwijd met uw omgeving communiceren. Op de kaart geven de oranje (inkomend) en rode (uitgaand) pijlen de richtingen van het verkeer aan. Als u een van deze pijlen selecteert, ziet u het type bedreiging en de richting van het verkeer.

    d.  **Details van de bedreiging**. Hier ziet u meer informatie over de bedreiging die u op de kaart hebt geselecteerd.

Ongeacht de tegel die u selecteert, is het dashboard dat wordt weergegeven altijd gebaseerd op het logboek van Zoeken in logboeken. Het enige verschil is het type query en het resultaat.

### <a name="threat-types"></a>Bedreigingstypen
Selecteer de tegel **Bedreigingstypen** om het dashboard **Zoeken in logboeken** te openen. U ziet filteropties aan de linkerkant en de queryresultaten aan de rechterkant.

![Zoeken in logboeken](./media/security-center-threat-intel/security-center-threat-intel-fig3.png)

In het queryresultaat worden de bedreigingen op naam aangegeven. Gebruik het linkerdeelvenster om het kenmerk te selecteren waarop u wilt filteren. Als u bijvoorbeeld alleen de bedreigingen wilt zien die momenteel zijn verbonden met de computers, selecteert u **Verbonden** > **Toepassen** bij **SESSIONSTATE**.

![Sessiestatus](./media/security-center-threat-intel/security-center-threat-intel-fig4.png)

Voor VM’s van Azure bevat het dashboard **Bedreigingsinformatie** alleen de netwerkgegevens die via de agent lopen. De volgende gegevenstypen worden ook gebruikt door bedreigingsinformatie:

- CEF-gegevens (Type=CommonSecurityLog)
- WireData (Type=WireData)
- IIS-logboeken (Type=W3CIISLog)
- Windows Firewall (Type=WindowsFirewall)
- DNS-gebeurtenissen (Type=DnsEvents)


## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd hoe u bedreigingsinformatie in Security Center gebruikt om u te helpen bij het identificeren van verdachte activiteiten. Zie de volgende artikelen voor meer informatie over Security Center:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center. 
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Zoek antwoorden op veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.

