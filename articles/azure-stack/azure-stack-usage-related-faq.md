---
title: API voor gebruik met veelgestelde vragen over betrekking | Microsoft Docs
description: Lijst met Azure Stack-meters, vergelijking met het gebruik van Azure-API, de tijd gebruikt en de tijd gerapporteerd foutcodes.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: e4bb17bd068076a9ffa9bcab5826f0efa84a19d7
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406002"
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Veelgestelde vragen over Azure Stack gebruiks-API

In dit artikel vindt u antwoorden op enkele veelgestelde vragen over de API van Azure Stack-gebruik.

## <a name="what-meter-ids-can-i-see"></a>Welke meter-id's kan ik zien?
Gebruik wordt voor de volgende resourceproviders gerapporteerd:

### <a name="network"></a>Netwerk
  
**ID meten**: F271A8A388C44D93956A063E1D2FA80B  
**Meternaam**: gebruik van statische IP-adres  
**Eenheid**: IP-adressen  
**Opmerkingen bij de**: aantal van de IP-adressen die worden gebruikt. Als u de API voor gebruik met een dagelijkse granulariteit aanroept, wordt de meter IP-adres vermenigvuldigd met het aantal uren.  
  
**ID meten**: 9E2739BA86744796B465F64674B822BA  
**Meternaam**: gebruik van dynamische IP-adres  
**Eenheid**: IP-adressen  
**Opmerkingen bij de**: aantal van de IP-adressen die worden gebruikt. Als u de API voor gebruik met een dagelijkse granulariteit aanroept, wordt de meter IP-adres vermenigvuldigd met het aantal uren.  
  
### <a name="storage"></a>Storage
  
**ID meten**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Meternaam**: TableCapacity  
**Eenheid**: GB\*uur  
**Opmerkingen bij de**: totale capaciteit die worden gebruikt door de tabellen.  
  
**ID meten**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Meternaam**: PageBlobCapacity  
**Eenheid**: GB\*uur  
**Opmerkingen bij de**: totale capaciteit van pagina-blobs gebruikt.  
  
**ID meten**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Meternaam**: QueueCapacity  
**Eenheid**: GB\*uur  
**Opmerkingen bij de**: totale capaciteit die worden gebruikt door de wachtrij.  
  
**ID meten**: 09F8879E-87E9-4305-A572-4B7BE209F857  
**Meternaam**: BlockBlobCapacity  
**Eenheid**: GB\*uur  
**Opmerkingen bij de**: totale capaciteit die worden gebruikt door de blok-blobs.  
  
**ID meten**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Meternaam**: TableTransactions  
**Eenheid**: aantal in 10 000's aanvragen  
**Opmerkingen bij de**: serviceaanvragen tabel (per 10.000).  
  
**ID meten**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Meternaam**: TableDataTransIn  
**Eenheid**: inkomende gegevens in GB  
**Opmerkingen bij de**: Table service binnenkomende in GB.  
  
**ID meten**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**Meternaam**: TableDataTransOut  
**Eenheid**: uitgaand verkeer in GB  
**Opmerkingen bij de**: tabel Uitgaande van de service-gegevens in GB  
  
**ID meten**: 43DAF82B-4618-444A-B994-40C23F7CD438  
**Meternaam**: BlobTransactions  
**Eenheid**: aanvragen tellen in 10 000's  
**Opmerkingen bij de**: serviceaanvragen Blob (per 10.000).  
  
**ID meten**: 9764F92C-E44A-498E-8DC1-AAD66587A810  
**Meternaam**: BlobDataTransIn  
**Eenheid**: inkomende gegevens in GB  
**Opmerkingen bij de**: Blob-service binnenkomende in GB.  
  
**ID meten**: 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**Meternaam**: BlobDataTransOut  
**Eenheid**: uitgaand verkeer in GB  
**Opmerkingen bij de**: Blob-service gegevensverkeer in GB.  
  
**ID meten**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Meternaam**: QueueTransactions  
**Eenheid**: aanvragen tellen in 10 000's  
**Opmerkingen bij de**: serviceaanvragen in de wachtrij (per 10.000).  
  
**ID meten**: E518E809-E369-4A45-9274-2017B29FFF25  
**Meternaam**: QueueDataTransIn  
**Eenheid**: inkomende gegevens in GB  
**Opmerkingen bij de**: service binnenkomende in GB in de wachtrij.  
  
**ID meten**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Meternaam**: QueueDataTransOut  
**Eenheid**: uitgaand verkeer in GB  
**Opmerkingen bij de**: uitgaande van de service-gegevens in GB in de wachtrij  

### <a name="compute"></a>Compute 
  
