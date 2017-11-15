---
title: Parameters van de server in Azure-Database configureren voor PostgreSQL via Azure portal | Microsoft Docs
description: In dit artikel wordt beschreven hoe de parameters van de server in Azure-Database configureren voor PostgreSQL via de Azure portal.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/08/2017
ms.openlocfilehash: 9e8262fbfcde2e69a656e356a7ab241f2d5043ad
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="configure-server-parameters-in-azure-portal"></a>Parameters van de server configureren in Azure-portal
U kunt lijst, weergeven en bijwerken van configuratieparameters voor een Azure-Database voor PostgreSQL-server via de Azure-portal.

## <a name="prerequisites"></a>Vereisten
Om deze procedures handleiding die u moet doorlopen:
- [Azure-Database voor PostgreSQL-server](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Weergeven en bewerken van parameters
1. Open de [Azure Portal](https://portal.azure.com).

2. Selecteer uw Azure-Database voor PostgreSQL-server.

3. Onder de **instellingen** sectie **serverparameters**. De pagina bevat een lijst met parameters, hun waarden en beschrijvingen.
![Overzichtspagina voor Parameters](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. Selecteer de **vervolgkeuzelijst** om weer te geven van de mogelijke waarden voor parameters zoals client_min_messages opgesomd-type.
![Verwijder omlaag opsommen](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png)

5. Selecteer of Beweeg de muisaanwijzer over de **ik** knop om te zien van het bereik van de mogelijke waarden voor numerieke parameters zoals cpu_index_tuple_cost (gegevens).
![informatieknop](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. Indien nodig, gebruiken de **zoekvak** beperken voor een specifieke parameter. De zoekopdracht is op de naam en beschrijving van de parameters.
![Zoekresultaten](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. Wijzig de parameterwaarden die u wilt aanpassen. Alle wijzigingen die u in een sessie aanbrengt worden in paars gemarkeerd. Wanneer u de waarden hebt gewijzigd, kunt u selecteren **opslaan**. U kunt **negeren** uw wijzigingen.
![Opslaan of wijzigingen negeren](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. Als u nieuwe waarden voor de parameters hebt opgeslagen, u kunt altijd terugkeren alles terug naar de standaardwaarden door te selecteren **opnieuw instellen van alle standaardwaarden**.
![Alles standaardwaarden herstellen](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:
- [Overzicht van de parameters van de server in Azure-Database voor PostgreSQL](concepts-servers.md)
- [Met de Azure CLI parameters configureren](howto-configure-server-parameters-using-cli.md)
