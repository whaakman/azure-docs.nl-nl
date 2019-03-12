---
title: Best practices voor Azure Stack-validatie | Microsoft Docs
description: Dit artikel wordt beschreven aanbevolen procedures voor het gebruik van validatie als een Service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 568817c6e25952f15a396e5748d314187345945b
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57769043"
---
# <a name="create-an-oem-package"></a>Een OEM-pakket maken

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Het Azure Stack OEM-uitbreidingspakket is het mechanisme dat door welke OEM specifieke inhoud in Azure Stack-infrastructuur, voor gebruik in de implementatie, evenals de operationele processen zoals update, uitbreiding en veld vervanging wordt toegevoegd.

## <a name="creating-the-package"></a>Het maken van het pakket

Nadat u hebt gemaakt en gevalideerd, kan het OEM-uitbreidingspakket in VaaS worden gebruikt.  Zorg ervoor dat u de stappen hebt voltooid voordat u doorgaat, [het maken van een OEM-pakket](https://microsoft.sharepoint.com/:w:/r/teams/cloudsolutions/Sacramento/_layouts/15/Doc.aspx?sourcedoc=%7BD7406069-7661-419C-B3B1-B6A727AB3972%7D&file=Azure%20Stack%20OEM%20Extension%20Package.docx&action=default&mobileredirect=true). Het pakket wordt vervolgens naar Microsoft verzonden, samen met de testresultaten VaaS voor het ondertekenen van in de werkstroom voor validatie van het pakket. De volgende stappen wordt beschreven hoe u de gegenereerde bestanden te bundelen in één zip-bestand die VaaS kunnen gebruiken.

1. Identificeert u de volgende inhoud voor het pakket:
    - Een zip-bestand met de inhoud van pakket
    - Een manifestbestand met de naam `oemMetadata.xml`, die moeten identiek zijn met de inhoud van het bestand metadata.xml in de hoofdmap van de inhoud van het pakket.

2. Selecteer de inhoudsbestanden en maak een zip-bestand van de inhoud:

    ![ZIP-bestandsinhoud](media/vaas-create-oem-package-1.png) ![item Inhoud comprimeren](media/vaas-create-oem-package-2.png)

3. Naam van het resulterende bestand zodat u beschrijvende genoeg is voor u om te kunnen identificeren.

## <a name="verifying-the-contents"></a>De inhoud controleren

Bekijk deze voor het valideren van de structuur van het zipbestand, en controleer of er geen submappen zijn. De TLD heeft de gecomprimeerde inhoud. Hieronder ziet u een geldig pakket-structuur.
> [!IMPORTANT]
> Het inpakken van de bovenliggende map in plaats van de inhoud zorgt ervoor dat pakket niet ondertekenen.

![Inhoud van een correct ZIP-pakket](media/vaas-create-oem-package-3.png)

Het zip-bestand kan nu worden geüpload naar VaaS en ondertekend door Microsoft in de werkstroom van het pakket niet valideren.

## <a name="next-steps"></a>Volgende stappen

- [Een OEM-pakket valideren](azure-stack-vaas-validate-oem-package.md)