**ID meten**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Meternaam**: uren van Base VM-grootte  
**Eenheid**: virtuele kern-uur  
**Opmerkingen bij de**: aantal virtuele kernen vermenigvuldigd met de uren dat de virtuele machine is uitgevoerd.  
  
**ID meten**: 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**Meternaam**: uren Windows VM-grootte  
**Eenheid**: virtuele kern-uur  
**Opmerkingen bij de**: aantal virtuele kernen vermenigvuldigd met de uren dat de virtuele machine is uitgevoerd.  
  
**ID meten**: 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**Meternaam**: uren voor VM-grootte  
**Eenheid**: VM-uren  
**Opmerkingen bij de**: basis- en Windows VM vastgelegd. Niet aangepast voor de kernen.  
  
### <a name="managed-disks"></a>Beheerde schijven

**ID meten**: 5d76e09f-4567-452a-94cc-7d1f097761f0   
**Meternaam**: S4   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: standaard beheerde schijf: 32 GB 

**ID meten**: dc9fc6a9-0782-432a-b8dc-978130457494   
**Meternaam**: S6   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: standaard beheerde schijf – 64 GB 

**ID meten**: e5572fce-9f58-49d7-840c-b168c0f01fff   
**Meternaam**: S10   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: standaard beheerde schijf: 128 GB 

**ID meten**: 9a8caedd-1195-4cd5-80b4-a4c22f9302b8   
**Meternaam**: S15   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: standaard beheerde schijf – 256 GB 

**ID meten**: 5938f8da-0ecd-4c48-8d5a-c7c6c23546be   
**Meternaam**: S20   
**Eenheid**: het aantal schijven      
**Opmerkingen bij de**: standaard beheerde schijf – 512 GB 

**ID meten**: 7705a158-bd8b-4b2b-b4c2-0782343b81e6   
**Meternaam**: S30   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: standaard beheerde schijf: 1024 GB 

**ID meten**: d9aac1eb-a5d1-42f2-b617-9e3ea94fed88   
**Meternaam**: S40   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: standaard beheerde schijf: 2048 GB 

**ID meten**: a54899dd-458e-4a40-9abd-f57cafd936a7   
**Meternaam**: S50   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: standaard beheerde schijf – 4096 GB 

**ID meten**: 5c105f5f-cbdf-435c-b49b-3c7174856dcc   
**Meternaam**: P4   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: Premium-beheerde schijf: 32 GB 

**ID meten**: 518b412b-1927-4f25-985f-4aea24e55c4f   
**Meternaam**: P6   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: Premium-beheerde schijf – 64 GB 

**ID meten**: 5cfb1fed-0902-49e3-8217-9add946fd624   
**Meternaam**: P10   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: Premium-beheerde schijf: 128 GB  

**ID meten**: 8de91c94-f740-4d9a-b665-bd5974fa08d4   
**Meternaam**: P15  
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: Premium-beheerde schijf – 256 GB 

**ID meten**: c7e7839c-293b-4761-ae4c-848eda91130b   
**Meternaam**: P20   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: Premium-beheerde schijf – 512 GB 

**ID meten**: 9f502103-adf4-4488-b494-456c95d23a9f   
**Meternaam**: P30   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: Premium-beheerde schijf: 1024 GB 

**ID meten**: 043757fc-049f-4e8b-8379-45bb203c36b1   
**Meternaam**: P40   
**Eenheid**: het aantal schijven    
**Opmerkingen bij de**: Premium-beheerde schijf: 2048 GB 

**ID meten**: c0342c6f-810b-4942-85d3-6eaa561b6570   
**Meternaam**: P50   
**Eenheid**: het aantal schijven   
**Opmerkingen bij de**: Premium-beheerde schijf – 4096 GB 

**ID meten**: 8a409390-1913-40ae-917b-08d0f16f3c38   
**Meternaam**: ActualStandardDiskSize   
**Eenheid**: bytes      
**Opmerkingen bij de**: de werkelijke grootte op schijf van de standard-beheerde schijven  

**ID meten**: 1273b16f-8458-4c34-8ce2-a515de551ef6  
**Meternaam**: ActualPremiumDiskSize   
**Eenheid**: bytes      
**Opmerkingen bij de**: de werkelijke grootte op schijf van de premium-beheerde schijf 

**ID meten**: 89009682-df7f-44fe-aeb1-63fba3ddbf4c  
**Meternaam**: ActualStandardSnapshotSize   
**Eenheid**: bytes   
**Opmerkingen bij de**: de werkelijke grootte op schijf van beheerde standard momentopname.  

**ID meten**: 95b0c03f-8a82-4524-8961-ccfbf575f536   
**Meternaam**: ActualPremiumSnapshotSize   
**Eenheid**: bytes   
**Opmerkingen bij de**: de werkelijke grootte op schijf van beheerde premium.   

### <a name="sql-rp"></a>SQL-RP
  
