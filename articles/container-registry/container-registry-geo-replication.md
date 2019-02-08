---
title: Een Azure container registry geo-replicatie
description: Aan de slag, het maken en beheren van geo-gerepliceerd Azure-containerregisters.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: overview-article
ms.date: 04/10/2018
ms.author: stevelas
ms.openlocfilehash: a83cf6b37a28ec38165778faa7a9ecc266cce7bd
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55858253"
---
# <a name="geo-replication-in-azure-container-registry"></a>Geo-replicatie in Azure Container Registry

Bedrijven die lokale aanwezigheid handhaaft, of een ' hot ' back-up wilt kiezen voor het uitvoeren van services van Azure-regio's. Als een best practice kunt een containerregister te brengen in elke regio waarin afbeeldingen worden uitgevoerd dichtbij het netwerk-bewerkingen, waardoor snelle, betrouwbare installatiekopie laag overdrachten. Geo-replicatie kunt een Azure container registry als één register functioneren in meerdere regio's met meerdere masters registers.

Een geo-gerepliceerde register biedt de volgende voordelen:

* Één register/afbeeldingscode namen kunnen worden gebruikt in meerdere regio 's
* Toegang tot het register van dichtbij het netwerk van regionale implementaties
* Geen kosten voor aanvullende uitgaand verkeer, als installatiekopieën worden opgehaald uit een lokale, gerepliceerde register in dezelfde regio als uw containerhost
* Één management van een register in meerdere regio 's

> [!NOTE]
> Als u kopieën van containerinstallatiekopieën in meer dan één Azure container registry moet, Azure Container Registry biedt ook ondersteuning voor [image importeren](container-registry-import-images.md). Bijvoorbeeld in een DevOps-werkstroom, kunt u importeren een afbeelding uit een register ontwikkeling naar een register productie zonder gebruik van Docker-opdrachten.
>

## <a name="example-use-case"></a>Voorbeeld van de use-case
Contoso wordt uitgevoerd een aanwezigheid van openbare website zich in de VS, Canada en Europa. Als u wilt deze markten met inhoud van lokale en dichtbij het netwerk fungeren, Contoso wordt uitgevoerd [Azure Kubernetes Service](/azure/aks/) (AKS)-clusters in VS-West, VS-Oost, Canada-centraal en West-Europa. De websitetoepassing geïmplementeerd als een Docker-installatiekopie maakt gebruik van de code en de installatiekopie in alle regio's. Inhoud, lokale voor deze regio wordt opgehaald uit een database, die uniek is ingericht in elke regio. Elke regionale implementatie heeft de unieke configuratie voor resources, zoals de lokale database.

Het ontwikkelteam bevindt zich in Seattle, WA, met behulp van het datacentrum VS-West.

![Pushen naar meerdere registers](media/container-registry-geo-replication/before-geo-replicate.png)<br />*Pushen naar meerdere registers*

Voordat u de functies voor geo-replicatie, had Contoso een register op basis van VS in VS-West, met een extra registry in West-Europa. Voor deze verschillende regio's, het ontwikkelteam installatiekopieën gepusht naar twee verschillende registers.

```bash
docker push contoso.azurecr.io/public/products/web:1.2
docker push contosowesteu.azurecr.io/public/products/web:1.2
```
![Binnenhalen van meerdere registers](media/container-registry-geo-replication/before-geo-replicate-pull.png)<br />*Binnenhalen van meerdere registers*

Typische uitdagingen van meerdere registers zijn onder andere:

* VS-Oost, VS-West en Canada-centraal clusters die alle op te halen uit het register VS-West, waarbij extra kosten voor uitgaand verkeer kosten als elk van deze externe containerhosts pull installatiekopieën vanuit VS-West-datacenters.
* Het ontwikkelteam moet installatiekopieën pushen naar VS-West en West-Europa registers.
* Het ontwikkelteam moet configureren en onderhouden van elke regionale implementatie met namen van installatiekopieën die verwijst naar het lokale register.
* Toegang tot het register moet worden geconfigureerd voor elke regio.

## <a name="benefits-of-geo-replication"></a>Voordelen van geo-replicatie

