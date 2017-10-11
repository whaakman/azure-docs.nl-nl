Vanwege de verdere ontwikkeling mogelijk de Android SDK-versie in Android Studio is geïnstalleerd niet overeen met de versie in de code. De Android SDK, waarnaar wordt verwezen in deze zelfstudie is versie 23, de meest recente op het moment van schrijven. Het versienummer toenemen naarmate nieuwe releases van de SDK worden weergegeven en wordt aangeraden de laatst beschikbare versie.

Er zijn twee symptomen van versie komt niet overeen:

- Wanneer u bouwen of bouw het project opnieuw, mogelijk dat u de foutberichten Gradle zoals '**kan niet worden gevonden doel Google Inc.:Google APIs:n**'.
- Standaard Android objecten in de code die moet worden omgezet op basis van `import` instructies foutberichten kunnen genereren.

Als een van beide wordt weergegeven, de versie van de Android SDK geïnstalleerd in Android Studio mogelijk niet overeen met de SDK-doel van het gedownloade project. Om te controleren of de versie, moet u de volgende wijzigingen aanbrengen:

1. Klik in Android Studio **extra** > **Android** > **SDK Manager**. Als u de nieuwste versie van het Platform SDK niet hebt geïnstalleerd, vervolgens klikt u op om het te installeren. Noteer het versienummer.
2. Op de **Projectverkenner** tabblad onder **Gradle Scripts**, open het bestand **build.gradle (modeule: app)**. Zorg ervoor dat de **compileSdkVersion** en **buildToolsVersion** zijn ingesteld op de nieuwste SDK versie is geïnstalleerd. De labels uitzien als volgt:

             compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
3. In de Android Studio Project Explorer met de rechtermuisknop op het projectknooppunt, kiest u **eigenschappen**, en kies in de linkerkolom **Android**. Zorg ervoor dat de **doel van de Projectbuild** is ingesteld op dezelfde SDK versie als de **targetSdkVersion**.

Het manifestbestand is niet meer in Android Studio gebruikt om op te geven van de SDK-doel en de minimale SDK-versie, in tegenstelling tot het geval is bij Eclipse.
