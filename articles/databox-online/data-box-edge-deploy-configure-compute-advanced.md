---
title: Zelfstudie om te filteren, analyseren van gegevens voor geavanceerde implementatie met compute op Azure Data Box Edge | Microsoft Docs
description: Leer hoe u berekeningsfunctie voor gegevens in het edge-apparaten configureren en gebruiken om gegevens te transformeren voor geavanceerde implementatie flow voordat ze worden verzonden naar Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 05/13/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Data Box Edge for advanced deployment flow so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: 836a3f3c17c4cf11ac972b7cc129fb3f83093024
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794057"
---
# <a name="tutorial-transform-data-with-azure-data-box-edge-for-advanced-deployment-flow"></a>Zelfstudie: Gegevens transformeren met Azure Data Box Edge voor geavanceerde implementatie flow

In deze zelfstudie wordt beschreven hoe u een compute-functie voor een stroom geavanceerde implementatie configureren op uw Azure Data Box-Edge-apparaat. Wanneer u de rekenrol hebt geconfigureerd, kan Data Box Edge gegevens transformeren voordat deze naar Azure worden verzonden.

COMPUTE kan worden geconfigureerd voor de stroom van eenvoudige of geavanceerde implementatie op uw apparaat.

|                  | Eenvoudige implementatie                                | Geavanceerde implementatie                   |
|------------------|--------------------------------------------------|---------------------------------------|
| Bedoeld voor     | IT-beheerders                                | Ontwikkelaars                            |
| Type             | Gegevens in het Edge-service gebruiken voor het implementeren van modules      | IoT Hub-service gebruiken om te implementeren van modules |
| Modules die zijn geïmplementeerd | Single                                           | In een keten of meerdere modules           |


Deze procedure kan ongeveer 20-30 minuten duren om uit te voeren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Rekenproces configureren
> * Shares toevoegen
> * Een trigger toevoegen
> * Een rekenprocesmodule toevoegen
> * Gegevenstransformatie controleren en gegevens overdragen

 
## <a name="prerequisites"></a>Vereisten

Zorg dat aan deze voorwaarde wordt voldaan voordat u een rekenprocesrol configureert op uw Data Box Edge-apparaat:

- U hebt het Data Box Edge-apparaat geactiveerd zoals beschreven in [Azure Data Box Edge aansluiten, instellen en activeren](data-box-edge-deploy-connect-setup-activate.md).


## <a name="configure-compute"></a>Rekenproces configureren

Als u wilt configureren rekenkracht voor uw gegevens in het edge-apparaten, maakt u een IoT Hub-resource.

1. In de Azure-portal van uw gegevens in het Edge-resource, gaat u naar **overzicht**. In het rechterdeelvenster op de **Compute** tegel, selecteer **aan de slag**.

    ![Aan de slag met compute](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-1.png)

2. Op de **configureren Edge Computing** tegel, selecteer **berekening configureren**.

    ![Aan de slag met compute](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-2.png)

3. Op de **configureren Edge Computing** blade, het volgende invoeren:

   
    |Veld  |Value  |
    |---------|---------|
    |IoT Hub     | Kies uit **nieuwe** of **bestaande**. <br> Standaard wordt een Standard-laag (S1) gebruikt om een IoT-resource te maken. Voor het gebruik van een gratis laag van IoT-resource, maakt u er een en selecteer vervolgens de bestaande resource. <br> In elk geval maakt gebruik van de IoT Hub-resource hetzelfde abonnement en resourcegroep die wordt gebruikt door de gegevens in het Edge-resource.     |
    |Name     |Voer een naam voor uw IoT-Hub.         |

    ![Aan de slag met compute](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-3.png)

4. Selecteer **Maken**. Het maken van de IoT Hub-resource duurt enkele minuten duren. Nadat de IoT Hub-resource is gemaakt, de **configureren Edge Computing** tegel updates om de rekenconfiguratie weer te geven. Om te bevestigen dat de Edge-compute-rol is geconfigureerd, selecteer **weergave config** op de **berekening configureren** tegel.
    
    ![Aan de slag met compute](./media/data-box-edge-deploy-configure-compute-advanced/configure-compute-4.png)

    Wanneer de Edge-rekenprocesrol wordt geconfigureerd op het Edge-apparaat, worden er twee apparaten aangemaakt: een IoT-apparaat en een IoT Edge-apparaat. Beide apparaten kunnen worden weergegeven in de IoT Hub-resource. Een IoT Edge-Runtime wordt ook uitgevoerd op dit apparaat IoT Edge.

    Op dit moment is alleen het Linux-platform beschikbaar voor uw IoT Edge-apparaat.