![Vanuit een geo-gerepliceerde register](media/container-registry-geo-replication/after-geo-replicate-pull.png)

Met behulp van de functie voor geo-replicatie van Azure Container Registry, deze voordelen gerealiseerd:

* Enkele registers beheren in alle regio's: `contoso.azurecr.io`
* Een eenmalige configuratie van implementaties beheren als alle regio's de dezelfde afbeeldings-URL gebruikt: `contoso.azurecr.io/public/products/web:1.2`
* Pushen naar een enkel register, terwijl ACR de geo-replicatie beheert, met inbegrip van regionale webhooks voor lokale meldingen

## <a name="configure-geo-replication"></a>Geo-replicatie configureren
Het configureren van geo-replicatie is net zo gemakkelijk als de regio's op een kaart te klikken.

Geo-replicatie is een functie van [Premium-registers](container-registry-skus.md) alleen. Als het register niet nog Premium, kunt u van Basic en Standard naar Premium in de [Azure-portal](https://portal.azure.com):

![Switch-SKU's in Azure portal](media/container-registry-skus/update-registry-sku.png)

Voor het configureren van geo-replicatie voor de Premium-register, moet u zich aanmelden bij Azure portal op http://portal.azure.com.

Navigeer naar uw Azure Container Registry, en selecteer **replicaties**:

![Replicaties in de containerregister-UI van Azure Portal](media/container-registry-geo-replication/registry-services.png)

Een kaart wordt weergegeven met alle huidige Azure-regio's:

 ![Kaart met regio's in Azure Portal](media/container-registry-geo-replication/registry-geo-map.png)

* Blauwe Zeshoeken vertegenwoordigen huidige replica 's
* Groene Zeshoeken vertegenwoordigen mogelijk replica regio 's
* Grijze Zeshoeken vertegenwoordigen de Azure-regio's nog niet beschikbaar voor replicatie

Voor het configureren van een replica, selecteert u een groene zeshoek en vervolgens **maken**:

 ![Gebruikersinterface voor het maken van een replicatie in Azure Portal](media/container-registry-geo-replication/create-replication.png)

Selecteer het groene Zeshoeken voor andere regio's voor het configureren van extra replica's, en klik op **maken**.

ACR begint installatiekopieën via de geconfigureerde replica's synchroniseren. Als u klaar bent, geeft de portal *gereed*. De status van de replica in de portal wordt niet automatisch bijgewerkt. Gebruik de vernieuwknop om de bijgewerkte status te bekijken.

## <a name="geo-replication-pricing"></a>Prijzen voor geo-replicatie

Geo-replicatie is een functie van de [Premium-SKU](container-registry-skus.md) van Azure Container Registry. Wanneer u een register naar de gewenste regio's repliceren, worden er kosten voor Premium-register voor elke regio.

In het voorgaande voorbeeld, Contoso geconsolideerd twee registers omlaag, replica's toe te voegen aan VS-Oost, Canada centraal en West-Europa. Contoso zou Premium vier keer per maand, zonder extra configuratie of management betalen. Elke regio haalt nu afbeeldingen lokaal, verbeterde prestaties, betrouwbaarheid zonder netwerk uitgaande kosten van VS-Canada West en VS-Oost.

## <a name="summary"></a>Samenvatting

Met geo-replicatie, kunt u uw regionale datacenters beheren als één globale cloud. Installatiekopieën door vele Azure-services heen worden gebruikt, kunt u profiteren van een enkel beheervlak terwijl dichtbij het netwerk, snelle en betrouwbare lokale installatiekopie ophaalt.

## <a name="next-steps"></a>Volgende stappen

Bekijk de driedelige zelfstudiereeks [Geo-replicatie in Azure Container Registry](container-registry-tutorial-prepare-registry.md). Helpen bij het maken van een geo-gerepliceerde register, het bouwen van een container en vervolgens implementeert u het met één `docker push` opdracht op meerdere regionale Web Apps for Containers-instanties.

> [!div class="nextstepaction"]
> [Geo-replicatie in Azure Container Registry](container-registry-tutorial-prepare-registry.md)
