---
title: Bedreigingsinformatie in Azure Security Center | Microsoft Docs
description: In dit document wordt uitgelegd hoe u de mogelijkheden van bedreigingsinformatie in Azure Security Center gebruikt voor het identificeren van mogelijke bedreigingen op uw virtuele machines en computers.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: 34474892d35c2c0bc299bbc4a2fb6507ffb24ee9
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022217"
---
# <a name="security-alerts-map-azure-security-center"></a>Beveiligingswaarschuwingen toewijzen Azure Security Center
In dit artikel helpt u bij het gebruik van de Azure Security Center security waarschuwingen kaart en beveiliging op basis van gebeurtenissen bedreigingsinformatiekaart beveiligingsgerelateerde om problemen te verhelpen.

## <a name="how-the-security-alerts-map-works"></a>Hoe werkt in de beveiligingswaarschuwingen worden toegewezen
Security Center biedt u een map die u helpt beveiligingsbedreigingen voor de omgeving identificeren. U kunt bijvoorbeeld bepalen of een bepaalde computer deel uitmaakt van een botnet en wanneer de bedreiging die afkomstig zijn van. Computers gaan knooppunten in een botnet wanneer aanvallers malware installeren waarmee bedieningscentrale communiceert met opdrachten en besturing die het botnet beheren. 

Voor het bouwen van deze kaart maakt gebruik van Security Center gegevens die afkomstig zijn uit meerdere bronnen binnen Microsoft. Security Center gebruikt deze gegevens voor het toewijzen van mogelijke bedreigingen voor uw omgeving. 

Een van de stappen van de [procedure om te reageren op een beveiligingsincident](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) is het identificeren van de ernst van het aangevallen systeem. In deze fase moet u de volgende taken uitvoeren:

- Bepaal de aard van de aanval.
- Bepaal de oorsprong van de aanval.
- Bepaal de bedoeling van de aanval. Was de aanval gericht op uw organisatie om specifieke gegevens te verkrijgen, of was het een willekeurige aanval?
- Stel vast welke systemen zijn aangevallen.
- Stel vast welke bestanden zijn geopend en bepaal de gevoeligheid van die bestanden.

U kunt de toewijzing van beveiliging waarschuwingen in Security Center gebruiken om te helpen bij deze taken.

## <a name="access-the-security-alerts-map"></a>Toegang tot de beveiliging waarschuwingen overzicht
Als u wilt de huidige bedreigingen in uw omgeving visualiseren, open de toewijzing van de waarschuwingen beveiliging:

1. Open het dashboard van **Security Center**.
2. In het linkerdeelvenster onder **Threat Protection** Selecteer **Security waarschuwingen kaart**. De kaart wordt geopend.
3. Voor meer informatie over de waarschuwing en herstelstappen ontvangen, klikt u op de waarschuwing stip op de kaart en volg de instructies. 
 
De toewijzing van de waarschuwingen beveiliging is gebaseerd op waarschuwingen. Deze waarschuwingen zijn gebaseerd op activiteiten die voor welk netwerk communicatie is gekoppeld aan een IP-adres dat is opgelost, is al dan niet het IP-adres een bekende riskante IP-adres (bijvoorbeeld, een bekende cryptominer is) of een IP-adres dat wordt niet herkend eerder als riskant. De kaart bevat waarschuwingen over abonnementen die u eerder hebt geselecteerd in Azure. 

De waarschuwingen op de kaart worden weergegeven op basis van de geografische locatie waar ze worden gedetecteerd als zijnde afkomstig van, en ze hebben een kleurcode op basis van ernst. 
 
## <a name="viewing-the-event-based-threat-intelligence-dashboard"></a>Het dashboard Bedreigingsinformatie op basis van gebeurtenissen weergeven
Als u de bedreigingsinformatiekaart op basis van onbewerkte beveiligingsgebeurtenissen, kunt u deze procedure te volgen. Deze kaart bevat alleen de gebeurtenissen die betrekking hebben op een IP-adres dat wordt beschouwd als risico's, bijvoorbeeld een IP-adres van een bekend botnet.

1. Open het dashboard van **Security Center**.

1. In het linkerdeelvenster onder **Threat Protection** Selecteer **Security waarschuwingen kaart**. De kaart wordt geopend.
2. Klik in de rechterbovenhoek, **gaat u naar beveiliging gebeurtenissen kaart**.
3. Selecteer de werkruimte waarvan u wilt om het dashboard weer te geven.
4. Selecteer aan de bovenkant van de kaart **klassieke bedreigingsinformatie weer**. De **Bedreigingsinformatie** dashboard wordt geopend.

   > [!NOTE]
   > Als in de meest rechtse kolom de tekst **UPGRADE ABONNEMENT** staat, is deze werkruimte gekoppeld aan het gratis abonnement. Voer een upgrade naar de Standard-versie uit om deze functie te gebruiken. Als in de meest rechtse kolom **VEREIST UPDATE** staat, moet u [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bijwerken om deze functie te gebruiken. Lees Prijzen van Azure Security Center voor meer informatie over de prijzen.
   >
5. Als er meer dan één werkruimte is om te onderzoeken, bepaalt u de prioriteit van het onderzoek via de kolom **Schadelijk IP**. Hier ziet u het huidige aantal schadelijke IP-adressen in deze werkruimte. Selecteer de werkruimte die u wilt gebruiken. Het dashboard **Bedreigingsinformatie** wordt nu weergegeven.

    ![Bedreigingsinformatie](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

6. Het dashboard is onderverdeeld in vier tegels:

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
