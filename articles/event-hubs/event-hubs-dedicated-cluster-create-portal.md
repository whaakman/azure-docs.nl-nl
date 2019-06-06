---
title: 'Azure-Snelstart: een speciale Event Hubs-cluster met behulp van de Azure portal maken | Microsoft Docs'
description: In deze snelstartgids leert u hoe u een Azure Event Hubs-cluster met behulp van Azure portal maakt.
services: event-hubs
documentationcenter: ''
author: xurui203
manager: ''
ms.service: event-hubs
ms.topic: quickstart
ms.custom: mvc
ms.date: 05/02/2019
ms.author: xurui
ms.openlocfilehash: 269ecca8683229a56d40dfacc354abbd7ce10762
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66688581"
---
# <a name="quickstart-create-a-dedicated-event-hubs-cluster-using-azure-portal"></a>Quickstart: Maken van een specifieke Event Hubs-cluster met behulp van Azure portal 
Event Hubs-clusters bieden één tenant-implementaties voor klanten met de meest veeleisende behoeften voor streaming. Deze aanbieding een gegarandeerde SLA van 99,99% is en is alleen beschikbaar op onze Dedicated prijscategorie. Een [Event Hubs-cluster](event-hubs-dedicated-overview.md) invoer miljoenen gebeurtenissen per seconde met een gegarandeerde latentie van capaciteit en subsecond kunt. Naamruimten en event hubs gemaakt binnen een cluster zijn alle functies van het standard-aanbod en nog veel meer, maar zonder eventuele beperkingen voor inkomend verkeer. De speciaal aanbod omvat ook de populaire [Event Hubs Capture](event-hubs-capture-overview.md) functie zonder extra kosten, zodat u kunt automatisch batch- en logboekbestanden gegevensstromen tot [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md) of [ Azure Data Lake-opslag Gen 1](../data-lake-store/data-lake-store-overview.md).