## <a name="add-shares"></a>Shares toevoegen

Voor de geavanceerde implementatie in deze zelfstudie, moet u twee shares: één Edge-share- en een andere lokale Edge-share.

1. Een Edge-share op het apparaat toevoegen aan de hand van de volgende stappen uit:

    1. In uw gegevens in het Edge-resource, gaat u naar **Edge compute > aan de slag**.
    2. Op de **share (s) toevoegen** tegel, selecteer **toevoegen**.
    3. Op de **toevoegen share** blade, geeft u de naam van de bestandsshare en selecteer het sharetype.
    4. Voor het koppelen van de Edge-share, schakel het selectievakje voor **gebruik van de bestandsshare met Edge Computing**.
    5. Selecteer de **opslagaccount**, **opslagservice**, een bestaande gebruiker en selecteer vervolgens **maken**.

        ![Een Edge-share toevoegen](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-1.png)

    <!--If you created a local NFS share, use the following remote sync (rsync) command option to copy files onto the share:

    `rsync <source file path> < destination file path>`

    For more information about the rsync command, go to [Rsync documentation](https://www.computerhope.com/unix/rsync.htm).-->

    Nadat de Edge-share is gemaakt, ontvangt u een melding is gemaakt. De lijst met de bestandsshare is bijgewerkt, zodat de nieuwe share.

2. Toevoegen van een lokale share Edge op het Edge-apparaat door de stappen in de voorgaande stap te herhalen en selecteer daarbij het selectievakje voor **configureren als lokale share Edge**. De gegevens in de lokale share blijft van toepassing op het apparaat.

    ![Een Edge lokale share toevoegen](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-2.png)

3. Op de **Shares** blade ziet u de bijgewerkte lijst met shares.

    ![Bijgewerkte lijst met shares](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-3.png)

4. Als u wilt de eigenschappen van de zojuist gemaakte lokale share weergeven, selecteert u de share in de lijst. Kopieer de waarde voor de share uit het vak **Lokaal koppelpunt voor Edge-rekenprocesmodules**.

    U gebruik dit lokale koppelpunt bij het implementeren van de module.

    ![Het vak Lokaal koppelpunt voor Edge-rekenprocesmodules](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-4.png)
 
5. Als u wilt weergeven van de eigenschappen van de Edge-share die u hebt gemaakt, selecteert u de share in de lijst. Kopieer de waarde voor de share uit het vak **Lokaal koppelpunt voor Edge-rekenprocesmodules**.

    U gebruik dit lokale koppelpunt bij het implementeren van de module.

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute-advanced/add-edge-share-5.png)


## <a name="add-a-trigger"></a>Een trigger toevoegen

1. Ga naar **Edge compute > Triggers**. Selecteer **+ toevoegen trigger**.

    ![Trigger toevoegen](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-1.png)

2. In de **toevoegen trigger** blade, voer de volgende waarden.

    |Veld  |Value  |
    |---------|---------|
    |Triggernaam     | Een unieke naam voor de trigger.         |
    |Triggertype     | Selecteer **bestand** trigger. Een bestand-trigger wordt geactiveerd wanneer een bestandgebeurtenis zich voordoet, zoals een bestand wordt geschreven naar de invoer-share. Een geplande trigger aan de andere kant wordt geactiveerd om op basis van een door u gedefinieerde planning. In dit voorbeeld moet de trigger van een bestand.    |
    |Invoer delen     | Selecteer een invoer-share. De invoer share in de lokale share Edge in dit geval is. Hier wordt gebruikt in de module verplaatst bestanden vanaf de rand lokale share naar een share Edge waar ze worden geüpload naar de cloud.        |

    ![Trigger toevoegen](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-2.png)

