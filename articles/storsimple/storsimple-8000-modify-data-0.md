---
title: Wijzigen van de DATA 0-instellingen op de StorSimple 8000-apparaat | Microsoft Docs
description: Informatie over het gebruik van Windows PowerShell voor StorSimple om de DATA 0-netwerkinterface op uw StorSimple-apparaat opnieuw te configureren.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 3cf136c5ddec8f4998d15c597914e1f806453945
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60631580"
---
# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-8000-series-device"></a>Wijzigen van de DATA 0 netwerkinterface-instellingen op uw apparaat StorSimple 8000-serie

## <a name="overview"></a>Overzicht

Uw Microsoft Azure StorSimple-apparaat heeft zes netwerkinterfaces, van de DATA 0 tot en met 5 van de gegevens. De DATA 0-interface wordt altijd geconfigureerd via de Windows PowerShell-interface of de seriële console, en is automatisch ingeschakeld voor de cloud. Houd er rekening mee dat u DATA 0-netwerkinterface via de Azure-portal niet configureren.

De DATA 0-interface is eerst geconfigureerd via de installatiewizard tijdens de eerste implementatie van het StorSimple-apparaat. Wanneer het apparaat zich in een operationele modus, moet u mogelijk opnieuw configureren van DATA 0 instellingen. Deze zelfstudie biedt twee methoden voor het wijzigen van de DATA 0 netwerkinstellingen, zowel via Windows PowerShell voor StorSimple.

Na het lezen van deze zelfstudie, kunt u zich aan:

* Wijzigen van de DATA 0-netwerkinstelling via de wizard setup
* DATA 0 instellingen wijzigen via de `Set-HcsNetInterface` cmdlet

## <a name="modify-data-0-network-settings-through-setup-wizard"></a>DATA 0 instellingen wijzigen via de wizard setup
U kunt DATA 0-netwerkinstellingen configureren door verbinding te maken met de Windows PowerShell-interface van uw StorSimple-apparaat en een sessie van de wizard setup te starten. Voer de volgende stappen uit als u wilt wijzigen van de DATA 0 instellingen:

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a>DATA 0 netwerkinstellingen via de installatiewizard wijzigen
1. Kies in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**. Wanneer u hierom wordt gevraagd bieden de **beheerderswachtwoord**. Is het standaardwachtwoord `Password1`.
2. Typ in de opdrachtprompt:
   
    `Invoke-HcsSetupWizard`
3. Een setup-wizard wordt weergegeven voor het configureren van de DATA 0-interface van uw apparaat. Nieuwe waarden opgeven voor de IP-adres, de gateway en de IP-masker.

> [!NOTE]
> De vaste IP-adressen-controllers moet opnieuw worden geconfigureerd via de **netwerkinstellingen** blade van de StorSimple-apparaat in Azure portal. Ga voor meer informatie naar [netwerkinterfaces wijzigen](storsimple-8000-modify-device-config.md#modify-network-interfaces).

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>DATA 0 instellingen wijzigen via de cmdlet Set-HcsNetInterface
Een alternatieve manier om te configureren van de DATA 0-netwerkinterface via het gebruik van is de `Set-HcsNetInterface` cmdlet. De cmdlet wordt uitgevoerd vanuit de Windows PowerShell-interface van uw StorSimple-apparaat. Wanneer u deze procedure gebruikt, kan het vaste IP-adressen voor de controllers ook hier worden geconfigureerd. Voer de volgende stappen uit voor het wijzigen van de DATA 0 instellingen: 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a>DATA 0 netwerkinstellingen via de cmdlet Set-HcsNetInterface wijzigen
1. Kies in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**. Wanneer u hierom wordt gevraagd het beheerderswachtwoord voor het apparaat opgeven. Is het standaardwachtwoord `Password1`.
2. Typ in de opdrachtprompt:
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    In de punthaken, typt u de volgende waarden voor de DATA 0:
   
   * IPv4-adres
   * IPv4-gateway
   * IPv4-subnetmasker
   * Vaste IPv4-adres van Controller 0
   * Vaste IPv4-adres van Controller 1
     
     Voor meer informatie over het gebruik van deze cmdlet, gaat u naar [Windows PowerShell voor StorSimple-cmdlet-verwijzing](https://technet.microsoft.com/library/dn688161.aspx).

## <a name="next-steps"></a>Volgende stappen
* Voor het configureren van netwerkinterfaces dan DATA 0, kunt u de [netwerkinstellingen configureren in Azure portal](storsimple-8000-modify-device-config.md). 
* Als u problemen ondervindt bij het configureren van de netwerkinterfaces, verwijzen naar [probleemoplossing voor implementatieproblemen](storsimple-troubleshoot-deployment.md).

