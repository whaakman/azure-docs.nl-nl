---
title: Gegevens transformeren met Azure Data Box Edge | Microsoft Docs
description: Leer hoe u een rekenprocesrol configureert in Data Box Edge en deze gebruikt om gegevens te transformeren voordat ze naar Azure worden verzonden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/19/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: e970f7cc0d4c4620f2da69286be36d0c22e0d747
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260360"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-preview"></a>Zelfstudie: Gegevens transformeren met Azure Data Box Edge (preview)

In deze zelfstudie wordt beschreven hoe u een rekenprocesrol configureert in Data Box Edge. Als rol is geconfigureerd, kan Data Box Edge gegevens transformeren voordat deze naar Azure worden verzonden.

Deze procedure neemt 30-45 minuten in beslag.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een IoT Hub-resource maken
> * Een rekenprocesrol configureren
> * Een rekenprocesmodule toevoegen
> * Gegevenstransformatie controleren en gegevens overdragen

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert.
 
## <a name="prerequisites"></a>Vereisten

Zorg dat aan deze voorwaarde is voldaan voordat u een rekenprocesrol configureert in Data Box Edge:

* Uw Data Box Edge-apparaat is geactiveerd zoals wordt beschreven in [Uw Azure Data Box-Edge aansluiten en activeren](data-box-edge-deploy-connect-setup-activate.md).


## <a name="create-an-iot-hub-resource"></a>Een IoT Hub-resource maken

Voordat u de rekenprocesrol configureert in Data Box Edge, moet u een IoT Hub-resource maken.

