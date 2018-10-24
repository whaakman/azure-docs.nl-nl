---
title: Azure CDN gebruiken met SAS | Microsoft Docs
description: Azure CDN biedt ondersteuning voor het gebruik van Shared Access Signature (SAS) beperkt toegang tot persoonlijke storage-containers.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: magattus
ms.openlocfilehash: 57891bcce289c30d7dce1cd00c301064aa9b97cc
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49955232"
---
# <a name="using-azure-cdn-with-sas"></a>Azure CDN gebruiken met SAS

Bij het instellen van een storage-account voor Azure Content Delivery Network (CDN) met inhoud van de cache, standaard iedereen weet de URL's voor uw storage-containers dat hebben toegang tot de bestanden die u hebt geüpload. Ter bescherming van de bestanden in uw storage-account, kunt u de toegang tot uw storage-containers uit openbare naar persoonlijke instellen. Als u dit doet, wordt er wel toegang tot uw bestanden. 

Als u beperkte toegang tot persoonlijke opslagcontainers wilt verlenen, kunt u de Shared Access Signature (SAS)-functie van uw Azure-opslagaccount gebruiken. Een SAS is een URI die beperkte toegangsrechten verleent aan uw Azure Storage-resources zonder uw accountsleutel prijs te geven. U kunt een SAS opgeven aan clients die u niet vertrouwt met uw opslagaccountsleutel, maar aan wie u wilt toegang tot bepaalde resources opslagaccount delegeren. Door het distribueren van een shared access signature URI voor deze clients hun toegang wordt verleend aan een resource voor een opgegeven periode.
 
