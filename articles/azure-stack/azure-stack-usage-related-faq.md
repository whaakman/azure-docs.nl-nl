---
title: Gebruiks-API gerelateerde Veelgestelde vragen | Microsoft Docs
description: Lijst van Stack Azure meters vergelijking van Azure usage API, gebruikstijd en gerapporteerd tijd, foutcodes.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: ac22ba34bff1d5321c05bc0a0a1b14ca742079a7
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051492"
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Veelgestelde vragen in Azure Stack gebruiks-API

Dit artikel worden enkele veelgestelde vragen over de Azure-API voor het gebruik van Stack.

## <a name="what-meter-ids-can-i-see"></a>Welke meter id's kan ik zien?
Gebruik wordt voor de volgende resourceproviders gerapporteerd:

**Netwerk**  
  
**ID meten**: F271A8A388C44D93956A063E1D2FA80B  
**De naam van de meter**: gebruik van statische IP-adres  
**Eenheid**: IP-adressen  
**Opmerkingen bij de**: aantal IP-adressen die worden gebruikt. Als u het gebruik van API met dagelijkse samenvattingen aanroept, retourneert de meter IP-adres vermenigvuldigd met het aantal uren.  
  
**ID meten**: 9E2739BA86744796B465F64674B822BA  
**De naam van de meter**: gebruik van dynamische IP-adres  
**Eenheid**: IP-adressen  
**Opmerkingen bij de**: aantal IP-adressen die worden gebruikt. Als u het gebruik van API met dagelijkse samenvattingen aanroept, retourneert de meter IP-adres vermenigvuldigd met het aantal uren.  
  
**Storage**  
  
**ID meten**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**De naam van de meter**: TableCapacity  
**Eenheid**: GB\*uur  
**Opmerkingen bij de**: totale capaciteit is verbruikt door tabellen.  
  
**ID meten**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**De naam van de meter**: PageBlobCapacity  
**Eenheid**: GB\*uur  
**Opmerkingen bij de**: totale capaciteit van pagina-blobs verbruikt.  
  
**ID meten**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**De naam van de meter**: QueueCapacity  
**Eenheid**: GB\*uur  
**Opmerkingen bij de**: totale capaciteit gebruikt door de wachtrij.  
  
**ID meten**: 09F8879E-87E9-4305-A572-4B7BE209F857  
**De naam van de meter**: BlockBlobCapacity  
**Eenheid**: GB\*uur  
**Opmerkingen bij de**: totale capaciteit is verbruikt door blok-blobs.  
  
**ID meten**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**De naam van de meter**: TableTransactions  
**Eenheid**: aantal in 10 000's aanvragen  
**Opmerkingen bij de**: tabel serviceaanvragen (per 10.000).  
  
**ID meten**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**De naam van de meter**: TableDataTransIn  
**Eenheid**: inkomende gegevens in GB  
**Opmerkingen bij de**: Table service data inkomend GB.  
  
**ID meten**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**De naam van de meter**: TableDataTransOut  
**Eenheid**: uitgaande in GB  
**Opmerkingen bij de**: tabel Uitgaande van de service-gegevens in GB  
  
**ID meten**: 43DAF82B-4618-444A-B994-40C23F7CD438  
**De naam van de meter**: BlobTransactions  
**Eenheid**: de 000's in 10, tellen aanvragen  
**Opmerkingen bij de**: serviceaanvragen-Blob (per 10.000).  
  
**ID meten**: 9764F92C-E44A-498E-8DC1-AAD66587A810  
**De naam van de meter**: BlobDataTransIn  
**Eenheid**: inkomende gegevens in GB  
**Opmerkingen bij de**: Blob-service gegevens inkomend GB.  
  
**ID meten**: 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**De naam van de meter**: BlobDataTransOut  
**Eenheid**: uitgaande in GB  
**Opmerkingen bij de**: Blob-service gegevens uitgaande GB.  
  
**ID meten**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**De naam van de meter**: QueueTransactions  
**Eenheid**: de 000's in 10, tellen aanvragen  
**Opmerkingen bij de**: serviceaanvragen (per 10.000) in de wachtrij.  
  
