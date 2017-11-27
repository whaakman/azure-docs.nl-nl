Vanwege de verdere ontwikkeling mogelijk de Android SDK-versie in Android Studio is geïnstalleerd niet overeen met de versie in de code. De Android SDK, waarnaar wordt verwezen in deze zelfstudie is versie 26, de meest recente op het moment van schrijven. Het versienummer toenemen naarmate nieuwe releases van de SDK worden weergegeven en wordt aangeraden de laatst beschikbare versie.

Er zijn twee symptomen van versie komt niet overeen:

- Wanneer u bouwen of bouw het project opnieuw, mogelijk dat u de foutberichten Gradle zoals `Gradle sync failed: Failed to find target with hash string 'android-XX'`.
- Standaard Android objecten in de code die moet worden omgezet op basis van `import` instructies foutberichten kunnen genereren.

Als een van beide wordt weergegeven, de versie van de Android SDK geïnstalleerd in Android Studio mogelijk niet overeen met de SDK-doel van het gedownloade project. Om te controleren of de versie, moet u de volgende wijzigingen aanbrengen:

1. Klik in Android Studio **extra** > **Android** > **SDK Manager**. Als u de nieuwste versie van het Platform SDK niet hebt geïnstalleerd, vervolgens klikt u op om het te installeren. Noteer het versienummer.

2. Op de **Projectverkenner** tabblad onder **Gradle Scripts**, open het bestand **build.gradle (Module: app)**. Zorg ervoor dat de **compileSdkVersion** en **targetSdkVersion** zijn ingesteld op de nieuwste SDK versie is geïnstalleerd. De `build.gradle` als volgt uitzien:

    ```gradle
    android {
        compileSdkVersion 26
        defaultConfig {
            targetSdkVersion 26
        }
    }
    ```
