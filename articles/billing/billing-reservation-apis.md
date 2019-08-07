---
title: Api's voor Azure reserverings Automation | Microsoft Docs
description: Meer informatie over de Azure-Api's die u kunt gebruiken om via een programma reserve ring gegevens te verkrijgen.
author: yashesvi
manager: yashesvi
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2019
ms.author: banders
ms.openlocfilehash: 36bc403c4000e58541f22c2cb44f77a28e81cb72
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779891"
---
# <a name="apis-for-azure-reservation-automation"></a>Api's voor automatisering van Azure-reserve ring

Gebruik Azure Api's om via programma code informatie te verkrijgen over uw organisatie over Azure-service of-software-reserve ringen.

## <a name="find-reservation-plans-to-buy"></a>Te kopen reserverings plannen zoeken

Gebruik de API reserverings aanbeveling om aanbevelingen te krijgen over welk reserve ring plan moet worden gekocht op basis van het gebruik van uw organisatie. Zie aanbevelingen voor reserverings [aanvragen](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation)voor meer informatie.

U kunt ook het gebruik van resources analyseren met behulp van het gebruik van de gebruiks gegevens van het verbruiks-API. Zie [gebruiks Details: lijst voor facturerings periode per facturerings account](/rest/api/consumption/usagedetails/list#billingaccountusagedetailslistforbillingperiod)voor meer informatie. De Azure-resources die u consistent gebruikt, zijn doorgaans de beste kandidaat voor een reserve ring.

## <a name="buy-a-reservation"></a>Een reservering kopen

U kunt met behulp van REST Api's Azure-reserve ringen en software plannen aanschaffen via een programma. Zie reserverings [order-Purchase API (](/rest/api/reserved-vm-instances/reservationorder/purchase)Engelstalig) voor meer informatie.

Hier volgt een voor beeld van een aanvraag voor aankoop met behulp van de REST API:

```
PUT https://management.azure.com/providers/Microsoft.Capacity/reservationOrders/<GUID>?api-version=2019-04-01
```

Aanvraagtekst:

```
{
 "sku": {
    "name": "standard_D1"
  },
 "location": "westus",
 "properties": {
    "reservedResourceType": "VirtualMachines",
    "billingScopeId": "/subscriptions/ed3a1871-612d-abcd-a849-c2542a68be83",
    "term": "P1Y",
    "quantity": "1",
    "displayName": "TestReservationOrder",
    "appliedScopes": null,
    "appliedScopeType": "Shared",
    "reservedResourceProperties": {
      "instanceFlexibility": "On"
    }
  }
}
```

U kunt ook een reserve ring kopen in de Azure Portal. Raadpleeg voor meer informatie de volgende artikelen:

Service plannen:
- [Virtuele machine](../virtual-machines/windows/prepay-reserved-vm-instances.md?toc=/azure/billing/TOC.json)
-  [Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md?toc=/azure/billing/TOC.json)
- [SQL Database](../sql-database/sql-database-reserved-capacity.md?toc=/azure/billing/TOC.json)

Software plannen:
- [SUSE Linux-software](../virtual-machines/linux/prepay-suse-software-charges.md?toc=/azure/billing/TOC.json)

## <a name="get-reservations"></a>Reserve ringen ophalen

Als u een Azure-klant bent met een Enterprise Agreement (EA-klant), kunt u de reserve ringen ophalen die uw organisatie heeft gekocht met behulp van de API voor het verkrijgen van een [trans actie voor gereserveerde exemplaar transacties](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges). Voor andere abonnementen krijgt u de lijst met reserve ringen die u hebt gekocht en beschikt u over de benodigde machtigingen voor het weer geven van de lijst met API [-reserverings orders](/rest/api/reserved-vm-instances/reservationorder/list). De eigenaar van het account of de persoon die de reserve ring heeft gekocht, heeft standaard machtigingen voor het weer geven van de reserve ring.

## <a name="see-reservation-usage"></a>Reserverings gebruik weer geven

Als u een EA-klant bent, kunt u programmatisch bekijken hoe de reserve ringen in uw organisatie worden gebruikt. Zie voor meer informatie het [gebruik van gereserveerde instanties voor zakelijke klanten ophalen](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage). Gebruik voor andere abonnementen de API [-reserve ringen samen vattingen: lijst per reserverings order en reserve ring](/rest/api/consumption/reservationssummaries/listbyreservationorderandreservation).

Als u merkt dat de reserve ringen van uw organisatie onder-worden gebruikt:

- Zorg ervoor dat de virtuele machines die uw organisatie maakt, overeenkomen met de VM-grootte die op de reserve ring is.
- Zorg ervoor dat de flexibiliteit van de instantie grootte is ingeschakeld. Zie voor meer informatie [Manage Reservations-instelling Optimize wijzigen voor gereserveerde VM-instanties](billing-manage-reserved-vm-instance.md#change-optimize-setting-for-reserved-vm-instances).
- Wijzig de reik wijdte van de reserve ring naar gedeeld zodat deze breder wordt toegepast. Zie [reserve ringen beheren-het bereik voor een reserve ring wijzigen](billing-manage-reserved-vm-instance.md#change-the-reservation-scope)voor meer informatie.
- Het niet-gebruikte aantal uitwisselen. Zie [reserve ringen beheren](billing-manage-reserved-vm-instance.md)voor meer informatie.

## <a name="give-access-to-reservations"></a>Toegang geven tot reserve ringen

De lijst met alle reserve ringen die een gebruiker heeft geopend, ophalen met behulp van de [API voor reserve ring-bewerking-List](/rest/api/reserved-vm-instances/reservationorder/list). Zie een van de volgende artikelen voor meer informatie over het programmatisch toekennen van een reserve ring:

- [Toegang beheren met RBAC en de REST API](../role-based-access-control/role-assignments-rest.md)
- [Toegang beheren met RBAC en Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)
- [Toegang beheren met RBAC en Azure CLI](../role-based-access-control/role-assignments-cli.md)

## <a name="split-or-merge-reservation"></a>Reserve ring splitsen of samen voegen

Nadat u meer dan één resource-exemplaar binnen een reserve ring hebt gekocht, wilt u mogelijk instanties binnen die reserve ring toewijzen aan verschillende abonnementen. U kunt het reserverings bereik wijzigen zodat dit van toepassing is op alle abonnementen binnen dezelfde facturerings context. Maar voor kosten beheer of budget tering wilt u het bereik mogelijk als ' één abonnement ' laten en reserverings instanties toewijzen aan een specifiek abonnement.

Als u een reserve ring wilt splitsen, gebruikt u de API [-reserve ring-splitsen](/rest/api/reserved-vm-instances/reservation/split). U kunt ook een reserve ring splitsen met behulp van Power shell. Zie reserve [ringen beheren-reserve ring splitsen in twee reserve ringen](billing-manage-reserved-vm-instance.md#split-a-single-reservation-into-two-reservations)voor meer informatie.

Gebruik de API [-reserve ring-samen voegen](/rest/api/reserved-vm-instances/reservation/merge)om twee reserve ringen samen te voegen in één reserve ring.

## <a name="change-scope-for-a-reservation"></a>Bereik wijzigen voor een reserve ring

Het bereik van een reserve ring kan één abonnement, één resource groep of alle abonnementen in uw facturerings context zijn. Als u het bereik instelt op één abonnement of één resource groep, wordt de reserve ring vergeleken met het uitvoeren van resources in het geselecteerde abonnement. Als u het abonnement of de resource groep verwijdert of verplaatst, wordt de reserve ring niet gebruikt.  Als u het bereik instelt op gedeeld, komt Azure overeen met de reserve ring voor resources die worden uitgevoerd in alle abonnementen binnen de facturerings context. De facturerings context is afhankelijk van het abonnement dat u hebt gebruikt om de reserve ring te kopen. U kunt het bereik bij aankoop selecteren of het tijdstip na de aankoop wijzigen. Zie [reserve ringen beheren-het bereik wijzigen](billing-manage-reserved-vm-instance.md#change-the-reservation-scope)voor meer informatie.

Als u de scope programmatisch wilt wijzigen, gebruikt u de API [-reserve ring-update](/rest/api/reserved-vm-instances/reservation/update).

## <a name="learn-more"></a>Meer informatie

- [Wat zijn reserve ringen voor Azure?](billing-save-compute-costs-reservations.md)
- [Begrijpen hoe de korting voor VM-reservering wordt toegepast](billing-understand-vm-reservation-charges.md)
- [Inzicht krijgen in de manier waarop de SUSE Linux Enter prise software plan korting wordt toegepast](billing-understand-suse-reservation-charges.md)
- [Begrijpen hoe andere reserverings kortingen worden toegepast](billing-understand-reservation-charges.md)
- [Het gebruik van de reserve ring begrijpen voor uw abonnement voor betalen naar gebruik](billing-understand-reserved-instance-usage.md)
- [Het gebruik van de reserve ring begrijpen voor uw Enter prise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Windows-software kosten niet inbegrepen bij reserve ringen](billing-reserved-instance-windows-software-costs.md)
- [Azure Reservations in het Partner Center-programma van de Cloud Solution Provider (CSP)](https://docs.microsoft.com/partner-center/azure-reservations)
