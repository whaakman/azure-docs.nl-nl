---
title: Inzicht in gegevens van Azure Cost Management | Microsoft Docs
description: In dit artikel kunt u beter begrijpen welke gegevens is opgenomen in Azure Cost Management en hoe vaak deze wordt verwerkt, die worden verzameld, wordt weergegeven en gesloten.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 07/01/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 44b95c92f51ca9782fca492f3dec3142087ecc91
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797022"
---
# <a name="understand-cost-management-data"></a>Inzicht in gegevens van Cost Management

In dit artikel kunt u beter begrijpen Azure kosten en gebruik gegevens die zijn opgenomen in Azure Cost Management. Hierin wordt uitgelegd hoe vaak gegevens worden verwerkt, die worden verzameld, wordt weergegeven en gesloten. U wordt maandelijks gefactureerd voor gebruik van Azure. Hoewel de factureringscyclus maandelijkse periodes, cyclus start zijn en end datums per abonnementstype variëren. Hoe vaak Cost Management ontvangt gebruik gegevens varieert op basis van verschillende factoren. Deze factoren omvatten hoe lang het duurt om de gegevens te verwerken en hoe vaak de gebruik het factureringssysteem voor het verzenden van Azure-services.

Cost Management omvat alle gebruik en aankopen, met inbegrip van reserveringen en aanbiedingen van derden voor Enterprise Agreement (EA)-accounts. Overeenkomst voor Microsoft-klanten (MCA) accounts en afzonderlijke abonnementen met betalen per gebruik-tarieven zijn alleen gebruik van Azure en Marketplace-services. Ondersteuning en andere kosten zijn niet opgenomen. Kosten worden geschat totdat een factuur wordt gegenereerd en niet doen factor aan tegoed.

## <a name="supported-microsoft-azure-offers"></a>Ondersteunde Microsoft Azure-aanbiedingen

