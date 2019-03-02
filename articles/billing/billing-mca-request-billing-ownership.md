---
title: Ophalen van eigendom van Azure-abonnementen | Microsoft Docs
description: Informatie over het aanvragen van eigendom van Azure-abonnementen van andere gebruikers.
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
ms.author: banders
ms.openlocfilehash: f8844f0a53b1c9595ce2766da23c385f75fed3ff
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57249051"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-users"></a>Ophalen van eigendom van Azure-abonnementen van andere gebruikers

Mogelijk wilt u eigenaar worden van Azure-abonnementen, als de eigenaar van de bestaande facturering van uw organisatie verlaat, of als u wilt betalen voor de abonnementen via uw factureringsrekening.

U kunt een aanvraag om eigenaar te worden van Azure-abonnementen van bestaande eigenaren in andere rekeningen te verzenden. Facturering verantwoordelijkheden van abonnementen eigenaar te worden overgedragen naar de sectie van uw factuur.

Om aan te vragen de eigendom, moet u een **factuur sectie eigenaar** of **factuur sectie Inzender**. Zie voor meer informatie, [sectie rollen taken factuur](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-access-to-a-microsoft-customer-agreement).

## <a name="request-billing-ownership-in-the-azure-portal"></a>Aanvraag voor facturatie eigendom in de Azure-portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van Azure portal zoeken voor kostenbeheer en facturering](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Ga naar het gedeelte factuur. Afhankelijk van uw toegang moet u mogelijk een factureringsaccount of factureringsprofiel selecteren. Selecteer in de facturering-account of profiel **factuur secties** en vervolgens een gedeelte van de factuur.
   <!-- Todo - Add a screenshot -->

4. Selecteer **aanvragen voor Abonnementsoverdracht** vanaf de linkerbenedenhoek.

5. Vanaf de bovenkant van de pagina, selecteer **toevoegen**.

6. Voer het e-mailadres van de gebruiker u om eigendom van vraagt. De gebruiker moet op een accountbeheerder op Microsoft Online Service Program factureringsaccount of de eigenaar van een account op een Enterprise Agreement. Zie voor meer informatie, [uw facturering accounts weergeven in Azure portal](billing-view-all-accounts.md).

   ![Schermafbeelding van een nieuwe overdrachtsaanvraag toevoegen](./media/billing-mca-request-billing-ownership/mca-new-transfer-request.png)

7. Selecteer **overdracht Verzendaanvraag**.

8. De gebruiker ontvangt een e-mail met instructies voor het controleren van uw aanvraag.

   ![Schermafbeelding die toont een beoordeling overdracht e-mail](./media/billing-mca-request-billing-ownership/mca-review-transfer-request-email.png)

