---
title: Beveiligingswaarschuwingen beheren in Azure Security Center | Microsoft Docs
description: Dit document bevat informatie over het gebruik van de mogelijkheden van Azure Security Center om beveiligingswaarschuwingen te beheren en hierop te reageren.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2018
ms.author: v-mohabe
ms.openlocfilehash: 39849514d772f128434daad590de22f941245af7
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69516110"
---
# <a name="managing-and-responding-to-security-alerts-in-azure-security-center"></a>Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center
Dit document bevat informatie over het gebruik van Azure Security Center om beveiligingswaarschuwingen te beheren en hierop te reageren.

> [!NOTE]
> Als u geavanceerde detectie wilt inschakelen, voert u een upgrade uit naar Azure Security Center Standard. Er is een gratis proefversie beschikbaar. Als u een upgrade wilt uitvoeren, selecteert u de prijscategorie in het [beveiligingsbeleid](tutorial-security-policy.md). Zie [Prijsinformatie over Azure Security Center](security-center-pricing.md) voor meer informatie.
>
>

## <a name="what-are-security-alerts"></a>Wat zijn beveiligingswaarschuwingen?
Security Center verzamelt, analyseert en integreert automatisch logboekgegevens van uw Azure-resources, het netwerk en verbonden partneroplossingen, zoals firewall- en eindpuntbeveiligingsoplossingen om werkelijke dreigingen te detecteren en fout-positieven te reduceren. In Security Center wordt een lijst met beveiligingswaarschuwingen met prioriteiten weergegeven samen met de informatie die u nodig hebt om snel onderzoek te doen naar het probleem en aanbevelingen voor het herstellen van een aanval.


> [!NOTE]
> Lees [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md) voor meer informatie over de werking van de detectiemogelijkheden van Azure Security Center.
>
>

## <a name="managing-security-alerts"></a>Beveiligingswaarschuwingen beheren
U kunt uw huidige waarschuwingen controleren met de tegel **Beveiligingswaarschuwingen**. Voer de volgende stappen uit om meer informatie over elke waarschuwing weer te geven:

