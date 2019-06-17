---
author: robinsh
manager: philmea
ms.author: robinsh
ms.topic: include
ms.date: 05/20/2019
ms.openlocfilehash: c164433efc6a34a3a06676a3145feb18d3de80b9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66249010"
---
## <a name="add-a-consumer-group-to-your-iot-hub"></a>Een consumentengroep toevoegen aan uw IoT-hub

[Consumentengroepen](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers) bieden onafhankelijke weergaven in de gebeurtenisstroom waarmee apps en gegevens uit hetzelfde Event Hub-eindpunt onafhankelijk van elkaar te gebruiken Azure-services. In deze sectie voegt u een consumergroep naar uw IoT-hub ingebouwde eindpunt dat verderop in deze zelfstudie wordt gebruikt om gegevens ophalen uit het eindpunt.

Een consumergroep toevoegen aan uw IoT-hub, de volgende stappen uit:

1. Open uw IoT-hub in [Azure Portal](https://portal.azure.com/).

2. Selecteer in het linkerdeelvenster **ingebouwde eindpunten**, selecteer **gebeurtenissen** in het rechterdeelvenster en voer een naam onder **consumentengroepen**. Selecteer **Opslaan**.

   ![Een consumentengroep in uw IoT-hub maken](./media/iot-hub-get-started-create-consumer-group/iot-hub-create-consumer-group-azure.png)
