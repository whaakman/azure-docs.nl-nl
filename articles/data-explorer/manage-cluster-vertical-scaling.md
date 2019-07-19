---
title: Verticaal schalen van een cluster beheren (omhoog schalen) in azure Data Explorer voor het wijzigen van de vraag
description: In dit artikel worden de stappen beschreven voor het omhoog en omlaag schalen van een Azure Data Explorer-cluster op basis van het wijzigen van de vraag.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: 80bbdf3a5d936719b06782cd78d56088b36cb21d
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67985486"
---
# <a name="manage-cluster-vertical-scaling-scale-up-in-azure-data-explorer-to-accommodate-changing-demand"></a>Verticaal schalen van een cluster beheren (omhoog schalen) in azure Data Explorer voor het wijzigen van de vraag

Het formaat van een cluster op de juiste wijze is essentieel voor de prestaties van Azure Data Explorer. De grootte van een statisch cluster kan leiden tot minder gebruik of over meer gebruiks mogelijkheden.

Omdat de vraag naar een cluster niet met absolute nauw keurigheid kan worden voor speld, is een betere benadering het *schalen* van een cluster, het toevoegen en verwijderen van capaciteits-en CPU-resources met veranderende vraag. 

Er zijn twee werk stromen voor het schalen van een Azure Data Explorer-cluster:

* [Horizon taal schalen](manage-cluster-horizontal-scaling.md), ook wel in-en uitschalen genoemd.
* Verticaal schalen, ook wel omhoog en omlaag schalen genoemd.

In dit artikel wordt de werk stroom voor verticale schaling uitgelegd:

## <a name="configure-vertical-scaling"></a>Verticaal schalen configureren

1. Ga in het Azure Portal naar de cluster bron van Azure Data Explorer. Selecteer bij **instellingen**de optie **Omhoog schalen**.

1. In het venster **Omhoog schalen** ziet u een lijst met beschik bare sku's voor uw cluster. In de volgende afbeelding zijn bijvoorbeeld slechts vier Sku's beschikbaar.

    ![Omhoog schalen](media/manage-cluster-vertical-scaling/scale-up.png)

    De Sku's zijn uitgeschakeld omdat ze de huidige SKU zijn, of ze zijn niet beschikbaar in de regio waar het cluster zich bevindt.

1. Als u uw SKU wilt wijzigen, selecteert u een nieuwe SKU en klikt u op **selecteren**.

> [!NOTE]
> * Het verticale schaal proces kan een paar minuten duren en tijdens die tijd wordt het cluster onderbroken. 
> * Omlaag schalen kan de prestaties van uw cluster nadelig beïnvloeden.
> * De prijs is een schatting van de virtuele machines van het cluster en de kosten van Azure Data Explorer services. Andere kosten zijn niet inbegrepen. Zie de pagina Azure Data Explorer [cost Estimator](https://dataexplorer.azure.com/AzureDataExplorerCostEstimator.html) voor een schatting en de [pagina met prijzen](https://azure.microsoft.com/pricing/details/data-explorer/) voor Azure Data Explorer voor volledige prijs informatie.

U hebt nu verticaal schalen geconfigureerd voor uw Azure Data Explorer-cluster. Voeg nog een regel toe voor Horizon taal schalen. Als u hulp nodig hebt bij problemen met cluster schaal, [opent u een ondersteunings aanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) in de Azure Portal.

## <a name="next-steps"></a>Volgende stappen

* [Horizon taal schalen op het cluster beheren](manage-cluster-horizontal-scaling.md) om het aantal instanties dynamisch uit te schalen op basis van metrische gegevens die u opgeeft.

* Controleer het gebruik van resources door dit artikel te volgen: [Bewaak de prestaties, de status en het gebruik van Azure Data Explorer met metrische gegevens](using-metrics.md).

