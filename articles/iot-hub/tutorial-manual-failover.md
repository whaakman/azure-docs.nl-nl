---
title: Handmatige failover van een Azure IoT Hub | Microsoft Docs
description: Hierin wordt getoond hoe u een handmatige failover voor een Azure IoT Hub uitvoert
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: bd9cb76557c65832de5d249cdccdc36101edf646
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55821283"
---
# <a name="tutorial-perform-manual-failover-for-an-iot-hub-public-preview"></a>Zelfstudie: Handmatige failover uitvoeren voor een IoT-hub (openbare preview)

Handmatige failover is een functie van de IoT Hub-service waarmee klanten [failover](https://en.wikipedia.org/wiki/Failover) voor de bewerkingen van hun hub kunnen uitvoeren van een primaire regio naar een overeenkomstige geografisch gekoppelde Azure-regio. Handmatige failover kan worden uitgevoerd in geval van een regionale noodgeval of een langdurige servicestoring. U kunt ook een geplande failover uitvoeren om de prestaties van uw noodherstel te testen. U wordt echter aangeraden een test-IoT-hub te gebruiken in plaats van een IoT-hub die in productie wordt uitgevoerd. De functie voor handmatige failover wordt klanten zonder aanvullende kosten aangeboden.

In deze zelfstudie voert u de volgende taken uit:

> [!div class="checklist"]
> * Een IoT-hub maken met behulp van Azure Portal. 
> * Een failover uitvoeren. 
> * De hub uitgevoerd zien worden in de secundaire locatie.
> * Een failback uitvoeren om de bewerkingen van de IoT-hub in de primaire locatie terug te plaatsen. 
> * Bevestigen dat de hub op de juiste locatie wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com). 

2. Klik op **+ Een resource maken**, selecteer **Internet of Things** en vervolgens **IoT Hub**.

   ![Schermopname van het maken van een IoT-hub](./media/tutorial-manual-failover/create-hub-01.png)

3. Selecteer het tabblad **Basis**. Vul de volgende velden in.

    **Abonnement**: selecteer het gewenste Azure abonnement.

    **Resourcegroep**: klik op **Nieuwe maken** en geef **ManlFailRG** op als naam voor de resourcegroep.

    **Regio**: selecteer een regio bij u in de buurt die deel uitmaakt van de preview. In deze zelfstudie wordt `westus2` gebruikt. Er kan alleen een failover tussen Azure-regio's worden uitgevoerd die geografische gekoppeld zijn. De regio die geografisch met westus2 is gekoppeld, is WestCentralUS.
    
   > [!NOTE]
   > Handmatige failover is momenteel in openbare preview en is *niet* beschikbaar in de volgende Azure-regio's: US - oost, US - west, Noord-Europa, West-Europa, Brazilië-Zuid en US - zuid-centraal.

   **Naam IoT-hub**: geef een naam op voor de IoT-hub. De naam van de hub moet wereldwijd uniek zijn. 

   ![Schermopname met het deelvenster Basis voor het maken van een IoT-hub](./media/tutorial-manual-failover/create-hub-02-basics.png)

   Klik op **Controleren + maken**. (De standaardwaarden voor grootte en schaal worden gebruikt.) 

4. Controleer de informatie en klik vervolgens op **Maken** om de IoT-hub te maken. 

   ![Schermopname met de laatste stap voor het maken van een IoT-hub](./media/tutorial-manual-failover/create-hub-03-create.png)

## <a name="perform-a-manual-failover"></a>Handmatige failover uitvoeren

Er kunnen maximaal twee failovers en twee failbacks per dag voor een IoT-hub worden uitgevoerd.

1. Klik op **Resourcegroepen** en selecteer resourcegroep **ManlFailRG**. Klik in de lijst met resources op uw hub. 

2. Klik in het deelvenster IoT Hub onder **Tolerantie** op **Handmatige failover (preview)**. Als uw hub niet op een veilige regio is ingesteld, wordt de optie Handmatige failover uitgeschakeld.

   ![Schermafbeelding met het venster IoT Hub-eigenschappen](./media/tutorial-manual-failover/trigger-failover-01.png)

3. Op het deelvenster Handmatige failover ziet u **Primaire locatie IoT Hub** en **Secundaire locatie IoT Hub**. De primaire locatie wordt in eerste instantie ingesteld op de locatie die u hebt opgegeven toen u de IoT-hub maakte. Deze geeft altijd de locatie aan waarin de hub momenteel actief is. De secundaire locatie is de standaard-Azure locatie die [geografisch is gekoppeld](../best-practices-availability-paired-regions.md) met de primaire locatie. U kunt de locatiewaarden niet wijzigen. Voor deze zelfstudie is `westus2` de primaire locatie en `WestCentralUS` de secundaire locatie.

   ![Schermafbeelding met het deelvenster Handmatige failover](./media/tutorial-manual-failover/trigger-failover-02.png)

