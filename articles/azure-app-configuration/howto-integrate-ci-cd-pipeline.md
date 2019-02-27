---
title: Integreren met een continue integratie en levering pijplijn met behulp van Azure App-configuratie | Microsoft Docs
description: Meer informatie over het genereren van een configuratiebestand met gegevens in Azure App-configuratie tijdens de continue integratie en levering
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 0e6b24175ffa28b2a0f361bc46fd6c41e09cae72
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885436"
---
# <a name="integrate-with-a-cicd-pipeline"></a>Integreren met een CI/CD-pijplijn

Ter verbetering van de flexibiliteit van uw toepassing meer veiligheid tegen de externe kans niet meer kunnen bereiken van Azure App-configuratie, moet u de huidige configuratiegegevens verpakken in een bestand dat is geïmplementeerd met de toepassing en lokaal geladen tijdens het opstarten . Deze aanpak zorgt ervoor dat uw toepassing standaardwaarden voor de instelling ten minste hebben. Deze waarden worden overschreven door nieuwe wijzigingen in een configuratie van apps wanneer deze beschikbaar is.

## <a name="automate-configuration-data-retrieval"></a>Configuratie van het ophalen van gegevens automatiseren

Met behulp van de [ **exporteren** ](./howto-import-export-data.md#export-data) functie van Azure App-configuratie, kunt u het proces van het ophalen van de huidige van configuratiegegevens als een enkel bestand automatiseren. U kunt dit bestand vervolgens opnemen in een stap build of implementatie in uw continue integratie en continue implementatiepijplijn.

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: Een ASP.NET-web-app maken](quickstart-aspnet-core-app.md)  
