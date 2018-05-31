---
title: De operatorweergaven aanpassen in Azure IoT Central | Microsoft Docs
description: Pas de operatorweergaven in uw Azure IoT Central-toepassing aan.
services: iot-central
author: sandeeppujar
ms.author: sadeepu
ms.date: 04/16/2018
ms.topic: tutorial
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 3e0dfab05fc7972a055853af45f0d1b13d52c0a1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34202319"
---
# <a name="3---customize-the-azure-iot-central-operators-view"></a>3 - De operatorweergave van Azure IoT Central aanpassen

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

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u de twee vorige zelfstudies uitvoeren:

1. [Een nieuw apparaattype definiëren in uw Azure IoT Central-toepassing](tutorial-define-device-type.md).
1. [Regels en acties voor uw apparaat configureren](tutorial-configure-rules.md).

## <a name="configure-your-device-dashboard"></a>Uw apparaatdashboard configureren

Als systeembouwer kunt u bepalen welke informatie in een apparaatdashboard wordt weergegeven. In de zelfstudie [Een nieuw apparaattype definiëren in uw toepassing](tutorial-define-device-type.md) hebt u een lijndiagram en andere informatie toegevoegd aan het dashboard **Connected Air Conditioner-1**.

1. U kunt de apparaatsjabloon **Connected Air Conditioner** bewerken door **Explorer** in het navigatiemenu links te kiezen:

    ![Device Explorer-pagina](media/tutorial-customize-operator/explorer.png)

1. Als u het dashboard van het verbonden airco-apparaat wilt aanpassen, selecteert u de apparaatsjabloon **Connected Air Conditioner (1.0.0)**. Kies het apparaat **Connected Air Conditioner-1** dat u hebt gemaakt in de zelfstudie [Een nieuw apparaattype definiëren in uw toepassing](tutorial-define-device-type.md):

    ![Selecteer het verbonden airco-apparaat](media/tutorial-customize-operator/selectdevice.png)

    Wanneer u een wijziging aanbrengt aan een apparaat, zoals **Connected Air Conditioner-1**, maakt u een wijziging in de onderliggende sjabloon. Zie [Een nieuwe versie van de apparaatsjabloon maken](howto-version-devicetemplate.md) voor meer informatie.

1. Als u het dashboard wilt bewerken, kiest u **Dashboard**:

    ![Dashboardpagina apparaatsjabloon](media/tutorial-customize-operator/dashboard.png)

1. Als u een KPI-tegel wilt toevoegen aan het dashboard, kiest u **KPI**:

    ![KPI toevoegen](media/tutorial-customize-operator/addkpi.png)

    Voor het definiëren van de KPI kunt u de informatie in de volgende tabel gebruiken:

    | Instelling     | Waarde |
    | ----------- | ----- |
    | Naam        | Maximale temperatuur |
    | Meting | temperatuur |
    | Aggregatie | Maximum |
    | Tijdsbereik  | Afgelopen week 1 |

1. Kies **Opslaan**. U ziet nu de KPI-tegel in het dashboard:

    ![KPI-tegel](media/tutorial-customize-operator/temperaturekpi.png)

1. Als u een tegel in het dashboard wilt verplaatsen of het formaat ervan wilt wijzigen, houdt u de muisaanwijzer boven de tegel. U kunt de tegel naar een nieuwe locatie slepen of het formaat van de tegel wijzigen:

    ![Dashboardindeling bewerken](media/tutorial-customize-operator/dashboardlayout.png)

## <a name="configure-your-settings-layout"></a>De indeling van uw instellingen configureren

Als systeembouwer kunt u ook de operatorweergave van de apparaatinstellingen configureren. Een operator gebruikt de pagina met apparaatinstellingen om een apparaat te configureren. Een operator gebruikt de instellingenpagina bijvoorbeeld om de doeltemperatuur voor het koelapparaat in te stellen.

1. Als u de indeling van de instellingen voor het verbonden airco-apparaat wilt bewerken, kiest u **Instellingen**:

    ![De pagina Instellingen](media/tutorial-customize-operator/settings.png)

1. U kunt de tegels voor instellingen verplaatsen en vergroten of verkleinen:

    ![De indeling van de instellingen bewerken](media/tutorial-customize-operator/settingslayout.png)

> [!NOTE]
> In de **ontwerpmodus** kunt u niet de waarden van de instellingen bewerken.

## <a name="configure-your-properties-layout"></a>De indeling van de eigenschappen configureren

Naast het dashboard en instellingen kunt u ook de operatorweergave van de apparaateigenschappen configureren. Een operator maakt gebruik van de pagina met apparaateigenschappen voor het beheren van metagegevens van apparaten. Een operator maakt bijvoorbeeld gebruik van de eigenschappenpagina om een serienummer van een apparaat te bekijken of om de contactgegevens van de fabrikant bij te werken.

