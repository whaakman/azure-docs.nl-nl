## <a name="install-the-prerequisites"></a>De vereiste software installeren

De stappen in deze zelfstudie wordt ervan uitgegaan dat u Ubuntu Linux wordt uitgevoerd.

De vereiste pakketten installeren, opent u een shell en voer de volgende opdrachten:

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake pkg-config libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

Voer de volgende opdracht voor het klonen van de Azure IoT rand GitHub-opslagplaats op uw lokale computer in de shell:

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-to-build-the-sample"></a>Het voorbeeld maken

Nu kunt u de rand van de IoT-runtime en voorbeelden bouwen op uw lokale machine:

1. Open een shell.

1. Navigeer naar de hoofdmap van uw lokale exemplaar van de **iot-edge**-opslagplaats.

1. Voer het build-script als volgt:

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

Dit script gebruikt het hulpprogramma **cmake** om een map **build** te maken in de hoofdmap van uw lokale exemplaar van de **iot-edge**-opslagplaats en een makefile te genereren. Het script bouwt vervolgens de oplossing, waarbij eenheidstest en end-to-end-tests worden overgeslagen. Als u wilt bouwen en de eenheidstests uitvoeren, voegt de `--run-unittests` parameter. Als u wilt bouwen en de end-to-end-tests uitvoeren, voegt de `--run-e2e-tests`.

> [!NOTE]
> Telkens wanneer u het script **build.sh** uitvoert, wordt de map **build** in de hoofdmap van het lokale exemplaar van de **iot-edge**-opslagplaats verwijderd en opnieuw gemaakt.