---
title: Inzicht in gegevens van Azure Cost Management | Microsoft Docs
description: In dit artikel kunt u beter begrijpen welke gegevens is opgenomen in Azure Cost Management en hoe vaak deze wordt verwerkt, die worden verzameld, wordt weergegeven en gesloten.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: f0632b0d6e651ba6e0b43586e60d582e29c5ce00
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58500539"
---
# <a name="understand-cost-management-data"></a>Inzicht in gegevens van Cost Management

In dit artikel kunt u beter begrijpen welke gegevens in Azure Cost Management is opgenomen. En wordt uitgelegd hoe vaak gegevens worden verwerkt, die worden verzameld, wordt weergegeven en gesloten. U wordt maandelijks gefactureerd voor gebruik van Azure. Het type Azure-abonnement bepaalt echter als uw Factureringsmaand wordt beëindigd. Hoe vaak Cost Management ontvangt gebruik gegevens varieert op basis van verschillende factoren. Deze factoren omvatten hoe lang het duurt om de gegevens te verwerken en hoe vaak de gebruik het factureringssysteem voor het verzenden van Azure-services.

## <a name="supported-microsoft-offers"></a>Ondersteunde Microsoft-aanbiedingen

De volgende informatie wordt weergegeven de momenteel ondersteunde [Microsoft Azure biedt](https://azure.microsoft.com/support/legal/offer-details/) in Azure Cost Management.  Een Azure-aanbieding is het type van de Azure-abonnement dat u hebt.

| Categorie  | **Naam van aanbieding** | **Quotum-ID** | **Nummer van de aanbieding** |
| --- | --- | --- | --- |
| **Azure Duitsland** | [Azure Duitsland-betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-de-0003p)      | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P |
| **Azure Government** | Azure Government Enterprise                                                         | EnterpriseAgreement_2014-09-01 | MS-AZR-USGOV-0017P |
| **Enterprise Agreement (EA)** | Enterprise Dev/Test                                                        | MSDNDevTest_2014-09-01 | MS-AZR-0148P |
| **Enterprise Agreement (EA)** | [Microsoft Azure Enterprise](https://azure.microsoft.com/offers/enterprise-agreement-support-upgrade) | EnterpriseAgreement_2014-09-01 | MS-AZR-0017P |
| **Microsoft Developer Network (MSDN)** | [MSDN Platforms](https://azure.microsoft.com/offers/ms-azr-0062p) | MSDN_2014-09-01 | MS-AZR-0062P |
| **Pay-As-You-Go** | [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p)                       | PayAsYouGo_2014-09-01 | MS-AZR-0003P |
| **Pay-As-You-Go** | [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p)              | MSDNDevTest_2014-09-01 | MS-AZR-0023P |
| **Pay-As-You-Go** | [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p)           | MPN_2014-09-01 | MS-AZR-0025P |
| **Pay-As-You-Go** | [Gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p)                          | FreeTrial_2014-09-01 | MS-AZR-0044P |
| **Pay-As-You-Go** | [Azure in Open](https://azure.microsoft.com/offers/ms-azr-0111p)                       | AzureInOpen_2014-09-01 | MS-AZR-0111P |
| **Pay-As-You-Go** | [Azure for Students](https://azure.microsoft.com/offers/ms-azr-0170p)                  | AzureForStudents_2018-01-01 | MS-AZR-0170P |
| **Pay-As-You-Go** | Azure Pass                                                                             | AzurePass_2014-09-01 | MS-AZR-0120P, MS-AZR-0122P - MS-AZR-0125P, MS-AZR-0128P - MS-AZR-0130P |
| **Visual Studio** | [Visual Studio Enterprise – MPN](https://azure.microsoft.com/offers/ms-azr-0029p)      | MPN_2014-09-01 | MS-AZR-0029P |
| **Visual Studio** | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p)          | MSDN_2014-09-01 | MS-AZR-0059P |
| **Visual Studio** | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p)     | MSDNDevTest_2014-09-01 | MS-AZR-0060P |
| **Visual Studio** | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p)            | MSDN_2014-09-01 | MS-AZR-0063P |
| **Visual Studio** | [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p)  | MSDN_2014-09-01 | MS-AZR-0064P |

De volgende tabel ziet u niet-ondersteunde aanbiedingen.

| Categorie  | **Naam van aanbieding** | **Quotum-ID** | **Nummer van de aanbieding** |
| --- | --- | --- | --- |
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

Voor klanten met categorieën van aanbieding voor betalen per gebruik, MSDN en Visual Studio is de gegevens beschikbaar in Cost Management vanaf 02-10-2018. Voor toegang tot gegevens voor uw abonnement voordat 02-10-2018, kunt u de [Azure-Accountcentrum](https://account.azure.com/subscriptions) voor het downloaden van uw gebruik van de gegevens in een CSV-bestand of kunt u de [Usage Details API](/rest/api/consumption/usagedetails).

## <a name="determine-your-offer-type"></a>Bepalen van het aanbiedingstype
Als er geen gegevens voor een abonnement en u bepalen wilt als het abonnement onder de ondersteunde biedt valt, kunt u valideren dat uw abonnement wordt ondersteund. Als u wilt valideren of een Azure-abonnement wordt ondersteund, aanmelden bij de [Azure-portal](https://portal.azure.com). Selecteer vervolgens **alle Services** in het menu links. Selecteer in de lijst met services, **abonnementen**. Klik op het abonnement dat u wilt controleren in het menu van de lijst met abonnementen. Uw abonnement wordt weergegeven op het tabblad Overzicht en ziet u de **bieden** en **aanbiedings-ID**. In de volgende afbeelding ziet u een voorbeeld.

![Voorbeeld van het tabblad van het abonnement overzicht weergegeven van de aanbieding en aanbiedings-ID](./media/understand-cost-mgt-data/offer-and-offer-id.png)

## <a name="costs-included-in-cost-management"></a>Kosten die zijn opgenomen in Cost Management

De volgende tabellen bevatten gegevens die is opgenomen of zich niet in Cost Management.

**Kosten en gebruiksgegevens**

| **Opgenomen** | **Niet inbegrepen** |
| --- | --- |
| Gebruik van Azure service<sup>1</sup> | Aankopen in de reservering: Zie voor meer informatie, [API's voor het automatiseren van Azure-reservering](../billing/billing-reservation-apis.md). |
| Gebruik van Marketplace-aanbiedingen | Marketplace-aankopen: Zie voor meer informatie, [externe servicekosten](../billing/billing-understand-your-azure-marketplace-charges.md). |
|   | Kosten - ondersteuning voor meer informatie, Zie [condities uitgelegd](../billing/billing-understand-your-invoice.md). |
|   | Belasting - Zie voor meer informatie, [condities uitgelegd](../billing/billing-understand-your-invoice.md). |
|   | Tegoed - Zie voor meer informatie, [condities uitgelegd](../billing/billing-understand-your-invoice.md). |

<sup>1</sup> gebruik van de azure-service is gebaseerd op de reservering en heeft onderhandeld over prijzen.

**Metadata**

| **Opgenomen** | **Niet inbegrepen** |
| --- | --- |
| Resourcetags<sup>2</sup> | groep resourcetags |

<sup>2</sup> resourcetags worden toegepast als het gebruik van elke service is verzonden en zijn niet beschikbaar met terugwerkende kracht voor historische gebruik.

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

Of u gebruikt de [Cost Management-API's](https://aka.ms/costmgmt/docs), Power BI, of de Azure-portal voor het ophalen van gegevens, verwacht dat de kosten voor de huidige factureringsperiode opnieuw worden geclassificeerd en als gevolg hiervan wijzigen, totdat de factuur wordt gesloten.

## <a name="usage-data-update-frequency-varies"></a>Gebruik gegevens updatefrequentie varieert

De beschikbaarheid van de gemaakte gebruiksgegevens in Cost Management, is afhankelijk van een aantal factoren, onder andere:

- Hoe vaak introduceren de Azure-services (zoals opslag, berekeningen, CDN en SQL) gebruik.
- De gebruikte tijd voor het verwerken van gegevens over gebruik tot en met de classificatie-engine en kosten van beheer van pijplijnen.

Sommige services introduceren gebruik vaker dan andere. Dus, ziet u mogelijk gegevens in Cost Management voor sommige services sneller dan andere services die gegevens die minder vaak te verzenden. Gebruik voor services duurt normaal gesproken 8 tot 24 uur worden weergegeven in Cost Management. Houd er rekening mee dat de gegevens voor een open maand wordt vernieuwd als u meer gebruik in rekening gebracht omdat updates cumulatief zijn.

## <a name="see-also"></a>Zie ook

- Als u dit nog niet hebt al de eerste snelstartgids voltooid voor kostenbeheer, lezen via [kosten analyseren](quick-acm-cost-analysis.md).