1. Als u de indeling van de eigenschappen voor het verbonden airco-apparaat wilt bewerken, kiest u **Eigenschappen**:

    ![De pagina Eigenschappen](media/tutorial-customize-operator/properties.png)

1. U kunt de eigenschapvelden verplaatsen en vergroten of verkleinen:

    ![De indeling van de eigenschappen bewerken](media/tutorial-customize-operator/propertieslayout.png)

> [!NOTE]
> In de **ontwerpmodus** kunt u niet de waarden van de eigenschappen bewerken.

## <a name="preview-the-connected-air-conditioner-device-as-an-operator"></a>Een voorbeeld van het verbonden airco-apparaat weergeven als een operator

In de **ontwerpmodus** kunt u de pagina's van het dashboard, de instellingen en de eigenschappen voor een operator aanpassen. Als u de **ontwerpmodus** uitschakelt, kunt u de toepassing als een operator bekijken.

1. Als u het verbonden airco-apparaat als een operator wilt weergeven, moet u de **ontwerpmodus** uitschakelen. Als u de **ontwerpmodus** wilt uitschakelen, schakelt uit **Ontwerpmodus** rechtsboven op de pagina uit.

1. Als u het serienummer van dit apparaat wilt bijwerken, bewerkt u de waarde in de tegel met het serienummer en kiest u **Opslaan**:

    ![Een eigenschapswaarde wijzigen](media/tutorial-customize-operator/editproperty.png)

1. Als u een instelling naar uw verbonden airco-apparaat wilt verzenden, kiest u **Instellingen**, wijzigt u een instellingswaarde in een tegel en kiest u **Bijwerken**:

    ![Instelling naar apparaat verzenden](media/tutorial-customize-operator/sendsetting.png)

    Wanneer het apparaat de waarde van de nieuwe instelling bevestigt, wordt de instelling weergegeven als **gesynchroniseerd** op de tegel.

1. Als operator kunt u het dashboard van het apparaat weergeven zoals deze is geconfigureerd door de systeembouwer:

    ![Operatorweergave van het dashboard van het apparaat](media/tutorial-customize-operator/operatordashboard.png)

## <a name="configure-the-default-home-page"></a>De standaardstartpagina configureren

Wanneer een systeembouwer of operator zich bij een Azure IoT Central-toepassing aanmeldt, wordt er een startpagina weergegeven. U kunt als systeembouwer de inhoud van deze startpagina configureren en de voor een operator meest nuttige en relevante inhoud opnemen.

1. Voor het aanpassen van de standaardstartpagina, gaat u naar de **Start**pagina en schakelt u **Ontwerpmodus** in rechtsboven op de pagina. Na het inschakelen van de **ontwerpmodus** schuift vanaf rechts een venster naar buiten met een lijst met objecten die u aan uw startpagina kunt toevoegen.

    ![Pagina Application Builder](media/tutorial-customize-operator/builderhome.png)

1. Voor het aanpassen van de startpagina voegt u tegels uit de **Bibliotheek** toe. Kies **Koppeling** en voeg gegevens toe van de website van uw organisatie. Kies vervolgens **Opslaan**:

    ![Koppeling naar startpagina toevoegen](media/tutorial-customize-operator/addlink.png)

    > [!NOTE]
    > U kunt ook koppelingen toevoegen voor pagina's in uw Azure IoT Central-toepassing. U kunt bijvoorbeeld een koppeling toevoegen voor een apparaatdashboard of instellingenpagina.

1. Kies desgewenst **Afbeelding** en upload een afbeelding die moet worden weergegeven op uw startpagina. Een afbeelding kan een URL hebben. Wanneer u op de URL klikt, gaat u naar de afbeelding:

    ![Afbeelding aan startpagina toevoegen](media/tutorial-customize-operator/addimage.png)

    Zie [Afbeeldingen voorbereiden en uploaden naar uw Azure IoT Central-toepassing](howto-prepare-images.md) voor meer informatie.

## <a name="preview-the-default-home-page-as-an-operator"></a>Voorbeeld van de standaardstartpagina weergeven als operator

Als u een voorbeeld van de startpagina wilt weergeven als een operator, schakelt u **Ontwerpmodus** in de rechterbovenhoek van de pagina uit:

![De ontwerpmodus in- en uitschakelen](media/tutorial-customize-operator/operatorviewhome.png)

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

* [Uw apparaten bewaken (als operator)](tutorial-monitor-devices.md)
* [Een nieuw apparaat toevoegen aan uw toepassing (als operator en ontwikkelaar van het apparaat)](tutorial-add-device.md)
