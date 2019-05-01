---
title: Kosten voor AWS en gebruik in Azure Cost Management beheren
description: Dit artikel helpt u begrijpen hoe u kostenanalyse en budgetten in Cost Management gebruiken voor het beheren van uw kosten voor AWS en het gebruik.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: 007b6c409dde248a4dde7a15fd16b543add234bc
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870310"
---
# <a name="manage-aws-costs-and-usage-in-azure"></a>Kosten voor AWS en gebruik in Azure beheren

Nadat u hebt ingesteld en geconfigureerd AWS kosten en gebruik rapport-integratie voor Azure Cost Management, bent u klaar om te beginnen met het beheren van uw kosten voor AWS en het gebruik. Dit artikel helpt u begrijpen hoe u kostenanalyse en budgetten in Cost Management gebruiken voor het beheren van uw kosten voor AWS en het gebruik.

Als u al de integratie nog niet hebt geconfigureerd, Zie [ingesteld omhoog en gebruik van AWS-rapport-integratie configureren](aws-integration-set-up-configure.md).

_Voordat u begint met_: Als u niet bekend met kostenanalyse bent, raadpleegt u de [verkennen en analyseren van kosten met Cost analysis](quick-acm-cost-analysis.md) Quick Start. En als u niet bekend met budgetten in Azure bent, raadpleegt u de [maken en beheren van Azure budgetten](tutorial-acm-create-budgets.md) zelfstudie.

## <a name="view-aws-costs-in-cost-analysis"></a>AWS-kosten bekijken in kostenanalyse

Kosten voor AWS zijn beschikbaar in Cost Analysis in de volgende bereiken:

- AWS gekoppelde accounts voor een beheergroep
- Kosten voor AWS gekoppeld account
- Kosten voor AWS geconsolideerd-account

De volgende secties wordt beschreven hoe u de bereiken gebruiken zodat u kosten en gebruik gegevens voor elk criterium zien.

### <a name="view-aws-linked-accounts-under-a-management-group"></a>Weergave AWS gekoppelde accounts voor een beheergroep

Kijken naar de kosten met behulp van het beheerbereik van de groep is de enige manier om te zien van de samengevoegde kosten die afkomstig zijn uit verschillende abonnementen en accounts gekoppeld. Met behulp van een beheergroep, biedt een cross-cloud-weergave.

Open de bereikskiezer voor in kostenanalyse, en selecteert u de beheergroep waarin uw gekoppelde AWS-accounts. Hier volgt een voorbeeld van de installatiekopie in Azure portal:

![Voorbeeld van de weergave selecteren bereik](./media/aws-integration-manage/select-scope01.png)



Hier volgt een voorbeeld van de kosten van de groep management in kostenanalyse, gegroepeerd per Provider (Azure en AWS).

![Voorbeeld van Azure en AWS kosten van een kwartaal in kostenanalyse](./media/aws-integration-manage/cost-analysis-aws-azure.png)

### <a name="view-aws-linked-account-costs"></a>Kosten voor AWS gekoppeld account weergeven

Kosten van AWS-account weergeven, opent u de bereikskiezer voor en selecteer de AWS-account gekoppeld. Houd er rekening mee dat de gekoppelde accounts gekoppeld aan een beheergroep zijn, zoals gedefinieerd in de AWS-connector.

Hier volgt een voorbeeld dat toont een AWS-accountbereik gekoppeld.

![Voorbeeld van de weergave selecteren bereik](./media/aws-integration-manage/select-scope02.png)



### <a name="view-aws-consolidated-account-costs"></a>Weergave AWS geconsolideerde kosten van account

Als u wilt weergeven van AWS geconsolideerd geconsolideerde account kosten, opent u de bereikskiezer voor en selecteert de AWS account. Hier volgt een voorbeeld dat toont een AWS-accountbereik geconsolideerd.

![Voorbeeld van de weergave selecteren bereik](./media/aws-integration-manage/select-scope03.png)



Dit bereik bevat dat een samengevoegde weergave van alle AWS gekoppelde accounts die zijn gekoppeld aan de geconsolideerde AWS-account. Hier volgt een voorbeeld van de kosten voor een AWS-account, gegroepeerd op servicenaam geconsolideerd.

![Voorbeeld van AWS geconsolideerde kosten in kostenanalyse](./media/aws-integration-manage/cost-analysis-aws-consolidated.png)

### <a name="dimensions-available-for-filtering-and-grouping"></a>Dimensies beschikbaar voor het filteren en groeperen

De volgende tabel beschrijft de beschikbare groep te filteren op in kostenanalyse dimensies.

| Dimensie | HUIDIGE Amazon-header | Bereiken | Opmerkingen |
| --- | --- | --- | --- |
| Beschikbaarheidszone | lineitem/AvailabilityZone | Alle |   |
| Locatie | product/regio | Alle |   |
| Naar gebruik |   | Alle |   |
| Metercategorie | lineItem/ProductCode | Alle |   |
| Subcategorie van de meter | lineitem/UsageType | Alle |   |
| Bewerking | lineItem/Operation | Alle |   |
| Resource | lineItem/ResourceId | Alle |   |
| Resourcetype | product/instanceType | Alle | Als het product/instanceType null is, wordt lineItem/UsageType gebruikt. |
| ResourceGuid | N/A | Alle | Azure-meterkenmerken GUID. |
| Servicenaam | product/ProductName | Alle | Als het product/ProductName null is, wordt lineItem/ProductCode gebruikt. |
| Servicelaag |   |   |   |
| Abonnements-id | lineItem/UsageAccountId | Geconsolideerde-account en beheer van groep |   |
| Abonnementsnaam | N/A | Geconsolideerde-account en beheer van groep | Namen worden verzameld met behulp van de organisatie-API van AWS. |
| Label | resourceTags/\* | Alle | De _gebruiker:_ voorvoegsel wordt verwijderd uit de gebruiker gedefinieerde tags om toe te staan van cross-cloud-tags. De _aws:_ voorvoegsel blijft intact. |
| Factureringsaccount-id | bill/PayerAccountId | Beheergroep |   |
| Naam van het factureringsaccount | N/A | Beheergroep | Namen worden verzameld met behulp van de organisatie-API van AWS. |
| Provider | N/A | Beheergroep | AWS of Azure. |

