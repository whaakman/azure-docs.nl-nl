---
title: Ophalen van eigendom van Azure-abonnementen
description: Informatie over het aanvragen van eigendom van Azure-abonnementen van andere gebruikers.
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
ms.openlocfilehash: 4c9a8c866a157757121e6a9dd07a0a8559937c5e
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490834"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-accounts"></a>Ophalen van eigendom van Azure-abonnementen van andere accounts

Het is raadzaam om over te nemen eigendom van Azure-abonnementen als de eigenaar van de bestaande facturering van uw organisatie verlaat, of als u wilt betalen voor de abonnementen via uw factureringsrekening. Facturering verantwoordelijkheden van abonnementen eigenaar te worden overgedragen aan uw account.

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. [Controleer of u toegang tot een Microsoft-KLANTOVEREENKOMST hebt](#check-for-access).

Om aan te vragen de eigendom, moet u een **factuur sectie eigenaar** of **factuur sectie Inzender**. Zie voor meer informatie, [sectie rollen taken factuur](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

## <a name="request-billing-ownership"></a>Aanvraag voor facturatie eigendom

1. Aanmelden bij de [Azure-portal](http://portal.azure.com) als een factuur sectie eigenaar of bijdrager voor een rekening voor de KLANTOVEREENKOMST van Microsoft.

2. Zoeken naar **kosten Management en facturering**.

   ![Schermafbeelding van Azure portal zoeken voor kostenbeheer en facturering](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Selecteer **factuur secties** aan de linkerkant. Afhankelijk van uw toegang moet u mogelijk een factureringsaccount of factureringsprofiel selecteren. Selecteer in de facturering-account of profiel **secties factuur**.
   
   ![Schermafbeelding van selecteren secties factuur](./media/billing-mca-request-billing-ownership/mca-select-invoice-sections.png)        

4. Selecteer een gedeelte van de factuur in de lijst. Nadat u het eigendom van de abonnementen overnemen, worden ze in deze sectie factuur gefactureerd.

5. Selecteer **aanvragen voor Abonnementsoverdracht** uit de linksonder en selecteer vervolgens **toevoegen**.
 
   ![Schermafbeelding waarin de aanvragen voor zoneoverdrachten selecteren](./media/billing-mca-request-billing-ownership/mca-select-transfer-requests.png)

6. Voer het e-mailadres van de gebruiker u om eigendom van vraagt. De gebruiker moet een Administrator-Account op Microsoft Online Service Program factureringsaccount of de eigenaar van een account op een Enterprise Agreement. Zie voor meer informatie, [uw facturering accounts weergeven in Azure portal](billing-view-all-accounts.md). Selecteer **overdracht Verzendaanvraag**.

   ![Schermopname die laat zien dat er een overdrachtsaanvraag](./media/billing-mca-request-billing-ownership/mca-send-transfer-requests.png)

7. De gebruiker ontvangt een e-mail met instructies voor het controleren van uw aanvraag.

   ![Schermafbeelding die toont een beoordeling overdracht e-mail](./media/billing-mca-request-billing-ownership/mca-review-transfer-request-email.png)

8. Voor het goedkeuren van de aanvraag, wordt de gebruiker de koppeling selecteert in het e-mailbericht en de instructies.

    ![Schermafbeelding die toont een beoordeling overdracht e-mail](./media/billing-mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-transfer-request-status"></a>Controleer de status van de aanvraag voor overdracht

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoeken naar **kosten Management en facturering**.

   ![Schermafbeelding van Azure portal zoeken voor kostenbeheer en facturering](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)


3. Selecteer **factuur secties** aan de linkerkant. Afhankelijk van uw toegang moet u mogelijk een factureringsaccount of factureringsprofiel selecteren. Selecteer in de facturering-account of profiel **secties factuur**.
   
   ![Schermafbeelding van selecteren secties factuur](./media/billing-mca-request-billing-ownership/mca-select-invoice-sections.png)        

4. Selecteer de sectie factuur in de lijst waarvoor u de aanvraag hebt verzonden.

5. Selecteer **aanvragen voor Abonnementsoverdracht** vanaf de linkerbenedenhoek. De overdracht aanvragen pagina bevat de volgende informatie:

    ![Schermafbeelding van lijst met aanvragen voor zoneoverdrachten](./media/billing-mca-request-billing-ownership/mca-select-transfer-requests-for-status.png)

   |Kolom|Definitie|
   |---------|---------|
   |Aanvraagdatum|De datum waarop de overdrachtsaanvraag is verzonden|
   |Ontvanger|Het e-mailadres van de gebruiker die u hebt verzonden de aanvraag voor het abonnement overdragen|
   |Vervaldatum|De datum waarop de aanvraag verloopt|
   |Status|De status van aanvraag|

    De overdrachtsaanvraag kan een van de volgende statussen hebben:

   |Status|Definitie|
   |---------|---------|
   |Bezig|De overdrachtsaanvraag is niet geaccepteerd door de gebruiker|
   |Verwerken|De gebruiker goedgekeurd de aanvraag. Facturering voor abonnementen die de gebruiker heeft geselecteerd wordt overgedragen naar de sectie van uw factuur|
   |Voltooid| De facturering voor abonnementen die de gebruiker heeft geselecteerd wordt overgebracht naar het gedeelte van uw factuur|
   |Voltooid met fouten|De aanvraag is voltooid, maar de facturering voor sommige abonnementen die de gebruiker heeft geselecteerd kan niet worden overgedragen|
   |Vervallen|De gebruiker de aanvraag niet accepteren op tijd en deze is verlopen|
   |Geannuleerd|Iemand met toegang tot de overdrachtsaanvraag de aanvraag geannuleerd|
   |Afgewezen|De gebruiker heeft de aanvraag geweigerd|

7. Selecteer een aanvraag om details weer te geven. De pagina met details van overdracht bevat de volgende informatie:
   
   ![Schermafbeelding van lijst met abonnementen overgedragen](./media/billing-mca-request-billing-ownership/mca-transfer-completed.png)
    
   |Kolom  |Definitie|
   |---------|---------|
   |Overdracht aanvraag-ID|De unieke ID voor uw overdrachtsaanvraag. Als u een ondersteuningsaanvraag indient,-ID delen met de ondersteuning van Azure voor het versnellen van uw ondersteuningsaanvraag|
   |Overdracht is aangevraagd op|De datum waarop de overdrachtsaanvraag is verzonden|
   |Overdracht is aangevraagd door|Het e-mailadres van de gebruiker die de aanvraag verzonden|
   |Overdrachtsaanvraag verloopt op| De datum waarop de overdrachtsaanvraag verloopt|
   |E-mailadres van ontvanger|Het e-mailadres van de gebruiker die u hebt verzonden de aanvraag voor het abonnement overdragen|
   |Overdrachtskoppeling verzonden naar ontvanger|De url die naar de gebruiker om te controleren van de overdrachtsaanvraag is verzonden|

## <a name="supported-subscription-types"></a>Ondersteunde abonnementstypen

U kunt aanvragen eigendom van de abonnementstypen die hieronder worden vermeld.

- [Actie pack](https://azure.microsoft.com/offers/ms-azr-0025p/)\* 
- [Azure in Open Licensing](https://azure.microsoft.com/offers/ms-azr-0111p/)\*
- [Azure Pass-Sponsorship](https://azure.microsoft.com/offers/azure-pass/)\*
- [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p/)\*
- [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) 
- [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Microsoft Azure Plan](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*
- [Microsoft Azure gesponsorde aanbieding](https://azure.microsoft.com/offers/ms-azr-0036p/)\*
- [Microsoft Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)\*
- [MSDN Platforms](https://azure.microsoft.com/offers/ms-azr-0062p/)\*
- [Abonnees van Visual Studio Enterprise (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/)\*
- [Visual Studio Enterprise (MPN)-abonnees](https://azure.microsoft.com/offers/ms-azr-0029p/)\*
- [Visual Studio Enterprise-abonnees](https://azure.microsoft.com/offers/ms-azr-0063p/)\*
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)\*
- [Visual Studio Test Professional-abonnees](https://azure.microsoft.com/offers/ms-azr-0060p/)\*

\* Tegoed beschikbaar voor het abonnement wordt na de overdracht niet meer beschikbaar in het nieuwe account.

\*\* Alleen ondersteund voor abonnementen in de accounts die zijn gemaakt tijdens de registratie op de Azure-website.


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

Toegang voor bestaande gebruikers, groepen of service-principals die is toegewezen met behulp van (Azure RBAC (op rollen gebaseerd toegangsbeheer)) [.. /Role-based-Access-Control/Overview.MD] wordt niet beïnvloed tijdens de overgang.

### <a name="azure-support-plan"></a>Azure-ondersteuningsplan

Ondersteuning van Azure overdragen met de abonnementen niet. Als de gebruiker alle Azure-abonnementen verplaatst, vraagt u ze op hun ondersteuningsplan annuleren.

### <a name="charges-for-transferred-subscription"></a>Kosten voor het abonnement overgedragen

De oorspronkelijke facturering eigenaar van de abonnementen is verantwoordelijk voor eventuele kosten die zijn gerapporteerd tot het tijdstip dat de overdracht is voltooid. Het gedeelte van uw factuur is verantwoordelijk voor de kosten in rekening gebracht vanaf het moment van de overdracht en hoger wordt gerapporteerd. Mogelijk zijn er enkele kosten in rekening gebracht die heeft plaatsgevonden vóór overdracht, maar daarna is gemeld. Deze kosten weergegeven op uw factuur-sectie.

### <a name="cancel-a-transfer-request"></a>Een overdrachtsaanvraag annuleren

U kunt de overdrachtsaanvraag annuleren totdat de aanvraag is goedgekeurd of afgewezen. Als u wilt de overdrachtsaanvraag annuleren, gaat u naar de [pagina met details van overdracht](#check-the-transfer-request-status) en selecteer onderaan de pagina te annuleren.

### <a name="software-as-a-service-saas-transfer"></a>Software als een Service (SaaS)-overdracht

SaaS-producten worden niet overgedragen aan de abonnementen. Vraag de gebruiker naar [Neem contact op met Azure-ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) eigendom van SaaS-producten over te dragen. De gebruiker kan samen met het eigendom, resource-eigenaar ook overdragen. Resource-eigenaar kunt u beheerbewerkingen uitvoeren zoals verwijderen en de details van het product weergeven. Gebruiker moet de eigenaar van een resource op het product SaaS om over te dragen van resource-eigenaar.

## <a name="check-for-access"></a>Controle van toegang
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning

Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen

- Het eigendom van de Azure-abonnementen wordt overgebracht naar het gedeelte van uw factuur. Bijhouden van de kosten voor deze abonnementen in de [Azure-portal](https://portal.azure.com).
- Andere machtigingen weergeven en beheren van facturering voor deze abonnementen geven. Zie voor meer informatie, [factuur sectie rollen en taken](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).
