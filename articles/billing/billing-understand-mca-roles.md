---
title: Meer informatie over facturering beheerdersrollen voor overeenkomsten in de Microsoft-klant - Azure
description: Meer informatie over facturering rollen voor de facturering van accounts in Azure voor Microsoft-klant-overeenkomsten.
services: billing
documentationcenter: ''
author: amberbhargava
manager: amberbhargava
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: 780870cc71e95507a52ba6a9338026f895a96ac1
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57834894"
---
# <a name="understand-microsoft-customer-agreement-administrative-roles-in-azure"></a>Informatie over Microsoft KLANTOVEREENKOMST beheerdersrollen in Azure

Voor het beheren van uw factureringsrekening voor een Microsoft-KLANTOVEREENKOMST, gebruikt u de rollen die worden beschreven in de volgende secties. Deze rollen zijn naast de ingebouwde rollen die Azure heeft toegang tot resources beheren. Zie voor meer informatie, [ingebouwde rollen voor Azure-resources](../role-based-access-control/built-in-roles.md).

In dit artikel is van toepassing op een rekening voor een Microsoft-KLANTOVEREENKOMST. Controleer of u toegang tot de KLANTOVEREENKOMST van een Microsoft hebt.

## <a name="billing-role-definitions"></a>Roldefinities voor facturering

De volgende tabel beschrijft de facturering rollen die u gebruiken voor het beheren van uw factureringsrekening facturering van profielen en secties factuur.

|Rol|Description|
|---|---|
|Eigenaar van factureringsaccount|Alles voor de facturering van account beheren|
|Inzender van factureringsaccount|Alles beheren behalve machtigingen voor de facturering account|
|Lezer van factureringsaccount|Alleen-lezen weergave van alles op de facturering van account|
|Eigenaar van factureringsprofiel|Alles voor de facturering van profiel beheren|
|Inzender van factureringsprofiel|Alles beheren behalve machtigingen voor het profiel voor facturering|
|Lezer van factureringsprofiel|Alleen-lezen weergave van alles op de facturering van profiel|
|Factuurbeheerder|Weergeven en facturen betalen voor de facturering van profiel|
|Factuursectie voor eigenaar|Alles op factuur sectie beheren|
|Factuursectie voor inzender|Alles beheren behalve machtigingen voor het gedeelte factuur|
|Factuursectie voor lezer|Alleen-lezen weergave van alles wat in de sectie factuur|
|De maker van de Azure-abonnement|Azure-abonnementen maken|

## <a name="billing-account-roles-and-tasks"></a>Rollen en taken voor factureringsaccount

