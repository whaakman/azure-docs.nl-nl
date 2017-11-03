---
title: Gebruik Azure IoT Hub apparaat inrichtingsservice inrichten van apparaten over load balanced IoT hubs | Microsoft Docs
description: DP's de inrichting van automatische inrichting voor load balanced IoT hubs in Azure Portal
services: iot-dps
keywords: 
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 4842944cd0d980fb7e817165da23b9c3c4037e94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="provision-devices-across-load-balanced-iot-hubs"></a>Inrichten van apparaten via IoT hubs Netwerktaakverdeling

Deze zelfstudie laat zien dat het inrichten van apparaten voor meerdere, taakverdeling IoT hubs met behulp van het apparaat wordt ingericht Service (DP's). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De Azure portal gebruiken voor het inrichten van een tweede apparaat met een tweede IoT-hub 
> * Een inschrijving lijst vermelding toevoegen aan het tweede apparaat
> * Het toewijzingsbeleid DP's ingesteld op **zelfs als deze distributie**
> * De nieuwe iothub aan DP's koppelen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie bouwt voort op de vorige [inrichten van het apparaat een hub](tutorial-provision-device-to-hub.md) zelfstudie.

## <a name="use-the-azure-portal-to-provision-a-second-device-to-a-second-iot-hub"></a>De Azure portal gebruiken voor het inrichten van een tweede apparaat met een tweede IoT-hub

Volg de stappen in de [inrichten van het apparaat een hub](tutorial-provision-device-to-hub.md) zelfstudie voor het inrichten van een tweede apparaat met een andere IoT-hub.

## <a name="add-an-enrollment-list-entry-to-the-second-device"></a>Een inschrijving lijst vermelding toevoegen aan het tweede apparaat

De lijst van de inschrijving wordt het DP's uitgelegd welke methode van attestation (de methode voor het bevestigen van een apparaat-id) die worden gebruikt met het apparaat. De volgende stap is het toevoegen van een vermelding van de lijst met inschrijving voor het tweede apparaat. 

1. Klik op de pagina voor uw DP's, **inschrijvingen beheren**. De **inschrijving lijst vermelding toevoegen** pagina wordt weergegeven. 
2. Klik boven aan de pagina op **toevoegen**.
2. Vul de velden en klik vervolgens op **opslaan**.

## <a name="set-the-dps-allocation-policy"></a>Stel het beleid van de toewijzing DP 's

Het toewijzingsbeleid is een DP's-instelling die bepaalt hoe apparaten worden toegewezen aan een IoT-hub. Er zijn drie ondersteunde toewijzingsbeleid: 

1. **Laagste latentie**: apparaten die zijn ingericht met een IoT-hub die is gebaseerd op de hub met de laagste latentie voor het apparaat.
2. **Gelijkmatig gewogen distributie** (standaard): gekoppelde IoT hubs zijn waarschijnlijk gelijkmatig ingericht op deze apparaten hebben. Dit is de standaardinstelling. Als u apparaten met slechts één IoT-hub inricht, kunt u deze instelling bewaren. 
3. **Statische configuratie via de lijst inschrijving**: specificatie van de gewenste IoT-hub in de lijst inschrijving heeft voorrang boven het toewijzingsbeleid DP's-niveau.

Volg deze stappen voor het instellen van het toewijzingsbeleid voor:

1. U stelt het toewijzingsbeleid door op de pagina DP's op **beheren toewijzingsbeleid voor**.
2. De van toewijzingsbeleid instellen op **gelijkmatig gewogen distributie**.
3. Klik op **Opslaan**.

## <a name="link-the-new-iot-hub-to-dps"></a>De nieuwe iothub aan DP's koppelen

De DP's en IoT-hub koppelen zodat DP's hub-apparaten kunt registreren.

1. In de **alle resources** pagina, klikt u op de DP's die u eerder hebt gemaakt.
2. Klik op de pagina DP's **gekoppelde IoT hubs**.
3. Klik op **Add**.
4. In de **koppeling toevoegen met iothub** pagina, gebruik de keuzerondjes om op te geven of de gekoppelde IoT-hub zich in het huidige abonnement of in een ander abonnement bevindt. Kies de naam van de IoT-hub uit de **IoT-hub** vak.
5. Klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De Azure portal gebruiken voor het inrichten van een tweede apparaat met een tweede IoT-hub 
> * Een inschrijving lijst vermelding toevoegen aan het tweede apparaat
> * Het toewijzingsbeleid DP's ingesteld op **zelfs als deze distributie**
> * De nieuwe iothub aan DP's koppelen

<!-- Advance to the next tutorial to learn how to 
 Replace this .md
> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)
-->
