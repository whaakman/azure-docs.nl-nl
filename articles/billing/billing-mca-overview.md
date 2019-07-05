---
title: Aan de slag met uw Microsoft-KLANTOVEREENKOMST factureringsrekening - Azure
description: Inzicht in de KLANTOVEREENKOMST van uw Microsoft account facturering
author: bandersmsft
manager: amberbhargava
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 87483c967641489e9548f38c99eebbf121d0d252
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490735"
---
# <a name="get-started-with-your-microsoft-customer-agreement-billing-account"></a>Aan de slag met uw Microsoft-KLANTOVEREENKOMST factureringsrekening

Factureringsaccount wordt gemaakt wanneer u zich aanmeldt om Azure te gebruiken. U gebruikt uw factureringsrekening voor het beheren van facturen, betalingen en kosten bijhouden. U kunt hebben toegang tot meerdere accounts voor facturering. Bijvoorbeeld, u mogelijk hebben geregistreerd voor Azure voor uw persoonlijke projecten. Kan hebt u ook toegang tot Azure via de Enterprise overeenkomst of Microsoft KLANTOVEREENKOMST van uw organisatie. Voor elk van deze scenario's moet u een afzonderlijke rekening.

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement).

## <a name="your-billing-account"></a>Uw factureringsrekening

Uw factureringsrekening voor de klant Microsoft overeenkomst bevat een of meer facturering profielen waarmee u uw facturen en betalingswijzen beheren. Elke factureringsprofiel bevat een of meer factuur secties waarmee u kosten voor de facturering van het profiel factuur kunt indelen.

Het volgende diagram toont de relatie tussen een factureringsaccount, profielen voor facturering en factuur secties.

![Diagram van de Microsoft-KLANTOVEREENKOMST hiërarchie facturering](./media/billing-mca-overview/mca-billing-hierarchy.png)