## <a name="set-budgets-on-aws-scopes"></a>Set-budgetten voor AWS-scopes

Gebruik van budgetten voor het proactief beheren van kosten en station gebruik in uw organisatie. Budgetten worden ingesteld op de geconsolideerde AWS-account en AWS gekoppeld accountbereiken. Hier volgt een voorbeeld van budgetten voor een geconsolideerd AWS-account wordt weergegeven in Cost Management:

![Voorbeeld van budgetten voor een AWS geconsolideerd account](./media/aws-integration-manage/budgets-aws-consolidated-account01.png)

## <a name="aws-data-collection-process"></a>Het proces voor gegevensverzameling (AWS)

Na het instellen van de AWS-connector, het verzamelen van gegevens en discovery-processen te starten. Het duurt enkele uren alle gebruiksgegevens te verzamelen. Afhankelijk van de duur:

- De tijd die nodig is om de huidige bestanden die zich in de AWS S3-bucket te verwerken.
- De tijd die nodig zijn voor het maken van de geconsolideerde van AWS-account en gekoppelde AWS-accountbereiken.
- De tijd en de frequentie van AWS worden de kosten en gebruiksrapport bestanden schrijven in de S3-bucket

## <a name="aws-integration-pricing"></a>Prijzen van AWS-integratie

Elke connector AWS Hiermee haalt u 90 dagen voor de gratis proefversie. Er zijn geen kosten tijdens de openbare Preview.

De prijs is 1% van uw maandelijkse kosten voor AWS. Elke maand in rekening gebracht op basis van uw gefactureerde kosten van de vorige maand.

Toegang tot de AWS-API's, mogelijk extra kosten in rekening.

## <a name="aws-integration-limitations"></a>Beperkingen voor AWS-integratie

- Cost Management biedt geen ondersteuning voor Kostenrapporten die meerdere valutatypen bevatten. Een foutbericht wordt weergegeven als u een bereik met meerdere valuta's.
- Cloud-connectors bieden geen ondersteuning AWS GovCloud (VS), AWS (overheid) of AWS China.
- Cost Management ziet u AWS _gebruikskosten_ alleen. BTW, ondersteuning, restituties, gereserveerde instanties, tegoed of andere kosten typen nog worden niet ondersteund.

## <a name="troubleshooting-aws-integration"></a>Problemen met AWS-integratie oplossen

Gebruik de volgende informatie voor het oplossen van problemen met veelvoorkomende problemen op te lossen.

### <a name="no-permission-to-aws-linked-accounts"></a>Geen machtiging voor de gekoppelde AWS-accounts

Er zijn twee manieren machtigingen voor toegang tot kosten voor AWS gekoppelde accounts op te halen:

- Krijg toegang tot de beheergroep die de gekoppelde AWS-accounts heeft.
- Iemand machtigen voor het gekoppelde AWS-account hebben.

De maker van het AWS-connector is standaard de eigenaar van de objecten die de connector hebt gemaakt. Met inbegrip van de AWS geconsolideerde-account en de AWS-account gekoppeld.

### <a name="collection-failed-with-assumerole"></a>Verzameling is mislukt met de AssumeRole

Deze fout betekent dat Cost Management kan de AWS AssumeRole API aan te roepen. Dit probleem kan optreden vanwege een probleem met de roldefinitie van de. Controleer of dat de volgende voorwaarden voldaan wordt:

- De externe ID is hetzelfde als de waarde in de roldefinitie van de en de definitie van de connector.
- Het Roltype is ingesteld op **AWS op een andere account behoort u of 3e partij.**
- De **MFA vereisen** keuze is uitgeschakeld.
- De vertrouwde AWS-account in de AWS-rol is _432263259397_.

### <a name="collection-failed-with-access-denied"></a>Verzameling is mislukt met de toegang geweigerd

Dit bericht betekent dat Cost Management kan geen toegang tot de huidige bestanden die zijn opgeslagen in de Amazon S3-bucket. Zorg ervoor dat de AWS JSON-beleid dat is gekoppeld aan de rol lijkt op het voorbeeld wordt weergegeven aan de onderkant van de [maken van een rol en het beleid in AWS](aws-integration-set-up-configure.md#create-a-role-and-policy-in-aws) sectie.

### <a name="connector-error-with-failedtofindreport"></a>Connector-fout opgetreden bij het FailedToFindReport

Deze fout betekent dat de kosten en gebruik rapport dat is gedefinieerd in de connector kan niet vinden Cost Management. Zorg ervoor dat deze wordt niet verwijderd en dat de AWS JSON-beleid dat is gekoppeld aan de rol lijkt op het voorbeeld wordt weergegeven aan de onderkant van de [maken van een rol en het beleid in AWS](aws-integration-set-up-configure.md#create-a-role-and-policy-in-aws) sectie.

## <a name="next-steps"></a>Volgende stappen

- Als u dit nog niet hebt al uw Azure-omgeving is geconfigureerd met beheergroepen, [voor de eerste installatie van beheergroepen](../governance/management-groups/index.md#initial-setup-of-management-groups).
