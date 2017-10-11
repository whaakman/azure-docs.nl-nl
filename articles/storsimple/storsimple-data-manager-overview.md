---
title: Overzicht van Microsoft Azure StorSimple Data Manager | Microsoft Docs
description: Biedt een overzicht van de StorSimple Data Manager-service (afgeschermd voorbeeld)
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: aedb44610fe57055851538b9dbdb810e66e58d73
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a>Overzicht van StorSimple Data Manager (afgeschermd voorbeeld)

## <a name="overview"></a>Overzicht

Microsoft Azure StorSimple is een hybride cloud-opslagoplossing die zijn gericht op de complexiteit van niet-gestructureerde gegevens die betrekking hebben op bestandsshares. StorSimple maakt gebruik van cloud-opslag als een uitbreiding van de on-premises oplossing en automatisch lagen gegevens voor de lokale opslag- en cloud-opslag. Gegevensbescherming, geïntegreerd met lokale en cloud worden opgeslagen, hoeft u voor een weids opslaginfrastructuur. Er is ook het archiveren en herstel na noodgevallen naadloze aan de cloud die fungeert als een externe locatie.

De service voor de transformatie van gegevens die worden geïntroduceerd in dit document, kunt u naadloos toegang krijgen tot de StorSimple-gegevens in de cloud. Deze service biedt API's om de gegevens ophalen uit de StorSimple en deze voor andere Azure-services in een indeling die gemakkelijk kan worden gebruikt. De indelingen ondersteund in dit voorbeeld zijn Azure-blobs en activa Azure Media Services. Deze transformatie kunt u eenvoudig wire-services zoals Azure Media Services, Azure HDInsight Azure Machine Learning en Azure Search gegevens op StorSimple 8000 series on-premises apparaat werken.

Een hoog niveau diagram ter illustratie van deze worden hieronder weergegeven.

![Diagram op hoog niveau](./media//storsimple-data-manager-overview/high-level-diagram.png)

Dit document wordt uitgelegd hoe u zich kunt aanmelden voor een persoonlijke voorbeeld van deze service. Ook wordt uitgelegd hoe u deze service om toepassingen die gebruikmaken van StorSimple-gegevens te schrijven en andere Azure-services kunt gebruiken in de cloud.

## <a name="sign-up-for-data-manager-preview"></a>Aanmelden voor de preview van Data Manager
Voordat u zich registreert voor de service Manager gegevens, controleert u de volgende vereisten.

### <a name="prerequisites"></a>Vereisten

In deze oefening wordt ervan uitgegaan dat u hebt
* Een actief Azure-abonnement.
* toegang tot een geregistreerd StorSimple 8000 series apparaat
* alle sleutels die zijn gekoppeld aan het apparaat van de serie StorSimple 8000.

### <a name="sign-up"></a>Aanmelden

De StorSimple Data Manager is private preview. Voer de volgende stappen voor het aanmelden voor een persoonlijke voorbeeld van deze service:

1.  Meld u aan bij de Azure-portal met de extensie StorSimple Data Manager op: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager). Uw Azure-referenties gebruiken om aan te melden.

2.  Klik op de  **+**  pictogram om een service te maken. Klik op **opslag** en klik vervolgens op **alle Zie** in de blade die wordt geopend.

    ![Pictogram voor StorSimple Manager gegevens zoeken](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. U ziet het pictogram StorSimple Data Manager.

    ![Selecteer de gegevens van de StorSimple Manager-pictogram](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. Klik op het pictogram StorSimple Data Manager en klik vervolgens op **maken**. Selecteer het abonnement dat u wilt inschakelen voor de private preview en klik vervolgens op **inschrijven!**

    ![Inschrijven](./media/storsimple-data-manager-overview/sign-me-up.png)

5. Dit wordt een aanvraag verzonden naar vrijgeven u. We zullen we u zo snel mogelijk. Nadat uw abonnement is ingeschakeld, kunt u een StorSimple Data Manager-service.

6. Voor eenvoudig toegang tot de StorSimple Data Manager-service, klikt u op het sterpictogram op vastmaken aan uw Favorieten.

    ![Toegang tot gegevens van de StorSimple Managers](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md).