---
title: bestand opnemen
description: bestand opnemen
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 01/02/2019
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 1c6579776b86decb78c172578cbe55a66c05d78f
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54026550"
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

Gedeeltelijk aanvragen via een programma kunnen worden gemaakt (via C#), via een REST-client, of een hulpprogramma zoals [Postman](https://www.getpostman.com/). REST-clienthulpprogramma's mogelijk verschillende niveaus van ondersteuning voor complexe meerdelige aanvragen. Controleer of welk hulpprogramma is het meest geschikt is voor uw behoeften.

> [!IMPORTANT]
> Gedeeltelijk aanvragen bij de Azure digitale dubbels Management API's hebben twee delen:
> * Blobmetagegevens (zoals een bijbehorende MIME-type) dat gedeclareerd **Content-Type** en **Content-Disposition**
> * BLOB-inhoud die met de ongestructureerde inhoud van een bestand dat moet worden geüpload
>
> Geen van de twee delen is vereist voor **PATCH** aanvragen. Beide zijn vereist voor **POST** of bewerkingen voor maken.
