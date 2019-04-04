---
title: Micro Focus Enterprise Server 4.0 worden uitgevoerd in een Docker-Container op Azure Virtual Machines
description: Rehost uw IBM z/OS mainframe-werkbelastingen door uit te voeren op Azure Virtual Machines Micro Focus Enterprise Server in een Docker-container.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: sread
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 30d99c3f4767eb50361f7074c0d508fcf309faca
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896432"
---
# <a name="run-micro-focus-enterprise-server-40-in-a-docker-container-on-azure"></a>Micro Focus Enterprise Server 4.0 worden uitgevoerd in een Docker-Container in Azure

U kunt Micro Focus Enterprise Server 4.0 in een Docker-container uitvoeren op Azure. In deze zelfstudie leert u hoe. De demonstratie van de acctdemo Windows CICS (systeem voor klantinformatie klant) wordt gebruikt voor Enterprise-Server.

Docker voegt draagbaarheid en isolatie voor toepassingen. U kunt bijvoorbeeld een Docker-installatiekopie exporteren vanuit een Windows VM uitvoeren op een andere, of vanuit een opslagplaats naar een Windows-server met Docker. De Docker-installatiekopie wordt uitgevoerd in de nieuwe locatie met dezelfde configuratie: zonder Enterprise-Server te installeren. Het deel van de afbeelding. Licenties nog steeds van toepassing.

In deze zelfstudie installeert de **Windows 2016 Datacenter met Containers** virtuele machine (VM) van de Azure Marketplace. Deze VM bevat **Docker 18.09.0**. De volgende stappen laten zien hoe de container te implementeren, uitvoeren en vervolgens verbinding maken met een 3270 emulator.

## <a name="prerequisites"></a>Vereisten

Voordat u aan de slag, bekijkt u deze vereisten voldoet:

- Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

- De Focus Micro-software en een geldige licentie of proeflicentie. Als u een bestaande Micro Focus-klant bent, moet u contact op met de vertegenwoordiger van uw Micro Focus. Anders [vraag een proefversie](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/).

     > [!NOTE]
     > De Docker-demonstratie-bestanden zijn opgenomen in Enterprise Server 4.0. Deze zelfstudie wordt gebruikgemaakt van ent\_server\_docker-bestanden\_4.0\_windows.zip. Openen vanaf dezelfde plaats dat u toegang het Enterprise-Server-installatiebestand tot of Ga naar *Micro Focus* aan de slag.