Toegewezen clusters zijn ingericht en gefactureerd op basis van **capaciteitseenheden (Cu's)** , een vooraf toegewezen hoeveelheid CPU en geheugenbronnen. U kunt 1, 2, 4, 8, 12, 16 of 20 Cu's voor elk cluster kopen. In deze snelstartgids begeleidt we u stapsgewijs door het maken van een 1 Capaciteitseenheid Event Hubs-cluster via Azure portal.

> [!NOTE]
> Deze ervaring Self-service voor stroomactiviteitvoortgang is momenteel beschikbaar in Preview-versie van [Azure Portal](https://aka.ms/eventhubsclusterquickstart). Als u vragen hebt over de speciale aanbieding hebt, neem contact op met de [team van Event Hubs](mailto:askeventhubs@microsoft.com).


## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

- Een Azure-account. Als u nog geen hebt, [koop een account](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) voordat u begint. Deze functie wordt niet ondersteund met een gratis Azure-account. 
- [Visual Studio](https://visualstudio.microsoft.com/vs/) 2017 Update 3 (versie 15.3, 26730.01) of hoger.
- [.NET Standard SDK](https://dotnet.microsoft.com/download), versie 2.0 of hoger.
- [Een resourcegroep gemaakt](../event-hubs/event-hubs-create.md#create-a-resource-group).

## <a name="create-an-event-hubs-dedicated-cluster"></a>Een specifieke Event Hubs-Cluster maken
Een Event Hubs-cluster biedt een unieke scoping container waarin u een of meer naamruimten kunt maken. In deze Preview-fase van de portal van de selfservice-ervaring, kunt u 1 CU-clusters in bepaalde regio's. Als u een cluster groter zijn dan 1 moet CU, kunt u indienen een Azure-ondersteuning voor het schalen van uw cluster nadat het is gemaakt.

Voor het maken van een cluster in de resourcegroep met behulp van de Azure portal, voert u de volgende stappen uit:

1. Ga als volgt [deze koppeling](https://aka.ms/eventhubsclusterquickstart) voor het maken van een cluster in Azure portal. Daarentegen, selecteer **alle services** in het navigatiedeelvenster links en typ in 'Event Hubs-Clusters' in de zoekbalk en selecteer 'Event Hubs-Clusters' in de lijst met resultaten.
2. Op de **Cluster maken** pagina, configureert u het volgende:
    1. Voer een **naam voor het cluster**. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.
    2. Selecteer de **abonnement** waarin u wilt maken van het cluster.
    3. Selecteer de **resourcegroep** waarin u wilt maken van het cluster.
    4. Selecteer een **locatie** voor het cluster. Als uw voorkeursregio lichter gekleurd wordt weergegeven, tijdelijk beperkt is en kunt u indienen een [ondersteuningsaanvraag](#submit-a-support-request) aan het team Event Hubs.
    5. Selecteer de **volgende: Tags** knop aan de onderkant van de pagina. U moet een paar minuten wachten voordat het systeem de resources volledig heeft ingericht.

        ![Event Hubs-Cluster maken - basisbeginselen pagina](./media/event-hubs-dedicated-cluster-create-portal/create-event-hubs-clusters-basics-page.png)
3. Op de **Tags** pagina, configureert u het volgende:
    1. Voer een **naam** en een **waarde** voor het label dat u wilt toevoegen. Deze stap is **optioneel**.  
    2. Selecteer de **revisie + maken** knop.

        ![Event Hubs-Cluster pagina - Tags maken](./media/event-hubs-dedicated-cluster-create-portal/create-event-hubs-clusters-tags-page.png)
4. Op de **beoordelen en maken** pagina, lees de informatie en selecteer **maken**. 

    ![Event Hubs-Cluster-pagina - beoordeling maken + pagina maken](./media/event-hubs-dedicated-cluster-create-portal/create-event-hubs-clusters-review-create-page.png)

## <a name="create-a-namespace-and-event-hub-within-a-cluster"></a>Een naamruimte en event hub binnen een cluster maken

1. Een naamruimte binnen een cluster maken op de **Event Hubs-Cluster** pagina voor uw cluster, selecteer **+ Namespace** in het menu bovenaan.

    ![Beheerpagina van de cluster - naamruimte knop toevoegen](./media/event-hubs-dedicated-cluster-create-portal/cluster-management-page-add-namespace-button.png)
2. Voer op het maken van een naamruimte-pagina, de volgende stappen uit:
    1. Voer een **naam in voor de naamruimte**.  Het systeem wordt gecontroleerd of de naam beschikbaar is.
    2. De naamruimte neemt de volgende eigenschappen:
        1. Abonnements-id
        2. Resourcegroep
        3. Locatie
        4. Clusternaam
    3. Selecteer **maken** om de naamruimte te maken. U kunt nu uw cluster beheren.  

        ![Naamruimte maken in de cluster-pagina](./media/event-hubs-dedicated-cluster-create-portal/create-namespace-cluster-page.png)
3. Zodra uw naamruimte is gemaakt, kunt u [maken van een event hub](event-hubs-create.md#create-an-event-hub) zoals u dit normaal gesproken binnen een naamruimte maken zou. 


## <a name="submit-a-support-request"></a>Een ondersteuningsaanvraag indienen

Als u de grootte van uw cluster wijzigen na het maken of wilt als de gewenste regio niet beschikbaar is, kunt u een ondersteuningsaanvraag indienen via de volgende stappen:

1. In [Azure-portal](https://portal.azure.com), selecteer **Help en ondersteuning** in het menu links.
2. Selecteer **+ nieuw ondersteuningsverzoek** in het menu ondersteuning.
3. Voer de volgende stappen uit op de ondersteuningspagina:
    1. Voor **probleemtype**, selecteer **technische** uit de vervolgkeuzelijst.
    2. Bij **Abonnement** selecteert u uw abonnement.
    3. Voor **Service**, selecteer **mijn services**, en selecteer vervolgens **Event Hubs**.
    4. Voor **Resource**, selecteert u het cluster als deze al bestaat, Selecteer anders **algemene vraag/Resource niet beschikbaar**.
    5. Voor **probleemtype**, selecteer **quotum**.
    6. Voor **probleem subtype**, selecteer een van de volgende uit de vervolgkeuzelijst waarden:
        1. Selecteer **aanvraag voor de SKU toegewezen** om aan te vragen voor de functie in uw regio worden ondersteund.
        2. Selecteer **aanvragen op schaal omhoog of omlaag toegewezen Cluster schalen** als u wilt vergroten of verkleinen van uw specifieke cluster. 
    7. Voor **onderwerp**, beschrijving van het probleem.

        ![Ondersteuningspagina ticket](./media/event-hubs-dedicated-cluster-create-portal/support-ticket.png)

 ## <a name="delete-a-dedicated-cluster"></a>Een specifieke cluster verwijderen
 
1. Selecteer wilt verwijderen van het cluster **verwijderen** in het menu bovenaan. Houd er rekening mee dat het cluster in rekening voor een minimum van 4 uur van het gebruik van nadat is gemaakt gebracht wordt. 
2. Er wordt een bericht weergegeven waaruit blijkt dat uw wilt verwijderen van het cluster.
3. Type de **naam van het cluster** en selecteer **verwijderen** om het cluster te verwijderen.

    ![Cluster-pagina verwijderen](./media/event-hubs-dedicated-cluster-create-portal/delete-cluster-page.png)


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u een Event Hubs-cluster gemaakt. Zie de volgende zelfstudies voor stapsgewijze instructies voor het verzenden en ontvangen van gebeurtenissen van een event hub en gebeurtenissen vastleggen naar een Azure storage of Azure Data Lake Store:

- [Verzenden en ontvangen van gebeurtenissen op .NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [Azure portal gebruiken voor Event Hubs Capture inschakelen](event-hubs-capture-enable-through-portal.md)
- [Gebruik Azure Eventhubs voor Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md)
