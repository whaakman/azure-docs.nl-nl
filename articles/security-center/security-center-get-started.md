---
title: Snelstartgids voor Azure Security Center | Microsoft Docs
description: Dit document helpt u snel aan de slag met Azure Security Center. U maakt kennis met de onderdelen voor bewaking van de veiligheid en het beleidsbeheer en u wordt doorverwezen naar de volgende stappen.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: terrylan
ms.openlocfilehash: c28f92af96f31d1c386cf072f83fc142b9a7f588
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-center-quick-start-guide"></a>Aan de slag met Azure Security Center
Dit artikel helpt u snel aan de slag te gaan met Azure Security Center. U maakt kennis met de onderdelen voor bewaking van de veiligheid en het beleidsbeheer van Security Center.

## <a name="prerequisites"></a>Vereisten
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

De gratis categorie van het Beveiligingscentrum is automatisch ingeschakeld op alle Azure-abonnementen en biedt beveiligingsbeleid, beoordeling van de continue veiligheid en bruikbare beveiligingsaanbevelingen voor hulp bij het beveiligen van uw Azure-resources.

Security Center is toegankelijk via [Azure Portal](https://azure.microsoft.com/features/azure-portal/). Zie de [documentatie van de portal](https://azure.microsoft.com/documentation/services/azure-portal/) voor meer informatie over Azure Portal.

## <a name="permissions"></a>Machtigingen
In Security Center ziet u alleen informatie met betrekking tot een bron wanneer u de rol van eigenaar, bijdrager of lezer voor het abonnement of resourcegroep die deel uitmaakt van een resource te worden toegewezen. Zie [machtigingen in Azure Security Center](security-center-permissions.md) voor meer informatie over de functies en de toegestane acties in Security Center.

## <a name="data-collection"></a>Gegevensverzameling
Security Center verzamelt gegevens van uw virtuele Azure-machines (VM's) en niet-Azure-computers om te controleren op beveiligingsproblemen en bedreigingen. Gegevens worden verzameld met behulp van Microsoft Monitoring Agent, die verschillende configuraties betrekking hebben op beveiliging en gebeurtenislogboeken van de machine leest en kopieert de gegevens naar de werkruimte voor analyse. Security Center voorziet in de Microsoft Monitoring Agent op alle bestaande ondersteund Azure VM's en nieuwe bestanden die zijn gemaakt. Zie [gegevensverzameling inschakelen](security-center-enable-data-collection.md) voor meer informatie over de werking van verzamelen van gegevens.

Automatische inrichting is het raadzaam en is vereist voor abonnementen op de prijscategorie Standard van Security Center. Uitschakelen van automatische inrichting limieten beveiligingsbewaking voor uw resources.

Zie [Security Center prijzen](security-center-pricing.md) voor meer informatie over de vrije en de prijscategorie Standard.

In de volgende stappen wordt beschreven hoe u de onderdelen van Security Center kunt openen en gebruiken.

> [!NOTE]
> In dit artikel wordt de service gepresenteerd aan de hand van een voorbeeldimplementatie. Dit artikel is geen stapsgewijze handleiding.
>
>

## <a name="access-security-center"></a>Security Center openen
Voer in de portal de volgende stappen uit om Security Center te openen:

1. Ga naar het **Microsoft Azure**-menu en selecteer **Security Center**.

   ![Menu van Azure][1]
2. Als u Security Center voor het eerst opent, wordt de blade **Welkom** weergegeven. Selecteer **starten Security Center** openen **Security Center**.
   ![Welkomstscherm][10]
3. Nadat u Security Center op de blade Welkom starten of selecteert u in het menu Microsoft Azure Security Center **Security Center** wordt geopend. Als u de blade **Security Center** in de toekomst snel wilt kunnen openen, selecteert u (rechtsboven) de optie **Blade vastmaken aan dashboard**.
   ![Optie Blade vastmaken aan dashboard][2]

## <a name="use-security-center"></a>Security Center gebruiken
U kunt beveiligingsbeleidsregels voor uw Azure-abonnementen en resourcegroepen configureren. Als u een beveiligingsbeleid wilt configureren voor uw abonnement, gaat u als volgt te werk:

1. Selecteer in het hoofdmenu Security Center **beveiligingsbeleid**.
2. Onder **Security Center - beveiligingsbeleid**, selecteer een abonnement.
3. Onder **beveiligingsbeleid - gegevensverzameling**, **automatische inrichting** is ingeschakeld. Security Center voorziet in de Microsoft Monitoring Agent op alle bestaande ondersteund Azure VM's en nieuwe bestanden die zijn gemaakt.

    ![Beveiligingsbeleid][12]

4. Op de **beveiligingsbeleid** blade, selecteer het beleidonderdeel dat **beveiligingsbeleid**.

     ![Beveiligingsbeleid][11]

5. Onder **aanbevelingen weergeven voor**, schakelt u de aanbevelingen die u wilt weergeven als onderdeel van uw beveiligingsbeleid. Voorbeelden:

   * Instelling **systeemupdates** naar **op** scans alle virtuele machines ondersteund voor de ontbrekende OS-updates.
   * Instelling **OS beveiligingslekken** naar **op** scans alle ondersteunde virtuele machines om te identificeren van de OS-configuraties die mogelijk de virtuele machine kwetsbaar voor aanvallen maken.

6. Selecteer **Opslaan**.

### <a name="view-recommendations"></a>Aanbevelingen bekijken
1. Ga terug naar de blade **Security Center** en selecteer de tegel **Aanbevelingen**. De beveiligingsstatus van uw Azure-bronnen worden regelmatig door Security Center gecontroleerd. Wanneer met Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen weergegeven in de blade **Aanbevelingen**.
   ![Aanbevelingen in Azure Security Center][5]
2. Selecteer een aanbeveling in de blade **Aanbevelingen** om meer informatie weer te geven en/of om actie te ondernemen om het probleem te verhelpen.

### <a name="view-the-security-state-of-your-resources"></a>De beveiligingsstatus van uw resources weergeven
1. Ga terug naar de blade **Security Center**. De **preventie** gedeelte van het dashboard bevat indicatoren van de beveiligingsstatus voor virtuele machines, netwerken, gegevens en toepassingen.
2. Selecteer **Compute** voor meer informatie. De **Compute** wordt een blade geopend waarin drie tabbladen:

  - **Overzicht** -bevat bewaking en aanbevelingen voor virtuele machine.
  - **Virtuele machines en computers** -geeft een lijst van de huidige beveiligingsstatus van alle virtuele machines en computers.
  - **Cloudservices** -web- en werkrollen rollen die worden bewaakt door Security Center bevat.

    ![Beveiligingsstatus berekenen][6]

3. Op de **overzicht** tabblad, selecteert u een aanbeveling onder voor meer informatie weergeven en/of maatregelen noodzakelijke besturingselementen te configureren.
4. Op de **VM's en computers** tabblad, selecteert u een resource om meer details te bekijken.

### <a name="view-security-alerts"></a>Beveiligingswaarschuwingen bekijken
1. Ga terug naar de blade **Security Center** en selecteer de tegel **Beveiligingswaarschuwingen**. De blade **Beveiligingswaarschuwingen** wordt geopend en u ziet een lijst met waarschuwingen. De waarschuwingen worden door Security Center gegenereerd op basis van de analyse van uw beveiligingslogboeken en netwerkactiviteit. Waarschuwingen van geïntegreerde partneroplossingen zijn ook opgenomen.
   ![Beveiligingswaarschuwingen in Azure Security Center][7]

2. Selecteer een waarschuwing als u extra informatie wilt weergeven. In dit voorbeeld gaan we selecteren **gewijzigd van systeem gedetecteerd in dump filter binaire**. Hiermee opent u blades met aanvullende informatie over de waarschuwing.
   ![Gegevens van beveiligingswaarschuwingen in Azure Security Center][8]

### <a name="view-the-health-of-your-partner-solutions"></a>De status van uw partneroplossingen bekijken
1. Ga terug naar de blade **Security Center**. De **beveiligingsoplossingen** tegel kunt u bewaken, in een oogopslag de status van de partneroplossingen die zijn geïntegreerd met uw Azure-abonnement.
2. Selecteer de **beveiligingsoplossingen** tegel. Er wordt een blade geopend met een lijst van alle partneroplossingen die zijn verbonden met Security Center.
   ![Partneroplossingen][9]
3. Selecteer een partneroplossing. Er wordt een blade geopend met de status van de partneroplossing en de gekoppelde resources van de oplossing. Selecteer **Oplossingenconsole** om de partnerbeheermogelijkheden voor deze oplossing te openen.

   ![Partneroplossingen][13]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kennisgemaakt met de onderdelen van beveiligingsbewaking en beleidsbeheer in Security Center. Nu u vertrouwd bent met Security Center, kunt u de volgende stappen proberen:

* Een beveiligingsbeleid voor uw Azure-abonnement configureren, Zie [beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md).
* Gebruik de aanbevelingen in Security Center voor hulp bij het beveiligen van uw Azure-resources, Zie [aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md).
* Bekijken en beheren van uw huidige beveiligingswaarschuwingen, Zie [beheren en erop reageren beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md).
- Meer informatie over de integratie met partners kunnen de algehele beveiliging vergroten, Zie [Partner en integratie met oplossingen](security-center-partner-integration.md).
- Informatie over hoe gegevens worden beheerd en beveiligd in Security Center, Zie [Azure Security Center-gegevensbeveiliging](security-center-data-security.md).
* Lees meer over de [functies voor geavanceerde detectie van bedreigingen](security-center-detection-capabilities.md) die zijn inbegrepen in de [prijscategorie Standard](security-center-pricing.md) van Security Center. De Standard-laag wordt gedurende de eerste 60 dagen gratis aangeboden.
* Als u vragen hebt over het gebruik van Security Center, raadpleegt u de [Veelgestelde vragen over Azure Security Center](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
[11]: ./media/security-center-get-started/show-recommendations-for.png
[12]: ./media/security-center-get-started/automatic-provisioning.png
[13]: ./media/security-center-get-started/partner-solutions-detail.png
