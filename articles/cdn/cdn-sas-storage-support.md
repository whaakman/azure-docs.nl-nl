---
title: Azure CDN gebruiken met SAS | Microsoft Docs
description: 
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: rli v-deasim
ms.openlocfilehash: de30f4319be75362131f8c8ad71aad57b0528f05
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="using-azure-cdn-with-sas"></a>Azure CDN gebruiken met SAS

Wanneer u inhoud van de opslagcontainer van uw opslagaccount, u mogelijk wilt beveiligen hoe gebruikers hebben toegang tot uw bestanden met persoonlijke toegang verlenen tot de storage-container. Anders wordt zijn een opslagcontainer waarvoor openbare toegang heeft gekregen toegankelijk voor iedereen die de URL herkent. Als u wilt beveiligen een opslagaccount dat u hebt het content delivery network (CDN) voor toegang tot toegestaan, kunt u de Shared Access Signature (SAS)-functie van Azure storage beperkte toegang tot persoonlijke storage-containers.

Een SAS is een URI die verleent rechten aan uw Azure Storage-resources beperkte zonder dat de sleutel van uw account. U kunt een SAS bieden aan clients die u niet vertrouwt door de sleutel van uw opslagaccount, maar aan wie u wilt toegang tot bepaalde resources storage account delegeren. Door het distribueren van een shared access signature URI voor deze clients verlenen u toegang tot een bron voor een opgegeven periode.
 
