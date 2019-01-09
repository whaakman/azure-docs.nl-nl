---
title: Gegevens transformeren met Azure Data Box Edge | Microsoft Docs
description: Leer hoe u een rekenprocesrol configureert in Data Box Edge en deze gebruikt om gegevens te transformeren voordat ze naar Azure worden verzonden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 11/27/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: c0901f22e4941fdfaa21138153a06e97c2d6095f
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630373"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-preview"></a>Zelfstudie: Gegevens transformeren met Azure Data Box Edge (preview)

In deze zelfstudie wordt beschreven hoe u een rekenprocesrol configureert op uw Azure Data Box Edge-apparaat. Wanneer u de rekenrol hebt geconfigureerd, kan Data Box Edge gegevens transformeren voordat deze naar Azure worden verzonden.

Deze procedure neemt 30-45 minuten in beslag.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure IoT Hub-resource maken
> * Een rekenprocesrol configureren
> * Een rekenprocesmodule toevoegen
> * Gegevenstransformatie controleren en gegevens overdragen

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Bekijk de [Azure-gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert.
 
## <a name="prerequisites"></a>Vereisten

Zorg dat aan deze voorwaarde wordt voldaan voordat u een rekenprocesrol configureert op uw Data Box Edge-apparaat:

* U hebt het Data Box Edge-apparaat geactiveerd zoals beschreven in [Azure Data Box Edge aansluiten, instellen en activeren](data-box-edge-deploy-connect-setup-activate.md).


## <a name="create-an-iot-hub-resource"></a>Een IoT Hub-resource maken

Voordat u een rekenprocesrol configureert in Data Box Edge, moet u een IoT Hub-resource maken.

Zie [Een IoT Hub maken](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub) voor gedetailleerde instructies. Gebruik hetzelfde abonnement en de resourcegroep die u hebt gebruikt voor uw Data Box Edge-resource.

![Een IoT Hub-resource maken](./media/data-box-edge-deploy-configure-compute/create-iothub-resource-1.png)

Als er nog geen Edge-rekenprocesrol is ingesteld, zijn de volgende zaken het geval:

- De IoT Hub-resource beschikt niet over Azure IoT-apparaten of Azure IoT Edge-apparaten.
- U kunt geen lokale Edge-shares maken. Wanneer u een share toevoegt, is de optie voor het maken van een lokale share voor Edge-computing niet ingeschakeld.


## <a name="set-up-compute-role"></a>Een rekenprocesrol configureren

Wanneer de Edge-rekenprocesrol wordt geconfigureerd op het Edge-apparaat, worden er twee apparaten aangemaakt: een IoT-apparaat en een IoT Edge-apparaat. Beide apparaten kunnen worden weergegeven in de IoT Hub-resource.

Als u de rekenprocesrol wilt instellen op het apparaat, doet u het volgende:

1. Ga naar de Data Box Edge-resource, ga naar **Overzicht** en selecteer **Rekenprocesrol configureren**. 

    ![De koppeling Overzicht in het linkerdeelvenster](./media/data-box-edge-deploy-configure-compute/setup-compute-1.png)
   
    Optioneel kunt u naar **Modules** gaan en selecteert u **Rekenproces configureren**.

    ![De koppelingen Modules en Rekenproces configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-2.png)
 
1. Selecteer in de vervolgkeuzelijst de **IoT Hub-resource** die u in de vorige stap hebt gemaakt.  
    Op dit moment is alleen het Linux-platform beschikbaar voor uw IoT Edge-apparaat. 
    
1. Klik op **Create**.

    ![De knop Maken](./media/data-box-edge-deploy-configure-compute/setup-compute-3.png)
 
    Het duurt een paar minuten om de rekenprocesrol te maken. Vanwege een fout in deze release, wordt het scherm niet vernieuwd, zelfs niet wanneer de rekenprocesrol is gemaakt. Ga naar **Modules** om te controleren of de Edge-rekenprocesrol is geconfigureerd.  

    ![De apparaatlijst voor 'Edge-rekenprocesrol configureren'](./media/data-box-edge-deploy-configure-compute/setup-compute-4.png)

1. Ga opnieuw naar **Overzicht**.  
    Het scherm wordt bijgewerkt om aan te geven dat de rekenprocesrol is geconfigureerd.

    ![Een rekenprocesrol configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-5.png)
 
1. Ga in de IoT Hub die u hebt gebruikt bij het maken van de Edge-rekenprocesrol naar **IoT-apparaten**.  
    Er wordt nu een IoT-apparaat ingeschakeld. 

    ![De pagina IoT-apparaten](./media/data-box-edge-deploy-configure-compute/setup-compute-6.png)

1. Selecteer **IoT Edge** in het linkerdeelvenster.  
    Er wordt ook een IoT Edge-apparaat ingeschakeld.

    ![Een rekenprocesrol configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-7.png)
 
1. Selecteer het IoT Edge-apparaat en klik erop.  
    Er wordt een Edge-agent uitgevoerd op dit IoT Edge-apparaat. 

    ![De pagina met apparaatdetails](./media/data-box-edge-deploy-configure-compute/setup-compute-8.png) 

    Er staan geen aangepaste modules op dit Edge-apparaat, maar u kunt nu wel een aangepaste module toevoegen. Ga naar [Een C#-module ontwikkelen voor uw Data Box Edge-apparaat](data-box-edge-create-iot-edge-module.md) voor meer informatie over het maken van een aangepaste module.


## <a name="add-a-custom-module"></a>Een aangepaste module toevoegen

In dit gedeelte voegt u een aangepaste module toe aan het IoT Edge-apparaat dat u hebt gemaakt in [Een C#-module ontwikkelen voor uw Data Box Edge](data-box-edge-create-iot-edge-module.md). 

In de volgende procedure wordt gebruikgemaakt van een voorbeeld waarin de aangepaste module bestanden van een lokale share op het Edge-apparaat neemt en deze verplaatst naar een cloudshare op het apparaat. De cloudshare pusht de bestanden vervolgens naar het Azure-opslagaccount dat is gekoppeld aan de cloudshare. 

1. U kunt op de volgende manier een lokale share toevoegen aan het Edge-apparaat:

    a. Ga in de Data Box Edge-resource naar **Shares**. 
    
    b. Selecteer **Share toevoegen**, geef een naam op voor de share en selecteer het sharetype. 
    
    c. Als u een lokale share wilt maken, schakelt u het selectievakje bij **Configureren als lokale Edge-share** in. 
    
    d. Selecteer **Nieuwe maken** of **Bestaande gebruiken** en selecteer vervolgens **Maken**.

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-1.png) 

    Als u een lokale NFS-share hebt gemaakt, gebruikt u de volgende optie van de opdracht rsync (op afstand synchroniseren) om bestanden naar de share te kopiëren:

    `rsync --inplace <source file path> < destination file path>`

    Zie de [Rsync-documentatie](https://www.computerhope.com/unix/rsync.htm) voor meer informatie over de opdracht rsync. 

    De lokale share wordt gemaakt en u ontvangt de melding dat het maken is voltooid. De lijst shares wordt mogelijk bijgewerkt. U moet echter wachten tot het maken van de share is voltooid.
    
1. Ga naar de lijst met shares. 

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-2.png) 
 
1. Als u de eigenschappen van de zojuist gemaakte lokale share wilt bekijken, selecteert u de share. 

1. Kopieer de waarde voor de share uit het vak **Lokaal koppelpunt voor Edge-rekenprocesmodules**.  
    U gebruik dit lokale koppelpunt bij het implementeren van de module.

    ![Het vak Lokaal koppelpunt voor Edge-rekenprocesmodules](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-3.png) 
 
1. Ga naar een bestaande cloudshare die is gemaakt op uw Data Box Edge-apparaat, ga naar het vak **Lokaal koppelpunt voor Edge-rekenprocesmodules** en kopieer en lokale koppelpunt voor Edge-rekenprocesmodules voor deze cloudshare.  
    U gebruik dit lokale koppelpunt bij het implementeren van de module.

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-4.png)  

1. Als u een aangepaste module wilt toevoegen aan het IoT Edge-apparaat, gaat u naar uw IoT Hub-resource en vervolgens naar **IoT Edge-apparaat**. 

1. Selecteer het apparaat en selecteer dan onder **Apparaatdetails** de optie **Modules instellen**. 

    ![De koppeling Modules instellen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-5.png) 

1. Doe het volgende in **Modules toevoegen**:

    a. Geef in de Containerregisterinstellingen waarden op voor Naam, Adres, Gebruikersnaam en Wachtwoord voor de aangepaste module.  
    De naam, het adres en de vermelde referenties worden gebruikt om modules met een overeenkomende URL op te halen. Als u deze module wilt implementeren, selecteert u **IoT Edge-module** onder **Implementatiemodules**. Deze IoT Edge-module is een Docker-container die u kunt implementeren op het IoT Edge-apparaat dat is gekoppeld aan uw Data Box Edge-apparaat.

    ![De pagina Modules instellen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-6.png) 
 
    b. Geef de instellingen voor de aangepaste IoT Edge-module op door de naam van uw module en de installatiekopie-URI van de bijbehorende containerinstallatiekopie op te geven. 
    
    ![De pagina Aangepaste IoT Edge-modules](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-7.png) 

    c. In het vak **Opties Container maken** voert u de lokale koppelpunten in voor de Edge-modules die u in de voorgaande stappen hebt gekopieerd voor de cloud- en lokale share.
    > [!IMPORTANT]
    > Gebruik de gekopieerde paden. Maak geen nieuwe paden. De lokale koppelpunten worden toegewezen aan de bijbehorende **InputFolderPath** en de **OutputFolderPath** die u hebt opgegeven in de module toen u [de module hebt bijgewerkt met aangepaste code](data-box-edge-create-iot-edge-module.md#update-the-module-with-custom-code). 
    
    In het vak **Opties Container maken** plakt u het volgende voorbeeld: 
    
    ```
    {
        "HostConfig": {
        "Binds": [
        "/home/hcsshares/mysmblocalshare:/home/LocalShare",
        "/home/hcsshares/mysmbshare1:/home/CloudShare"
        ]
        }
    }
    ```

    Geef hier eventuele omgevingsvariabelen op voor uw module.

    ![Het vak Opties Container maken](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-8.png) 
 
    d. Configureer indien nodig de geavanceerde runtime-instellingen voor Edge en klik dan op **Volgende**.

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-9.png) 
 
1.  Onder **Routes opgeven** kunt u routes tussen modules instellen.  
    In dit voorbeeld geeft u de naam op van de lokale share die gegevens naar de cloudshare pusht.

    U kunt de *route* vervangen door de volgende routetekenreeks: `"route": "FROM /* WHERE topic = 'mysmblocalshare' INTO BrokeredEndpoint(\"/modules/filemovemodule/inputs/input1\")"`

    ![Het gedeelte Routes opgeven](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-10.png) 

1. Selecteer **Volgende**. 

1.  Controleer alle instellingen onder **Implementatie controleren**. Klik vervolgens op **Verzenden** om de module te verzenden voor implementatie.

    ![De pagina Modules instellen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-11.png) 
 
    Met deze actie wordt gestart met het implementeren van de module, zoals in de volgende afbeelding wordt weergegeven:

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-12.png) 

