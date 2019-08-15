---
title: Eigendom van de facturering van Azure-abonnementen ophalen
description: Meer informatie over het aanvragen van een facturerings eigendom van Azure-abonnementen van andere gebruikers.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 110bd975de1d865c2aa3d3b088c98cdc8b9e45ad
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69019559"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-accounts"></a>Eigendom van de facturering van Azure-abonnementen van andere accounts ophalen

Mogelijk wilt u het eigendom van Azure-abonnementen overnemen als de bestaande facturerings eigenaar uw organisatie verlaat of als u wilt betalen voor de abonnementen via uw facturerings account. Als u eigenaar bent, worden facturerings verantwoordelijkheden van abonnementen naar uw account overgedragen.

Dit artikel is van toepassing op een facturerings account voor een klant overeenkomst van micro soft. [Controleer of u toegang hebt tot een micro soft-klant overeenkomst](#check-for-access).

Als u het eigendom van de facturering wilt aanvragen, moet u een **factuur sectie eigenaar** of **bijdrager deel**van de factuur zijn. Zie voor meer informatie het [gedeelte taken van de factuur sectie](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="request-billing-ownership"></a>Eigendom van facturering aanvragen

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als een factuur sectie eigenaar of bijdrager voor een facturerings account voor de micro soft-klant overeenkomst.

2. Zoeken naar **Cost Management en facturering**.

   ![Scherm opname van Azure Portal zoek naar Cost Management + billing](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Selecteer **factuur secties** aan de linkerkant. Afhankelijk van uw toegang moet u mogelijk een facturerings account of facturerings profiel selecteren. Selecteer **factuur secties**van het facturerings account of-profiel.
   
   ![Scherm opname van het selecteren van factuur secties](./media/billing-mca-request-billing-ownership/mca-select-invoice-sections.png)        

4. Selecteer een factuur gedeelte in de lijst. Zodra u het eigendom van de abonnementen overneemt, worden deze in rekening gebracht voor deze factuur sectie.

5. Selecteer **overdrachts aanvragen** aan de linkerkant en selecteer vervolgens **toevoegen**.
 
   ![Scherm opname van het selecteren van overdrachts aanvragen](./media/billing-mca-request-billing-ownership/mca-select-transfer-requests.png)

6. Voer het e-mail adres in van de gebruiker voor wie u het eigendom van de facturering wilt aanvragen. De gebruiker moet een account beheerder zijn op een micro soft online service-programma facturerings account of een account eigenaar op een Enterprise Agreement. Zie [uw facturerings accounts weer geven in azure Portal](billing-view-all-accounts.md)voor meer informatie. Selecteer **overdrachts aanvraag verzenden**.

   ![Scherm opname van het verzenden van een overdrachts aanvraag](./media/billing-mca-request-billing-ownership/mca-send-transfer-requests.png)

7. De gebruiker ontvangt een e-mail met instructies om uw overdrachts aanvraag te controleren.

   ![Scherm opname van e-mail over de overdrachts aanvraag bekijken](./media/billing-mca-request-billing-ownership/mca-review-transfer-request-email.png)

8. De gebruiker selecteert de koppeling in het e-mail bericht en volgt de instructies om de overdrachts aanvraag goed te keuren.

    ![Scherm opname van e-mail over de overdrachts aanvraag bekijken](./media/billing-mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-transfer-request-status"></a>Controleer de status van de overdrachts aanvraag

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoeken naar **Cost Management en facturering**.

   ![Scherm opname van Azure Portal zoek naar Cost Management + billing](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)


3. Selecteer **factuur secties** aan de linkerkant. Afhankelijk van uw toegang moet u mogelijk een facturerings account of facturerings profiel selecteren. Selecteer **factuur secties**van het facturerings account of-profiel.
   
   ![Scherm opname van het selecteren van factuur secties](./media/billing-mca-request-billing-ownership/mca-select-invoice-sections.png)        

4. Selecteer de sectie factuur in de lijst waarvoor u de overdrachts aanvraag hebt verzonden.

5. Selecteer **overdrachts aanvragen** aan de linkerkant. Op de pagina overdrachts aanvragen wordt de volgende informatie weer gegeven:

    ![Scherm opname van de lijst met overdrachts aanvragen](./media/billing-mca-request-billing-ownership/mca-select-transfer-requests-for-status.png)

   |Kolom|Definitie|
   |---------|---------|
   |Aanvraag datum|De datum waarop de overdrachts aanvraag is verzonden|
   |Geadresseerde|Het e-mail adres van de gebruiker aan wie u de aanvraag voor het overdragen van het eigendom van de facturering hebt verzonden|
   |Verloopdatum|De datum waarop de aanvraag verloopt|
   |Status|De status van de overdrachts aanvraag|

    De overdrachts aanvraag kan een van de volgende statussen hebben:

   |Status|Definitie|
   |---------|---------|
   |Wordt uitgevoerd|De gebruiker heeft de overdrachts aanvraag niet geaccepteerd|
   |Verwerken|De gebruiker heeft de overdrachts aanvraag goedgekeurd. Facturering voor abonnementen die de gebruiker heeft geselecteerd, wordt overgebracht naar uw factuur gedeelte|
   |Voltooid| De facturering voor abonnementen die de gebruiker heeft geselecteerd, wordt overgebracht naar uw factuur gedeelte|
   |Voltooid met fouten|De aanvraag is voltooid, maar de facturering voor sommige abonnementen die de gebruiker heeft geselecteerd, kan niet worden overgedragen|
   |Vervallen|De gebruiker heeft de aanvraag niet op tijd geaccepteerd en is verlopen|
   |Geannuleerd|Iemand met toegang tot de overdrachts aanvraag heeft de aanvraag geannuleerd|
   |Geweigerd|De gebruiker heeft de overdrachts aanvraag geweigerd|

7. Selecteer een overdrachts aanvraag om details weer te geven. Op de pagina met gegevens over de overdracht wordt de volgende informatie weer gegeven:
   
   ![Scherm opname van de lijst met verzonden abonnementen](./media/billing-mca-request-billing-ownership/mca-transfer-completed.png)
    
   |Kolom  |Definitie|
   |---------|---------|
   |ID van de overdrachts aanvraag|De unieke ID voor uw overdrachts aanvraag. Als u een ondersteunings aanvraag indient, deelt u de ID met de ondersteuning van Azure om uw ondersteunings aanvraag te versnellen|
   |Overdracht aangevraagd op|De datum waarop de overdrachts aanvraag is verzonden|
   |Overdracht aangevraagd door|Het e-mail adres van de gebruiker die de overdrachts aanvraag heeft verzonden|
   |De overdrachts aanvraag verloopt op| De datum waarop de overdrachts aanvraag verloopt|
   |E-mailadres van ontvanger|Het e-mail adres van de gebruiker aan wie u de aanvraag voor het overdragen van het eigendom van de facturering hebt verzonden|
   |Overdrachts koppeling verzonden naar ontvanger|De URL die naar de gebruiker is verzonden om de overdrachts aanvraag te controleren|

## <a name="supported-subscription-types"></a>Ondersteunde abonnements typen

U kunt het eigendom van de facturering aanvragen van de hieronder vermelde typen abonnementen.

- [Actie pakket](https://azure.microsoft.com/offers/ms-azr-0025p/)\* 
- [Azure in Open Licensing](https://azure.microsoft.com/offers/ms-azr-0111p/)\*
- [Azure Pass sponsoring](https://azure.microsoft.com/offers/azure-pass/)\*
- [Enter prise dev/test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Gratis proef versie](https://azure.microsoft.com/offers/ms-azr-0044p/)\*
- [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) 
- [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Microsoft Azure Plan](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*
- [Gesponsorde aanbieding Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0036p/)\*
- [Microsoft Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)\*
- [MSDN Platforms](https://azure.microsoft.com/offers/ms-azr-0062p/)\*
- [BizSpark-abonnees (Visual Studio Enter prise)](https://azure.microsoft.com/offers/ms-azr-0064p/)\*
- [MPN-abonnees (Visual Studio Enter prise)](https://azure.microsoft.com/offers/ms-azr-0029p/)\*
- [Visual Studio Enter prise-abonnees](https://azure.microsoft.com/offers/ms-azr-0063p/)\*
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)\*
- [Visual Studio test Professional-abonnees](https://azure.microsoft.com/offers/ms-azr-0060p/)\*

\*Alle tegoeden die beschikbaar zijn voor het abonnement, zijn na de overdracht niet meer beschikbaar in het nieuwe account.

\*\*Alleen ondersteund voor abonnementen in accounts die worden gemaakt tijdens het aanmelden op de Azure-website.


## <a name="additional-information"></a>Aanvullende informatie

In de volgende sectie vindt u meer informatie over het overdragen van abonnementen.

### <a name="no-service-downtime"></a>Geen uitval tijd van service

Azure-Services in het abonnement blijven actief zonder onderbreking. We scha kelen alleen de facturerings relatie uit voor de Azure-abonnementen die de gebruiker selecteert om over te dragen.

### <a name="disabled-subscriptions"></a>Uitgeschakelde abonnementen

Uitgeschakelde abonnementen kunnen niet worden overgedragen. Abonnementen moeten de status actief hebben om hun eigendom van de facturering te kunnen overdragen.

### <a name="azure-resources-transfer"></a>Azure-resources overdragen

Alle resources van de abonnementen, zoals Vm's, schijven en websites, worden overgebracht.

### <a name="azure-marketplace-products-transfer"></a>Azure Marketplace-Producten overdragen

Azure Marketplace-producten worden overgebracht samen met hun respectieve abonnementen.

### <a name="azure-reservations-transfer"></a>Azure Reservations overdracht

Azure Reservations worden niet automatisch verplaatst met abonnementen. [Neem contact op met de ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om reserve ringen te verplaatsen.

### <a name="access-to-azure-services"></a>Toegang tot Azure-Services

Toegang voor bestaande gebruikers, groepen of service-principals die zijn toegewezen met (Azure RBAC (op rollen gebaseerd toegangs beheer)) [.. /Role-based-Access-Control/overview.MD] wordt niet beïnvloed tijdens de overgang.

### <a name="azure-support-plan"></a>Azure-ondersteuningsplan

Ondersteuning voor Azure wordt niet overgedragen met de abonnementen. Als de gebruiker alle Azure-abonnementen overdraagt, vraagt u deze om hun ondersteunings plan te annuleren.

### <a name="charges-for-transferred-subscription"></a>Kosten voor overgedragen abonnement

De oorspronkelijke facturerings eigenaar van de abonnementen is verantwoordelijk voor alle kosten die zijn gerapporteerd tot het moment waarop de overdracht is voltooid. Uw factuur gedeelte is verantwoordelijk voor de kosten die worden gerapporteerd vanaf het tijdstip van de overdracht. Er zijn mogelijk enkele kosten die plaatsvonden vóór de overdracht, maar die daarna werd gerapporteerd. Deze kosten worden weer gegeven in uw factuur gedeelte.

### <a name="cancel-a-transfer-request"></a>Een overdrachts aanvraag annuleren

U kunt de overdrachts aanvraag annuleren totdat de aanvraag is goedgekeurd of geweigerd. Als u de overdrachts aanvraag wilt annuleren, gaat u naar de [pagina Details overdragen](#check-the-transfer-request-status) en selecteert u annuleren onder aan de pagina.

### <a name="software-as-a-service-saas-transfer"></a>SaaS-overdracht (Software as a Service)

SaaS-producten worden niet overgedragen met de abonnementen. Vraag de gebruiker om [contact](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) op te nemen met de ondersteuning van Azure voor het overdragen van de eigendom van SaaS-producten. Samen met het eigendom van de facturering kan de gebruiker ook het eigendom van resources overdragen. Met bron eigendom kunt u beheer bewerkingen uitvoeren, zoals het verwijderen en weer geven van de details van het product. De gebruiker moet een resource-eigenaar zijn van het SaaS-product om eigendom van de resource over te dragen.

## <a name="check-for-access"></a>Controleren op toegang
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning

Als u hulp nodig hebt, [neemt u contact op met de ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel op te lossen.

## <a name="next-steps"></a>Volgende stappen

- Het eigendom van de Azure-abonnementen wordt overgezet naar uw factuur gedeelte. Houd de kosten voor deze abonnementen bij in de [Azure Portal](https://portal.azure.com).
- Andere machtigingen geven om facturering voor deze abonnementen weer te geven en te beheren. Zie voor meer informatie [factuur sectie rollen en taken](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).