SAS kunt u verschillende parameters van toegang tot een blob, zoals begin-en verloopdatum machtigingen (lezen/schrijven) en IP-adresbereiken definiëren. In dit artikel wordt beschreven hoe SAS gebruiken in combinatie met Azure CDN. Zie voor meer informatie over SAS, waaronder het maken van deze en de bijbehorende parameteropties [Using shared access signatures (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Instellen van Azure CDN voor het werken met SAS-opslag
De volgende drie opties worden aanbevolen voor het gebruik van SAS met Azure CDN. Alle opties wordt ervan uitgegaan dat u al een werkende SAS (Zie vereisten) hebt gemaakt. 
 
### <a name="prerequisites"></a>Vereisten
Als u wilt starten, een opslagaccount maken en vervolgens een SAS genereren voor uw asset. U kunt twee soorten handtekeningen opgeslagen toegang genereren: een service-SAS of een SAS-account. Zie voor meer informatie [typen handtekeningen voor gedeelde toegang](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Nadat u een SAS hebt gegenereerd, kunt u toegang tot uw blob-opslag-bestand met een URL die de volgende indeling heeft:`https://<account>.blob.core.windows.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
Bijvoorbeeld:
 ```
https://democdnstorage1.blob.core.windows.net/container1/sasblob.txt?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Zie voor meer informatie over parameters die instelling [SAS parameter overwegingen](#sas-parameter-considerations) en [Shared access signature parameters](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![CDN SAS-instellingen](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-the-cdn"></a>Optie 1: Via SAS met Pass Through-query naar de blob storage vanaf de CDN

Deze optie is de eenvoudigste en slechts een enkele SAS-token, die wordt doorgegeven vanaf de CDN naar de oorspronkelijke server gebruikt. Dit wordt ondersteund door **Azure CDN van Verizon**, voor zowel Standard en Premium-profielen en **Azure CDN van Akamai**. 
 
1. Selecteer een eindpunt, klikt u op **regels opslaan in cache**, selecteer daarna **elke unieke URL in de Cache** van de **Query opslaan in cache** lijst.

    ![In het cachegeheugen CDN-regels](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Gebruik de SAS-token na het instellen van SAS van uw opslagaccount met de CDN-URL voor toegang tot het bestand. 
   
   De resulterende URL heeft de volgende indeling:`https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`

   Bijvoorbeeld:   
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. De Cacheduur verfijnen met behulp van de cache in regels of door toe te voegen `Cache-Control` headers aan de oorsprong. Omdat de CDN het SAS-token als een gewone queryreeks behandelt, als best practice moet u instellen een cache in duur dat op of vóór de verlooptijd van de SAS verloopt. Anders als een bestand is in de cache voor een langere duur dan de SAS actief is, het bestand mogelijk toegankelijk is vanaf de CDN-bronserver nadat de verlooptijd van de SAS is verstreken. Als dit het geval is en u wilt uw cache-bestand niet toegankelijk te maken, moet u een opschonen-bewerking op het bestand naar deze uit de cache wissen uitvoeren. Zie voor meer informatie over het instellen van de Cacheduur van de op de CDN [besturingselement Azure Content Delivery Network cachegedrag met caching regels](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-security-token-using-rewrite-rule"></a>Optie 2: Verborgen CDN beveiligingstoken met behulp van de regel herschrijven
 
Met deze optie kunt u de oorsprong blob-opslag beveiligen zonder een SAS-token voor de gebruiker CDN. U wilt Gebruik deze optie als u specifieke toegangsbeperkingen voor het bestand niet nodig, maar u wilt voorkomen dat gebruikers toegang krijgen tot de oorsprong opslag rechtstreeks ter verbetering van CDN-offload tijden. Deze optie is alleen beschikbaar voor **Azure CDN Premium van Verizon** profielen. 
 
1. Gebruik de [regelengine](cdn-rules-engine.md) om een regel voor het herschrijven van URL's te maken. Nieuwe regels duren ongeveer 90 minuten worden doorgegeven.

   ![Knop CDN beheren](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Knop CDN regels-engine](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Deze regel voor het herschrijven van URL's van voorbeeld heeft de volgende patronen:
   
   Bron:   
   `/test/demo.jpg`
   
   Bestemming:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![Regel voor het herschrijven van CDN URL 's](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
 
2. Toegang tot het bestand op uw CDN zonder het SAS-token in de volgende indeling:`https://<endpoint>.azureedge.net/<folder>/<file>`
 
   Bijvoorbeeld:   
   `https://demoendpoint.azureedge.net/test/demo.jpg`
       
   Houd er rekening mee dat iedereen, ongeacht of ze een SAS-token gebruiken toegang heeft tot een CDN-eindpunt. 

3. De Cacheduur verfijnen met behulp van de cache in regels of door toe te voegen `Cache-Control` headers aan de oorsprong. Omdat de CDN het SAS-token als een gewone queryreeks behandelt, als best practice moet u instellen een cache in duur dat op of vóór de verlooptijd van de SAS verloopt. Anders als een bestand is in de cache voor een langere duur dan de SAS actief is, het bestand mogelijk toegankelijk is vanaf de CDN-bronserver nadat de verlooptijd van de SAS is verstreken. Als dit het geval is en u wilt uw cache-bestand niet toegankelijk te maken, moet u een opschonen-bewerking op het bestand naar deze uit de cache wissen uitvoeren. Zie voor meer informatie over het instellen van de Cacheduur van de op de CDN [besturingselement Azure Content Delivery Network cachegedrag met caching regels](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Optie 3: Tokenverificatie CDN-beveiliging gebruiken met een regel herschrijven

Deze optie is de veiligste en aanpasbare. CDN-beveiliging token om verificatie te gebruiken, hebt u een **Azure CDN Premium van Verizon** profiel. Toegang van clients is gebaseerd op de beveiligingsparameters ingesteld voor het beveiligingstoken CDN. Echter, als de SAS ongeldig is, de CDN niet mogelijk voor het valideren van de inhoud op de bronserver.

1. [Maken van een beveiligingstoken CDN](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) en activeer deze met behulp van de regelengine voor voor de CDN-eindpunt en het pad waar uw gebruikers toegang krijgen tot het bestand.

   Een SAS-URL heeft de volgende indeling:   
   `https://<endpoint>.azureedge.net/<folder>/<file>?sv=<SAS_TOKEN>`
 
   Bijvoorbeeld:   
   ```
   https://demoendpoint.azureedge.net/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   De parameteropties voor een tokenverificatie voor CDN-beveiliging zijn anders dan de parameteropties voor een SAS-token. Als u gebruiken een verlooptijd wanneer u een beveiligingstoken CDN maakt wilt, kunt u deze op dezelfde waarde als de verlooptijd van de SAS-token instellen. Hiermee zorgt u ervoor dat de verlooptijd voorspelbaar is. 
 
2. Gebruik de [regelengine](cdn-rules-engine.md) om een regel herschrijven van URL's voor token-toegang tot alle blobs in de container te maken. Nieuwe regels duren ongeveer 90 minuten worden doorgegeven.

   Deze regel voor het herschrijven van URL's van voorbeeld heeft de volgende patronen:
   
   Bron:   
   `/test/demo.jpg`
   
   Bestemming:   
   `/test/demo.jpg?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D`

   ![Regel voor het herschrijven van CDN URL 's](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)

3. Als u de SAS vernieuwt, kunt u de regel herschrijven van Url's voor het gebruik van het nieuwe SAS-token bijwerken. 

## <a name="sas-parameter-considerations"></a>Overwegingen voor SAS-parameter

Omdat de SAS-parameters zijn niet zichtbaar voor de CDN, kan de CDN functioneert levering op basis van deze niet wijzigen. De parameter gedefinieerde beperkingen zijn van toepassing alleen op aanvragen die de CDN naar de oorspronkelijke server, niet voor aanvragen van de client naar de CDN. Deze onderscheid is belangrijk rekening moet houden bij het instellen van de SAS-parameters. Als deze geavanceerde mogelijkheden vereist zijn en u [optie 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), stel de juiste beperkingen van het CDN-beveiligingstoken.

| De naam van de SAS | Beschrijving |
| --- | --- |
| Starten | De tijd waarop de CDN voor toegang tot de blob-bestand kan beginnen. Als gevolg van de klok leiden tot onjuiste (wanneer een signaal klok binnenkomt op verschillende tijdstippen voor verschillende onderdelen), kies een tijd eerder 15 minuten als u wilt dat de asset onmiddellijk beschikbaar zijn. |
| Einde | De tijd waarna de CDN niet langer toegang de blob-bestand tot. Eerder zijn de bestanden in de cache op de CDN nog steeds toegankelijk. Voor het beheren van de verlooptijd van het bestand de juiste verlooptijd ingesteld voor het beveiligingstoken CDN of opschonen van de asset. |
| Toegestane IP-adressen | Optioneel. Als u **Azure CDN van Verizon**, stel deze parameter in op de bereiken die zijn gedefinieerd in [Azure CDN van Verizon Edge Server IP-adresbereiken](https://msdn.microsoft.com/library/mt757330.aspx). Als u **Azure CDN van Akamai**, u kunt de IP-adresbereiken-parameter niet instellen omdat de IP-adressen niet statisch zijn.|
| Toegestane protocollen | De protocollen toegestaan voor een aanvraag met de account-SAS. De HTTPS-instelling wordt aanbevolen.|

## <a name="see-also"></a>Zie ook
- [Met behulp van handtekeningen voor gedeelde toegang (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Shared Access Signatures, deel 2: Maken en gebruiken van een SAS met Blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Azure Content Delivery Network activa met tokenverificatie beveiligen](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
