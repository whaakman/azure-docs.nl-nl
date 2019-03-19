---
title: Adaptieve netwerk beveiliging in Azure Security Center | Microsoft Docs
description: " Informatie over het inschakelen van adaptieve netwerk beperken in Azure Security Center. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: monhaber
ms.assetid: 09d62d23-ab32-41f0-a5cf-8d80578181dd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/13/2019
ms.author: monhaber
ms.openlocfilehash: cede54f69fbeec5e01c17d84436bdc4c9ee91002
ms.sourcegitcommit: f596d88d776a3699f8c8cf98415eb874187e2a48
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2019
ms.locfileid: "58117049"
---
# <a name="adaptive-network-hardening-in-azure-security-center"></a>Adaptieve netwerk beveiliging in Azure Security Center
Informatie over het configureren van adaptieve netwerk beperken in Azure Security Center.

## <a name="what-is-adaptive-network-hardening"></a>Wat is adaptieve netwerk beperken?
Toepassen van [netwerkbeveiligingsgroepen (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview) voor het filteren van verkeer van en naar resources, verbetert u de beveiligingsstatus van uw netwerk. Er kan echter nog steeds zijn sommige gevallen waarin de werkelijke verkeersstroom naar de NSG een subset van de NSG-regels gedefinieerd is. In dergelijke gevallen kan verder verbeteren van de beveiligingsstatus worden bereikt door het beperken van de NSG-regels op basis van de werkelijke verkeerspatronen.

Adaptieve netwerk beperken bevat aanbevelingen voor het verder beperken van de NSG-regels. Het maakt gebruik van een machine learning-algoritme waarin rekening wordt gehouden in het werkelijke verkeer, vertrouwde configuratie, bedreigingsinformatie en andere indicatoren van inbreuk, bekend en vervolgens biedt aanbevelingen om toe te staan alleen verkeer van specifieke IP/poort tuples.

Bijvoorbeeld: Stel dat de bestaande NSG-regel waarmee verkeer van 140.20.30.10/24 op poort 22. De adaptieve netwerk beperken van de aanbeveling, op basis van de analyse zou zijn om te beperken van het bereik en het toestaan van verkeer van 140.23.30.10/29: dit een smaller IP-adresbereik is, en al het andere verkeer naar die poort weigeren.

![netwerk hardening weergeven](./media/security-center-adaptive-network-hardening/traffic-hardening.png)


> [!NOTE]
> Adaptieve aanbevelingen voor beperking van het netwerk worden ondersteund op de volgende poorten: 22, 3389, 21, 23, 445, 4333, 3306, 1433, 1434, 53, 20, 5985, 5986, 5432, 139, 66, 1128

## <a name="view-adaptive-network-hardening-alerts-and-rules"></a>Waarschuwingen adaptieve netwerk beperken en regels weergeven

1. Selecteer in Security Center, **netwerken** -> **adaptieve netwerk beperken**. De netwerk-VM's worden vermeld in drie afzonderlijke tabbladen:
   * **Resources niet in orde**: Virtuele machines die momenteel zijn aanbevelingen en waarschuwingen die zijn geactiveerd door het uitvoeren van het algoritme adaptieve netwerk beperken. 
   * **In orde resources**: Virtuele machines zonder waarschuwingen en aanbevelingen.
   * **Niet-gescande resources**: Virtuele machines die het algoritme adaptieve netwerk beperken, kan niet worden uitgevoerd vanwege een van de volgende redenen:
      * **Virtuele machines zijn VM's met klassieke**:-alleen Azure Resource Manager VM's worden ondersteund.
      * **Er is onvoldoende gegevens beschikbaar**: Security Center vereist om te kunnen genereren nauwkeurige verkeer beperken van aanbevelingen, ten minste 30 dagen van de verkeersgegevens.
      * **Virtuele machine niet wordt beveiligd door ASC (Standard)**: Alleen virtuele machines die zijn ingesteld op de Standard-prijscategorie van Security Center, komen in aanmerking voor deze functie.

     ![resources niet in orde](./media/security-center-adaptive-network-hardening/unhealthy-resources.png)

2. Uit de **resources niet in orde** tabblad, selecteert u een virtuele machine voor het weergeven van waarschuwingen en de aanbevolen beveiligingsregels om toe te passen.

    ![Hardening waarschuwingen](./media/security-center-adaptive-network-hardening/hardening-alerts.png)


## <a name="review-and-apply-adaptive-network-hardening-recommended-rules"></a>Bekijken en adaptieve Hardening van netwerk aanbevolen regels toepassen

1. Uit de **resources niet in orde** tabblad, selecteert u een virtuele machine. De waarschuwingen en aanbevolen hardening regels worden weergegeven.
   ![Hardening waarschuwingen](./media/security-center-adaptive-network-hardening/hardening-alerts.png)

   > [!NOTE]
   > De **regels** tabblad bevat de regels die adaptieve netwerk beperken raadt u toevoegen. De **waarschuwingen** tabblad geeft een lijst van de waarschuwingen die zijn gegenereerd door verkeer naar de resource, niet binnen het IP-adresbereik dat is toegestaan in de aanbevolen regels valt.

   ![Beveiligingsregels](./media/security-center-adaptive-network-hardening/hardening-rules.png)

2. Als u wijzigen van de parameters van een regel wilt, u kunt deze wijzigen, zoals uitgelegd in [wijzigen van een regel](#modify-rule).
   > [!NOTE]
   > U kunt ook [verwijderen](#delete-rule) of [toevoegen](#add-rule) een regel.

3. Selecteer de regels die u wilt toepassen op de NSG en op **afdwingen**. 

### Een regel wijzigen  <a name ="modify-rule"> </a>

U wilt wijzigen van de parameters van een regel die is aanbevolen. U wilt bijvoorbeeld wijzigen van de aanbevolen IP-adresbereiken.

Enkele belangrijke richtlijnen voor het wijzigen van een regel voor het beperken van adaptieve netwerk:

* U kunt de parameters van de regels alleen 'toestaan' wijzigen. 
* U kunt regels om te worden 'toestaan' regels ' toestaan' niet wijzigen. 

  > [!NOTE]
  > Het maken en wijzigen 'toestaan' regels is uitgevoerd rechtstreeks op de NSG voor meer informatie, Zie [maken, wijzigen of verwijderen van een netwerkbeveiligingsgroep](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

* Een **al het verkeer weigeren** regel is het enige type regel 'toestaan' die hier worden vermeld, en kan niet worden gewijzigd. U kunt echter het kunt verwijderen (Zie [verwijderen van een regel](#delete-rule)).
  > [!NOTE]
  > Een **al het verkeer weigeren** regel wanneer wordt aanbevolen, als gevolg hiervan van het uitvoeren van het algoritme, Security Center identificeert niet het verkeer dat moet worden toegestaan, op basis van de bestaande configuratie van de NSG. Daarom is het aanbevolen regel voor het weigeren van al het verkeer naar de opgegeven poort. De naam van dit type regel wordt weergegeven als 'systeem gegenereerd'. Na het afdwingen van deze regel worden de werkelijke naam van de NSG in een tekenreeks die bestaat uit het protocol, de richting van verkeer, "DENY" en een willekeurig getal.

*Een regel voor het beperken van adaptieve netwerk wijzigen:*

1. Sommige van de parameters van een regel te wijzigen de **regels** tabblad, klik op de drie puntjes (...) aan het einde van rij van de regel en klikt u op **regel bewerken**.

   ![regel bewerken](./media/security-center-adaptive-network-hardening/edit-hard-rule.png)

1. In de **regel bewerken** venster, de details die u wilt wijzigen en klik op bijwerken **opslaan**.

   > [!NOTE]
   > Nadat u hebt geklikt **opslaan**, u hebt de regel is gewijzigd. *Echter, u hebt niet toegepast op de NSG.* Als u wilt toepassen, moet u de regel in de lijst selecteren en op **afdwingen** (zoals uitgelegd in de volgende stap).

3. Om toe te passen de bijgewerkte regel in de lijst, selecteert u de bijgewerkte regel en klikt u op **afdwingen**.

### Een nieuwe regel toevoegen <a name ="add-rule"> </a>


U kunt een regel voor 'toestaan' die niet door Security Center aanbevolen is kunt toevoegen.

> [!NOTE]
> Alleen 'toestaan' regels kunnen hier worden toegevoegd. Als u toevoegen wilt 'toestaan' regels, kunt u doen dus rechtstreeks op de NSG. Zie voor meer informatie, [maken, wijzigen of verwijderen van een netwerkbeveiligingsgroep](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

*Een regel voor het beperken van adaptieve netwerk toevoegen:*

1. Klik op **regel toevoegen** (te vinden in de linkerbovenhoek).

   ![regel toevoegen](./media/security-center-adaptive-network-hardening/add-hard-rule.png)

1. In de **regel bewerken** venster, geef de details en klik op **opslaan**.

   > [!NOTE]
   > Nadat u hebt geklikt **opslaan**, hebt u de regel toegevoegd en wordt deze weergegeven met de aanbevolen regels. Echter, u hebt niet toegepast op de NSG. Om deze te activeren, moet u de regel in de lijst selecteren en op **afdwingen** (zoals uitgelegd in de volgende stap).

3. Om toe te passen de nieuwe regel in de lijst, selecteert u de nieuwe regel en klikt u op **afdwingen**.



### Een regel verwijderen <a name ="delete-rule"> </a>

Indien nodig, kunt u een aanbevolen regel verwijderen. U kunt bijvoorbeeld besluiten dat het toepassen van een voorgestelde regel legitieme verkeer mogelijk blokkeert.

*Een regel voor het beperken van adaptieve netwerk verwijderen:*

1. In de **regels** tabblad, klik op de drie puntjes (...) aan het einde van rij van de regel en klikt u op **regel voor het verwijderen**.

   ![Regel verwijderen](./media/security-center-adaptive-network-hardening/delete-hard-rule.png)







 

