## <a name="build-iot-edge"></a>IoT-rand bouwen

Deze zelfstudie maakt gebruik van aangepaste modules die IoT rand om te communiceren met de vooraf geconfigureerde oplossing voor externe controle. Daarom moet u voor het bouwen van de rand van de IoT-modules van aangepaste broncode. De volgende secties wordt beschreven hoe IoT rand installeren en de aangepaste rand van de IoT-module maken.

### <a name="install-iot-edge"></a>IoT-rand installeren

De volgende stappen wordt beschreven hoe de vooraf gecompileerde IoT Edge-software te installeren op de Intel NUC:

1. Configureer de opslagplaatsen vereist smart pakket met de volgende opdrachten op de Intel NUC:

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    Voer `y` wanneer de opdracht wordt u gevraagd te **dit kanaal opnemen?**.

1. De smartcard package manager bijwerken met de volgende opdracht:

    ```bash
    smart update
    ```

1. Installeer het Azure IoT Edge-pakket met de volgende opdracht:

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. Controleer of de installatie door het uitvoeren van de steekproef "Hallo wereld". Dit voorbeeld wordt een Hallo wereld naar het bestand log.txT elke vijf seconden. De volgende opdrachten uitvoeren in het voorbeeld 'Hallo wereld':

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    Alle **ongeldig argument** wanneer u stopt met het voorbeeld van berichten.

    Gebruik de volgende opdracht om de inhoud van het logboekbestand weer te geven:

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a>Problemen oplossen

Als u de foutmelding "Er is geen pakket biedt util-linux-dev', de Intel NUC opnieuw op te starten.