1. Op het dashboard van Security Center ziet u de tegel **Beveiligingswaarschuwingen**.

    ![De tegel Beveiligingswaarschuwingen in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. Klik op de tegel om de pagina **Beveiligingswaarschuwingen** te openen, met meer informatie over de waarschuwingen.

   ![Beveiligingswaarschuwingen in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

Onder aan deze pagina ziet u de details voor elke waarschuwing. Als u wilt sorteren, klikt u op de kolom waarop u wilt sorteren. Hieronder volgt de definitie voor elke kolom:

* **Beschrijving**: Een korte uitleg van de waarschuwing.
* **Aantal**: Een lijst met alle waarschuwingen van dit specifieke type die zijn gedetecteerd op een specifieke dag.
* **Gedetecteerd door**: De service die verantwoordelijk is voor het activeren van de waarschuwing.
* **Datum**: De datum waarop de gebeurtenis heeft plaatsgevonden.
* **Status**: De huidige status van de waarschuwing. Er zijn twee soorten statussen:
  * **Actief**: De beveiligings waarschuwing is gedetecteerd.
  * **Gesloten**: De beveiligings waarschuwing is door de gebruiker genegeerd. Deze status wordt doorgaans gebruikt voor waarschuwingen die zijn onderzocht en die zijn verholpen of die geen echte aanval kunnen zijn.
* **Ernst**: Het Ernst niveau, dat hoog, gemiddeld of laag kan zijn.

> [!NOTE]
> Beveiligingswaarschuwingen die door Security Center worden gegenereerd, worden ook weergegeven onder het Azure-activiteitenlogboek. Zie [Activiteitenlogboeken bekijken om de acties op resources te controleren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) voor meer informatie over toegang tot het Azure-activiteitenlogboek.
>


### <a name="alert-severity"></a>Ernst van waarschuwingen

-   **Hoog**: Er is een hoge waarschijnlijkheid dat uw resource is aangetast. U ziet het meteen. Security Center heeft hoge betrouw baarheid in zowel de schadelijke intentie als de bevindingen die worden gebruikt voor het uitgeven van de waarschuwing. Een waarschuwing waarmee de uitvoering van een bekend schadelijk hulp programma, zoals Mimikatz, wordt gedetecteerd, is een algemeen hulp programma dat wordt gebruikt voor referentie diefstal. 
-   **Gemiddeld**: Dit is waarschijnlijk een verdachte activiteit die kan erop wijzen dat een bron is aangetast.
Het vertrouwen van Security Center in het analyse programma of het zoeken is gemiddeld en het vertrouwen van de schadelijke intentie is gemiddeld voor hoog. Deze worden doorgaans machine learning en detecties op basis van anomalieën. Bijvoorbeeld een aanmeldings poging vanaf een afwijkende locatie.
-   **Laag**: Dit kan een goed aardige positieve of een geblokkeerde aanval zijn. 
    - Security Center niet zeker weet dat de intentie schadelijk is en de activiteit mogelijk onschuld is. Logboek wissen is bijvoorbeeld een actie die zich kan voordoen wanneer een aanvaller hun tracks probeert te verbergen, maar in veel gevallen is een routine bewerking door beheerders uitgevoerd.
    - Security Center vertelt u doorgaans niet wanneer er aanvallen zijn geblokkeerd, tenzij dit een interessant geval is. 
-   **Ter informatie**: Er worden alleen informatieve waarschuwingen weer geven wanneer u inzoomt op een beveiligings incident of als u de REST API met een specifieke waarschuwings-ID gebruikt. Een incident bestaat doorgaans uit een aantal waarschuwingen, wat soms alleen ter informatie kan worden weer gegeven, maar in de context van de andere waarschuwingen kan het een betrouw bare zijn.  

> [!NOTE]
> Als u de API **-versie 2015-06-01-preview** gebruikt, zijn er verschillen waarbij alarm code typen worden toegepast op welke scenario's, van wat hierboven wordt vermeld.  

### <a name="filtering-alerts"></a>Waarschuwingen filteren
U kunt waarschuwingen filteren op basis van datum, status en ernst. Het filteren van waarschuwingen kan nuttig zijn wanneer u minder beveiligingswaarschuwingen wilt weergeven. U kunt u bijvoorbeeld concentreren op de beveiligingswaarschuwingen van de afgelopen 24 uur, omdat u een mogelijke inbreuk in het systeem onderzoekt.

1. Klik op **Filteren** op de pagina **Beveiligingswaarschuwingen**. De pagina **Filteren** wordt geopend en u kunt de gewenste waarden selecteren voor datum, status en ernst.

    ![Waarschuwingen filteren in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-to-security-alerts"></a>Reageren op beveiligingswaarschuwingen
Selecteer een beveiligingswaarschuwing voor meer informatie over de gebeurtenis(sen) waarmee de waarschuwing is geactiveerd en welke stappen u zo nodig moet uitvoeren om een aanval te verhelpen. Beveiligingswaarschuwingen zijn gegroepeerd op type en datum. Als u op een beveiligingswaarschuwing klikt, wordt een pagina met een lijst gegroepeerde waarschuwingen geopend.

![Op beveiligingswaarschuwingen reageren in Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

In dit geval verwijzen de geactiveerde waarschuwingen naar verdachte RDP-activiteiten (Remote Desktop Protocol). In de eerste kolom ziet u welke resources zijn aangevallen, de tweede kolom toont hoe vaak de resource is aangevallen, de derde kolom bevat de tijd waarop de aanval heeft gevonden; de vierde kolom bevat de status van de waarschuwing en in de vijfde kolom wordt de ernst van de aanval weergegeven. Bekijk deze informatie en klik op de resource die is aangevallen.

![Suggesties voor wat u kunt doen bij beveiligingswaarschuwingen in Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

In het veld **Beschrijving** vindt u meer informatie over deze gebeurtenis. Deze aanvullende informatie biedt inzicht in wat de beveiligingswaarschuwing heeft geactiveerd, de doelresource, eventueel het bron-IP-adres en aanbevelingen voor het herstellen.  In sommige gevallen zal het bron-IP-adres leeg zijn (niet beschikbaar), omdat niet alle Windows-logboeken voor beveiligingsgebeurtenissen het IP-adres bevatten.

Het herstel dat door Security Center wordt voorgesteld, verschilt per beveiligingswaarschuwing. In sommige gevallen moet u wellicht andere Azure-mogelijkheden gebruiken om het aanbevolen herstel te implementeren. Het herstel voor deze aanval is bijvoorbeeld het IP-adres waarmee deze aanval wordt gegenereerd, niet toe te staan met behulp van een [netwerk toegangs beheer lijst](../virtual-network/virtual-networks-acl.md) of een regel voor een [netwerk beveiligings groep](../virtual-network/security-overview.md#security-rules) . Lees de [typen beveiligings waarschuwingen](security-center-alerts-overview.md#security-alert-types)voor meer informatie over de verschillende typen waarschuwingen.

> [!NOTE]
> Security Center heeft een nieuwe set detecties vrijgegeven voor beperkte preview. Deze detecties maken gebruik van controlerecords, een gemeenschappelijk controleframework, voor het detecteren van de schadelijke gedragingen op Linux-machines. [Stuur ons een e-mail](mailto:ASC_linuxdetections@microsoft.com) met uw abonnement-id's om deel te nemen aan de preview.


## <a name="see-also"></a>Zie ook
In dit document hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsincidenten afhandelen in Azure Security Center](security-center-incident.md)
* [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md)
* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md)
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.
