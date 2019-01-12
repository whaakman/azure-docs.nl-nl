---
title: bestand opnemen
description: bestand opnemen
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 01/10/2019
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 47f66fc61fdd397350efb934bf21f488960a27a2
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54233056"
---
> [!NOTE]
> Gedeeltelijk aanvragen moeten doorgaans drie onderdelen:
> * Een **Content-Type** header:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * Een **Content-Disposition**:
>   * `form-data; name="metadata"`
> * De inhoud van het bestand te uploaden
>
> **Content-Type** en **Content-Disposition** , hangt af van het scenario voor gebruik.

Gedeeltelijk aanvragen via een programma kunnen worden gemaakt (via C#), via een REST-client, of een hulpprogramma zoals [Postman](https://docs.microsoft.com/azure/digital-twins/how-to-configure-postman#multi). REST-clienthulpprogramma's mogelijk verschillende niveaus van ondersteuning voor complexe meerdelige aanvragen. Controleer of welk hulpprogramma is het meest geschikt is voor uw behoeften.

> [!IMPORTANT]
> Gedeeltelijk aanvragen bij de Azure digitale dubbels Management API's doorgaans heeft twee onderdelen:
> * Blobmetagegevens (zoals een bijbehorende MIME-type) dat gedeclareerd **Content-Type** en **Content-Disposition**
> * BLOB-inhoud die met de ongestructureerde inhoud van een bestand dat moet worden geüpload
>
> Geen van de twee delen is vereist voor **PATCH** aanvragen. Beide zijn vereist voor **POST** of bewerkingen voor maken.

De [bezetting Quick Start-broncode](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/api/update.cs) bevat volledige C# voorbeelden die laten zien hoe meerdelige-aanvragen op basis van de Azure digitale dubbels Management API's.