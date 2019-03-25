---
title: Gegevens transformeren met Azure Data Box Edge | Microsoft Docs
description: Leer hoe u een rekenprocesrol configureert in Data Box Edge en deze gebruikt om gegevens te transformeren voordat ze naar Azure worden verzonden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: 31911c124aeafecb8ee37d14e58d3a0bdc0d4955
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400747"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge"></a>Zelfstudie: Gegevens transformeren met Azure Data Box Edge

In deze zelfstudie wordt beschreven hoe u een rekenprocesrol configureert op uw Azure Data Box Edge-apparaat. Wanneer u de rekenrol hebt geconfigureerd, kan Data Box Edge gegevens transformeren voordat deze naar Azure worden verzonden.

Deze procedure kan ongeveer 10 tot 15 minuten duren om uit te voeren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Rekenproces configureren
> * Shares toevoegen
> * Een rekenprocesmodule toevoegen
> * Gegevenstransformatie controleren en gegevens overdragen

 
## <a name="prerequisites"></a>Vereisten

Zorg dat aan deze voorwaarde wordt voldaan voordat u een rekenprocesrol configureert op uw Data Box Edge-apparaat:

- U hebt het Data Box Edge-apparaat geactiveerd zoals beschreven in [Azure Data Box Edge aansluiten, instellen en activeren](data-box-edge-deploy-connect-setup-activate.md).


## <a name="configure-compute"></a>Rekenproces configureren

Als u wilt configureren rekenkracht voor uw gegevens in het edge-apparaten, maakt u een IoT Hub-resource.

1. In de Azure-portal van uw gegevens in het Edge-resource, gaat u naar het overzicht. In het rechterdeelvenster op de **Compute** tegel, selecteer **aan de slag**.

    ![Aan de slag met compute](./media/data-box-edge-deploy-configure-compute/configure-compute-1.png)

2. Op de **configureren Edge Computing** tegel, selecteer **berekening configureren**.
3. Op de **configureren Edge Computing** blade, het volgende invoeren:

   
    |Veld  |Value  |
    |---------|---------|
    |IoT Hub     | Kies uit **nieuwe** of **bestaande**. <br> Standaard wordt een Standard-laag (S1) gebruikt om een IoT-resource te maken. Voor het gebruik van een gratis laag van IoT-resource, maakt u er een en selecteer vervolgens de bestaande resource. <br> In elk geval maakt gebruik van de IoT Hub-resource hetzelfde abonnement en resourcegroep die wordt gebruikt door de gegevens in het Edge-resource.     |
    |Name     |Voer een naam voor uw IoT-Hub.         |

    ![Aan de slag met compute](./media/data-box-edge-deploy-configure-compute/configure-compute-2.png)

4. Selecteer **Maken**. Het maken van de IoT Hub-resource duurt enkele minuten duren. Nadat de IoT Hub-resource is gemaakt, de **berekening configureren** tegel updates om de rekenconfiguratie weer te geven. Om te bevestigen dat de Edge-compute-rol is geconfigureerd, selecteer **weergave Compute** op de **berekening configureren** tegel.
    
    ![Aan de slag met compute](./media/data-box-edge-deploy-configure-compute/configure-compute-3.png)

    Wanneer de Edge-rekenprocesrol wordt geconfigureerd op het Edge-apparaat, worden er twee apparaten aangemaakt: een IoT-apparaat en een IoT Edge-apparaat. Beide apparaten kunnen worden weergegeven in de IoT Hub-resource. Een IoT Edge-Runtime wordt ook uitgevoerd op dit apparaat IoT Edge. Op dit moment is alleen het Linux-platform beschikbaar voor uw IoT Edge-apparaat.


## <a name="add-shares"></a>Shares toevoegen

Voor de eenvoudige implementatie in deze zelfstudie, moet u twee shares: één Edge-share- en een andere lokale Edge-share.

