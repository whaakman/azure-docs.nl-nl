---
title: Een gesimuleerd apparaat X.509 met Azure IoT Hub met Java en de inschrijving groepen inrichten | Microsoft Docs
description: Zelfstudie voor Azure - maken en inrichten van een gesimuleerde X.509-apparaat met Java apparaat en de service SDK en de inschrijving van groepen voor IoT Hub apparaat inrichten van Service
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 01/04/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: 14e5e7613fd5df650625cf8997d569b754ceb689
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="create-and-provision-a-simulated-x509-device-using-java-device-and-service-sdk-and-group-enrollments-for-iot-hub-device-provisioning-service"></a>Maken en inrichten van een gesimuleerde X.509-apparaat met behulp van Java-apparaat en de SDK-service en de groep inschrijvingen voor IoT Hub apparaat inrichten van Service

Deze stappen laten zien hoe u een X.509-apparaat op uw ontwikkelcomputer Windows-besturingssysteem uitgevoerd te simuleren en een voorbeeld van code gebruiken deze gesimuleerde apparaten verbinding met de Service voor het inrichten van apparaten en uw IoT-hub met groepen van de inschrijving. 

Voltooi de stappen in [Set up the IoT Hub Device Provisioning Service with the Azure portal](./quick-setup-auto-provision.md) (IoT Hub Device Provisioning Service instellen met Azure Portal) voordat u verdergaat.


## <a name="prepare-the-environment"></a>De omgeving voorbereiden 

1. Zorg ervoor dat [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) is geïnstalleerd op de computer.