Zie [Een IoT Hub maken](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#create-an-iot-hub) voor gedetailleerde instructies. Gebruik hetzelfde abonnement en de resourcegroep die u hebt gebruikt voor uw Data Box Edge-resource.

![IoT Hub-resource maken](./media/data-box-edge-deploy-configure-compute/create-iothub-resource-1.png)

Het volgende geldt als de rekenprocesrol niet is ingesteld in Edge:

- De IoT Hub-resource beschikt niet over IoT-apparaten of IoT Edge-apparaten.
- U kunt geen lokale Edge-shares maken. Wanneer u een share toevoegt, is de optie voor het maken van een lokale share voor Edge-computing niet ingeschakeld.


## <a name="set-up-compute-role"></a>Een rekenprocesrol configureren

Wanneer de rekenprocesrol wordt geconfigureerd op het Edge-apparaat, worden er twee apparaten aangemaakt: een IoT-apparaat en een IoT Edge-apparaat. Beide apparaten kunnen worden weergegeven in de IoT Hub-resource.

Als u de rekenprocesrol wilt instellen op het apparaat, moet u de volgende stappen uitvoeren.

1. Ga naar de Data Box Edge-resource, ga naar **Overzicht** en klik op **Rekenprocesrol configureren**. 

    ![Een rekenprocesrol configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-1.png)
   
    U kunt ook naar **Modules** gaan en op **Rekenproces configureren** klikken.

    ![Een rekenprocesrol configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-2.png)
 
2. Selecteer in de vervolgkeuzelijst de **IoT-Hub resource** die u in de vorige stap hebt gemaakt. Alleen het Linux-platform is op dit moment beschikbaar voor uw IoT Edge-apparaat. Klik op **Create**.

    ![Een rekenprocesrol configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-3.png)
 
3. Het duurt een paar minuten om de rekenprocesrol te maken. Vanwege een fout in deze release, wordt het scherm niet vernieuwd, zelfs niet wanneer de rekenprocesrol is gemaakt. Ga naar **Modules** en u ziet daar dat Edge-computing is geconfigureerd.  

    ![Een rekenprocesrol configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-4.png)

4. Ga opnieuw naar **Overzicht** en nu wordt het scherm bijgewerkt om aan te geven dat de rekenprocesrol is geconfigureerd.

    ![Een rekenprocesrol configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-5.png)
 
5. Ga naar de IoT-Hub die u hebt gebruikt bij het maken van de Edge-rekenprocesrol. Ga naar **IoT-apparaten**. U kunt zien dat er nu een IoT-apparaat is ingeschakeld. 

    ![Een rekenprocesrol configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-6.png)

6. Ga naar **IoT Edge** en u ziet dat er ook een IoT Edge-apparaat is ingeschakeld.

    ![Een rekenprocesrol configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-7.png)
 
7. Selecteer het IoT Edge-apparaat en klik erop. Er wordt een Edge-agent uitgevoerd op dit IoT Edge-apparaat. 

    ![Een rekenprocesrol configureren](./media/data-box-edge-deploy-configure-compute/setup-compute-8.png) 

Er zijn echter geen aangepaste modules aanwezig op dit Edge-apparaat. U kunt nu een aangepaste module toevoegen aan dit apparaat. Ga naar [Een C#-module ontwikkelen voor uw Data Box Edge](data-box-edge-create-iot-edge-module.md) voor meer informatie over het maken van een aangepaste module.


## <a name="add-a-custom-module"></a>Een aangepaste module toevoegen

In deze sectie voegt u een aangepaste module toe aan het IoT Edge-apparaat dat u hebt gemaakt in [Een C#-module ontwikkelen voor uw Data Box Edge](data-box-edge-create-iot-edge-module.md). 

Deze procedure maakt gebruik van een voorbeeld waarin de gebruikte aangepaste module bestanden van een lokale share op het Edge-apparaat neemt en deze verplaatst naar een cloudshare op het apparaat. De cloudshare pusht de bestanden vervolgens naar het Azure-opslagaccount dat is gekoppeld aan de cloudshare. 

1. De eerste stap is het toevoegen van een lokale share op het Edge-apparaat. Ga in de Data Box Edge-resource naar **Shares**. Klik op **+ Share toevoegen**. Geef een naam op voor de share en selecteer het type. Als u een lokale share wilt maken, schakelt u het selectievakje **Configureren als lokale Edge-share** in. Selecteer een **bestaande gebruiker** of **maak een nieuwe**. Klik op **Create**.

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-1.png) 

    Als u een lokale NFS-share hebt gemaakt, gebruikt u de volgende optie van de opdracht rsync om bestanden naar de share te kopiëren:

    `rsync --inplace <source file path> < destination file path>`

     Zie de [Rsync-documentatie](https://www.computerhope.com/unix/rsync.htm) voor meer informatie over de opdracht rsync. 

 
2. Zodra de lokale share is gemaakt en u hiervan een melding hebt ontvangen (de lijst met shares kan eerder zijn bijgewerkt maar u moet wachten tot u de melding hebt gekregen), gaat u naar de lijst met shares. 

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-2.png) 
 
3. Selecteer en klik op de zojuist gemaakte lokale share en bekijk de eigenschappen van de share. Kopieer en bewaar het **lokale koppelpunt voor Edge-modules** dat overeenkomt met deze share.

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-3.png) 
 
    Ga naar een bestaande cloudshare die op het Edge-apparaat is gemaakt. Kopieer en bewaar ook het lokale koppelpunt voor Edge-rekenmodules voor deze cloudshare. Deze lokale koppelpunten worden gebruikt bij het implementeren van de module.

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-4.png)  

4. Als u een aangepaste module wilt toevoegen aan het IoT Edge-apparaat, gaat u naar uw IoT Hub-resource en vervolgens naar **IoT Edge-apparaat**. Selecteer het apparaat en klik erop. Klik in het venster **Apparaatdetails**, in de opdrachtbalk bovenaan het scherm, op **Modules instellen**. 

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-5.png) 

