---
title: Deactiveren en verwijderen van een StorSimple-apparaat | Microsoft Docs
description: Beschrijving van het StorSimple-apparaat verwijderen uit de service met deze eerst te deactiveren en vervolgens te verwijderen.
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 155cda38-c5ae-45dc-b7e8-6444494afc9e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c000a642aa088ac80cc7077453b87e9a47f96900
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="deactivate-and-delete-a-storsimple-8000-series-device-via-storsimple-manager-service"></a>Deactiveren en verwijderen van een StorSimple 8000 series apparaat via de StorSimple Manager-service
## <a name="overview"></a>Overzicht
U kunt desgewenst een StorSimple-apparaat buiten dienst duren (bijvoorbeeld, als u uw apparaat upgraden of vervangen of als u niet langer StorSimple gebruikt). Als dit het geval is, moet u het apparaat deactiveren voordat u het kunt verwijderen. Deactiveren verbreekt de verbinding tussen het apparaat en de bijbehorende StorSimple Manager-service. Deze zelfstudie wordt uitgelegd hoe u een StorSimple-apparaat verwijdert uit de service door het eerste te deactiveren en vervolgens te verwijderen. 

Wanneer u een apparaat deactiveert, is geen gegevens die lokaal zijn opgeslagen op het apparaat niet langer toegankelijk. Alleen de gegevens die zijn gekoppeld aan het apparaat dat is opgeslagen in de cloud kunnen worden hersteld.  

> [!WARNING]
> Deactivering is een permanente bewerking en kan niet ongedaan worden gemaakt. Een gedeactiveerde apparaat kan niet worden geregistreerd met de StorSimple Manager-service, tenzij u deze eerst naar de standaardinstellingen van de factory opnieuw ingesteld. 
> 
> De fabrieksinstellingen van proces verwijdert alle gegevens die lokaal zijn opgeslagen op uw apparaat. Daarom is het essentieel dat u een momentopname cloud van al uw gegevens voordat u een apparaat deactiveren. Hierdoor kunt u alle gegevens in een later stadium te herstellen.
> 
> 

Deze zelfstudie wordt uitgelegd hoe:

* Een apparaat deactiveren en verwijderen van de gegevens
* Een apparaat deactiveren en de gegevens behouden

Ook wordt uitgelegd hoe deactiveren en verwijderen op een virtueel StorSimple-apparaat werkt.

> [!NOTE]
> Voordat u een virtueel StorSimple fysiek of virtueel apparaat deactiveert, moet u stoppen of verwijderen van clients en hosts die afhankelijk van dat apparaat zijn.
> 
> 

## <a name="deactivate-and-delete-data"></a>Deactiveren en verwijderen van gegevens
Als u geïnteresseerd bent in het apparaat volledig verwijderen en niet wilt bewaren van gegevens op het apparaat, klikt u vervolgens de volgende stappen uitvoeren.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Het apparaat deactiveren en verwijderen van de gegevens
1. Voordat u een apparaat deactiveert, moet u het volume containers (en de volumes) die zijn gekoppeld aan het apparaat. Nadat u de gekoppelde back-ups hebt verwijderd, kunt u volumecontainers verwijderen.
2. Als volgt te werk om het apparaat te deactiveren:
   
   1. Op de StorSimple Manager-service **apparaten** pagina, selecteert u het apparaat dat u wilt uitschakelen en klik op aan de onderkant van de pagina **deactiveren**.
   2. Er wordt een bevestigingsbericht weergegeven. Klik op **Ja** om door te gaan. Het proces deactiveren start en een paar minuten duren.
3. Na de deactivering, kunt u het apparaat volledig verwijderen. Als u een apparaat verwijdert, wordt deze verwijderd uit de lijst met apparaten die zijn verbonden met de service. De service kan niet langer dan de verwijderde apparaatinschrijvingsbeheerder beheren. Gebruik de volgende stappen uit om te verwijderen van het apparaat:
   
   1. Op de StorSimple Manager-service **apparaten** pagina, selecteert u een gedeactiveerde apparaat dat u wilt verwijderen.
   2. Klik onderaan op de pagina **verwijderen**.
   3. U wordt gevraagd om bevestiging. Klik op **Ja** om door te gaan.
      
      Duurt enkele minuten duren voordat het apparaat moet worden verwijderd.

## <a name="deactivate-and-retain-data"></a>Deactiveren en behoud van gegevens
Als u bent geïnteresseerd in het verwijderen van het apparaat, maar u wilt dat de gegevens wilt behouden, klikt u vervolgens de volgende stappen uitvoeren.

#### <a name="to-deactivate-a-device-and-retain-the-data"></a>Een apparaat deactiveren en de gegevens behouden
1. Het apparaat deactiveren. De volumecontainers en de momentopnamen van het apparaat blijven.
   
   1. Op de StorSimple Manager-service **apparaten** pagina, selecteert u het apparaat dat u wilt uitschakelen en klik op aan de onderkant van de pagina **deactiveren**.
   2. Er wordt een bevestigingsbericht weergegeven. Klik op **Ja** om door te gaan. Het proces deactiveren start en een paar minuten duren.
2. U kunt nu de volumecontainers en de bijbehorende momentopnamen failover. Voor procedures gaat u naar [Failover en herstel na noodgevallen voor uw StorSimple-apparaat](storsimple-device-failover-disaster-recovery.md).
3. Na deactiveren en failover, kunt u het apparaat volledig verwijderen. Als u een apparaat verwijdert, wordt deze verwijderd uit de lijst met apparaten die zijn verbonden met de service. De service kan niet langer dan de verwijderde apparaatinschrijvingsbeheerder beheren. De volgende stappen voor het verwijderen van het apparaat:
   
   1. Op de StorSimple Manager-service **apparaten** pagina, selecteert u een gedeactiveerde apparaat dat u wilt verwijderen.
   2. Klik onderaan op de pagina **verwijderen**.
   3. U wordt gevraagd om bevestiging. Klik op **Ja** om door te gaan.
      
      Duurt enkele minuten duren voordat het apparaat moet worden verwijderd.

## <a name="deactivate-and-delete-a-virtual-device"></a>Deactiveren en verwijderen van een virtueel apparaat
Voor een virtueel StorSimple-apparaat deallocates deactivering van de virtuele machine. Vervolgens kunt u de virtuele machine en de resources die zijn gemaakt tijdens het inrichten verwijderen. Wanneer het virtuele apparaat wordt gedeactiveerd, kan het niet meer worden hersteld naar de oorspronkelijke staat. 

Deactivering van resultaten in de volgende acties:

* Het virtuele StorSimple-apparaat wordt verwijderd.
* De OSDisk en gegevensschijven die zijn gemaakt voor het virtuele StorSimple-apparaat worden verwijderd.
* De gehoste Service en virtuele netwerken die zijn gemaakt tijdens het inrichten, blijven behouden. Als u deze entiteiten niet gebruikt, moet u deze handmatig verwijderen.
* Cloudmomentopnamen die zijn gemaakt door het virtuele StorSimple-apparaat worden bewaard.

## <a name="next-steps"></a>Volgende stappen
* Als u wilt herstellen het gedeactiveerde apparaat naar de standaardwaarden, gaat u naar [opnieuw instellen van het apparaat fabrieksinstellingen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Voor technische ondersteuning [contact op met Microsoft Support](storsimple-contact-microsoft-support.md).
* Voor meer informatie over het gebruik van de StorSimple Manager-service, gaat u naar [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md). 

