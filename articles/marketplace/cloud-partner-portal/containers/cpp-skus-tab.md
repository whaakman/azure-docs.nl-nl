---
title: SKU's voor een Azure Containers-installatiekopie | Microsoft Docs
description: SKU's voor een Azure-container configureren.
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
ms.date: 11/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 81f7e46e626bb061881be53e8cace36e1478e0e1
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683975"
---
# <a name="container-skus-tab"></a>Container-SKU's tabblad

De **SKU's** tabblad van de **nieuwe aanbieding** pagina kunt u een of meer SKU's maken en deze koppelt aan uw nieuwe aanbieding.  U kunt verschillende SKU's gebruiken om te onderscheiden van een oplossing door functiesets, factureringsmodellen of andere kenmerken.

## <a name="sku-settings"></a>SKU-instellingen

Wanneer u het maken van een nieuwe aanbieding start, er zijn niet alle SKU's die zijn gekoppeld aan de aanbieding. Volg deze stappen voor het maken van een nieuwe SKU:

1. Selecteer in het tabblad SKU's **nieuwe Voorraadeenheid**

   ![Nieuwe SKU-prompt](./media/containers-sku-settings.png)

2. Geef de vereiste SKU en container-gegevens. Elke SKU correspondeert met een containerinstallatiekopie. Er zijn twee onderdelen naar een SKU:

    -   SKU-metagegevens
    -   Metagegevenscontainer

### <a name="sku-metadata"></a>SKU-metagegevens

De SKU-metagegevens bevat storefront weergave-informatie voor de container-aanbieding.

![SKU-metagegevens](./media/containers-sku-details.png)

### <a name="container-metadata"></a>Metagegevenscontainer

De containermetagegevens van de heeft de referentie-informatie van de gegevens van uw installatiekopie-opslagplaats in Azure Container Registry (ACR). Azure Marketplace wordt deze installatiekopie gekopieerd naar een Marketplace-specifieke, openbaar register en vervolgens de installatiekopie van het beschikbaar voor klanten na certificering. Alle aanvragen van de gebruiker van Azure gebruiken voor een Azure Marketplace-containerinstallatiekopie wordt uit openbare register van de Marketplace, geen ACR voldaan.

![Metagegevenscontainer](./media/containers-image-repository.png)
    
De **installatiekopie opslagplaats Details** in het vorige scherm vastleggen van de volgende velden bevat:

-   **Abonnements-ID** -de Azure abonnements-ID waarin de ACR zich bevindt.
-   **De naam van resourcegroep** -naam van de resourcegroep van de ACR.
-   **Naam van het containerregister** -de ACR-naam.
-   **Naam van de opslagplaats** -naam van de opslagplaats. Nadat deze naam is ingesteld, kan deze waarde kan niet worden gewijzigd. Gebruik een unieke naam om te voorkomen dat een conflict met andere aanbiedingen in uw account.
-   **Gebruikersnaam** -de gebruikersnaam (admin-username) de ACR-installatiekopie.
-   **Wachtwoord** -het wachtwoord dat is gekoppeld aan de ACR-installatiekopie.

    >[!NOTE]
    >De gebruikersnaam en wachtwoord zijn vereist om ervoor te zorgen dat partners toegang tot de ACR vermeld in het publicatieproces hebben.

### <a name="image-version"></a>Installatiekopieversie

Bij het publiceren van een containerinstallatiekopie, kunt u een of meer installatiekopielabels opgeven en SHA verwerkingen.

**Tag Image- of Samenvattingsupdates**
 
- Deze tag- of samenvattingsupdates moet bevatten een `latest` tag en een versietag (bijvoorbeeld beginnen met `xx.xx.xx-` waarbij xx is een getal). Ze moeten [manifest tags](https://github.com/estesp/manifest-tool) om u te richten op meerdere platforms. Alle tags waarnaar wordt verwezen door een manifest tag moeten ook worden toegevoegd, zodat we kunnen het uploaden. 
- U kunt verschillende versies van de container met behulp van labels toevoegen. Alle tags manifest (met uitzondering van `latest`) moet beginnen met een `X.Y-` of `X.Y.Z-` waarbij X, Y, Z gehele getallen zijn. <br/> Bijvoorbeeld, als een `latest` label verwijst naar `1.0.1-linux-x64`, `1.0.1-linux-arm32`, en `1.0.1-windows-arm32`, deze tags moeten hier niet worden toegevoegd.

>[!NOTE]
>Houd er rekening mee om toe te voegen een **tag testen** aan de installatiekopie, zodat u de installatiekopie tijdens het testen identificeren kunt.

## <a name="next-steps"></a>Volgende stappen

Gebruik de [Marketplace tabblad](./cpp-marketplace-tab.md) te maken van een marketplace-beschrijving van uw aanbieding. 