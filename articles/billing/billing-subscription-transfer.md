---
title: Azure-abonnement eigendom overdragen aan een ander account | Microsoft Docs
description: Hierin wordt beschreven hoe u een Azure-abonnement overbrengen naar een andere gebruiker en enkele veelgestelde vragen (FAQ) over het proces
keywords: azure-abonnement, azure-overdracht abonnement overdraagt, azure-abonnement verplaatsen naar een ander abonnement eigenaar van de account, azure wijzigen, azure-abonnement overbrengen naar een ander account
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: c8ecdc1e-c9c5-468c-a024-94ae41e64702
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/13/2017
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7f061197cbe9fd52594d9fb000d8f3bcbd2d5285
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="transfer-ownership-of-an-azure-subscription-to-another-account"></a>Eigendom van een Azure-abonnement overdragen aan een ander account

Uw abonnement overbrengen naar een andere gebruiker in het midden van het Account voor het wijzigen van de accountbeheerder en afgeven abonnement eigendom. Zie voor informatie over het wijzigen van uw abonnement op een andere aanbieding [overschakelen van uw Azure-abonnement op een andere aanbieding](billing-how-to-switch-azure-offer.md).

> [!IMPORTANT]
> 
> Momenteel niet ondersteund abonnementsoverdracht voor gratis proefversie of [Azure in Open (AIO)](https://azure.microsoft.com/offers/ms-azr-0111p/) abonnementen. Zie voor een tijdelijke oplossing [resources verplaatsen naar de nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

## <a name="transfer-ownership-of-an-azure-subscription"></a>Het eigendom overdraagt van een Azure-abonnement

> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]
>
>