**ID meten**: E518E809-E369-4A45-9274-2017B29FFF25  
**De naam van de meter**: QueueDataTransIn  
**Eenheid**: inkomende gegevens in GB  
**Opmerkingen bij de**: service gegevens inkomend in GB in de wachtrij.  
  
**ID meten**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**De naam van de meter**: QueueDataTransOut  
**Eenheid**: uitgaande in GB  
**Opmerkingen bij de**: uitgaande van de service-gegevens in GB in de wachtrij  
  
**Sql RP**  
  
**ID meten**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**De naam van de meter**: DatabaseSizeHourSqlMeter  
**Eenheid**: MB\*uur  
**Opmerkingen bij de**: totale DB-capaciteit op maken. Als u het gebruik van API met dagelijkse samenvattingen aanroept, retourneert de meter MB vermenigvuldigd met het aantal uren.  
  
**MySql RP**  
  
**ID meten**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**De naam van de meter**: DatabaseSizeHourMySqlMeter  
**Eenheid**: MB\*uur  
**Opmerkingen bij de**: totale DB-capaciteit op maken. Als u het gebruik van API met dagelijkse samenvattingen aanroept, retourneert de meter MB vermenigvuldigd met het aantal uren.  
  
**Compute**  
  
**ID meten**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**De naam van de meter**: uren Base VM-grootte  
**Eenheid**: virtuele core-uren  
**Opmerkingen bij de**: het aantal virtuele kernen vermenigvuldigd met de uren dat de virtuele machine wordt uitgevoerd.  
  
**ID meten**: 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**De naam van de meter**: uren Windows VM-grootte  
**Eenheid**: virtuele core-uren  
**Opmerkingen bij de**: het aantal virtuele kernen vermenigvuldigd met uur uitgevoerd voor de virtuele machine.  
  
**ID meten**: 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**De naam van de meter**: uren voor VM-grootte  
**Eenheid**: uren VM  
**Opmerkingen bij de**: basis- en Windows VM vastgelegd. Niet aangepast voor kernen.  
  
**Key Vault**  
  
**ID meten**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**De naam van de meter**: Sleutelkluis transacties  
**Eenheid**: aantal in 10 000's aanvragen  
**Opmerkingen bij de**: nummer van de REST API-aanvragen ontvangen door vlak voor Sleutelkluis-gegevens.  
  
**ID meten**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**De naam van de meter**: geavanceerde sleutels transacties  
**Eenheid**: 10 K transacties  
**Opmerkingen bij de**: RSA 3 K/4 K ECC-sleutel transacties. (preview).  
  
*App service**  
  
**ID meten**: 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**De naam van de meter**: App Service  
**Eenheid**: virtuele core-uren  
**Opmerkingen bij de**: het aantal virtuele kernen gebruikt voor het uitvoeren van app service. Opmerking: Microsoft gebruikt deze meter rekening te brengen van de App-Service op Azure-Stack. Cloudserviceproviders kunt de App-Service meters (Zie hieronder) gebruiken voor het berekenen van gebruiksgegevens voor hun tenants.  
  
**ID meten**: 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE  
**De naam van de meter**: functies aanvragen  
**Eenheid**: 10 aanvragen  
**Opmerkingen bij de**: totaal aantal aangevraagde uitvoeringen (per 10 uitvoeringen). Uitvoeringen worden geteld telkens wanneer een functie wordt uitgevoerd in reactie op een gebeurtenis of door een binding wordt geactiveerd.  
  
**ID meten**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**De naam van de meter**: functies - berekenen  
**Eenheid**: GB-s  
**Opmerkingen bij de**: brongebruik gemeten in seconden gigabyte (GB/s). **Brongebruik waargenomen** wordt berekend door het gemiddelde geheugengrootte GB vermenigvuldigen met de tijd in milliseconden die nodig is voor het uitvoeren van de functie. Geheugen dat wordt gebruikt door een functie wordt gemeten door afronding tot de dichtstbijzijnde 128 MB, tot aan de grootte van de maximale hoeveelheid geheugen 1.536 MB met uitvoertijd berekend door het afronden naar het dichtstbijzijnde 1 ms. De minimale uitvoeringstijd en het geheugen voor de uitvoering van een functie is respectievelijk 100 ms en 128 mb.  
  