Factureringsaccount kunt u beheren de facturering voor uw organisatie. U factureringsaccount gebruiken voor het ordenen van kosten, monitor kosten in rekening gebracht en facturen en facturering toegangsbeheer voor uw organisatie. Zie voor meer informatie, [begrijpen factureringsaccount](billing-mca-overview.md#understand-billing-account).

De volgende tabellen ziet u welke rol u taken uitvoeren in de context van het factureringsaccount wilt.

### <a name="manage-billing-account-permissions-and-properties"></a>Facturering accountmachtigingen en eigenschappen beheren

|Taak|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount|
|---|---|---|---|
|Bestaande machtigingen voor de facturering van account weergeven|✔|✔|✔|
|Machtigingen weergeven en beheren van het factureringsaccount anderen|✔|✘|✘|
|Facturering-account-eigenschappen weergeven, zoals bedrijfsnaam, adres en meer|✔|✔|✔|

### <a name="manage-billing-profiles-for-billing-account"></a>Facturering-profielen beheert voor facturering van account

|Taak|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount|
|---|---|---|---|
|Alle facturering profielen weergeven in het account|✔|✔|✔|

### <a name="manage-invoices-for-billing-account"></a>Facturen voor de facturering van account beheren

|Taak|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount|
|---|---|---|---|
|Alle facturen weergeven in het account|✔|✔|✔|
|Downloaden van facturen, Azure files voor gebruik en de kosten, prijzenoverzichten en btw-documenten in het account|✔|✔|✔|

### <a name="manage-invoice-sections-for-billing-account"></a>Factuur secties voor factureringsaccount beheren

|Taak|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount|
|---|---|---|---|
|Alle secties van de factuur bekijken in het account|✔|✔|✔|

### <a name="manage-transactions-for-billing-account"></a>Transacties voor de facturering van account beheren

|Taak|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount|
|---|---|---|---|
|Alle facturering transacties voor het account weergeven|✔|✔|✔|
|Alle producten die zijn gekocht voor het account weergeven|✔|✔|✔|

### <a name="manage-subscriptions-for-billing-account"></a>Abonnementen voor de facturering van account beheren

|Taak|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount|
|---|---|---|---|
|Alle Azure-abonnementen weergeven in de factureringsaccount|✔|✔|✔|

## <a name="billing-profile-roles-and-tasks"></a>Rollen en taken voor factureringsprofiel

Een factureringsprofiel kunt u uw facturen en betalingswijzen beheren. Een maandelijkse factuur wordt gegenereerd voor de Azure-abonnementen en andere producten die zijn gekocht met het profiel voor facturering. De betalingsmethoden kunt u de factuur betaalt. Zie voor meer informatie, [inzicht in facturering profielen](billing-mca-overview.md#understand-billing-profiles).

De volgende tabellen ziet u welke rol u taken uitvoeren in de context van het profiel voor facturering moet.

### <a name="manage-billing-profile-permissions-properties-and-policies"></a>Machtigingen voor uw facturering, eigenschappen en beleidsregels beheren

|Taak|Eigenaar van factureringsprofiel|Inzender van factureringsprofiel|Lezer van factureringsprofiel|Factuur-Manager|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount
|---|---|---|---|---|---|---|---|
|Bestaande machtigingen voor de facturering profiel weergeven|✔|✔|✔|✔|✔|✔|✔|
|Machtigingen weergeven en beheren van de factureringsprofiel anderen|✔|✘|✘|✘|✘|✘|✘|
|Facturering Profieleigenschappen weergeven, zoals IO-nummer, e-mailbericht factuur voorkeur en meer|✔|✔|✔|✔|✔|✔|✔|
|Facturering profieleigenschappen bijwerken |✔|✔|✘|✘|✘|✘|✘|
|Weergave-beleid toegepast op de facturering profiel, zoals aankopen in de Azure-reservering inschakelen, schakelt u Azure marketplace-aankopen, en meer|✔|✔|✔|✔|✔|✔|✔|
|Beleidsregels toepassen op het profiel voor facturering |✔|✔|✘|✘|✘|✘|✘|

### <a name="manage-invoices-for-billing-profile"></a>Facturen voor de facturering van profiel beheren

|Taak|Eigenaar van factureringsprofiel|Inzender van factureringsprofiel|Lezer van factureringsprofiel|Factuur-Manager|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount
|---|---|---|---|---|---|---|---|
|De facturen voor de facturering profiel weergeven|✔|✔|✔|✔|✔|✔|✔|
|Downloaden van facturen, Azure files voor gebruik en de kosten, prijzenoverzichten en btw-documenten voor de facturering-profiel|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-invoice-sections-for-billing-profile"></a>Factuur secties voor facturering profiel beheren

|Taak|Eigenaar van factureringsprofiel|Inzender van factureringsprofiel|Lezer van factureringsprofiel|Factuur-Manager|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount
|---|---|---|---|---|---|---|---|
|Alle secties in de factuur voor de facturering profiel weergeven|✔|✔|✔|✔|✔|✔|✔|
|Nieuwe sectie van de factuur voor de facturering-profiel maken|✔|✔|✘|✘|✘|✘|✘|

### <a name="manage-transactions-for-billing-profile"></a>Transacties voor de facturering van profiel beheren

|Taak|Eigenaar van factureringsprofiel|Inzender van factureringsprofiel|Lezer van factureringsprofiel|Factuur-Manager|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount
|---|---|---|---|---|---|---|---|
|Alle facturering transacties voor de facturering profiel weergeven|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-payment-methods-for-billing-profile"></a>Betalingswijzen voor de facturering van profiel beheren

|Taak|Eigenaar van factureringsprofiel|Inzender van factureringsprofiel|Lezer van factureringsprofiel|Factuur-Manager|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount
|---|---|---|---|---|---|---|---|
|Betalingsmethoden voor de facturering profiel weergeven|✔|✔|✔|✔|✔|✔|✔|
|Azure-tegoed saldo voor de facturering profiel volgen|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-subscriptions-for-billing-profile"></a>Abonnementen voor de facturering van profiel beheren

|Taak|Eigenaar van factureringsprofiel|Inzender van factureringsprofiel|Lezer van factureringsprofiel|Factuur-Manager|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount
|---|---|---|---|---|---|---|---|
|Alle Azure-abonnementen voor de facturering profiel weergeven|✔|✔|✔|✔|✔|✔|✔|

## <a name="invoice-section-roles-and-tasks"></a>Rollen en taken voor factuursectie

Een gedeelte van de factuur kunt u de kosten op uw factuur te organiseren. U kunt een sectie voor het ordenen van uw kosten per afdeling, ontwikkelomgeving, maken of op basis van de behoeften van uw organisatie. Machtiging voor het maken van Azure-abonnementen voor de sectie met anderen. Alle kosten voor het gebruik en aankopen voor de abonnementen weergeven in de sectie van de factuur. Zie voor meer informatie, [gedeelte van de factuur begrijpen](billing-mca-overview.md#understand-invoice-sections).

De volgende tabellen ziet u welke rol u taken uitvoeren in de context van de factuur secties moet.

### <a name="manage-invoice-section-permissions-and-properties"></a>Eigenschappen en de factuur sectie Machtigingen beheren

|Taken|Factuursectie voor eigenaar|Factuursectie voor inzender|Factuursectie voor lezer|De maker van de Azure-abonnement|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount | |
|---|---|---|---|---|---|---|---|---|
|Alle machtigingen voor gedeelte factuur weergeven|✔|✔|✔|✔|✔|✔|✔| |
|Machtigingen weergeven en beheren van het gedeelte factuur anderen|✔|✘|✘|✘|✘|✘|✘| |
|Eigenschappen van sectie factuur weergeven|✔|✔|✔|✔|✔|✔|✔| |
|Eigenschappen van de sectie factuur bijwerken|✔|✔|✘|✘|✘|✘|✘|✘|

### <a name="manage-products-for-invoice-section"></a>Beheren van producten voor factuur-sectie

|Taken|Factuursectie voor eigenaar|Factuursectie voor inzender|Factuursectie voor lezer|De maker van de Azure-abonnement|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount
|---|---|---|---|---|---|---|---|
|Alle producten die zijn gekocht in de sectie factuur weergeven|✔|✔|✔|✘|✔|✔|✔|
|Beheren de facturering voor producten, zoals annuleren voor factuur, uitschakelen van automatische verlenging, en meer|✔|✔|✘|✘|✘|✘|✘|
|Factuur-gedeelte voor de producten wijzigen|✔|✔|✘|✘|✘|✘|✘|

### <a name="manage-subscriptions-for-invoice-section"></a>Abonnementen voor factuur sectie beheren

|Taken|Factuursectie voor eigenaar|Factuursectie voor inzender|Factuursectie voor lezer|De maker van de Azure-abonnement|Eigenaar van factureringsaccount|Inzender van factureringsaccount|Lezer van factureringsaccount
|---|---|---|---|---|---|---|---|
|Alle Azure-abonnementen voor gedeelte factuur weergeven|✔|✔|✔|✘|✔|✔|✔|
|Factuur-gedeelte voor de abonnementen wijzigen|✔|✔|✘|✘|✘|✘|✘|
|Eigendom van abonnementen aanvragen van gebruikers in andere facturering-accounts|✔|✔|✘|✘|✘|✘|✘|

## <a name="subscription-billing-roles-and-tasks"></a>Rollen en taken voor abonnementsfacturering

De volgende tabel ziet u welke rol u taken uitvoeren in de context van een abonnement moet.

|Taken|Factuursectie voor eigenaar|Factuursectie voor inzender|Factuursectie voor lezer|De maker van de Azure-abonnement|
|---|---|---|---|---|
|Azure-abonnementen maken|✔|✔|✘|✔|
|Kostenplaats voor het abonnement bijwerken|✔|✔|✘|✘|
|Factuur-gedeelte voor het abonnement wijzigen|✔|✔|✘|✘|

## <a name="manage-billing-roles-in-the-azure-portal"></a>Facturering rollen in Azure portal beheren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van zoeken in Azure portal](./media/billing-understand-mca-roles/billing-search-cost-management-billing.png)

3. Selecteer **toegangsbeheer (IAM)** met een bereik, zoals factureringsaccount, factureringsprofiel of een gedeelte van de factuur, waar u om toegang te geven.

4. De Access control (IAM)-pagina geeft een lijst van gebruikers en groepen die zijn toegewezen aan elke rol voor deze scope.

   ![Schermafbeelding van lijst met beheerders voor factureringsaccount](./media/billing-understand-mca-roles/billing-list-admins.png)

5. Selecteer om toegang te verlenen aan een gebruiker, **toevoegen** vanaf de bovenkant van de pagina. Selecteer een rol in de vervolgkeuzelijst rol. Voer het e-mailadres van de gebruiker aan wie u wilt om toegang te geven. Selecteer **opslaan** aan de rol toe te wijzen.

   ![Schermafbeelding van een beheerder toe te voegen aan een factureringsaccount](./media/billing-understand-mca-roles/billing-add-admin.png)

6. Als u wilt toegang voor een gebruiker verwijderen, selecteert u de gebruiker met de roltoewijzing die u wilt verwijderen. Selecteer verwijderen.

   ![Schermafbeelding van een beheerder verwijderen uit een factureringsaccount](./media/billing-understand-mca-roles/billing-remove-admin.png)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Toegang tot een Microsoft-KLANTOVEREENKOMST controleren
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning
Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over uw factureringsrekening:

- [Aan de slag met uw factureringsrekening voor KLANTOVEREENKOMST van Microsoft](billing-mca-overview.md)
- [Een Azure-abonnement voor uw factureringsrekening voor Microsoft-KLANTOVEREENKOMST maken](billing-mca-create-subscription.md)