5. Ga als volgt te werk onder **Modules toevoegen**:

    1. Geef bij **Containerregisterinstellingen** waarden op voor **Naam**, **Adres**, **Gebruikersnaam** en **Wachtwoord** voor de aangepaste module. De naam, het adres en de vermelde referenties worden gebruikt om modules met een overeenkomende URL op te halen. Als u deze module wilt implementeren, selecteert u **IoT Edge-module** onder **Implementatiemodules**. Deze IoT Edge-module is een Docker-container die u kunt implementeren op het IoT Edge-apparaat dat is gekoppeld aan uw Data Box Edge-apparaat.

        ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-6.png) 
 
    2. Geef de instellingen voor de aangepaste IoT Edge-module op. Geef de **naam** van de module en **URI installatiekopie** op voor de bijbehorende containerinstallatiekopie. 
    
        ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-7.png) 

    3. Geef bij **Opties Container maken** de lokale koppelpunten op voor de Edge-modules die u in de voorgaande stappen hebt gekopieerd voor de cloudshare en de lokale share (het is belangrijk dat u deze paden gebruikt in plaats van nieuwe maakt). De lokale koppelpunten worden toegewezen aan de bijbehorende **InputFolderPath** en de **OutputFolderPath** die u hebt opgegeven in de module toen u [de module hebt bijgewerkt met aangepaste code](data-box-edge-create-iot-edge-module.md#update-the-module-with-custom-code). 
    
        U kunt het voorbeeld dat hieronder wordt weergegeven in uw **opties Container maken** kopiëren en plakken: 
        
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

        Geef hier ook eventuele omgevingsvariabelen op voor uw module.

        ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-8.png) 
 
    4. Klik desgewenst op **Geavanceerde runtime-instellingen voor Edge configureren** en klik vervolgens op **Volgende**.

        ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-9.png) 
 
6.  Onder **Routes opgeven** kunt u routes tussen modules instellen. In dit geval geeft u de naam op van de lokale share die gegevens naar de cloudshare pusht. Klik op **Volgende**.

    U kunt route vervangen door de volgende routetekenreeks:       "route": "FROM /* WHERE topic = 'mysmblocalshare' INTO BrokeredEndpoint(\"/modules/filemovemodule/inputs/input1\")"

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-10.png) 
 
7.  Controleer alle instellingen onder **Implementatie controleren**. Als u tevreden bent, klikt u op **Verzenden** om de module aan te bieden voor implementatie.

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-11.png) 
 
Hiermee start u de implementatie van de module, zoals wordt aangegeven bij **Aangepaste IoT Edge-module** onder **Modules**.

![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute/add-a-custom-module-12.png) 

### <a name="verify-data-transform-and-transfer"></a>Gegevenstransformatie controleren en gegevens overdragen

De laatste stap is controleren of de module is verbonden en wordt uitgevoerd zoals verwacht. Voer de volgende stappen uit om te controleren of de module wordt uitgevoerd.

1. De runtimestatus van de module moet 'Wordt uitgevoerd' zijn voor uw IoT Edge-apparaat in de IoT Hub-resource.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-1.png) 
 
2. Selecteer de module en klik erop en kijk onder **Module-identiteitsdubbel**. De clientstatus voor het Edge-apparaat en de module moet worden weergegeven als **Connected**.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-2.png) 
 
3.  Zodra de module wordt uitgevoerd, wordt deze ook weergegeven onder de lijst met Edge-modules in uw Data Box Edge-resource. De **runtimestatus** van de module die u hebt toegevoegd, is **wordt uitgevoerd**.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-3.png) 
 
4.  Maak via Verkenner verbinding met zowel de lokale share als de cloudshare die u hebt gemaakt.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-4.png) 
 
5.  Voeg gegevens toe aan de lokale share.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-5.png) 
 
6.  De gegevens worden verplaatst naar de cloudshare.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-6.png)  

7.  De gegevens worden vervolgens vanuit de cloudshare naar het opslagaccount gepusht. Ga naar de Storage Explorer om de gegevens te bekijken.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute/verify-data-transform-7.png) 
 
Hiermee is het validatieproces afgerond.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Data Box Edge, zoals:

> [!div class="checklist"]
> * Een IoT Hub-resource maken
> * Een rekenprocesrol configureren
> * Een rekenprocesmodule toevoegen
> * Gegevenstransformatie controleren en gegevens overdragen

Ga naar de volgende zelfstudie om te lezen hoe u Data Box Edge kunt beheren.

> [!div class="nextstepaction"]
> [Lokale webgebruikersinterface gebruiken om Data Box Edge te beheren](https://aka.ms/dbg-docs)


