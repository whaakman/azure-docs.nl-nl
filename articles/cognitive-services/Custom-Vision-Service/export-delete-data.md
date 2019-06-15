---
title: Exporteren of verwijderen van uw gegevens - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Informatie over het exporteren of uw gegevens in de Custom Vision Service verwijderen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: e662e61a9df45cf3d57d5698337a26b7b8fc55a3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60605478"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Exporteren of verwijderen van gebruikersgegevens in Custom Vision

Custom Vision verzamelt gebruikersgegevens als de service wilt gebruiken, maar klanten hebben volledige controle over het weergeven van, exporteren en verwijderen van hun gegevens met behulp van de visie van aangepaste [Training API's](https://go.microsoft.com/fwlink/?linkid=865446).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Voor informatie over het exporteren en verwijderen van de gebruikersgegevens in aangepaste Vision, Zie de volgende tabel.

| Gegevens | De exportbewerking | Verwijderbewerking |
| ---- | ---------------- | ---------------- |
| Accountgegevens (Abonnementssleutels) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Verwijderen met behulp van Azure portal (Azure-abonnementen). Of met behulp van de knop 'Uw Account verwijderen' in de instellingenpagina CustomVision.ai (abonnementen op Microsoft-Account) | 
| Details van de herhaling | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Details van de prestaties herhaling | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Lijst met herhalingen | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Projecten en projectdetails | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) en [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Installatiekopie-tags | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) en [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Installatiekopieën | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (uri biedt voor het downloaden van) en [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (uri biedt voor het downloaden van) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Geëxporteerde modellen | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Verwijderd bij het account verwijderen |
