---
title: bestand opnemen
description: bestand opnemen
services: storage
author: alkohli
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: 4285ee94d2f2a9b5fff9d1896cdf7013f88801ab
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45979155"
---
#### <a name="to-create-a-new-service"></a>Een nieuwe service maken

1.  Met behulp van de referenties van uw Microsoft-account, zich aanmelden bij Azure portal op de volgende URL: <https://portal.azure.com/>. Als de implementatie van het apparaat in Government-portal, moet u zich aanmelden bij: <https://portal.azure.us/>

2.  Klik in de Azure-portal op **+ een resource maken** &gt; **opslag** &gt; **virtuele StorSimple-serie**.

    ![Nieuwe service maken](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  In de **StorSimple Device Manager** blade die wordt geopend, het volgende doen:

    1.  Geef een unieke **resourcenaam** voor uw service op. Naam van de resource is een beschrijvende naam die kan worden gebruikt om de service te identificeren. De naam kan tussen 2 en 50 tekens bevatten (letters, cijfers en afbreekstreepjes). De naam moet beginnen en eindigen met een letter of cijfer.

    2.  Kies een **abonnement** in de vervolgkeuzelijst. Het abonnement is gekoppeld aan uw factureringsrekening. Dit veld wordt niet weergegeven als u slechts één abonnement hebt.

    3.  Voor **resourcegroep**, selecteer een bestaande of maak een nieuwe groep. Zie [Azure-resourcegroepen](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/) voor meer informatie.

    4.  Geef een **locatie** voor uw service op. Zie [Azure-regio's](https://azure.microsoft.com/regions/#services) voor meer informatie over welke services beschikbaar in welke regio zijn. Kies in het algemeen een **locatie** die het dichtst bij de geografische regio waar u uw apparaat implementeert. U kunt ook rekening houden met het volgende:

        -   Als u bestaande workloads in Azure die u ook wilt implementeren met uw StorSimple-apparaat hebt, raden wij aan dat u dit datacenter.

        -   Uw StorSimple Device Manager en Azure storage kan zich op twee verschillende locaties bevinden. In dat geval moet u de StorSimple--apparaatbeheerfunctie en het Azure Storage-account afzonderlijk maken. Voor het maken van een Azure storage-account, gaat u naar Azure Storage in Azure portal en volg de stappen beschreven in [een opslagaccount maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). Nadat u dit account hebt gemaakt, voegt u het toe aan de StorSimple-apparaatbeheerfunctie met de stappen in [Een nieuw opslagaccount voor de service maken](https://azure.microsoft.com/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service).

        -   Als het virtuele apparaat in Government-Portal implementeert, is de StorSimple Device Manager-service is beschikbaar in VS Iowa en ons Virginia locaties.

    5.  Selecteer **een nieuw Azure storage-account maken** automatisch een opslagaccount maken met de service. Geef een **opslagaccountnaam**. Als u een andere locatie voor uw gegevens wilt kiezen, schakelt u dit vakje uit.

    6.  Schakel **Aan dashboard vastmaken** in als u een snelkoppeling naar deze service op uw dashboard wilt.

    7.  Klik op **Maken** om de StorSimple-apparaatbeheerfunctie te maken.

        ![Nieuwe service maken](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

U bent omgeleid naar de **Service** landingspagina. Het maken van de service duurt enkele minuten. Nadat de service is gemaakt, krijgt u hierover een melding en wordt de status van de service gewijzigd in **Actief**.


