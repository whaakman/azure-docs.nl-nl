---
title: Zelfstudie over installeren van fysiek Azure Data Box Edge-apparaat | Microsoft Docs
description: De tweede zelfstudie over het installeren van Azure Data Box Edge gaat over het uitpakken, in een rek monteren en het bekabelen van het fysieke apparaat.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 21ac3de793f5ce559c3a03de2a09f11ccb86b12a
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167355"
---
# <a name="tutorial-install-azure-data-box-edge-preview"></a>Zelfstudie: Azure Data Box Edge (preview-versie) installeren

In deze zelfstudie wordt beschreven hoe u een fysiek Data Box Edge-apparaat installeert. De installatieprocedure omvat het uitpakken, in een rek monteren en bekabelen van het apparaat. 

De installatie kan ongeveer twee uur in beslag nemen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Uitpakken van apparaat
> * In rek monteren van apparaat
> * Apparaat bekabelen

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert.

## <a name="prerequisites"></a>Vereisten

De vereisten voor het installeren van een fysiek apparaat zijn als volgt.

### <a name="for-the-data-box-edge-resource"></a>Voor de Data Box Edge-resource

Zorg voordat u begint voor het volgende:

* U hebt alle stappen in [Prepare the portal for Data Box Edge](data-box-edge-deploy-prep.md) (De portal voorbereiden voor Data Box Edge) uitgevoerd.
    * U hebt een Data Box Edge-resource gemaakt voor het implementeren van uw apparaat.
    * U hebt de activeringscode gegenereerd voor het activeren van uw apparaat met de Data Box Edge-resource.

 
### <a name="for-the-data-box-edge-physical-device"></a>Voor het fysieke Data Box Edge-apparaat

Voordat u een apparaat implementeert, controleert u het volgende:

- Het apparaat staat veilig op een vlak, stabiel en horizontaal werkoppervlak (of vergelijkbaar).
- De locatie site waar u het apparaat wilt opstellen, voldoet aan deze voorwaarden:
    - Standaardnetstroom aanwezig van een onafhankelijke bron, of
    - Een PDU (Power Distribution Unit) aanwezig met een noodvoeding (UPS).
- Een vrij 1U-slot in het rek waarin u het apparaat wilt monteren.

### <a name="for-the-network-in-the-datacenter"></a>Voor het netwerk in het datacenter

Voordat u begint:

