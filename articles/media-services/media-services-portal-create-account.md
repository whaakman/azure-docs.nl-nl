---
title: " Een Azure Media Services-account maken via de Azure Portal | Microsoft Docs"
description: In deze zelfstudie wordt stapsgewijs uitgelegd hoe u een Azure Media Services-account maakt via de Azure Portal.
services: media-services
documentationcenter: ''
author: Juliako
manager: erikre
editor: ''

ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/07/2016
ms.author: juliako

---
# Een Azure Media Services-account maken via de Azure Portal
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)
> 
> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
> 
> 

De Azure Portal biedt een manier om snel een Azure Media Services-account (AMS) te maken. U kunt uw account gebruiken voor toegang tot Media Services waarmee u media-inhoud in Azure kunt opslaan, versleutelen, coderen, beheren en streamen. Op het moment dat u een Media Services-account maakt, maakt u ook een bijbehorend opslagaccount (of gebruikt u een bestaand account) in hetzelfde geografische gebied als het Media Services-account.

In dit artikel worden enkele algemene concepten toegelicht en wordt uitgelegd hoe u een Media Services-account maakt via de Azure Portal.

## Concepten
Voor toegang tot Media Services zijn twee gekoppelde accounts vereist: 

* Een Media Services-account. Met uw account hebt u toegang tot een set Media Services in de cloud die beschikbaar zijn in Azure. Er wordt geen echte media-inhoud opgeslagen in een Media Services-account. In plaats daarvan worden de metagegevens over de media-inhoud en taken voor de verwerking van media opgeslagen in uw account. Op het moment dat u het account maakt, selecteert u een beschikbare Media Services-regio. De regio die u selecteert, is een datacenter waarin de records met metagegevens voor uw account worden opgeslagen.
  
    Beschikbare Media Services-regio's (AMS) zijn onder meer: Noord-Europa, West-Europa, VS - west, VS - oost, Zuidoost-Azië, Azië, Japan - west, Japan - oost. Media Services gebruikt geen affiniteitsgroepen.
  
    AMS is nu ook beschikbaar in de volgende datacenters: Brazilië - zuid, India, westen, India - zuid en India, midden. U kunt de Azure Portal nu gebruiken om Media Service-accounts te maken en diverse taken uit te voeren die hier worden beschreven. Live Encoding is echter niet ingeschakeld in deze datacenters. Bovendien zijn niet alle soorten gereserveerde coderingseenheden beschikbaar in deze datacenters.
  
  * Brazilië - zuid: alleen Standard en Basic gereserveerde coderingseenheden zijn beschikbaar.
  * India - west, India - zuid: geef Storage-blobs op voor mediabestanden; opslagaccounts moeten zich in dezelfde geografische regio bevinden als het Media Services-account. Wanneer u een Media Services-account maakt, kunt u een bestaand opslagaccount in dezelfde regio kiezen. U kunt ook een nieuw opslagaccount maken in dezelfde regio. Als u een Media Services-account verwijdert, worden de blobs in uw gerelateerde opslagaccount niet verwijderd.

## Een AMS-account maken
In de stappen in deze sectie wordt uitgelegd hoe u een AMS-account maakt.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Klik op **+Nieuw** > **Media + CDN** > **Mediaservices**.
   
    ![Media Services-account maken](./media/media-services-portal-vod-get-started/media-services-new1.png)
3. Voer bij **MEDIA SERVICES-ACCOUNT MAKEN** de vereiste waarden in.
   
    ![Media Services-account maken](./media/media-services-portal-vod-get-started/media-services-new3.png)
   
   1. Voer in **Accountnaam** de naam van het nieuwe AMS-account in. Voor de naam van een Media Services-account mogen alleen cijfers of kleine letters zonder spaties worden gebruikt. De naam mag 3 tot 24 tekens lang zijn.
   2. Selecteer in Abonnement een van de verschillende Azure-abonnementen waartoe u toegang hebt.
   3. Selecteer in **Resourcegroep** de nieuwe of bestaande resource.  Een resourcegroep is een verzameling resources met dezelfde levenscyclus, dezelfde machtigingen en hetzelfde beleid. Klik [hier](../resource-group-overview.md#resource-groups) voor meer informatie.
   4. Selecteer in **Locatie** de geografische regio die wordt gebruikt om de media en metagegevensrecords voor uw Media Services-account op te slaan. Deze regio wordt gebruikt om uw media te verwerken en te streamen. Alleen de beschikbare Media Services-regio's worden in de vervolgkeuzelijst weergegeven. 
   5. Selecteer bij **Opslagaccount** een opslagaccount om Blob Storage van de media-inhoud vanaf uw Media Services-account te leveren. U kunt een bestaand opslagaccount selecteren in dezelfde geografische regio als uw Media Services-account of u kunt een opslagaccount maken. Een nieuw opslagaccount wordt in dezelfde regio gemaakt. De regels voor opslagaccountnamen zijn hetzelfde als voor Media Services-accounts.
      
       Klik [hier](../storage/storage-introduction.md) voor meer informatie over opslag.
   6. Selecteer **Vastmaken aan dashboard** om de voortgang van de implementatie van het account te bekijken.
4. Klik op **Maken** onder in het formulier.
   
    Als het account is gemaakt, wordt de status gewijzigd in **Actief**. 
   
    ![Media Services-instellingen](./media/media-services-portal-vod-get-started/media-services-settings.png)
   
    Als u uw AMS-account wilt beheren (bijvoorbeeld video's uploaden, assets coderen, de voortgang van een taak bewaken), gebruikt u het venster **Instellingen**.

## Sleutels beheren
U hebt de accountnaam en de primaire-sleutelgegevens nodig om programmatisch toegang te krijgen tot het Media Services-account.

1. Selecteer uw account in de Azure Portal. 
   
    Het venster **Instellingen** wordt aan de rechterkant weergegeven. 
2. Selecteer in het venster **Instellingen** de optie **Sleutels**. 
   
    In het venster **Sleutels beheren** worden de accountnaam en de primaire en secundaire sleutel weergegeven. 
3. Klik op de knop Kopiëren om de waarden te kopiëren.
   
    ![Media Services-sleutels](./media/media-services-portal-vod-get-started/media-services-keys.png)

## Volgende stappen
U kunt nu bestanden uploaden naar uw AMS-account. Zie [Bestanden uploaden](media-services-portal-upload-files.md) voor meer informatie.

## Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!--HONumber=Sep16_HO3-->


