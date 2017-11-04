## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Een Azure Storage-account met IoT Hub koppelen

Omdat de gesimuleerde apparaattoepassing een bestand naar een blob uploadt, hebt u een [Azure Storage](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) account dat is gekoppeld met IoT Hub. Wanneer u een Azure Storage-account aan een IoT-hub koppelen, genereert de iothub een SAS-URI. Een apparaat kunt u deze SAS-URI gebruiken voor het veilig een bestand te uploaden naar een blob-container. De IoT Hub-service en de apparaat-SDK's coördinatie van het proces dat de SAS-URI genereert en beschikbaar is op een apparaat moet worden gebruikt om een bestand te uploaden.

Volg de instructies in [configureren bestand uploadt met de Azure portal](../articles/iot-hub/iot-hub-configure-file-upload.md) om te koppelen van een Azure Storage-account aan uw IoT-hub. Zorg ervoor dat een blob-container gekoppeld aan uw IoT-hub is en dat bestandsmeldingen zijn ingeschakeld.

![Schakel meldingen in bestand in de portal](media/iot-hub-associate-storage/enable-file-notifications.png)