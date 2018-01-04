---
title: Aan de slag met Beveiliging en controle van Operations Management Suite | Microsoft Docs
description: In dit document leest u hoe u met de mogelijkheden van Beveiliging en controle van Operations Management Suite uw hybride cloud kunt bewaken.
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 754796ef-a43e-468a-86c9-04a2eda55b5b
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.openlocfilehash: 9772e4c1022cf8609e52ccfe16376959b67e92eb
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Aan de slag met Beveiliging en controle van Operations Management Suite
In dit document worden alle opties beschreven, zodat u snel aan de slag kunt met de mogelijkheden van Beveiliging en controle van Operations Management Suite (OMS).

## <a name="what-is-oms"></a>Wat is OMS?
Microsoft Operations Management Suite (OMS) is een cloudoplossing voor IT-beheer van Microsoft waarmee u uw on-premises en cloudinfrastructuur kunt beheren en beveiligen. Lees het artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx) voor meer informatie over OMS.

## <a name="oms-security-and-audit-dashboard"></a>OMS-dashboard Beveiliging en controle
De oplossing OMS Beveiliging en controle geeft een uitgebreid overzicht van de IT-beveiliging van uw organisatie met ingebouwde zoekquery's voor belangrijke problemen die uw aandacht nodig hebben. Het dashboard **Beveiliging en controle** is het startscherm voor alles wat betrekking heeft op beveiliging in OMS. Het zorgt voor hoogwaardig inzicht in de beveiligingsstatus van uw computers. Ook kunnen op het startscherm alle gebeurtenissen van de afgelopen 24 uur, 7 dagen of een ander tijdsbestek worden weergegeven. Voer deze stappen uit voor toegang tot het dashboard **Beveiliging en controle**:

