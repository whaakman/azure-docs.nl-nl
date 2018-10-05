---
title: Store van uw pakket AppSource naar Azure storage en het genereren van een URL met SAS-sleutel voor | Microsoft Docs
description: Details van de stappen die nodig zijn om te uploaden en beveiligen van een pakket met AppSource.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 335590cc616035020f66cfc5208e21d4c5128fe6
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810004"
---
<a name="store-your-appsource-package-to-azure-storage-and-generate-a-url-with-sas-key"></a>Store van uw pakket AppSource naar Azure storage en het genereren van een URL met SAS-sleutel
=============================================================================

Voor het onderhouden van beveiliging van uw bestanden, moeten alle partners hun AppSource-pakketbestand opslaan in een Azure blob storage-account en een SAS-sleutel gebruiken om deze te delen. Het pakketbestand wordt opgehaald uit uw Azure-opslaglocatie voor certificering en deze gebruikt voor proefversies van AppSource.

Gebruik de volgende stappen voor het uploaden van uw pakket naar blob-opslag:

1.  Ga naar <http://azure.microsoft.com> en maak een gratis proefversie of gefactureerd-account.

2.  Meld u aan bij [Azure-portal](http://portal.azure.com/).

3.  Een nieuw Opslagaccount maken door te klikken op **+ nieuw** en Ga naar de **gegevens en opslag** account.

  ![Gegevens en opslag-blade op Microsoft Azure Portal](media/CRMScreenShot7.png)

4. Voer een **naam** en **resourcegroep** een naam geven en klik op **maken** knop.

  ![Storage-account maken in Microsoft Azure Portal](media/CRMScreenShot8.png)

5. Navigeer naar uw nieuwe resourcegroep en een nieuwe blobcontainer maken.

  ![Pakket uploaden als blob met behulp van Microsoft Azure Portal](media/CRMScreenShot9.png)

6.  Als u hebt nog niet gedaan, downloadt en installeert de Microsoft [Azure Storage Explorer](http://storageexplorer.com/).

7.  Storage Explorer openen en gebruiken van het pictogram verbinding maken met uw Azure storage-account.

8.  Navigeer naar de blob-container die u hebt gemaakt en klik op **uploaden** uw pakket zip-bestand toe te voegen.

  ![Uploaden met behulp van Microsoft Storage Explorer-pakket](media/CRMScreenShot10.png)

9.  Klik met de rechtermuisknop op uw bestand en selecteer **Shared Access Signature ophalen**.

  ![Handtekening voor gedeelde toegang van een Azure-bestand ophalen](media/CRMScreenShot11.png)

10.  Wijzig de **verlooptijd** als u wilt de SAS activeren voor een maand, klikt u vervolgens op **maken**.

  ![De SAS-vervaldatum van een Azure-bestand wijzigen](media/CRMScreenShot12.png)

11.  Kopieer het veld-URL en bewaar het voor later. U moet deze URL invoeren bij het maken van de bijbehorende aanbieding. 

  ![Kopieer de SAS-URL van een Azure-bestand](media/CRMScreenShot13.png)

