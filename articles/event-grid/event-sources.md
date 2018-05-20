---
title: Azure Event raster voor de bronnen van gebeurtenissen
description: Beschrijft de ondersteunde gebeurtenisbronnen voor Azure Event raster
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: tomfitz
ms.openlocfilehash: f9c3bcb6b92b43fe5b5bad72c99e6ce199c17448
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="event-sources-in-azure-event-grid"></a>Bronnen van gebeurtenissen in Azure gebeurtenis raster

Een gebeurtenisbron is waar de gebeurtenis plaatsvindt. Verschillende Azure-services worden automatisch geconfigureerd voor het verzenden van gebeurtenissen. U kunt ook aangepaste toepassingen die gebeurtenissen verzenden maken. Aangepaste toepassingen hoeft niet te worden gehost in Azure met gebeurtenis raster voor gebeurtenisdistributie.

Dit artikel bevat koppelingen naar inhoud voor elke gebeurtenisbron.

## <a name="azure-subscriptions"></a>Azure-abonnementen

Zich abonneren op Azure-abonnementen gebeurtenissen om te reageren op wijzigingen in de resources in een Azure-abonnement.

|Titel |Beschrijving  |
|---------|---------|
| [Azure Automation integreren met gebeurtenis raster en Microsoft-Teams](ensure-tags-exists-on-new-virtual-machines.md) |Een virtuele machine maken, waarmee een gebeurtenis wordt verzonden. Een Automation-runbook die labels van de virtuele machine en een bericht dat wordt verzonden naar een Microsoft-Teams-kanaal wordt geactiveerd voor de gebeurtenis wordt geactiveerd. |
| [Gebeurtenis-schema](event-schema-subscriptions.md) | Bevat velden in de Azure-abonnement gebeurtenissen. |

## <a name="custom-topics"></a>Aangepaste-onderwerpen

Zich abonneren op aangepaste onderwerpen om te reageren op toepassingsgebeurtenissen.

|Titel  |Beschrijving  |
|---------|---------|
| [Maken en het routeren van aangepaste gebeurtenissen met Azure CLI](custom-event-quickstart.md) | Laat zien hoe u met Azure CLI aangepaste gebeurtenissen verzendt. |
| [Maken en het routeren van aangepaste gebeurtenissen met Azure PowerShell](custom-event-quickstart-powershell.md) | Laat zien hoe u met Azure PowerShell aangepaste gebeurtenissen verzendt. |
| [Maken en het routeren van aangepaste gebeurtenissen met de Azure-portal](custom-event-quickstart-portal.md) | Laat zien hoe u aangepaste gebeurtenissen verzenden via de portal. |
| [Post een bericht naar aangepaste onderwerp](post-to-custom-topic.md) | Laat zien hoe u een gebeurtenis naar een aangepaste onderwerp plaatsen. |
| [Aangepaste gebeurtenissen van de route naar Azure Queue storage](custom-event-to-queue-storage.md) | Beschrijft hoe u aangepaste gebeurtenissen verzenden naar een Queue storage. |
| [Gebeurtenis-schema](event-schema.md) | Bevat velden in aangepaste gebeurtenissen. |

## <a name="event-hubs"></a>Event Hubs

Zich abonneren op gebeurtenissen van Event Hubs om te reageren voor het vastleggen van gebeurtenissen van bestand.

|Titel  |Beschrijving  |
|---------|---------|
| [Big data streamen naar een datawarehouse](event-grid-event-hubs-integration.md) | Wanneer Event Hubs een bestand opnemen maakt, verzendt raster gebeurtenis een gebeurtenis voor een functie-app. De app haalt de Capture-bestand en gegevens migreert naar een datawarehouse. |
| [Gebeurtenis-schema](event-schema-event-hubs.md) | Bevat velden in de gebeurtenissen van Event Hubs. |

## <a name="iot-hub"></a>IoT Hub

Abonneren met IoT Hub gebeurtenissen om te reageren op apparaat gemaakt en gebeurtenissen verwijderd.

|Titel  |Beschrijving  |
|---------|---------|
| [Verzenden van e-mailmeldingen over gebeurtenissen van Azure IoT Hub met Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Een logische app verzendt een e-mailmelding telkens wanneer een apparaat wordt toegevoegd aan uw IoT-hub. |
| [Reageren op gebeurtenissen van de IoT Hub met behulp van de gebeurtenis raster acties starten](../iot-hub/iot-hub-event-grid.md) | Overzicht van Iot Hubs integreren met gebeurtenis raster. |
| [Gebeurtenis-schema](event-schema-iot-hub.md) | Velden weergegeven van IoT Hub gebeurtenissen. |

## <a name="media-services"></a>Media Services

Zich abonneren op Media Services-gebeurtenissen te reageren op statusgebeurtenissen van de taak.

|Titel  |Beschrijving  |
|---------|---------|
| [Reageren op Media Services-gebeurtenissen](../media-services/latest/reacting-to-media-services-events.md) | Overzicht van het Media Services integreren met gebeurtenis raster. |
| [Azure Media Services-gebeurtenissen te routeren naar een aangepaste website-eindpunt met CLI](../media-services/latest/job-state-events-cli-how-to.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Laat zien hoe het verzenden van gebeurtenissen van Media Services. |
| [Gebeurtenis-schema](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Bevat velden in Media Services-gebeurtenissen. |

## <a name="resource-groups"></a>Resourcegroepen

Zich abonneren op resource group-gebeurtenissen te reageren op wijzigingen in de resources in een resourcegroep.

|Titel  |Beschrijving  |
|---------|---------|
| [Monitor virtual machine changes with Azure Event Grid and Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) (Wijzigingen in virtuele machines bewaken met Azure Event Grid en Logic Apps) | Een logische app registreert wijzigingen aan een virtuele machine en verzonden e-mailberichten over deze wijzigingen. |
| [Gebeurtenisschema](event-schema-resource-groups.md) | Velden weergegeven van de resource gebeurtenissen groeperen. |

## <a name="service-bus"></a>Service Bus

Zich abonneren op Service Bus-gebeurtenissen te reageren op berichten zonder een actieve listener.

|Titel  |Beschrijving  |
|---------|---------|
| [Azure Service Bus Azure gebeurtenis raster integratie-voorbeelden](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Gebeurtenis raster verzendt berichten van Service Bus-onderwerp functioneren app en logische app. |
| [Azure Service Bus Event raster integratie-overzicht](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md) | Overzicht van Service Bus integreren met gebeurtenis raster. |
| [Gebeurtenis-schema](event-schema-service-bus.md) | Bevat velden in Service Bus-gebeurtenissen. |

## <a name="storage"></a>Storage

Zich abonneren op gebeurtenissen voor Blob Storage blob gemaakt en verwijderde gebeurtenissen reageren op.

|Titel  |Beschrijving  |
|---------|---------|
| [Blob storage gebeurtenissen routeren naar een aangepaste website-eindpunt met Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Laat zien hoe Azure CLI gebruiken voor het verzenden van gebeurtenissen voor blob-opslag. |
| [Blob storage gebeurtenissen routeren naar een aangepaste website-eindpunt met PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Laat zien hoe u Azure PowerShell gebruiken voor het verzenden van gebeurtenissen voor blob-opslag. |
| [Reageren op gebeurtenissen van Blob Storage](../storage/blobs/storage-blob-event-overview.md) | Overzicht van de integratie van Blob-opslag met gebeurtenis raster. |
| [Gebeurtenis-schema](event-schema-blob-storage.md) | Bevat velden in Blob Storage-gebeurtenissen. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot gebeurtenis raster, [over gebeurtenis raster](overview.md).
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).
