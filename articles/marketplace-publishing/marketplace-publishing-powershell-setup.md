---
title: PowerShell instellen voor een virtuele machine maken voor de Marketplace | Microsoft Docs
description: "Instructies voor het instellen van Azure PowerShell en gebruiken als een optioneel proces stromen voor het maken van VM-installatiekopieën implementeren op en verkopen op Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e19d6cda-76df-4e42-be84-c9fe47a636db
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2016
ms.author: hascipio
ms.openlocfilehash: bbcce5093d2bbd5326523063db7d0e565fe4de6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Azure PowerShell instellen voor het maken van een aanbieding voor Azure Marketplace
Zie voor gedetailleerde informatie over het instellen van PowerShell in Azure [installeren en configureren van Azure PowerShell](/powershell/azure/overview). Een eenvoudige benadering is het gebruik van de methode certificaat, die worden gedownload en importeert een certificaat nodig voor verificatie. Voor het benodigde certificaat verkrijgen, gebruikt u de **Get-AzurePublishSettingsFile** cmdlet. Sla het bestand wanneer u wordt gevraagd. Gebruikt u het certificaat importeren in een PowerShell-sessie door de **importeren AzurePublishSettingsFile** cmdlet.

Als u wilt configureren en opslaan van de algemene instellingen voor Microsoft Azure-abonnement voor de PowerShell-sessie, gebruikt u de **Set-AzureSubscription** en **Select-AzureSubscription** cmdlets:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

De eerste opdracht wordt een standaardopslagaccount gekoppeld aan het abonnement (nodig voor bepaalde bewerkingen van VM-inrichting).  De tweede maakt het abonnement van de huidige versie (herkend door andere cmdlets).

## <a name="see-also"></a>Zie ook
* [Aan de slag: hoe een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md)
* [De installatiekopie van een virtuele machine maken voor de Marketplace](marketplace-publishing-vm-image-creation.md)

