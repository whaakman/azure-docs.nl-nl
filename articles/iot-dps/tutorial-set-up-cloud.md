---
title: Cloud instellen voor Azure IoT Hub Device Provisioning Service in portal | Microsoft Docs
description: Automatische apparaatinrichting van IoT Hub in Azure Portal
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: e334ff0c8dec3a9611b60f64e565111064d10c18
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38619279"
---
# <a name="configure-cloud-resources-for-device-provisioning-with-the-iot-hub-device-provisioning-service"></a>Cloudresources configureren voor apparaatinrichting met de IoT Hub Device Provisioning Service

In deze zelfstudie wordt beschreven hoe u de cloud instelt voor automatische apparaatinrichting met de IoT Hub Device Provisioning Service. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Azure Portal gebruiken voor het maken van een IoT Hub Device Provisioning Service en het ophalen van het id-bereik
> * Een IoT Hub maken
> * De IoT-hub koppelen aan de Device Provisioning Service
> * Het toewijzingsbeleid instellen voor de Device Provisioning Service

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-device-provisioning-service-instance-and-get-the-id-scope"></a>Een instantie van een Device Provisioning Service maken en het id-bereik ophalen

Volg deze stappen om een nieuwe instantie van een Device Provisioning Service te maken.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Typ in het zoekvak **device provisioning**. 
3. Klik op **IoT Hub Device Provisioning Service**.
4. Vul in het formulier **IoT Hub Device Provisioning Service** de volgende gegevens in:
    
   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Naam** | Een unieke naam | -- | 
   | **Abonnement** | Uw abonnement  | Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   | **Resourcegroep** | myResourceGroup | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   | **Locatie** | Een geldige locatie | Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's. |   

   ![Voer basisinformatie in over uw DPS in de portal](./media/tutorial-set-up-cloud/create-iot-dps-portal.png)

5. Klik op **Create**.
6. Het *id-bereik* wordt gebruikt voor het identificeren van de registratie-id's en biedt de zekerheid dat de registratie-id uniek is. Om deze waarde op te halen, klikt u op **Overzicht** om de pagina **Essentials** te openen voor de Device Provisioning Service. Kopieer de waarde voor het **id-bereik** naar een tijdelijke locatie voor later gebruik.
7. Maak ook een notitie van de waarde van het **Service-eindpunt** of kopieer deze naar een tijdelijke locatie op voor later gebruik. 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

U hebt nu uw IoT Hub gemaakt en u hebt de hostnaam en de IoT Hub-verbindingsreeks die u nodig hebt voor de rest van deze handleiding.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>De Device Provisioning Service koppelen aan een IoT-hub

De volgende stap bestaat uit het koppelen van de Device Provisioning Service en IoT-hub, zodat de IoT Hub Device Provisioning Service apparaten kan registreren bij die hub. De service kan alleen apparaten die zijn gekoppeld aan de Device Provisioning Service inrichten voor IoT-hubs. Volg deze stappen.

1. Klik op de pagina **Alle resources** op de instantie van de Device Provisioning Service die u eerder hebt gemaakt.
2. Klik op de pagina Device Provisioning Service op **Gekoppelde IoT-hubs**.
3. Klik op **Add**.
4. Geef op de pagina **Koppeling toevoegen aan IoT-hub** met behulp van de keuzerondjes aan of de gekoppelde IoT-hub zich in het huidige abonnement of in een ander abonnement bevindt. Kies vervolgens de naam van de IoT-hub in het vak **IoT-hub**.
5. Klik op **Opslaan**.

   ![De naam van de hub koppelen aan de DPS in de portal](./media/tutorial-set-up-cloud/link-iot-hub-to-dps-portal.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Het toewijzingsbeleid instellen voor de Device Provisioning Service

Het toewijzingsbeleid is een instelling voor de IoT Hub Device Provisioning Service die bepaalt hoe apparaten aan een IoT-hub worden toegewezen. Er worden drie soorten toewijzingsbeleid ondersteund: 

1. **Laagste latentie**: apparaten worden ingericht voor een IoT-hub op basis van de hub met de laagste latentie voor het apparaat.
2. **Gelijk gewogen distributie** (standaardinstelling): gekoppelde IoT-hubs hebben evenveel kans dat apparaten voor ze worden ingericht. Dit is de standaardinstelling. Als u apparaten voor slechts één IoT-hub inricht, kunt u deze instelling bewaren. 
3. **Statische configuratie via de lijst inschrijving**: specificatie van de gewenste IoT-hub in de registratielijst heeft voorrang boven het toewijzingsbeleid op het niveau van de Device Provisioning Service.

Als u het toewijzingsbeleid wilt instellen, klikt u op de pagina Device Provisioning Service op **Toewijzingsbeleid beheren**. Zorg ervoor dat het toewijzingsbeleid is ingesteld op **Gelijk gewogen distributie** (de standaardinstelling). Als u geen wijzigingen aanbrengt, klikt u op **Opslaan** wanneer u klaar bent.

![Toewijzingsbeleid beheren](./media/tutorial-set-up-cloud/iot-dps-manage-allocation.png)

## <a name="clean-up-resources"></a>Resources opschonen

Andere zelfstudies in deze verzameling zijn op deze zelfstudie gebaseerd. Als u wilt doorgaan met andere snelstartgidsen of met de zelfstudies, verwijdert u de resources die u in deze zelfstudie hebt gemaakt, niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze zelfstudie in Azure Portal zijn gemaakt.

1. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer de instantie van uw IoT Hub Device Provisioning Service. Klik boven aan de pagina **Alle resources** op **Verwijderen**.  
2. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer vervolgens uw IoT-hub. Klik boven aan de pagina **Alle resources** op **Verwijderen**.
 
## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Azure Portal gebruiken voor het maken van een IoT Hub Device Provisioning Service en het ophalen van het id-bereik
> * Een IoT Hub maken
> * De IoT-hub koppelen aan de Device Provisioning Service
> * Het toewijzingsbeleid instellen voor de Device Provisioning Service

Ga door naar de volgende zelfstudie om te leren hoe u uw apparaat instelt voor inrichting.

> [!div class="nextstepaction"]
> [Apparaat instellen voor inrichting](tutorial-set-up-device.md)
