---
title: 'Azure-snelstart: Een event hub maken met behulp van Azure Portal | Microsoft Docs'
description: In deze snelstart leert u hoe u een Azure event hub maakt met behulp van Azure Portal en vervolgens gebeurtenissen verzendt en ontvangt met behulp van .NET Standard SDK.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/23/2019
ms.author: shvija
ms.openlocfilehash: 15b2b5863f5b78bd24ffd8cbd55b9914f52e32a1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072800"
---
# <a name="quickstart-create-an-event-hub-using-azure-portal"></a>Snelstartgids: Een event hub maken met behulp van Azure Portal
Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

In deze snelstart maakt u een Event Hub met behulp van de [Azure-portal](https://portal.azure.com).

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

- Azure-abonnement. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.
- [Visual Studio 2019)](https://www.visualstudio.com/vs) of hoger.
- [.NET Standard SDK](https://www.microsoft.com/net/download/windows), versie 2.0 of hoger.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische verzameling Azure-resources. Alle resources worden geïmplementeerd en beheerd in een resourcegroep. Een resourcegroep maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Kik in het linkernavigatievenster op **Resourcegroepen**. Klik vervolgens op **Toevoegen**.

   ![Resourcegroepen - Knop Toevoegen](./media/event-hubs-quickstart-portal/resource-groups1.png)

2. Klik bij **Abonnement** op de naam van het Azure-abonnement waarin u de resourcegroep wilt maken.
3. Typ een unieke **naam voor de resourcegroep**. Het systeem controleert onmiddellijk of de naam beschikbaar is in het momenteel geselecteerde Azure-abonnement.
4. Selecteer een **regio** voor de resourcegroep.
5. Selecteer **Controleren + maken**.

   ![Resourcegroep - Maken](./media/event-hubs-quickstart-portal/resource-groups2.png)
6. Selecteer op de pagina **Controleren + maken** de optie **Maken**. 

## <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken

Een Event Hubs-naamruimte biedt een unieke scopingcontainer, waarnaar wordt verwezen met de volledig gekwalificeerde domeinnaam (FQDN), waarin u een of meer Event Hubs maakt. Ga als volgt te werk om een ​​naamruimte in uw resourcegroep te maken met behulp van de portal:

1. Open de Azure-portal en klik op **Een resource maken** linksboven in het scherm.
2. Selecteer **Alle services** in het menu aan de linkerkant en selecteer de **ster (`*`)** naast **Event Hubs** in de categorie **Analyse**. Controleer of **Event Hubs** is toegevoegd aan **FAVORIETEN** in het navigatiemenu aan de linkerkant. 
    
   ![Zoeken naar Event Hubs](./media/event-hubs-quickstart-portal/select-event-hubs-menu.png)
3. Selecteer **Event Hubs** onder **FAVORIETEN** in het navigatiemenu links en selecteer **Toevoegen** op de werkbalk.

   ![Knop Toevoegen](./media/event-hubs-quickstart-portal/event-hubs-add-toolbar.png)
4. Voer op de pagina **Naamruimte maken** de volgende stappen uit:
    1. Voer een naam in voor de naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.
    2. Kies de prijscategorie (Basic of Standard).
    3. Selecteer het **abonnement** waarin u de naamruimte wilt maken.
    4. Selecteer een **locatie** voor de naamruimte.
    5. Selecteer **Maken**. U moet een paar minuten wachten voordat het systeem de resources volledig heeft ingericht.

       ![Een Event Hub-naamruimte maken](./media/event-hubs-quickstart-portal/create-event-hub1.png)
5. Vernieuw de **Event Hubs**-pagina. De Event Hub-naamruimte wordt dan weergegeven. U kunt de status van het Event Hub-creatieproces bekijken in de waarschuwingen. 

    ![Een Event Hub-naamruimte maken](./media/event-hubs-quickstart-portal/event-hubs-refresh.png)
6. Selecteer de naamruimte. U ziet in de portal de startpagina van uw **Event Hubs-naamruimte**. 

   ![Startpagina voor de naamruimte](./media/event-hubs-quickstart-portal/namespace-home-page.png)
    
## <a name="create-an-event-hub"></a>Een Event Hub maken

Ga als volgt te werk om een Event Hub in de naamruimte te maken:

1. Op de pagina Event Hubs-naamruimte selecteert u **Event Hubs** in het menu links.
1. Klik op **+ Event Hub** bovenaan in het venster.
   
    ![Event Hub toevoegen - Knop](./media/event-hubs-quickstart-portal/create-event-hub4.png)
1. Typ een naam voor uw Event Hub en klik vervolgens op **Maken**.
   
    ![Event hub maken](./media/event-hubs-quickstart-portal/create-event-hub5.png)
4. U kunt de status van het Event Hub-creatieproces bekijken in de waarschuwingen. Wanneer de Event Hub is gemaakt, wordt deze weergegeven in de lijst Event Hubs, zoals in de volgende afbeelding is te zien:

    ![Event Hub gemaakt](./media/event-hubs-quickstart-portal/event-hub-created.png)

Gefeliciteerd! U hebt de portal gebruikt om een ​​Event Hubs-naamruimte en een Event Hub binnen die naamruimte te maken. 

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een resourcegroep, een Event Hubs-naamruimte en een Event Hub gemaakt. Zie voor stapsgewijze instructies voor het verzenden van gebeurtenissen naar (of) gebeurtenissen ontvangen van een event hub, de **verzenden en ontvangen van gebeurtenissen** zelfstudies: 

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.js](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (alleen verzenden)](event-hubs-c-getstarted-send.md)
- [Apache Storm (alleen reecive)](event-hubs-storm-getstarted-receive.md)


[Azure portal]: https://portal.azure.com/
[3]: ./media/event-hubs-quickstart-portal/sender1.png
[4]: ./media/event-hubs-quickstart-portal/receiver1.png
