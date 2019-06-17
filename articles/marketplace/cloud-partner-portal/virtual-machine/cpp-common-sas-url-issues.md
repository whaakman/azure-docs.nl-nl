---
title: SAS-URL voor veelvoorkomende problemen en oplossingen voor de Azure Marketplace
description: Lijst met algemene problemen rond de handtekening voor gedeelde toegang URI's en mogelijke oplossingen.
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
ms.service: marketplace
ms.topic: article
ms.date: 09/27/2018
ms.author: pabutler
ms.openlocfilehash: 4f2770312624e1ca4c939ade458a451eb03f9d20
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64938383"
---
# <a name="common-sas-url-issues-and-fixes"></a>Veelvoorkomende problemen met SAS-URL en oplossingen

De volgende tabel bevat enkele van de algemene problemen bij het werken met handtekeningen voor gedeelde toegang (die worden gebruikt om te bepalen en de geüploade VHD's voor uw oplossing delen), samen met voorgestelde oplossingen.

| **Probleem** | **Foutbericht** | **Fix** | 
| --------- | ------------------- | ------- | 
| &emsp;  *Fout bij het kopiëren van afbeeldingen* |  |  |
| '? ' is niet gevonden in de SAS-URL | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Update de SAS-URL met behulp van aanbevolen hulpprogramma's. |
| parameters voor "st" en "se" niet in de SAS-URL | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Bijwerken van de SAS-URL met de juiste **begindatum** en **einddatum** waarden. | 
| “sp=rl” not in SAS URL | `Failure: Copying Images. Not able to download blob using provided SAS Uri` | Bijwerken van de SAS-URL met de machtigingen die zijn ingesteld als `Read` en `List`. | 
| SAS-URL heeft spaties in VHD-naam | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | De SAS-URL als u wilt verwijderen van spaties worden bijgewerkt. |
| SAS-URL-autorisatie-fout | `Failure: Copying Images. Not able to download blob due to authorization error` | Controleren en corrigeren van de SAS-URI-indeling. Genereer een nieuwe indien nodig. |
| SAS-URL "st" en 'se'-parameters hebben geen volledige datum / tijd-specificatie | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | SAS-URL **begindatum** en **einddatum** parameters (`st` en `se` subtekenreeksen) zijn vereist om volledige datum-/ tijdindeling, zoals `11-02-2017T00:00:00Z`. Verkorte versies zijn niet geldig. (Sommige opdrachten in de Azure CLI kunnen afgekorte waarden genereren standaard.) | 
|  |  |  |

Zie voor meer informatie, [Using shared access signatures (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).
