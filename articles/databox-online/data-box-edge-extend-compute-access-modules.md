---
title: Compute-netwerk in Azure Data Box Edge toegang modules beheren | Microsoft Docs
description: Beschrijft hoe u de compute-netwerk op de rand van uw gegevens in voor toegang tot modules via een extern IP-adres uitbreiden.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/17/2019
ms.author: alkohli
ms.openlocfilehash: 907647725dd6795b3b6482476de7442fbbf66114
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65917233"
---
# <a name="enable-compute-network-on-your-azure-data-box-edge"></a>Compute-netwerk voor uw Azure Data Box-edge-apparaten inschakelen

Dit artikel wordt beschreven hoe de modules die worden uitgevoerd op uw Azure Data Box-Edge toegang krijgen tot de compute-netwerk dat is ingeschakeld op het apparaat.

Voor het configureren van het netwerk, moet u de volgende stappen uitvoeren:

- Inschakelen van een netwerkinterface op uw gegevens in het Edge-apparaat voor compute
- Een module toevoegen aan toegang tot compute-netwerk voor uw gegevens in het edge-apparaten
- Controleer of dat de module kan toegang krijgen tot de netwerkinterface ingeschakeld

In deze zelfstudie maakt u een abonneedatabase app-module gebruikt ter illustratie van het scenario.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u op:

- Een Data Box Edge-apparaat met de Apparaatinstallatie van is voltooid.
- U hebt voltooid, **berekening configureren** stap volgens de [zelfstudie: Gegevens transformeren met Azure Data Box Edge](data-box-edge-deploy-configure-compute-advanced.md#configure-compute) op uw apparaat. Uw apparaat moet een gekoppelde IoT-Hub-resource, een IoT-apparaat en een IoT Edge-apparaat hebben.

## <a name="enable-network-interface-for-compute"></a>Inschakelen van de netwerkinterface voor compute

Voor toegang tot de modules die worden uitgevoerd op uw apparaat via een extern netwerk, moet u een IP-adres toewijzen aan een netwerkinterface op uw apparaat. U kunt deze beheren compute-instellingen van de lokale webgebruikersinterface.

De volgende stappen op de lokale web-UI compute-instellingen configureren.

1. Ga in de lokale webgebruikersinterface naar **Configuration > Compute instellingen**.  

2. **Schakel** de netwerkinterface die u gebruiken wilt voor verbinding met een compute-module die u op het apparaat uitvoert.

    - Als u statische IP-adressen gebruikt, voert u een IP-adres voor de netwerkinterface.
    - Als u DHCP gebruikt, worden de IP-adressen automatisch toegewezen. In dit voorbeeld maakt gebruik van DHCP.

    ![Schakel de compute-instellingen 1](media/data-box-edge-extend-compute-access-modules/enable-compute-setting-1.png)

3. Selecteer **toepassen** instellingen te kunnen toepassen. Noteer het IP-adres toegewezen aan de netwerkinterface als DHCP.

    ![Schakel de compute-instellingen](media/data-box-edge-extend-compute-access-modules/enable-compute-setting-2.png)

## <a name="add-webserver-app-module"></a>Abonneedatabase app module toevoegen

Voer de volgende stappen uit om toe te voegen een abonneedatabase app-module op uw gegevens in het Edge-apparaat.

1. Ga naar de IoT Hub-resource die is gekoppeld aan uw gegevens in het Edge-apparaat en selecteer vervolgens **IoT Edge-apparaat**.
2. Selecteer het IoT Edge-apparaat dat is gekoppeld aan uw gegevens in het Edge-apparaat. Op de **Apparaatdetails**, selecteer **modules instellen**. Op **modules toevoegen**, selecteer **+ toevoegen** en selecteer vervolgens **IoT Edge-Module**.
3. In de **aangepaste IoT Edge-modules** blade:

    1. Geef een **naam** voor uw webserver-app-module die u wilt implementeren.
    2. Geef een **URI installatiekopie** voor de installatiekopie van de module. Een module die overeenkomt met de opgegeven naam en de labels worden opgehaald. In dit geval `nginx:stable` een stabiele nginx-installatiekopie (met tags als stabiel) wordt opgehaald uit het publiek [Docker-opslagplaats](https://hub.docker.com/_/nginx/).
    3. In de **Container maken opties**, plak de volgende voorbeeldcode:  

        ```
        {
            "HostConfig": {
                "PortBindings": {
                    "80/tcp": [
                        {
                            "HostPort": "8080"
                        }
                    ]
                }
            }
        }
        ```

        Deze configuratie kunt u toegang tot de module met behulp van de compute-netwerk-IP-via *http* op TCP-poort 8080 (met de webserver standaardpoort 80 wordt).

        ![Geef informatie over de poort in IoT Edge aangepaste module-blade](media/data-box-edge-extend-compute-access-modules/module-information.png)

    4. Selecteer **Opslaan**.

## <a name="verify-module-access"></a>De module toegang controleren

1. Controleer of de module met succes is geïmplementeerd en wordt uitgevoerd. Op de **Apparaatdetails** pagina de **Modules** tabblad de runtimestatus van de module moet **met**.  
2. Verbinding maken met de web server app-module. Open een browservenster en typ:

    `http://<compute-network-IP-address>:8080`

    U ziet dat de abonneedatabase app wordt uitgevoerd.

    ![Verbinding met de module via de opgegeven poort controleren](media/data-box-edge-extend-compute-access-modules/verify-connect-module-1.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Gebruikers beheren via Azure Portal](data-box-edge-manage-users.md).
