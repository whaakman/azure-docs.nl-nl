---
title: Exporteren of verwijderen van uw gegevens - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Informatie over het exporteren of uw gegevens in de Custom Vision Service verwijderen.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: c98c72101d893f107d4a6c3185836a75bfe41007
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367488"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Exporteren of verwijderen van gebruikersgegevens in Custom Vision

Content Moderator verzamelt gebruikersgegevens als de service wilt gebruiken, maar klanten hebben volledige controle over het weergeven van, exporteren en verwijderen van hun gegevens met behulp van de Custom Vision Service [Training API](https://go.microsoft.com/fwlink/?linkid=865446).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Zie de volgende tabel voor meer informatie over het exporteren en verwijderen van de gebruikersgegevens in aangepaste Vision.

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