U kunt de verschillende parameters van toegang naar een blob, zoals de begin- en tijden, machtigingen (lezen/schrijven) en IP-bereiken definiëren met een SAS. In dit artikel wordt beschreven hoe u SAS gebruiken in combinatie met Azure CDN. Zie voor meer informatie over SAS, waaronder het maken van deze en de bijbehorende parameteropties [Using shared access signatures (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Instellen van Azure CDN voor het werken met SAS-opslag
De volgende drie opties worden aanbevolen voor SAS gebruiken met Azure CDN. Alle opties wordt ervan uitgegaan dat u een werkende SAS (Zie vereisten) al hebt gemaakt. 
 
### <a name="prerequisites"></a>Vereisten
Als u wilt starten, een opslagaccount maken en vervolgens een SAS genereren voor uw asset. U kunt twee soorten opgeslagen toegangshandtekeningen genereren: een service-SAS of een account-SAS. Zie voor meer informatie, [typen handtekeningen voor gedeelde toegang](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures).

Nadat u een SAS-token hebt gegenereerd, kunt u uw blob storage-bestand openen door toe te voegen `?sv=<SAS token>` naar uw URL. Deze URL heeft de volgende indeling: 

`https://<account name>.blob.core.windows.net/<container>/<file>?sv=<SAS token>`
 
Bijvoorbeeld:
 ```
https://democdnstorage1.blob.core.windows.net/container1/demo.jpg?sv=2017-07-29&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Zie voor meer informatie over parameters van de instelling [SAS parameter overwegingen](#sas-parameter-considerations) en [Shared access signature parameters](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#shared-access-signature-parameters).

![CDN SAS-instellingen](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-azure-cdn"></a>Optie 1: Met behulp van SAS met Pass Through-query naar blob-opslag van Azure CDN

Deze optie is de eenvoudigste en maakt gebruik van een enkele SAS-token die vanuit Azure CDN wordt doorgegeven aan de oorspronkelijke server. Dit wordt ondersteund door **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen. 
 
1. Selecteer een eindpunt **regels voor Caching**en selecteer vervolgens **elke unieke URL in de Cache** uit de **Query queryreeksen opslaan in cache** lijst.

    ![CDN caching-regels](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Nadat u de SAS hebt ingesteld in uw storage-account, moet u de SAS-token met de CDN-eindpunt en de oorsprong server URL's gebruiken voor toegang tot het bestand. 
   
   De resulterende URL van het CDN-eindpunt heeft de volgende indeling: `https://<endpoint hostname>.azureedge.net/<container>/<file>?sv=<SAS token>`

   Bijvoorbeeld:   
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg/?sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. De Cacheduur verfijnen met behulp van regels voor opslaan in cache of door toe te voegen `Cache-Control` headers op de oorspronkelijke server. Omdat Azure CDN het SAS-token als een gewone queryreeks behandelt, als een best practice moet u instellen een Cacheduur die op of vóór de verlooptijd van de SAS is verlopen. Anders als een bestand wordt opgeslagen in de cache gedurende een langere periode dan de SAS actief is, het bestand mogelijk toegankelijk is vanaf de server van Azure CDN origin nadat de verlooptijd van de SAS is verstreken. Als deze situatie doet zich voor, en u ervoor dat uw bestand in de cache niet toegankelijk, kunt u een bewerking opschonen van Logboeken op het bestand om het uit de cache wissen moet uitvoeren. Zie voor meer informatie over het instellen van de Cacheduur van de op Azure CDN [Control Azure CDN caching-gedrag met regels voor caching](cdn-caching-rules.md).

### <a name="option-2-hidden-cdn-sas-token-using-a-rewrite-rule"></a>Optie 2: Verborgen CDN SAS-token met behulp van een herschrijvingsregel voor
 
Deze optie is alleen beschikbaar voor **Azure CDN Premium van Verizon** profielen. Met deze optie kunt u de blob-opslag op de oorspronkelijke server beveiligen. U wilt gebruiken met deze optie als u geen specifieke beperkingen voor het bestand hoeft, maar u wilt voorkomen dat gebruikers toegang hebben tot de opslag-oorsprong rechtstreeks ter verbetering van Azure CDN-offload tijden. De SAS-token voor de gebruiker onbekend is, is vereist voor iedereen toegang tot bestanden in de opgegeven container van de oorspronkelijke server. Echter, vanwege de regel herschrijven van URL's, het SAS-token is niet vereist op het CDN-eindpunt.
 
1. Gebruik de [regels-engine](cdn-rules-engine.md) om een regel herschrijven van URL's te maken. Nieuwe regels worden vier uur worden doorgegeven.

   ![Knop CDN beheren](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![Knop voor CDN regels-engine](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Het volgende voorbeeld herschrijven van URL's-regel maakt gebruik van een reguliere-expressiepatroon met een groep vastleggen en een eindpunt met de naam *sasstoragedemo*:
   
   Bron:   
   `(container1\/.*)`
   
   Bestemming:   
   ```
   $1?sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   ![CDN URL Rewrite regel - links](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
   ![regel CDN URL Rewrite - rechts](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-4.png)

2. Nadat de nieuwe regel geactiveerd wordt, kan iedereen toegang krijgen tot de bestanden in de opgegeven container op het CDN-eindpunt, ongeacht of dat een SAS-token wordt gebruikt in de URL. Hier volgt de indeling: `https://<endpoint hostname>.azureedge.net/<container>/<file>`
 
   Bijvoorbeeld:   
   `https://sasstoragedemo.azureedge.net/container1/demo.jpg`
       

3. De Cacheduur verfijnen met behulp van regels voor opslaan in cache of door toe te voegen `Cache-Control` headers op de oorspronkelijke server. Omdat Azure CDN het SAS-token als een gewone queryreeks behandelt, als een best practice moet u instellen een Cacheduur die op of vóór de verlooptijd van de SAS is verlopen. Anders als een bestand wordt opgeslagen in de cache gedurende een langere periode dan de SAS actief is, het bestand mogelijk toegankelijk is vanaf de server van Azure CDN origin nadat de verlooptijd van de SAS is verstreken. Als deze situatie doet zich voor, en u ervoor dat uw bestand in de cache niet toegankelijk, kunt u een bewerking opschonen van Logboeken op het bestand om het uit de cache wissen moet uitvoeren. Zie voor meer informatie over het instellen van de Cacheduur van de op Azure CDN [Control Azure CDN caching-gedrag met regels voor caching](cdn-caching-rules.md).

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Optie 3: Tokenverificatie voor CDN-beveiliging gebruiken met een herschrijvingsregel voor

Azure CDN security token om verificatie te gebruiken, hebt u een **Azure CDN Premium van Verizon** profiel. Deze optie is de veiligste en kan worden aangepast. Clienttoegang is gebaseerd op de beveiligingsparameters die u op het beveiligingstoken instelt. Nadat u hebt gemaakt en instellen van het beveiligingstoken, wordt het vereist op alle CDN-eindpunt-URL's. Echter, vanwege de regel herschrijven van URL's, het SAS-token is niet vereist op het CDN-eindpunt. Als de SAS-token later ongeldig wordt, is Azure CDN niet langer kunnen de inhoud van de oorspronkelijke server te valideren.

1. [Maken van een Azure CDN security token](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) en activeer deze met behulp van de regelengine voor het CDN-eindpunt en het pad waar uw gebruikers hebben toegang tot het bestand.

   Een security token-eindpunt-URL heeft de volgende indeling:   
   `https://<endpoint hostname>.azureedge.net/<container>/<file>?<security_token>`
 
   Bijvoorbeeld:   
   ```
   https://sasstoragedemo.azureedge.net/container1/demo.jpg?a4fbc3710fd3449a7c99986bkquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   De parameteropties voor een token beveiligingsverificatie zijn anders dan de parameteropties voor een SAS-token. Als u ervoor een verlooptijd bij het maken van een beveiligingstoken te gebruiken kiest, moet u deze ingesteld op dezelfde waarde als de verlooptijd van het SAS-token. Hiermee zorgt u ervoor dat de verlooptijd voorspelbare is. 
 
2. Gebruik de [regels-engine](cdn-rules-engine.md) om een regel herschrijven van URL's voor SAS-token toegang tot alle blobs in de container te maken. Nieuwe regels worden vier uur worden doorgegeven.

   Het volgende voorbeeld herschrijven van URL's-regel maakt gebruik van een reguliere-expressiepatroon met een groep vastleggen en een eindpunt met de naam *sasstoragedemo*:
   
   Bron:   
   `(container1\/.*)`
   
   Bestemming:   
   ```
   $1&sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   ![CDN URL Rewrite regel - links](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
   ![regel CDN URL Rewrite - rechts](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-4.png)

3. Als u de SAS vernieuwt, zorgt u ervoor dat u de regel herschrijven van Url's met de nieuwe SAS-token bijwerken. 

## <a name="sas-parameter-considerations"></a>Overwegingen met betrekking tot SAS-parameter

Omdat het SAS-parameters zijn niet zichtbaar voor Azure CDN, kan de van leveringsgedrag op basis van deze Azure CDN niet wijzigen. De gedefinieerde parameter beperkingen van toepassing alleen op aanvragen die naar de oorspronkelijke server, niet voor aanvragen van de client naar het Azure CDN kunt u Azure CDN. Dit verschil is het belangrijk om te overwegen bij het instellen van SAS-parameters. Als deze geavanceerde mogelijkheden vereist zijn en u [optie 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule), de juiste beperkingen instellen voor het Azure CDN-beveiligingstoken.

| Naam van de SAS | Beschrijving |
| --- | --- |
| Starten | Het tijdstip waarop Azure CDN beginnen kunt met het toegang krijgen tot de blobbestand. Vanwege de klok scheeftrekken (wanneer een kloksignaal binnenkomt op verschillende tijdstippen voor verschillende onderdelen), kiest u een periode van 15 minuten eerder als u wilt dat de asset onmiddellijk beschikbaar zijn. |
| Einde | De tijd waarna Azure CDN geen toegang meer de blob-bestand tot kan. Eerder zijn de bestanden in de cache op Azure CDN nog steeds toegankelijk. Voor het beheren van de verlooptijd van het bestand, instellen van de juiste verlooptijd op het Azure CDN-beveiligingstoken of opschonen van de asset. |
| Toegestane IP-adressen | Optioneel. Als u **Azure CDN van Verizon**, kunt u deze parameter instellen op de bereiken die zijn gedefinieerd [Azure CDN van Verizon Edge-Server IP-adresbereiken](https://msdn.microsoft.com/library/mt757330.aspx). Als u **Azure CDN van Akamai**, u kunt de IP-adresbereiken parameter niet instellen omdat de IP-adressen niet statisch zijn.|
| Toegestane protocollen | De netwerkprotocollen die zijn toegestaan voor een aanvraag voor de account-SAS. De HTTPS-instelling wordt aanbevolen.|

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over SAS:
- [Shared Access Signatures (SAS) gebruiken](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Shared Access Signatures, deel 2: Maken en een SAS gebruiken met Blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)

Zie voor meer informatie over het instellen van tokenverificatie [Securing Azure Content Delivery Network assets met tokenverificatie](https://docs.microsoft.com/azure/cdn/cdn-token-auth).
