---
title: Gebruiks-API gerelateerde Veelgestelde vragen | Microsoft Docs
description: Lijst van Stack Azure meters vergelijking van Azure usage API, gebruikstijd en gerapporteerd tijd, foutcodes.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: alfredop
ms.openlocfilehash: 9ec0b0456a0e7bc5f86f33981d4be6289f5fbc71
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Veelgestelde vragen in Azure Stack gebruiks-API
Dit artikel worden enkele veelgestelde vragen over de Azure-API voor het gebruik van Stack.

## <a name="what-meter-ids-can-i-see"></a>Welke meter id's kan ik zien?
Gebruik wordt voor de volgende resourceproviders gerapporteerd.

| **Resourceprovider** | **ID van de meter** | **De naam van de meter** | **Eenheid** | **Aanvullende informatie** |
| --- | --- | --- | --- | --- |
| **Netwerk** |F271A8A388C44D93956A063E1D2FA80B |Gebruik van statische IP-adres |IP-adressen| Telling van IP-adressen die worden gebruikt |
| |9E2739BA86744796B465F64674B822BA |Gebruik van dynamische IP-adres |IP-adressen| Telling van IP-adressen die worden gebruikt |
| **Storage** |B4438D5D-453B-4EE1-B42A-DC72E377F1E4 |TableCapacity |GB\*uur |Totale capaciteit verbruikt door tabellen |
| |B5C15376-6C94-4FDD-B655-1A69D138ACA3 |PageBlobCapacity |GB\*uur |Totale capaciteit van pagina-blobs gebruikt |
| |B03C6AE7-B080-4BFA-84A3-22C800F315C6 |QueueCapacity |GB\*uur |Totale capaciteit gebruikt door de wachtrij |
| |09F8879E-87E9-4305-A572-4B7BE209F857 |BlockBlobCapacity |GB\*uur |Totale capaciteit verbruikt door blok-blobs |
| |B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 |TableTransactions |Aantal verzoeken in 10 000's |Aanvragen van de tabel-service (in 10 000's) |
| |50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D |TableDataTransIn |Inkomende gegevens in GB |Tabel-service gegevens inkomend in GB |
| |1B8C1DEC-EE42-414B-AA36-6229CF199370 |TableDataTransOut |Uitgaande in GB |Uitgaande van een tabel-service-gegevens, in GB |
| |43DAF82B-4618-444A-B994-40C23F7CD438 |BlobTransactions |Aanvragen tellen in 10, 000's |Serviceaanvragen BLOB (per 10 000's) |
| |9764F92C-E44A-498E-8DC1-AAD66587A810 |BlobDataTransIn |Inkomende gegevens in GB |BLOB-service gegevens inkomend in GB |
| |3023FEF4-ECA5-4D7B-87B3-CFBC061931E8 |BlobDataTransOut |Uitgaande in GB |Uitgaande van BLOB-service gegevens in GB |
| |EB43DD12-1AA6-4C4B-872C-FAF15A6785EA |QueueTransactions |Aanvragen tellen in 10, 000's |Wachtrij-serviceaanvragen (per 10 000's) |
| |E518E809-E369-4A45-9274-2017B29FFF25 |QueueDataTransIn |Inkomende gegevens in GB |Wachtrij-service gegevens inkomend in GB |
| |DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2 |QueueDataTransOut |Uitgaande in GB |Uitgaande van Queue-service gegevens in GB |
| **Compute** |FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5 |Basis VM-grootte-uren |Virtuele core minuten | Aantal virtuele kernen time-out minuten die de virtuele machine is uitgevoerd |
| |9CD92D4C-BAFD-4492-B278-BEDC2DE8232A |Uren Windows VM-grootte |Virtuele core minuten | Aantal virtuele kernen time-out minuten die de virtuele machine is uitgevoerd |
| |6DAB500F-A4FD-49C4-956D-229BB9C8C793 |Uren voor VM-grootte |VM-uren |Basis- en Windows VM bevat. Niet aangepast voor kernen |
| **Key Vault** |EBF13B9F-B3EA-46FE-BF54-396E93D48AB4 |Sleutelkluis-transacties | Aantal verzoeken in 10 000's| Aantal ontvangen door de Sleutelkluis gegevens vlak REST-API-aanvragen |
| **App service** |190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  | App Service   | Virtuele core-uren  | Aantal virtuele kernen gebruikt voor het uitvoeren van app service |

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Hoe kan het Azure-Stack gebruik API's met vergelijken de [Azure gebruiks-API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (momenteel in de openbare preview)?
* De Tenant-API voor informatie over het gebruik is consistent met de Azure-API, met één uitzondering: de *showDetails* vlag momenteel niet ondersteund in Azure-Stack.
* De Provider gebruik API geldt alleen voor Azure-Stack.
* Op dit moment wordt de [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) is beschikbaar in Azure is niet beschikbaar in Azure-Stack.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Wat is het verschil tussen de gebruikstijd van de informatie over het en gerapporteerde tijd?
Rapporten over gebruik van gegevens hebben twee belangrijkste tijdwaarden:

* **Keer gerapporteerd**. De tijd waarop de gebeurtenis gebruik van het systeem gebruik ingevoerd
* **Gebruikstijd**. De tijd wanneer de bron van de Azure-Stack is verbruikt

U ziet een discrepantie waarden in voor het gebruik van tijd en gerapporteerd voor een specifieke gebruiksgebeurtenis worden gerapporteerd. De vertraging kan meerdere uren in elke omgeving zo lang zijn.

Op dit moment kunt u kunt een query alleen door *gerapporteerd tijd*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Wat deze gebruik API-foutcodes?
| **HTTP-statuscode** | **Foutcode** | **Beschrijving** |
| --- | --- | --- |
| 400/onjuiste aanvraag |*NoApiVersion* |De *api-versie* query-parameter ontbreekt. |
| 400/onjuiste aanvraag |*InvalidProperty* |Een eigenschap ontbreekt of heeft een ongeldige waarde. Het bericht in de foutcode in de antwoordtekst identificeert de eigenschap ontbreekt. |
| 400/onjuiste aanvraag |*RequestEndTimeIsInFuture* |De waarde voor *ReportedEndTime* in de toekomst ligt. Waarden worden in de toekomst niet toegestaan voor dit argument. |
| 400/onjuiste aanvraag |*SubscriberIdIsNotDirectTenant* |Een serviceprovider-API-aanroep heeft een abonnements-ID is niet een geldige tenant van de aanroepfunctie gebruikt. |
| 400/onjuiste aanvraag |*SubscriptionIdMissingInRequest* |De abonnements-ID van de aanroepfunctie ontbreekt. |
| 400/onjuiste aanvraag |*InvalidAggregationGranularity* |Er is een ongeldige aggregatie granulatie aangevraagd. Geldige waarden zijn dagelijkse en per uur. |
| 503 |*ServiceUnavailable* |Er is een herstelbare fout opgetreden omdat de service bezet is of de oproep wordt beperkt. |

## <a name="next-steps"></a>Volgende stappen
[Klant facturering en terugstorting in Azure-Stack](azure-stack-billing-and-chargeback.md)

[Provider Resourcegebruik API](azure-stack-provider-resource-api.md)

[Tenant-Resourcegebruik API](azure-stack-tenant-resource-usage-api.md)
