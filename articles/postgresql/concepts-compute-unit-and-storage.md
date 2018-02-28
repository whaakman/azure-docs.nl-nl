---
title: Eenheden per Azure-Database waarin wordt uitgelegd berekenen voor PostgreSQL | Microsoft Docs
description: 'Azure DB voor PostgreSQL: dit artikel wordt uitgelegd van de concepten van Compute-eenheden en wat er gebeurt wanneer uw werkbelasting het maximum aantal eenheden voor Compute bereikt.'
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: dbb9f733455fa0492358b24b178c8c637ff08c71
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="explaining-compute-units-in-azure-database-for-postgresql"></a>In Azure PostgreSQL-Database waarin wordt uitgelegd Compute-eenheden
Dit onderwerp wordt het concept van Compute-eenheden en wat er gebeurt wanneer uw werkbelasting het maximumniveau van Compute-eenheden bereikt.

## <a name="what-are-compute-units"></a>Wat zijn eenheden berekenen?
Rekenuren voor dat eenheden zijn een meting van CPU-verwerking doorvoer die beschikbaar zijn voor één Azure-Database voor PostgreSQL-server is gegarandeerd. Een Compute-eenheid is een gecombineerde meting van CPU en geheugenbronnen. In het algemeen neer 50 Compute-eenheden op met de helft van een kern. 100 compute eenheden neer naar één kern. 2000 compute eenheden neer 20 kernen van gegarandeerde verwerking doorvoer beschikbaar is voor uw server.

De hoeveelheid geheugen per eenheid Compute is geoptimaliseerd voor het Basic en Standard Prijscategorieën. De Compute-eenheden verdubbeling doordat het prestatieniveau is gelijk aan de hoeveelheid CPU en geheugen aan die één Azure-Database voor PostgreSQL verdubbeling.

Bijvoorbeeld, biedt een 800 Compute standaardeenheden doorvoer van 8 x meer CPU en geheugen dan een 100 Compute standaardeenheden-configuratie. Terwijl 100 Compute standaardeenheden dezelfde CPU doorvoer wordt gehaald, bieden zoals het Basic 100 Compute-eenheden, is de hoeveelheid geheugen die vooraf is geconfigureerd in de prijscategorie Standard dubbele hoeveelheid geheugen die is geconfigureerd voor het Basic prijscategorie. Daarom biedt de Standard-prijscategorie betere prestaties van de werkbelasting en lagere latentie mogelijk transactie dan de Basic prijscategorie met dezelfde Compute eenheden geselecteerd.

## <a name="how-can-i-determine-the-number-of-compute-units-needed-for-my-workload"></a>Hoe kan ik zien dat het aantal Compute-eenheden voor de werkbelasting nodig?
Als u wilt migreren van een bestaande PostgreSQL-server lokaal wordt uitgevoerd of op een virtuele machine, u het aantal eenheden Compute bepalen kunt door te schatten hoeveel kernen van verwerking doorvoer de werkbelasting van uw behoeften. 

Als uw bestaande on-premises of de server van de virtuele machine is momenteel gebruik van 4 kernen (zonder CPU hyperthread tellen) wordt eerst 400 Compute eenheden configureren voor uw Azure-Database voor PostgreSQL-server. COMPUTE eenheden kunnen worden dynamisch uitgebreid omhoog of omlaag, afhankelijk van de behoeften van uw werkbelasting met vrijwel geen uitvaltijd voor toepassingen. 

Controleren van de grafiek metrische gegevens in de Azure portal of Azure CLI-opdrachten voor het meten van Compute eenheden schrijven. Relevante meetgegevens voor het bewaken van zijn het percentage van de Compute-eenheid en de limiet van de Compute-eenheid.

>[!IMPORTANT]
> Als u opslag die IOP 's niet volledig worden gebruikt voor het maximum vinden, kunt u ook het gebruik van de Compute-eenheden bewaking. De Compute-eenheden verhogen mogelijk voor een hogere i/o-doorvoer door vermindering van het prestatieknelpunt vanwege beperkte CPU of geheugen.

## <a name="what-happens-when-i-hit-my-maximum-compute-units"></a>Wat gebeurt er wanneer ik mijn maximum aantal eenheden voor Compute bereikt?
Prestaties zijn kalibreren en geregeld om bronnen voor het uitvoeren van de werkbelasting van uw database tot de maximale limiet voor de geselecteerde prijscategorie prijscategorie en prestatieniveau niveau. 

Als uw werkbelasting maxima in de Compute-eenheden of ingerichte IOP's bereikt, het gebruik van de bronnen op de maximaal toegestane niveau kan worden voortgezet, maar uw query's zijn waarschijnlijk hogere latenties. Deze limieten resulteren niet in fouten, maar in plaats daarvan een vertraging van de werkbelasting, tenzij de vertraging wordt zo ernstig die query time-out. 

Als uw werkbelasting de bovengrenzen voor het aantal verbindingen bereikt, worden de expliciete fouten gegenereerd. Zie voor meer informatie over bronnen limieten [beperkingen in Azure-Database voor PostgreSQL](concepts-limits.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het Prijscategorieën [Azure Database voor PostgreSQL Prijscategorieën](./concepts-service-tiers.md).
