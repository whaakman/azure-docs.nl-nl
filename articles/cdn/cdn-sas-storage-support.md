---
title: Azure CDN gebruiken met SAS | Microsoft Docs
description: Azure CDN ondersteunt het gebruik van Shared Access Signature (SAS) beperkte toegang tot persoonlijke storage-containers.
services: cdn
documentationcenter: ''
author: dksimpson
manager: ''
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: v-deasim
ms.openlocfilehash: 09efd5cd54fbd05d85939b3ae08bfbb37e91058d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="using-azure-cdn-with-sas"></a>Azure CDN gebruiken met SAS

Bij het instellen van een opslagaccount voor Azure inhoud Delivery Network (CDN) met inhoud van de cache standaard iedereen die de URL's voor uw storage-containers kent hebben toegang tot de bestanden die u hebt geüpload. Ter bescherming van de bestanden in uw storage-account, kunt u de toegang van uw storage-containers van openbaar naar privéclouds instellen. Echter, als u doet dit, niemand zich toegang tot uw bestanden. 

Als u beperkte toegang tot persoonlijke storage-containers wilt, kunt u de Shared Access Signature (SAS)-functie van uw Azure storage-account. Een SAS is een URI die verleent rechten aan uw Azure Storage-resources beperkte zonder dat de sleutel van uw account. U kunt een SAS bieden aan clients die u niet vertrouwt door de sleutel van uw opslagaccount, maar aan wie u wilt toegang tot bepaalde resources storage account delegeren. Door het distribueren van een shared access signature URI voor deze clients verlenen u toegang tot een bron voor een opgegeven periode.
 
