---
title: Verbinding maken met Azure analyseservices met Power BI | Microsoft Docs
description: Informatie over het verbinding maken met een Azure Analysis Services-server met behulp van Power BI.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 3a2af043feddb4a1d6d63f50e838c8a39035449f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-power-bi"></a>Verbinding maken met Power BI

Zodra u hebt een server in Azure gemaakt en geïmplementeerd een model in tabelvorm, zijn gebruikers in uw organisatie gereed om te koppelen en gebruiken van gegevens. 

> [!TIP]
> Zorg ervoor dat u de nieuwste versie van [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Verbinding maken in Power BI Desktop

1. Klik in Power BI Desktop op **gegevens ophalen** > **Azure** > **Azure Analysis Services-database**.

2. In **Server**, voer de naam van de server. 
    
    Zorg dat u de volledige URL opnemen. Bijvoorbeeld: asazure://westcentralus.asazure.windows.net/advworks.

3. In **Database**, als u weet dat de naam van de tabellaire modeldatabase of perspectief die u verbinding maken met wilt, plakt u deze hier. Anders kunt u dit veld leeg laten en later een database of perspectief selecteren.

4. Laat de standaardwaarde **Connect live** optie en druk vervolgens **Connect**. 

5. Als u wordt gevraagd, voert u de aanmeldingsreferenties. 

6. In **Navigator**, vouw de server uit en selecteer vervolgens het model of perspectief die u verbinding wilt, klikt u vervolgens op **Connect**. Klik op een model of perspectief om alle objecten voor deze weergave weer te geven.

    Het model wordt geopend in Power BI Desktop met een leeg rapport in de rapportweergave. De veldenlijst bevat alle modelobjecten niet-verborgen. Verbindingsstatus wordt weergegeven in de rechterbenedenhoek.

## <a name="connect-in-power-bi-service"></a>Verbinding maken in Power BI (service)

1. Maak een Power BI Desktop-bestand met een actieve verbinding met het model op uw server.
2. In [Power BI](https://powerbi.microsoft.com), klikt u op **gegevens ophalen** > **bestanden**. Zoek en selecteer het bestand.



## <a name="see-also"></a>Zie ook
[Verbinding maken met Azure analyseservices](analysis-services-connect.md)   
[Clientbibliotheken](analysis-services-data-providers.md)

