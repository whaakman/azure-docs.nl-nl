---
title: Eigendom van Azure-abonnement naar een ander account overdragen | Microsoft Docs
description: Beschrijving van het eigendom van een Azure-abonnement overdragen naar een ander account, en enkele veelgestelde vragen (FAQ) over het proces
keywords: overdracht van abonnement voor azure-abonnement, azure-overdracht, azure-abonnement verplaatsen naar een andere eigenaar van het wijzigen abonnement-account, azure, azure-abonnement overdragen naar een ander account, azure-overdracht facturering
author: bandersmsft
manager: amberb
tags: billing,top-support-issue
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cff3c57c31526119ab81225a1c70b163173be937
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514438"
---
# <a name="transfer-billing-ownership-of-an-azure-subscription-to-another-account"></a>Eigendom van een Azure-abonnement overdragen naar ander account

Het is raadzaam eigendom over te dragen van uw Azure-abonnement als u uw organisatie verlaat, of als u wilt dat uw abonnement naar een ander account in rekening gebracht. Overdracht van eigendom naar een ander account biedt de beheerders in de machtiging voor de nieuwe account om uit te voeren taken facturering als betalingsmethode wijzigen, kosten bekijken en het abonnement hebt geannuleerd.

Als u wilt dat het eigendom maar wijzigen het type van uw abonnement, Zie [uw Azure-abonnement overstappen naar een andere aanbieding](billing-how-to-switch-azure-offer.md). Als u om te bepalen wie resources in het abonnement beheren wilt kan, Zie [ingebouwde rollen voor Azure-resources](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Als u een Enterprise-Agreement(EA)-klant bent, kunnen uw Ondernemingsadministrators eigendom van uw abonnementen overdragen tussen accounts. Zie voor meer informatie, [overdragen van Enterprise Agreement (EA)-abonnementen](#transfer-billing-ownership-of-enterprise-agreement-ea-subscriptions).

## <a name="transfer-billing-ownership-of-an-azure-subscription"></a>Eigendom van een Azure-abonnement overdragen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als beheerder van de rekening met het abonnement dat u wilt overdragen. Als u wilt weten als u beheerder bent, Zie [Veelgestelde vragen over](#faq).

1. Zoek naar **kostenbeheer en facturering**.

   ![Schermafbeelding van zoeken in Azure portal](./media/billing-subscription-transfer/billing-search-cost-management-billing.png)

1. Selecteer **abonnementen** in het linkerdeelvenster. Afhankelijk van uw toegang mogelijk moet u een facturering bereik selecteren en selecteer vervolgens **abonnementen** of **Azure-abonnementen**.

1. Selecteer **overdragen naar ander account** voor het abonnement dat u wilt overdragen. 

   ![Abonnement selecteren om over te dragen](./media/billing-subscription-transfer/billing-select-subscription-to-transfer.png)

1. Voer het e-mailadres van een gebruiker die een beheerder van de facturering van het account dat wordt de nieuwe eigenaar van het abonnement en selecteer vervolgens **overdracht Verzendaanvraag**.

    > [!IMPORTANT]
    >
    > Als u eigendom van uw abonnement overdragen aan een gebruikersaccount in een andere Azure AD-tenant, alle [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md) toewijzingen voor het beheren van resources in het abonnement worden definitief verwijderd. Alleen de nieuwe eigenaar heeft toegang tot resources in het abonnement te beheren. Zie voor meer informatie, [abonnement overdragen aan een gebruiker in een andere Azure AD-tenant](../active-directory/managed-identities-azure-resources/known-issues.md).
  
    ![Gegevensoverdracht-pagina verzenden](./media/billing-subscription-transfer/billing-send-transfer-request.PNG)

1. De gebruiker ontvangt een e-mail met instructies voor het controleren van uw aanvraag.

   ![E-mail van abonnement overbrengen naar ontvanger](./media/billing-subscription-transfer/billing-receiver-email.png)

1. Voor het goedkeuren van de aanvraag, wordt de gebruiker de koppeling selecteert in het e-mailbericht en de instructies. De gebruiker moet een betalingsmethode dat wordt gebruikt om te betalen voor het abonnement te selecteren. Bovendien, als de gebruiker niet over een Azure-account, zou er om u te registreren voor een nieuw account. 

   ![Eerste webpagina voor overdracht van abonnement](./media/billing-subscription-transfer/billing-accept-ownership-step1.png)

   ![Tweede webpagina voor overdracht van abonnement](./media/billing-subscription-transfer/billing-accept-ownership-step2.png)

   ![Tweede webpagina voor overdracht van abonnement](./media/billing-subscription-transfer/billing-accept-ownership-step3.png)

1. Gelukt! Nu wordt het abonnement overgedragen.

## <a name="transferring-subscription-to-an-account-in-another-azure-ad-tenant"></a>Abonnement overbrengen naar een account in een andere Azure AD-tenant

Een tenant Azure Active Directory (AD) is voor u gemaakt wanneer u zich aanmeldt voor Azure. Uw account wordt vertegenwoordigd door de tenant. U kunt de tenant gebruiken voor het beheren van toegang tot uw abonnementen en resources.

Wanneer u een nieuw abonnement maakt, wordt deze wordt gehost in de Azure AD-tenant van uw account. Als u wilt dat anderen toegang tot uw abonnement of resources, moet u ze aan uw tenant uitnodigen. Dit helpt u toegang tot uw abonnementen en resources beheren.

Wanneer u eigendom van uw abonnement aan een account in een andere Azure AD-tenant overdraagt, wordt het abonnement is verplaatst naar het nieuwe account-tenant. Alle gebruikers, groepen of service-principals die gehad [op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control/overview.md) voor het beheren van resources in het abonnement raken deze toegang kwijt. Alleen de gebruiker in het nieuwe account die uw overdrachtsaanvraag accepteert heeft toegang om de resources te beheren. Om toegang te bieden voor gebruikers die oorspronkelijk toegang was, de nieuwe eigenaar zelf zou moeten [deze gebruikers handmatig toevoegen aan het abonnement](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).


## <a name="transferring-visual-studio-microsoft-partner-network-mpn-and-pay-as-you-go-devtest-subscriptions"></a>Overdracht van Visual Studio, Microsoft Partner Network (MPN) en betalen Dev/Test-abonnementen

Visual Studio en Microsoft Partner Network-abonnementen hebt maandelijks terugkerende Azure-tegoed dat is gekoppeld aan deze. Wanneer u deze abonnementen overdraagt, is uw tegoed niet beschikbaar in de doel-factureringsaccount. Het abonnement maakt gebruik van de creditcard in de doel-factureringsaccount. Als Bob een Visual Studio Enterprise-abonnement naar de account op 9 september en ANS ANS van overdraagt accepteert bijvoorbeeld de overdracht. Nadat de overdracht is voltooid, wordt het abonnement wordt gestart met behulp van de creditcard in ANS de account. Het tegoed worden opnieuw ingesteld op 9de elke maand. 


<a id="EA"></a>

## <a name="transfer-billing-ownership-of-enterprise-agreement-ea-subscriptions"></a>Overdragen van Enterprise Agreement (EA)-abonnementen

De Enterprise-beheerder kan eigendom van abonnementen overdragen tussen accounts binnen een inschrijving. Zie voor meer informatie, [accounteigendom Transfer](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) in de EA-portal.

<a id="CSP"></a>

## <a name="next-steps-after-accepting-billing-ownership"></a>Volgende stappen na de eigendom accepteren

Als u het eigendom van een Azure-abonnement hebt geaccepteerd, raden we dat u de volgende stappen bekijken:

1. Controleren en bijwerken van de servicebeheerder, Co-beheerders en andere RBAC-rollen. Zie voor meer informatie, [toevoegen of wijzigen Azure-abonnementbeheerders](billing-add-change-azure-subscription-administrator.md) en [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).
1. Referenties van de update die is gekoppeld aan services van dit abonnement met inbegrip van:
   1. Van beheercertificaten die aan de gebruiker beheerdersrechten voor resources-abonnement. Zie voor meer informatie, [maken en uploaden van een certificaat voor Azure](../cloud-services/cloud-services-certs-create.md)
   1. Toegangssleutels voor services zoals opslag. Zie voor meer informatie, [over Azure storage-accounts](../storage/common/storage-create-storage-account.md)
   1. Referenties voor externe toegang voor services zoals Azure Virtual Machines.
1. Als u met een partner werkt, kunt u overwegen de partner-ID voor dit abonnement wordt bijgewerkt. U kunt bijwerken de partner-ID in de [Azure-portal](https://portal.azure.com). Zie voor meer informatie, [een partner-ID koppelen aan uw Azure-accounts](billing-partner-admin-link-started.md)

<a id="supported"></a>

## <a name="supported-subscription-types"></a>Ondersteunde abonnementstypen

Overdracht van het abonnement in Azure portal is beschikbaar voor de hieronder vermelde abonnementstypen. Overdracht wordt momenteel niet ondersteund voor [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p/) of [Azure in Open (AIO)](https://azure.microsoft.com/offers/ms-azr-0111p/) abonnementen. Zie voor een tijdelijke oplossing [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md). Om over te dragen van andere abonnementen, zoals [Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/) of ondersteuningsabonnementen, [Neem contact op met ondersteuning voor Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*
- [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)  
- [Visual Studio Enterprise (MPN)-abonnees](https://azure.microsoft.com/offers/ms-azr-0029p/) 
- [MSDN Platforms](https://azure.microsoft.com/offers/ms-azr-0062p/)  
- [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) 
- [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)
- [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/)
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)
- [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)
- [Microsoft Azure Plan](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*

\* [Via de portal EA](#EA).

\*\* Alleen ondersteund voor accounts die zijn gemaakt tijdens het aanmelden van op de Azure-website. 

<a id="faq"></a>

## <a name="frequently-asked-questions-faq-for-senders"></a>Veelgestelde vragen (FAQ) over afzenders

Deze Veelgestelde vragen over de van toepassing op gebruikers die het eigendom van een Azure-abonnement naar een ander account overbrengt.

### <a name="whoisaa"></a> Wie is een factureringsbeheerder van een account?

Factureringsbeheerder is een persoon die de machtiging voor het beheren van facturering voor een account heeft. Ze zijn gemachtigd voor toegang tot facturering op de [Azure-portal](https://portal.azure.com) en uitvoeren van verschillende facturering taken zoals het maken van abonnementen, facturen weergeven en betalen of update betalingswijzen.

Voor het identificeren van abonnementen waarvoor u een facturering-beheerder bent, gebruikt u de volgende stappen uit:

1. Ga naar de [kostenbeheer en facturering-pagina in Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview).
1. Selecteer **abonnementen** in het linkerdeelvenster. Afhankelijk van uw toegang mogelijk moet u een facturering bereik selecteren en selecteer vervolgens **abonnementen** of **Azure-abonnementen**
1. De pagina abonnementen geeft een lijst van alle abonnementen waarvoor u een factureringsbeheerder zijn.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Alles wat overbrengen? Met inbegrip van resourcegroepen, virtuele machines, schijven en andere actieve services?

Al uw resources, zoals virtuele machines, schijven en websites worden overgedragen naar het nieuwe account. Echter, als u abonnement overdraagt aan een account in een andere Azure AD-tenant, een [beheerdersrollen](billing-add-change-azure-subscription-administrator.md) en [rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/role-assignments-portal.md) toewijzingen voor het abonnement [niet overdracht](#transferring-subscription-to-an-account-in-another-azure-ad-tenant). Bovendien [app-registraties](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) en andere tenant-specifieke services samen met het abonnement niet overdragen.

### <a name="can-i-transfer-ownership-to-an-account-in-another-country"></a>Kan ik overdragen aan een account in een ander land?
Helaas, cross-land dient overdracht kunnen niet worden uitgevoerd in Azure portal. Voor het overdragen van uw abonnement in de landen, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="i-am-an-administrator-on-two-accounts-can-i-transfer-a-subscription-from-one-of-my-accounts-to-another"></a>Ik ben een beheerder op twee accounts. Kan ik een abonnement overbrengen van een van mijn accounts naar een andere?
Ja, kunt u abonnement overbrengen tussen uw accounts. Uw accounts worden conceptueel gezien beschouwd als accounts van twee verschillende gebruikers, zodat u de bovenstaande stappen gebruiken kunt om over te dragen van abonnementen tussen uw accounts.

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Resulteert overdracht van het abonnement in een service-uitvaltijd?

Als u een abonnement aan een gebruiker in dezelfde Azure AD-tenant overdragen, zijn er geen gevolgen voor de resources die worden uitgevoerd in het abonnement.  Echter, als u het abonnement overdraagt aan een gebruiker in een andere tenant, alle gebruikers, groepen en service-principals die gehad [op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control/overview.md) voor het beheren van resources in het abonnement raken deze toegang kwijt. 

### <a name="does-the-recipient-have-access-to-usage-and-billing-history"></a>Beschikt over de ontvanger toegang tot de gebruiks- en factureringsgeschiedenis?

De enige informatie die beschikbaar zijn voor de ontvanger is van de afgelopen maand kosten voor uw abonnement. De rest van het gebruik en de factureringsgeschiedenis draagt aan het abonnement

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Hoe Migreer ik gegevens en services voor mijn Azure-abonnement naar een nieuw abonnement?

Als u niet kan het abonnement overdragen, kunt u handmatig uw resources migreren. Zie [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

### <a name="if-i-transfer-a-visual-studio-or-microsoft-partner-network-subscription-does-my-credit-carry-forward-with-the-subscription-in-the-new-account"></a>Als ik een Visual Studio of Microsoft Partner Network-abonnement overdraagt, Mijn tegoed overdragen aan het abonnement in het nieuwe account?

Nee, uw tegoed is niet beschikbaar in het nieuwe account. De gebruiker die de overdrachtsaanvraag accepteert, moet beschikken over een Visual Studio-licentie te accepteren van de aanvraag voor overdracht. Het abonnement gebruikmaakt van het tegoed voor Visual Studio die beschikbaar is in het gebruikersaccount. Zie voor meer informatie, [Visual Studio overdragen, Microsoft Partner Network (MPN) en betalen Dev/Test-abonnementen](#transferring-visual-studio-microsoft-partner-network-mpn-and-pay-as-you-go-devtest-subscriptions)


## <a name="frequently-asked-questions-faq-for-recipients"></a>Veelgestelde vragen (FAQ) over ontvangers

Deze Veelgestelde vragen over de van toepassing op gebruikers die via het eigendom van een Azure-abonnement van een ander account.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-account-do-users-in-that-account-continue-to-have-access-to-my-resources"></a>Als ik eigendom van een abonnement van een ander account overnemen, doet u gebruikers in dat account nog steeds toegang tot mijn resources?

Als het abonnement wordt overgedragen aan een account in dezelfde Azure AD-tenant, alle gebruikers, groepen en service-principals die gehad [op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control/overview.md) voor het beheren van resources in het abonnement behouden hun toegang. Als u wilt weergeven van gebruikers die beschikken over RBAC toegang tot het abonnement, gebruikt u de volgende stappen uit:

1. Ga naar de [abonnementspagina in de Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer het abonnement dat u wilt controleren, en selecteer vervolgens **toegangsbeheer (IAM)** in het linkerdeelvenster.
1. Selecteer **roltoewijzingen** vanaf de bovenkant van de pagina. De rol toewijzingen pagina geeft een lijst van alle gebruikers die beschikken over RBAC voor het abonnement.

Als het abonnement wordt overgedragen aan een account in een andere Azure AD-tenant, alle gebruikers, groepen en service-principals die gehad [op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control/overview.md) voor het beheren van resources in het abonnement raken deze toegang kwijt. Zelfs als ze geen RBAC toegang meer hebt, kunnen ze echter nog steeds toegang tot het abonnement via bepaalde beveiligingsmechanismen, met inbegrip van hebben:

* Van beheercertificaten die aan de gebruiker beheerdersrechten voor resources-abonnement. Zie voor meer informatie, [maken en uploaden van een Beheercertificaat voor Azure](../cloud-services/cloud-services-certs-create.md).
* Toegangssleutels voor services zoals opslag. Zie [Over Azure-opslagaccounts](../storage/common/storage-create-storage-account.md) voor meer informatie.
* Referenties voor externe toegang voor services zoals Azure Virtual Machines.

Als de ontvanger nodig heeft om de toegang tot hun resources te beperken, moeten ze rekening houden met alle geheimen die zijn gekoppeld aan de service wordt bijgewerkt. De meeste resources kunnen worden bijgewerkt met behulp van de volgende stappen uit:

  1. Meld u aan bij [Azure Portal](https://portal.azure.com).
  2. Selecteer in het menu Hub **alle resources**.
  3. Selecteer de resource.
  4. Klik in de pagina voor de resource op **instellingen**. Hier kunt u bekijken en bijwerken van bestaande geheimen.

### <a name="if-i-take-over-the-billing-ownership-of-a-subscription-in-the-middle-of-the-billing-cycle-do-i-have-to-pay-for-the-entire-billing-cycle"></a>Als ik het eigendom van een abonnement in het midden van de factureringscyclus overnemen, heb ik betalen voor de hele factureringscyclus?

Uw account is verantwoordelijk voor de betaling voor het gebruik die moet worden gerapporteerd vanaf het moment van de overdracht en hoger. Mogelijk zijn er enkele gebruik die heeft plaatsgevonden vóór overdracht, maar daarna is gemeld. Het gebruik is opgenomen in de factuur van uw account.

### <a name="can-i-use-a-different-payment-method"></a>Kan ik een andere betalingswijze gebruiken?

Ja. Tijdens het accepteren van de aanvraag voor overdracht, kunt u een bestaande betalingwijze die is gekoppeld aan uw account of een nieuwe betalingswijze toevoegen.

## <a name="troubleshooting"></a>Problemen oplossen

### <a id="no-button"></a> Waarom zie ik niet de knop 'Abonnement overdragen'?

De overdracht selfserviceoverdracht van abonnementen is helaas niet beschikbaar voor uw factureringsrekening. Op dit moment ondersteund niet overdragen van het eigendom van Enterprise Agreement (EA)-account in Azure portal. Bovendien KLANTOVEREENKOMST van Microsoft-accounts die zijn gemaakt tijdens het uitvoeren van Microsoft-verkoop bieden geen ondersteuning voor het overdragen van het eigendom van abonnementen. 

### <a id="no-button"></a> Waarom wordt mijn abonnement op ondersteuning voor overdracht Typ? 

Helaas ondersteuning niet alle typen abonnementen voor facturering worden overgedragen. Lijst met abonnementstypen die ondersteuning bieden voor overdrachten, Zie [abonnementstypen ondersteund](#supported-subscription-types)

### <a id="no-button"></a> Waarom krijg ik een fout: toegang geweigerd wanneer ik probeer om over te dragen van eigendom van een abonnement? 

U ziet deze fout als u probeert om over te dragen van een Microsoft Azure-abonnement en u niet de benodigde machtiging hebt. Om over te dragen van een Microsoft Azure-abonnement, moet u een eigenaar of bijdrager in het gedeelte van de factuur waarop het abonnement wordt in rekening gebracht. Zie voor meer informatie, [beheren van abonnementen voor factuur sectie](billing-understand-mca-roles.md#manage-subscriptions-for-invoice-section).


## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

- Controleren en bijwerken van de servicebeheerder, Co-beheerders en andere RBAC-rollen. Zie voor meer informatie, [toevoegen of wijzigen Azure-abonnementbeheerders](billing-add-change-azure-subscription-administrator.md) en [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).
