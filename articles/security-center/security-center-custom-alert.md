---
title: Regels voor aangepaste waarschuwingen in Azure Security Center | Microsoft Docs
description: In dit document leest u hoe u regels voor aangepaste waarschuwingen maakt in Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f335d8c4-0234-4304-b386-6f1ecda07833
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2018
ms.author: rkarlin
ms.openlocfilehash: 8845cb0a91edefa0350558f35488519ec37d064d
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663574"
---
# <a name="custom-alert-rules-in-azure-security-center-retired"></a>Aangepaste waarschuwings regels in Azure Security Center (buiten gebruik gesteld)
Dit document helpt u bij het maken van aangepaste waarschuwings regels (preview) in Azure Security Center.

> [!NOTE]
> Aangepaste waarschuwingen zijn op 30 juni 2019 van Azure Security Center buiten gebruik gesteld. 

## <a name="retirement-of-custom-alert-rules-in-azure-security-center"></a>Buiten gebruik stellen van aangepaste waarschuwings regels in Azure Security Center

De aangepaste waarschuwings ervaring is op 30 juni 2019 buiten gebruik gesteld vanwege de buiten gebruiks telling van de onderliggende infra structuur. Aangepaste waarschuwingen die zijn gedefinieerd vóór retriement, worden niet doorgevoerd en beveiligings waarschuwingen op basis van deze aangepaste waarschuwings regels worden niet gegenereerd. U kunt nog steeds de query's van uw aangepaste waarschuwings regels weer geven in Security Center om ze opnieuw te maken in de hieronder genoemde alternatieven:

Gebruikers worden geadviseerd om:
- [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) inschakelen en de ingebouwde [analyse](https://docs.microsoft.com/azure/sentinel/tutorial-detect-threats) functionaliteit gebruiken om hun waarschuwings regels opnieuw te maken
- Hun waarschuwingen opnieuw maken met Azure Monitor logboek waarschuwingen
                                     
Als u uw bestaande waarschuwingen wilt blijven gebruiken en deze opnieuw wilt maken in azure Sentinel, [start u Azure Sentinel](https://portal.azure.com/#create/Microsoft.ASI/preview). Als eerste stap selecteert u de werk ruimte waar uw aangepaste waarschuwingen worden opgeslagen en selecteert u vervolgens het menu-item Analytics om uw aangepaste waarschuwings regels in te stellen. Raadpleeg de [documentatie](https://docs.microsoft.com/azure/sentinel/tutorial-detect-threats) voor meer informatie.

> [!NOTE]
> Aangepaste waarschuwingen met behulp van [Zoek](https://docs.microsoft.com/azure/azure-monitor/log-query/search-queries) -of [samenvoeg](https://docs-analytics-eus.azurewebsites.net/queryLanguage/query_language_unionoperator.html) instructie query's worden niet ondersteund in azure Sentinel. Wijzig deze waarschuwingen voordat u de migratie uitvoert.

Zie [logboek waarschuwingen maken, weer geven en beheren met Azure monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)voor meer informatie over het opnieuw maken van waarschuwingen met behulp van Azure monitor logboek waarschuwingen. Zie [waarschuwingen registreren in azure monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log)voor een algemeen overzicht.

## <a name="what-are-custom-alert-rules-in-security-center"></a>Wat zijn regels voor aangepaste waarschuwingen in Security Center?

Security Center bevat een set vooraf gedefinieerde [beveiligingswaarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts), die worden geactiveerd wanneer een bedreiging of verdachte activiteit wordt gedetecteerd. In sommige scenario's kan het nodig zijn om een aangepaste waarschuwing te maken die inspeelt op specifieke behoeften van uw omgeving.

In Security Center kunt u met behulp van regels voor aangepaste waarschuwingen nieuwe beveiligingswaarschuwingen definiëren op basis van gegevens die al zijn verzameld voor uw omgeving. U kunt query's maken en het resultaat van deze query's als criteria gebruiken voor de aangepaste regel. Zodra wordt voldaan aan deze criteria, wordt de regel uitgevoerd. U kunt aangepaste query's maken aan de hand van beveiligingsgebeurtenissen op computers, beveiligingslogboeken van partneroplossingen of gegevens die zijn opgenomen via een API.

> [!NOTE]
> Aangepaste waarschuwingen worden niet ondersteund in de [onderzoek functie](security-center-investigation.md)van de Security Center.
>
>

## <a name="how-to-create-a-custom-alert-rule-in-security-center"></a>Regels voor aangepaste waarschuwingen maken in Security Center

Open het dashboard van **Security Center** en volg deze stappen om een regel voor een aangepaste waarschuwing te maken:

1.  Klik in het linkerdeelvenster, onder **Detectie**, op **Aangepaste waarschuwingsregels (Preview)** .
2.  Klik op de pagina **Security Center – Aangepaste waarschuwingsregels (Preview)** op **Nieuwe aangepaste waarschuwingsregel**.

    ![Aangepaste waarschuwing](./media/security-center-custom-alert/security-center-custom-alert-fig1.png)

3.  De pagina Aangepaste waarschuwingsregel maken wordt weergegeven met de volgende opties:

    ![Maken](./media/security-center-custom-alert/security-center-custom-alert-fig2.png)

4.  Typ de naam voor deze aangepaste regel in het veld **Naam**.
5.  Typ in het veld **Beschrijving** een korte beschrijving die de bedoeling van deze regel aangeeft.
6.  Selecteer in het veld **Ernst** het ernstniveau (Hoog, Gemiddeld, Laag) dat aansluit bij uw behoeften.
7.  Selecteer in het veld **Abonnement** het abonnement waarin deze regel van toepassing is.
8.  Selecteer de werk ruimte die u met deze regel wilt bewaken in het **werkruimte** veld en in het veld **Zoek query** , de query die u wilt gebruiken om de resultaten te verkrijgen.

    > [!NOTE]
    > U hebt schrijf machtigingen nodig in de werk ruimte die u hebt geselecteerd om uw aangepaste waarschuwing op te slaan.
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
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.
