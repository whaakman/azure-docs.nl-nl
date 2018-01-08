---
title: Een gesimuleerd X.509-apparaat inrichten in Azure IoT Hub met behulp van Java | Microsoft Docs
description: 'Azure-quickstart: een gesimuleerd X.509-apparaat maken en inrichten voor IoT Hub Device Provisioning Service met de SDK voor Java'
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 12/21/2017
ms.topic: quickstart
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: 508df6e7c677d87c6b893519c59b02880e16d7ec
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="create-and-provision-a-simulated-x509-device-using-java-device-sdk-for-iot-hub-device-provisioning-service"></a>Een gesimuleerd X.509-apparaat maken en inrichten voor IoT Hub Device Provisioning Service met behulp van de Java apparaat-SDK
> [!div class="op_single_selector"]
> * [C](quick-create-simulated-device-x509.md)
> * [Java](quick-create-simulated-device-x509-java.md)
> * [C#](quick-create-simulated-device-x509-csharp.md)
> * [Python](quick-create-simulated-device-x509-python.md)

In deze stappen wordt getoond hoe u een gesimuleerd X.509-apparaat maakt op een ontwikkelcomputer met Windows OS en het codevoorbeeld gebruikt om dit gesimuleerde apparaat te verbinden met de Device Provisioning Service en uw IoT-hub. 

Voltooi de stappen in [Set up the IoT Hub Device Provisioning Service with the Azure portal](./quick-setup-auto-provision.md) (IoT Hub Device Provisioning Service instellen met Azure Portal) voordat u verdergaat.


## <a name="prepare-the-environment"></a>De omgeving voorbereiden 

1. Zorg ervoor dat [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) is geïnstalleerd op de computer.

1. Download en installeer [Maven](https://maven.apache.org/install.html).

1. Zorg ervoor dat `git` op de computer wordt geïnstalleerd en toegevoegd aan de omgevingsvariabelen die voor het opdrachtvenster toegankelijk zijn. Zie [Software Freedom Conservancy's Git client tools](https://git-scm.com/download/) (Git-clienthulpprogramma's van Software Freedom Conservancy) om de nieuwste versie van `git`-hulpprogramma's te installeren, waaronder **Git Bash**, de opdrachtregel-app die u kunt gebruiken voor interactie met de lokale Git-opslagplaats. 

1. Open een opdrachtprompt. Kloon het codevoorbeeld voor de GitHub-opslagplaats voor apparaatsimulatie:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. Navigeer naar het certificaatgeneratorproject en bouw het project. 

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator
    mvn clean install
    ```

1. Navigeer naar de doelmap en voer het gemaakte JAR-bestand uit.

    ```cmd/sh
    cd target
    java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
    ```

1. Maak de inschrijvingsgegevens op een van de volgende manieren, afhankelijk van de installatie:

    - **Afzonderlijke inschrijving**:

        1. Voer **N** in bij _Wilt u een algemene naam invoeren?_ Kopieer de uitvoer van `Client Cert` vanaf *-----BEGIN CERTIFICATE-----* tot *-----END CERTIFICATE-----* naar het klembord.

            ![Generator voor afzonderlijke certificaten](./media/java-quick-create-simulated-device-x509/individual.png)

        1. Maak een bestand met de naam **_X509individual.pem_** op de Windows-computer, open het in een editor naar keuze en kopieer de inhoud van het klembord naar dit bestand. Sla het bestand op.

        1. Voer **N** in bij _Wilt u de verificatiecode invoeren?_ en houd de programma-uitvoer geopend zodat u deze later in deze quickstart kunt raadplegen. Noteer de waarden voor _Clientcertificaat_ en _Persoonlijke sleutel van clientcertificaat_.
    
    - **Inschrijvingsgroepen**:

        1. Voer **N** in bij _Wilt u een algemene naam invoeren?_ Kopieer de uitvoer van `Root Cert` vanaf *-----BEGIN CERTIFICATE-----* tot *-----END CERTIFICATE-----* naar het klembord.

            ![Generator voor groepscertificaten](./media/java-quick-create-simulated-device-x509/group.png)

        1. Maak een bestand met de naam **_X509group.pem_** op de Windows-computer, open het in een editor naar keuze en kopieer de inhoud van het klembord naar dit bestand. Sla het bestand op.

        1. Voer **J** in bij _Wilt u de verificatiecode invoeren?_ en houd het programma geopend zodat u het later in deze quickstart kunt raadplegen. Noteer de waarden voor _Clientcertificaat_, _Persoonlijke sleutel van clientcertificaat_, _Certificaatondertekening_ en _Basiscertificaat_.


## <a name="create-a-device-enrollment-entry"></a>Een vermelding voor apparaatinschrijving maken

1. Meld u aan bij Azure Portal, klik in het linkermenu op de knop **All resources** en open uw Provisioning-service.

1. Voer de inschrijvingsgegevens op een van de volgende manieren in, afhankelijk van de installatie:

    - **Afzonderlijke inschrijving**: 

        1. Selecteer **Manage enrollments** in de overzichtsblade van Device Provisioning Service. Selecteer het tabblad **Individual Enrollments** en klik bovenaan op de knop **Add**. 

        1. Voer onder bij **Registratielijstitem toevoegen** de volgende gegevens in:
            - Selecteer **X.509** als *mechanisme* voor identiteitscontrole.
            - Selecteer onder *PEM- of CER-bestand voor certificaat* het certificaatbestand **_X509individual.pem_** dat in de vorige stappen is gemaakt met behulp van de widget *Bestandenverkenner*.
            - Desgewenst kunt u de volgende informatie verstrekken:
                - Selecteer een IoT-hub die is gekoppeld aan uw inrichtingsservice.
                - Voer een unieke apparaat-id in. Vermijd gevoelige gegevens bij het benoemen van uw apparaat. 
                - Werk de **initiële status van de apparaatdubbel** bij met de gewenste beginconfiguratie voor het apparaat.
            - Klik op de knop **Save** als u klaar bent. 

        ![Gegevens van X.509-apparaatinschrijving invoeren in de portalblade](./media/java-quick-create-simulated-device-x509/enter-device-enrollment.png)  

       Als het apparaat is ingeschreven, wordt het X.509-apparaat weergegeven als **microsoftriotcore** onder de kolom *Registratie-id* op het tabblad *Afzonderlijke registraties*. 

    - **Inschrijvingsgroepen**: 

        1. Selecteer **Certificaten** in de overzichtsblade Device Provisioning Service en klik op de knop **Toevoegen** bovenaan.

        1. Voer bij **Certificaat toevoegen** de volgende gegevens in:
            - Voer een unieke certificaatnaam in.
            - Selecteer het bestand **_X509group.pem_** dat u eerder hebt gemaakt.
            - Klik op de knop **Save** als u klaar bent.

        ![Certificaat toevoegen](./media/java-quick-create-simulated-device-x509/add-certificate.png)

        1. Selecteer het zojuist gemaakte netwerk:
            - Klik op **Verificatiecode genereren**. Kopieer de gegenereerde code.
            - Voer de _verificatiecode_ in of klik met de rechtermuisknop om te plakken in het actieve venster _provisioning-x509-cert-generator_.  Druk op **Enter**.
            - Kopieer de uitvoer van `Verification Cert` vanaf *-----BEGIN CERTIFICATE-----* tot *-----END CERTIFICATE-----* naar het klembord.
            
                ![Generator voor validaties](./media/java-quick-create-simulated-device-x509/validation-generator.png)

            - Maak een bestand met de naam **_X509validation.pem_** op de Windows-computer, open het in een editor naar keuze en kopieer de inhoud van het klembord naar dit bestand. Sla het bestand op.
            - Selecteer het bestand **_X509validation.pem_** in Azure Portal. Klik op **Verifiëren**.

            ![Certificaat valideren](./media/java-quick-create-simulated-device-x509/validate-certificate.png)

        1. Selecteer **Inschrijvingen beheren**. Selecteer het tabblad **Groepsinschrijvingen** en klik bovenaan op de knop **Toevoegen**.
            - Voer een unieke groepsnaam in.
            - Selecteer de uniek certificaatnaam die u eerder hebt gemaakt
            - Desgewenst kunt u de volgende informatie verstrekken:
                - Selecteer een IoT-hub die is gekoppeld aan uw inrichtingsservice.
                - Werk de **initiële status van de apparaatdubbel** bij met de gewenste beginconfiguratie voor het apparaat.

        ![Gegevens van X.509-groepsinschrijving invoeren in de portalblade](./media/java-quick-create-simulated-device-x509/enter-group-enrollment.png)

        Nadat de inschrijving is voltooid, wordt de X.509-apparaatgroep weergegeven in de kolom *Groepsnaam* op het tabblad *Inschrijvingsgroepen*.


## <a name="simulate-the-device"></a>Het apparaat simuleren

1. Selecteer **Overzicht** in de overzichtsblade Device Provisioning Service en noteer de waarden voor _Id-bereik_ en _Globaal eindpunt voor inrichtingsservice_.

    ![Service-informatie](./media/java-quick-create-simulated-device-x509/extract-dps-endpoints.png)

1. Open een opdrachtprompt. Navigeer naar de voorbeeldprojectmap.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-X509-sample
    ```

1. Voer de inschrijvingsgegevens op een van de volgende manieren in, afhankelijk van de installatie:

    - **Afzonderlijke inschrijving**: 

        1. Bewerk `/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningX509Sample.java` om de eerder genoteerde waarden voor _Id-bereik_ en _Globaal eindpunt voor inrichtingsservice_ toe te voegen. Neem ook de waarden voor _Clientcertificaat_ en _Persoonlijke sleutel van clientcertificaat_ op die u eerder hebt genoteerd.

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

    - **Inschrijvingsgroepen**: 

        1. Volg de instructies voor **Afzonderlijke inschrijving** hierboven.

        1. Voeg de volgende regels code toe aan het begin van de functie `main`.
        
            ```java
            String intermediatePem = "<Your Signer Certificate here>";          
            String rootPem = "<Your Root Certificate here>";
                
            signerCertificates.add(intermediatePem);
            signerCertificates.add(root);
            ```
    
            - Gebruik de volgende notatie om de certificaten op te nemen:
        
                ```java
                String intermediatePem = "-----BEGIN CERTIFICATE-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "-----END CERTIFICATE-----\n";
                String rootPem = "-----BEGIN CERTIFICATE-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "-----END CERTIFICATE-----\n";
                ```

1. Bouw het voorbeeld. Navigeer naar de doelmap en voer het gemaakte JAR-bestand uit.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-x509-sample-{version}-with-deps.jar
    ```

1. Navigeer in de portal naar de IoT-hub die is gekoppeld aan uw Provisioning-service en open de blade **Device Explorer**. Wanneer het inrichten van het gesimuleerde X.509-apparaat voor de hub is geslaagd, wordt de apparaat-ID weergegeven op de blade **Device Explorer** met de *STATUS* **ingeschakeld**. Let op: u moet mogelijk klikken op de knop **Vernieuwen** bovenaan als u de blade vóór het uitvoeren van de voorbeeldapparaattoepassing al hebt geopend. 

    ![Apparaat wordt geregistreerd voor de IoT-hub](./media/java-quick-create-simulated-device-x509/hub-registration.png) 

> [!NOTE]
> Als u de standaardwaarde van de *initiële status van de apparaatdubbel* hebt gewijzigd in de inschrijvingsvermelding voor uw apparaat, kan de gewenste status van de dubbel uit de hub worden gehaald en er dienovereenkomstig naar worden gehandeld. Zie [Apparaatdubbels begrijpen en gebruiken in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md) voor meer informatie.
>


## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt blijven doorwerken met het voorbeeld van de apparaatclient en deze beter wilt leren kennen, wis de resources die in deze quickstart zijn gemaakt dan niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources die via deze quickstart zijn gemaakt, te verwijderen.

1. Sluit het uitvoervenster van het voorbeeld van de apparaatclient op de computer.
1. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer uw Device Provisioning Service. Open de blade **Inschrijvingen beheren** voor uw service en klik vervolgens op het tabblad **Afzonderlijke inschrijvingen**. Selecteer de *registratie-id* van het apparaat dat u hebt ingeschreven met behulp van deze quickstart. Klik vervolgens bovenaan op de knop **Verwijderen**. 
1. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer vervolgens uw IoT-hub. Open de blade **IoT-apparaten** voor uw hub, selecteer de *apparaat-id* van het apparaat dat u hebt geregistreerd in deze quickstart en klik vervolgens bovenaan op de knop **Verwijderen**.


## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een gesimuleerd X.509-apparaat op uw Windows-computer gemaakt en het ingericht voor uw IoT-hub met de Azure IoT Hub Device Provisioning Service in de portal. Als u wilt weten hoe u uw X.509-apparaat programmatisch kunt registreren, gaat u verder met de quickstart voor programmatische registratie van een X.509-apparaat. 

> [!div class="nextstepaction"]
> [Azure-quickstart: X.509-apparaat inschrijven bij Azure IoT Hub Device Provisioning Service](quick-enroll-device-x509-java.md)
