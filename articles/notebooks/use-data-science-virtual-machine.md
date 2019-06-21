---
title: Azure Data Science Virtual Machines gebruiken
description: Verbinding maken naar een Azure Data Science Virtual Machine (DSVM) de beschikbare compute-kracht uitbreiden naar Azure-notitieblokken.
services: app-service
documentationcenter: ''
author: getroyer
manager: andneil
ms.assetid: 0ccc2529-e17f-4221-b7c7-9496d6a731cc
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2019
ms.author: getroyer
ms.openlocfilehash: 9b762f1b3f1c17e15b051e72f5d2cf98bef446bf
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137757"
---
# <a name="use-azure-data-science-virtual-machines"></a>Azure Data Science Virtual Machines gebruiken

Standaard projecten uitvoeren op de **gratis Compute** laag, wat beperkt is tot 4 GB geheugen en 1 GB aan gegevens om misbruik te voorkomen. Met behulp van een andere virtuele machine die u in een Azure-abonnement hebt ingericht, kunt u deze beperkingen negeren. De beste keuze is voor dit doel een Azure Data Science Virtual Machine (DSVM) met behulp van de **Data Science Virtual Machine voor Linux (Ubuntu)** installatiekopie. Een dergelijke DSVM wordt geleverd vooraf geconfigureerd met alles wat u nodig hebt voor Azure-notitieblokken en wordt automatisch op de **uitvoeren** vervolgkeuzelijst in notitieblokken van Azure.

> [!Note]
> Azure-notitieblokken wordt alleen ondersteund op Dsvm die zijn gemaakt met de op Linux Ubuntu-installatiekopie. Notitieblokken worden niet ondersteund in Windows 2012, Windows 2016 of CentOS Linux-installatiekopieën.

## <a name="create-a-dsvm-instance"></a>Een DSVM-exemplaar maken

Voor het maken van een nieuw exemplaar van de DSVM, volgt u de instructies op [maken van een Ubuntu Data Science VM](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). Zie voor meer informatie, met inbegrip van details over de prijzen, [virtuele Machines voor Datatechnologie](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="connect-to-the-dsvm"></a>Verbinding maken met de DSVM

Als u de DSVM worden gemaakt, selecteert u de **uitvoeren** vervolgkeuzelijst op de Azure-Notebooks dashboard-project en selecteer het juiste exemplaar van de DSVM. De vervolgkeuzelijst bevat de DSVM-exemplaren als de volgende voorwaarden voldaan wordt:

- U bent aangemeld bij Azure-laptops met een account dat gebruikmaakt van Azure Active Directory (AAD), zoals een bedrijfsaccount.
- Uw account is verbonden met een Azure-abonnement.
- Hebt u een of meer virtuele machines in dat aan het abonnement, met ten minste leestoegang hebben, die gebruikmaakt van de virtuele Machine voor Datatechnologie voor Linux (Ubuntu)-installatiekopie.)

![Data Science Virtual Machine-exemplaren in de vervolgkeuzelijst in het projectdashboard](media/project-compute-tier-dsvm.png)

Wanneer u een exemplaar van de DSVM selecteert, Azure-notitieblokken wordt u mogelijk gevraagd om de specifieke machine-referenties gebruikt bij het maken van de virtuele machine.

Als een van de voorwaarden worden niet voldaan, kunt u nog steeds verbinding met de DSVM. Selecteer op de vervolgkeuzelijst de **Direct Compute** optie, die wordt u gevraagd een naam (om weer te geven in de lijst), IP-adres van de VM en poort (meestal 8000, de standaardpoort voor welke luistert JupyterHub) en de referenties van de virtuele machine:

![Prompt voor het verzamelen van informatie over server voor de directe Compute option](media/project-compute-tier-direct.png)

U kunt deze waarden verkrijgt in de DSVM-pagina in Azure portal.

## <a name="accessing-azure-notebooks-files-from-the-dsvm"></a>Azure-notitieblokken bestanden vanuit de DSVM te openen

Pariteit van paden met behouden de **gratis Compute** -laag, kunt u alleen een project openen op een tijdstip op een DSVM. Als u wilt een nieuw project openen, sluit u het project hebt geopend eerst.

![Knop Afsluiten in Azure-notitieblokken](media/shutdown.png)

Wanneer een project wordt uitgevoerd op een virtuele machine, de bestanden zijn gekoppeld aan de hoofdmap van de Jupyter-server (de map die wordt weergegeven in JupyterHub), de standaard Azure-notitieblokken bestanden worden vervangen. Wanneer u het afsluiten van de virtuele machine met behulp de **afsluiten** knop in de notebook-gebruikersinterface,-laptops Azure worden de standaard-bestanden hersteld.

![Knop Afsluiten in Azure-notitieblokken](media/shutdown.png)

## <a name="create-new-dsvm-users"></a>Maken van nieuwe DSVM-gebruikers

Als meerdere gebruikers een DSVM delen, kunt u voorkomen elkaar door het maken en gebruiken van een gebruiker DSVM voor elke gebruiker notebook blokkeren:

1. Op de [Azure Portal](https://portal.azure.com), gaat u naar uw virtuele machine.
1. Onder **ondersteuning en probleemoplossing** selecteren in de linkermarge **wachtwoord opnieuw instellen**.
1. Voer een nieuwe gebruikersnaam en wachtwoord in en selecteer **Update**. (Bestaande gebruikersnamen worden niet beïnvloed.)
1. Herhaal de vorige stap voor elke extra gebruikers.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de Dsvm op [Inleiding tot Azure Data Science Virtual Machines](/azure/machine-learning/data-science-virtual-machine/overview).
