---
title: Bronnen van Azure Event Grid-gebeurtenissen
description: Beschrijving van ondersteunde gebeurtenisbronnen voor Azure Event Grid
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: tomfitz
ms.openlocfilehash: adef9d2f2d859c62d3b3b3a542536698fa668f9a
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498297"
---
# <a name="event-sources-in-azure-event-grid"></a>Bronnen van gebeurtenissen in Azure Event Grid

Een gebeurtenisbron is waar de gebeurtenis plaatsvindt. Meerdere Azure-services worden automatisch geconfigureerd voor het verzenden van gebeurtenissen. U kunt ook aangepaste toepassingen die gebeurtenissen verzenden maken. Aangepaste toepassingen hoeft te worden gehost in Azure Event Grid gebruiken om gebeurtenissen te worden gedistribueerd.

In dit artikel bevat koppelingen naar inhoud voor de bron van elke gebeurtenis.

## <a name="azure-subscriptions"></a>Azure-abonnementen

Abonneren op gebeurtenissen van de Azure-abonnementen om te reageren op wijzigingen in de resources in een Azure-abonnement.

|Titel |Beschrijving  |
|---------|---------|
| [Zelfstudie: Azure Automation met Event Grid en Microsoft Teams](ensure-tags-exists-on-new-virtual-machines.md) |Maak een virtuele machine, waarmee een gebeurtenis wordt verzonden. De gebeurtenis activeert een Automation-runbook dat labels van de virtuele machine en een bericht dat wordt verzonden naar een Microsoft Teams-kanaal wordt geactiveerd. |
| [Hoe: abonneren op gebeurtenissen tot en met portal](subscribe-through-portal.md) | De portal gebruiken om u te abonneren op gebeurtenissen voor een Azure-abonnement. |
| [Azure CLI: abonneren op gebeurtenissen voor een Azure-abonnement](./scripts/event-grid-cli-azure-subscription.md) |Voorbeeld van een script maakt u een Event Grid-abonnement naar een Azure-abonnement en verzendt gebeurtenissen naar een WebHook. |
| [PowerShell: abonneren op gebeurtenissen voor een Azure-abonnement](./scripts/event-grid-powershell-azure-subscription.md)| Voorbeeld van een script maakt u een Event Grid-abonnement naar een Azure-abonnement en verzendt gebeurtenissen naar een WebHook. |
| [Gebeurtenisschema](event-schema-subscriptions.md) | Velden weergegeven van gebeurtenissen voor Azure-abonnement. |

## <a name="container-registry"></a>Container Registry

Abonneren op gebeurtenissen van de Container Registry om te reageren op wijzigingen in afbeeldingen.

