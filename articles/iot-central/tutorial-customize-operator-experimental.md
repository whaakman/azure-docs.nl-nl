---
title: De operatorweergaven aanpassen in Azure IoT Central | Microsoft Docs
description: Pas de operatorweergaven in uw Azure IoT Central-toepassing aan.
author: sandeeppujar
ms.author: sandeepu
ms.date: 01/29/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 267a619fe32a8d4af0ee9cc8a5001d7a321c3098
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55765145"
---
# <a name="tutorial-customize-the-azure-iot-central-operators-view-new-ui-design"></a>Zelfstudie: De operatorweergave van Azure IoT Central aanpassen (nieuw ontwerp van gebruikersinterface)

In deze zelfstudie leert u, als systeembouwer, hoe u de operatorweergave van uw toepassing kunt aanpassen. Wanneer u als systeembouwer een wijziging in de toepassing aanbrengt, kunt u een voorbeeld van de operatorweergave in de Microsoft Azure IoT Central-toepassing bekijken.

In deze zelfstudie past u de toepassing aan om relevante informatie over het verbonden airco-apparaat weer te geven aan een operator. Dankzij uw aanpassingen kan de operator de airco-apparaten beheren die met de toepassing verbonden zijn.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Uw apparaatdashboard configureren
> * De indeling van uw apparaatinstellingen configureren
> * De indeling van uw apparaateigenschappen configureren
> * Voorbeeld van het apparaat weergeven als operator
> * De standaardstartpagina configureren
> * Voorbeeld van de standaardstartpagina weergeven als operator

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u de twee vorige zelfstudies uitvoeren:

