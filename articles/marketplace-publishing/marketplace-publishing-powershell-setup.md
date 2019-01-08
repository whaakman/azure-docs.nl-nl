---
title: PowerShell instellen voor een virtuele machine maken voor de Marketplace | Microsoft Docs
description: Instructies voor het instellen van Azure PowerShell en het gebruik van deze als een optionele proces voor het maken van VM-installatiekopieën te implementeren naar flow en verkopen op Azure Marketplace
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: e19d6cda-76df-4e42-be84-c9fe47a636db
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: e5175558f18dfc903c280ea6bbe487e0a3ee8189
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54076600"
---
# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Azure PowerShell instellen voor het maken van een aanbieding voor Azure Marketplace

Zie voor gedetailleerde informatie over het instellen van PowerShell in Azure [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview). Een eenvoudige benadering is het gebruik van de certificaat-methode, die wordt gedownload en de invoer van een certificaat dat nodig is voor verificatie. Voor het benodigde certificaat verkrijgen, gebruikt u de **Get-AzurePublishSettingsFile** cmdlet. Sla het bestand wanneer u wordt gevraagd. Als u wilt het certificaat importeren in een PowerShell-sessie, gebruikt u de **importeren AzurePublishSettingsFile** cmdlet.

Als u wilt configureren en opslaan van de algemene instellingen voor de Microsoft Azure-abonnement voor de PowerShell-sessie, gebruikt u de **instellen-Azure-abonnement** en **Select-AzureSubscription** cmdlets:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

De eerste opdracht wordt een standaardopslagaccount gekoppeld aan het abonnement (die nodig zijn voor bepaalde bewerkingen van VM-inrichting).  De tweede kunt u het abonnement de huidige versie (wordt herkend door andere cmdlets).

Raadpleeg voor meer informatie de volgende artikelen:
* [Aan de slag: Een aanbieding publiceren op Azure Marketplace](marketplace-publishing-getting-started.md)
* [Het maken van een VM-installatiekopie voor de Marketplace](marketplace-publishing-vm-image-creation.md)

