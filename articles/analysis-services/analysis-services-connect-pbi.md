---
title: Verbinding maken met Azure analyseservices met Power BI | Microsoft Docs
description: Leer hoe u verbinding maken met een Azure Analysis Services-server met behulp van Power BI.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 707bc41a2a66782d9540d95606c41685908e9848
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429686"
---
# <a name="connect-with-power-bi"></a>Verbinding maken met Power BI

Nadat u hebt gemaakt van een server in Azure en een tabellair model geïmplementeerd, zijn gebruikers in uw organisatie klaar om te verbinden en begint met het verkennen van gegevens. 

> [!TIP]
> Zorg ervoor dat u de nieuwste versie van [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Verbinden maken in Power BI Desktop

1. Klik in Power BI Desktop op **Gegevens ophalen** > **Azure** > **Azure Analysis Services-database**.

2. In **Server**, voer de naam van de server. Zorg ervoor dat u de volledige URL; bijvoorbeeld, asazure://westcentralus.asazure.windows.net/advworks.

3. In **Database**, als u bekend bent met de naam van de database tabellaire model of perspectief die u verbinding maken met wilt, plak deze hier. Anders kunt u dit veld leeg laten en later een database of een perspectief selecteren.

4. Selecteer een verbindingsoptie voor en druk vervolgens op **Connect**. 

    Beide **live Connect** en **importeren** opties worden ondersteund. Echter, raden we dat u live-verbindingen gebruiken, omdat de modus Import heeft enkele beperkingen; met name, kan prestaties van de server worden beïnvloed tijdens het importeren. Ook als het model worden vernieuwd in de Power BI-service is de **toegang toestaan vanuit Power BI** instelling geldt alleen bij het kiezen van **live Connect**.

5. Als u hierom wordt gevraagd, voert u uw aanmeldingsreferenties. 

6. In **Navigator**, vouw de server uit en selecteer vervolgens het model of perspectief die u wilt verbinding maken met en klik vervolgens op **Connect**. Klik op een model of perspectief om alle objecten voor deze weergave weer te geven.

    Het model wordt geopend in Power BI Desktop met een leeg rapport in de rapportweergave. De lijst met velden wordt weergegeven voor alle modelobjecten niet-verborgen. De verbindingsstatus wordt rechtsonder weergegeven.

## <a name="connect-in-power-bi-service"></a>Verbinding maken in Power BI (service)

1. Maak een Power BI Desktop-bestand met een live-verbinding aan uw model op uw server.
2. In [Power BI](https://powerbi.microsoft.com), klikt u op **gegevens ophalen** > **bestanden**, zoekt en selecteert u uw pbix-bestand.



## <a name="see-also"></a>Zie ook
[Verbinding maken met Azure analyseservices](analysis-services-connect.md)   
[Clientbibliotheken](analysis-services-data-providers.md)

