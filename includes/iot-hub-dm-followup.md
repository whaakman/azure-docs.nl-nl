## <a name="customize-and-extend-the-device-management-actions"></a>Aanpassen en uitbreiden van het apparaat beheeracties

Uw IoT-oplossingen kunnen Breid de gedefinieerde set apparaat management patronen of aangepaste patronen inschakelen met behulp van het apparaat twin en cloud-naar-apparaat methode primitieven. Andere voorbeelden van acties voor apparaat zijn fabrieksinstellingen, firmware-update, software-update, energiebeheer, netwerk-en connectiviteit en versleuteling van gegevens.

## <a name="device-maintenance-windows"></a>Onderhoudsvensters voor apparaat

Meestal configureert u apparaten bewerkingen uit te voeren op een tijdstip dat onderbrekingen en uitvaltijd minimaliseert. Apparaat onderhoudsvensters zijn een veelgebruikte patroon definiëren de tijd wanneer de configuratie moet worden bijgewerkt door een apparaat. Uw back-end-oplossingen kunnen u de gewenste eigenschappen van het apparaat twin gebruiken om te definiëren en activeren van een beleid op uw apparaat waarmee een onderhoudsvenster. Wanneer een apparaat de onderhoudsbeleid venster ontvangt, kunt het de gerapporteerde eigenschap van het apparaat twin gebruiken om de status van het beleid te rapporteren. De back-endserver voor apps kunt apparaat twin query's sta in voor de compatibiliteit van apparaten en elk beleid.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een directe methode voor het activeren van een extern opnieuw opstarten op een apparaat. U gebruikt de gerapporteerde eigenschappen voor het rapporteren van de laatste keer opnieuw opstarten van het apparaat en de twin apparaat voor het detecteren van de laatste keer opnieuw opstarten van het apparaat uit de cloud opgevraagd.

Zie het volgende om door te gaan aan de slag met IoT Hub en device management patronen, zoals extern via de lucht firmware-update:

[Zelfstudie: Hoe u een firmware-update][lnk-fwupdate]

Zie voor meer informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten aanroepen, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

Zie het volgende om door te gaan aan de slag met IoT Hub [aan de slag met IoT rand][lnk-iot-edge].

[lnk-fwupdate]: ../articles/iot-hub/iot-hub-node-node-firmware-update.md
[lnk-tutorial-jobs]: ../articles/iot-hub/iot-hub-node-node-schedule-jobs.md
[lnk-iot-edge]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md