1. Meld u aan bij [Azure-Accountcentrum](https://account.windowsazure.com/Subscriptions) als de beheerder van het Account. Als u wilt weten wie de accountbeheerder van het abonnement is, Zie [Veelgestelde vragen over](#faq).

1. Selecteer het abonnement om over te dragen.

1. Controleer of uw abonnement in aanmerking komen voor overdracht eigen beheer door het controleren van de **bieden** en **bieden ID** met de [lijst van ondersteunde aanbiedingen](#supported).

   ![Aanbieding-ID van het abonnement in Accountcentrum controleren](./media/billing-subscription-transfer/image0.png)
1. Klik op **Abonnement overdragen**.

   ![Tabblad account voor Azure-abonnementen](./media/billing-subscription-transfer/image1.png)
1. Geef de ontvanger op.

   ![Dialoogvenster overdracht-abonnement](./media/billing-subscription-transfer/image2.PNG)
1. De ontvanger ontvangt automatisch een e-mail met een acceptatielink.

   ![Abonnement overdracht e-mailbericht naar ontvanger](./media/billing-subscription-transfer/image3.png)
1. De ontvanger klikt op de link en volgt de instructies, waaronder het invoeren van zijn of haar betaalgegevens.

   ![Eerste abonnement overdracht webpagina](./media/billing-subscription-transfer/image4.png)

   ![Tweede abonnement overdracht webpagina 's](./media/billing-subscription-transfer/image5.png)
1. Geslaagd Het abonnement worden nu overgedragen.

<a id="EA"></a>

## <a name="transfer-subscription-ownership-for-enterprise-agreement-ea-customers"></a>Het abonnement eigendom overdraagt voor klanten met Enterprise Agreement (EA)

De Enterprise-beheerder kan het eigendom overdraagt van abonnementen binnen een inschrijving. Om te beginnen, Zie [accounteigendom overdragen](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) in de portal EA.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Volgende stappen na het eigendom van een abonnement accepteren

1. U bent nu de Account-beheerder. Controleren en bijwerken van de servicebeheerder, Co-beheerders en andere RBAC-rollen. Zie voor meer informatie, [toevoegen of wijzigen Azure-beheerdersrollen die het abonnement of de services beheren](billing-add-change-azure-subscription-administrator.md).
1. Werk de referenties die zijn gekoppeld met dit abonnement services, waaronder:
   1. Van beheercertificaten die de gebruiker Administrator-rechten aan abonnementresources toewijzen. Zie voor meer informatie [maken en uploaden een beheer van het certificaat voor Azure](../cloud-services/cloud-services-certs-create.md)
   1. Sneltoetsen voor services zoals opslag. Zie voor meer informatie [over Azure storage-accounts](../storage/common/storage-create-storage-account.md)
   1. Referenties voor externe toegang voor services zoals Azure Virtual Machines. 
1. [Factureringsmeldingen bijwerken voor dit abonnement](billing-set-up-alerts.md) op de [Azure-Accountcentrum](https://account.windowsazure.com/Subscriptions). 
1. Als u met een partner werkt, kunt u het bijwerken van de partner-ID voor dit abonnement. U kunt bijwerken de partner-ID in de [Azure-portal](https://portal.azure.com).

<a id="supported"></a>

## <a name="whats-supported"></a>Wat wordt ondersteund:

Eigen beheer abonnementsoverdracht is beschikbaar voor de aanbiedingen of typen abonnementen die worden vermeld in de volgende tabel. Voor het overdragen van andere abonnementen, zoals [managers](https://azure.microsoft.com/offers/ms-azr-0036p/) of plannen, ondersteuning [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

| Naam van aanbieding:                                                                             | Nummer van de aanbieding |
|----------------------------------------------------------------------------------------|--------------|
| [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*|MS-AZR - 0017P        |
| [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)          | MS-AZR - 0025P        |
| [MSDN-Platforms](https://azure.microsoft.com/offers/ms-azr-0062p/)                     | MS-AZR - 0062P        |
| [Betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/)                      | MS-AZR - 0003P        |
| [Betalen naar gebruik ontwikkelen en testen](https://azure.microsoft.com/offers/ms-azr-0023p/)             | MS-AZR - 0023P        |
| [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)           | MS-AZR - 0063P        |
| [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/) | MS-AZR - 0064P        |
| [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)         | MS-AZR - 0059P        |
| [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)    | MS-AZR - 0060P        |

\*[Via EA-portal](#EA)

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen

### <a name="whoisaa"></a>Wie de accountbeheerder van het abonnement is?

De accountbeheerder is de persoon die zich heeft aangemeld of het Azure-abonnement hebt aangeschaft. Ze bevoegd zijn voor toegang tot de [Accountcentrum](https://account.azure.com/Subscriptions) en uitvoeren van verschillende beheertaken, zoals het maken van abonnementen, abonnementen annuleren, de facturering voor een abonnement te wijzigen of wijzigen van de servicebeheerder. Als u niet zeker weet wie de accountbeheerder is voor een abonnement, gebruikt u de volgende stappen uit om erachter te komen.

1. Ga naar de [pagina van de abonnementen in Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecteer het abonnement dat u wilt controleren, en ga vervolgens naar **instellingen**.
1. Selecteer **eigenschappen**. De accountbeheerder van het abonnement wordt weergegeven in de **accountbeheerder** vak.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Alles overbrengen? Met inbegrip van resourcegroepen, virtuele machines, schijven en andere actieve services?

Al uw resources, zoals virtuele machines, schijven en websites overdracht naar de nieuwe eigenaar. Echter [beheerdersrollen](billing-add-change-azure-subscription-administrator.md) en [op rollen gebaseerde toegangsbeheer (RBAC)](../active-directory/role-based-access-control-configure.md) beleidsregels die u hebt ingesteld, worden niet overgedragen op andere mappen. Bovendien [app registraties](../active-directory//develop/active-directory-integrating-applications.md) en andere services tenantspecifieke langs niet overdragen.

### <a id="no-button"></a>Waarom zie ik niet de knop 'Abonnement overdragen'?

Eigen beheer abonnementsoverdracht is helaas niet beschikbaar voor uw aanbieding of land. Om over te dragen van uw abonnement, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Resulteert overdracht van het abonnement in alle services actief blijven?

Er zijn geen gevolgen voor de service. Het abonnement overdragen Hiermee annuleert u het abonnement onder de huidige Account Administrator en maakt u een abonnement onder de account van de geadresseerde. Het nieuwe abonnement is gekoppeld aan de onderliggende Azure-services. De abonnements-ID blijft hetzelfde.

### <a name="how-do-i-use-this-process-to-change-the-directory-for-subscription"></a>Hoe gebruik ik dit proces de map voor het abonnement wilt wijzigen?

Een Azure-abonnement wordt gemaakt in de map die de accountbeheerder behoort. Als u de map, moet u het abonnement overbrengen naar een gebruikersaccount in de doelmap. Als die gebruiker de stappen voor het accepteren van overdracht voltooit, wordt het abonnement automatisch verplaatst naar de doelmap.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-organization-do-they-continue-to-have-access-to-my-resources"></a>Als ik eigendom van een abonnement van een andere organisatie overneemt, ze toegang hebben tot mijn resources blijven?

Als het abonnement is overgebracht naar een andere tenant, verliest de gebruikers die zijn gekoppeld aan de vorige tenant toegang tot het abonnement. Zelfs als een gebruiker niet een servicebeheerder is of Co-beheerder voordoet, die nog steeds toegang tot het abonnement via andere beveiligingsmechanismen hebben mogelijk, met inbegrip van:

* Van beheercertificaten die de gebruiker Administrator-rechten aan abonnementresources toewijzen. Zie voor meer informatie [maken en uploaden van een Beheercertificaat voor Azure](../cloud-services/cloud-services-certs-create.md).
* Sneltoetsen voor services zoals opslag. Zie voor meer informatie [over Azure storage-accounts](../storage/common/storage-create-storage-account.md).
* Referenties voor externe toegang voor services zoals Azure Virtual Machines.

Als de ontvanger moet toegang tot hun bronnen te beperken, overwegen ze geen geheimen die zijn gekoppeld aan de service bijwerken. De meeste bronnen kunnen worden bijgewerkt met behulp van de volgende stappen uit:

  1. Ga naar de [Azure Portal](https://portal.azure.com).
  2. Selecteer in het menu Hub **alle resources**.
  3. Selecteer de resource.
  4. Klik op de resourceblade **instellingen**. Hier kunt u weergeven en bijwerken van bestaande geheimen.

### <a name="if-i-transfer-the-subscription-in-the-middle-of-the-billing-cycle-does-the-recipient-pay-for-the-entire-billing-cycle"></a>Als ik het abonnement in het midden van de factureringscyclus overdraagt, de ontvanger betalen voor de hele facturering-cyclus?

De afzender is verantwoordelijk voor de betaling voor enig gebruik die is gerapporteerd naar het punt dat de overdracht is voltooid. De ontvanger is verantwoordelijk voor het gebruik gerapporteerd na het tijdstip van de overdracht en hoger. Mogelijk zijn er enkele verbruik dat heeft plaatsgevonden vóór overdracht maar later is gerapporteerd. Het gebruik is opgenomen in de ontvanger factuur.

### <a name="does-the-recipient-have-access-to-usage-and-billing-history"></a>De ontvanger toegang hebben tot gebruiks- en factureringsgeschiedenis?

  De enige informatie die beschikbaar zijn voor de ontvanger het bedrag van de laatste factuur is of als het abonnement is overgebracht voordat de eerste factuur is gegenereerd, het huidige saldo. De rest van de gebruiks- en factureringsgeschiedenis niet kan worden overgedragen aan het abonnement.

### <a name="can-the-offer-be-changed-during-a-transfer"></a>Kan de aanbieding worden gewijzigd tijdens een overdracht?

De aanbieding moet hetzelfde blijven. Zie het wijzigen van uw aanbieding [overschakelen van uw Azure-abonnement op een andere aanbieding](billing-how-to-switch-azure-offer.md).

### <a name="can-i-transfer-a-subscription-to-a-user-account-in-another-country"></a>Kan ik een abonnement overbrengen naar een gebruikersaccount in een ander land

Nee, een abonnement overbrengen naar een gebruikersaccount in een ander land wordt niet ondersteund. Het account van de geadresseerde gebruiker moet zich in hetzelfde land.

### <a name="can-the-recipient-use-a-different-payment-method"></a>Kan de ontvanger een andere betalingsmethode gebruiken?

Ja. Maar de abonnement-factureringsgeschiedenis verdeeld is over twee accounts.  

### <a name="is-the-payment-method-impacted-after-i-transferred-an-azure-subscription"></a>Is de betalingsmethode gevolgen nadat ik een Azure-abonnement hebt overgebracht?

Voor het accepteren van de overdracht van het abonnement moet een creditcard of vergelijkbare betalingsmethode om te betalen voor het abonnement worden opgegeven. Als Bob een abonnement naar ANS overdraagt en ANS de overdracht accepteert, moet ANS een betalingswijze om te betalen voor het abonnement opgeven. Nadat de overdracht voltooid is, wordt voor het abonnement niet Bob ANS gefactureerd.

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Hoe ik gegevens en services voor mijn Azure-abonnement migreren naar nieuw abonnement?

Als u niet kunt abonnement overdragen, kunt u handmatig migreren uw resources. Zie [resources verplaatsen naar de nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.

Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.