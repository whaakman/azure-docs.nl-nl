---
title: Containers | Microsoft Docs
description: Stappen voor het publiceren van een containerinstallatiekopie.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5653e4b2ec9aa5e21a107611b9d4142168630d4a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809953"
---
<a name="publishing-a-container-image-in-the-cloud-partner-portal"></a>Een Containerinstallatiekopie publiceren in de Cloud Partner-Portal
========================================================

In dit artikel wordt beschreven hoe u een nieuwe containerinstallatiekopie publiceren in de Cloud Partner-Portal.

Gebruik de volgende stappen voor het publiceren van een nieuwe containerinstallatiekopie.

1. Selecteer **nieuwe aanbieding** en selecteer vervolgens **Containers**

    ![Selecteer de optie containers voor nieuwe aanbieding](media/cpp-containers-guide/azure-container-offer.png)

2. Voer in het tabblad instellingen bieden onder identiteit bieden de **aanbiedings-ID**, **uitgevers-ID**, en **naam**.

    ![Identiteit](media/cpp-containers-guide/containers-offer-settings.png)

3. Selecteer in het tabblad SKU's **nieuwe Voorraadeenheid**.
    >[!NOTE]
    >U kunt meer dan één Voorraadeenheid toevoegen.

    ![Nieuwe SKU-prompt](media/cpp-containers-guide/containers-sku-settings.png)

4. Bevatten informatie van SKU's en -container. Elke SKU correspondeert met een containerinstallatiekopie. Er zijn twee onderdelen naar een SKU:

    -   SKU-metagegevens
    -   Metagegevenscontainer

    De SKU-metagegevens bevat de weergavegegevens van de voor de containerinstallatiekopieën.

    ![SKU-metagegevens](media/cpp-containers-guide/containers-sku-details.png)

    De containermetagegevens van de heeft de referentie-informatie van de gegevens van uw installatiekopie-opslagplaats in Azure container registry (ACR). Azure Marketplace wordt deze installatiekopie gekopieerd naar het register van de openbare marketplace, en is beschikbaar voor klanten na certificering. Alle aanvragen van de gebruiker van Azure gebruiken voor de installatiekopie van een container worden geleverd vanuit de Marketplace container registry.

   ![Metagegevenscontainer](media/cpp-containers-guide/containers-image-repository.png)

    De Details van de afbeelding-opslagplaats in de vorige schermafbeelding bevat de volgende velden:

    -   **Abonnements-ID** -de Azure abonnements-id waarin ACR-register zich bevindt.
    -   **De naam van resourcegroep** -naam van de resourcegroep van het ACR-register.
    -   **Naam van het containerregister** -naam van de ACR-register.
    -   **Naam van de opslagplaats** -naam van de opslagplaats. Nadat deze is ingesteld, kan niet deze waarde later worden gewijzigd. Dit moet worden gegeven een unieke naam zodat er geen ander aanbod voor uw account dezelfde naam heeft.
    -   **Gebruikersnaam** -de gebruikersnaam die is gekoppeld aan een ACR (admin gebruikersnaam).
    -   **Wachtwoord** -het wachtwoord dat is gekoppeld aan ACR.

    >[!NOTE]
    >De gebruikersnaam en wachtwoord zijn vereist om ervoor te zorgen dat partners toegang tot de ACR vermeld in het publicatieproces hebben.

    Tijdens het publiceren van een containerinstallatiekopie, kunt u ook een of meer installatiekopielabels opgeven. Partners kunnen ook SHA verwerkingen bieden naast van een afbeeldingscode. Zorg ervoor dat u toevoegt een **tag testen** aan de installatiekopie zodat u de installatiekopie tijdens het testen identificeren kan.

5. Voeg uw marketing-specifieke inhoud op het tabblad Marketplace.

    ![De Marketplace-gegevens](media/cpp-containers-guide/containers-marketplace-tab.png)

6. Op het tabblad ondersteuning, voert u uw contact op met de technische en informatie over klantondersteuning.

   ![Ondersteuningsinformatie](media/cpp-containers-guide/containers-support-tab.png)

7. Selecteer **publiceren** voor het publiceren van de aanbieding. Nadat u publiceren selecteert, ziet u een tijdlijn waarin de stappen die betrokken zijn voor het publiceren van uw containerinstallatiekopie.