3. U krijgt een melding nadat de trigger is gemaakt. De lijst met triggers wordt bijgewerkt om de zojuist gemaakte trigger weer te geven. Selecteer de trigger die u zojuist hebt gemaakt.

    ![Trigger toevoegen](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-3.png)

4. Kopieer en bewaar de voorbeeld-route. U wijzigt deze voorbeeld-route en gebruikt deze verderop in de IoT-Hub.

    `"sampleroute": "FROM /* WHERE topic = 'mydbesmbedgelocalshare1' INTO BrokeredEndpoint(\"/modules/modulename/inputs/input1\")"`

    ![Trigger toevoegen](./media/data-box-edge-deploy-configure-compute-advanced/add-trigger-4.png)

## <a name="add-a-module"></a>Een module toevoegen

Er zijn geen aangepaste modules op deze Edge-apparaat. U kunt een aangepaste of een vooraf samengestelde module toevoegen. Ga naar [Een C#-module ontwikkelen voor uw Data Box Edge-apparaat](data-box-edge-create-iot-edge-module.md) voor meer informatie over het maken van een aangepaste module.

In dit gedeelte voegt u een aangepaste module toe aan het IoT Edge-apparaat dat u hebt gemaakt in [Een C#-module ontwikkelen voor uw Data Box Edge](data-box-edge-create-iot-edge-module.md). Deze aangepaste module bestanden in een lokale share Edge op het Edge-apparaat gaat en verplaatst naar een bestandsshare Edge (cloud) op het apparaat. De cloudshare pusht de bestanden vervolgens naar het Azure-opslagaccount dat is gekoppeld aan de cloudshare.

1. Ga naar **Edge compute > aan de slag**. Op de **modules toevoegen** tegel, selecteert u het scenariotype als **geavanceerde**. Selecteer **gaat u naar IoT Hub**.

    ![Geavanceerde implementatie selecteren](./media/data-box-edge-deploy-configure-compute-advanced/add-module-1.png)

<!--2. In the **Configure and add module** blade, input the following values:  

    |Input share     | Select an input share. The Edge local share is the input share in this case. The module used here moves files from the Edge local share to an Edge share where they are uploaded into the cloud.        |
    |Output share     | Select an output share. The Edge share is the output share in this case.        |
-->

2. In de bron van uw IoT-Hub, gaat u naar **IoT Edge-apparaat** en selecteer vervolgens uw IoT Edge-apparaat.

    ![Ga naar de IoT Edge-apparaat in IoT Hub](./media/data-box-edge-deploy-configure-compute-advanced/add-module-2.png)

3. Op **Apparaatdetails**, selecteer **Modules instellen**.

    ![De koppeling Modules instellen](./media/data-box-edge-deploy-configure-compute-advanced/add-module-3.png)

4. Doe het volgende in **Modules toevoegen**:

    1. Geef in de Containerregisterinstellingen waarden op voor Naam, Adres, Gebruikersnaam en Wachtwoord voor de aangepaste module.
    De naam, het adres en de vermelde referenties worden gebruikt om modules met een overeenkomende URL op te halen. Als u deze module wilt implementeren, selecteert u **IoT Edge-module** onder **Implementatiemodules**. Deze IoT Edge-module is een Docker-container die u kunt implementeren op het IoT Edge-apparaat dat is gekoppeld aan uw Data Box Edge-apparaat.

        ![De pagina Modules instellen](./media/data-box-edge-deploy-configure-compute-advanced/add-module-4.png) 
 
    2. Geef de instellingen voor de aangepaste IoT Edge-module op. Voer de volgende waarden.
     
        |Veld  |Value  |
        |---------|---------|
        |Name     | Een unieke naam voor de module. Deze module is een docker-container die u kunt implementeren op het IoT Edge-apparaat dat is gekoppeld aan de rand van uw gegevens in.        |
        |URI installatiekopie     | De afbeelding URI voor de bijbehorende containerinstallatiekopie voor de module.        |
        |Referenties die nodig zijn     | Als dit selectievakje inschakelt, worden gebruikersnaam en wachtwoord gebruikt om op te halen van modules met een overeenkomende URL.        |
    
        In de **Container maken opties** voert u de lokale koppelpunten voor het Edge-modules die u in de voorgaande stappen voor het Edge-share- en Edge lokale share gekopieerd.

        > [!IMPORTANT]
        > De paden die hier worden gebruikt zijn gekoppeld aan uw container, zodat ze moeten overeenkomen met wat de functionaliteit in uw container verwacht. Als u [maakt u een aangepaste module](data-box-edge-create-iot-edge-module.md#update-the-module-with-custom-code), de code die is opgegeven in deze module wordt verwacht dat de gekopieerde paden. Deze paden mag niet worden gewijzigd.
    
        In het vak **Opties Container maken** plakt u het volgende voorbeeld:
    
        ```
        {
            "HostConfig": {
            "Binds": [
            "/home/hcsshares/mydbesmbedgelocalshare1:/home/input",
            "/home/hcsshares/mydbesmbedgeshare1:/home/output"
            ]
            }
        }
        ```

        Geef omgevingsvariabelen die voor de module wordt gebruikt. Omgevingsvariabelen Geef optioneel informatie die helpen bij het definiëren van de omgeving waarin de module wordt uitgevoerd.

        ![Het vak Opties Container maken](./media/data-box-edge-deploy-configure-compute-advanced/add-module-5.png) 
 
    4. Configureer indien nodig de geavanceerde runtime-instellingen voor Edge en klik dan op **Volgende**.

        ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute-advanced/add-module-6.png)
 
5.  Onder **Routes opgeven** kunt u routes tussen modules instellen.  
    
    ![De Routes opgeven](./media/data-box-edge-deploy-configure-compute-advanced/add-module-7.png)

    U kunt vervangen *route* met de volgende route-tekenreeks die u eerder hebt gekopieerd. In dit voorbeeld geeft u de naam op van de lokale share die gegevens naar de cloudshare pusht. Vervang de `modulename` met de naam van de module. Selecteer **Next**.
        
    ```
    "route": "FROM /* WHERE topic = 'mydbesmbedgelocalshare1' INTO BrokeredEndpoint(\"/modules/filemove/inputs/input1\")"
    ```

    ![Het gedeelte Routes opgeven](./media/data-box-edge-deploy-configure-compute-advanced/add-module-8.png)

6.  Controleer alle instellingen onder **Implementatie controleren**. Klik vervolgens op **Verzenden** om de module te verzenden voor implementatie.

    ![De pagina Modules instellen](./media/data-box-edge-deploy-configure-compute-advanced/add-module-9.png)
 
    Deze actie wordt de implementatie van beleidsmodule gestart. Nadat de implementatie voltooid is, de **runtimestatus** van module is **met**.

    ![Aangepaste module toevoegen](./media/data-box-edge-deploy-configure-compute-advanced/add-module-10.png)

## <a name="verify-data-transform-transfer"></a>Controleer of de gegevens transformeren, overdragen

De laatste stap is controleren of de module is verbonden en wordt uitgevoerd zoals verwacht. De runtimestatus van de module moet 'Wordt uitgevoerd' zijn voor uw IoT Edge-apparaat in de IoT Hub-resource.

De volgende stappen om te controleren of gegevens transformeren en overbrengen naar Azure.
 
1.  In Windows Verkenner, verbinding maken met de lokale Edge en de Edge-shares die u eerder hebt gemaakt.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-2.png)
 
1.  Voeg gegevens toe aan de lokale share.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-3.png)
 
    De gegevens worden verplaatst naar de cloudshare.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-4.png)  

    De gegevens worden vervolgens vanuit de cloudshare naar het opslagaccount gepusht. Als u wilt de gegevens bekijken, gaat u naar uw storage-account en selecteer vervolgens **Opslagverkenner**. U kunt de geüploade gegevens weergeven in uw storage-account.

    ![Gegevenstransformatie controleren](./media/data-box-edge-deploy-configure-compute-advanced/verify-data-5.png)
 
U hebt het validatieproces voltooid.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Rekenproces configureren
> * Shares toevoegen
> * Een trigger toevoegen
> * Een rekenprocesmodule toevoegen
> * Gegevenstransformatie controleren en gegevens overdragen

Als u wilt weten hoe u uw Data Box Edge-apparaat beheert, gaat u naar:

> [!div class="nextstepaction"]
> [Lokale webgebruikersinterface gebruiken om Data Box Edge te beheren](data-box-edge-manage-access-power-connectivity-mode.md)