1. Een Edge-share op het apparaat toevoegen aan de hand van de volgende stappen uit:

    1. In uw gegevens in het Edge-resource, gaat u naar **Edge compute > aan de slag**.
    2. Op de **share (s) toevoegen** tegel, selecteer **toevoegen**.
    3. Op de **toevoegen share** blade, geeft u de naam van de bestandsshare en selecteer het sharetype.
    4. Voor het koppelen van de Edge-share, schakel het selectievakje voor **gebruik van de bestandsshare met Edge Computing**.
    5. Selecteer de **opslagaccount**, **opslagservice**, een bestaande gebruiker en selecteer vervolgens **maken**.

        ![Een Edge-share toevoegen](./media/data-box-edge-deploy-configure-compute/add-edge-share-1.png) 

    Als u een lokale NFS-share hebt gemaakt, gebruikt u de volgende optie van de opdracht rsync (op afstand synchroniseren) om bestanden naar de share te kopiëren:

    `rsync <source file path> < destination file path>`

    Zie de [Rsync-documentatie](https://www.computerhope.com/unix/rsync.htm) voor meer informatie over de opdracht rsync.

    De Edge-share is gemaakt en u ontvangt een melding is gemaakt. De lijst shares wordt mogelijk bijgewerkt. U moet echter wachten tot het maken van de share is voltooid.

2. Toevoegen van een lokale share Edge op het Edge-apparaat door de stappen in de voorgaande stap te herhalen en selecteer daarbij het selectievakje voor **configureren als lokale share Edge**. De gegevens in de lokale share blijft van toepassing op het apparaat.

    ![Een Edge lokale share toevoegen](./media/data-box-edge-deploy-configure-compute/add-edge-share-2.png)

  
3. Selecteer **share (s) toevoegen** om te zien van de bijgewerkte lijst met shares.

    ![Bijgewerkte lijst met shares](./media/data-box-edge-deploy-configure-compute/add-edge-share-3.png) 
 

## <a name="add-a-module"></a>Een module toevoegen

U kunt een aangepaste of een vooraf samengestelde module toevoegen. Er zijn geen aangepaste modules op deze Edge-apparaat. Ga naar [Een C#-module ontwikkelen voor uw Data Box Edge-apparaat](data-box-edge-create-iot-edge-module.md) voor meer informatie over het maken van een aangepaste module.

In dit gedeelte voegt u een aangepaste module toe aan het IoT Edge-apparaat dat u hebt gemaakt in [Een C#-module ontwikkelen voor uw Data Box Edge](data-box-edge-create-iot-edge-module.md). Deze aangepaste module bestanden in een lokale share Edge op het Edge-apparaat gaat en verplaatst naar een bestandsshare Edge (cloud) op het apparaat. De cloudshare pusht de bestanden vervolgens naar het Azure-opslagaccount dat is gekoppeld aan de cloudshare.

1. Ga naar **Edge compute > aan de slag**. Op de **modules toevoegen** tegel, selecteert u het scenariotype als **eenvoudige**. Selecteer **Toevoegen**.
2. In de **configureren en voegt u de module** blade, voer de volgende waarden:

    
    |Veld  |Value  |
    |---------|---------|
    |Name     | Een unieke naam voor de module. Deze module is een docker-container die u kunt implementeren op het IoT Edge-apparaat dat is gekoppeld aan de rand van uw gegevens in.        |
    |URI installatiekopie     | De afbeelding URI voor de bijbehorende containerinstallatiekopie voor de module.        |
    |Referenties die nodig zijn     | Als dit selectievakje inschakelt, worden gebruikersnaam en wachtwoord gebruikt om op te halen van modules met een overeenkomende URL.        |
    |Invoer delen     | Selecteer een invoer-share. De invoer share in de lokale share Edge in dit geval is. Hier wordt gebruikt in de module verplaatst bestanden vanaf de rand lokale share naar een share Edge waar ze worden geüpload naar de cloud.        |
    |Uitvoer delen     | Selecteer een uitvoer-share. De Edge-share is in dit geval de uitvoer-share.        |
    |Triggertype     | Selecteer een **bestand** of **planning**. Een bestand-trigger wordt geactiveerd wanneer een bestandgebeurtenis zich voordoet, zoals een bestand wordt geschreven naar de invoer-share. Een geplande trigger wordt geactiveerd om op basis van een door u gedefinieerde planning.         |
    |Triggernaam     | Een unieke naam voor de trigger.         |
    |Omgevingsvariabelen| Optionele informatie die wordt het definiëren van de omgeving waarin de module wordt uitgevoerd.   |

    ![Toevoegen en configureren van module](./media/data-box-edge-deploy-configure-compute/add-module-1.png)

3. Selecteer **Toevoegen**. De module wordt toegevoegd. De **toevoegen module** tegel updates om aan te geven dat de module wordt geïmplementeerd. 

    ![Module geïmplementeerd](./media/data-box-edge-deploy-configure-compute/add-module-2.png)

### <a name="verify-data-transform-and-transfer"></a>Gegevenstransformatie controleren en gegevens overdragen

De laatste stap is controleren of de module is verbonden en wordt uitgevoerd zoals verwacht. De runtimestatus van de module moet 'Wordt uitgevoerd' zijn voor uw IoT Edge-apparaat in de IoT Hub-resource.

Doe het volgende om te controleren of de module wordt uitgevoerd:

1. Selecteer de **toevoegen module** tegel. Hiermee gaat u naar de **Modules** blade. Identificeer de module die u hebt geïmplementeerd in de lijst met modules. De runtimestatus van de module die u hebt toegevoegd moet *met*.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-1.png)
 
1.  In Windows Verkenner, verbinding maken met de lokale Edge en de Edge-shares die u eerder hebt gemaakt.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-2.png) 
 
1.  Voeg gegevens toe aan de lokale share.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-3.png) 
 
    De gegevens worden verplaatst naar de cloudshare.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-4.png)  

    De gegevens worden vervolgens vanuit de cloudshare naar het opslagaccount gepusht. Ga naar de Storage Explorer om de gegevens te bekijken.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-5.png) 
 
U hebt het validatieproces voltooid.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Rekenproces configureren
> * Shares toevoegen
> * Een rekenprocesmodule toevoegen
> * Gegevenstransformatie controleren en gegevens overdragen

Als u wilt weten hoe u uw Data Box Edge-apparaat beheert, gaat u naar:

> [!div class="nextstepaction"]
> [Lokale webgebruikersinterface gebruiken om Data Box Edge te beheren](data-box-edge-manage-access-power-connectivity-mode.md)
