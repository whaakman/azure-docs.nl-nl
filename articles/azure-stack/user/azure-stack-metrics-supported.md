---
title: Ondersteunde metrische gegevens met Azure Monitor in Azure Stack | Microsoft Docs
description: Meer informatie over de ondersteunde metrische gegevens voor Azure Monitor in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.lastreviewed: 12/06/2018
ms.openlocfilehash: cccc646026457648fc477ac4a858cae7504dcc2a
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57759352"
---
# <a name="supported-metrics-with-azure-monitor-on-azure-stack"></a>Ondersteunde metrische gegevens met Azure Monitor in Azure Stack

*Van toepassing op: Azure Stack-geïntegreerde systemen*

U kunt uw metrische gegevens ophalen uit Azure monitor in Azure Stack in hetzelfde als de globale Azure. U kunt uw metingen maken in de portal, de REST-API ophalen of query's voor uitvoert met PowerShell of CLI.

De volgende tabellen worden de metrische gegevens beschikbaar met metrische gegevens van Azure Monitor-pijplijn in Azure Stack. Als u wilt opvragen en toegang tot deze metrische gegevens, moet u gebruiken de **2018-01-01** api-versie van de API-profiel. Zie voor meer informatie over API-profielen en Azure Stack [beheren API-versieprofielen in Azure Stack](azure-stack-version-profiles.md).

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

| Gegevens | De naam van de metrische gegevens weergeven | Eenheid | Aggregatietype | Description | Dimensies |
|----------------|---------------------|---------|------------------|-----------------------------------------------------------------------------------------------|---------------|
| Percentage CPU | Percentage CPU | Procent | Gemiddeld | Het percentage toegewezen berekeningseenheden dat momenteel door de virtuele machine(s) wordt gebruikt | Er zijn geen dimensies |

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

| Gegevens | De naam van de metrische gegevens weergeven | Eenheid | Aggregatietype | Description | Dimensies |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| UsedCapacity | Gebruikte capaciteit | Bytes | Gemiddeld | Gebruikte capaciteit van account | Er zijn geen dimensies |
| Transacties | Transacties | Count | Totaal | Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie. | De ResponseType, GeoType, ApiName |
| Inkomend verkeer | Inkomend verkeer | Bytes | Totaal | De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure. | GeoType, ApiName |
| Uitgaand verkeer | Uitgaand verkeer | Bytes | Totaal | De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer. | GeoType, ApiName |
| SuccessServerLatency | Geslaagde serverlatentie | Milliseconden | Gemiddeld | De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde omvat niet de netwerklatentie die in AverageE2ELatency is opgegeven. | GeoType, ApiName |
| SuccessE2ELatency | Geslaagde E2E-latentie | Milliseconden | Gemiddeld | De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen. | GeoType, ApiName |
| Beschikbaarheid | Beschikbaarheid | Procent | Gemiddeld | Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot verminderde beschikbaarheid voor de opslagservice of de opgegeven API-bewerking. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

| Gegevens | De naam van de metrische gegevens weergeven | Eenheid | Aggregatietype | Description | Dimensies |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| BlobCapacity | Blobcapaciteit | Bytes | Totaal | De hoeveelheid opslag die wordt gebruikt door de Blob-service van het opslagaccount (in bytes). | BlobType |
| BlobCount | Aantal blobs | Count | Totaal | Het aantal blobs in de Blob-service van het opslagaccount. | BlobType |
| ContainerCount | Aantal blobcontainers | Count | Gemiddeld | Het aantal containers in de Blob-service van het opslagaccount. | Er zijn geen dimensies |
| Transacties | Transacties | Count | Totaal | Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie. | De ResponseType, GeoType, ApiName |
| Inkomend verkeer | Inkomend verkeer | Bytes | Totaal | De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure. | GeoType, ApiName |
| Uitgaand verkeer | Uitgaand verkeer | Bytes | Totaal | De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer. | GeoType, ApiName |
| SuccessServerLatency | Geslaagde serverlatentie | Milliseconden | Gemiddeld | De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde omvat niet de netwerklatentie die in AverageE2ELatency is opgegeven. | GeoType, ApiName |
| SuccessE2ELatency | Geslaagde E2E-latentie | Milliseconden | Gemiddeld | De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen. | GeoType, ApiName |
| Beschikbaarheid | Beschikbaarheid | Procent | Gemiddeld | Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot verminderde beschikbaarheid voor de opslagservice of de opgegeven API-bewerking. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

