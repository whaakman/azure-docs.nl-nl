---
title: Een Azure container registry geo repliceren
description: Maken en beheren van Azure container geogerepliceerde registers aan de slag.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: overview-article
ms.date: 10/24/2017
ms.author: stevelas
ms.openlocfilehash: 6b82f49d2bf4ed321f5e847d11780535e01531f7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="geo-replication-in-azure-container-registry"></a>Geo-replicatie in Azure Container Registry

Bedrijven die de aanwezigheid van een lokale of een hot back-up wilt kiezen voor het uitvoeren van de services van meerdere Azure-regio's. Als een best practice kunt als u een register container in elke regio waarin afbeeldingen worden uitgevoerd netwerk sluiten bewerkingen, snelle, betrouwbare installatiekopie laag overdrachten inschakelen.

Geo-replicatie kunt een Azure container registry functioneren als een enkele register meerdere regio's met meerdere masters registers fungeren.

> [!IMPORTANT]
> De functie voor geo-replicatie van Azure Container Registry is momenteel beschikbaar als **preview-versie**. Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord met gaat de [aanvullende gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sommige aspecten van deze functionaliteit kunnen wijzigen voordat deze functionaliteit algemeen beschikbaar wordt.
>

Een register geo-replicatie biedt de volgende voordelen:

* Enkele register/afbeeldingscode namen kunnen worden gebruikt in meerdere regio 's
* Toegang tot het netwerk sluit Register van regionale implementaties
* Er zijn geen kosten meer uitgaande als afbeeldingen worden opgehaald van een lokale, gerepliceerde register in dezelfde regio bevinden als uw containerhost
* Één management van een register over meerdere regio 's

## <a name="example-use-case"></a>Voorbeeld gebruiksvoorbeeld
Contoso voert een openbare aanwezigheid website zich in de VS, Canada en Europa. Voor deze markten met inhoud van lokale en netwerk-sluiten, Contoso wordt uitgevoerd [Azure Container Service](/azure/container-service/kubernetes/) (ACS) Kubernetes-clusters in VS-West, VS-Oost, Canada centraal en West-Europa. De websitetoepassing die is geïmplementeerd als een installatiekopie Docker maakt gebruik van de code en de installatiekopie in alle regio's. Inhoud lokaal op de regio die wordt opgehaald uit een database die uniek is ingericht in elke regio. Elke regionale implementatie heeft de configuratie van de unieke voor resources, zoals de lokale database.

Het ontwikkelteam bevindt zich in Haarlem WA, met behulp van het datacenter VS-West.

![Pushen naar meerdere registers](media/container-registry-geo-replication/before-geo-replicate.png)<br />*Pushen naar meerdere registers*

Voordat u de functies geo-replicatie, had Contoso een register VS gebaseerde in VS-West, met een extra registerwaarden in West-Europa. Voor deze verschillende regio's, moest het ontwikkelingsteam installatiekopieën push naar twee verschillende registers.

```bash
docker push contoso.azurecr.io/pubic/products/web:1.2
docker push contosowesteu.azurecr.io/pubic/products/web:1.2
```
![Binnenhalen van meerdere registers](media/container-registry-geo-replication/before-geo-replicate-pull.png)<br />*Binnenhalen van meerdere registers*

Typische uitdagingen van meerdere registers zijn onder andere:

* VS-Oost, VS-West en Canada Central clusters die alle ophalen uit het register VS-West, kosten voor uitgaande steeds als elk van deze hosts externe container pull-installatiekopieën van datacenters VS-West.
* Het ontwikkelteam moet installatiekopieën pushen naar registers VS-West en West-Europa.
* Het ontwikkelteam moet configureren en onderhouden van elke regionale implementatie met de namen van afbeeldingen die verwijzen naar het lokale register.
* Toegang tot het register moet worden geconfigureerd voor elke regio.

## <a name="benefits-of-geo-replication"></a>Voordelen van geo-replicatie

![Binnenhalen van een register geogerepliceerde](media/container-registry-geo-replication/after-geo-replicate-pull.png)

Met de functie geo-replicatie van Azure Container register, deze voordelen gerealiseerd:

* Een enkele register in alle regio's beheren: `contoso.azurecr.io`
* Beheren van implementaties van installatiekopieën van een configuratie voor één zoals alle regio's dezelfde afbeeldings-URL gebruikt: `contoso.azurecr.io/public/products/web:1.2`
* Push naar een enkele register, terwijl ACR de geo-replicatie beheert, met inbegrip van regionale webhooks voor lokale meldingen

## <a name="configure-geo-replication"></a>Geo-replicatie configureren
Het configureren van geo-replicatie is net zo eenvoudig als regio's op een kaart te klikken.

Geo-replicatie is een functie van [Premium registers](container-registry-skus.md) alleen. Als het register niet nog Premium, kunt u van Basic en Standard naar Premium in de [Azure-portal](https://portal.azure.com):

![Overschakelen van SKU's in de Azure portal](media/container-registry-skus/update-registry-sku.png)

Voor het configureren van geo-replicatie voor het Premium-register, meld u aan bij de Azure portal op http://portal.azure.com.

Navigeer naar uw Azure-Container-register en selecteer **replicaties**:

![Replicaties in de containerregister-UI van Azure Portal](media/container-registry-geo-replication/registry-services.png)

Een kaart wordt weergegeven met alle huidige Azure-regio's:

 ![Kaart met regio's in Azure Portal](media/container-registry-geo-replication/registry-geo-map.png)

* Blauw Zeshoeken vertegenwoordigen de huidige replica 's
* Groen Zeshoeken vertegenwoordigen mogelijk replica regio 's
* Grijze Zeshoeken vertegenwoordigen Azure-regio's nog niet beschikbaar voor replicatie

Voor het configureren van een replica, selecteer een groene zeshoek en vervolgens **maken**:

 ![Gebruikersinterface voor het maken van een replicatie in Azure Portal](media/container-registry-geo-replication/create-replication.png)

Als u wilt configureren als u meer replica's, selecteer de groene Zeshoeken voor andere regio's en klik op **maken**.

ACR begint installatiekopieën via de geconfigureerde replica's synchroniseren. Als u klaar is, duidt op de portal *gereed*. De status van de replica in de portal wordt niet automatisch bijgewerkt. Gebruik de knop Vernieuwen om de bijgewerkte status.

## <a name="geo-replication-pricing"></a>Prijzen van geo-replicatie

Geo-replicatie is een functie van de [Premium-SKU](container-registry-skus.md) van Azure Container register. Wanneer u een register naar de gewenste regio's repliceren, u Premium register kosten voor elke regio hebt gemaakt.

In het voorgaande voorbeeld Contoso geconsolideerd twee registers naar beneden, replica's toe te voegen aan VS-Oost, Canada centraal en West-Europa. Contoso zou Premium vier keer per maand, zonder extra configuratie of management betalen. Elke regio haalt nu hun installatiekopieën lokaal, verbeterde prestaties, betrouwbaarheid zonder netwerk uitgaande kosten van de VS-West naar Canada en VS-Oost.

## <a name="summary"></a>Samenvatting

Met geo-replicatie, kunt u uw regionale gegevenscentra beheren als één globale cloud. Omdat installatiekopieën van alle veel Azure-services worden gebruikt, kunt u profiteren van een vlak één management snel behoud netwerk sluiten en betrouwbare lokale image haalt.

## <a name="next-steps"></a>Volgende stappen

Bekijk de zelfstudie reeks driedelige [Geo-replicatie in Azure Container register](container-registry-tutorial-prepare-registry.md). Helpt bij het maken van een register geogerepliceerde, bouwt een container en vervolgens implementeert u het met één `docker push` opdracht naar meerdere regionale Web-Apps voor exemplaren van de Containers.

> [!div class="nextstepaction"]
> [Geo-replicatie in Azure Container register](container-registry-tutorial-prepare-registry.md)