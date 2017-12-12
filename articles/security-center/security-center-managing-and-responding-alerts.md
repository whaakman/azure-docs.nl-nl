---
title: Beveiligingswaarschuwingen beheren in Azure Security Center | Microsoft Docs
description: Dit document bevat informatie over het gebruik van de mogelijkheden van Azure Security Center om beveiligingswaarschuwingen te beheren en hierop te reageren.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/30/2017
ms.author: yurid
ms.openlocfilehash: 1388a351b82beb6b3e7eb61a3a0517aa90c695f5
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="managing-and-responding-to-security-alerts-in-azure-security-center"></a>Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center
Dit document bevat informatie over het gebruik van Azure Security Center om beveiligingswaarschuwingen te beheren en hierop te reageren.

> [!NOTE]
> Als u geavanceerde detectie wilt inschakelen, voert u een upgrade uit naar Azure Security Center Standard. Er is een gratis proefversie voor 60 dagen beschikbaar. Als u een upgrade wilt uitvoeren, selecteert u de prijscategorie in het [beveiligingsbeleid](security-center-policies.md). Zie [Prijsinformatie over Azure Security Center](security-center-pricing.md) voor meer informatie.
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

* **Beschrijving**: een korte beschrijving van de waarschuwing.
* **Aantal**: een lijst met alle waarschuwingen van dit specifieke type die zijn gedetecteerd op een specifieke dag.
* **Gedetecteerd door**: de service die verantwoordelijk is voor activering van de waarschuwing.
* **Datum**: de datum waarop de gebeurtenis heeft plaatsgevonden.
* **Status**: de huidige status voor deze waarschuwing. Er zijn twee soorten statussen:
  * **Actief**: de beveiligingswaarschuwing is gedetecteerd.
* **Ernst**: de ernst kan hoog, gemiddeld of laag zijn.

> [!NOTE]
> Beveiligingswaarschuwingen die door Security Center worden gegenereerd, worden ook weergegeven onder het Azure-activiteitenlogboek. Zie [Activiteitenlogboeken bekijken om de acties op resources te controleren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) voor meer informatie over toegang tot het Azure-activiteitenlogboek.
>

### <a name="filtering-alerts"></a>Waarschuwingen filteren
U kunt waarschuwingen filteren op basis van datum, status en ernst. Het filteren van waarschuwingen kan nuttig zijn wanneer u minder beveiligingswaarschuwingen wilt weergeven. U kunt u bijvoorbeeld concentreren op de beveiligingswaarschuwingen van de afgelopen 24 uur, omdat u een mogelijke inbreuk in het systeem onderzoekt.

1. Klik op **Filteren** op de pagina **Beveiligingswaarschuwingen**. De pagina **Filteren** wordt geopend en u kunt de gewenste waarden selecteren voor datum, status en ernst.

    ![Waarschuwingen filteren in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-to-security-alerts"></a>Reageren op beveiligingswaarschuwingen
Selecteer een beveiligingswaarschuwing voor meer informatie over de gebeurtenis(sen) waarmee de waarschuwing is geactiveerd en welke stappen u zo nodig moet uitvoeren om een aanval te verhelpen. Beveiligingswaarschuwingen zijn gegroepeerd op type en datum. Als u op een beveiligingswaarschuwing klikt, wordt een pagina met een lijst gegroepeerde waarschuwingen geopend.

![Op beveiligingswaarschuwingen reageren in Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

In dit geval verwijzen de geactiveerde waarschuwingen naar verdachte RDP-activiteiten (Remote Desktop Protocol). In de eerste kolom ziet u welke resources zijn aangevallen, de tweede kolom toont hoe vaak de resource is aangevallen, de derde kolom bevat de tijd waarop de aanval heeft gevonden; de vierde kolom bevat de status van de waarschuwing en in de vijfde kolom wordt de ernst van de aanval weergegeven. Bekijk deze informatie en klik op de resource die is aangevallen.

![Suggesties voor wat u kunt doen bij beveiligingswaarschuwingen in Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

Het veld **Beschrijving** bevat meer informatie over deze gebeurtenis. Deze aanvullende informatie biedt inzicht in wat de beveiligingswaarschuwing heeft geactiveerd, de doelresource, eventueel het bron-IP-adres en aanbevelingen voor het herstellen.  In sommige gevallen zal het bron-IP-adres leeg zijn (niet beschikbaar), omdat niet alle Windows-logboeken voor beveiligingsgebeurtenissen het IP-adres bevatten.

Het herstel dat door Security Center wordt voorgesteld, verschilt per beveiligingswaarschuwing. In sommige gevallen moet u wellicht andere Azure-mogelijkheden gebruiken om het aanbevolen herstel te implementeren. Als herstel voor deze aanval moet bijvoorbeeld het IP-adres op een zwarte lijst worden gezet dat door de aanval is gegenereerd. Hiervoor is de regel van een [netwerk-ACL](../virtual-network/virtual-networks-acl.md) of een [netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md) nodig. Lees voor meer informatie over de verschillende typen waarschuwingen [Beveiligingswaarschuwingen per type in Azure Security Center](security-center-alerts-type.md).

> [!NOTE]
> Security Center heeft een nieuwe set detecties vrijgegeven voor beperkte preview. Deze detecties maken gebruik van controlerecords, een gemeenschappelijk controleframework, voor het detecteren van de schadelijke gedragingen op Linux-machines. Stuur [ons](mailto:ASC_linuxdetections@microsoft.com) een e-mail met uw abonnement-id's om deel te nemen aan de preview.


## <a name="see-also"></a>Zie ook
In dit document hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsincidenten afhandelen in Azure Security Center](security-center-incident.md)
* [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md)
* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md)
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.
