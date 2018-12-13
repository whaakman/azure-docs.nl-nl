---
title: Kies een Azure-oplossing voor de overdracht van | Microsoft Docs
description: Meer informatie over het kiezen van een Azure-oplossing voor de overdracht van op basis van gegevensgroottes en de beschikbare netwerkbandbreedte in uw omgeving
services: storage
author: alkohli
ms.service: storage
ms.subservice: blob
ms.topic: article
ms.date: 12/10/2018
ms.author: alkohli
ms.openlocfilehash: b90e84082eb73dde5a2303f166e9aa254c6f3101
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53263816"
---
# <a name="choose-an-azure-solution-for-data-transfer"></a>Kies een Azure-oplossing voor gegevensoverdracht

Dit artikel bevat een overzicht van enkele van de algemene Azure-gegevensoverdracht-oplossingen. Het artikel bevat ook koppelingen om naar de aanbevolen opties, afhankelijk van de bandbreedte van het netwerk in uw omgeving en de grootte van de gegevens die u van plan bent om over te dragen.

## <a name="types-of-data-movement"></a>Typen verplaatsing van gegevens

Overdracht van gegevens is offline of via de netwerkverbinding. Kies uw oplossing, afhankelijk van uw:

- **Gegevensgrootte** -grootte van de gegevens die zijn bedoeld voor overdracht
- **Frequentie Transfer** -eenmalig of periodieke gegevensopname en
- **Netwerk** – bandbreedte die beschikbaar is voor gegevens overbrengen in uw omgeving.

De verplaatsing van gegevens kan zijn van de volgende typen:

- **Offline overdracht met behulp van shippable apparaten** -fysieke shippable apparaten gebruiken als u wilt doen offline eenmalige grote hoeveelheden gegevens worden overgebracht. Microsoft stuurt u een schijf of op een veilige gespecialiseerde apparaat. U kunt ook aanschaffen en uw eigen schijven verzenden. U gegevens kopiëren naar het apparaat en klikt u vervolgens verzendt deze naar Azure waar de gegevens is geüpload.  De beschikbare opties in dit geval zijn Data Box-schijf, Data Box, gegevens in het zware en Import/Export (Gebruik uw eigen schijven).

- **Overdracht van het netwerk** -u uw gegevens overbrengen naar Azure via uw netwerkverbinding. Dit kan op veel verschillende manieren worden gedaan.

    - **Grafische interface** -als u af en toe enkele bestanden overbrengen en niet hoeft voor het automatiseren van de overdracht van gegevens, kunt u een grafische interface hulpprogramma zoals Azure Storage Explorer of een hulpprogramma voor het web gebaseerde verkennen in Azure portal.
    - **Scripts of programmatische overdracht** -kunt u geoptimaliseerde softwaretools dat we bieden of het rechtstreeks aanroepen van onze REST API's / SDK's. De beschikbare scriptbare hulpprogramma's zijn AzCopy, Azure PowerShell en Azure CLI. Gebruik een van de SDK's voor de programma-interface voor .NET, Java, Python, Node/JS, C++, Go, PHP of Ruby.
    - **On-premises apparaten** -geven We u een fysiek of virtueel apparaat dat deel uitmaakt van uw datacenter en optimaliseert de gegevens worden overgebracht via het netwerk. Deze apparaten bieden ook een lokale cache van veelgebruikte bestanden. Het fysieke apparaat is de Data Box-rand en het virtuele apparaat is de Data Box-Gateway. Beide permanent worden uitgevoerd in uw lokale en verbinding maken met Azure via het netwerk.
    - **Beheerde gegevenspijplijn** -u kunt een pijplijn cloud zo instellen dat er regelmatig bestanden overbrengt tussen verschillende Azure-services, on-premises of een combinatie van twee. Gebruik Azure Data Factory instellen en beheren van gegevenspijplijnen, en verplaatsen en transformeren van gegevens voor analyse.

De volgende visualisatie ziet u de richtlijnen voor het kiezen van de verschillende Azure-gegevensoverdracht hulpprogramma's, afhankelijk van de netwerkbandbreedte die beschikbaar zijn voor de overdracht, gegevensgrootte is bedoeld voor overdracht en frequentie van de overdracht.

![Hulpprogramma's voor Azure gegevensoverdracht](media/storage-choose-data-transfer-solution/azure-data-transfer-options-3.png)

**De bovenste limieten van de overdracht van offline-apparaten - Data Box-schijf, Data Box en gegevens in het zware kunnen worden uitgebreid door meerdere bestellingen van een apparaattype.*

## <a name="selecting-a-data-transfer-solution"></a>Selecteren van een oplossing voor overdracht

Beantwoord de volgende vragen om hulp bij selecteren van een oplossing voor overdracht:

- De beschikbare netwerkbandbreedte beperkt of niet-bestaande is en u wilt overbrengen van grote gegevenssets?
  
    Zo ja, Zie: [Scenario 1: Overdracht van grote gegevenssets zonder of met lage netwerkbandbreedte](storage-solution-large-dataset-low-network.md).
- U wilt overbrengen van grote gegevenssets via netwerk en u hebt een gemiddeld naar hoogste netwerkbandbreedte?

    Zo ja, Zie: [Scenario 2: Het overbrengen van grote gegevenssets met gemiddeld naar hoge netwerkbandbreedte](storage-solution-large-dataset-moderate-high-network.md).
- Wilt u zo nu en dan een paar bestanden overdragen via het netwerk?

    Zo ja, Zie [Scenario 3: Kleine gegevenssets met beperkte dragen naar gemiddelde netwerkbandbreedte](storage-solution-small-dataset-low-moderate-network.md).
- Zoekt u point-in-time-gegevensoverdracht met regelmatige tussenpozen?

    Zo ja, gebruik van het script/programmatische opties die worden beschreven in [Scenario 4: Periodieke gegevensoverdracht](storage-solution-periodic-data-transfer.md).
- Zoekt u continue, continue gegevensoverdracht?

    Zo ja, gebruik de opties in [Scenario 4: Periodieke gegevensoverdracht](storage-solution-periodic-data-transfer.md).

## <a name="next-steps"></a>Volgende stappen

- [Een inleiding tot Azure Storage Explorer](https://azure.microsoft.com/resources/videos/introduction-to-microsoft-azure-storage-explorer/).
- [Lees een overzicht van AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10).
- [Azure PowerShell gebruiken met Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-powershell-guide-full)
- [Azure CLI gebruiken met Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-azure-cli)
- Meer informatie over:

    - [Azure Data Box, Azure Data Box-schijf en Azure Data Box zware voor offline-overdrachten](https://docs.microsoft.com/azure/databox/).
    - [Azure Data Box-Gateway en Azure Data Box Edge voor online-overdrachten](https://docs.microsoft.com/azure/databox-online/).
- [Ontdek wat is Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview).
- De REST API's gebruikt voor het overbrengen van gegevens

    - [In .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
    - [In Java](https://docs.microsoft.com/java/api/overview/azure/storage/client)