U kunt verschillende parameters van toegang naar een blob, zoals begin-en verloopdatum machtigingen (lezen/schrijven) en IP-adresbereiken definiëren met een SAS. In dit artikel wordt beschreven hoe SAS gebruiken in combinatie met Azure CDN. Zie voor meer informatie over SAS, waaronder het maken van deze en de bijbehorende parameteropties [Using shared access signatures (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Instellen van Azure CDN voor het werken met SAS-opslag
De volgende drie opties worden aanbevolen voor het gebruik van SAS met Azure CDN. Alle opties wordt ervan uitgegaan dat u al een werkende SAS (Zie vereisten) hebt gemaakt. 
 
### <a name="prerequisites"></a>Vereisten
Als u wilt starten, een opslagaccount maken en vervolgens een SAS genereren voor uw asset. U kunt twee soorten handtekeningen opgeslagen toegang genereren: een service-SAS of een SAS-account. Zie voor meer informatie [typen handtekeningen voor gedeelde toegang](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Nadat u een SAS-token hebt gegenereerd, kunt u uw blob-opslag-bestand openen door toe te voegen `?sv=<SAS token>` naar uw URL. Deze URL heeft de volgende indeling: 

`https://<account name>.blob.core.windows.net/<container>/<file>?sv=<SAS token>`
 
Bijvoorbeeld:
 ```
https://democdnstorage1.blob.core.windows.net/container1/demo.jpg?sv=2017-04-17&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Zie voor meer informatie over parameters die instelling [SAS parameter overwegingen](#sas-parameter-considerations) en [Shared access signature parameters](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![CDN SAS-instellingen](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-azure-cdn"></a>Optie 1: Via SAS met pass-through naar blob storage van Azure CDN

Deze optie is de eenvoudigste en maakt gebruik van een enkele SAS-token, dat van Azure CDN is doorgegeven aan de bronserver. Dit wordt ondersteund door **Azure CDN van Verizon** en **Azure CDN van Akamai**. 
 
1. Een eindpunt selecteert, selecteert u **regels opslaan in cache**, selecteer daarna **elke unieke URL in de Cache** van de **Query opslaan in cache** lijst.

    ![In het cachegeheugen CDN-regels](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Na het instellen van SAS van uw opslagaccount, moet u het SAS-token met de CDN-eindpunt en de bron-URL's gebruiken voor toegang tot het bestand. 
   
   De resulterende CDN-eindpunt-URL heeft de volgende indeling: `https://<endpoint hostname>.azureedge.net/<container>/<file>?sv=<SAS token>`

   Bijvoorbeeld:   
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg/?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. De Cacheduur verfijnen met behulp van de cache in regels of door toe te voegen `Cache-Control` headers op de bronserver. Omdat Azure CDN het SAS-token als een gewone queryreeks behandelt, als best practice moet u instellen een cache in duur dat op of vóór de verlooptijd van de SAS verloopt. Anders als een bestand is in de cache voor een langere duur dan de SAS actief is, het bestand mogelijk toegankelijk is vanaf de bronserver Azure CDN nadat de verlooptijd van de SAS is verstreken. Als deze situatie en u wilt uw cache-bestand niet toegankelijk te maken, moet u een opschonen-bewerking op het bestand naar deze uit de cache wissen uitvoeren. Zie voor meer informatie over het instellen van de Cacheduur van de op Azure CDN [besturingselement Azure CDN cachegedrag met caching regels](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-sas-token-using-a-rewrite-rule"></a>Optie 2: Verborgen CDN SAS-token met behulp van een regel herschrijven
 
Deze optie is alleen beschikbaar voor **Azure CDN Premium van Verizon** profielen. Met deze optie kunt u de blob-opslag op de oorspronkelijke server beveiligen. U wilt Gebruik deze optie als u specifieke toegangsbeperkingen voor het bestand niet nodig, maar u wilt voorkomen dat gebruikers toegang krijgen tot de oorsprong opslag rechtstreeks ter verbetering van Azure CDN offload tijden. De SAS-token onbekend bij de gebruiker is, is vereist voor iedereen toegang krijgen tot bestanden in de opgegeven container van de bronserver. Echter, vanwege de regel herschrijven van URL's het SAS-token is niet vereist op het CDN-eindpunt.
 
1. Gebruik de [regelengine](cdn-rules-engine.md) om een regel voor het herschrijven van URL's te maken. Nieuwe regels duren ongeveer 90 minuten worden doorgegeven.

   ![Knop CDN beheren](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Knop CDN regels-engine](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Het volgende voorbeeld herschrijven van URL's regel maakt gebruik van een reguliere-expressiepatroon met een vastgelegde groep en een eindpunt met de naam *storagedemo*:
   
   Bron:   
   `(/test/.*)`
   
   Bestemming:   
   ```
   $1?sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```

   ![Regel voor het herschrijven van CDN URL 's](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-2.png)

2. Nadat de nieuwe regel geactiveerd wordt, kan iedereen toegang tot bestanden in de opgegeven container van het CDN-eindpunt, ongeacht of ze een SAS-token in de URL gebruiken. Dit is de indeling: `https://<endpoint hostname>.azureedge.net/<container>/<file>`
 
   Bijvoorbeeld:   
   `https://demoendpoint.azureedge.net/container1/demo.jpg`
       

3. De Cacheduur verfijnen met behulp van de cache in regels of door toe te voegen `Cache-Control` headers op de bronserver. Omdat Azure CDN het SAS-token als een gewone queryreeks behandelt, als best practice moet u instellen een cache in duur dat op of vóór de verlooptijd van de SAS verloopt. Anders als een bestand is in de cache voor een langere duur dan de SAS actief is, het bestand mogelijk toegankelijk is vanaf de bronserver Azure CDN nadat de verlooptijd van de SAS is verstreken. Als deze situatie en u wilt uw cache-bestand niet toegankelijk te maken, moet u een opschonen-bewerking op het bestand naar deze uit de cache wissen uitvoeren. Zie voor meer informatie over het instellen van de Cacheduur van de op Azure CDN [besturingselement Azure CDN cachegedrag met caching regels](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Optie 3: Tokenverificatie CDN-beveiliging gebruiken met een regel herschrijven

Azure CDN security token om verificatie te gebruiken, hebt u een **Azure CDN Premium van Verizon** profiel. Deze optie is de veiligste en aanpasbare. Toegang van clients is gebaseerd op de beveiligingsparameters die u op het beveiligingstoken instelt. Nadat u hebt gemaakt en instellen van het beveiligingstoken, deze moet op alle CDN-eindpunt-URL's. Echter, vanwege de regel herschrijven van URL's het SAS-token is niet vereist op het CDN-eindpunt. Als het SAS-token later ongeldig wordt, is Azure CDN niet langer kunnen valideren van de inhoud op de bronserver.

1. [Maken van een beveiligingstoken Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) en activeer deze met behulp van de regelengine voor voor de CDN-eindpunt en het pad waar uw gebruikers toegang krijgen tot het bestand.

   Een security token eindpunt-URL heeft de volgende indeling:   
   `https://<endpoint hostname>.azureedge.net/<container>/<file>?<security_token>`
 
   Bijvoorbeeld:   
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg?a4fbc3710fd3449a7c99986bkquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   De parameteropties voor een tokenverificatie voor beveiliging zijn anders dan de parameteropties voor een SAS-token. Als u een verlooptijd bij het maken van een beveiligingstoken gebruiken kiest, moet u deze instellen op dezelfde waarde als de verlooptijd van de SAS-token. Hiermee zorgt u ervoor dat de verlooptijd voorspelbaar is. 
 
2. Gebruik de [regelengine](cdn-rules-engine.md) om een regel herschrijven van URL's voor SAS-token toegang tot alle blobs in de container te maken. Nieuwe regels duren ongeveer 90 minuten worden doorgegeven.

   Het volgende voorbeeld herschrijven van URL's regel maakt gebruik van een reguliere-expressiepatroon met een vastgelegde groep en een eindpunt met de naam *storagedemo*:
   
   Bron:   
   `(/test/.*)`
   
   Bestemming:   
   ```
   $1&sv=2017-04-17&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```

   ![Regel voor het herschrijven van CDN URL 's](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-3.png)

3. Als u de SAS verlengt, zorg ervoor dat u de regel herschrijven van Url's met het nieuwe SAS-token bijwerken. 

## <a name="sas-parameter-considerations"></a>Overwegingen voor SAS-parameter

Omdat de SAS-parameters zijn niet zichtbaar voor Azure CDN, kan de van leveringsgedrag op basis van deze Azure CDN niet wijzigen. De parameter gedefinieerde beperkingen zijn van toepassing alleen op aanvragen die door Azure CDN doorgevoerd op de bronserver, niet voor aanvragen van de client voor Azure CDN. Deze onderscheid is belangrijk rekening moet houden bij het instellen van SAS-parameters. Als deze geavanceerde mogelijkheden vereist zijn en u [optie 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), de juiste beperkingen ingesteld voor het beveiligingstoken Azure CDN.

| De naam van de SAS | Beschrijving |
| --- | --- |
| Starten | De tijd waarop Azure CDN beginnen kunt met het toegang krijgen tot de blob-bestand. Als gevolg van de klok leiden tot onjuiste (wanneer een kloksignaal binnenkomt op verschillende tijdstippen voor verschillende onderdelen), kies een tijd eerder 15 minuten als u wilt dat de asset onmiddellijk beschikbaar zijn. |
| Einde | De tijd waarna Azure CDN niet langer toegang de blob-bestand tot. Eerder zijn de bestanden in de cache op Azure CDN nog steeds toegankelijk. Voor het beheren van de verlooptijd van het bestand de juiste verlooptijd ingesteld voor het beveiligingstoken Azure CDN of opschonen van de asset. |
| Toegestane IP-adressen | Optioneel. Als u **Azure CDN van Verizon**, stel deze parameter in op de bereiken die zijn gedefinieerd in [Azure CDN van Verizon Edge Server IP-adresbereiken](https://msdn.microsoft.com/library/mt757330.aspx). Als u **Azure CDN van Akamai**, u kunt de IP-adresbereiken-parameter niet instellen omdat de IP-adressen niet statisch zijn.|
| Toegestane protocollen | De protocollen toegestaan voor een aanvraag met de account-SAS. De HTTPS-instelling wordt aanbevolen.|

## <a name="see-also"></a>Zie ook
- [Met behulp van handtekeningen voor gedeelde toegang (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Shared Access Signatures, deel 2: Maken en gebruiken van een SAS met Blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)
- [Azure Content Delivery Network activa met tokenverificatie beveiligen](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