| Gegevens | De naam van de metrische gegevens weergeven | Eenheid | Aggregatietype | Description | Dimensies |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| TableCapacity | Tabelcapaciteit | Bytes | Gemiddeld | De hoeveelheid opslag die wordt gebruikt door de Table-service van het opslagaccount (in bytes). | Er zijn geen dimensies |
| TableCount | Aantal tabellen | Count | Gemiddeld | Het aantal tabellen in de Table-service van het opslagaccount. | Er zijn geen dimensies |
| TableEntityCount | Aantal tabelentiteiten | Count | Gemiddeld | Het aantal tabelentiteiten in de Table-service van het opslagaccount. | Er zijn geen dimensies |
| Transacties | Transacties | Count | Totaal | Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie. | De ResponseType, GeoType, ApiName |
| Inkomend verkeer | Inkomend verkeer | Bytes | Totaal | De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure. | GeoType, ApiName |
| Uitgaand verkeer | Uitgaand verkeer | Bytes | Totaal | De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer. | GeoType, ApiName |
| SuccessServerLatency | Geslaagde serverlatentie | Milliseconden | Gemiddeld | De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde omvat niet de netwerklatentie die in AverageE2ELatency is opgegeven. | GeoType, ApiName |
| SuccessE2ELatency | Geslaagde E2E-latentie | Milliseconden | Gemiddeld | De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen. | GeoType, ApiName |
| Beschikbaarheid | Beschikbaarheid | Procent | Gemiddeld | Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot verminderde beschikbaarheid voor de opslagservice of de opgegeven API-bewerking. | GeoType, ApiName |

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

| Gegevens | De naam van de metrische gegevens weergeven | Eenheid | Aggregatietype | Description | Dimensies |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| QueueCapacity | Wachtrijcapaciteit | Bytes | Gemiddeld | De hoeveelheid opslag die wordt gebruikt door de Queue-service van het opslagaccount (in bytes). | Er zijn geen dimensies |
| QueueCount | Aantal wachtrijen | Count | Gemiddeld | Het aantal wachtrijen in de Queue-service van het opslagaccount. | Er zijn geen dimensies |
| QueueMessageCount | Aantal wachtrijberichten | Count | Gemiddeld | Het geschatte aantal wachtrijberichten in de Queue-service van het opslagaccount. | Er zijn geen dimensies |
| Transacties | Transacties | Count | Totaal | Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen evenals aanvragen die tot fouten. Gebruik de ResponseType-dimensie voor het aantal verschillende type reactie. | De ResponseType, GeoType, ApiName |
| Inkomend verkeer | Inkomend verkeer | Bytes | Totaal | De hoeveelheid inkomende gegevens in bytes. Hieronder vallen de inkomende gegevens van een externe client in Azure Storage evenals de inkomende gegevens binnen Azure. | GeoType, ApiName |
| Uitgaand verkeer | Uitgaand verkeer | Bytes | Totaal | De hoeveelheid uitgaande gegevens in bytes. Hieronder vallen de uitgaande gegevens van een externe client in Azure Storage evenals de uitgaande gegevens binnen Azure. Daarom geeft deze hoeveelheid niet de factureerbare uitgaande gegevens weer. | GeoType, ApiName |
| SuccessServerLatency | Geslaagde serverlatentie | Milliseconden | Gemiddeld | De gemiddelde latentie die wordt gebruikt door Azure Storage voor het verwerken van een aanvraag is gelukt, in milliseconden. Deze waarde omvat niet de netwerklatentie die in AverageE2ELatency is opgegeven. | GeoType, ApiName |
| SuccessE2ELatency | Geslaagde E2E-latentie | Milliseconden | Gemiddeld | De gemiddelde end-to-end latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking, in milliseconden. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage die nodig is om de aanvraag te lezen, het antwoord te verzenden en bevestiging van het antwoord te ontvangen. | GeoType, ApiName |
| Beschikbaarheid | Beschikbaarheid | Procent | Gemiddeld | Het percentage van de beschikbaarheid van de opslagservice of de opgegeven API-bewerking. Beschikbaarheid wordt berekend door de TotalBillableRequests-waarde maken en delen door het aantal toepasselijke aanvragen, met inbegrip van die onverwachte fouten zijn opgetreden. Alle onverwachte fouten leiden tot verminderde beschikbaarheid voor de opslagservice of de opgegeven API-bewerking. | GeoType, ApiName |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Azure bewaken in Azure Stack](azure-stack-metrics-azure-data.md).
