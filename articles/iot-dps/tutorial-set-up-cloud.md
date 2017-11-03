---
title: Cloud voor Azure IoT Hub apparaat-inrichtingsservice ingesteld in de portal | Microsoft Docs
description: IoT Hub apparaat automatische inrichting in Azure Portal
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
ms.openlocfilehash: 088d127521ce89d3a82e30ad8797fe5746ae7e03
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-cloud-resources-for-device-provisioning-with-the-iot-hub-device-provisioning-service"></a>Cloudresources configureren voor mobiele apparaten inrichten met de IoT Hub apparaat inrichten van Service

Deze zelfstudie ziet het instellen van de cloud voor automatische inrichting inrichten met de IoT Hub apparaat inrichtingsservice. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De Azure portal gebruiken voor het maken van een Service IoT Hub apparaat-inrichting en ophalen van het ID-bereik
> * Een IoT Hub maken
> * Koppelen van de IoT-hub aan de Service voor het inrichten van apparaten
> * Het toewijzingsbeleid instellen voor de Service voor het inrichten van apparaat

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-device-provisioning-service-instance-and-get-the-id-scope"></a>Maken van een apparaat inrichtingsservice-exemplaar en ophalen van het ID-bereik

Volg deze stappen voor het maken van een nieuw apparaat inrichtingsservice-exemplaar.

1. Klik in de linkerbovenhoek van de Azure portal op **nieuw**.
2. Typ in het zoekvak **apparaten inrichten**. 
3. Klik op **IoT Hub apparaat-Service inricht**.
4. Vul de **IoT Hub apparaat-inrichtingsservice** formulier met de volgende informatie:
    
   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Naam** | Een unieke naam | -- | 
   | **Abonnement** | Uw abonnement  | Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   | **Resourcegroep** | myResourceGroup | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   | **Locatie** | Een geldige locatie | Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's. |   

   ![Geef algemene informatie over uw DP's in de portal](./media/tutorial-set-up-cloud/create-iot-dps-portal.png)

5. Klik op **Create**.
6. De *ID-bereik* wordt gebruikt voor het identificeren van de registratie-id's en biedt de zekerheid dat de registratie-ID uniek is. Als u deze waarde, klikt u **overzicht** openen de **Essentials** pagina voor de Service voor het inrichten van apparaten. Kopieer de **ID-bereik** waarde naar een tijdelijke locatie voor later gebruik.
7. Maak ook een notitie van de **Service-eindpunt** waarde of kopieer deze naar een tijdelijke locatie op voor later gebruik. 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

U hebt nu uw IoT Hub gemaakt en u hebt de hostnaam en de IoT Hub-verbindingsreeks die u nodig hebt voor de rest van deze handleiding.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>De Service voor het inrichten van apparaten koppelen aan een IoT-hub

De volgende stap is het koppelen van de inrichtingsservice apparaat en IoT hub, zodat de IoT Hub apparaat inrichtingsservice hub-apparaten kunt registreren. De service kan alleen apparaten met IoT-hubs die zijn gekoppeld aan de Service voor het inrichten van apparaten inrichten. Volg deze stappen.

1. In de **alle resources** pagina, klikt u op het apparaat wordt ingericht Service-exemplaar die u eerder hebt gemaakt.
2. Klik op de pagina apparaat inrichtingsservice **gekoppelde IoT hubs**.
3. Klik op **Add**.
4. In de **koppeling toevoegen met iothub** pagina, gebruik de keuzerondjes om op te geven of de gekoppelde IoT-hub zich in het huidige abonnement of in een ander abonnement bevindt. Kies de naam van de IoT-hub uit de **IoT-hub** vak.
5. Klik op **Opslaan**.

   ![De naam van de hub om te koppelen aan de DP's in de portal koppelen](./media/tutorial-set-up-cloud/link-iot-hub-to-dps-portal.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Het toewijzingsbeleid instellen voor de Service voor het inrichten van apparaat

Het toewijzingsbeleid is een IoT Hub apparaat inrichtingsservice-instelling die bepaalt hoe apparaten worden toegewezen aan een IoT-hub. Er zijn drie ondersteunde toewijzingsbeleid: 

1. **Laagste latentie**: apparaten die zijn ingericht met een IoT-hub die is gebaseerd op de hub met de laagste latentie voor het apparaat.
2. **Gelijkmatig gewogen distributie** (standaard): gekoppelde IoT hubs zijn waarschijnlijk gelijkmatig ingericht op deze apparaten hebben. Dit is de standaardinstelling. Als u apparaten met slechts één IoT-hub inricht, kunt u deze instelling bewaren. 
3. **Statische configuratie via de lijst inschrijving**: specificatie van de gewenste IoT-hub in de lijst inschrijving heeft voorrang boven het toewijzingsbeleid van mobiele apparaten inrichten serviceniveau.

Het toewijzingsbeleid instellen in de pagina apparaat inrichtingsservice op **beheren toewijzingsbeleid voor**. Zorg ervoor dat het toewijzingsbeleid is ingesteld op **gelijkmatig gewogen distributie** (de standaardinstelling). Als u geen wijzigingen aanbrengen, klikt u op **opslaan** wanneer u klaar bent.

![Toewijzingsbeleid voor beheren](./media/tutorial-set-up-cloud/iot-dps-manage-allocation.png)

## <a name="clean-up-resources"></a>Resources opschonen

Andere zelfstudies in deze verzameling bouwen voort op deze zelfstudie. Als u van plan bent om door te gaan op om te werken met de volgende snel aan de slag of met de zelfstudies, geen clean up maakt van de resources in deze zelfstudie hebt gemaakt. Als u niet van plan bent om door te gaan, gebruik de volgende stappen uit om te verwijderen van alle resources die zijn gemaakt met deze zelfstudie in de Azure portal.

1. Klik in het menu links in de Azure portal op **alle resources** en selecteer vervolgens uw IoT Hub apparaat inrichtingsservice-exemplaar. Aan de bovenkant van de **alle resources** pagina, klikt u op **verwijderen**.  
2. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer vervolgens uw IoT-hub. Aan de bovenkant van de **alle resources** pagina, klikt u op **verwijderen**.
 
## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De Azure portal gebruiken voor het maken van een Service IoT Hub apparaat-inrichting en ophalen van het ID-bereik
> * Een IoT Hub maken
> * Koppelen van de IoT-hub aan de Service voor het inrichten van apparaten
> * Het toewijzingsbeleid instellen voor de Service voor het inrichten van apparaat

Ga naar de volgende zelfstudie voor informatie over het instellen van uw apparaat voor het inrichten.

> [!div class="nextstepaction"]
> [Apparaat voor het inrichten van instellen](tutorial-set-up-device.md)
