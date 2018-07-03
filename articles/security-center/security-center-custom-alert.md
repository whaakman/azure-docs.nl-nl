---
title: Regels voor aangepaste waarschuwingen in Azure Security Center | Microsoft Docs
description: In dit document leest u hoe u regels voor aangepaste waarschuwingen maakt in Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: f335d8c4-0234-4304-b386-6f1ecda07833
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2018
ms.author: terrylan
ms.openlocfilehash: 326b07a18b07f15850e0283ad51a05b42c1ef0ab
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341310"
---
# <a name="custom-alert-rules-in-azure-security-center-preview"></a>Regels voor aangepaste waarschuwingen in Azure Security Center (Preview)
In dit document leest u hoe u regels voor aangepaste waarschuwingen maakt in Azure Security Center.

## <a name="what-are-custom-alert-rules-in-security-center"></a>Wat zijn regels voor aangepaste waarschuwingen in Security Center?

Security Center bevat een set vooraf gedefinieerde [beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts), die worden geactiveerd wanneer een bedreiging of verdachte activiteit wordt gedetecteerd. In sommige scenario's kan het nodig zijn om een aangepaste waarschuwing te maken die inspeelt op specifieke behoeften van uw omgeving.

In Security Center kunt u met behulp van regels voor aangepaste waarschuwingen nieuwe beveiligingswaarschuwingen definiëren op basis van gegevens die al zijn verzameld voor uw omgeving. U kunt query's maken en het resultaat van deze query's als criteria gebruiken voor de aangepaste regel. Zodra wordt voldaan aan deze criteria, wordt de regel uitgevoerd. U kunt aangepaste query's maken aan de hand van beveiligingsgebeurtenissen op computers, beveiligingslogboeken van partneroplossingen of gegevens die zijn opgenomen via een API.

> [!NOTE]
> Aangepaste waarschuwingen worden niet ondersteund in Security Center [onderzoeksfunctie](security-center-investigation.md).
>
>

## <a name="how-to-create-a-custom-alert-rule-in-security-center"></a>Regels voor aangepaste waarschuwingen maken in Security Center

Open het dashboard van **Security Center** en volg deze stappen om een regel voor een aangepaste waarschuwing te maken:

1.  Klik in het linkerdeelvenster, onder **Detectie**, op **Aangepaste waarschuwingsregels (Preview)**.
2.  Klik op de pagina **Security Center – Aangepaste waarschuwingsregels (Preview)** op **Nieuwe aangepaste waarschuwingsregel**.

    ![Aangepaste waarschuwing](./media/security-center-custom-alert/security-center-custom-alert-fig1.png)

3.  De pagina Aangepaste waarschuwingsregel maken wordt weergegeven met de volgende opties:

    ![Maken](./media/security-center-custom-alert/security-center-custom-alert-fig2.png)

4.  Typ de naam voor deze aangepaste regel in het veld **Naam**.
5.  Typ in het veld **Beschrijving** een korte beschrijving die de bedoeling van deze regel aangeeft.
6.  Selecteer in het veld **Ernst** het ernstniveau (Hoog, Gemiddeld, Laag) dat aansluit bij uw behoeften.
7.  Selecteer in het veld **Abonnement** het abonnement waarin deze regel van toepassing is.
8.  Selecteer de werkruimte die u wilt controleren met deze regel in de **werkruimte** veld, en klik in de **zoekquery** veld, de query die u wilt gebruiken om de resultaten te verkrijgen.

    > [!NOTE]
    > U moet de machtiging schrijven in de werkruimte die u selecteert voor het opslaan van uw aangepaste waarschuwing.
    >
    >

    Het resultaat van de query zorgt uiteindelijk voor activering van de waarschuwing. U ziet een groen vinkje in de rechterbovenhoek van dit veld wanneer u een geldige query hebt ingevoerd:

    ![Query’s uitvoeren](./media/security-center-custom-alert/security-center-custom-alert-fig3.png)

10. Selecteer in het veld **Periode** de periode waarin de bovenstaande query moet worden uitgevoerd. U ziet dat het zoekresultaat onder in dit veld verandert als u tijdsduur hier aanpast.

    ![Periode](./media/security-center-custom-alert/security-center-custom-alert-fig4.png)

11. Selecteer in het veld **Evaluatie** de frequentie waarmee deze regel moet worden geëvalueerd en uitgevoerd.
12. Selecteer in het veld **Aantal resultaten** de gewenste operator (groter dan of kleiner dan).
13. Typ in het veld **Drempelwaarde** een getal dat wordt gebruikt als referentie voor de operator die u eerder hebt geselecteerd.
14. Schakel de optie **Waarschuwingen onderdrukken** in als u een interval wilt instellen waarna Security Center een nieuwe waarschuwing mag versturen voor deze regel.
15. Klik op **OK** om de regel op te slaan.

Nadat u de nieuwe waarschuwingsregel hebt gemaakt, wordt deze opgenomen in de lijst met aangepaste regels voor waarschuwingen. Zodra aan de voorwaarden van deze regel wordt voldaan, wordt er een nieuwe waarschuwing geactiveerd. Deze kunt u bekijken in het dashboard **Beveiligingswaarschuwingen**.

![Waarschuwing](./media/security-center-custom-alert/security-center-custom-alert-fig5.png)

U ziet dat de parameters (zoekquery, drempelwaarde, enzovoort) die zijn vastgelegd tijdens het maken van de regel, beschikbaar zijn in de waarschuwing voor deze aangepaste regel.

## <a name="see-also"></a>Zie ook
In dit document hebt u kunnen lezen hoe u een aangepaste regel voor een waarschuwing maakt in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over het Azure Beveiligingscentrum:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.
