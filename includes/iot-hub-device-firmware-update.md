## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie doet u het volgende:

* U maakt een Node.js-console-app die reageert op een directe methode die door de cloud wordt aangeroepen.
* U activeert een gesimuleerde firmware-update.
* U gebruikt de gerapporteerde eigenschappen om apparaatdubbelquery's in te schakelen die de apparaten identificeren en vaststellen wanneer er voor het laatst een firmware-update op deze apparaten is uitgevoerd.

1. U maakt een lege map met de naam **simulateddevice**.  Maak in de map **simulateddevice** een bestand met de naam package.json door achter de opdrachtprompt de volgende opdracht op te geven. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```

2. Voer achter de opdrachtprompt in de map **simulateddevice** de volgende opdracht uit om de Device SDK-pakketten **azure-iot-device** en **azure-iot-device-mqtt** te installeren:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Gebruik een teksteditor om in de map **manageddevice** het bestand **dmpatterns_fwupdate_device.js** te maken.

4. Voeg de volgende 'require'-instructies toe aan het begin van het bestand **dmpatterns_fwupdate_device.js**:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Voeg een **connectionString**-variabele toe en gebruik deze om een **client**exemplaar te maken. Vervang de tijdelijke aanduiding `{yourdeviceconnectionstring}` door de verbindingsreeks die u eerder hebt genoteerd in de sectie Een apparaat-id maken:
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Voeg de volgende functie toe. Deze wordt gebruikt om gerapporteerde eigenschappen bij te werken:
   
    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };
   
      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported: ' + firmwareUpdateValue.status);
      });
    };
    ```

7. Voeg de volgende functies toe. Deze simuleren het downloaden en toepassen van de firmware-installatiekopie:
   
    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
   
      console.log("Downloading image from " + imageUrl);
   
      callback(error, image);
    }
   
    var simulateApplyImage = function(imageData, callback) {
      var error = null;
   
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
   
      callback(error);
    }
    ```

8. Voeg de volgende functie toe. Hiermee wordt de status van de firmware-update via de gerapporteerde eigenschappen gewijzigd in **Wachten**. Normaal gesproken worden apparaten op de hoogte gesteld van een beschikbare update, waarna een door de beheerder gedefinieerd beleid ervoor zorgt dat het apparaat de update downloadt en toepast. In deze functie moet de logica voor het inschakelen van dat beleid worden uitgevoerd. Voor het gemak wacht de steekproef vier seconden voordat u de firmware-installatiekopie te downloaden:
   
    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
   
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```

9. Voeg de volgende functie toe. Hiermee wordt de status van de firmware via de gerapporteerde eigenschappen gewijzigd in **Downloaden**. De functie simuleert vervolgens het downloaden van de firmware, waarna de updatestatus van de firmware wordt gewijzigd in **downloadFailed** of **downloadComplete**:
   
    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
   
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
   
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
   
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
   
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
   
      }, 4000);
    }
    ```

10. Voeg de volgende functie toe. Hiermee wordt de status van de firmware-update via de gerapporteerde eigenschappen gewijzigd in **Toepassen**. De functie simuleert vervolgens het toepassen van de firmware, waarna de updatestatus van de firmware wordt gewijzigd in **applyFailed** of **applyComplete**:
    
    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
    
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
    
      setTimeout(function() {
    
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
    
          }
        });
    
        setTimeout(callback, 4000);
    
      }, 4000);
    }
    ```

11. Voeg de volgende functie toe. Deze handelt de directe methode **firmwareUpdate** af en start het uit meerdere fasen bestaande firmware-updateproces:
    
    ```
    var onFirmwareUpdate = function(request, response) {
    
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });
    
      // Get the parameter from the body of the method request
      var fwPackageUri = request.payload.fwPackageUri;
    
      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
    
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });
    
        }
      });
    }
    ```

12. Voeg tot slot de volgende code toe die de verbinding met uw IoT-hub tot stand brengt:
    
    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
    
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate);
    });
    ```

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. In productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout](https://msdn.microsoft.com/library/hh675232.aspx).
> 
> 