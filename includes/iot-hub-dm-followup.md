## <a name="customize-and-extend-the-device-management-actions"></a>Aanpassen en uitbreiden van het apparaat beheeracties

Uw IoT-oplossingen kunnen uitbreiden van de gedefinieerde set apparaatbeheerpatronen of aangepaste patronen inschakelen met behulp van het dubbele apparaat en cloud-naar-apparaat methode primitieven. Andere voorbeelden van acties voor het beheer van apparaten zijn Fabrieksinstellingen terugzetten, firmware-update, software-update, energiebeheer, netwerk-en-connectiviteit en versleuteling van gegevens.

## <a name="device-maintenance-windows"></a>Onderhoudsvensters voor apparaat

Meestal configureert u apparaten bewerkingen uit te voeren op een tijdstip dat onderbrekingen en uitvaltijd minimaliseert. Apparaat-onderhoudsvensters zijn een veelgebruikte patroon voor het definiëren van de tijd wanneer de configuratie ervan door een apparaat moet worden bijgewerkt. Uw back-end-oplossingen kunnen u de gewenste eigenschappen van het dubbele apparaat gebruiken om te definiëren en activeren van een beleid op uw apparaat waarmee een onderhoudsvenster. Wanneer een apparaat het onderhoud-venster-beleid ontvangt, kunt deze de gerapporteerde eigenschap van het dubbele apparaat gebruiken om de status van het beleid te rapporteren. De back-end-app kunt vervolgens apparaatdubbel-query's gebruiken om te bevestigen voor de naleving van apparaten en elk beleid.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een rechtstreekse methode voor het activeren van een extern opnieuw opstarten op een apparaat. U gebruikt de gerapporteerde eigenschappen voor het rapporteren van de laatste keer opnieuw opstarten van het apparaat en query uitgevoerd op het dubbele apparaat voor het detecteren van de laatste keer opnieuw opstarten van het apparaat vanuit de cloud.

Zie het volgende om door te gaan aan de slag met IoT Hub en patronen voor Apparaatbeheer zoals het op afstand via de lucht firmware-update:

[Zelfstudie: Hoe u doet een firmware-update][lnk-fwupdate]

Zie voor informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten roept, de [taken plannen en uitzenden] [ lnk-tutorial-jobs] zelfstudie.

Om door te gaan aan de slag met IoT Hub [aan de slag met IoT Edge][lnk-iot-edge].

[lnk-fwupdate]: ../articles/iot-hub/iot-hub-node-node-firmware-update.md
[lnk-tutorial-jobs]: ../articles/iot-hub/iot-hub-node-node-schedule-jobs.md
[lnk-iot-edge]: ../articles/iot-edge/tutorial-simulate-device-linux.md