De volgende informatie wordt weergegeven de momenteel ondersteunde [Microsoft Azure biedt](https://azure.microsoft.com/support/legal/offer-details/) in Azure Cost Management. Een Azure-aanbieding is het type van de Azure-abonnement dat u hebt. Gegevens zijn beschikbaar in Cost Management vanaf de **gegevens beschikbaar is via** datum. Als een abonnement wordt gewijzigd biedt, zijn kosten vóór de datum van de aanbieding niet meer beschikbaar. 

| **Categorie**  | **Naam van aanbieding** | **Quotum-ID** | **Nummer van de aanbieding** | **Gegevens die beschikbaar is via** |
| --- | --- | --- | --- | --- |
| **Azure Duitsland** | [Azure Duitsland-betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-de-0003p)      | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P | 2 oktober 2018<sup>2</sup> |
| **Azure Government** | Azure Government Enterprise                                                         | EnterpriseAgreement_2014-09-01 | MS-AZR-USGOV-0017P | Mei 2014<sup>1</sup> |
| **Enterprise Agreement (EA)** | Enterprise Dev/Test                                                        | MSDNDevTest_2014-09-01 | MS-AZR-0148P | Mei 2014<sup>1</sup> |
| **Enterprise Agreement (EA)** | [Microsoft Azure Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support-upgrade) | EnterpriseAgreement_2014-09-01 | MS-AZR-0017P | Mei 2014<sup>1</sup> |
| **Overeenkomst van de klant van Microsoft** | [Microsoft Azure Plan](https://azure.microsoft.com/offers/ms-azr-0017g) | EnterpriseAgreement_2014-09-01 | N/A | Maart 2019<sup>3</sup> |
| **Overeenkomst van de klant van Microsoft** | [Microsoft Azure-abonnement voor ontwikkelen en testen](https://azure.microsoft.com/offers/ms-azr-0148g) | MSDNDevTest_2014-09-01 | N/A | Maart 2019<sup>3</sup> |
| **Microsoft Developer Network (MSDN)** | [MSDN Platforms](https://azure.microsoft.com/offers/ms-azr-0062p)<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0062P | 2 oktober 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p)                  | PayAsYouGo_2014-09-01 | MS-AZR-0003P | 2 oktober 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p)         | MSDNDevTest_2014-09-01 | MS-AZR-0023P | 2 oktober 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p)      | MPN_2014-09-01 | MS-AZR-0025P | 2 oktober 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p)<sup>4</sup>         | FreeTrial_2014-09-01 | MS-AZR-0044P | 2 oktober 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Azure in Open](https://azure.microsoft.com/offers/ms-azr-0111p)<sup>4</sup>      | AzureInOpen_2014-09-01 | MS-AZR-0111P | 2 oktober 2018<sup>2</sup> |
| **Pay-As-You-Go** | [Azure for Students](https://azure.microsoft.com/offers/ms-azr-0170p)<sup>4</sup> | AzureForStudents_2018-01-01 | MS-AZR-0170P | 2 oktober 2018<sup>2</sup> |
| **Pay-As-You-Go** | Azure Pass<sup>4</sup>                                                            | AzurePass_2014-09-01 | MS-AZR-0120P, MS-AZR-0122P - MS-AZR-0125P, MS-AZR-0128P - MS-AZR-0130P | 2 oktober 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise – MPN](https://azure.microsoft.com/offers/ms-azr-0029p)<sup>4</sup>     | MPN_2014-09-01 | MS-AZR-0029P | 2 oktober 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p)<sup>4</sup>         | MSDN_2014-09-01 | MS-AZR-0059P | 2 oktober 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p)<sup>4</sup>    | MSDNDevTest_2014-09-01 | MS-AZR-0060P | 2 oktober 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p)<sup>4</sup>           | MSDN_2014-09-01 | MS-AZR-0063P | 2 oktober 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p)<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0064P | 2 oktober 2018<sup>2</sup> |

_<sup>**1** </sup> voor gegevens vóór mei 2014, gaat u naar de [Azure Enterprise portal](https://ea.azure.com)._

_<sup>**2** </sup> voor gegevens vóór 2 oktober 2018, gaat u naar de [Azure-Accountcentrum](https://account.azure.com/subscriptions)._

_<sup>**3** </sup> Microsoft Customer overeenkomsten aan de slag in maart 2019 en ook geen historische gegevens voordat u dit punt._

_<sup>**4** </sup> historische gegevens voor abonnementen op basis van een creditcard en vooraf betalen mogelijk niet overeen met uw factuur. Zie [historische gegevens mogelijk niet overeenkomt met de factuur](#historical-data-might-not-match-invoice) hieronder._

De volgende tabel bevat aanbiedingen die nog niet worden ondersteund.

| Categorie  | **Naam van aanbieding** | **Quotum-ID** | **Nummer van de aanbieding** |
| --- | --- | --- | --- |
| **Azure Duitsland** | [Azure Duitsland-betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-de-0003p) | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P |
| **Cloud Solution Provider (CSP)** | Microsoft Azure                                    | CSP_2015-05-01 | MS-AZR-0145P |
| **Cloud Solution Provider (CSP)** | Azure Government CSP                               | CSP_2015-05-01 | MS-AZR-USGOV-0145P |
| **Cloud Solution Provider (CSP)** | Azure Duitsland in CSP voor Microsoft Cloud Duitsland   | CSP_2015-05-01 | MS-AZR-DE-0145P |
| **Pay-As-You-Go**                 | Azure for Students Starter | DreamSpark_2015-02-01 | MS-AZR-0144P |
| **Pay-As-You-Go**                 | [Microsoft Azure Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/) | Sponsored_2016-01-01 | MS-AZR-0036P |
| **Ondersteuningsabonnementen** | Standard Support                    | Default_2014-09-01 | MS-AZR-0041P |
| **Ondersteuningsabonnementen** | Professional Direct-ondersteuning         | Default_2014-09-01 | MS-AZR-0042P |
| **Ondersteuningsabonnementen** | Developer-ondersteuning                   | Default_2014-09-01 | MS-AZR-0043P |
| **Ondersteuningsabonnementen** | Duitsland-ondersteuningsplan                | Default_2014-09-01 | MS-AZR-DE-0043P |
| **Ondersteuningsabonnementen** | Azure Government Standard Support   | Default_2014-09-01 | MS-AZR-USGOV-0041P |
| **Ondersteuningsabonnementen** | Azure Government Pro-direct Support | Default_2014-09-01 | MS-AZR-USGOV-0042P |
| **Ondersteuningsabonnementen** | Azure gov-Developer Support  | Default_2014-09-01 | MS-AZR-USGOV-0043P |

## <a name="determine-your-offer-type"></a>Bepalen van het aanbiedingstype
Als er geen gegevens voor een abonnement en u bepalen wilt als het abonnement onder de ondersteunde biedt valt, kunt u valideren dat uw abonnement wordt ondersteund. Als u wilt valideren of een Azure-abonnement wordt ondersteund, aanmelden bij de [Azure-portal](https://portal.azure.com). Selecteer vervolgens **alle Services** in het menu links. Selecteer in de lijst met services, **abonnementen**. Klik op het abonnement dat u wilt controleren in het menu van de lijst met abonnementen. Uw abonnement wordt weergegeven op het tabblad Overzicht en ziet u de **bieden** en **aanbiedings-ID**. In de volgende afbeelding ziet u een voorbeeld.

![Voorbeeld van het tabblad van het abonnement overzicht weergegeven van de aanbieding en aanbiedings-ID](./media/understand-cost-mgt-data/offer-and-offer-id.png)

## <a name="costs-included-in-cost-management"></a>Kosten die zijn opgenomen in Cost Management

De volgende tabellen bevatten gegevens die is opgenomen of zich niet in Cost Management. Alle kosten worden geschat totdat een factuur wordt gegenereerd. Kosten weergegeven, omvatten geen voorafbetaalde en gratis tegoed.

**Kosten en gebruiksgegevens**

| **Opgenomen** | **Niet inbegrepen** |
| --- | --- |
| Gebruik van Azure service<sup>5</sup>        | Kosten - ondersteuning voor meer informatie, Zie [condities uitgelegd](../billing/billing-understand-your-invoice.md). |
| Marketplace-aanbieding gebruik<sup>6</sup> | Belasting - Zie voor meer informatie, [condities uitgelegd](../billing/billing-understand-your-invoice.md). |
| Marketplace-aankopen<sup>6</sup>      | Tegoed - Zie voor meer informatie, [condities uitgelegd](../billing/billing-understand-your-invoice.md). |
| Aankopen in de reservering<sup>7</sup>      |  |

_<sup>**5** </sup> gebruik van de azure-service is gebaseerd op de reservering en heeft onderhandeld over prijzen._

_<sup>**6** </sup> marketplace gebruiks- en -aankopen zijn niet beschikbaar voor betalen per gebruik, MSDN, en Visual Studio biedt op dit moment._

_<sup>**7** </sup> reservering aankopen zijn op dit moment alleen beschikbaar voor Enterprise Agreement (EA)-accounts._

**Metadata**

| **Opgenomen** | **Niet inbegrepen** |
| --- | --- |
| Resourcetags<sup>8</sup> | groep resourcetags |

_<sup>**8** </sup> resourcetags worden toegepast als het gebruik van elke service is verzonden en zijn niet beschikbaar met terugwerkende kracht voor historische gebruik._

## <a name="rated-usage-data-refresh-schedule"></a>Schema voor gegevensvernieuwing geclassificeerde gebruik

Kosten en gebruik gegevens zijn beschikbaar in kostenbeheer en facturering in Azure portal en [ondersteuning voor API's](index.yml). Houd er rekening mee met de volgende punten als u kosten bekijken:

- Geschatte kosten in rekening gebracht voor de huidige factureringsperiode worden zes keer per dag bijgewerkt.
- Geschatte kosten in rekening gebracht voor de huidige factureringsperiode kunnen wijzigen als u meer gebruik in rekening worden gebracht.
- Elke update is cumulatief en bevat de regels en gegevens uit de vorige update.
- Azure wordt of _wordt gesloten_ de huidige factureringsperiode tot 72 uur (drie kalenderdagen) nadat de facturering afloopt.

De volgende voorbeelden laten zien hoe factureringsperioden kan beëindigen.

Enterprise Agreement (EA)-abonnementen: als de Factureringsmaand op 31 maart eindigt, geschatte kosten van 72 uur later worden bijgewerkt. In dit voorbeeld door April 4 middernacht (UTC).

Betalen per gebruik-abonnementen: als de Factureringsmaand wordt beëindigd op 15 mei en vervolgens de geschatte kosten van bijgewerkt naar de 72 uur later opnieuw mogelijk. In dit voorbeeld door 19 mei middernacht (UTC).

### <a name="rerated-data"></a>Rerated gegevens

Of u gebruikt de [Cost Management-API's](index.yml), Power BI of de Azure-portal voor het ophalen van gegevens, verwacht dat de kosten voor de huidige factureringsperiode opnieuw worden geclassificeerd en als gevolg hiervan wijzigen, totdat de factuur wordt gesloten.

## <a name="usage-data-update-frequency-varies"></a>Gebruik gegevens updatefrequentie varieert

De beschikbaarheid van de gemaakte gebruiksgegevens in Cost Management, is afhankelijk van een aantal factoren, onder andere:

- Hoe vaak introduceren de Azure-services (zoals opslag, berekeningen, CDN en SQL) gebruik.
- De gebruikte tijd voor het verwerken van gegevens over gebruik tot en met de classificatie-engine en kosten van beheer van pijplijnen.

Sommige services introduceren gebruik vaker dan andere. Dus, ziet u mogelijk gegevens in Cost Management voor sommige services sneller dan andere services die gegevens die minder vaak te verzenden. Gebruik voor services duurt normaal gesproken 8 tot 24 uur worden weergegeven in Cost Management. Houd er rekening mee dat de gegevens voor een open maand wordt vernieuwd als u meer gebruik in rekening gebracht omdat updates cumulatief zijn.

## <a name="historical-data-might-not-match-invoice"></a>Historische gegevens mogelijk niet overeen met factuur

Historische gegevens voor aanbiedingen op basis van een creditcard en vooraf betaalt, mogelijk niet overeen met uw factuur. Sommige Azure betalen naar gebruik, MSDN en Visual Studio biedt kunnen zijn Azure-tegoed en geavanceerde betalingen toegepast op de factuur. De historische gegevens weergegeven in Cost Management is echter gebaseerd op alleen de kosten voor uw geschatte verbruik. Historische gegevens van Cost Management bevat geen betalingen en tegoeden. De historische gegevens weergegeven voor de volgende aanbiedingen mogelijk als gevolg hiervan niet overeenkomt met precies afgestemd op uw factuur.

- Azure for Students (MS-AZR - 0170P)
- Azure in Open (MS-AZR - 0111P)
- Azure geslaagd (MS-AZR - 0120P, MS - AZR - 0123P, MS - AZR - 0125P, MS - AZR - 0128P, MS - AZR - 0129P)
- Gratis proefversie (MS-AZR - 0044P)
- MSDN (MS-AZR-0062P)
- Visual Studio (MS-AZR-0029P, MS-AZR-0059P, MS-AZR-0060P, MS-AZR-0063P, MS-AZR-0064P)

## <a name="see-also"></a>Zie ook

- Als u dit nog niet hebt al de eerste snelstartgids voltooid voor kostenbeheer, lezen via [kosten analyseren](quick-acm-cost-analysis.md).