**ID meten**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**Meternaam**: DatabaseSizeHourSqlMeter  
**Eenheid**: MB\*uur  
**Opmerkingen bij de**: totaal aantal DB capaciteit bij het maken ervan. Als u de API voor gebruik met een dagelijkse granulariteit aanroept, retourneert de meter MB vermenigvuldigd met het aantal uren.  
  
### <a name="mysql-rp"></a>MySql-RP   
  
**ID meten**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Meternaam**: DatabaseSizeHourMySqlMeter  
**Eenheid**: MB\*uur  
**Opmerkingen bij de**: totaal aantal DB capaciteit bij het maken ervan. Als u de API voor gebruik met een dagelijkse granulariteit aanroept, retourneert de meter MB vermenigvuldigd met het aantal uren.    
### <a name="key-vault"></a>Key Vault   
  
**ID meten**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Meternaam**: Key Vault-transacties  
**Eenheid**: aantal in 10 000's aanvragen  
**Opmerkingen bij de**: nummer van de REST-API-aanvragen die zijn ontvangen door Key Vault-gegevenslaag.  
  
**ID meten**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Meternaam**: geavanceerde sleutels transacties  
**Eenheid**: 10 K transacties  
**Opmerkingen bij de**: RSA K 3/4 K, ECC-sleutel transacties. (preview).  
  
### <a name="app-service"></a>App Service   
  
**ID meten**: 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**Meternaam**: App Service  
**Eenheid**: virtuele kern-uur  
**Opmerkingen bij de**: aantal virtuele kernen gebruikt voor het uitvoeren van appservice. Opmerking: Microsoft gebruikt deze meter in rekening te brengen van de App Service in Azure Stack. Cloud-serviceproviders kunnen de andere App Service meters (Zie hieronder) gebruiken om te berekenen gebruik voor hun tenants.  
  
**ID meten**: 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE  
**Meternaam**: Functions-aanvragen  
**Eenheid**: 10 aanvragen  
**Opmerkingen bij de**: totaal aantal aangevraagde uitvoeringen (per 10 uitvoeringen). Uitvoeringen worden steeds geteld wanneer een functie wordt uitgevoerd in reactie op een gebeurtenis of wordt geactiveerd door een binding.  
  
**ID meten**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Meternaam**: Functions - Compute  
**Eenheid**: GB-s  
**Opmerkingen bij de**: resourceverbruik gemeten in gigabyte-seconden (GB/s). **Waargenomen resourceverbruik** wordt berekend door de gemiddelde resourcegrootte in GB vermenigvuldigen met de tijd in milliseconden die nodig is voor het uitvoeren van de functie. Geheugenverbruik van een functie wordt gemeten door naar de dichtstbijzijnde 128 MB tot de maximale geheugengrootte van 1536 MB is afronding uitvoeringstijd wordt berekend door af te ronden op de dichtstbijzijnde 1 ms. De minimale uitvoeringstijd en het geheugen voor een enkele functie-uitvoering is respectievelijk 100 ms en 128 mb.  
  
**ID meten**: 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**Meternaam**: gedeeld App Service-uren  
**Eenheid**: 1 uur  
**Opmerkingen bij de**: Per uur gebruik van App Service-Plan shard. Abonnementen worden gemeten op basis van afzonderlijke Apps.  
  
**ID meten**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**Meternaam**: gratis App Service-uren  
**Eenheid**: 1 uur  
**Opmerkingen bij de**: Per uur gebruik van de gratis App Service-Plan. Abonnementen worden gemeten op basis van afzonderlijke Apps.  
  
**ID meten**: 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**Meternaam**: kleine standaard App Service-uren  
**Eenheid**: 1 uur  
**Opmerkingen bij de**: berekend op basis van de grootte en het aantal exemplaren.  
  
**ID meten**: 83A2A13E-4788-78DD-5D55-2831B68ED825  
**Meternaam**: Standard App Service-uren gemiddeld  
**Eenheid**: 1 uur  
**Opmerkingen bij de**: berekend op basis van de grootte en het aantal exemplaren.  
  
**ID meten**: 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**Meternaam**: grote standaard App Service-uren  
**Eenheid**: 1 uur  
**Opmerkingen bij de**: berekend op basis van de grootte en het aantal exemplaren.  
  
### <a name="custom-worker-tiers"></a>Aangepaste Werkrolniveaus   
  
**ID meten**: *aangepaste Werkrolniveaus*  
**Meternaam**: aangepaste Werkrolniveaus  
**Eenheid**: uur  
**Opmerkingen bij de**: deterministische meter-ID is gemaakt op basis van SKU's en de naam van de aangepaste werknemer-laag. Deze meter-ID is uniek voor elke aangepaste werknemer-laag.  
  
