---
title: Media Services bijwerken nadat u hebt opslagtoegangssleutels rolling | Microsoft Docs
description: In dit artikel geeft richtlijnen over het bijwerken van Media Services na rolling toegangssleutels voor opslag.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: milanga;cenkdin;juliako
ms.openlocfilehash: 304e72e0d2d4a7e95df513e6d5481def9eae3f68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a>Media Services bijwerken na rolling toegangssleutels voor opslag

Wanneer u een nieuw Azure Media Services (AMS)-account maakt, kunt u wordt ook gevraagd om te selecteren van een Azure Storage-account dat wordt gebruikt voor het opslaan van uw media-inhoud. U kunt meer dan één storage-accounts toevoegen aan uw Media Services-account. Dit onderwerp wordt beschreven hoe opslagsleutels draaien. Ook ziet u hoe storage-accounts toevoegen aan een media-account. 

Als u de acties beschreven in dit onderwerp, moet u [ARM API's](https://docs.microsoft.com/rest/api/media/mediaservice) en [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).  Zie voor meer informatie [hoe Azure-resources beheren met PowerShell en Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).

## <a name="overview"></a>Overzicht

Wanneer een nieuw opslagaccount is gemaakt, genereert Azure twee 512-bits opslagtoegangssleutels, die worden gebruikt voor het verifiëren van toegang tot uw storage-account. Uw Opslagverbindingen meer om veilig te houden, is het aanbevolen periodiek opnieuw genereren en draaien van uw toegangssleutel voor opslag. Twee toegangssleutels (primair en secundair) worden opgegeven zodat u verbindingen met het opslagaccount met behulp van één toegangssleutel terwijl u de toegangssleutel opnieuw genereert onderhouden. Deze procedure is een afkorting voor 'toegangssleutels rolling'.

Media Services is afhankelijk van een opslagsleutel opgegeven. De locators die worden gebruikt om te streamen of downloaden van de activa is specifiek, afhankelijk van de toegangssleutel voor opslag opgegeven. Wanneer een AMS-account is gemaakt die een afhankelijkheid op de primaire opslag toegangssleutel nodig standaard maar als een gebruiker kunt u de opslagsleutel met AMS bijwerken. U moet controleren om te laten weten welke sleutel moet worden gebruikt door de volgende stappen in dit onderwerp beschreven Media-Services.  

>[!NOTE]
> Als u meerdere opslagaccounts hebt, kunt u deze procedure met elk opslagaccount wilt uitvoeren. De volgorde waarin u opslagsleutels draaien is niet opgelost. U kunt de secundaire sleutel en vervolgens de primaire sleutel of vice versa draaien.
>
> Zorg dat u op een account vóór productie te testen voordat het uitvoeren van de stappen die worden beschreven in dit onderwerp voor een productie-account.
>

## <a name="steps-to-rotate-storage-keys"></a>Stappen voor het opslagsleutels draaien 
 
 1. Wijzigen van de primaire sleutel van storage-account via de powershell-cmdlet of [Azure](https://portal.azure.com/) portal.
 2. Roep de cmdlet Sync AzureRmMediaServiceStorageKeys met de juiste parameters om af te dwingen media-account om op te halen toegangscodes voor opslag
 
    Het volgende voorbeeld laat zien hoe sleutels naar storage-accounts synchroniseren.
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. Wacht een uur. Controleer of dat de streaming-scenario's werken.
 4. Secundaire opslagaccountsleutel via de powershell-cmdlet of Azure-portal wijzigen.
 5. Aanroepen Sync AzureRmMediaServiceStorageKeys powershell met de juiste parameters om af te dwingen media-account om op te halen nieuwe toegangscodes voor opslag. 
 6. Wacht een uur. Controleer of dat de streaming-scenario's werken.
 
### <a name="a-powershell-cmdlet-example"></a>Een voorbeeld van een powershell-cmdlet 

Het volgende voorbeeld laat zien hoe het storage-account ophalen en deze te synchroniseren met de AMS-account.

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a>Stappen voor het storage-accounts toevoegen aan uw AMS-account

Het volgende onderwerp laat zien hoe storage-accounts toevoegen aan uw AMS-account: [meerdere opslagaccounts aan een Media Services-account koppelen](meda-services-managing-multiple-storage-accounts.md).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>Bevestigingen
Wij willen graag erkent de volgende personen die hebben bijgedragen tot de verwezenlijking van dit document: Cenk Dingiloglu, Milaan Gada, Seva Titov.
