---
title: De Azure IoT Hub Device Provisioning-service gebruiken om apparaten in te richten op IoT-hubs met gelijke taakverdeling | Microsoft Docs
description: DPS voor automatische apparaatinrichting voor IoT-hubs met gelijke taakverdeling in Azure Portal
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: d0a3720fe729d5e260bbe5b0902460c8c7cfc7cb
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629623"
---
# <a name="provision-devices-across-load-balanced-iot-hubs"></a>Apparaten inrichten in IoT-hubs met gelijke taakverdeling

In deze zelfstudie kunt u zien hoe u apparaten kunt inrichten voor meerdere IoT-hubs met gelijke taakverdeling met behulp van de Device Provisioning-service (DPS). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Azure Portal gebruiken om een tweede apparaat in te richten op een tweede IoT-hub 
> * Een vermelding in de registratielijst toevoegen aan het tweede apparaat
> * Het DPS-toewijzingsbeleid instellen op **gelijkmatige verdeling**
> * De nieuwe IoT-hub aan DPS koppelen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie gaat verder op de vorige zelfstudie [Een apparaat inrichten op een hub](tutorial-provision-device-to-hub.md).

## <a name="use-the-azure-portal-to-provision-a-second-device-to-a-second-iot-hub"></a>Azure Portal gebruiken om een tweede apparaat in te richten op een tweede IoT-hub

Volg de stappen in de zelfstudie [Een apparaat inrichten op een hub](tutorial-provision-device-to-hub.md) om een tweede apparaat in te richten op een andere IoT-hub.

## <a name="add-an-enrollment-list-entry-to-the-second-device"></a>Een vermelding in de registratielijst toevoegen aan het tweede apparaat

Via de registratielijst wordt voor DPS aangegeven welke methode van attestation (de methode waarmee een apparaat-id wordt bevestigd) wordt gebruikt voor het apparaat. In de volgende stap voegt u een vermelding aan de registratielijst toe voor het tweede apparaat. 

1. Klik op de pagina voor uw DPS op **Registraties beheren**. De pagina **Vermelding aan registratielijst toevoegen** wordt weergegeven. 
2. Klik boven aan de pagina op **Toevoegen**.
2. Vul de velden in en klik op **Opslaan**.

## <a name="set-the-dps-allocation-policy"></a>Het DPS-toewijzingsbeleid instellen

Het toewijzingsbeleid is een DPS-instelling waarmee wordt bepaald hoe apparaten aan een IoT-hub worden toegewezen. Er worden drie soorten toewijzingsbeleid ondersteund: 

1. **Laagste latentie**: apparaten worden ingericht voor een IoT-hub op basis van de hub met de laagste latentie voor het apparaat.
2. **Gelijk gewogen distributie** (standaardinstelling): gekoppelde IoT-hubs hebben evenveel kans dat apparaten voor ze worden ingericht. Dit is de standaardinstelling. Als u apparaten voor slechts één IoT-hub inricht, kunt u deze instelling bewaren. 
3. **Statische configuratie via de registratielijst**: specificatie van de gewenste IoT-hub in de registratielijst heeft voorrang boven het toewijzingsbeleid op DPS-niveau.

Volg deze stappen om het toewijzingsbeleid in te stellen:

1. Als u het toewijzingsbeleid wilt instellen, klikt u op de pagina DPS op **Toewijzingsbeleid beheren**.
2. Stel het DPS-toewijzingsbeleid in op **Gelijk gewogen distributie**.
3. Klik op **Opslaan**.

## <a name="link-the-new-iot-hub-to-dps"></a>De nieuwe IoT-hub aan DPS koppelen

Koppel de DPS en IoT-hub, zodat via DPS apparaten kunnen worden geregistreerd naar deze hub.

1. Klik op de pagina **Alle resources** op de DPS die u eerder hebt gemaakt.
2. Klik op de pagina DPS op **Gekoppelde IoT-hubs**.
3. Klik op **Add**.
4. Geef op de pagina **Koppeling toevoegen aan IoT-hub** met behulp van de keuzerondjes aan of de gekoppelde IoT-hub zich in het huidige abonnement of in een ander abonnement bevindt. Kies vervolgens de naam van de IoT-hub in het vak **IoT-hub**.
5. Klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Azure Portal gebruiken om een tweede apparaat in te richten op een tweede IoT-hub 
> * Een vermelding in de registratielijst toevoegen aan het tweede apparaat
> * Het DPS-toewijzingsbeleid instellen op **gelijkmatige verdeling**
> * De nieuwe IoT-hub aan DPS koppelen

<!-- Advance to the next tutorial to learn how to 
 Replace this .md
> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)
-->
