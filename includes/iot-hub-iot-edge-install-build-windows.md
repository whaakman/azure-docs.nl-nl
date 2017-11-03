## <a name="install-the-prerequisites"></a>De vereiste software installeren

1. Installeer [Visual Studio 2015 of 2017](https://www.visualstudio.com). U kunt de editie free-Community gebruiken als u voldoet aan de licentievereisten. Zorg dat u Visual C++ en NuGet Package Manager bevatten.

1. Installeer [git](http://www.git-scm.com) en zorg ervoor dat u kunt git.exe uitvoeren vanaf de opdrachtregel.

1. Installeer [CMake](https://cmake.org/download/) en zorg ervoor dat u kunt cmake.exe uitvoeren vanaf de opdrachtregel. CMake versie 3.7.2 of later wordt aanbevolen. De **.msi** installatieprogramma is de eenvoudigste optie in Windows. Voeg ten minste CMake toe aan het pad voor de huidige gebruiker wanneer het installatieprogramma wordt u gevraagd.

1. Installeer [Python 2.7](https://www.python.org/downloads/release/python-27). Zorg ervoor dat u bij het toevoegen van Python die moet worden uw `PATH` omgevingsvariabele. Ga naar **Configuratiescherm** > **systeem en beveiliging** > **System** > **Geavanceerde systeeminstellingen**  >  **Omgevingsvariabelen**. Voeg `C:\Python27` naar uw pad. 

1. Voer bij een opdrachtprompt de volgende opdracht voor het klonen van de Azure IoT rand GitHub-opslagplaats met uw lokale machine:

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-to-build-the-sample"></a>Het voorbeeld maken

Nu kunt u de rand van de IoT-runtime en voorbeelden bouwen op uw lokale machine:

1. Open **opdrachtprompt voor ontwikkelaars voor VS 2015** of **opdrachtprompt voor ontwikkelaars voor VS 2017**, afhankelijk van uw versie.

1. Navigeer naar de hoofdmap van uw lokale exemplaar van de **iot-edge**-opslagplaats.

1. Voer het build-script als volgt:

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

Dit script maakt een bestand van Visual Studio-oplossing en de oplossing is gebaseerd. U vindt de Visual Studio-oplossing in de **bouwen** map in de lokale kopie van de **iot-edge** opslagplaats. Als u wilt bouwen en de eenheidstests uitvoeren, voegt de `--run-unittests` parameter. Als u wilt bouwen en de end-to-end-tests uitvoeren, voegt de `--run-e2e-tests`.

> [!NOTE]
> Telkens wanneer u uitvoert het **build.cmd** script, op het worden verwijderd en vervolgens opnieuw de **bouwen** map in de hoofdmap van uw lokale exemplaar van de **iot-edge** opslagplaats.