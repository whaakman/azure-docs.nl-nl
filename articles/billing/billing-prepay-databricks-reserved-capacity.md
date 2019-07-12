---
title: De kosten van Azure Databricks met een pre-Purchase optimaliseren
description: Meer informatie over hoe u kunt vooraf betaald voor kosten voor Azure Databricks met gereserveerde capaciteit om geld te besparen.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: banders
ms.openlocfilehash: 99eb4de86aa227d558bec54d011a0b1548d27cf0
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811256"
---
# <a name="optimize-azure-databricks-costs-with-a-pre-purchase"></a>De kosten van Azure Databricks met een pre-Purchase optimaliseren

U kunt besparen op uw Azure Databricks per eenheid (DBU) als u pre-aankoopplan van Azure Databricks doorvoeren eenheden (DBCU) voor één of drie jaar. U kunt de vooraf aangeschafte DBCUs gebruiken op elk gewenst moment gedurende de termijn van aankoop. In tegenstelling tot virtuele machines, de vooraf aangeschafte eenheden op uurbasis niet verloopt en u deze gebruiken op elk gewenst moment gedurende de termijn van de aankoop.

Gebruik Azure Databricks trekt automatisch uit de vooraf aangeschafte dbu's. U hoeft te implementeren of een vooraf aangeschafte abonnement toewijzen aan uw Azure Databricks-werkruimten voor het gebruik DBU om op te halen van de kortingen pre-Purchase.

De korting pre-Purchase geldt alleen voor de DBU-gebruik. Andere kosten in rekening gebracht, zoals rekentijd, opslag en netwerken worden afzonderlijk in rekening gebracht.

## <a name="determine-the-right-size-to-buy"></a>Bepaal de juiste grootte te kopen

Databricks pre-Purchase is van toepassing op alle lagen en Databricks-werkbelastingen. U kunt zien van de pre-Purchase als een groep van de vooraf betaalde Databricks commit-eenheden. Gebruik wordt afgetrokken van de groep, ongeacht de werkbelasting of laag. Gebruik, wordt in mindering gebracht in de volgende verhouding:

| **Workload** | **DBU-toepassing verhouding - Standard-laag** | **DBU-toepassing-verhouding, Premium-laag** |
| --- | --- | --- |
| Gegevensanalyse | 0.4 | 0.55 |
| Data Engineering | 0.15 | 0,30 |
| Data Engineering Light | 0.07 | 0.22 |

Bijvoorbeeld, wanneer een aantal van de Data Analytics-Standard-laag wordt verbruikt, de Databricks-eenheden voor vooraf aangeschafte doorvoeren wordt in mindering gebracht door 0,4 eenheden.

Voordat u aanschaft, berekenen van de totale DBU hoeveelheid voor verschillende werkbelastingen en lagen gebruikt. Gebruik de voorgaande ratio's normaliseren naar DBCU en voer vervolgens een projectie van het totale gebruik van vervolgens één of drie jaar.

## <a name="purchase-databricks-commit-units"></a>Databricks doorvoeren eenheden kopen

U kunt de Databricks-abonnementen kopen de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22Databricks%22%7D). U moet de rol van eigenaar voor ten minste één enterprise-abonnement hebben voor het kopen van gereserveerde capaciteit.

- Op dit moment pre-aankoopplan is alleen beschikbaar voor Enterprise Agreement-klanten.
- U moet zich in een eigenaarsrol voor ten minste één Enterprise-abonnement.
- Voor Enterprise-abonnementen, **gereserveerde instanties toevoegen** moet zijn ingeschakeld in de [EA-portal](https://ea.azure.com/). Of, als deze instelling is uitgeschakeld, moet u een EA-beheerder van het abonnement zijn.

**Om aan te schaffen:**

1. Ga naar de [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22Databricks%22%7D).
1. Selecteer een abonnement. Gebruik de **abonnement** lijst om het abonnement dat wordt gebruikt om te betalen voor de gereserveerde capaciteit te selecteren. De betalingswijze van het abonnement wordt in rekening gebracht de kosten vooraf voor de gereserveerde capaciteit. Kosten worden in mindering gebracht op de monetaire toezeggingsbedrag van de inschrijving of kosten in rekening gebracht als overschrijding.
1. Selecteer een bereik. Gebruik de **bereik** om te selecteren van een bereik van abonnement:
    - **Enkele resource groepsbereik** : de reserveringskorting is van toepassing op de overeenkomende resources in alleen de geselecteerde resourcegroep.
    - **Eén abonnementsbereik** : de reserveringskorting is van toepassing op de overeenkomende resources in het geselecteerde abonnement.
    - **Gedeeld bereik** : de reserveringskorting is van toepassing op het zoeken naar resources in die in aanmerking komen abonnementen die zich in de context van de facturering. Voor Enterprise Agreement-klanten is de context van de facturering van de inschrijving.
1. Hoeveel Azure Databricks commit-eenheden u wilt kopen en voltooi de aankoop selecteren.


![Voorbeeld van Azure Databricks-aankoop in de Azure portal](./media/billing-prepay-databricks-reserved-capacity/data-bricks-pre-purchase.png)

## <a name="change-scope-and-ownership"></a>Bereik wijzigen en het eigendom

U kunt de volgende typen wijzigingen aanbrengen in een reservering na de aankoop:

- Reserveringsbereik bijwerken
- Toegang op basis van rollen

U kunt geen splitsen of samenvoegen van de pre-Purchase van Databricks commit-eenheid. Zie voor meer informatie over het beheren van reserveringen [reserveringen beheren na de aankoop](billing-manage-reserved-vm-instance.md).

## <a name="cancellations-and-exchanges"></a>Annuleringen en uitwisselingen

Annuleren en exchange wordt niet ondersteund voor Databricks pre-Purchase plannen. Alle aankopen in de zijn laatste.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:
  - [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
  - [Begrijpen hoe een Azure Databricks pre-Purchase DBCU korting wordt toegepast](billing-reservation-discount-databricks.md)
  - [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