|Titel |Beschrijving  |
|---------|---------|
| [Snelstartgids: container registry gebeurtenissen verzenden](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Laat zien hoe u Azure CLI gebruiken voor het verzenden van gebeurtenissen voor Container Registry. |
| [Gebeurtenisschema](event-schema-container-registry.md) | Velden worden weergegeven in de Container Registry-gebeurtenissen. |

## <a name="custom-topics"></a>Aangepaste onderwerpen

Abonneren op aangepaste onderwerpen om te reageren op toepassingsgebeurtenissen.

|Titel  |Beschrijving  |
|---------|---------|
| [QuickStart: maken en routeren van aangepaste gebeurtenissen met Azure CLI](custom-event-quickstart.md) | Laat zien hoe u Azure CLI gebruiken voor het verzenden van aangepaste gebeurtenissen. |
| [QuickStart: maken en routeren van aangepaste gebeurtenissen met Azure PowerShell](custom-event-quickstart-powershell.md) | Laat zien hoe u Azure PowerShell gebruiken voor het verzenden van aangepaste gebeurtenissen. |
| [QuickStart: maken en routeren van aangepaste gebeurtenissen met de Azure-portal](custom-event-quickstart-portal.md) | Laat zien hoe het gebruik van de portal voor het verzenden van aangepaste gebeurtenissen. |
| [Snelstartgids: aangepaste gebeurtenissen routeren naar Azure Queue storage](custom-event-to-queue-storage.md) | Beschrijft hoe u aangepaste gebeurtenissen verzenden naar een Queue storage. |
| [Hoe: plaatsen op aangepast onderwerp](post-to-custom-topic.md) | Laat zien hoe u een gebeurtenis om een aangepast onderwerp te plaatsen. |
| [Azure CLI: aangepast Event Grid-onderwerp maken](./scripts/event-grid-cli-create-custom-topic.md)|Voorbeeld van een script dat wordt gemaakt van een aangepast onderwerp. Het script wordt het eindpunt en een sleutel opgehaald.|
| [Azure CLI: abonneren op gebeurtenissen voor een aangepast onderwerp](./scripts/event-grid-cli-subscribe-custom-topic.md)|Voorbeeld van een script dat wordt gemaakt van een abonnement voor een aangepast onderwerp. Verzendt gebeurtenissen naar een WebHook.|
| [PowerShell: aangepast Event Grid-onderwerp maken](./scripts/event-grid-powershell-create-custom-topic.md)|Voorbeeld van een script dat wordt gemaakt van een aangepast onderwerp. Het script wordt het eindpunt en een sleutel opgehaald.|
| [PowerShell: abonneren op gebeurtenissen voor een aangepast onderwerp](./scripts/event-grid-powershell-subscribe-custom-topic.md)|Voorbeeld van een script dat wordt gemaakt van een abonnement voor een aangepast onderwerp. Verzendt gebeurtenissen naar een WebHook.|
| [Resource Manager-sjabloon: aangepast onderwerp en WebHook-eindpunt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) | Een Resource Manager-sjabloon waarmee u een aangepast onderwerp en abonnement op dat aangepaste onderwerp maakt. Verzendt gebeurtenissen naar een WebHook. |
|
| [Resource Manager-sjabloon: aangepast onderwerp en het eindpunt van de Event Hubs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Een Resource Manager-sjabloon maakt u een abonnement voor een aangepast onderwerp. Verzendt gebeurtenissen naar een Azure Event Hubs. |
| [Gebeurtenisschema](event-schema.md) | Velden weergegeven in aangepaste gebeurtenissen. |

## <a name="event-hubs"></a>Event Hubs

Abonneren op gebeurtenissen van Event Hubs voor het vastleggen van bestand gebeurtenissen reageren. Eventhubs kunnen fungeren als een bron van gebeurtenis- of gebeurtenis-handler. De volgende artikelen laten zien hoe u Event Hubs gebruikt als een bron.

|Titel  |Beschrijving  |
|---------|---------|
| [Zelfstudie: big data streamen naar een datawarehouse](event-grid-event-hubs-integration.md) | Wanneer Event Hubs een bestand vastleggen maakt, verzendt Event Grid een gebeurtenis aan een functie-app. De app haalt de Capture-bestand en migreert gegevens naar een datawarehouse. |
| [Gebeurtenisschema](event-schema-event-hubs.md) | Velden weergegeven van gebeurtenissen van Event Hubs. |

Zie voor meer voorbeelden van Event Hubs als een handler [Event Hubs-handler](event-handlers.md#event-hubs).

## <a name="iot-hub"></a>IoT Hub

Abonneren op gebeurtenissen van IoT Hub om te reageren op het apparaat is gemaakt, verwijderd, verbonden en niet-verbonden gebeurtenissen.

|Titel  |Beschrijving  |
|---------|---------|
| [Verzenden van e-mailmeldingen over Azure IoT Hub-gebeurtenissen met Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Een logische app verzendt een e-mailmelding telkens wanneer een apparaat wordt toegevoegd aan uw IoT-Hub. |
| [Reageren op gebeurtenissen van IoT-Hub met behulp van Event Grid om acties starten](../iot-hub/iot-hub-event-grid.md) | Overzicht van de integratie van IoT-Hub met Event Grid. |
| [Gebeurtenisschema](event-schema-iot-hub.md) | Velden weergegeven van IoT-Hub gebeurtenissen. |
| [Apparaat is verbonden en het apparaat verbroken gebeurtenissen](../iot-hub/iot-hub-how-to-order-connection-state-events.md) | Laat zien hoe apparaatverbinding statusgebeurtenissen rangschikken. |

## <a name="media-services"></a>Media Services

Abonneren op gebeurtenissen van Media Services om te reageren op gebeurtenissen die taak staat.

|Titel  |Beschrijving  |
|---------|---------|
| [Overzicht: reageren op gebeurtenissen van de Media Services](../media-services/latest/reacting-to-media-services-events.md) | Overzicht van Media Services integreren met Event Grid. |
| [Zelfstudie: Azure Media Services-gebeurtenissen routeren naar een aangepaste web-eindpunt met behulp van CLI](../media-services/latest/job-state-events-cli-how-to.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Laat zien hoe voor het verzenden van gebeurtenissen van Media Services. |
| [Gebeurtenisschema](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Velden bevat in Media Services-gebeurtenissen. |

## <a name="resource-groups"></a>Resourcegroepen

Abonneren op gebeurtenissen van de resource-groep om te reageren op wijzigingen in de resources in een resourcegroep.

|Titel  |Beschrijving  |
|---------|---------|
| [Zelfstudie: wijzigingen van de virtuele machine met Azure Event Grid en Logic Apps bewaken](monitor-virtual-machine-changes-event-grid-logic-app.md) | Een logische app bewaakt van wijzigingen aan een virtuele machine en e-mailberichten over deze wijzigingen verzendt. |
| [Azure CLI: abonneren op gebeurtenissen voor een resourcegroep](./scripts/event-grid-cli-resource-group.md)| Voorbeeld van een script dat ze zich op gebeurtenissen voor een resourcegroep abonneren. Verzendt gebeurtenissen naar een WebHook. |
| [Azure CLI: abonneren op gebeurtenissen voor een resourcegroep en -filter voor een resource](./scripts/event-grid-cli-resource-group-filter.md) | Voorbeeld van een script dat ze zich abonneren op gebeurtenissen voor een resourcegroep en gebeurtenissen voor één resource gefilterd. |
| [PowerShell: abonneren op gebeurtenissen voor een resourcegroep](./scripts/event-grid-powershell-resource-group.md) | Voorbeeld van een script dat ze zich op gebeurtenissen voor een resourcegroep abonneren. Verzendt gebeurtenissen naar een WebHook. |
| [PowerShell: abonneren op gebeurtenissen voor een resourcegroep en -filter voor een resource](./scripts/event-grid-powershell-resource-group-filter.md) | Voorbeeld van een script dat ze zich abonneren op gebeurtenissen voor een resourcegroep en gebeurtenissen voor één resource gefilterd. |
| [Resource Manager-sjabloon: resourceabonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) | Hiermee abonneert u zich op gebeurtenissen voor een Azure-abonnement of resource-groep. Verzendt gebeurtenissen naar een WebHook. |
| [Gebeurtenisschema](event-schema-resource-groups.md) | Velden weergegeven van de resource group-gebeurtenissen. |

## <a name="service-bus"></a>Service Bus

Abonneren op Service Bus-gebeurtenissen om te reageren op berichten zonder een actieve listener.

|Titel  |Beschrijving  |
|---------|---------|
| [Zelfstudie: Azure Service Bus met voorbeelden van de Azure Event Grid-integratie](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid verzendt berichten van Service Bus-onderwerp functioneren app en de logische app. |
| [Overzicht: Azure Service Bus met Event Grid-integratie](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Overzicht van de integratie van Service Bus met Event Grid. |
| [Gebeurtenisschema](event-schema-service-bus.md) | Velden weergeven in Service Bus-gebeurtenissen |

## <a name="storage"></a>Storage

Abonneren op gebeurtenissen van Blob-opslag om te reageren op blob-gebeurtenissen gemaakt en verwijderd.

|Titel  |Beschrijving  |
|---------|---------|
| [Snelstartgids: Blob-opslaggebeurtenissen doorsturen naar een aangepaste web-eindpunt met Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Laat zien hoe u Azure CLI gebruiken voor het verzenden van gebeurtenissen van blob storage naar een WebHook. |
| [Snelstartgids: Blob-opslaggebeurtenissen doorsturen naar een aangepaste web-eindpunt met PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Laat zien hoe u Azure PowerShell gebruikt voor het verzenden van gebeurtenissen van blob storage naar een WebHook. |
| [QuickStart: maken en routeren van gebeurtenissen van Blob storage met Azure portal](blob-event-quickstart-portal.md) | Laat zien hoe het gebruik van de portal voor het verzenden van gebeurtenissen van blob storage naar een WebHook. |
| [Azure CLI: abonneren op gebeurtenissen voor een Blob storage-account](./scripts/event-grid-cli-blob.md) | Voorbeeld van een script dat ze zich op gebeurtenissen voor een Blob storage-account abonneren. Wordt de gebeurtenis verzonden naar een WebHook. |
| [PowerShell: abonneren op gebeurtenissen voor een Blob storage-account](./scripts/event-grid-powershell-blob.md) | Voorbeeld van een script dat ze zich op gebeurtenissen voor een Blob storage-account abonneren. Wordt de gebeurtenis verzonden naar een WebHook. |
| [Resource Manager-sjabloon: Blob-opslag en een abonnement maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage) | Hiermee implementeert u een Azure Blob-opslagaccount en abonneert u zich op gebeurtenissen van die opslagaccount. Verzendt gebeurtenissen naar een WebHook. |
| [Overzicht: reageren op gebeurtenissen van Blob storage](../storage/blobs/storage-blob-event-overview.md) | Overzicht van de integratie van Blob-opslag met Event Grid. |
| [Gebeurtenisschema](event-schema-blob-storage.md) | Velden weergegeven van gebeurtenissen van Blob Storage. |

## <a name="next-steps"></a>Volgende stappen

* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).
