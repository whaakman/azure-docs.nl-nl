---
title: Media Services bijwerken na gebruik toegangssleutels voor opslag | Microsoft Docs
description: In dit artikel geeft u informatie over het Media Services bijwerken na gebruik toegangssleutels voor opslag.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: milanga;cenkdin;juliako
ms.openlocfilehash: c688169dc21304f234aead7196f377a3fa5fd633
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58294923"
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a>Media Services bijwerken na gebruik toegangssleutels voor opslag 

Wanneer u een nieuwe Azure Media Services (AMS)-account maakt, kunt u wordt ook gevraagd om te selecteren van een Azure Storage-account dat wordt gebruikt voor het opslaan van uw media-inhoud. U kunt meer dan één storage-accounts toevoegen aan uw Media Services-account. In dit artikel laat zien hoe opslagsleutels draaien. U ziet ook hoe u storage-accounts toevoegen aan een media-account. 

Als u wilt de in dit artikel beschreven acties uitvoeren, moet u [Azure Resource Manager-API's](/rest/api/media/operations/azure-media-services-rest-api-reference) en [Powershell](https://docs.microsoft.com/powershell/module/az.media).  Zie voor meer informatie, [over het beheren van Azure-resources met PowerShell en Resource Manager](../../azure-resource-manager/manage-resource-groups-powershell.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Overzicht

Als u een nieuw opslagaccount maakt, genereert Azure twee 512-bits opslagtoegangssleutels, die worden gebruikt voor het verifiëren van toegang tot uw storage-account. Uw Opslagverbindingen om veiliger te houden, is het aanbevolen om periodiek opnieuw genereren en uw toegangssleutel voor opslag draaien. Twee toegangssleutels (primaire en secundaire) worden geleverd zodat u het onderhouden van verbinding met de storage-account met behulp van één toegangssleutel terwijl u de toegangssleutel opnieuw genereren. Deze procedure wordt ook 'rolling toegangssleutels' genoemd.

Media Services is afhankelijk van een storage-sleutel opgegeven. De locators die worden gebruikt om te streamen of te downloaden van de activa is specifiek, afhankelijk van de opgegeven toegangssleutel. Wanneer een AMS-account wordt gemaakt, het duurt een afhankelijkheid op de primaire toegangssleutel standaard, maar als een gebruiker kunt u de toegangssleutel van het opslagaccount met AMS bijwerken. U moet ervoor zorgen om te laten weten welke sleutel voor gebruik door de volgende stappen in dit artikel beschreven mediaservices.  

>[!NOTE]
> Als u meerdere opslagaccounts hebt, kunt u deze procedure met de storage-account wilt uitvoeren. De volgorde waarin u de opslagsleutels draaien is niet opgelost. U kunt de secundaire sleutel en vervolgens de primaire sleutel of vice versa draaien.
>
> Zorg dat u ze op een Pre-productie-account testen alvorens de stappen in dit artikel wordt beschreven in een productieaccount.
>

## <a name="steps-to-rotate-storage-keys"></a>Stappen voor het opslagsleutels draaien 
 
 1. Wijzigen van de primaire sleutel uit de storage-account via de powershell-cmdlet of [Azure](https://portal.azure.com/) portal.
 2. Aanroepen van de cmdlet Sync-AzMediaServiceStorageKeys met de juiste parameters om af te dwingen van media-account om op te halen opslagaccountsleutels
 
    Het volgende voorbeeld laat zien hoe om te synchroniseren van sleutels voor storage-accounts.
  
         Sync-AzMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. Wacht een uur. Controleer of dat de streaming-scenario's werken.
 4. Secundaire sleutel van het opslagaccount via de powershell-cmdlet of Azure-portal wijzigen.
 5. Synchronisatie-AzMediaServiceStorageKeys powershell met de juiste parameters om af te dwingen van media-account om op te halen nieuwe opslagaccountsleutels aanroepen. 
 6. Wacht een uur. Controleer of dat de streaming-scenario's werken.
 
### <a name="a-powershell-cmdlet-example"></a>Een powershell-cmdlet-voorbeeld 

Het volgende voorbeeld laat zien hoe u het opslagaccount ophaalt en deze synchroniseren met de AMS-account.

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a>Stappen voor het storage-accounts toevoegen aan uw AMS-account

Het volgende artikel laat zien hoe storage-accounts toevoegen aan uw AMS-account: [Meerdere opslagaccounts koppelen aan een Media Services-account](meda-services-managing-multiple-storage-accounts.md).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>Bevestigingen
Wij willen graag om te bevestigen van de volgende personen die heeft bijgedragen tot het maken van dit document: Cenk Dingiloglu, Milan Gada, Seva Titov.
