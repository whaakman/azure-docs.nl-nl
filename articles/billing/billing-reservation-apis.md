---
title: API's voor het automatiseren van Azure-reservering | Microsoft Docs
description: Meer informatie over de Azure API's die u gebruiken kunt via een programma om reserveringsinformatie te verkrijgen.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/10/2018
ms.author: banders
ms.openlocfilehash: a71c456f017e80da85f27437738aa963ec967a3c
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904153"
---
# <a name="apis-for-azure-reservation-automation"></a>API's voor het automatiseren van Azure-reservering

Gebruik Azure API's via een programma om informatie te verkrijgen voor uw organisatie over Azure-service of software-reserveringen.

## <a name="find-reservation-plans-to-buy"></a>Vinden van de reservering wil kopen

Gebruik de reservering aanbeveling API aanbevelingen waarop reserveringen wilt kopen ophalen op basis van gebruik van uw organisatie. Zie voor meer informatie, [reservering aanbevelingen krijgen](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation).

U kunt ook uw Resourcegebruik analyseren met behulp van de Gebruiksdetails van het verbruik-API. Zie voor meer informatie, [gebruiksgegevens - lijst voor facturering periode door factureringsaccount](/rest/api/consumption/usagedetails/listforbillingperiodbybillingaccount). De Azure-resources die u consistent gebruikt zijn meestal de beste kandidaat voor een reservering.

## <a name="buy-a-reservation"></a>Een reservering kopen

U kan niet via een programma op dat moment een reservering kopen. Als u wilt een reservering kopen, Zie de volgende artikelen:

Service-plannen:
- [Virtuele machine](../virtual-machines/windows/prepay-reserved-vm-instances.md?toc=/azure/billing/TOC.json)
-  [Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md?toc=/azure/billing/TOC.json)
- [SQL Database](../sql-database/sql-database-reserved-capacity.md?toc=/azure/billing/TOC.json)

Software-abonnement:
- [SUSE Linux-software](../virtual-machines/linux/prepay-suse-software-charges.md?toc=/azure/billing/TOC.json)

## <a name="get-reservations"></a>Reserveringen ophalen

Als u een Azure-klant bent met een Enterprise Agreement (EA-klant), krijgt u de reserveringen uw organisatie hebt gekocht via het [gereserveerde instantie ophalen transactie in rekening gebracht API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges). Voor andere abonnementen, lijst van alle reserveringen die u hebt gekocht en over machtigingen beschikt om weer te geven met behulp van de API [Reserveringsorder - lijst](/rest/api/reserved-vm-instances/reservationorder/list). De eigenaar van account of de persoon die de reservering hebt gekocht heeft standaard machtigingen voor het weergeven van de reservering.

## <a name="see-reservation-usage"></a>Zie reservering gebruik

Als u een EA-klant bent, kunt u programmatisch bekijken hoe de reserveringen in uw organisatie worden gebruikt. Zie voor meer informatie, [gebruik van de gereserveerde instanties ophalen voor zakelijke klanten](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage). Gebruik de API voor andere abonnementen, [reserveringen samenvattingen - lijst door reservering volgorde en reservering](/rest/api/consumption/reservationssummaries/listbyreservationorderandreservation).

Als u vindt dat van uw organisatie reserveringen onder gebruikt worden:

- Zorg ervoor dat de virtuele machines die uw organisatie maakt overeenkomen met de VM-grootte die in de reservering.
- Controleer of grootte-instantieflexibiliteit op. Zie voor meer informatie, [reserveringen beheren - wijziging optimaliseren voor gereserveerde VM-instanties](billing-manage-reserved-vm-instance.md#change-optimize-setting-for-reserved-vm-instances).
- Het bereik van de reservering gedeeld zodat deze van toepassing is grotere schaal wijzigen. Zie voor meer informatie, [reserveringen beheren - wijzigen van het bereik voor een reservering](billing-manage-reserved-vm-instance.md#change-the-scope-for-a-reservation).
- De niet-gebruikte hoeveelheid uitwisselen. Zie voor meer informatie, [reserveringen - annuleringen en uitwisselingen beheren](billing-manage-reserved-vm-instance.md#cancellations-and-exchanges).

## <a name="give-access-to-reservations"></a>Toegang verlenen tot reserveringen

De lijst met alle reserveringen ophalen dat een gebruiker toegang tot met behulp van heeft de [reservering - bewerking - lijst API](/rest/api/reserved-vm-instances/reservationorder/list). Om toegang te verlenen aan een reservering via een programma, ziet u een van de volgende artikelen:

- [Beheren van toegang met RBAC en de REST-API](../role-based-access-control/role-assignments-rest.md)
- [Toegang met RBAC en Azure PowerShell beheren](../role-based-access-control/role-assignments-powershell.md)
- [Beheren van toegang via RBAC en Azure CLI](../role-based-access-control/role-assignments-cli.md)

## <a name="split-or-merge-reservation"></a>Reservering van splitsen of samenvoegen

Nadat u meer dan één exemplaar van de resource binnen een reservering koopt, kunt u exemplaren binnen deze reservering toewijzen aan verschillende abonnementen. U kunt het reserveringsbereik wijzigen zodat deze van toepassing op alle abonnementen binnen de context van de dezelfde facturering. Maar voor cost management of budgettering doeleinden, kunt u de scope als 'enkel abonnement' behouden en exemplaren van de reservering toewijzen aan een specifiek abonnement. 

Als u wilt splitsen een reservering, gebruikt u de API [reservering - gesplitst](/rest/api/reserved-vm-instances/reservation/split). U kunt ook een reservering splitsen met behulp van PowerShell. Zie voor meer informatie, [reserveringen - reservering splitsen in twee reserveringen beheren](billing-manage-reserved-vm-instance.md#split-a-single-reservation-into-two-reservations).

Als u wilt samenvoegen twee reserveringen in één reservering, gebruiken de API [reservering - Merge](/rest/api/reserved-vm-instances/reservation/merge).

## <a name="change-scope-for-a-reservation"></a>Bereik wijzigen voor een reservering

Het bereik van een reservering kan worden voor één abonnement of voor alle abonnementen in de context van de facturering. Als u het bereik aan één abonnement hebt ingesteld, wordt de reservering wordt vergeleken met het uitvoeren van resources in het geselecteerde abonnement. Als u het bereik op gedeelde, Azure komt overeen met de reservering naar resources die worden uitgevoerd in alle abonnementen binnen de context van de facturering instellen. De context van de facturering is afhankelijk van het abonnement dat u hebt gebruikt om de reservering kopen. Zie voor meer informatie, [-reserveringen beheren - het bereik wijzigen](billing-manage-reserved-vm-instance.md#change-the-scope-for-a-reservation).

U kunt het bereik via een programma wijzigen met de API [reservering - Update](/rest/api/reserved-vm-instances/reservation/update).

## <a name="learn-more"></a>Meer informatie

- [Wat zijn de reserveringen voor Azure](billing-save-compute-costs-reservations.md)
- [Begrijpen hoe de VM-reserveringskorting wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Begrijpen hoe de korting voor SUSE Linux Enterprise software plan wordt toegepast](billing-understand-suse-reservation-charges.md)
- [Begrijpen hoe andere reservering kortingen zijn toegepast](billing-understand-reservation-charges.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Kosten van de Windows-software is niet opgenomen in de reserveringen](billing-reserved-instance-windows-software-costs.md)
- [Azure-reserveringen in programma Partner Center Cloud Solution Provider (CSP)](https://docs.microsoft.com/partner-center/azure-reservations)