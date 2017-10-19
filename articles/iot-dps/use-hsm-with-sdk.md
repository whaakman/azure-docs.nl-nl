---
title: 'Azure-procedure: Andere Hardware Security Modules gebruiken met de SDK van de Device Provisioning Service-client in Azure| Microsoft Docs'
description: 'Azure-procedure: Andere Hardware Security Modules gebruiken met de SDK van de Device Provisioning Service-client in Azure'
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 08/28/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: 
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 620d86b62cf43c3e1a5f7f5c724fcf00174f30e8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-different-hardware-security-modules-with-device-provisioning-service-client-sdk"></a>Andere Hardware Security Modules gebruiken met Provisioning Service Client SDK
Deze stappen laten zien hoe u een andere [Hardware Security Module (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) met Provisioning Service Client SDK in C gebruikt met behulp van fysiek apparaat en simulator.  De inrichtingsservice ondersteunt twee verificatiemodi: X**.** 509 en TPM (Trusted Platform Module).

## <a name="prerequisites"></a>Vereisten

Bereid de ontwikkelomgeving voor volgens de richtlijnen in het gedeelte 'De ontwikkelomgeving voorbereiden' in de gids [Een gesimuleerd apparaat maken en inrichten] (./quick-create-simulated-device.md).

## <a name="enable-authentication-with-different-hsms"></a>Verificatie met verschillende HSM's inschakelen

Verificatiemodus (X**.**509 of TPM) moet zijn ingeschakeld voor het fysieke apparaat of de simulator voordat deze kunnen worden geregistreerd in Azure Portal.  Navigeer naar de hoofdmap voor azure-iot-sdk-c.  Voer afhankelijk van de gekozen verificatiemodus de opgegeven opdracht uit.

### <a name="use-x509-with-simulator"></a>X**.**509 gebruiken met simulator

De inrichtingsservice wordt geleverd met een DICE-emulator (Device Identity Composition Engine) die een X**.**509-certificaat genereert om het apparaat te authenticeren.  Voer de volgende opdracht uit om X**.**509-verificatie in te schakelen:

```
cmake -Ddps_auth_type=x509 ..
```

Informatie met betrekking tot hardware met DICE vindt u [hier](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/).

### <a name="use-x509-with-hardware"></a>X**.**509 gebruiken met hardware

De inrichtingsservice kan met X**.**509 worden gebruikt op andere hardware.  Er is een interface tussen de hardware en de SDK nodig om verbinding te maken.  Neem contact op met uw HSM-fabrikant voor informatie over de interface.

### <a name="use-tpm"></a>TPM gebruiken

De inrichtingsservice kan verbinding maken met TPM-chips van Windows- en Linux-hardware met SAS-token.  Voer de volgende opdracht uit om TPM-verificatie in te schakelen:

```
cmake -Ddps_auth_type=tpm ..
```

### <a name="use-tpm-with-simulator"></a>TPM met simulator gebruiken

Als u geen apparaat met TPM-chips hebt, kunt u een simulator gebruiken als u wilt ontwikkelen voor Windows-besturingssystemen.  Voer de volgende opdracht uit om TPM-verificatie in te schakelen en de TPM-simulator uit te voeren:

```
cmake -Ddps_auth_type=tpm_simulator ..
```

## <a name="build-the-sdk"></a>De SDK bouwen
Bouw de SDK voordat u een apparaatregistratie maakt.

### <a name="linux"></a>Linux
- De SDK bouwen in Linux:
    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    cmake ..
    cmake --build .  # append '-- -j <n>' to run <n> jobs in parallel
    ```
- Voor het maken van binaire foutopsporingsbestanden voegt u de bijbehorende CMake-optie toe aan de opdracht voor het genereren van het project, bijvoorbeeld:
    ```
    cmake -DCMAKE_BUILD_TYPE=Debug ..
    ```

- Er zijn veel [CMake-configuratieopties](https://cmake.org/cmake/help/v3.6/manual/cmake.1.html) beschikbaar voor het bouwen van de SDK. Zo kunt u bijvoorbeeld een van de beschikbare protocolstacks uitschakelen door een argument toe te voegen aan de opdracht voor het maken van een CMake-project:
    ```
    cmake -Duse_amqp=OFF ..
    ```
- U kunt ook een moduletest bouwen en uitvoeren:
    ```
    cmake -Drun_unittests=ON ..
    cmake --build .
    ctest -C "debug" -V
    ```

### <a name="windows"></a>Windows
- Als u de SDK in Windows wilt bouwen, voer dan de volgende stappen uit om projectbestanden te genereren:
    - Open een 'Opdrachtprompt voor ontwikkelaars voor VS2015'
    - Voer de volgende CMake-opdrachten uit vanuit de hoofdmap van de opslagplaats:
      ```
      cd azure-iot-sdk-c
      mkdir cmake
      cd cmake
      cmake -G "Visual Studio 14 2015" ..
      ```
    Met deze opdracht bouwt u x86-bibliotheken. Wijzig het argument van de cmake-generator als u x64-bibliotheken wilt bouwen: 
    ```
    cmake .. -G "Visual Studio 14 2015 Win64"
    ```

- Als het project met succes is gegenereerd, wordt er onder de map `cmake` een Visual Studio-oplossingsbestand (.sln) weergegeven. De SDK bouwen:
    - Open **cmake\azure_iot_sdks.sln** in Visual Studio en bouw het, **of**
    - Voer de volgende opdracht uit achter de opdrachtprompt die u hebt gebruikt om de projectbestanden te genereren:
      ```
      cmake --build . -- /m /p:Configuration=Release
      ```
- Gebruik het bijbehorende MSBuild-argument als u binaire debugbestanden wilt bouwen: 
    ```
    cmake --build . -- /m /p:Configuration=Debug`
    ```
- Er zijn veel CMake-configuratieopties beschikbaar voor het bouwen van de SDK. Zo kunt u bijvoorbeeld een van de beschikbare protocolstacks uitschakelen door een argument toe te voegen aan de opdracht voor het maken van een CMake-project:
    ```
    cmake -G "Visual Studio 14 2015" -Duse_amqp=OFF ..
    ```
- U kunt ook moduletests bouwen en uitvoeren:
    ```
    cmake -G "Visual Studio 14 2015" -Drun_unittests=ON ..
    cmake --build . -- /m /p:Configuration=Debug
    ctest -C "debug" -V
    ```
  
### <a name="libraries-to-include"></a>Bibliotheken die u moet opnemen
- Deze bibliotheken moeten worden opgenomen in de SDK:
    - De inrichtingsservice: dps_http_transport, dps_client, dps_security_client
    - IoTHub Security: iothub_security_client

## <a name="create-a-device-enrollment-entry-in-dps"></a>Een vermelding voor apparaatregistratie maken in DPS

### <a name="tpm"></a>TPM
Als u een TPM gebruikt, volgt u de instructies in ['Een gesimuleerd apparaat maken en inrichten met IoT Hub Device Provisioning Service'](./quick-create-simulated-device.md) als u een vermelding voor een apparaatregistratie wilt maken in DPS en simuleer de eerste keer opstarten.

### <a name="x509"></a>X**.**509
1. Als u een apparaat wilt registreren in de inrichtingsservice, moet u de goedkeuringssleutel en registratie-id voor elk apparaat noteren. Deze worden weergegeven in het inrichtingshulpprogramma dat door de Client SDK is verstrekt. Voer de volgende opdracht om het basis-CA-certificaat (voor inschrijvingsgroepen) en het certificaat van de ondertekenaar (voor afzonderlijke inschrijving) af te drukken:
      ```
      ./azure-iot-sdk-c/dps_client/tools/x509_device_provision/x509_device_provision.exe
      ```
2. Meld u aan bij de Azure Portal, klik in het linkermenu op de knop **All resources** en open uw DPS-service.
   - Afzonderlijke inschrijving voor X**.**509: selecteer **Manage enrollments** in de overzichtsblade van de Device Provisioning Service. Selecteer het tabblad **Individual Enrollments** en klik bovenaan op de knop **Add**. Selecteer **X**.**509** als het *mechanisme* voor identiteitswaarmerking en upload het certificaat van de ondertekenaar zoals is vereist door de blade. Klik op de knop **Save** als u klaar bent. 
   - X**.**509-groepsregistratie: selecteer **Manage enrollments** in de overzichtsblade van de Device Provisioning Service. Selecteer het tabblad **Group Enrollments** en klik bovenaan op de knop **Add**. Selecteer **X**.**509** als het *mechanisme* voor identiteitswaarmerking, voer een groepsnaam en certificaatnaam in en upload het basis-CA-certificaat zoals is vereist door de blade. Klik op de knop **Save** als u klaar bent. 

## <a name="connecting-to-iot-hub-after-provisioning"></a>Na inrichting verbinding maken met IoT Hub

Zodra het apparaat is ingericht met de inrichtingsservice, gebruikt deze API de HSM-verificatiemodus om verbinding te maken met IoT Hub: 
  ```
  IOTHUB_CLIENT_LL_HANDLE handle = IoTHubClient_LL_CreateFromDeviceAuth(iothub_uri, device_id, iothub_transport);
  ```
