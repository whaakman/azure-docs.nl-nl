---
title: Azure Event raster gebeurtenis-handlers
description: Beschrijft de ondersteunde gebeurtenis-handlers voor Azure Event raster
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: tomfitz
ms.openlocfilehash: 24cecdc65c45d5d1ee5443740d9874ccfd74e387
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627712"
---
# <a name="event-handlers-in-azure-event-grid"></a>Gebeurtenis-handlers in Azure gebeurtenis raster

Een gebeurtenis-handler is de plaats waar de gebeurtenis wordt verzonden. De handler duurt enkele verdere actie voor het verwerken van de gebeurtenis. Verschillende Azure-services worden automatisch geconfigureerd om gebeurtenissen te verwerken. U kunt ook een webhook gebruiken voor het verwerken van gebeurtenissen. De webhook hoeft niet te worden gehost in Azure om gebeurtenissen te verwerken. Gebeurtenis raster ondersteunt alleen HTTPS-webhook-eindpunten.

Dit artikel bevat koppelingen naar inhoud voor elke gebeurtenis-handler.

## <a name="azure-automation"></a>Azure Automation

Gebruik Azure Automation om gebeurtenissen te verwerken met geautomatiseerde runbooks.

|Titel  |Beschrijving  |
|---------|---------|
|[Azure Automation integreren met gebeurtenis raster en Microsoft-Teams](ensure-tags-exists-on-new-virtual-machines.md) |Een virtuele machine maken, waarmee een gebeurtenis wordt verzonden. Een Automation-runbook die labels van de virtuele machine en een bericht dat wordt verzonden naar een Microsoft-Teams-kanaal wordt geactiveerd voor de gebeurtenis wordt geactiveerd. |

## <a name="azure-functions"></a>Azure Functions

Gebruik Azure Functions voor zonder server reactie op gebeurtenissen.

Wanneer u Azure Functions als de handler gebruikt, gebruikt u de Event Grid-trigger in plaats van algemene HTTP-triggers. Functie-triggers van Event Grid worden namelijk automatisch gevalideerd. Bij gebruik van algemene HTTP-triggers moet u een [validatie-antwoord](security-authentication.md#webhook-event-delivery) implementeren.

|Titel  |Beschrijving  |
|---------|---------|
| [Gebeurtenis raster trigger voor Azure Functions](../azure-functions/functions-bindings-event-grid.md) | Overzicht van het gebruik van de gebeurtenis raster trigger in functies. |
| [Het wijzigen van het formaat van geüploade afbeeldingen automatiseren met behulp van Event Grid](resize-images-on-storage-blob-upload-event.md) | Gebruikers uploaden afbeeldingen via web-app naar storage-account. Wanneer een blob storage is gemaakt, verzendt raster gebeurtenis een gebeurtenis naar de functie-app, die de grootte van de installatiekopie van het geüploade wordt aangepast. |
| [Big data streamen naar een datawarehouse](event-grid-event-hubs-integration.md) | Wanneer Event Hubs een bestand opnemen maakt, verzendt raster gebeurtenis een gebeurtenis voor een functie-app. De app haalt de Capture-bestand en gegevens migreert naar een datawarehouse. |
| [Azure Service Bus Azure gebeurtenis raster integratie-voorbeelden](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Gebeurtenis raster verzendt berichten van Service Bus-onderwerp functioneren app en logische app. |

## <a name="hybrid-connections"></a>Hybride verbindingen

Azure Relay hybride verbindingen gebruiken voor het verzenden van gebeurtenissen naar toepassingen die binnen een bedrijfsnetwerk en een eindpunt dat openbaar toegankelijk niet hebben.

|Titel  |Beschrijving  |
|---------|---------|
| [Verzenden van gebeurtenissen hybride verbinding](custom-event-to-hybrid-connection.md) | Verzendt een aangepaste gebeurtenis naar een bestaande hybride verbinding voor verwerking door een listener-toepassing. |

## <a name="logic-apps"></a>Logic Apps

Logische Apps gebruiken om bedrijfsprocessen als reactie op gebeurtenissen te automatiseren.

|Titel  |Beschrijving  |
|---------|---------|
| [Monitor virtual machine changes with Azure Event Grid and Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) (Wijzigingen in virtuele machines bewaken met Azure Event Grid en Logic Apps) | Een logische app registreert wijzigingen aan een virtuele machine en verzonden e-mailberichten over deze wijzigingen. |
| [Verzenden van e-mailmeldingen over gebeurtenissen van Azure IoT Hub met Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Een logische app verzendt een e-mailmelding telkens wanneer een apparaat wordt toegevoegd aan uw IoT-hub. |
| [Azure Service Bus Azure gebeurtenis raster integratie-voorbeelden](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Gebeurtenis raster verzendt berichten van Service Bus-onderwerp functioneren app en logische app. |

## <a name="queue-storage"></a>Queue Storage

Queue storage gebruiken voor het ontvangen van gebeurtenissen die moeten worden opgehaald.

|Titel  |Beschrijving  |
|---------|---------|
| [Aangepaste gebeurtenissen van de route naar Azure Queue storage met Azure CLI en gebeurtenis raster](custom-event-to-queue-storage.md) | Beschrijft hoe u aangepaste gebeurtenissen verzenden naar een Queue storage. |

## <a name="webhooks"></a>WebHooks

Gebruik webhooks voor aanpasbare eindpunten die op gebeurtenissen reageren.

|Titel  |Beschrijving  |
|---------|---------|
| [Ontvangen van gebeurtenissen naar een HTTP-eindpunt](receive-events.md) | Beschrijft hoe een HTTP-eindpunt voor het ontvangen van gebeurtenissen van een gebeurtenisabonnement en ontvangen en gebeurtenissen deserialiseren valideren. |

## <a name="next-steps"></a>Volgende stappen

* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).
