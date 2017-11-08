---
title: Wijzigen van de DATA 0-instellingen op een StorSimple-apparaat | Microsoft Docs
description: Informatie over het gebruik van Windows PowerShell voor StorSimple opnieuw configureren van de DATA 0-netwerkinterface op uw StorSimple-apparaat.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 58e3d509-f425-4a7f-b650-d496a7c85193
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 089988354c5e1ca2e8d5d1554084062a655e43da
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-device"></a>De DATA 0 netwerkinterface-instellingen op uw StorSimple-apparaat wijzigen
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. De versie van dit artikel voor de nieuwe Azure portal, Ga naar [wijzigen van de DATA 0 netwerkinterface-instellingen op uw StorSimple-apparaat](storsimple-8000-modify-data-0.md). Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Overzicht
Uw Microsoft Azure StorSimple-apparaat heeft zes netwerkinterfaces van DATA 0 tot en met 5 van de gegevens. De DATA 0-interface is altijd geconfigureerd via de Windows PowerShell-interface of de seriële console en is automatisch ingeschakeld voor de cloud. Houd er rekening mee dat u kunt DATA 0-netwerkinterface via de klassieke Azure portal niet configureren. 

De DATA 0 interface eerst wordt geconfigureerd via de wizard setup tijdens de eerste implementatie van het StorSimple-apparaat. Wanneer het apparaat zich in een bedrijfsmodus, moet u mogelijk opnieuw configureren van DATA 0 instellingen. Deze zelfstudie bestaat uit twee methoden voor het wijzigen van de DATA 0 netwerk beide via Windows PowerShell voor StorSimple-instellingen.

Na het lezen van deze zelfstudie, kunt u zich kunt:

* Wijzigen van de DATA 0-instelling via de wizard setup-netwerk
* Wijzigen van de DATA 0 netwerkinstellingen via de `Set-HcsNetInterface` cmdlet

## <a name="modify-data-0-network-settings-through-setup-wizard"></a>DATA 0 netwerkinstellingen via de wizard setup wijzigen
U kunt DATA 0-netwerkinstellingen configureren door verbinding te maken met de Windows PowerShell-interface van uw StorSimple-apparaat en het starten van een sessie van de wizard setup. Voer de volgende stappen uit voor het wijzigen van de DATA 0 instellingen:

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a>Instellingen voor DATA 0 netwerk via de wizard setup wijzigen
1. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**. Als u wordt gevraagd bieden de **wachtwoord apparaatbeheerder**. Is het standaardwachtwoord `Password1`.
2. Typ het volgende achter de opdrachtprompt:
   
    `Invoke-HcsSetupWizard`
3. Een instellingenwizard weergegeven voor hulp bij het configureren van de DATA 0-interface van het apparaat. Nieuwe waarden opgeven voor de IP-adres, de gateway en de IP-masker.

> [!NOTE]
> De vaste IP-adressen-controllers moet opnieuw worden geconfigureerd via de **configureren** pagina van het StorSimple-apparaat in de klassieke Azure portal. Ga voor meer informatie naar [netwerkinterfaces wijzigen](storsimple-modify-device-config.md#modify-network-interfaces).
> 
> 

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>DATA 0 netwerkinstellingen via de cmdlet Set-HcsNetInterface wijzigen
Een andere methode voor het configureren van de DATA 0-netwerkinterface met behulp van is de `Set-HcsNetInterface` cmdlet. De cmdlet wordt uitgevoerd vanuit de Windows PowerShell-interface van uw StorSimple-apparaat. Wanneer u deze procedure gebruikt, kan de controller vaste IP-adressen ook hier worden geconfigureerd. Voer de volgende stappen uit voor het wijzigen van de DATA 0 instellingen: 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a>Instellingen voor DATA 0-netwerk via de cmdlet Set-HcsNetInterface wijzigen
1. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**. Als u wordt gevraagd het beheerderswachtwoord van het apparaat te bieden. Is het standaardwachtwoord `Password1`.
2. Typ het volgende achter de opdrachtprompt:
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    Typ de volgende waarden voor de DATA 0 in de punthaken:
   
   * IPv4-adres
   * IPv4-gateway
   * IPv4-subnetmasker
   * Vaste IPv4-adressen voor Controller 0
   * Vaste IPv4-adres voor de Controller 1
     
     Ga voor meer informatie over het gebruik van deze cmdlet naar [Windows PowerShell voor StorSimple cmdlet-verwijzing](https://technet.microsoft.com/library/dn688161.aspx).

## <a name="next-steps"></a>Volgende stappen
* Als u wilt configureren netwerkinterfaces, behalve DATA 0, kunt u de [pagina configureren in de klassieke Azure portal](storsimple-modify-device-config.md). 
* Als u problemen ondervindt bij het configureren van uw netwerkinterfaces, raadpleegt u [implementatieproblemen oplossen](storsimple-troubleshoot-deployment.md).

