---
title: bestand opnemen
description: bestand opnemen
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 09/17/2018
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: 7bac2b291bec2ceda118c877cde997a2fa9f9f37
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49322784"
---
1. Meld u aan bij [Azure Portal](http://portal.azure.com).

1. Klik in het navigatiepaneel aan de linkerkant op **Resource maken**. Zoek op *digital twins* en selecteer **Digital Twins (preview)**. Klik op **Create** om het implementatieproces te starten.

    ![Digital Twins maken](./media/create-digital-twins-portal/create-digital-twins.png)

1. In het deelvenster **Digital Twins** voert u de volgende informatie in:
   * **Resourcenaam**: geef uw instantie van Digital Twins een unieke naam.
   * **Abonnement**: kies het abonnement dat u wilt gebruiken om deze instantie van Digital Twins te maken. 
   * **Resourcegroep**: selecteer of maak een [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) voor de instantie van Digital Twins.
   * **Locatie**: selecteer de locatie die het dichtst bij uw apparaten in de buurt is.

    ![Digital Twins maken](./media/create-digital-twins-portal/create-digital-twins-param.png)

1. Controleer de informatie van uw Digital Twins en klik vervolgens op **Maken**. Het kan een paar minuten duren voordat uw instantie van Digital Twins is gemaakt. U kunt de voortgang bewaken via het deelvenster **Meldingen**.

1. Open het deelvenster **Overzicht** van de instantie van Digital Twins. Zoals u ziet wordt er een koppeling weergegeven onder **Beheer API**.

    1. De indeling van de URL van de **Beheer API** is: **_https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger_**. Deze URL leidt u naar de documentatie van de REST API van Azure Digital Twins, die van toepassing is op uw instantie. Zie [Het gebruik van Azure Digital Twins Swagger](../articles/digital-twins/how-to-use-swagger.md) voor informatie over hoe u deze API-documentatie dient te lezen en gebruiken.

    1. Wijzig de URL van de **Beheer API** in deze indeling: **_https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/_**. Uw toepassing gebruikt de aangepaste URL als de basis-URL voor toegang tot uw instantie. Kopieer deze gewijzigde URL naar een tijdelijk bestand. U hebt deze in de volgende sectie nodig.

    ![Beheer API’s](./media/create-digital-twins-portal/digital-twins-management-api.png)