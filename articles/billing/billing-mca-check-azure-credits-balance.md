---
title: Azure-tegoed saldo voor een Microsoft-KLANTOVEREENKOMST volgen
description: Informatie over het controleren van de Azure-tegoed voor een Microsoft-KLANTOVEREENKOMST.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 352737b3ea61a51a39e066d4211c8f4ceae74184
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490966"
---
# <a name="track-microsoft-customer-agreement-azure-credit-balance"></a>Microsoft Customer overeenkomst Azure-tegoed bijhouden

U kunt de Azure-tegoed voor Microsoft-KLANTOVEREENKOMST controleren in Azure portal. U credits gebruiken om te betalen voor de kosten die worden gedekt door het tegoed.

U betaalt wanneer u producten die niet zijn gekoppeld aan het tegoed gebruiken of uw gebruik uw tegoed overschrijdt. Zie voor meer informatie [producten die niet zijn gekoppeld aan Azure-tegoed. () #products-that-aren't-covered-by-azure-credits).

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement).

## <a name="check-your-credit-balance"></a>Uw saldo controleren

1. Meld u aan bij [Azure Portal]( https://portal.azure.com).

2. Zoeken naar **kosten Management en facturering**.

    ![Schermafbeelding van zoeken in de portal voor kostenbeheer en facturering](./media/billing-mca-check-azure-credits-balance/billing-search-cost-management-billing.png)

3.  Selecteer **Azure-tegoed** aan de linkerkant. Afhankelijk van uw toegang mogelijk moet u voor het selecteren van een factureringsaccount of een profiel voor facturering en selecteer vervolgens **Azure-tegoed**.

4. De Azure-tegoed pagina bevat de volgende informatie:

   ![Schermopname van het saldo en transacties voor een profiel voor facturering](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-overview.png)

   | Termijn               | Definitie                           |
   |--------------------|--------------------------------------------------------|
   | Geschatte saldo  | Geschatte hoeveelheid tegoed hebt nadat u alle kosten in rekening gebracht en transacties in behandeling |
   | Het huidige saldo    | De hoeveelheid tegoed sinds uw laatste factuur. Deze niet opgenomen in afwachting van transacties |
   | Transacties       | Alle facturering transacties die beïnvloed saldo van uw Azure-tegoed |

   Wanneer het saldo van uw geschatte op 0 komt, wordt u in rekening gebracht voor alle uw gebruik, met inbegrip van producten die worden gedekt door tegoed.

6. Selecteer **tegoed lijst** aan de lijst met tegoed voor de facturering profiel weergeven. De lijst met tegoed bevat de volgende informatie:

   ![Schermafbeelding van de lijsten tegoed voor een profiel voor facturering](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-list.png)

   | Termijn | Definitie |
   |---|---|
   | Geschatte saldo | Hoeveelheid van het Azure-tegoed hebt na aftrek van in aanmerking komende kosten van de niet-gefactureerde tegoed van uw huidige saldo|
   | Het huidige saldo | Hoeveelheid van het Azure-tegoed hebt voordat tegoed voor niet-gefactureerde kosten voor in aanmerking komt. Dit wordt berekend door het toevoegen van nieuwe Azure-tegoed die u ontvangen en heb het saldo op het moment van uw laatste factuur|
   | source | De overname-bron van het tegoed |
   | Begindatum | De datum waarop u het tegoed hebt aangeschaft |
   | Vervaldatum | De datum waarop het tegoed verloopt |
   | Saldo | Het saldo van uw laatste factuur |
   | Oorspronkelijke hoeveelheid | De oorspronkelijke hoeveelheid tegoed |
   | Status | De huidige status van krediet. De status kan actief is, worden gebruikt, is verlopen of verloopt |

## <a name="how-credits-are-used"></a>Hoe tegoed is gebruikt

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

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over factureringsaccount voor KLANTOVEREENKOMST van Microsoft](billing-mca-overview.md)
- [Meer informatie over uw factuur KLANTOVEREENKOMST van Microsoft](billing-mca-understand-your-invoice.md)
