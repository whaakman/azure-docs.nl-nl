---
title: Bewaking en Responding to Security Alerts in de beveiliging van Operations Management Suite en Audit oplossing | Microsoft Docs
description: Dit document helpt u de threat intelligence optie die beschikbaar zijn in de OMS-beveiliging en controle gebruiken om te controleren en reageren op beveiligingswaarschuwingen.
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 7d45a32b-1341-4bb5-a436-1f42a8a2590a
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2017
ms.author: yurid
ms.openlocfilehash: df82afab2c38431e134146143524edc080ee38f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Controleren en reageren op beveiligingswaarschuwingen in de beveiliging van Operations Management Suite en Audit-oplossing
Dit document helpt u met de threat intelligence optie die beschikbaar zijn in de OMS-beveiliging en controle kunt bewaken en hierop reageren op beveiligingswaarschuwingen.

## <a name="what-is-oms"></a>Wat is OMS?
Microsoft Operations Management Suite (OMS) is van Microsoft-cloud-gebaseerde IT-beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur. Lees het artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx) voor meer informatie over OMS.

## <a name="threat-intelligence"></a>Informatie over bedreigingen
In een bedrijfsomgeving waar gebruikers ruime toegang hebben tot het netwerk en een verscheidenheid aan apparaten gebruiken voor verbinding met zakelijke gegevens, is het noodzakelijk actief uw resources bewaken en snel reageren op beveiligingsincidenten. Dit is bijzonder belangrijk vanuit het perspectief van de levenscyclus van beveiliging, omdat sommige cybersecurity bedreigingen kunnen niet verhogen waarschuwingen of verdachte activiteiten die kunnen worden geïdentificeerd door technische traditionele beveiligingsmechanismen. 

