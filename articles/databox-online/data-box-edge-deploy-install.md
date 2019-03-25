---
title: Zelfstudie over installeren van een fysiek Azure Data Box Edge-apparaat | Microsoft Docs
description: De tweede zelfstudie over het installeren van Azure Data Box Edge gaat over het uitpakken, het in een rek monteren en het bekabelen van het fysieke apparaat.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: a1357e92b868f85556fc4d665eb475abd095fece
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400003"
---
# <a name="tutorial-install-azure-data-box-edge"></a>Zelfstudie: Azure Data Box Edge installeren

In deze zelfstudie wordt beschreven hoe u een fysiek Data Box Edge-apparaat installeert. De installatieprocedure omvat het uitpakken, het in een rek monteren en het bekabelen van het apparaat. 

De installatie duurt ongeveer twee uur.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Het apparaat uitpakken
> * Het apparaat in een rek monteren
> * Het apparaat bekabelen

## <a name="prerequisites"></a>Vereisten

De vereisten voor het installeren van een fysiek apparaat zijn als volgt:

### <a name="for-the-data-box-edge-resource"></a>Voor de Data Box Edge-resource

Zorg voordat u begint voor het volgende:

* Hebt u alle stappen in [voorbereidingen voor het implementeren van Azure Data Box Edge](data-box-edge-deploy-prep.md).
    * U hebt een Data Box Edge-resource gemaakt voor het implementeren van uw apparaat.
    * U hebt de activeringscode gegenereerd voor het activeren van uw apparaat met de Data Box Edge-resource.

 
### <a name="for-the-data-box-edge-physical-device"></a>Voor het fysieke Data Box Edge-apparaat

Voordat u een apparaat implementeert, controleert u het volgende:

- Het apparaat staat veilig op een vlak, stabiel en horizontaal werkoppervlak.
- De locatie site waar u het apparaat wilt opstellen, voldoet aan deze voorwaarden:
    - Standaardnetstroom aanwezig van een onafhankelijke bron

        OF
    - Een PDU (Power Distribution Unit) aanwezig met een noodvoeding (UPS)
    - Een vrij 1U-slot in het rek waarin u het apparaat wilt monteren

### <a name="for-the-network-in-the-datacenter"></a>Voor het netwerk in het datacenter

Voordat u begint:

