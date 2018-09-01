---
title: Configureren en gebruiken van de Opslagemulator met Visual Studio | Microsoft Docs
description: Configureren en gebruiken van de Opslagemulator met Visual Studio
services: visual-studio-online
author: ghogen
manager: douge
assetId: c8e7996f-6027-4762-806e-614b93131867
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 8/17/2017
ms.author: ghogen
ms.openlocfilehash: 4b14fa44f484735f2a5efee4d631a6f48e6a8bf6
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382633"
---
# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Configureren en gebruiken van de Opslagemulator met Visual Studio
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Overzicht
De ontwikkelomgeving van de Azure SDK bevat de opslagemulator, een hulpprogramma dat overeenkomt met de Blob, Queue en Table storage-services beschikbaar zijn in Azure op uw lokale ontwikkelcomputer. Als u het met het bouwen van een cloudservice die maakt gebruik van de Azure storage-services of schrijven van een externe toepassing waarmee de storage-services wordt aangeroepen, kunt u uw code lokaal tegen de opslagemulator testen. De Azure Tools voor Microsoft Visual Studio integreren beheer van de opslagemulator met Visual Studio. De Azure-hulpprogramma's initialiseren van de database van de opslagemulator bij het eerste gebruik, de service storage-emulator wordt gestart wanneer u uitvoert of fouten opsporen in uw code vanuit Visual Studio en biedt alleen-lezen toegang tot de gegevens van de opslagemulator via de Azure Storage Explorer.

Zie voor gedetailleerde informatie over de opslagemulator, met inbegrip van de systeemvereisten en aangepaste configuratie-instructies, [de Azure-Opslagemulator gebruiken voor ontwikkeling en testen](storage/common/storage-use-emulator.md).

> [!NOTE]
> Er zijn enkele verschillen in functionaliteit tussen de simulatie van storage emulator en de Azure storage-services. Zie [verschillen tussen de Opslagemulator en Azure Storage-Services](storage/common/storage-use-emulator.md) in de Azure SDK-documentatie voor meer informatie over de specifieke verschillen.
> 
> 

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Configureren van een verbindingsreeks voor de opslagemulator
Voor toegang tot de opslagemulator vanuit code binnen een rol, wilt u een verbindingsreeks die verwijst naar de opslagemulator en die kan later worden gewijzigd om te verwijzen naar een Azure storage-account configureren. Een verbindingsreeks is een configuratie-instelling die uw rol kunt lezen tijdens runtime verbinding maken met een storage-account. Zie voor meer informatie over het maken van tekenreeksen voor databaseverbindingen [configureren van Azure Storage-verbindingsreeksen](/azure/storage/common/storage-configure-connection-string).

> [!NOTE]
> U kunt een verwijzing naar het opslagaccount van de emulator vanuit uw code retourneert met behulp van de **DevelopmentStorageAccount** eigenschap. Deze methode werkt goed als u wilt de opslagemulator openen vanuit uw code, maar als u van plan bent voor het publiceren van uw toepassing in Azure, u een verbindingsreeks moet voor toegang tot uw Azure storage-account en wijzig de code voor het gebruik van die verbinding maken tekenreeks voordat u deze publiceert. Als u vaak tussen het opslagaccount van de emulator en een Azure storage-account schakelen zijn, wordt een verbindingsreeks vereenvoudigen van dit proces.
> 
> 

## <a name="initializing-and-running-the-storage-emulator"></a>Het initialiseren en uitvoeren van de opslagemulator
U kunt opgeven dat Visual Studio automatisch de opslagemulator wordt gestart wanneer u uitvoert of fouten opsporen in uw service in Visual Studio. Open in Solution Explorer het snelmenu voor uw **Azure** toepassingsproject en kies **eigenschappen**. Op de **ontwikkeling** tabblad, in de **Start Azure-Opslagemulator** Kies **waar** (als deze nog niet is ingesteld op die waarde).

De eerste keer dat u uitvoert of fouten opsporen in uw service in Visual Studio naar een initialisatieproces wordt gestart door de opslagemulator. Dit proces lokale poorten reserveert voor de opslagemulator en de storage emulator database wordt gemaakt. Als u klaar bent, hoeft dit proces niet opnieuw uit te voeren, tenzij de database van de opslag-emulator wordt verwijderd.

> [!NOTE]
> Vanaf de release van juni 2012 van de Azure-hulpprogramma's, de opslagemulator wordt uitgevoerd, wordt standaard in SQL Express LocalDB. In eerdere versies van de Azure-hulpprogramma's, de opslagemulator wordt uitgevoerd op een standaardexemplaar van SQL Express 2005 of 2008, die u moet installeren voordat u de Azure SDK kunnen installeren. U kunt ook de opslagemulator op basis van een benoemd exemplaar van SQL Express of een benoemd exemplaar of een standaardexemplaar van Microsoft SQL Server uitvoeren. Als u configureren van de opslagemulator wilt wilt uitvoeren op basis van een exemplaar dan het standaardexemplaar, Zie [de Azure-Opslagemulator gebruiken voor ontwikkeling en testen](storage/common/storage-use-emulator.md).
> 
> 

De opslagemulator biedt een gebruikersinterface voor het weergeven van de status van de lokale opslag-services en om te starten, stoppen en opnieuw ingesteld. Zodra de service storage-emulator is gestart, kunt u de gebruikersinterface weergeven of starten of stoppen van de service met de rechtermuisknop op het pictogram voor de Microsoft Azure-Emulator in de taakbalk van Windows.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Storage-emulator gegevens weergeven in Server Explorer
Het Azure Storage-knooppunt in Server Explorer kunt u gegevens bekijken en wijzigen van instellingen voor de blob en tabel gegevens in uw storage-accounts, met inbegrip van de opslagemulator. Zie [beheren Azure Blob Storage-resources met Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs) voor meer informatie.

