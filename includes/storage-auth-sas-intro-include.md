---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/25/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 3af8077627d56ce44c5e2959e2c79b967b09d9e5
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "69011943"
---
Met een Shared Access Signature (SAS) kunt u beperkte toegang verlenen tot containers en blobs in uw opslag account. Wanneer u een SAS maakt, geeft u de beperkingen op, waaronder Azure Storage bronnen die een client mag openen, welke machtigingen ze hebben op deze resources en hoe lang de SAS geldig is.

Elke SAS is ondertekend met een sleutel. U kunt op een van de volgende twee manieren een SAS ondertekenen:

- Met een sleutel die is gemaakt met behulp van Azure Active Directory-referenties (Azure AD). Een SAS die is ondertekend met Azure AD-referenties is een *gebruikers* delegering sa's.
- Met de sleutel van het opslag account. Zowel een *service-SAS* als een *account-SAS* zijn ondertekend met de sleutel van het opslag account.
