---
title: De Azure Data Box instellen | Microsoft Docs
description: Meer informatie over de software- en netwerkvereisten voor de Azure Data Box
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 6623744a26d79ccbc8b68ba84baf3a37024e0ed7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46952304"
---
# <a name="tutorial-cable-and-connect-to-your-azure-data-box"></a>Zelfstudie: De Azure Data Box bekabelen en aansluiten

In deze zelfstudie wordt beschreven hoe u de Azure Data Box bekabelt, aansluit en inschakelt.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De Data Box bekabelen
> * De Data Box aansluiten

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

1. U hebt de [zelfstudie: Azure Data Box bestellen](data-box-deploy-ordered.md) voltooid.
2. U hebt uw Data Box ontvangen en de bestelstatus in de portal is bijgewerkt naar **Geleverd**. 
3. U hebt de [Data Box-veiligheidsrichtlijnen](data-box-safety.md) bekeken.
4. U hebt één geaard netsnoer voor gebruik met uw opslagapparaat van 100 TB ontvangen.
5. U hebt een hostcomputer waarop de gegevens staan die u naar de Data Box wilt kopiëren. Op uw hostcomputer moet
    - Een [ondersteund besturingssysteem](data-box-system-requirements.md) worden uitgevoerd.
    - Verbinding zijn met een netwerk met hoge snelheid. Het wordt aangeraden dat u ten minste één 10 GbE-verbinding hebt. Als er geen 10 GbE-verbinding beschikbaar is, kan een 1 GbE-gegevenskoppeling worden gebruikt. Dit heeft echter wel invloed op de kopieersnelheid. 
6. U moet de beschikking hebben over een plat oppervlak waarop u de Data Box kunt plaatsen. Als u het apparaat op een plank van een standaardrek wilt plaatsen, moet het datacenterrek beschikken over een 7U-sleuf. U kunt het apparaat plat of rechtop in het rek plaatsen.
7. U hebt de volgende kabels aangeschaft om uw Data Box aan te sluiten op de hostcomputer.
    - Twee 10 GbE SFP + Twinax koperen kabels (gebruiken met DATA 1-, DATA 2-netwerkinterfaces)
    - Eén CAT-6 RJ-45-netwerkkabel (gebruiken met de MGMT-netwerkinterface)
    - Eén RJ-45 CAT 6A- OF één RJ-45 CAT 6-netwerkkabel (gebruiken met DATA 3-netwerkinterface die is geconfigureerd als respectievelijk 10 Gbps of 1 Gbps)

## <a name="cable-your-device"></a>Uw apparaat bekabelen

Voer de volgende stappen uit om uw apparaat te bekabelen.

1. Controleer of er met het apparaat is geknoeid en of het is beschadigd. Als er met het apparaat is geknoeid of het ernstig is beschadigd, gaat u niet verder. Neem direct contact op met Microsoft Ondersteuning om te bepalen of het apparaat in een goede staat is of dat er een vervangend apparaat moet worden verzonden.
2. Verplaats het apparaat naar de locatie waar u het wilt inschakelen. Plaats het apparaat op een plat oppervlak. Het apparaat kan ook op een plank van een standaardrek worden geplaatst.
3. Sluit het netsnoer en de netwerkkabels aan. Hieronder ziet u de achterzijde van een verbonden apparaat voor een algemene configuratie. 
    
    ![Bekabeling achterzijde van Data Box-apparaat](media/data-box-deploy-set-up/data-box-cabled-dhcp.png)

    1. Sluit de voedingskabel aan op de gelabelde stroomingang. Het andere uiteinde van de voedingskabel moet zijn verbonden met een stroomverdeler.
    2. Gebruik de RJ-45 CAT 6-kabel om de MGMT-poort aan het ene uiteinde en een laptop aan het andere uiteinde aan te sluiten.            
    3. Sluit het ene uiteinde van de RJ-45 CAT 6A-kabel aan op de DATA 3-poort. DATA 3 is geconfigureerd als 10 GbE als u de RJ-45 CAT 6A-kabel gebruikt en als 1 GbE als u de RJ-45 CAT 6-kabel gebruikt.
    4. Gebruik respectievelijk de 10 GbE SFP + Twinax koperen kabels om de DATA 1- en DATA 2-poort aan te sluiten. 
    5. Het andere uiteinde van de kabels van de gegevenspoorten zijn op de hostcomputer aangesloten via een 10 GbE-switch.

4. Zoek de aan/uit-knop op het bedieningspaneel aan de voorzijde van het apparaat. Schakel het apparaat in.

    ![Aan-/uit-knop Data Box](media/data-box-deploy-set-up/data-box-powered-door-open.png)

## <a name="connect-to-your-device"></a>Verbinding maken met uw apparaat

Voer de volgende stappen uit om uw apparaat in te stellen met behulp van de lokale webgebruikersinterface en de gebruikersinterface van de portal.

1. Configureer de Ethernet-adapter op de laptop waarmee u verbinding maakt met het apparaat met het statische IP-adres 192.168.100.5 en het subnet 255.255.255.0. 
2. Maak verbinding met de MGMT-poort van uw apparaat en open de lokale webgebruikersinterface via https://192.168.100.10. Dit kan tot 5 minuten duren nadat u het apparaat hebt ingeschakeld.
3. Klik op **Details** en klik vervolgens op **Ga verder naar de webpagina**.

   ![Verbinding maken met de lokale webgebruikersinterface](media/data-box-deploy-set-up/data-box-connect-local-web-ui.png) 

4. U ziet een pagina **Aanmelden** voor de lokale webgebruikersinterface. Zorg ervoor dat het serienummer van het apparaat overeenkomt in zowel de gebruikersinterface van de portal als de lokale webgebruikersinterface. Het apparaat is op dit moment vergrendeld.
5. Meld u aan bij de [Azure Portal](https://portal.azure.com).
6. Download de referenties van het apparaat vanuit de portal. Ga naar **Algemeen > Apparaatdetails**. Kopieer het **Apparaatwachtwoord**. Het wachtwoord van het apparaat is aan een specifieke volgorde gekoppeld in de portal. 

    ![Apparaatreferenties ophalen](media/data-box-deploy-set-up/data-box-device-credentials.png)
    
    
7. Geef het apparaatwachtwoord op dat u in de vorige stap hebt verkregen via Azure Portal om u aan te melden bij de lokale webgebruikersinterface van het apparaat. Klik op **Aanmelden**.
8. Zorg er op het **Dashboard** voor dat de netwerkinterfaces zijn geconfigureerd. 
    - Als DHCP is ingeschakeld in uw omgeving, worden netwerkinterfaces automatisch geconfigureerd. 
    - Als DHCP niet is ingeschakeld, gaat u naar **Netwerkinterfaces instellen** en wijst u indien nodig statische IP-adressen toe.

    ![Apparaatdashboard](media/data-box-deploy-set-up/data-box-dashboard-1.png)

Zodra de installatie van het apparaat is voltooid, kunt u verbinding maken met de apparaat-shares en de gegevens van uw computer naar het apparaat kopiëren. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Data Box, zoals:

> [!div class="checklist"]
> * De Data Box bekabelen
> * De Data Box aansluiten

Ga naar de volgende zelfstudie om te lezen hoe u gegevens kopieert naar uw Data Box.

> [!div class="nextstepaction"]
> [Uw gegevens naar Azure Data Box kopiëren](./data-box-deploy-copy-data.md)