- De documentatie voor [Enterprise Server en Enterprise Developer](https://www.microfocus.com/documentation/enterprise-developer/#").

## <a name="create-a-vm"></a>Een virtuele machine maken

1. De media beveiligen vanaf de ent\_server\_docker-bestanden\_4.0\_windows.zip-bestand. ES-Docker-Prod-XXXXXXXX.mflic het bestand met de (vereist voor het bouwen van de Docker-installatiekopieën) beveiligen.

2. De virtuele machine maken. Open Azure portal, selecteert u doet dit door **een resource maken** uit de linksboven en filteren op *WindowsServer*. Selecteer in de resultaten **Windows Server 2016 Datacenter – met Containers**.

     ![Zoekresultaten met Azure portal](media/01-portal.png)

3. Als u wilt de eigenschappen voor de virtuele machine configureert, kiest u exemplaardetails:

    1. Kies een VM-grootte. Voor deze zelfstudie, kunt u overwegen een **Standard DS2\_v2** VM met 2 vcpu's en 7 GB geheugen.

    2. Selecteer de **regio** en **resourcegroep** u wilt implementeren.

    3. Voor **Beschikbaarheidsopties**, gebruik de standaardinstelling.

    4. Voor **gebruikersnaam**, typt u het administrator-account dat u wilt gebruiken en het wachtwoord.

    5. Zorg ervoor dat **poort 3389 RDP** is geopend. Alleen deze poort moet openbaar beschikbaar wordt gemaakt, zodat u zich bij de virtuele machine aanmelden kunt. Vervolgens Accepteer alle standaardwaarden en klik op **revisie + maken**.

     ![Een deelvenster van de virtuele machine maken](media/container-02.png)

4. Wacht tot de implementatie is voltooid (een aantal minuten). Er wordt aangegeven dat uw virtuele machine is gemaakt.

5. Klik op **naar de Resource gaan** naar de **overzicht** blade voor uw virtuele machine.

6. Aan de rechterkant, klikt u op de **Connect** knop. De **verbinding maken met virtuele machine** opties worden weergegeven aan de rechterkant.

7. Klik op de **RDP-bestand downloaden** knop voor het downloaden van het RDP-bestand waarmee u koppelen aan de virtuele machine.

8. Nadat het bestand is gedownload, open het en typ de gebruikersnaam en het wachtwoord dat u hebt gemaakt voor de virtuele machine.

     > [!NOTE]
     > Gebruik niet uw zakelijke referenties aan te melden. (De RDP-client wordt ervan uitgegaan dat u kunt het gebruik van deze. U dit niet doet.)

9.  Selecteer **meer keuze**, selecteer vervolgens de referenties van uw virtuele machine.

Op dit moment is de virtuele machine actief is en gekoppelde via RDP. U bent aangemeld en klaar voor de volgende stap.

## <a name="create-a-sandbox-directory-and-upload-the-zip-file"></a>Maak een sandbox-map en upload het zipbestand

1.  Maak een map op de virtuele machine waar u de licentie voor demo's en bestanden kunt uploaden. Bijvoorbeeld, **C:\\Sandbox**.

2.  Uploaden **ent\_server\_docker-bestanden\_4.0\_windows.zip** en de **ES-Docker-Prod-XXXXXXXX.mflic** bestand naar de map die u hebt gemaakt.

3.  Pak de inhoud van het zip-bestand naar de **ent\_server\_docker-bestanden\_4.0\_windows** directory die zijn gemaakt door het uitpakproces. Deze map bevat een Leesmij-bestand (zoals HTML- en txt-bestand) en twee submappen **EnterpriseServer** en **voorbeelden**.

4.  Kopie **ES-Docker-Prod-XXXXXXXX.mflic** naar station C:\\Sandbox\\ent\_server\_docker-bestanden\_4.0\_windows\\ EnterpriseServer en C:\\Sandbox\\ent\_server\_docker-bestanden\_4.0\_windows\\voorbeelden\\CICS mappen.

> [!NOTE]
> Zorg ervoor dat u het bestand met de kopiëren naar beide directory's. Ze zijn vereist voor de Docker-build-stap om ervoor te zorgen dat de installatiekopieën van het juiste licenties worden verleend.

## <a name="check-docker-version-and-create-base-image"></a>Docker-versie controleren en basisinstallatiekopie maken

> [!IMPORTANT]
> Het maken van de juiste Docker-installatiekopie is een proces in twee stappen. Maak eerst de basisinstallatiekopie van het Enterprise Server 4.0. Maak vervolgens een andere afbeelding voor de x64 platform. Hoewel u een x86 (32-bits) kunt maken met installatiekopieën, gebruikt u de 64-bits installatiekopie.

1. Open een opdrachtprompt.

2. Controleer of Docker is geïnstalleerd. Typ in de opdrachtprompt:

    ```
        docker version
    ```

     De versie is bijvoorbeeld 18.09.0 wanneer dit is geschreven.

3. Wijzig de directory te typen **cd \Sandbox\ent_server_dockerfiles_4.0_windows\EnterpriseServer**.

4. Type **bld.bat IacceptEULA** om te beginnen met het bouwproces voor de eerste basisinstallatiekopie. Wacht een paar minuten voor dit proces om uit te voeren. In de resultaten u ziet de twee installatiekopieën die zijn gemaakt: één voor x64 en één voor x86:

     ![Opdrachtvenster afbeeldingen weergeven](media/container-04.png)

5. Voor het maken van de uiteindelijke installatiekopie voor de demonstratie CICS, schakel over naar de map CICS door te typen **cd\\Sandbox\\ent\_server\_docker-bestanden\_4.0\_windows\\ Voorbeelden\\CICS**.

6. Typ voor het maken van de installatiekopie van het **bld.bat x64**. Wacht een paar minuten voor het proces om uit te voeren en het bericht weergegeven dat de installatiekopie is gemaakt.

7. Type **docker-installatiekopieën** om een lijst met alle van de Docker-installatiekopieën die is geïnstalleerd op de virtuele machine weer te geven. Zorg ervoor dat **microfocus/es-acctdemo** is een van beide.

     ![Opdrachtvenster es acctdemo afbeelding weergeven](media/container-05.png)

## <a name="run-the-image"></a>De installatiekopie uitvoeren 

1.  Starten van Enterprise Server 4.0 en de toepassing acctdemo, klikt u op de opdrachtprompt te typen:

    ```
         docker run -p 16002:86/tcp -p 16002:86/udp -p 9040-9050:9040-9050 -p 9000-9010:9000-9010 -ti --network="nat" --rm microfocus/es-acctdemo:win_4.0_x64
    ```

2.  Installeren van een terminal emulator 3270 zoals [x3270](http://x3270.bgp.nu/) en deze gebruiken om te koppelen, via poort 9040, met de installatiekopie die wordt uitgevoerd.

3.  Haal het IP-adres van de container acctdemo dus Docker kan fungeren als een DHCP-server voor de containers die ermee wordt beheerd:

    1.  Haal de ID van de container die wordt uitgevoerd. Type **Docker ps** achter de opdrachtprompt en de-id (**22a0fe3159d0** in dit voorbeeld). Bewaar het voor de volgende stap.

    2.  Voor het IP-adres voor de container acctdemo, gebruikt u de container-ID uit de vorige stap als volgt:

    ```
       docker inspect <containerID> --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```

       Bijvoorbeeld:

    ```   
        docker inspect 22a0fe3159d0 --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```
4. Houd er rekening mee de IP-adres voor de installatiekopie van het acctdemo. Bijvoorbeeld, is het adres in de volgende uitvoer 172.19.202.52.

     ![Opdrachtvenster met IP-adres](media/container-06.png)

5. Koppel de installatiekopie met behulp van de emulator. De emulator voor het gebruik van het adres van de acctdemo afbeelding en de poort 9040 configureren. Er hier **172.19.202.52:9040**. Uw zijn vergelijkbaar. De **aanmelden op CICS** scherm wordt geopend.

    ![Aanmelden CICS scherm](media/container-07.png)

6. Aanmelden bij de regio CICS door in te voeren **SYSAD** voor de **USERID** en **SYSAD** voor de **wachtwoord**.

7. Schakel het scherm, met behulp van de emulator keymap. Voor x3270, selecteert u de **Keymap** menu-optie.

8. Als u wilt de toepassing acctdemo starten, typt u **ACCT**. Het eerste scherm voor de toepassing wordt weergegeven.

     ![Scherm voor account-Demo](media/container-08.png)

9. Experimenteer met weergave-accounttypen. Typ bijvoorbeeld **D** voor de aanvraag en **11111** voor de **ACCOUNT**. Andere getallen account om te proberen zijn 22222, 33333, enzovoort.

     ![Scherm voor account-Demo](media/container-09.png)

10. Als u de console Beheer van Enterprise-Server, gaat u naar de opdrachtprompt en typ **http:172.19.202.52:86 starten**

     ![Enterprise Server-beheerconsole](media/container-010.png)

Dat is alles. U bent nu uitvoeren en beheren van een toepassing CICS in een Docker-container.

## <a name="next-steps"></a>Volgende stappen

- [Micro Focus Enterprise Server 4.0 en Enterprise Developer 4.0 installeren op Azure](./set-up-micro-focus-azure.md)
- [Mainframe-toepassingen migreren](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)