Rollen op de rekening zijn het hoogste niveau van machtigingen. Standaard alleen de gebruiker die zich hebben aangemeld voor Azure krijgt toegang tot de factureringsaccount. Deze rollen moeten worden toegewezen aan gebruikers die willen facturen weergeven en de kosten voor uw hele organisatie, zoals de afdeling Financiën of IT-managers bijhouden. Zie voor meer informatie, [facturering account rollen en taken](billing-understand-mca-roles.md#billing-account-roles-and-tasks).

## <a name="billing-profiles"></a>Facturering-profielen

Gebruik een facturering profiel voor het beheren van uw factuur- en -methoden. Een maandelijkse factuur wordt gegenereerd aan het begin van de maand voor elke factureringsprofiel in uw account. De factuur bevat respectieve kosten in rekening gebracht voor alle Azure-abonnementen en andere aankopen in de vorige maand.

Een profiel voor facturering wordt automatisch gemaakt voor uw factureringsrekening. Het bevat gedeelte van een factuur standaard. U kunt extra secties voor het eenvoudig bijhouden en kosten op basis van uw behoeften ordenen maken of is per project, afdeling of ontwikkelingsomgeving. Deze secties ziet u op de facturering van het profiel factuur zetten op basis van het gebruik van elk abonnement en u hebt toegewezen aan deze aankopen.

Rollen op de facturering profielen hebben machtigingen voor facturen en betalingswijzen weergeven en beheren. Deze rollen toewijzen aan gebruikers die facturen, zoals leden van het team accounting in uw organisatie betalen. Zie voor meer informatie, [facturering profiel rollen en taken](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="each-billing-profile-gets-a-monthly-invoice"></a>Elke factureringsprofiel haalt een maandelijkse factuur

Een maandelijkse factuur wordt gegenereerd aan het begin van de maand voor elke factureringsprofiel. De factuur bevat alle kosten van de vorige maand.

U kunt de factuur bekijken, documenten downloaden en instellen om op te halen van toekomstige facturen per e-mail, in de Azure-portal. Zie voor meer informatie, [facturen downloaden voor een Microsoft-KLANTOVEREENKOMST](billing-download-azure-invoice-daily-usage-date.md#download-invoices-for-a-microsoft-customer-agreement).

### <a name="invoice-payment-methods"></a>De betalingswijzen factuur

Elke factureringsprofiel heeft een eigen betalingswijzen die worden gebruikt om te betalen van de facturen. De volgende betalingswijzen worden ondersteund:

| Type             | Definitie  |
|------------------|-------------|
|Azure-tegoed    |  Tegoeden worden automatisch toegepast op de in aanmerking komende kosten op uw factuur, verminderen de hoeveelheid die u nodig hebt om te betalen. Zie voor meer informatie, [saldo van de Azure-tegoed voor uw factureringsprofiel volgen](billing-mca-check-azure-credits-balance.md). |
|Selectievakje/wire overdracht | Als uw account is goedgekeurd voor betaling aan via selectievakje/overschrijving. U kunt het bedrag betalen verschuldigd zijn voor uw factuur via selectievakje/overschrijving. De instructies voor de betaling worden gegeven op de factuur |
|Creditcard | Klanten die zich aanmelden voor Azure via de Azure-website kunnen betalen via een creditcard. |

### <a name="apply-policies-to-control-purchases"></a>Beleid toepassen om te bepalen van aankopen

Beleid toepassen op het besturingselement Azure Marketplace en aankopen in de reservering met behulp van een profiel voor facturering. U kunt beleid voor het uitschakelen van de aankoop van Azure-reserveringen en Marketplace-producten instellen. Wanneer het beleid wordt toegepast, worden niet-abonnementen die in rekening aan het profiel van de facturering gebracht worden voor deze aankopen gebruikt.

### <a name="azure-plans-determine-pricing-and-service-level-agreement-for-subscriptions"></a>De ondersteuningsabonnementen Azure bepalen prijs- en service level agreement voor abonnementen

De ondersteuningsabonnementen Azure bepalen de prijzen en serviceovereenkomsten voor Azure-abonnementen. Ze zijn automatisch ingeschakeld wanneer u een facturering profiel maakt. Alle factuur secties die gekoppeld aan de factureringsprofiel zijn kunnen deze abonnementen gebruiken. Gebruikers met toegang tot het gedeelte factuur gebruiken de abonnementen te maken van Azure-abonnementen. De volgende Azure-abonnementen worden ondersteund in de facturering van accounts voor Microsoft-KLANTOVEREENKOMST:

| Plannen             | Definitie  |
|------------------|-------------|
|Microsoft Azure Plan   | Gebruikers toestaan te maken van abonnementen die werkbelastingen kunnen worden uitgevoerd. Zie voor meer informatie, [plannen van Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0017g/) |
|Microsoft Azure-abonnement voor ontwikkelen en testen | Visual Studio-abonnees die abonnementen maken die beperkt voor de ontwikkeling van zijn toestaan of testworkloads. Deze abonnementen Profiteer van voordelen zoals lagere tarieven en toegang tot exclusieve virtuele machine-installatiekopieën in Azure portal. Zie voor meer informatie, [voor DevTest plannen Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0148g/)|

## <a name="invoice-sections"></a>Factuur secties

Factuur secties om in te delen van de kosten op uw factuur maken. Bijvoorbeeld, wellicht u een enkele factuur voor uw organisatie, maar wilt om te organiseren van kosten per afdeling, team of project. Voor dit scenario hebt u één facturering profiel waarin u een gedeelte van de factuur voor elke afdeling, team of project maken.

Wanneer een gedeelte van de factuur wordt gemaakt, kunt u anderen machtiging voor het maken van Azure-abonnementen die in rekening aan de sectie gebracht worden geven. Alle kosten voor het gebruik en aankopen voor de abonnementen worden vervolgens naar de sectie gefactureerd.

Rollen in de sectie factuur beschikken over machtigingen om te bepalen wie Azure-abonnementen maakt. Deze rollen toewijzen aan gebruikers die Azure-omgeving voor teams in onze organisatie, zoals technische leads en technische architecten instellen. Zie voor meer informatie, [factuur sectie rollen en taken](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over uw factureringsrekening:

- [Informatie over Microsoft KLANTOVEREENKOMST beheerdersrollen in Azure](billing-understand-mca-roles.md)
- [Een extra Azure-abonnement voor Microsoft-KLANTOVEREENKOMST maken](billing-mca-create-subscription.md)
- [Secties maken op uw factuur voor het ordenen van uw kosten](billing-mca-section-invoice.md)
