---
title: Nieuwe verificatie voor virtuele opslagmatrices van StorSimple | Microsoft Docs
description: Wordt uitgelegd hoe u op basis van AAD-verificatie gebruiken voor uw service, het genereren van nieuwe registratiesleutel en het uitvoeren van handmatige registratie van de apparaten.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: alkohli
ms.openlocfilehash: 080f49ca1078858462264f229e9acfee6fad17d1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61387618"
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>De nieuwe verificatie voor uw StorSimple gebruiken

## <a name="overview"></a>Overzicht

De StorSimple Device Manager-service wordt uitgevoerd in Microsoft Azure en verbindt met meerdere virtuele StorSimple-matrices. Tot op heden heeft StorSimple Device Manager-service een Access Control service (ACS) gebruikt voor verificatie van de service voor uw StorSimple-apparaat. Het mechanisme voor ACS worden binnenkort worden afgeschaft en vervangen door een Azure Active Directory (AAD)-authenticatie.

De informatie in dit artikel is van toepassing op beide StorSimple 1200-serie alleen virtuele matrices. Dit artikel beschrijft de details van de AAD-verificatie en de bijbehorende nieuwe serviceregistratiesleutel en wijzigingen in de firewall-regels, zoals van toepassing op het StorSimple-apparaten.

De AAD-verificatie wordt uitgevoerd in virtuele StorSimple-matrices (1200 model) met Update 1 of hoger.

Vanwege de introductie van de AAD-verificatie optreden wijzigingen in:

- URL-patronen voor firewall-regels.
- Serviceregistratiesleutel.

Deze wijzigingen worden besproken in de volgende secties.

## <a name="url-changes-for-aad-authentication"></a>Wijzigingen in de URL voor AAD-verificatie

Om ervoor te zorgen dat de service op basis van AAD-verificatie gebruikt, moeten alle gebruikers de nieuwe verificatie-URL's opnemen in hun firewall-regels.

Als u StorSimple Virtual Array, zorgt u ervoor dat de volgende URL wordt opgenomen in de firewall-regels:

| URL-patroon                         | Cloud | Onderdeel/functionaliteit         |
|------------------------------------|-------|---------------------------------|
| `https://login.windows.net`        | Azure Public |AAD authentication-service      |
| `https://login.microsoftonline.us` | Amerikaanse overheid |AAD authentication-service      |

Voor een volledige lijst van URL voor virtuele opslagmatrices van StorSimple patronen, gaat u naar [URL-patronen voor firewallregels](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules).

Als de verificatie-URL niet in de firewall-regels na de datum van de afschaffing opgenomen is, zien de gebruikers een kritieke waarschuwing dat het StorSimple-apparaat kan niet worden geverifieerd met de service. De service zich niet kan communiceren met het apparaat. Als de gebruikers deze waarschuwing zien, moeten ze de nieuwe verificatie-URL. Ga voor meer informatie over de waarschuwing naar [waarschuwingen gebruiken voor het bewaken van uw StorSimple-apparaat](storsimple-virtual-array-manage-alerts.md#networking-alerts).

## <a name="device-version-and-authentication-changes"></a>Wijzigingen in apparaat-versie en -verificatie

Als u een StorSimple Virtual Array, gebruikt u de volgende tabel om te bepalen welke actie u dient te ondernemen op basis van de software-versie van het apparaat die wordt uitgevoerd.

| Als uw apparaat wordt uitgevoerd  | De volgende actie ondernemen                                    |
|----------------------------|--------------------------------------------------------------|
| Update 1.0 of hoger en offline is. <br> U ziet een waarschuwing dat de URL niet opgenomen in de whitelist is.| 1. Wijzig de firewall-regels zodanig dat de verificatie-URL. Zie [verificatie-URL's](#url-changes-for-aad-authentication). <br> 2. [De registratiesleutel AAD ophalen van de service](#aad-based-registration-keys). <br> 3. Voer stappen 1-5 [verbinding maken met de Windows PowerShell-interface van de virtuele matrix](storsimple-virtual-array-deploy2-provision-hyperv.md#step-2-provision-a-virtual-array-in-hypervisor).<br> 4. Gebruik `Invoke-HcsReRegister` cmdlet voor het registreren van het apparaat via de Windows PowerShell. Geef de sleutel die u hebt verkregen in de vorige stap.|
| Update 1.0 of hoger en het apparaat is online.| Geen actie vereist.                                       |
| Update 0.6 of eerder en het apparaat is offline. | 1. [Download de Update 1.0 via catalogusserver](storsimple-virtual-array-install-update-1.md#download-the-update-or-the-hotfix).<br>2. [Update 1.0 toepassen via de lokale webgebruikersinterface](storsimple-virtual-array-install-update-1.md#install-the-update-or-the-hotfix).<br>3. [De registratiesleutel AAD ophalen van de service](#aad-based-registration-keys). <br>4. Voer stappen 1-5 [verbinding maken met de Windows PowerShell-interface van de virtuele matrix](storsimple-virtual-array-deploy2-provision-hyperv.md#step-2-provision-a-virtual-array-in-hypervisor).<br>5. Gebruik `Invoke-HcsReRegister` cmdlet voor het registreren van het apparaat via de Windows PowerShell. Geef de sleutel die u hebt verkregen in de vorige stap.|
| Update 0.6 of eerder en het apparaat is online | Wijzig de firewall-regels zodanig dat de verificatie-URL.<br> Installeer Update 1.0 via Azure portal. |

## <a name="aad-based-registration-keys"></a>Van AAD op basis van registratiesleutels

Vanaf Update 1.0 voor StorSimple virtuele matrices, op basis van AAD registratie van nieuwe sleutels worden gebruikt. U de registratiesleutels gebruiken voor het registreren van uw StorSimple Device Manager-service met het apparaat.

U kunt de nieuwe registratiesleutels van de AAD-service niet gebruiken als u een virtuele StorSimple-matrices met Update 0.6 of eerder. U moet de serviceregistratiesleutel opnieuw genereren. Nadat u de sleutel opnieuw genereert, wordt de nieuwe sleutel wordt gebruikt voor het registreren van de volgende apparaten. De oude sleutel is niet meer geldig.

- De nieuwe AAD-registratiesleutel verloopt na drie dagen.
- De AAD-registratie sleutels in combinatie alleen met StorSimple 1200-serie virtuele matrices actieve Update 1 of hoger. De AAD-registratiesleutel van een StorSimple 8000-apparaat werkt niet.
- De AAD-registratiesleutels zijn langer zijn dan de bijbehorende sleutels van de ACS-registratie.

Voer de volgende stappen uit voor het genereren van een AAD-serviceregistratiesleutel.

#### <a name="to-generate-the-aad-service-registration-key"></a>Voor het genereren van de AAD-serviceregistratiesleutel

1. In **StorSimple Device Manager**, gaat u naar **Management &gt;**  **sleutels**.
    
    ![Ga naar de sleutels](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key1.png)

2. Klik op **sleutel genereren**.

    ![Klik op sleutel opnieuw genereren](./media/storsimple-virtual-array-aad-registration-key/aad-click-generate-registration-key.png)

3. Kopieer de nieuwe sleutel. De oudere sleutel werkt niet meer.

    ![Controleer of de sleutel opnieuw genereren](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key2.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het implementeren [StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md)