1. Download en installeer [Maven](https://maven.apache.org/install.html).

1. Zorg ervoor dat `git` op de computer wordt geïnstalleerd en toegevoegd aan de omgevingsvariabelen die voor het opdrachtvenster toegankelijk zijn. Zie [Software Freedom Conservancy's Git client tools](https://git-scm.com/download/) (Git-clienthulpprogramma's van Software Freedom Conservancy) om de nieuwste versie van `git`-hulpprogramma's te installeren, waaronder **Git Bash**, de opdrachtregel-app die u kunt gebruiken voor interactie met de lokale Git-opslagplaats. 

1. Gebruik de volgende [certificaat overzicht](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) om uw testcertificaten te maken. Zie voor een meer diepgaande blik op het maken van certificaten, [PowerShell-scripts voor het beheren van CA ondertekend X.509-certificaten](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-security-x509-create-certificates).

    > [!NOTE]
    > Deze stap moet u [OpenSSL](https://www.openssl.org/), die kunnen ofwel worden gebouwd en geïnstalleerd vanuit de bron of gedownload en geïnstalleerd vanaf een [3e partij](https://wiki.openssl.org/index.php/Binaries) zoals [dit](https://sourceforge.net/projects/openssl/). Als u al hebt gemaakt uw _hoofdmap_, _tussenliggende_ en _apparaat_ certificaten kunt u deze stap overslaan.
    >

1. Maak de informatie over de inschrijving:

    1. Doorlopen **stap 1** en **stap 2** maken uw _hoofdmap_ en _tussenliggende_ certificaten.

    1. Meld u aan bij Azure Portal, klik in het linkermenu op de knop **All resources** en open uw Provisioning-service.

        1. Selecteer **Certificaten** in de overzichtsblade Device Provisioning Service en klik op de knop **Toevoegen** bovenaan.

        1. Voer bij **Certificaat toevoegen** de volgende gegevens in:
            - Voer een unieke certificaatnaam in.
            - Selecteer de  **_RootCA.pem_**  bestand dat u zojuist hebt gemaakt.
            - Klik op de knop **Save** als u klaar bent.

        ![Certificaat toevoegen](./media/tutorial-group-enrollments/add-certificate.png)

        1. Selecteer het zojuist gemaakte netwerk:
            - Klik op **Verificatiecode genereren**. Kopieer de gegenereerde code.
            - Doorlopen **stap 3**. Voer de _verificatiecode_ of klik met de rechtermuisknop te plakken in uw actieve PowerShell-venster.  Druk op **Enter**.
            - Selecteer de zojuist gemaakte  **_verifyCert4.pem_**  bestand in de Azure-portal. Klik op **controleren**.

            ![Certificaat valideren](./media/tutorial-group-enrollments/validate-certificate.png)

1. Voltooien door het uitvoeren van **stap 4** en **stap 5** om uw certificaten voor apparaten en resources opschonen te maken.

> [!NOTE]
> Bij het maken van certificaten voor apparaten worden gebruik alleen kleine letters en cijfers en afbreekstreepjes in de apparaatnaam van uw.
>


## <a name="create-a-device-enrollment-entry"></a>Een vermelding voor apparaatinschrijving maken

1. Open een opdrachtprompt. Kloon de GitHub-opslagplaats voor Java SDK-codevoorbeelden:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. Navigeer in de gedownloade broncode naar de voorbeeldmap **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**. Open het bestand **_/src/main/java/samples/com/microsoft/azure/sdk/iot/ServiceEnrollmentGroupSample.java_** in een editor naar keuze, en voeg de volgende gegevens toe:

    1. Voeg de `[Provisioning Connection String]` voor de inrichtingsservice als volgt toe vanuit de portal:

        1. Navigeer naar de inrichtingsservice in [Azure Portal](https://portal.azure.com). 

        1. Open **Gedeeld toegangsbeleid** en selecteer een beleid met de machtiging *EnrollmentWrite*.
    
        1. Kopieer de **Verbindingsreeks van de primaire sleutel**. 

            ![De verbindingsreeks voor de inrichting ophalen uit de portal](./media/tutorial-group-enrollments/provisioning-string.png)  

        1. Vervang `[Provisioning Connection String]` in het voorbeeldcodebestand **_ServiceEnrollmentGroupSample.java_** door de **Verbindingsreeks van de primaire sleutel**.

            ```java
            private static final String PROVISIONING_CONNECTION_STRING = "[Provisioning Connection String]";
            ```

    1. Open de  **_RootCA.pem_**  bestand in een teksteditor. Wijs de waarde van het **Basiscertificaat** toe aan de parameter **PUBLIC_KEY_CERTIFICATE_STRING** zoals hieronder wordt weergegeven:

        ```java
        private static final String PUBLIC_KEY_CERTIFICATE_STRING =
                "-----BEGIN CERTIFICATE-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "-----END CERTIFICATE-----\n";
        ```
 
    1. Navigeer naar de IoT-hub die is gekoppeld aan de inrichtingsservice in [Azure Portal](https://portal.azure.com). Open het tabblad **Overzicht** voor de hub en kopieer de **Hostnaam**. Wijs deze **Hostnaam** toe aan de parameter *IOTHUB_HOST_NAME*.

        ```java
        private static final String IOTHUB_HOST_NAME = "[Host name].azure-devices.net";
        ```

    1. Bestudeer de voorbeeldcode. Hiermee wordt een groepsinschrijving voor X.509-apparaten gemaakt, bijgewerkt, en verwijderd, en wordt voor deze inschrijving een query uitgevoerd. Markeer de volgende coderegels aan het einde van het bestand _ServiceEnrollmentGroupSample.java_ tijdelijk als commentaar voor een geslaagde inschrijving via de portal:

        ```java
        // ************************************** Delete info of enrollmentGroup ***************************************
        System.out.println("\nDelete the enrollmentGroup...");
        provisioningServiceClient.deleteEnrollmentGroup(enrollmentGroupId);
        ```

    1. Sla het bestand _ServiceEnrollmentGroupSample.java_ op. 

1. Open een opdrachtvenster en navigeer naar de map **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**.

1. Bouw de voorbeeldcode met behulp van deze opdracht:

    ```cmd\sh
    mvn install -DskipTests
    ```

1. Voer het voorbeeld uit met behulp van deze opdrachten in het opdrachtvenster:

    ```cmd\sh
    cd target
    java -jar ./service-enrollment-group-sample-{version}-with-deps.jar
    ```

1. Houd het uitvoervenster in de gaten voor een geslaagde inschrijving.

    ![geslaagde inschrijving](./media/tutorial-group-enrollments/enrollment.png) 

1. Navigeer naar de inrichtingsservice in Azure Portal. Klik op **Inschrijvingen beheren**. U ziet dat de groep X.509-apparaten wordt weergegeven op het tabblad **Inschrijvingsgroepen** met een automatisch gegenereerde *GROEPSNAAM*. 


## <a name="simulate-the-device"></a>Het apparaat simuleren

1. Selecteer **Overzicht** in de overzichtsblade Device Provisioning Service en noteer de waarden voor _Id-bereik_ en _Globaal eindpunt voor inrichtingsservice_.

    ![Service-informatie](./media/tutorial-group-enrollments/extract-dps-endpoints.png)

1. Open een opdrachtprompt. Navigeer naar de voorbeeldprojectmap.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-X509-sample
    ```

1. Voer de informatie over de inschrijving in de volgende manier:

    - Bewerk `/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningX509Sample.java` om de eerder genoteerde waarden voor _Id-bereik_ en _Globaal eindpunt voor inrichtingsservice_ toe te voegen. Open uw  **_{deviceName}-public.pem_**  bestands- en deze waarde als uw _Client Cert_. Open uw  **_{deviceName}-all.pem_**  -bestand en kopieer de tekst van _---BEGIN persoonlijke sleutel---_ naar _---einde persoonlijke sleutel---_.  Gebruik deze als uw _Client Cert persoonlijke sleutel_.

        ```java
        private static final String idScope = "[Your ID scope here]";
        private static final String globalEndpoint = "[Your Provisioning Service Global Endpoint here]";
        private static final ProvisioningDeviceClientTransportProtocol PROVISIONING_DEVICE_CLIENT_TRANSPORT_PROTOCOL = ProvisioningDeviceClientTransportProtocol.HTTPS;
        private static final String leafPublicPem = "<Your Public PEM Certificate here>";
        private static final String leafPrivateKey = "<Your Private PEM Key here>";
        ```

        - Gebruik de volgende notatie om uw certificaat en sleutel op te nemen:
            
            ```java
            private static final String leafPublicPem = "-----BEGIN CERTIFICATE-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "-----END CERTIFICATE-----\n";
            private static final String leafPrivateKey = "-----BEGIN PRIVATE KEY-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXX\n" +
                "-----END PRIVATE KEY-----\n";
            ```

1. Bouw het voorbeeld. Navigeer naar de doelmap en voer het gemaakte JAR-bestand uit.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-x509-sample-{version}-with-deps.jar
    ```

    ![Succesvolle registratie](./media/tutorial-group-enrollments/registration.png)

1. Navigeer in de portal naar de IoT-hub die is gekoppeld aan uw Provisioning-service en open de blade **Device Explorer**. Wanneer het inrichten van het gesimuleerde X.509-apparaat voor de hub is geslaagd, wordt de apparaat-ID weergegeven op de blade **Device Explorer** met de *STATUS* **ingeschakeld**. Let op: u moet mogelijk klikken op de knop **Vernieuwen** bovenaan als u de blade vóór het uitvoeren van de voorbeeldapparaattoepassing al hebt geopend. 

    ![Apparaat wordt geregistreerd voor de IoT-hub](./media/tutorial-group-enrollments/hub-registration.png) 


## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt blijven doorwerken met het voorbeeld van de apparaatclient en deze beter wilt leren kennen, wis de resources die in deze Snelstartgids zijn gemaakt dan niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources die via deze Snelstartgids zijn gemaakt, te verwijderen.

1. Sluit het uitvoervenster van het voorbeeld van de apparaatclient op de computer.
1. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer uw Device Provisioning Service. Open de blade **Inschrijvingen beheren** voor uw service en klik vervolgens op het tabblad **Afzonderlijke inschrijvingen**. Selecteer de *registratie-id* van het apparaat dat u hebt ingeschreven met behulp van deze quickstart. Klik vervolgens bovenaan op de knop **Verwijderen**. 
1. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer vervolgens uw IoT-hub. Open de blade **IoT-apparaten** voor uw hub, selecteer de *apparaat-id* van het apparaat dat u hebt geregistreerd in deze quickstart en klik vervolgens bovenaan op de knop **Verwijderen**.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een gesimuleerde X.509-apparaat op uw Windows-computer gemaakt en deze naar uw iothub met behulp van de Azure IoT Hub apparaat inrichtingsservice en de inschrijving groepen ingericht. Voor meer informatie over het X.509-apparaat, doorgaan met het apparaat concepten. 

> [!div class="nextstepaction"]
> [IoT Hub apparaat-inrichtingsservice apparaat concepten](concepts-device.md)
