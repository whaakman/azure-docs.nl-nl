---
title: Exporteren of verwijderen van uw gegevens in aangepaste visie, cognitieve Azure-Services | Microsoft Docs
description: Informatie over het exporteren of verwijderen van uw gegevens in aangepaste visie.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 49fc49b3181f56c23167cfbae18911e7db441f8c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345740"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Exporteren of gebruikersgegevens in aangepaste visie verwijderen

Inhoud beheerder verzamelt gebruikersgegevens als de service wilt gebruiken, maar klanten hebben volledige controle over het weergeven en verwijderen van hun gegevens exporteren met de Service voor aangepaste visie [Training API](https://go.microsoft.com/fwlink/?linkid=865446).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Zie de volgende tabel voor meer informatie over het exporteren en verwijderen van gebruikersgegevens in aangepaste visie.

| Gegevens | De exportbewerking | Verwijderbewerking |
| ---- | ---------------- | ---------------- |
| Accountgegevens (abonnement sleutels) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Verwijder met behulp van Azure portal (Azure-abonnementen). Of met de knop 'Uw Account verwijderen' in de instellingenpagina CustomVision.ai (abonnementen van Microsoft-Account) |
| Details van de herhaling | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Details van de prestaties herhaling | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Lijst met herhalingen | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Projecten en projectgegevens | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) en [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) |
| Labels van de afbeelding | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) en [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) |
| Installatiekopieën | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (uri biedt voor het downloaden van) en [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (uri biedt voor het downloaden van) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) |
| Geëxporteerde modellen | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Bij verwijdering van de account wordt verwijderd |