**ID meten**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**Meternaam**: SNI SSL  
**Eenheid**: Per SNI SSL-Binding  
**Opmerkingen bij de**: App Service ondersteunt twee typen SSL-verbindingen: SSL-verbindingen met Server Name Indication (SNI) en SSL-verbindingen voor IP-adres. Op SNI gebaseerd SSL werkt in moderne browsers terwijl op IP gebaseerde SSL in alle browsers werkt.  
  
**ID meten**: 60B42D72-DC1C-472C-9895-6C516277EDB4  
**Meternaam**: IP SSL  
**Eenheid**: Per IP op basis van SSL-Binding  
**Opmerkingen bij de**: App Service ondersteunt twee typen SSL-verbindingen: SSL-verbindingen met Server Name Indication (SNI) en SSL-verbindingen voor IP-adres. Op SNI gebaseerd SSL werkt in moderne browsers terwijl op IP gebaseerde SSL in alle browsers werkt.  
  
**ID meten**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Meternaam**: webverwerking  
**Eenheid**:  
**Opmerkingen bij de**: per actieve site per uur berekend.  
  
**ID meten**: 5887D39B-0253-4E12-83C7-03E1A93DFFD9  
**Meternaam**: externe bandbreedte voor uitgaand verkeer  
**Eenheid**: GB  
**Opmerkingen bij de**: totaal aantal binnenkomende aanvraag antwoordbytes + totaal aantal uitgaande aanvraag bytes + totaal aantal binnenkomende FTP-aanvragen antwoordbytes + totaal aantal binnenkomende web antwoordbytes voor aanvraag implementeren.  
  

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsdocsmicrosoftcomazurebillingbilling-usage-rate-card-overviewazure-resource-usage-api-preview-currently-in-public-preview"></a>Hoe kan het Azure Stack-gebruik API's zich tot verhouden de [gebruik van Azure API](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-usage-api-preview) (momenteel in openbare preview)?
* De API voor het gebruik van Tenant is consistent zijn met de API van Azure, met één uitzondering: de *showDetails* vlag wordt momenteel niet ondersteund in Azure Stack.
* De Provider Usage API geldt alleen voor Azure Stack.
* Op dit moment de [RateCard API](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-ratecard-api-preview) die beschikbaar zijn in Azure is niet beschikbaar in Azure Stack.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Wat is het verschil tussen de gebruikstijd en gerapporteerde tijd?
Gebruiksrapporten van de gegevens hebben twee belangrijkste tijd-waarden:

* **Gerapporteerde tijd**. De tijd waarop de gebeurtenis gebruik het systeem gebruik ingevoerd
* **Gebruikstijd**. De tijd waarop de Azure Stack-resource is verbruikt

U ziet een verschil in waarden voor het gebruik van tijd en gerapporteerd voor een specifieke gebruiksgebeurtenis worden gerapporteerd. De vertraging kan meerdere uren in een omgeving zo lang zijn.

Op dit moment kunt u opvragen alleen *gemelde tijd*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Wat betekenen deze gebruik API-foutcodes?
| **HTTP-statuscode** | **Foutcode** | **Beschrijving** |
| --- | --- | --- |
| / 400-Ongeldige aanvraag |*NoApiVersion* |De *api-versie* queryparameter ontbreekt. |
| / 400-Ongeldige aanvraag |*InvalidProperty* |Een eigenschap ontbreekt of heeft een ongeldige waarde. Het bericht in de foutcode in de antwoordtekst identificeert de eigenschap ontbreekt. |
| / 400-Ongeldige aanvraag |*RequestEndTimeIsInFuture* |De waarde voor *ReportedEndTime* in de toekomst ligt. Waarden zijn in de toekomst niet toegestaan voor dit argument. |
| / 400-Ongeldige aanvraag |*SubscriberIdIsNotDirectTenant* |Een serviceprovider-API-aanroep heeft een abonnements-ID is niet een geldige tenant van de oproepende functie gebruikt. |
| / 400-Ongeldige aanvraag |*SubscriptionIdMissingInRequest* |De abonnements-ID van de oproepende functie ontbreekt. |
| / 400-Ongeldige aanvraag |*InvalidAggregationGranularity* |Een ongeldige aggregatie granulariteit is aangevraagd. Geldige waarden zijn per dag en uur. |
| 503 |*ServiceUnavailable* |Er is een herstelbare fout opgetreden omdat de service bezet is of de oproep wordt beperkt. |

## <a name="next-steps"></a>Volgende stappen
[De facturering van klanten en chargeback in Azure Stack](azure-stack-billing-and-chargeback.md)

[Resourcegebruik-provider API](azure-stack-provider-resource-api.md)

[Resourcegebruik-API voor de tenantsleutel](azure-stack-tenant-resource-usage-api.md)
