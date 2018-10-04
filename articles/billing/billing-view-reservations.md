---
title: Azure reserveringen weergeven | Microsoft Docs
description: Leer hoe u Azure-reserveringen weergeven in Azure portal.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2018
ms.author: cwatson
ms.openlocfilehash: 2ae30ca55f3ca03a64438025960ddd807e288216
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48272639"
---
# <a name="view-azure-reservations-in-the-azure-portal"></a>Azure-reserveringen weergeven in Azure portal

Afhankelijk van uw abonnementstype en machtigingen zijn er een aantal manieren om Azure-reserveringen weer te geven.

## <a name="view-reservations-as-owner-or-reader"></a>Reserveringen weergeven als de eigenaar of lezer

Standaard, wanneer u een reservering koopt vindt u en de accountbeheerder de reservering. U en de accountbeheerder krijgt automatisch de rol van eigenaar van de reservering. Als u wilt dat anderen om de reservering weer te geven, moet u ze als toevoegen een **eigenaar** of **lezer** op de reservering. Zie voor meer informatie, [toevoegen of wijzigen van de gebruikers die een reservering kunnen beheren](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
 
Om een reservering als een eigenaar of lezer, weer te geven

1. Meld u aan bij [Azure Portal]( http://portal.azure.com).
1. Zoeken op **reserveringen**.

    ![Schermafbeelding van zoeken in Azure portal](./media/billing-view-reservation/portal-reservation-search.png)

1. U ziet een lijst van de reserveringen waar u de rol eigenaar of lezer hebt.

Als u wilt wijzigen van het bereik van een reservering, wijzigen die een reservering kan beheren of gesplitste een reservering Zie [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md).

## <a name="view-reservation-transactions-for-enterprise-enrollments"></a>Reservering-transacties voor Enterprise-inschrijvingen weergeven

 Als u een partner onder leiding van Enterprise-inschrijving hebt, reserveringen weergeven door te gaan naar **rapporten** in de EA-portal. Voor andere Enterprise-inschrijvingen, kunt u reserveringen weergeven in de EA-portal en in de Azure-portal. U moet een EA-beheerder om reservering transacties weer te geven.

Transacties van de reservering bekijken in Azure-portal

1. Meld u aan bij [Azure Portal]( http://portal.azure.com).
1. Zoeken op **kosten Management en facturering**.

    ![Schermafbeelding van zoeken in Azure portal](./media/billing-view-reservation/portal-cm-billing-search.png)

1. Selecteer **reservering transacties**.
1. Als u wilt de resultaten te filteren, selecteert u **Timespan**, **Type**, of **beschrijving**.
1. Selecteer **Toepassen**.

    ![Schermafbeelding van reservering transacties resultaten](./media/billing-view-reservation/portal-billing-reservation-transaction-results.png)

Als u de gegevens met behulp van een API, Zie [gereserveerde instantie ophalen transactiekosten in rekening gebracht voor zakelijke klanten](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
- [Betaal vooruit voor Cosmos DB gereserveerde capaciteit](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Informatie over het gebruik van de reservering voor CSP-abonnementen](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog meer vragen hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
