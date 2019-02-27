---
title: Gegevens met Azure App-configuratie importeren of exporteren | Microsoft Docs
description: Meer informatie over het importeren of exporteren van gegevens naar of van Azure App-configuratie
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 33ba01440f599e2f019db46c304b0658632d9342
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885282"
---
# <a name="import-or-export-configuration-data"></a>Configuratiegegevens importeren of exporteren

Azure App ondersteunt configuratiegegevens importeren en exporteren van bewerkingen. Dit kunnen u werken met de configuratiegegevens in bulk als goed als exchange-gegevens tussen uw app-configuratie opslaan en project-code. U kunt bijvoorbeeld één app-configuratie-winkel voor testen en een andere voor productie en kopieer toepassingsinstellingen tussen deze via een bestand instellen zodat u niet moet de gegevens tweemaal op enter.

Dit artikel bevat een handleiding voor het importeren en exporteren van gegevens met App-configuratie.

## <a name="import-data"></a>Gegevens importeren

Import worden configuratie opslaan van gegevens in de App-configuratie van een bestaande gegevensbron, in plaats van handmatig in te voeren. U kunt de importfunctie gebruiken om gegevens te migreren naar een app opslaan van de configuratie- of statistische gegevens uit meerdere bronnen. Ondersteuning voor App-configuratie importeren uit een bestand met JSON, YAML of eigenschappen.

U kunt importeren gegevens met behulp van de [Azure-portal](https://aka.ms/azconfig/portal) of [Azure CLI](./scripts/cli-import.md). Voer de volgende stappen uit vanuit de Azure-portal:

1. Blader naar de opslaan van de configuratie van uw app en klikt u op **Import/Export**.

2. In de **importeren** tabblad **bron service** en **configuratiebestand**.

3. Kies **voor taal** en **bestandstype**.

4. Klik op de **map** pictogram en blader naar het bestand te importeren.

    ![Bestand importeren](./media/import-file.png)

5. Kies een **scheidingsteken** en (optioneel) Voer een **voorvoegsel** moet worden gebruikt voor geïmporteerde sleutelnamen.

6. Kies desgewenst een **Label**.

7. Klik op **toepassen** om uit te voeren van het importeren.

    ![Bestand importeren voltooid](./media/import-file-complete.png)

## <a name="export-data"></a>Gegevens exporteren

Uitvoer schrijft configuratiegegevens zijn opgeslagen in de App-configuratie naar een andere bestemming. Kunt u de exportfunctie, bijvoorbeeld bij het opslaan van gegevens in een configuratie van apps naar een bestand dat tijdens de implementatie met code van uw toepassing worden ingesloten.

U kunt gegevens met behulp van exporteren de [Azure-portal](https://aka.ms/azconfig/portal) of [Azure CLI](./scripts/cli-export.md). Voer de volgende stappen uit vanuit de Azure-portal:

1. Blader naar de opslaan van de configuratie van uw app en klikt u op **Import/Export**.

2. In de **exporteren** tabblad **doel service** en **configuratiebestand**.

3. (Optioneel) Voer een **voorvoegsel** en kies een **Label** en een point-in-time voor sleutels moet worden geëxporteerd.

4. Kies een **bestandstype** en **scheidingsteken**.

5. Klik op **toepassen** naar de export voltooien.

    ![Volledige-bestand exporteren](./media/export-file-complete.png)

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: Een ASP.NET-web-app maken](quickstart-aspnet-core-app.md)  
