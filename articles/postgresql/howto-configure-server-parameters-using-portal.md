---
title: Serverparameters configureren in Azure Database for PostgreSQL via Azure portal
description: In dit artikel wordt beschreven hoe u de parameters van de server configureren in Azure Database for PostgreSQL via Azure portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 0d0626c48ecebdead604aab93ab0602c698d0d77
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540534"
---
# <a name="configure-server-parameters-in-azure-portal"></a>De serverparameters configureren in Azure portal
U kunt weergeven, weergeven en bijwerken van parameters voor de configuratie voor een Azure Database for PostgreSQL-server via de Azure-portal.

## <a name="prerequisites"></a>Vereisten
In deze gebruiksaanwijzing die u moet doorlopen:
- [Azure Database for PostgreSQL-server](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Weergeven en bewerken van parameters
1. Open de [Azure Portal](https://portal.azure.com).

2. Selecteer uw Azure Database for PostgreSQL-server.

3. Onder de **instellingen** sectie, selecteer **serverparameters**. De pagina bevat een lijst met parameters, hun waarden en beschrijvingen.
![Overzichtspagina voor Parameters](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. Selecteer de **vervolgkeuzelijst** om weer te geven van de mogelijke waarden voor parameters zoals client_min_messages geïnventariseerd-type.
![Vervolgkeuzelijst omlaag opsommen](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png)

5. Selecteer of Beweeg de muisaanwijzer over de **ik** knop om te zien van het bereik van de mogelijke waarden voor numerieke parameters, zoals cpu_index_tuple_cost (gegevens).
![knop gegevens](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. Als het nodig is, gebruikt u de **zoekvak** om te beperken voor een specifieke parameter. De zoekopdracht is op de naam en beschrijving van de parameters.
![Zoekresultaten](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. Wijzig de parameterwaarden die u wilt aanpassen. Alle wijzigingen die u in een sessie aanbrengt worden in paars gemarkeerd. Zodra u de waarden hebt gewijzigd, kunt u selecteren **opslaan**. U kunt ook **negeren** uw wijzigingen.
![Opslaan of wijzigingen negeren](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. Als u nieuwe waarden voor de parameters hebt opgeslagen, kunt u altijd terugkeren alles weer op de standaardwaarden hiervoor **alle standaardinstellingen opnieuw instellen**.
![Alle standaardinstellingen opnieuw instellen](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:
- [Overzicht van de parameters van de server in Azure Database for PostgreSQL](concepts-servers.md)
- [Parameters met de Azure CLI configureren](howto-configure-server-parameters-using-cli.md)