- Controleer de netwerkvereisten voor het implementeren van een Data Box Edge en configureer het netwerk van het datacenter aan de hand van die vereisten. Zie [Netwerkvereisten voor Data Box Edge](data-box-edge-system-requirements.md#networking-port-requirements) voor meer informatie.

- Voor een optimale werking van het apparaat heeft internet een minimale bandbreedte van 20 Mbps nodig.


## <a name="unpack-the-device"></a>Het apparaat uitpakken

Dit apparaat wordt verzonden in één doos. Voer de volgende stappen uit om het apparaat uit te pakken. 

1. Leg de doos op een vlak, horizontaal oppervlak.
2. Controleer de doos en het verpakkingsmateriaal op deuken, scheuren, waterschade of andere duidelijk zichtbare schade. Als de doos of het verpakkingsmateriaal ernstig beschadigd is, moet u de doos niet openen. Neem contact op met Microsoft Support om in overleg te bepalen of het apparaat nog in goede staat zal zijn.
3. Pak de doos uit. Controleer of de inhoud van de doos bestaat uit de volgende items:
    - Eén Edge-apparaat met enkele behuizing
    - Twee netsnoeren
    - Een spoor kit assembly
    - Veiligheid, milieu en wettelijke informatie brochure

Als een of meer van de bovenstaande items ontbreken, neemt u contact op met de Data Box Edge-ondersteuning. De volgende stap is het in het rek monteren van uw apparaat.


## <a name="rack-the-device"></a>Het apparaat in het rek monteren

Het apparaat moet worden geïnstalleerd in een standaard 19-inch rek. Gebruik de volgende procedure met rack uw apparaat op een standaard 19-inch rack koppelen.

> [!IMPORTANT]
> Data Box Edge-apparaten moeten voor een goede werking in een rek worden gemonteerd.


### <a name="prerequisites"></a>Vereisten

- Voordat u begint, lees de veiligheidsinstructies in uw veiligheid, milieu en wettelijke informatie brochure zijn. In deze handleiding is geleverd met het apparaat.
- Voordat de installatie van de rails in de toegewezen ruimte die zich het dichtst bij de onderkant van het rackbehuizing.
- Voor de configuratie van tooled spoor koppelen, moet u acht schroeven opgeven: #10-32, #12-24 uur per dag, #M5 of #M6. De kop diameter van de schroeven moet minder dan 10 mm (0,4-").

### <a name="identify-the-rail-kit-contents"></a>De inhoud van de kit spoor te identificeren

Zoek de onderdelen voor het installeren van de trein kit assembly:
1. Twee A7 Dell ReadyRails II schuiven spoor assembly 's
2. Twee haakje en lus banden

![Spoor kit inhoud te identificeren](./media/data-box-edge-deploy-install/identify-rail-kit-contents.png)

### <a name="install-and-remove-tool-less-rails-square-hole-or-round-hole-racks"></a>Installeren en verwijderen van gereedschap rails (vierkante hole of ronde openingen rekken)

1. Plaats de linker- en spoor end-onderdelen met het label **FRONT** naar binnen gerichte en elk gegeven end aan in de gaten op de front-zijde van de verticale rackflenzen plaatsen.
2. Elk onderdeel end in de gaten onderste en bovenste van de gewenste U spaties uitlijnen.
3. De back-end van de trein betrekken voor de seats op de verticale rack moer totdat het volledig en de vergrendeling op klikt in plaats. Herhaal deze stappen om te plaatsen en het stuk front-end in de verticale rack moer seat.
4. Voor het verwijderen van de rails, de vergrendeling release-knop op het middelpunt van end-deel ophalen en elke spoor unseat.

![Installeren en verwijderen van gereedschap rails](./media/data-box-edge-deploy-install/installing-removing-tool-less-rails.png)

### <a name="install-and-remove-tooled-rails-threaded-hole-racks"></a>Installeren en verwijderen van tooled rails (rekken hiërarchische hole)

1. De pincodes verwijderen uit de voor- en achterkant koppelen met behulp van een plat draai vierkante haken.
2. Ophalen van en het draaien van de trein vergrendeling subverzamelingen als u wilt verwijderen uit de haakjes koppelen.
3. Links en rechts koppelen rails op de front verticale rackflenzen met behulp van twee sets schroeven koppelen.
4. Schuif de linker- en achterkant vierkante haken doorsturen op basis van de achterste verticale rackflenzen en koppelt u ze met behulp van twee sets schroeven.

![Installeren en verwijderen van tooled rails](./media/data-box-edge-deploy-install/installing-removing-tooled-rails.png)

### <a name="install-the-system-in-a-rack"></a>Het systeem installeert op een rek

1. Haal de rails binnenste dia buiten het rack totdat ze vergrendeld in plaats.
2. Zoek de achterste spoor standoff voor elke zijde van het systeem en deze aan op de achterkant J-sleuven voor de assembly dia's verlagen. Het systeem omlaag draaien totdat alle standoffs van de trein worden geplaatst in de "j"-sleuven.
3. Push naar binnen het systeem-totdat de remhendels vergrendeling op naar de juiste plaats.
4. Druk op de knoppen van de vergrendeling dia-release op rails en dia het systeem in het rek te plaatsen.

![Systeem installeren in een rack](./media/data-box-edge-deploy-install/installing-system-rack.png)

### <a name="remove-the-system-from-the-rack"></a>Het systeem verwijderen uit het rek

1. Zoek de vergrendeling remhendels aan de zijde van de binnenste rails.
2. Elke hendel ontgrendelen roteren tot de positie van de release.
3. Het aantal zijden van het systeem goed rapportelementen en naar voren halen totdat het spoor standoffs aan het begin van de "j"-sleuven zijn. Het systeem optillen up en er vandaan lopen van het rack en plaats het op een niveau voor aanvallen.

![Systeem verwijderen uit het rek](./media/data-box-edge-deploy-install/removing-system-rack.png)

### <a name="engage-and-release-the-slam-latch"></a>Neem contact op en de vergrendeling slam vrijgeven

OPMERKING: Voor systemen die niet zijn uitgerust met slam vergrendelingen, het systeem met schroeven, worden beveiligd zoals beschreven in stap 3 van deze procedure.

1. De front-end gerichte, de vergrendeling slam Zoek aan beide zijden van het systeem.
2. De vergrendelingen betrekken automatisch als het systeem in het rack is gepusht en door te halen van op de vergrendelingen zijn vrijgegeven.
3. Voor het beveiligen van het systeem voor verzending in het rack of voor andere instabiel omgevingen, zoek de schroeven harde koppeling onder elke vergrendeling en elke installatie met een #2 webserverbeheerders kruiskopschroevendraaier.

![Neem contact op en release slam vergrendeling](./media/data-box-edge-deploy-install/engaging-releasing-slam-latch.png)

### <a name="route-the-cables"></a>De kabels routeren

> [!NOTE]
>  Als u niet de optionele kabel Management Arm (CMA) bestellen hebt, gebruikt u de twee hook en lus banden opgegeven in het spoor kit voor het routeren van de kabels aan het einde van uw systeem.

1. Zoek de buitenste CMA vierkante haken aan de binnenkant zijden van beide rackflenzen.
2. De kabels voorzichtig, bundelen binnenhalen ze wissen van de systeem-connectors voor de links en rechts.
3. De banden haakje en lus via de tooled sleuven op het buitenste CMA vierkante haken voor elke zijde van het systeem voor het beveiligen van de kabel bundels van thread.


![De kabels routeren](./media/data-box-edge-deploy-install/routing-cables.png)

## <a name="cable-the-device"></a>Het apparaat bekabelen

In de volgende procedures wordt uitgelegd hoe uw Edge-apparaat kunt bekabelen voor stroomtoevoer en aansluiting op het netwerk.

Voordat u begint met de bekabeling van uw apparaat, zorgt u ervoor dat u over de volgende zaken beschikt:

- Uw fysieke Edge-apparaat, uitgepakt en in het rek gemonteerd.
- Twee netsnoeren. 
- Ten minste één 1-GbE RJ-45-netwerkkabel voor verbinding met de beheerinterface. Er zijn twee 1-GbE-netwerkinterfaces, één beheerinterface en één gegevensinterface, op het apparaat aanwezig.
- Eén koperen 25-GbE SFP+-kabel voor elke te configureren netwerkinterface. Ten minste één netwerkinterface (PORT 2, PORT 3, PORT 4, PORT 5 of PORT 6) moet worden verbonden met internet (en met Azure).  
- Toegang tot twee Power Distribution Units (aanbevolen).

> [!NOTE]
> - Als u slechts één netwerkinterface van de gegevens zijn verbonden, wordt u aangeraden dat u een netwerkinterface van 25/10-GbE, zoals poort 3, 4-poort, poort 5 of 6 poort gebruiken om gegevens te verzenden naar Azure. 
> - Voor de beste prestaties en voor het verwerken van grote gegevensvolumes, kunt u eventueel alle gegevenspoorten verbinden.
> - Het Edge-apparaat moet worden aangesloten op het netwerk van het datacenter, zodat het gegevens kan opnemen van gegevensbronservers.

Uw Edge-apparaat heeft 8 SSD's van het type NVMe. Het voorpaneel bevat ook status-LED's en aan/uit-knoppen. Het apparaat is aan de achterzijde voorzien van redundante Power Supply Units (PSU's). Uw apparaat heeft zes netwerkinterfaces:

- Twee 1 Gbps-interfaces
- Vier 25 Gbps-interfaces die ook als 10 Gbps-interfaces dienen kunnen.
- Een baseboard management controller (BMC). 

Hieronder ziet u de verschillende poorten en aansluitingen op het achterpaneel van het apparaat.
 
  ![Achterpaneel van een bekabeld apparaat](./media/data-box-edge-deploy-install/backplane-cabled.png)

Het apparaat heeft twee netwerkkaarten die overeenkomt met de 6-poorten: 

 - QLogic FastLinQ 41264
 - QLogic FastLinQ 41262

Voor een volledige lijst van ondersteunde kabels en switches infraroodzenders voor deze netwerkkaarten, gaat u naar [Cavium FastlinQ 41000 reeks interoperabiliteit Matrix](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).
 
Voer de volgende stappen uit om uw apparaat te bekabelen voor stroom en verbinding met het netwerk.

1. Sluit de netsnoeren aan op de PSU's in de behuizing. Voor een hoge beschikbaarheid moet u beide PSU's verbinden met verschillende stopcontacten.

2. Sluit de netsnoeren aan op de PDU's van het rek. Zorg ervoor dat de twee PSU's op verschillende stopcontacten zijn aangesloten.

3. Verbind PORT 1 van de 1-GbE netwerkinterface met de computer die wordt gebruikt voor het configureren van het fysieke apparaat. PORT 1 is de gereserveerde beheerinterface.

4. Verbind één of meer interfaces, bijvoorbeeld PORT 2, PORT 3, PORT 4, PORT 5 of PORT 6 aan het netwerk van het datacenter of internet. Gebruik de RJ-45-netwerkkabel als u PORT 2 verbindt. Gebruik de kabels SFP + koperen voor de 25-10-GbE-netwerkinterfaces.  

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Data Box Edge, zoals:

> [!div class="checklist"]
> * Het apparaat uitpakken
> * Het apparaat in het rek monteren
> * Het apparaat bekabelen

Ga naar de volgende zelfstudie voor informatie over het verbinden, instellen en activeren van uw apparaat.

> [!div class="nextstepaction"]
> [Data Box Edge aansluiten en instellen](./data-box-edge-deploy-connect-setup-activate.md)