9. Voor het goedkeuren van de aanvraag, wordt de gebruiker de koppeling selecteert in het e-mailbericht en de instructies.

    ![Schermafbeelding die toont een beoordeling overdracht e-mail](./media/billing-mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-status-of-your-transfer-request-in-the-azure-portal"></a>Controleer de status van uw aanvraag in Azure portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van Azure portal zoeken voor kostenbeheer en facturering](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Ga naar het gedeelte factuur. Afhankelijk van uw toegang moet u mogelijk een factureringsaccount of factureringsprofiel selecteren. Selecteer in de facturering-account of profiel **factuur secties** en vervolgens een gedeelte van de factuur.
   <!-- Todo - Add a screenshot -->

4. Selecteer **aanvragen voor Abonnementsoverdracht** vanaf de linkerbenedenhoek.

5. De overdracht aanvragen pagina bevat de volgende informatie:

    ![Schermafbeelding van lijst met aanvragen voor zoneoverdrachten](./media/billing-mca-request-billing-ownership/mca-view-transfer-requests.png)

   |Kolom|Definitie|
   |---------|---------|
   |Aanvraagdatum|De datum waarop de overdrachtsaanvraag is verzonden|
   |Geadresseerde|Het e-mailadres van de gebruiker die u hebt verzonden de aanvraag voor het abonnement overdragen|
   |Verloopdatum|De datum waarop de aanvraag verloopt|
   |Status|De status van aanvraag|

    De overdrachtsaanvraag kan een van de volgende statussen hebben:

   |Status|Definitie|
   |---------|---------|
   |Wordt uitgevoerd|De overdrachtsaanvraag is niet geaccepteerd door de gebruiker|
   |Verwerken|De gebruiker goedgekeurd de aanvraag. Facturering voor abonnementen die de gebruiker heeft geselecteerd wordt overgedragen naar de sectie van uw factuur|
   |Voltooid| De facturering voor abonnementen die de gebruiker heeft geselecteerd wordt overgebracht naar het gedeelte van uw factuur|
   |Voltooid met fouten|De aanvraag is voltooid, maar de facturering voor sommige abonnementen die de gebruiker heeft geselecteerd kan niet worden overgedragen|
   |Vervallen|De gebruiker de aanvraag niet accepteren op tijd en deze is verlopen|
   |Geannuleerd|Iemand met toegang tot de overdrachtsaanvraag de aanvraag geannuleerd|
   |Geweigerd|De gebruiker heeft de aanvraag geweigerd|

6. Selecteer een aanvraag om details weer te geven. De pagina met details van overdracht bevat de volgende informatie: <!-- Todo - Add a screenshot -->

   |Kolom  |Definitie|
   |---------|---------|
   |Overdracht aanvraag-ID|De unieke ID voor uw overdrachtsaanvraag. Als u een ondersteuningsaanvraag indient,-ID delen met de ondersteuning van Azure voor het versnellen van uw ondersteuningsaanvraag|
   |Overdracht is aangevraagd op|De datum waarop de overdrachtsaanvraag is verzonden|
   |Overdracht is aangevraagd door|Het e-mailadres van de gebruiker die de aanvraag verzonden|
   |Overdrachtsaanvraag verloopt op| De datum waarop de overdrachtsaanvraag verloopt|
   |E-mailadres van ontvanger|Het e-mailadres van de gebruiker die u hebt verzonden de aanvraag voor het abonnement overdragen|
   |Overdrachtskoppeling verzonden naar ontvanger|De url die naar de gebruiker om te controleren van de overdrachtsaanvraag is verzonden|

## <a name="additional-information"></a>Aanvullende informatie

De volgende sectie bevat aanvullende informatie over het overdragen van abonnementen.

### <a name="no-service-downtime"></a>Er is geen service-uitvaltijd

Azure-services in het abonnement blijven zonder enige onderbreking uitvoeren. We alleen overgang de facturering regelen voor de Azure-abonnementen die de gebruiker selecteert om over te dragen.

### <a name="disabled-subscriptions"></a>Uitgeschakelde abonnementen

Uitgeschakelde abonnementen kunnen niet worden overgedragen. Abonnementen moeten zich in de actieve status hun eigendom over te dragen.

### <a name="azure-resources-transfer"></a>Azure-resources overdragen

Alle resources van de abonnementen, zoals virtuele machines, schijven en websites-overdracht.

### <a name="azure-marketplace-products-transfer"></a>Overdracht van Azure Marketplace-producten

Azure Marketplace-producten overdragen, samen met hun respectieve abonnementen.

### <a name="azure-reservations-transfer"></a>Azure reserveringen-overdracht

Azure-reserveringen niet automatisch worden verplaatst met abonnementen. [Neem contact op met ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) reserveringen verplaatsen.

### <a name="access-to-azure-services"></a>Toegang tot Azure-services

Toegang tot Azure-resources die was ingesteld met behulp van Azure RBAC (op rollen gebaseerd toegangsbeheer) wordt niet beïnvloed tijdens de overgang.

### <a name="azure-support-plan"></a>Azure-ondersteuningsplan

Ondersteuning van Azure overdragen met de abonnementen niet. Als de gebruiker alle Azure-abonnementen verplaatst, vraagt u ze op hun ondersteuningsplan annuleren.

### <a name="charges-for-transferred-subscription"></a>Kosten voor het abonnement overgedragen

De oorspronkelijke facturering eigenaar van de abonnementen is verantwoordelijk voor eventuele kosten die zijn gerapporteerd tot het tijdstip dat de overdracht is voltooid. Het gedeelte van uw factuur is verantwoordelijk voor de kosten in rekening gebracht vanaf het moment van de overdracht en hoger wordt gerapporteerd. Mogelijk zijn er enkele kosten in rekening gebracht die heeft plaatsgevonden vóór overdracht, maar daarna is gemeld. Deze kosten weergegeven op uw factuur-sectie.

### <a name="supported-offers"></a>Ondersteunde aanbiedingen

Abonnement van typen of biedt, maar CSP biedt kan worden overgedragen.

### <a name="cancel-a-transfer-request"></a>Een overdrachtsaanvraag annuleren

U kunt de overdrachtsaanvraag annuleren totdat de aanvraag is goedgekeurd of afgewezen. De overdrachtsaanvraag annuleren [Ga naar de pagina met details van overdracht](#check-status-of-your-transfer-request-in-the-Azure-portal) en selecteer onderaan de pagina te annuleren.

### <a name="software-as-a-service-saas-transfer"></a>Software als een Service (SaaS)-overdracht

SaaS-producten worden niet overgedragen aan de abonnementen. Vraag de gebruiker naar [Neem contact op met Azure-ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) eigendom van SaaS-producten over te dragen. De gebruiker kan samen met het eigendom, resource-eigenaar ook overdragen. Resource-eigenaar kunt u beheerbewerkingen uitvoeren zoals verwijderen en de details van het product weergeven. Gebruiker moet de eigenaar van een resource op het product SaaS om over te dragen van resource-eigenaar.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen

- Het eigendom van de Azure-abonnementen wordt overgebracht naar het gedeelte van uw factuur. Bijhouden van de kosten voor deze abonnementen in de [Azure-portal](https://portal.azure.com).
- Andere machtigingen weergeven en beheren van facturering voor deze abonnementen geven. Zie voor meer informatie, [factuur sectie rollen en taken](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).