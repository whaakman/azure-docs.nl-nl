---
title: Configureren en gebruiken van de Opslagemulator met Visual Studio | Microsoft Docs
description: Configureren en gebruiken van de Opslagemulator met Visual Studio
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: c8e7996f-6027-4762-806e-614b93131867
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/17/2017
ms.author: kraigb
ms.openlocfilehash: f4cd8ccc3b186cf2b4178b7d8a98d8928c705cbc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Configureren en gebruiken van de Opslagemulator met Visual Studio
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Overzicht
De ontwikkelomgeving Azure SDK bevat de opslagemulator, een hulpprogramma dat de Blob, wachtrijen en tabellen storage-services beschikbaar zijn in Azure op uw lokale ontwikkelcomputer simuleert. Als u de Azure storage-services bouwen van een cloudservice die in dienst of schrijven van elke externe toepassing waarmee de storage-services wordt aangeroepen, kunt u uw code te vergelijken met de opslagemulator lokaal testen. De Azure-hulpprogramma's voor Microsoft Visual Studio geïntegreerd beheer van de opslagemulator in Visual Studio. De hulpprogramma's Azure initialiseren van de database van de emulator opslag bij het eerste gebruik, de service storage emulator wordt gestart wanneer u uitvoert of fouten opsporen in uw code vanuit Visual Studio en alleen-lezen toegang biedt tot de gegevens van de emulator opslag via de Azure Storage Explorer.

Zie voor gedetailleerde informatie over de opslagemulator, met inbegrip van de systeemvereisten en aangepaste configuratie-instructies [de Azure-Opslagemulator gebruiken voor ontwikkeling en testen](storage/common/storage-use-emulator.md).

> [!NOTE]
> Er zijn bepaalde verschillen in functionaliteit tussen de simulatie van de emulator opslag en de Azure storage-services. Zie [verschillen tussen de Opslagemulator en Azure Storage-Services](storage/common/storage-use-emulator.md) in de Azure SDK-documentatie voor informatie over de specifieke verschillen.
> 
> 

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Een verbindingsreeks configureren voor de opslagemulator
Voor toegang tot de opslagemulator vanuit de programmacode binnen een rol, wilt u een verbindingsreeks die verwijst naar de opslagemulator en die kan later worden gewijzigd om te verwijzen naar een Azure storage-account configureren. Een verbindingsreeks is een configuratie-instelling die uw rol kan worden gelezen tijdens runtime verbinding maken met een opslagaccount. Zie voor meer informatie over het maken van verbindingsreeksen [configureren van de toepassing Azure](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

> [!NOTE]
> U kunt een verwijzing naar het opslagaccount van de emulator vanuit uw code retourneert met behulp van de **DevelopmentStorageAccount** eigenschap. Deze methode werkt goed als u de opslagemulator vanuit uw code toegang wilt, maar als u van plan bent om uw toepassing naar Azure te publiceren, u moet voor het maken van een verbindingsreeks voor toegang tot uw Azure storage-account en wijzig uw code voor het gebruik van deze verbindingsreeks voordat u deze hebt gepubliceerd. Als u tussen het opslagaccount van de emulator en een Azure storage-account vaak overschakelt, wordt een verbindingsreeks dit proces vereenvoudigen.
> 
> 

## <a name="initializing-and-running-the-storage-emulator"></a>Het initialiseren en uitvoeren van de opslagemulator
U kunt opgeven dat Visual Studio automatisch de opslagemulator wordt gestart tijdens het uitvoeren of fouten opsporen in uw service in Visual Studio. Open in Solution Explorer het snelmenu voor uw **Azure** project en kies **eigenschappen**. Op de **ontwikkeling** tabblad, in de **Start Azure-Opslagemulator** Kies **True** (indien deze nog niet is ingesteld op die waarde).

De eerste keer dat u uitvoert of fouten opsporen in uw service vanuit Visual Studio een initialisatieproces Hiermee start u de opslagemulator. Dit proces lokale poorten zijn gereserveerd voor de opslagemulator en de database van de emulator opslag wordt gemaakt. Als u klaar is, hoeft dit proces niet opnieuw uit te voeren als de database van de emulator opslag wordt verwijderd.

> [!NOTE]
> Beginnen met de release van juni 2012 van de Azure-hulpprogramma's, de opslagemulator wordt uitgevoerd, standaard in SQL Express LocalDB. In eerdere versies van de hulpprogramma's Azure de opslagemulator wordt uitgevoerd op een standaardexemplaar van SQL Express 2005 of 2008, die u moet installeren voordat u kunnen de Azure SDK installeren. U kunt ook de opslagemulator op basis van een benoemd exemplaar van SQL Express of een benoemde of de standaardinstantie van Microsoft SQL Server uitvoeren. Als u nodig hebt voor het configureren van de opslagemulator als u wilt uitvoeren op een exemplaar dan het standaardexemplaar, Zie [de Azure-Opslagemulator gebruiken voor ontwikkeling en testen](storage/common/storage-use-emulator.md).
> 
> 

De opslagemulator biedt een gebruikersinterface voor de status van de services van de lokale opslag weergeven en om te starten, stoppen en opnieuw ingesteld. Zodra de service storage emulator wordt gestart, kunt u de gebruikersinterface worden weergegeven of starten of stoppen van de service met de rechtermuisknop op het pictogram voor de Microsoft Azure-Emulator in de taakbalk van Windows.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Storage emulator gegevens in Server Explorer bekijken
Het Azure Storage-knooppunt in Server Explorer kunt u instellingen voor blob- en gegevens in uw storage-accounts, met inbegrip van de opslagemulator gegevens weergeven en wijzigen. Zie [beheren Azure Blob Storage-resources met Opslagverkenner (Preview)](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs) voor meer informatie.