### <a name="verify-data-transform-and-transfer"></a>Gegevenstransformatie controleren en gegevens overdragen

De laatste stap is controleren of de module is verbonden en wordt uitgevoerd zoals verwacht. De runtimestatus van de module moet 'Wordt uitgevoerd' zijn voor uw IoT Edge-apparaat in de IoT Hub-resource.

![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-1.png) 
 
Doe het volgende om te controleren of de module wordt uitgevoerd:

1. Selecteer de module en bekijk de module-identiteitsdubbel.  
    De clientstatus voor het Edge-apparaat en de module moet worden weergegeven als *Verbonden*.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-2.png) 
 
    Zodra de module wordt uitgevoerd, wordt deze ook weergegeven onder de lijst met Edge-modules in uw Data Box Edge-resource. De runtimestatus van de module die u hebt toegevoegd, is *Wordt uitgevoerd*.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-3.png) 
 
1.  Maak via Verkenner verbinding met zowel de lokale share als de cloudshare die u eerder hebt gemaakt.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-4.png) 
 
1.  Voeg gegevens toe aan de lokale share.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-5.png) 
 
    De gegevens worden verplaatst naar de cloudshare.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-6.png)  

    De gegevens worden vervolgens vanuit de cloudshare naar het opslagaccount gepusht. Ga naar Storage Explorer om de gegevens te bekijken.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-7.png) 
 
U hebt het validatieproces voltooid.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een IoT Hub-resource maken
> * Een rekenprocesrol configureren
> * Een rekenprocesmodule toevoegen
> * Gegevenstransformatie controleren en gegevens overdragen

Als u wilt weten hoe u uw Data Box Edge-apparaat beheert, gaat u naar:

> [!div class="nextstepaction"]
> [Lokale webgebruikersinterface gebruiken om Data Box Edge te beheren](https://aka.ms/dbg-docs)


