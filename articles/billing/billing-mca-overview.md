---
title: Aan de slag met uw factureringsrekening voor een Microsoft-KLANTOVEREENKOMST - Azure | Microsoft Docs
description: Meer informatie over factureringsaccount voor een Microsoft-KLANTOVEREENKOMST
services: billing
documentationcenter: ''
author: amberbhargava
manager: amberbhargava
editor: banders
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: c263669d7f74d8abebcd84e818ae2ccf2dcac0f0
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57249018"
---
# <a name="get-started-with-your-billing-account-for-a-microsoft-customer-agreement"></a>Aan de slag met uw factureringsrekening voor een Microsoft-KLANTOVEREENKOMST

Factureringsaccount is gemaakt voor elke overeenkomst dat u zich aanmelden met Microsoft om Azure te gebruiken. U kunt uw factureringsrekening beheren de facturering en kosten bijhouden. U kunt hebben toegang tot meerdere accounts voor facturering. Bijvoorbeeld, u mogelijk hebben geregistreerd voor Azure voor uw persoonlijke projecten. Kan hebt u ook toegang tot Azure via de Enterprise overeenkomst of Microsoft KLANTOVEREENKOMST van uw organisatie. Voor elk van deze scenario's moet u een afzonderlijke rekening.

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement).

## <a name="understand-billing-account"></a>Factureringsaccount begrijpen

Uw factureringsrekening voor de klant Microsoft overeenkomst bevat een of meer profielen voor facturering. Elke factureringsprofiel heeft een eigen factuur- en -methoden. De facturering profiel bevat een of meer factuur secties waarmee u kosten voor de facturering van het profiel factuur kunt indelen.

Het volgende diagram toont de relatie tussen een factureringsaccount, de profielen voor facturering en de factuur secties.

![Diagram met facturering hiërarchie voor KLANTOVEREENKOMST van Microsoft](./media/billing-mca-overview/mca-billing-hierarchy.png)