- Controleer de netwerkvereisten voor het implementeren van een Data Box Edge en configureer het netwerk van het datacenter aan de hand van die vereisten. Zie [Netwerkvereisten voor Data Box Edge](data-box-gateway-system-requirements.md#networking-requirements) voor meer informatie.
- Voor een optimale werking van het apparaat heeft internet een minimale bandbreedte van 20 Mbps nodig.


## <a name="unpack-the-device"></a>Het apparaat uitpakken

Dit apparaat wordt verzonden in één doos. Voer de volgende stappen uit om het apparaat uit te pakken. 

1. Leg de doos op een vlak, horizontaal oppervlak.
2. Controleer de doos en het verpakkingsmateriaal op deuken, scheuren, waterschade of andere duidelijk zichtbare schade. Als de doos of het verpakkingsmateriaal ernstig beschadigd is, moet u de doos niet openen. Neem contact op met Microsoft Support om in overleg te bepalen of het apparaat nog in goede staat zal zijn.
3. Pak de doos uit. Controleer of de inhoud van de doos bestaat uit de volgende items:
    - Eén Edge-apparaat met enkele behuizing
    - Twee netsnoeren
    - Kit voor rekmontage zonder gebruik van gereedschap (inclusief twee zijrails en bevestigingsmateriaal)
4. Als een of meer van de bovenstaande items ontbreken, neemt u contact op met Data Box Edge Support. De volgende stap is het in het rek monteren van uw apparaat.


## <a name="rack-the-device"></a>Het apparaat in het rek monteren

Het apparaat moet worden geïnstalleerd in een standaard 19-inch rek. Gebruik de volgende procedure om het apparaat te monteren in een standaard 19-inch rek met voor- en achterstijlen.

> [!IMPORTANT]
> Data Box Edge-apparaten moeten voor een goede werking in een rek worden gemonteerd.


1. Schuif het palletje in de richting van de pijl om de binnenste rail los te maken van de geleider. Ontgrendel het palletje en druk de middelste rail naar binnen om de rail in te trekken. De binnenste en buitenste rails moeten nu van elkaar zijn gescheiden.

    ![Montagerails installeren](./media/data-box-edge-deploy-install/rack-mount-rail-1.png)

2. Plaats de buitenste rails nu de verticale elementen van het rek. Om te helpen met een juiste montage, staat er Front op de geleiders en die kant moet naar de voorkant van de behuizing worden gericht. 
    
    1. Zoek de pennen van de rails aan voor- en achterzijde van de rail. Maak de rail langer zodat deze tussen de stijlen van het rek past. Bevestig de buitenste rail eerst aan de achterzijde van het rek. Plaats de achterste bevestigingsbeugel zo dat deze binnen de achterste montagegaten van het rek valt.   
    2. Houd het palletje op de achterste beugel ingedrukt om de metalen haken bloot te leggen. Laat deze in de montagegaten vallen en laat het palletje los.
    3. Lijn de voorste beugel uit met het montagegat.
    4. De voorste beugel moet nu zijn vastgezet aan het rek. U kunt desgewenst M5 X 10L-schroeven gebruiken om de rails met stijlen vast te zetten, indien nodig. 

    ![Montagerails installeren](./media/data-box-edge-deploy-install/rack-mount-rail-2.png)

3. Om de binnenste rails aan het chassis te bevestigen, moeten de spiegaten op de binnenste rail zijn uitgelijnd met de opsluitpennen aan de zijkant van het chassis. Zorg ervoor dat de koppen van de pennen van het chassis via de spiegaten in de binnenste rail steken. Trek de rail naar de voorzijde van het chassis totdat u de rail hoort vastklikken. Herhaal dit voor de andere binnenste rail. Duw het chassis met de binnenste rail in de geleider om de montage te voltooien.

    ![Montagerails installeren](./media/data-box-edge-deploy-install/rack-mount-rail-3.png)

## <a name="cable-the-device"></a>Het apparaat bekabelen

In de volgende procedures wordt uitgelegd hoe uw Edge-apparaat kunt bekabelen voor stroomtoevoer en aansluiting op het netwerk.

## <a name="prerequisites"></a>Vereisten

Voordat u begint met de bekabeling van uw apparaat hebt u het volgende nodig:

- Uw fysieke Edge-apparaat, uitgepakt en in het rek gemonteerd.
- Twee netsnoeren. 
- Twee 1 GbE-RJ-45-netwerkkabels en vier 25-GbE SFP + koperen kabels.
- Toegang tot twee Power Distribution Units (aanbevolen).

Uw Edge-apparaat heeft 8 SSD's van het type NVMe. Het voorpaneel bevat ook status-LED's en aan/uit-knoppen. Het apparaat is aan de achterzijde voorzien van redundante Power Supply Units (PSU's). Uw apparaat heeft zes netwerkinterfaces: twee 1-Gbps interfaces en vier 25-Gbps interfaces. Het apparaat is uitgerust met een Baseboard Management Controller (BMC). Hieronder ziet u de verschillende poorten en aansluitingen op het achterpaneel van het apparaat.
 
  ![Achterpaneel van een bekabeld apparaat](./media/data-box-edge-deploy-install/backplane-cabled.png)
 
Voer de volgende stappen uit om uw apparaat te bekabelen voor stroom en netwerk.

1. Sluit de netsnoeren aan op de PSU's in de behuizing. Voor een hoge beschikbaarheid moet u beide PSU's verbinden met verschillende stopcontacten.
2. Sluit de netsnoeren aan op de PDU's van het rek. Zorg ervoor dat de twee PSU's op verschillende stopcontacten zijn aangesloten.
3. Verbind PORT 1 van de 1-GbE netwerkinterface met de computer die wordt gebruikt voor het configureren van het fysieke apparaat. PORT 1 is de gereserveerde beheerinterface.
4. Verbind PORT 2 van de 1-GbE netwerkinterface via de RJ-45-netwerkkabels met het netwerk van het datacenter/internet. 
5. Verbind de vier 25-GbE-netwerkinterfaces PORT 3, PORT 4, PORT 5 en PORT 6 met SFP + koperen kabels met het netwerk van het datacenter/internet. 

> [!NOTE]
> - Ten minste één netwerkinterface, PORT 3, PORT 4, PORT 5 of PORT 6 moet worden verbonden met internet (verbindingen naar Azure). 
> - Het wordt aangeraden om een 25-GbE netwerkinterface, zoals PORT 3, PORT 4, PORT 5, of PORT 6 te gebruiken om gegevens te verzenden naar Azure. 
> - Het Edge-apparaat moet worden aangesloten op het netwerk van het datacenter, zodat het gegevens kan opnemen van gegevensbronservers.  


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Data Box Edge, zoals:

> [!div class="checklist"]
> * Het apparaat uitpakken
> * Het apparaat in het rek monteren
> * Het apparaat bekabelen

Ga naar de volgende zelfstudie voor informatie over het verbinden, instellen en activeren van uw apparaat.

> [!div class="nextstepaction"]
> [Data Box Edge aansluiten en instellen](./data-box-edge-deploy-connect-setup-activate.md)


