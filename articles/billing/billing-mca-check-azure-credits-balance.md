---
title: Azure-tegoed saldo volgen voor Microsoft-KLANTOVEREENKOMST | Microsoft Docs
description: Leer hoe u Azure-tegoed saldo controleren voor KLANTOVEREENKOMST van Microsoft.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: amberb
ms.openlocfilehash: 1e8c3e6863b9cd8f2f5ced18a57918c32c865e75
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57544593"
---
# <a name="track-azure-credit-balance-for-microsoft-customer-agreement"></a>Azure-tegoed voor Microsoft-KLANTOVEREENKOMST bijhouden

U kunt de Azure-tegoed voor Microsoft-KLANTOVEREENKOMST controleren in Azure portal. U credits gebruiken om te betalen voor producten die worden gedekt door het tegoed.

U betaalt wanneer u producten die niet zijn gekoppeld aan het tegoed gebruiken of uw gebruik uw tegoed overschrijdt. Zie voor meer informatie, [producten die niet zijn gekoppeld aan Azure-tegoed.](#products-that-arent-covered-by-azure-credits)

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement).

## <a name="check-credit-balance-in-the-azure-portal"></a>Saldo in Azure portal controleren

1. Meld u aan bij [Azure Portal]( https://portal.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van zoeken in de portal voor kostenbeheer en facturering](./media/billing-mca-check-azure-credits-balance/billing-search-cost-management-billing.png)

3. Ga naar het profiel voor facturering. Afhankelijk van uw toegang moet u mogelijk facturering-account selecteren. Selecteer in het factureringsaccount **facturering profielen** en vervolgens een profiel voor facturering.

4. Selecteer **Azure-tegoed**.

5. De Azure-tegoed pagina bevat de volgende informatie:

   ![Schermopname van het saldo en transacties voor een profiel voor facturering](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-overview.png)

   | Termijn               | Definitie                           |
   |--------------------|--------------------------------------------------------|
   | Geschat saldo  | Geschatte hoeveelheid tegoed hebt nadat u alle kosten in rekening gebracht en transacties in behandeling |
   | Huidig saldo    | De hoeveelheid tegoed sinds uw laatste factuur. Deze niet opgenomen in afwachting van transacties |
   | Transacties       | Alle facturering transacties die beïnvloed saldo van uw Azure-tegoed |

   Wanneer het saldo van uw geschatte op 0 komt, wordt u in rekening gebracht voor alle uw gebruik, met inbegrip van producten die worden gedekt door tegoed.

6. Selecteer **tegoed lijst** aan de lijst met tegoed voor de facturering profiel weergeven. De lijst met tegoed bevat de volgende informatie:

   ![Schermafbeelding van de lijsten tegoed voor een profiel voor facturering](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-list.png)

   | Termijn                 | Definitie                           |
   |----------------------|--------------------------------------------------------|
   | Bron               | De overname-bron van het tegoed |
   | Begindatum           | De datum waarop u het tegoed hebt aangeschaft |
   | Verloopdatum      | De datum waarop het tegoed verloopt |
   | Saldo              | Het saldo van uw laatste factuur |
   | Oorspronkelijke hoeveelheid      | De oorspronkelijke hoeveelheid tegoed |
   | Status               | De huidige status van krediet. De status kan actief is, worden gebruikt, is verlopen of verloopt |

## <a name="how-credits-are-used-in-microsoft-customer-agreement"></a>Hoe tegoed is gebruikt in Microsoft-overeenkomst voor de klant

In een rekening voor een Microsoft-KLANTOVEREENKOMST, kunt u facturering profielen gebruiken om uw facturen en de betalingswijzen te beheren. Een maandelijkse factuur wordt gegenereerd voor elke factureringsprofiel en u de betalingswijzen gebruiken de factuur te betalen.

Azure-tegoed vormen een van de betalingswijzen. U ontvangt tegoed van Microsoft, zoals aanbiedingsprijzen credit en servicevergoeding niveau. Deze tegoeden zijn toegewezen aan een profiel voor facturering. Als er een factuur wordt gegenereerd voor de facturering profiel worden tegoed automatisch toegepast op het totaal aantal gefactureerde bedrag voor het berekenen van de hoeveelheid die u nodig hebt om te betalen. U het resterende bedrag betalen met een andere betalingswijze, zoals controle- of overschrijving.

## <a name="products-that-arent-covered-by-azure-credits"></a>Producten die niet zijn gekoppeld aan Azure-tegoed

 De volgende producten niet zijn gekoppeld aan uw Azure-tegoed. U betaalt voor het gebruik van deze producten, ongeacht uw saldo:

- Canonical
- Citrix XenApp Essentials
- Citrix XenDesktop 
- Geregistreerde gebruiker
- Openlogic
- Externe toegang rechten XenApp Essentials geregistreerde gebruiker
- Ubuntu Advantage
- Visual Studio Enterprise (maandelijks)
- Visual Studio Enterprise (jaarlijks)
- Visual Studio Professional (maandelijks)
- Visual Studio Professional (jaarlijks)
- Azure Marketplace-producten
- Azure-ondersteuningsplannen

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over factureringsaccount voor KLANTOVEREENKOMST van Microsoft](billing-mca-overview.md)
- [Meer informatie over uw factuur KLANTOVEREENKOMST van Microsoft](billing-mca-understand-your-invoice.md)