Rollen op de rekening zijn het hoogste niveau van machtigingen. Deze rollen toewijzen aan gebruikers die nodig hebt om facturen te bekijken en de kosten voor uw hele organisatie, zoals de afdeling Financiën of IT-managers bijhouden. Zie voor meer informatie, [facturering account rollen en taken](billing-understand-mca-roles.md#billing-account-roles-and-tasks).

## <a name="understand-billing-profiles"></a>Inzicht in facturering-profielen

Gebruik een facturering profiel voor het beheren van uw factuur- en -methoden. Een maandelijkse factuur wordt gegenereerd voor Azure-abonnementen en andere producten die zijn gekocht met het profiel voor facturering. De betalingsmethoden kunt u de factuur betaalt.

Een profiel voor facturering wordt automatisch gemaakt voor uw factureringsrekening. U kunt nieuwe facturering profielen voor het instellen van extra facturen maken. Bijvoorbeeld, kunt u verschillende facturen voor elke afdeling of project in uw organisatie.

U kunt ook factuur secties voor het ordenen van kosten op een factureringsprofiel factuur maken. Kosten voor Azure-abonnementen en producten die zijn gekocht voor een gedeelte van de factuur weergegeven in de sectie. De facturering van het profiel factuur omvat de kosten voor alle secties factuur.

Rollen op de facturering profielen hebben machtigingen voor facturen en betalingswijzen weergeven en beheren. Deze rollen toewijzen aan gebruikers die facturen, zoals leden van het team accounting in uw organisatie betalen. Zie voor meer informatie, [facturering profiel rollen en taken](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="monthly-invoice-generated-for-each-billing-profile"></a>Maandelijkse factuur voor elke factureringsprofiel gegenereerd

Een maandelijkse factuur wordt gegenereerd op de datum voor elk profiel facturering. De factuur bevat alle kosten voor de vorige maand.

U kunt de factuur bekijken, documenten downloaden en instellen om op te halen van toekomstige facturen per e-mail, in de Azure-portal. Zie voor meer informatie, [ophalen van uw factuur in e-mailbericht](billing-download-azure-invoice-daily-usage-date.md#get-your-invoice-in-email-pdf).

### <a name="invoices-paid-through-payment-methods"></a>Facturen betaald via betalingsmethoden

Elke factureringsprofiel heeft een eigen betalingswijzen die worden gebruikt om te betalen van de facturen. De volgende betalingswijzen worden ondersteund:

| Type             | Definitie  |
|------------------|-------------|
|Azure-tegoed    |  Tegoeden worden automatisch toegepast op het totaal aantal gefactureerde bedrag op uw factuur voor het berekenen van de hoeveelheid die u nodig hebt om te betalen. Zie voor meer informatie, [bijhouden Azure saldo van uw factureringsprofiel](billing-mca-check-azure-credits-balance.md). |
|Cheque of directe overboeking | U kunt het verschuldigde bedrag betalen voor uw factuur overdragen via het selectievakje of -bedrading. De instructies voor de betaling worden gegeven op de factuur |

### <a name="control-azure-marketplace-and-reservation-purchases-by-applying-policies"></a>Besturingselement voor Azure Marketplace en reservering aankopen door het toepassen van beleid

Toepassen van beleid voor het beheren van aankopen gedaan met behulp van een profiel voor facturering. U kunt beleid voor het uitschakelen van de aankoop van Azure-reserveringen en Marketplace-producten instellen. Wanneer het beleid wordt toegepast, kunnen niet abonnementen die zijn gemaakt voor de factuur secties in het profiel van de facturering worden gebruikt voor het kopen van reserveringen voor Azure en Marketplace-producten.

### <a name="allow-users-to-create-azure-subscriptions-by-enabling-azure-plans"></a>Gebruikers toestaan te maken van Azure-abonnementen doordat Azure-abonnementen

Azure-abonnementen zijn automatisch ingeschakeld wanneer u een facturering profiel maakt. Alle factuur secties in het profiel voor facturering krijgen toegang tot deze abonnementen. Gebruikers met toegang tot het gedeelte factuur gebruiken de abonnementen te maken van Azure-abonnementen. Zij kunnen niet Azure-abonnementen maken, tenzij een Azure-abonnement is ingeschakeld voor het profiel voor facturering. De volgende Azure-abonnementen worden ondersteund in de facturering van accounts voor Microsoft-KLANTOVEREENKOMST:

| Plannen             | Definitie  |
|------------------|-------------|
|Microsoft Azure Plan   | Gebruikers toestaan te maken van abonnementen die werkbelastingen kunnen worden uitgevoerd. Zie voor meer informatie, [plannen van Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0017g/) |
|Microsoft Azure-abonnement voor ontwikkelen en testen | Visual Studio-abonnees die abonnementen maken die beperkt voor de ontwikkeling van zijn toestaan of testworkloads. Deze abonnementen Profiteer van voordelen zoals lagere tarieven en toegang tot exclusieve virtuele machine-installatiekopieën in Azure portal. Zie voor meer informatie, [voor DevTest plannen Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0148g/)| <!--- TODO - Add the link to plan details page -->

## <a name="understand-invoice-sections"></a>Secties factuur begrijpen

Factuur secties om in te delen van de kosten op een factureringsprofiel factuur maken. Bijvoorbeeld, wellicht u een enkele factuur voor uw organisatie, maar wilt om te organiseren van kosten per afdeling, team of project. Voor dit scenario hebt u één facturering profiel waarin u een gedeelte van de factuur voor elke afdeling, team of project maken.

Wanneer een gedeelte van de factuur wordt gemaakt, kunt u anderen machtiging voor het maken van Azure-abonnementen voor de sectie geven. Alle kosten voor het gebruik en aankopen voor de abonnementen worden vervolgens doorgevoerd op de juiste sectie van de factuur.

Rollen in de sectie factuur beschikken over machtigingen om te bepalen wie Azure-abonnementen maakt. Deze rollen toewijzen aan gebruikers die Azure-omgeving voor teams in onze organisatie, zoals technische leads en technische architecten instellen. Zie voor meer informatie, [factuur sectie rollen en taken](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over uw factureringsrekening:

- [Informatie over Microsoft KLANTOVEREENKOMST beheerdersrollen in Azure](billing-understand-mca-roles.md)
- [Een Azure-abonnement voor uw factureringsrekening voor Microsoft-KLANTOVEREENKOMST maken](billing-mca-create-subscription.md)
- [Kosten met factuur secties organiseren](billing-mca-section-invoice.md)