* [Een nieuw apparaattype definiëren in uw Azure IoT Central-toepassing](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
* [Regels en acties voor uw apparaat configureren](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="configure-your-device-dashboard"></a>Uw apparaatdashboard configureren

Als systeembouwer kunt u bepalen welke informatie in een apparaatdashboard wordt weergegeven. In de zelfstudie [Een nieuw apparaattype definiëren in uw toepassing](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) hebt u een lijndiagram en andere informatie toegevoegd aan het dashboard **Connected Air Conditioner**.

1. U kunt de apparaatsjabloon **Connected Air Conditioner** bewerken door in het linkernavigatiemenu **Apparaatsjablonen** te kiezen:

    ![De pagina Apparaatsjablonen](media/tutorial-customize-operator-experimental/devicetemplates.png)

2. Klik op de apparaatsjabloon **Connected Air Conditioner (1.0.0)** die u hebt gemaakt in de zelfstudie [Een nieuw apparaattype definiëren in uw toepassing](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json), om het apparaatdashboard aan te passen.

3. Als u het dashboard wilt bewerken, selecteert u het tabblad **Dashboard**.

4. Als u een KPI-tegel (Key Performance Indicator) wilt toevoegen aan het dashboard, kiest u **KPI**:

    Voor het definiëren van de KPI kunt u de informatie in de volgende tabel gebruiken:

    | Instelling     | Waarde |
    | ----------- | ----- |
    | Naam        | Maximale temperatuur |
    | Tijdsbereik  | Afgelopen week 1 |
    | Metingtype | Telemetrie |
    | Meting | temperatuur |
    | Aggregatie | Maximum |
    | Zichtbaarheid  | Ingeschakeld |

    ![KPI toevoegen](media/tutorial-customize-operator-experimental/addkpi.png)

5. Klik op **Opslaan**. U ziet nu de KPI-tegel in het dashboard:

    ![KPI-tegel](media/tutorial-customize-operator-experimental/temperaturekpi.png)

6. Als u een tegel in het dashboard wilt verplaatsen of het formaat ervan wilt wijzigen, houdt u de muisaanwijzer boven de tegel. U kunt de tegel naar een nieuwe locatie slepen of het formaat van de tegel wijzigen.

## <a name="configure-your-settings-layout"></a>De indeling van uw instellingen configureren

Als systeembouwer kunt u ook de operatorweergave van de apparaatinstellingen configureren. Een operator gebruikt het tabblad Apparaatinstellingen om een apparaat te configureren. Een operator gebruikt het tabblad Instellingen bijvoorbeeld om de doeltemperatuur voor het verbonden airco-apparaat in te stellen.

1. Als u de indeling van de instellingen voor het verbonden airco-apparaat wilt bewerken, kiest u het tabblad **Instellingen**.

2. U kunt de tegels voor instellingen verplaatsen en vergroten of verkleinen:

    ![De indeling van de instellingen bewerken](media/tutorial-customize-operator-experimental/settingslayout.png)

## <a name="configure-your-properties-layout"></a>De indeling van de eigenschappen configureren

Naast het dashboard en instellingen kunt u ook de operatorweergave van de apparaateigenschappen configureren. Een operator maakt gebruik van het tabblad Apparaateigenschappen voor het beheren van metagegevens van apparaten. Een operator maakt bijvoorbeeld gebruik van het tabblad Eigenschappen om een serienummer van een apparaat te bekijken of om de contactgegevens van de fabrikant bij te werken.

1. Als u de indeling van de eigenschappen voor het verbonden airco-apparaat wilt bewerken, kiest u het tabblad **Eigenschappen**.

2. U kunt de eigenschapvelden verplaatsen en vergroten of verkleinen:

    ![De indeling van de eigenschappen bewerken](media/tutorial-customize-operator-experimental/propertieslayout.png)

## <a name="preview-the-connected-air-conditioner-device-as-an-operator"></a>Een voorbeeld van het verbonden airco-apparaat weergeven als een operator

U gebruikt de pagina **Apparaatsjablonen** om het dashboard, de instellingen, en het tabblad eigenschappen voor een operator aan te passen. U kunt de pagina **Apparaat** gebruiken om de apparaatsjabloon weer te geven en te gebruiken.

1. Als u het verbonden airco-apparaat als een operator wilt gebruiken, navigeert u naar de pagina **Device Explorer** en kiest u het gesimuleerde apparaat dat met IoT Central is gegenereerd uit de sjabloon:

    ![De apparaatsjabloon weergeven en gebruiken](media/tutorial-customize-operator-experimental/usetemplate.png)

2. Als u de locatie van dit apparaat wilt bijwerken, kiest u **Eigenschappen** en bewerkt u de waarde in de tegel Locatie. Klik vervolgens op **Opslaan**:

    ![Een eigenschapswaarde wijzigen](media/tutorial-customize-operator-experimental/editproperty.png)

3. Als u een instelling naar uw verbonden airco-apparaat wilt verzenden, kiest u **Instellingen**, wijzigt u een instellingswaarde in een tegel en kiest u **Bijwerken**:

    ![Instelling naar apparaat verzenden](media/tutorial-customize-operator-experimental/sendsetting.png)

    Wanneer het apparaat de waarde van de nieuwe instelling bevestigt, wordt de instelling weergegeven als **gesynchroniseerd** op de tegel.

4. Als operator kunt u het dashboard van het apparaat weergeven zoals deze is geconfigureerd door de systeembouwer:

    ![Operatorweergave van het dashboard van het apparaat](media/tutorial-customize-operator-experimental/operatordashboard.png)

## <a name="configure-the-default-home-page"></a>De standaardstartpagina configureren

Wanneer een systeembouwer of operator zich bij een Azure IoT Central-toepassing aanmeldt, wordt er een startpagina weergegeven. U kunt als systeembouwer de inhoud van deze startpagina configureren en de voor een operator meest nuttige en relevante inhoud opnemen.

1. Als u de standaardstartpagina wilt aanpassen, gaat u naar de **startpagina** en selecteert u rechtsboven op de pagina de optie **Bewerken**. Vanaf de linkerkant komt nu een paneel tevoorschijn met een lijst met objecten die u kunt toevoegen aan de **startpagina**:

    ![Pagina Application Builder](media/tutorial-customize-operator-experimental/builderhome.png)

2. Als u de **startpagina** wilt aanpassen, voegt u tegels toe vanuit de **Bibliotheek**. Kies **Koppeling** en voeg gegevens toe van de website van uw organisatie. Klik vervolgens op **Opslaan**:

    ![Koppeling naar startpagina toevoegen](media/tutorial-customize-operator-experimental/addlink.png)

    > [!NOTE]
    > U kunt ook koppelingen toevoegen voor pagina's in uw Azure IoT Central-toepassing. U kunt bijvoorbeeld een koppeling toevoegen voor een apparaatdashboard of instellingenpagina.

3. Kies desgewenst **Afbeelding** en upload een afbeelding die moet worden weergegeven op uw startpagina. Een afbeelding kan een URL hebben. Wanneer u op de URL klikt, gaat u naar de afbeelding:

    ![Afbeelding aan startpagina toevoegen](media/tutorial-customize-operator-experimental/addimage.png)

    Zie [Afbeeldingen voorbereiden en uploaden naar uw Azure IoT Central-toepassing](howto-prepare-images-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) voor meer informatie.

## <a name="preview-the-default-home-page-as-an-operator"></a>Voorbeeld van de standaardstartpagina weergeven als operator

Als u een voorbeeld van de startpagina wilt weergeven als een operator, klikt u rechtsboven op de pagina op **Gereed**:

![De ontwerpmodus in- en uitschakelen](media/tutorial-customize-operator-experimental/operatorviewhome.png)

U kunt op de koppeling en afbeeldingtegels klikken om te navigeren naar de URL's die u als systeembouwer instelt.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de operatorweergave van de toepassing kunt aanpassen.

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Uw apparaatdashboard configureren
> * De indeling van uw apparaatinstellingen configureren
> * De indeling van uw apparaateigenschappen configureren
> * Voorbeeld van het apparaat weergeven als operator
> * De standaardstartpagina configureren
> * Voorbeeld van de standaardstartpagina weergeven als operator

Nu u hebt geleerd hoe u de operatorweergave van de toepassing kunt aanpassen, zijn dit de voorgestelde volgende stappen:

* [Uw apparaten bewaken (als operator)](tutorial-monitor-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [Een nieuw apparaat toevoegen aan uw toepassing (als operator en ontwikkelaar van het apparaat)](tutorial-add-device-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
