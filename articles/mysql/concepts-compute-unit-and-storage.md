---
title: Eenheden per Azure-Database waarin wordt uitgelegd berekenen voor MySQL | Microsoft Docs
description: 'Azure DB voor MySQL: dit artikel wordt uitgelegd van de concepten van Compute-eenheden en wat er gebeurt wanneer uw werkbelasting het maximum aantal eenheden voor Compute bereikt.'
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/29/2017
ms.openlocfilehash: 2c4894ae9a4235f9ced4a8d9b991238543646f53
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="explaining-compute-units-in-azure-database-for-mysql"></a>In Azure MySQL-Database waarin wordt uitgelegd Compute-eenheden
Dit onderwerp wordt het concept van Compute-eenheden en wat er gebeurt wanneer uw werkbelasting het maximum aantal eenheden voor Compute bereikt.

## <a name="what-are-compute-units"></a>Wat zijn eenheden berekenen?
Rekenuren voor dat eenheden zijn een meting van CPU-verwerking doorvoer die beschikbaar zijn voor één Azure-Database voor de MySQL-server is gegarandeerd. Een Compute-eenheid is een gecombineerde meting van CPU en geheugenbronnen. In het algemeen neer 50 Compute-eenheden op met de helft van een kern. 100 compute eenheden neer naar één kern. 2.000 compute eenheden neer 20 kernen van gegarandeerde verwerking doorvoer beschikbaar is voor uw server.

De hoeveelheid geheugen per eenheid Compute is geoptimaliseerd voor het Basic en Standard Prijscategorieën. Verdubbeling van de Compute-eenheden door te verhogen van het prestatieniveau is gelijk aan de set van de resource beschikbaar is voor die één Azure-Database voor MySQL verdubbelen.

Bijvoorbeeld biedt een 800 Compute standaardeenheden doorvoer van 8 x meer CPU en geheugen dan een 100 Compute standaardeenheden-configuratie. 100 Compute standaardeenheden zorgen voor de dezelfde CPU-doorvoer in vergelijking met 100 Compute basiseenheden, is de hoeveelheid geheugen die vooraf is geconfigureerd in de prijscategorie Standard dubbele hoeveelheid geheugen die is geconfigureerd voor het Basic prijscategorie. Daarom biedt de Standard-prijscategorie betere prestaties van de werkbelasting en lagere latentie mogelijk transactie dan de Basic prijscategorie met dezelfde Compute eenheden geselecteerd.

## <a name="how-can-i-determine-the-number-of-compute-units-needed-for-my-workload"></a>Hoe kan ik zien dat het aantal Compute-eenheden voor de werkbelasting nodig?
Als u wilt migreren van een bestaande MySQL-server lokaal wordt uitgevoerd of op een virtuele machine, u het aantal eenheden Compute bepalen kunt door te schatten hoeveel kernen van doorvoer van de verwerking van uw workload. 

Als uw bestaande on-premises server van de virtuele machine is momenteel in gebruik of 4 kernen (zonder CPU hyperthread tellen), eerst 400 Compute eenheden configureren voor uw Azure-Database voor de MySQL-server. COMPUTE eenheden kunnen worden geschaald omhoog of omlaag dynamisch, afhankelijk van de behoeften van uw werkbelasting, en met vrijwel geen uitvaltijd voor toepassingen. 

Controleren van de grafiek metrische gegevens in de Azure portal of Azure CLI-opdrachten voor het meten van Compute eenheden schrijven. Relevante meetgegevens voor het bewaken van zijn het percentage van de Compute-eenheid en de limiet van de Compute-eenheid.

>[!IMPORTANT]
> Als u opslag die IOP 's niet volledig worden gebruikt voor het maximum vinden, kunt u ook het gebruik van de Compute-eenheden bewaking. De Compute-eenheden verhogen mogelijk voor een hogere i/o-doorvoer door vermindering van het prestatieknelpunt ten gevolge van beperkte CPU of geheugen.

## <a name="what-happens-when-i-hit-my-maximum-compute-units"></a>Wat gebeurt er wanneer ik mijn maximum aantal eenheden voor Compute bereikt?
Prestaties zijn kalibreren en geregeld om bronnen voor het uitvoeren van de werkbelasting van uw database tot de maximale limiet voor de geselecteerde prijscategorie prijscategorie en prestatieniveau niveau. 

Als uw werkbelasting de maximale limieten in ofwel de Compute-eenheden of ingerichte IOPS-limieten bereikt, gebruik van de bronnen op de maximaal toegestane niveau kan worden voortgezet, maar uw query's zijn waarschijnlijk hogere latenties optreden. Deze limieten leiden tot een vertraging van de werkbelasting in plaats van fouten, tenzij de vertraging wordt zo ernstig die query time-out. 

Als uw werkbelasting de bovengrenzen voor het aantal verbindingen bereikt, worden de expliciete fouten gegenereerd. Zie voor meer informatie over bronnen limieten [beperkingen in Azure-Database voor MySQL](concepts-limits.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het Prijscategorieën [Azure Database voor MySQL Prijscategorieën](./concepts-service-tiers.md).