1. Klik in het hoofddashboard **Microsoft Operations Management Suite** op de tegel **Instellingen** aan de linkerkant.
2. Klik op de blade **Instellingen** onder **Oplossingen** op de optie **Beveiliging en controle**.
3. Het dashboard **Beveiliging en controle** wordt weergegeven:
   
    ![OMS-dashboard Beveiliging en controle](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Als u dit dashboard voor het eerst opent en er geen apparaten met OMS worden bewaakt, staan er in de tegels geen gegevens die van de agent zijn verkregen. Nadat u de agent hebt geïnstalleerd, kan het even duren voordat de gegevens zijn ingevuld. Het is dus mogelijk dat bepaalde gegevens ontbreken, omdat het uploaden naar de cloud nog niet is voltooid.  In dat geval is het normaal dat sommige tegels geen concrete gegevens bevatten. Lees [Connect Windows computers directly to OMS](https://technet.microsoft.com/library/mt484108.aspx) (Windows-computers rechtstreeks met OMS verbinden) voor meer informatie over het installeren van de OMS-agent in een Windows-systeem en [Connect Linux computers to OMS](https://technet.microsoft.com/library/mt622052.aspx) (Linux-computers verbinden met OMS) voor meer informatie over het uitvoeren van deze taak in een Linux-systeem.

> [!NOTE]
> De informatie wordt door de agent verzameld op basis van de huidige gebeurtenissen die zijn ingeschakeld, bijvoorbeeld computernaam, IP-adres en gebruikersnaam. Maar er worden geen document-/bestandsgegevens, databasenaam of persoonlijke gegevens verzameld.   
> 
> 

Oplossingen omvatten een verzameling logische, visualisatie- en gegevensverzamelingsregels waarmee klanten hun belangrijkste uitdagingen kunnen aangaan. Beveiliging en controle is één oplossing. Andere oplossingen kunnen afzonderlijk worden toegevoegd. Lees het artikel [Add solutions](https://technet.microsoft.com/library/mt674635.aspx) (Oplossingen toevoegen) voor meer informatie over het toevoegen van een nieuwe oplossing.

Het OMS-dashboard Beveiliging en controle is ingedeeld in vier hoofdcategorieën:

* **Beveiligingsdomeinen**: in dit gebied kunt u beveiligingsrecords voor een periode doornemen, malware-evaluaties, update-evaluaties, de netwerkbeveiliging, identiteits- en toegangsinformatie en computers met beveiligingsgebeurtenissen bekijken en snel toegang krijgen tot het Azure Security Center-dashboard.
* **Problemen die aandacht vereisen**: met deze optie kunt u snel het aantal actieve problemen en de ernst van deze problemen vaststellen.
* **Detecties (preview)**: hiermee kunt u aanvalspatronen identificeren door beveiligingswaarschuwingen te visualiseren wanneer de aanvallen op uw resources plaatsvinden.
* **Bedreigingsinformatie**: hiermee kunt u aanvalspatronen identificeren door het totale aantal servers met schadelijk uitgaand IP-verkeer, het type schadelijke dreigingen en een kaart waarop wordt aangegeven waar deze IP-adressen van afkomstig zijn te visualiseren. 
* **Algemene beveiligingsquery's**: met deze optie krijgt u een lijst van de meest voorkomende beveiligingsquery's, die u kunt gebruiken om uw omgeving te bewaken. Wanneer u op een van deze query's klikt, wordt de blade **Zoeken** geopend met de resultaten voor deze query.

> [!NOTE]
> Lees 'How OMS secures your data' (Uw gegevens beveiligen met OMS) voor meer informatie over hoe uw gegevens met OMS worden beveiligd.
> 
> 

## <a name="security-domains"></a>Beveiligingsdomeinen
Bij het bewaken van resources is het belangrijk dat u snel toegang kunt krijgen tot de huidige status van uw omgeving. Het is echter ook belangrijk dat u gebeurtenissen die in het verleden zijn opgetreden, kunt herleiden voor beter inzicht in wat er op een bepaald punt in tijd in uw omgeving gebeurt. 

> [!NOTE]
> Gegevensretentie vindt plaats volgens de prijsstelling voor OMS. Ga voor meer informatie naar de pagina met prijzen van [Microsoft Operations Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx).
> 
> 

Scenario’s voor incidentresponses en forensisch onderzoek kunnen direct worden verbeterd met de resultaten die beschikbaar zijn in de tegel **Beveiligingsrecords in de loop van de tijd**.

![Beveiligingsrecords in de loop van de tijd](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Wanneer u op deze tegel klikt, wordt de blade **Zoeken** geopend met alle queryresultaten voor **Beveiligingsgebeurtenissen** (Type = SecurityEvents). De gegevens zijn gebaseerd op de afgelopen zeven dagen, zoals hieronder is weergegeven:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Beveiligingsrecords in de loop van de tijd](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Het zoekresultaat is verdeeld over twee deelvensters. In het linkerdeelvenster zijn het aantal gevonden gebeurtenissen, de computers waarop deze gebeurtenissen zijn gevonden, het aantal accounts die op deze computers zijn aangetroffen en de soorten activiteiten uitgesplitst. In het rechterdeelvenster staan het totale aantal resultaten en een chronologische weergave van de beveiligingsgebeurtenissen met de naam en gebeurtenisactiviteit van de computer. U kunt ook op **Meer weergeven** klikken om meer informatie over deze gebeurtenis te bekijken, zoals de gebeurtenisgegevens, de gebeurtenis-id en de gebeurtenisbron.

> [!NOTE]
> Lees [OMS search reference](https://technet.microsoft.com/library/mt450427.aspx) (Engelstalig) voor meer informatie over zoekquery’s in OMS.
> 
> 

### <a name="antimalware-assessment"></a>Antimalware-evaluatie
Met deze optie kunt u snel computers met onvoldoende beveiliging en computers die door malware zijn aangetast identificeren. De evaluatiestatus voor malware en gedetecteerde bedreigingen op de bewaakte servers worden gelezen en vervolgens worden de gegevens voor verwerking naar de OMS-service in de cloud verzonden. Servers waarop bedreigingen zijn gedetecteerd en servers met onvoldoende beveiliging worden weergegeven in het dashboard voor malware-evaluatie, dat toegankelijk is via de tegel **Antimalware-evaluatie**. 

![malware-evaluatie](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Wanneer u op de blade **Zoeken** klikt, wordt net als bij andere live-tegels in het OMS-dashboard het queryresultaat weergegeven. Als u voor deze optie op de optie **Geen rapportage** onder **Beveiligingsstatus** klikt, wordt in het queryresultaat deze ene vermelding weergegeven met de naam en de positie van de computer, zoals hieronder is weergegeven:

![zoekresultaat](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [!NOTE]
> *positie* is een beoordeling die de status van de beveiliging (ingeschakeld, uitgeschakeld, bijgewerkt enzovoort) en gevonden bedreigingen weergeeft. Als dit als nummer wordt weergegeven, vergemakkelijkt dit aggregaties.
> 
> 

Als u op de naam van de computer klikt, wordt de beveiligingsstatus voor deze computer chronologisch weergegeven. Dat is heel handig voor scenario's waarin u moet weten of de antimalware ooit is geïnstalleerd en op een bepaald moment is verwijderd.   

### <a name="update-assessment"></a>Update-evaluatie
Met deze optie kunt u snel de algemene blootstelling aan mogelijke beveiligingsproblemen vaststellen en bepalen of en hoe essentieel deze updates zijn voor uw omgeving. Met de oplossing OMS Beveiliging en controle worden deze updates alleen gevisualiseerd. De werkelijke gegevens zijn afkomstig van [de oplossing voor updatebeheer](oms-solution-update-management.md). Dit is een andere module van OMS. Hier ziet u een voorbeeld van de updates:

![systeemupdates](./media/oms-security-getting-started/oms-getting-started-fig6-new.png)

> [!NOTE]
> Lees [De oplossing voor updatebeheer in OMS](oms-solution-update-management.md) voor meer informatie over de oplossing voor updatebeheer.
> 
> 

### <a name="identity-and-access"></a>Identiteit en toegang
Identiteit moet de controlelaag voor uw onderneming zijn. Het beveiligen van uw identiteit moet de hoogste prioriteit hebben. De omheining rond bedrijven vormde vroeger een van de primaire verdedigingslinies. Doordat gegevens en apps steeds vaker naar de cloud worden verplaatst, is identiteit tegenwoordig de nieuwe omheining. 

> [!NOTE]
> De gegevens zijn momenteel alleen gebaseerd op aanmeldingsgegevens van beveiligingsgebeurtenissen (gebeurtenis-is 4624). In de toekomst worden Office365-aanmeldingen en Azure AD-gegevens ook opgenomen.
> 
> 

Door uw identiteitsactiviteiten te bewaken, kunt u proactieve maatregelen treffen voordat een incident plaatsvindt of reactieve maatregelen nemen om een aanvalspoging te stoppen. Op het dashboard **Identiteit en toegang** vindt u een overzicht van uw identiteitsstatus, waaronder het aantal mislukte aanmeldingspogingen, het gebruikersaccount dat voor deze pogingen is gebruikt, accounts die zijn vergrendeld, accounts met een gewijzigd of opnieuw ingesteld wachtwoord en het aantal accounts dat op dat moment is aangemeld. 

Wanneer u op de tegel **Identiteit en toegang** klikt, wordt het volgende dashboard geopend:

![identiteit en toegang](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

U kunt de informatie in dit dashboard onmiddellijk gebruiken om mogelijk verdachte activiteiten vast te stellen. Er is bijvoorbeeld 338 keer geprobeerd om aan te melden als **beheerder** en alle pogingen zijn mislukt. Dit kan worden veroorzaakt door een beveiligingsaanval op dit account. Als u op dit account klikt, wordt meer informatie weergegeven waarmee u de doelresource van deze mogelijke aanval kunt vaststellen:

![zoekresultaten](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

In het gedetailleerde rapport staat belangrijke informatie over deze gebeurtenis, waaronder: de doelcomputer, het type aanmelding (in dit geval netwerkaanmelding), de activiteit (in dit geval gebeurtenis 4625) en een uitgebreide tijdlijn van elke poging. 

### <a name="computers"></a>Computers
Deze tegel kan worden gebruikt voor toegang tot alle computers met actieve beveiligingsgebeurtenissen. Wanneer u op deze tegel klikt, ziet u een lijst met computers met beveiligingsgebeurtenissen en het aantal gebeurtenissen op elke computer:

![Computers](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

U kunt uw onderzoek voortzetten door op elke computer te klikken en de gebeurtenissen die zijn gemarkeerd te controleren.

### <a name="threat-intelligence"></a>Bedreigingsinformatie

Door de optie Bedreigingsinformatie te gebruiken in OMS Beveiliging en controle, kunnen IT-beheerders beveiligingsbedreigingen identificeren voor de omgeving. Zo kunnen ze bijvoorbeeld bepalen of een bepaalde computer deel uitmaakt van een botnet. Computers kunnen knooppunten worden in een botnet wanneer aanvallers malware installeren waarmee de computer wordt verbonden met de bedieningscentrale. Er kunnen ook potentiële bedreigingen mee worden geïdentificeerd die afkomstig zijn van underground communicatiekanalen zoals darknet. Lees [Beveiligingswaarschuwingen in de oplossing Beveiliging en controle van Operations Management Suite bewaken en erop reageren](oms-security-responding-alerts.md) voor meer informatie over bedreigingsinformatie.

In sommige scenario's ziet u mogelijk een potentieel schadelijke IP-adres dat is geopend vanaf een bewaakte computer:

![kaart met gegevens van bedreigingen](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

Deze waarschuwing en anderen binnen dezelfde categorie worden gegenereerd via OMS-beveiliging door gebruik te maken van [bedreigingsinformatie van Microsoft](https://youtu.be/O4WtxgUrDc8). Deze informatie over bedreigingen wordt verzameld door Microsoft, maar ook gekocht van toonaangevende leveranciers van dit type informatie. De informatie wordt regelmatig bijgewerkt en aangepast als er nieuwe bedreigingen zijn ontdekt. Vanwege de aard van de informatie, moet deze worden gecombineerd met andere bronnen van beveiligingsgegevens tijdens het [onderzoeken](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) van een beveiligingswaarschuwing. 

### <a name="baseline-assessment"></a>Basislijnevaluatie

Microsoft definieert samen met brancheorganisaties en overheidsinstanties overal ter wereld een Windows-configuratie die garandeert dat maximaal beveiligde serverimplementaties worden gebruikt. Deze configuratie bestaat uit een verzameling registersleutels, controlebeleidsinstellingen en beveiligingsbeleidsinstellingen, gecombineerd met waarden die door Microsoft voor deze instellingen worden aanbevolen. Deze verzameling staat bekend als de beveiligingsbasislijn. Lees [Basislijnevaluatie in de oplossing Beveiliging en controle in Operations Management Suite](oms-security-baseline.md) voor meer informatie over deze optie.

### <a name="azure-security-center"></a>Azure Security Center
Deze tegel is in feite een snelkoppeling voor toegang tot het Azure Security Center-dashboard. Lees [Aan de slag met Azure Security Center](../security-center/security-center-get-started.md) voor meer informatie over deze oplossing.

## <a name="notable-issues"></a>Problemen die aandacht vereisen
Het belangrijkste doel van deze groep opties is een snel overzicht te geven van de problemen u in uw omgeving door ze in te delen in de categorieën Kritiek, Waarschuwing en Ter informatie. De tegel voor het type actieve problemen is een visualisatie van deze problemen, maar u kunt er niet meer informatie mee bekijken. Daarvoor dient het onderste gedeelte van deze tegel. Daarin staan de naam van het probleem (NAAM), het aantal objecten waarvoor het probleem is opgetreden (AANTAL) en hoe kritiek het is (ERNST).

![Problemen die aandacht vereisen](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Deze problemen zijn al aan bod gekomen in andere gebieden van de groep **Beveiligingsdomeinen** die het doel van deze weergave versterkt: de belangrijkste problemen in uw omgeving op één plek visualiseren.

## <a name="detections-preview"></a>Detecties (preview)
Deze optie heeft als belangrijkste doel de IT-afdeling toestaan mogelijke bedreigingen voor de omgeving en de ernst van deze bedreigingen vast te stellen.

![Bedreigingsinformatie](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Deze optie kan tijdens een [onderzoek naar de incidentrespons](https://blogs.msdn.microsoft.com/azuresecurity/2016/11/30/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) ook worden gebruikt om de evaluatie uit te voeren en meer informatie te verkrijgen over de aanval.

> [!NOTE]
> Bekijk de video [How to Leverage the Azure Security Center & Microsoft Operations Management Suite for an Incident Response](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) (Azure Security Center en Microsoft Operations Management Suite gebruiken voor een incidentrespons) voor meer informatie over het gebruik van OMS voor incidentresponses.
> 
> 

## <a name="threat-intelligence"></a>Bedreigingsinformatie
Met de nieuwe sectie Bedreigingsinformatie van de oplossing Beveiliging en controle kunt u aanvalspatronen op verschillende manieren visualiseren: het totale aantal servers met schadelijk uitgaand IP-verkeer, het type schadelijke dreigingen en een kaart waarop wordt aangegeven waar deze IP-adressen van afkomstig zijn. U kunt in de kaart werken en op de IP-adressen klikken voor meer informatie.

Gele punaises op de kaart geven inkomend verkeer van schadelijke IP-adressen aan. Het is niet ongebruikelijk dat voor servers die verbinding hebben met internet schadelijk inkomend verkeer wordt aangegeven. Het is wel raadzaam deze pogingen te controleren om er zeker van te zijn dat ze zijn mislukt. Deze indicatoren zijn gebaseerd op IIS-logboeken, WireData en logboeken van Windows Firewall.  

![Bedreigingsinformatie](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Algemene beveiligingsquery's
Met de lijst met beschikbare algemene beveiligingquery's kunt u snel toegang krijgen tot informatie over de resource en deze aan te passen op basis van de behoeften van uw omgeving. Deze algemene query's zijn onder meer:

* Alle beveiligingsactiviteiten
* Beveiligingsactiviteiten op computer computer01.contoso.com (vervangen door uw eigen computernaam)
* Beveiligingsactiviteiten op computer computer01.contoso.com voor account Beheerder (vervangen door uw eigen computer- en accountnaam)
* Aanmeldingsactiviteit per computer
* Accounts waarmee Microsoft Antimalware op een computer is beëindigd
* Computers waarop het Microsoft Antimalware-proces is beëindigd
* Computers waarop hash.exe is uitgevoerd (vervangen door een andere procesnaam)
* Alle procesnamen die zijn uitgevoerd
* Aanmeldingsactiviteit per account
* Accounts die op afstand worden aangemeld bij computer computer01.contoso.com (vervangen door uw eigen computernaam)

## <a name="see-also"></a>Zie ook
In dit document hebt u kennis gemaakt met de oplossing Beveiliging en controle van OMS. Raadpleeg de volgende artikelen voor meer informatie over OMS Beveiliging:

* [Overzicht van Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Beveiligingswaarschuwingen in de oplossing Beveiliging en controle van Operations Management Suite bewaken en erop reageren](oms-security-responding-alerts.md)
* [Resources bewaken in de oplossing Beveiliging en controle van Operations Management Suite ](oms-security-monitoring-resources.md)

