---
title: Azure Data Box-Gateway Preview release-opmerkingen | Microsoft Docs
description: Beschrijving van essentiële openstaande problemen en oplossingen voor de Azure Data Box-Gateway met Preview-versie.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 01/07/2019
ms.author: alkohli
ms.openlocfilehash: 738cc5b4e90d9572b65f122076973e2d9f1b264f
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54450487"
---
# <a name="azure-data-box-gateway-preview-release-notes"></a>Releaseopmerkingen voor Azure Data Box-Gateway-Preview

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen Identificeer de kritieke problemen voor het openen en de problemen opgelost voor Microsoft Azure Data Box-Gateway Preview-versie.

Opmerkingen bij de release worden voortdurend bijgewerkt en als kritieke problemen waarvoor een tijdelijke oplossingen nodig zijn gedetecteerd, worden ze toegevoegd. Voordat u uw Data Box-Gateway implementeert, zorgvuldig door de informatie in de release-opmerkingen.

Preview-versie komt overeen met de softwareversie **Data Box Gateway Preview-versie 2.0**.

## <a name="issues-fixed-in-preview-release"></a>Problemen opgelost in Preview-versie

De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Probleem |
| --- | --- |
| **1.** | In deze release, wanneer een bestand dat is geüpload door een ander hulpprogramma (AzCopy) wordt vernieuwd en wordt bijgewerkt op een manier die de bestandsgrootte toeneemt/doorloopt vervolgens de volgende fout in acht wordt genomen: *Fout 400: InvalidBlobOrBlock (de opgegeven blob of block-inhoud is ongeldig.)*|
| **2.** |Vanwege een fout in deze release, ziet u mogelijk exemplaren van foutcode 110 in *error.xml* met onherkenbare itemnamen. | 
| **3.** |Vanwege een fout in deze release ziet u mogelijk exemplaren van foutcode 2003 tijdens het uploaden van bestanden. | 
| **4.** |In deze release kunt u slechts één share vernieuwen op een tijdstip. | 


## <a name="known-issues-in-preview-release"></a>Bekende problemen in de Preview-versie

De volgende tabel bevat een overzicht van de bekende problemen voor de Data Box-Gateway met preview-versie.

| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
| --- | --- | --- | --- |
| **1.** |Updates |De Data Box-Gateway-apparaten die zijn gemaakt in het eerdere voorbeeld releases kunnen niet worden bijgewerkt naar deze versie. |De installatiekopieën van virtuele schijven van de nieuwe versie downloaden en configureren en implementeren van nieuwe apparaten. Ga voor meer informatie naar [voorbereidingen voor het implementeren van Azure Data Box Gateway](data-box-gateway-deploy-prep.md). |
| **2.** |Ingerichte gegevensschijf |Nadat u een gegevensschijf van een bepaalde opgegeven grootte hebt ingericht en de bijbehorende Data Box-Gateway hebt gemaakt, u moet niet verkleinen de gegevensschijf. Er wordt geprobeerd voor het verkleinen van de resultaten van de schijf in een verlies van de lokale gegevens op het apparaat. | |
| **3.** |Naam wijzigen |Naam wijzigen van objecten wordt niet ondersteund. |Neem contact op met Microsoft Support als deze functie is van cruciaal belang voor uw werkstroom. |
| **4.** |Exemplaar| Als een alleen-lezen-bestand wordt gekopieerd naar het apparaat, wordt de alleen-lezen eigenschap niet behouden. | |
| **5.** |Bestandstypen | De volgende typen van de Linux-bestanden worden niet ondersteund: teken bestanden, bestanden blokkeren, sockets, pipes, symbolische koppelingen.  |Deze bestanden zijn gekopieerd resultaten in 0 heeft een lengte van bestanden die worden gemaakt op de NFS delen. Deze bestanden blijven in een foutstatus en er wordt ook gerapporteerd *error.xml*. |
| **6.** |Verwijderen | Vanwege een fout in deze release, als een NFS-share is verwijderd, kan klikt u vervolgens de share niet worden verwijderd. De status van de share wordt weergegeven *verwijderen*.  |Dit gebeurt alleen als de share met behulp van de naam van een niet-ondersteund bestandstype. |
| **7.** |Vernieuwen | Machtigingen en toegangsbeheerlijsten (ACL's) blijven niet behouden via een vernieuwingsbewerking.  | |
| **8.** |Online-help |De Help-koppelingen in de Azure-portal kan niet koppelen aan documentatie.|De Help-koppelingen werken in de algemeen beschikbare versie. |



## <a name="next-steps"></a>Volgende stappen

- [Voorbereidingen voor het implementeren van Azure Data Box Gateway](data-box-gateway-deploy-prep.md).