Met behulp van de **dreigingen** optie beschikbaar in de OMS-beveiliging en controle, IT-beheerders kunnen identificeren bedreigingen van de omgeving, bijvoorbeeld, identificeren als een bepaalde computer deel uitmaakt van een [ botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Computers kunnen knooppunten worden in een botnet wanneer aanvallers malware installeren waarmee de computer wordt verbonden met de bedieningscentrale. Mogelijke bedreigingen die afkomstig zijn van ondergrondse communicatiekanalen, zoals kan ook worden aangegeven [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

Om deze dreigingen maken, gebruikt de oplossing OMS beveiligings- en Audit gegevens die afkomstig zijn uit meerdere bronnen binnen Microsoft. OMS beveiligings- en Audit zal gebruikmaken van deze gegevens om te identificeren mogelijke bedreigingen van uw omgeving.

Het deelvenster Bedreigingsinformatie bestaat uit drie belangrijke onderdelen:

* Servers met uitgaand schadelijk verkeer
* Typen gedetecteerde bedreigingen
* Bedreigingsinformatiekaart

> [!NOTE]
> Lees voor een overzicht van al deze opties [aan de slag met beveiliging van Operations Management Suite-oplossing van de Audit en](oms-security-getting-started.md).
> 
> 

### <a name="responding-to-security-alerts"></a>Reageren op beveiligingswaarschuwingen
Een van de stappen van een [beveiliging respons op incidenten](https://technet.microsoft.com/library/cc512623.aspx) proces is het identificeren van de ernst van de inbreuk-systemen. In deze fase moet u de volgende taken uitvoeren:

* Bepaal de aard van de aanval
* Bepaal de oorsprong van de aanval
* Bepaal de bedoeling van de aanval. Was de aanval specifiek gericht op uw organisatie om specifieke gegevens te verkrijgen, of was het een willekeurige aanval?
* Identificeren welke systemen zijn gecompromitteerd
* Identificeren van de bestanden die zijn geopend en de gevoeligheid van die bestanden bepalen

U kunt gebruikmaken van **dreigingen** informatie in de OMS-beveiligings- en Audit-oplossing om te helpen bij deze taken. Volg de stappen hieronder om naar deze **dreigingen** opties:

1. Klik in het hoofddashboard van **Microsoft Operations Management Suite** op de tegel **Beveiliging en controle**.
   
    ![Beveiliging en controle](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. In de **beveiligings- en Audit** dashboard ziet u de **dreigingen** opties in de rechterkant zoals hieronder wordt weergegeven:
   
    ![Threat intel](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Deze drie tegels krijgt u een overzicht van de huidige bedreigingen. In de **Server met uitgaand schadelijk verkeer** kun je om te bepalen of er is een computer die u wilt controleren (binnen of buiten uw netwerk) is dat het schadelijk verkeer verzenden naar Internet. 

De **gedetecteerd threat typen** tegel geeft een samenvatting weer van bedreigingen die huidige 'in het wild zijn", als u op deze tegel klikt ziet u meer informatie over deze bedreigingen zoals u hierna ziet:

![Gedetecteerde bedreigingstypen](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

U kunt meer informatie over elke bedreiging uitpakken door erop te klikken. Het volgende voorbeeld ziet meer informatie over Botnet:

![meer informatie over een bedreiging](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Zoals beschreven in het begin van deze sectie, kan deze informatie nuttig zijn bij een aanvraag voor respons op incidenten. Het kan ook worden belangrijke tijdens een forensisch onderzoek, waarin u wilt de bron van de aanval, welk systeem is geknoeid en de tijdlijn vinden. In dit rapport dat u een aantal belangrijke details van de aanval zoals eenvoudig kunt herkennen: de bron van de aanval, het lokale IP-adres waarmee is geknoeid en de status van de huidige sessie van de verbinding. 

De **threat intelligence kaart** helpt u bij de huidige locaties wereldwijd waarvoor schadelijk verkeer te identificeren. Er zijn oranje (inkomend) en rood (uitgaand) pijlen in deze kaart die de richting van verkeer, identificeren als u in een van deze pijlen klikt, wordt het het soort dreiging en de richting van verkeer zoals hieronder weergegeven:

![kaart met gegevens van bedreigingen](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [!NOTE]
> U kunt een demonstratie zien over het gebruik van deze mogelijkheid tijdens een respons op incidenten volgen de presentatie [datacenter beveiligingsrisico's verhelpen met begeleide onderzoek met Operations Management Suite](https://myignite.microsoft.com/videos/5000) geleverd bij Microsoft Ignite.
> 

### <a name="responding-to-distinct-malicious-ip-accessed"></a>Reageren op unieke schadelijke IP geopend
In sommige scenario's ziet u mogelijk een potentieel schadelijke IP-adres dat is geopend vanaf een bewaakte computer:

![kaart met gegevens van bedreigingen](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.PNG)

Deze waarschuwing en anderen binnen dezelfde categorie worden gegenereerd via OMS-beveiliging door gebruik te maken van [bedreigingsinformatie van Microsoft](https://youtu.be/O4WtxgUrDc8). Deze informatie over bedreigingen wordt verzameld door Microsoft, maar ook gekocht van toonaangevende leveranciers van dit type informatie. De informatie wordt regelmatig bijgewerkt en aangepast als er nieuwe bedreigingen zijn ontdekt. Vanwege de aard van de informatie, moet deze worden gecombineerd met andere bronnen van beveiligingsgegevens tijdens het [onderzoeken](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) van een beveiligingswaarschuwing. 

## <a name="customize-alerts-received-via-e-mail"></a>Meldingen ontvangen per e-mail aanpassen

U kunt aanpassen welke gebruikers in uw organisatie wordt gewaarschuwd wanneer er beveiligingswaarschuwingen worden geactiveerd door OMS-beveiliging. Deze optie is beschikbaar onder overzicht / instellingen op het dashboard OMS:

![E-mail](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig7.png)

## <a name="see-also"></a>Zie ook
In dit document hebt u geleerd hoe u de **dreigingen** optie in OMS beveiligings- en Audit-oplossing om te reageren op beveiligingswaarschuwingen. Raadpleeg de volgende artikelen voor meer informatie over OMS Beveiliging:

* [Overzicht van Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Aan de slag met beveiliging van Operations Management Suite en Audit-oplossing](oms-security-getting-started.md)
* [Resources bewaken in de oplossing Beveiliging en controle van Operations Management Suite ](oms-security-monitoring-resources.md)