**ID meten**: 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**De naam van de meter**: Shared App Service-uren  
**Eenheid**: 1 uur  
**Opmerkingen bij de**: Per uur gebruik van shard-App Service-Plan. Abonnementen worden gemeten op basis van de per-App.  
  
**ID meten**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**De naam van de meter**: gratis App Service-uren  
**Eenheid**: 1 uur  
**Opmerkingen bij de**: Per uur gebruik van de gratis App Service-Plan. Abonnementen worden gemeten op basis van de per-App.  
  
**ID meten**: 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**De naam van de meter**: kleine Standard-App Service-uren  
**Eenheid**: 1 uur  
**Opmerkingen bij de**: berekend op basis van de grootte en het aantal exemplaren.  
  
**ID meten**: 83A2A13E-4788-78DD-5D55-2831B68ED825  
**De naam van de meter**: Standard-App Service-uren gemiddeld  
**Eenheid**: 1 uur  
**Opmerkingen bij de**: berekend op basis van de grootte en het aantal exemplaren.  
  
**ID meten**: 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**De naam van de meter**: grote Standard-App Service-uren  
**Eenheid**: 1 uur  
**Opmerkingen bij de**: berekend op basis van de grootte en het aantal exemplaren.  
  
**Aangepaste Worker lagen**  
  
**ID meten**: *aangepaste Worker lagen*  
**De naam van de meter**: aangepaste Worker lagen  
**Eenheid**: uren  
**Opmerkingen bij de**: deterministische meter-ID is gemaakt op basis van SKU's en de naam van de aangepaste werknemer-laag. Deze meter-ID is uniek voor elke aangepaste worker-laag.  
  
**ID meten**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**De naam van de meter**: SNI SSL  
**Eenheid**: Per SNI-SSL-Binding  
**Opmerkingen bij de**: App Service ondersteunt twee soorten SSL-verbindingen: Server Name vermelding (SNI) SSL-verbindingen en IP-adres SSL-verbindingen. Op SNI gebaseerd SSL werkt in moderne browsers terwijl op IP gebaseerde SSL in alle browsers werkt.  
  
**ID meten**: 60B42D72-DC1C-472C-9895-6C516277EDB4  
**De naam van de meter**: IP SSL  
**Eenheid**: Per IP op basis van SSL-Binding  
**Opmerkingen bij de**: App Service ondersteunt twee soorten SSL-verbindingen: Server Name vermelding (SNI) SSL-verbindingen en IP-adres SSL-verbindingen. Op SNI gebaseerd SSL werkt in moderne browsers terwijl op IP gebaseerde SSL in alle browsers werkt.  
  
**ID meten**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**De naam van de meter**: Web-proces  
**Eenheid**:  
**Opmerkingen bij de**: berekend per actieve site per uur.  
  
**ID meten**: 5887D39B-0253-4E12-83C7-03E1A93DFFD9  
**De naam van de meter**: externe uitgaande bandbreedte  
**Eenheid**: GB  
**Opmerkingen bij de**: totaal aantal binnenkomende aanvraag antwoordbytes + totaal aantal uitgaande aanvraag bytes + totale binnenkomende FTP-aanvragen antwoordbytes + totaal aantal binnenkomende Internet implementeren aanvraag antwoordbytes.  
  

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Hoe kan het Azure-Stack gebruik API's met vergelijken de [Azure gebruiks-API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (momenteel in de openbare preview)?
* De Tenant-API voor informatie over het gebruik is consistent met de Azure-API, met één uitzondering: de *showDetails* vlag momenteel niet ondersteund in Azure-Stack.
* De Provider gebruik API geldt alleen voor Azure-Stack.
* Op dit moment wordt de [RateCard API](https://msdn.microsoft.com/library/azure/mt219004.aspx) is beschikbaar in Azure is niet beschikbaar in Azure-Stack.

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