3. Klik boven aan het deelvenster Handmatige failover op **Failover starten**. U ziet het deelvenster **Handmatige failover bevestigen**. Vul de naam in van uw IoT-hub om te bevestigen dat u hiervoor failover wilt uitvoeren. Klik vervolgens op **OK** om de failover te starten.

   De tijd die nodig is om de handmatige failover uit te voeren is evenredig met het aantal apparaten die voor uw hub zijn geregistreerd. Als u bijvoorbeeld 100.000 apparaten hebt, dan kan het vijftien minuten duren, maar als u vijf miljoen apparaten hebt, dan kan het minstens een uur duren.

4. Vul in het deelvenster **Handmatige failover bevestigen** de naam in van uw IoT-hub om te bevestigen dat u hiervoor failover wilt uitvoeren. Klik vervolgens op OK om de failover te starten. 

   ![Schermafbeelding met het deelvenster Handmatige failover](./media/tutorial-manual-failover/trigger-failover-03-confirm.png)

   Terwijl de handmatige failover wordt uitgevoerd, verschijnt er een banner boven het deelvenster Manual failover waarin staat aangegeven dat er een handmatige failover wordt uitgevoerd. 

   ![Schermafbeelding waarin wordt aangegeven dat een handmatige failover wordt uitgevoerd](./media/tutorial-manual-failover/trigger-failover-04-in-progress.png)

   Als u het deelvenster IoT Hub sluit en opnieuw opent door op het deelvenster Resourcegroep te klikken, ziet u een banner waarin wordt aangegeven dat de hub niet actief is. 

   ![Schermafbeelding waarin wordt aangegeven dat IoT Hub inactief is](./media/tutorial-manual-failover/trigger-failover-05-hub-inactive.png)

   Als de handmatige failover is voltooid, worden de primaire en secundaire regio's op de pagina Manual failover omgedraaid en is de hub weer actief. In dit voorbeeld is `WestCentralUS` nu de primaire locatie en `westus2` de secundaire locatie. 

   ![Schermafbeelding waarin wordt aangegeven dat de failover is voltooid](./media/tutorial-manual-failover/trigger-failover-06-finished.png)

## <a name="perform-a-failback"></a>Failback uitvoeren 

Nadat u een handmatige failover hebt uitgevoerd, kunt u de bewerkingen van de hub terugschakelen naar de oorspronkelijke primaire regio. Dit wordt een failback genoemd. Als u net een failover hebt uitgevoerd, moet u ongeveer een uur wachten voordat u een failback kunt aanvragen. Als u de failback eerder wilt uitvoeren, wordt er een foutbericht weergegeven.

Een failback wordt op dezelfde manier uitgevoerd als een handmatige failover. Dit zijn de stappen: 

1. Als u een failback wilt uitvoeren, gaat u terug naar het deelvenster IoT Hub voor uw IoT-hub.

2. Klik in het deelvenster IoT Hub onder **Tolerantie** op **Handmatige failover (preview)**. 

3. Klik boven aan het deelvenster Handmatige failover op **Failover starten**. U ziet het deelvenster **Handmatige failover bevestigen**. 

4. Vul in het deelvenster **Handmatige failover bevestigen** de naam in van uw IoT-hub om te bevestigen dat u hiervoor failback wilt uitvoeren. Klik op OK om de failback te starten. 

   ![Schermafbeelding met de aanvraag voor handmatige failback](./media/tutorial-manual-failover/trigger-failback-01-regions.png)

   De banners worden weergegeven zoals uitgelegd in de sectie Failover uitvoeren. Als de failback is voltooid, wordt hier opnieuw `westus2` als primaire locatie en `WestCentralUS` als secundaire locatie getoond, zoals oorspronkelijk ingesteld.

## <a name="clean-up-resources"></a>Resources opschonen 

Als u de resources die u in deze zelfstudie hebt gemaakt, wilt verwijderen, verwijdert u de resourcegroep. Hiermee verwijdert u ook alle resources in de groep. In dit geval worden de IoT-hub en de resourcegroep zelf verwijderd. 

1. Klik op **Resourcegroepen**. 

2. Selecteer de resourcegroep **ManlFailRG**. Klik erop om het te openen. 

3. Klik op **Resourcegroep verwijderen**. Als u de naam van de resourcegroep wordt gevraagd, voert u deze in en klikt u op **Verwijderen** ter bevestiging. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een handmatige failover moet configureren en uitvoeren en hoe u een failback kunt aanvragen door de volgende taken uit te voeren:

> [!div class="checklist"]
> * Een IoT-hub maken met behulp van Azure Portal. 
> * Een failover uitvoeren. 
> * De hub uitgevoerd zien worden in de secundaire locatie.
> * Een failback uitvoeren om de bewerkingen van de IoT-hub in de primaire locatie terug te plaatsen. 
> * Bevestigen dat de hub op de juiste locatie wordt uitgevoerd.

Ga door naar de volgende zelfstudie voor informatie over het beheren van de toestand van een IoT-apparaat. 

> [!div class="nextstepaction"]
[De status van een IoT-apparaat beheren](tutorial-device-twins.md)
