---
title: Nieuwe verificatie voor virtuele StorSimple-matrices | Microsoft Docs
description: Legt uit hoe u AAD gebaseerde verificatie gebruiken voor uw service, nieuwe registratiesleutel genereren en voert u handmatig de registratie van de apparaten.
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: 8d033cc09de8e115324067d7bbdf052751730d63
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>Gebruik de nieuwe verificatie voor uw StorSimple

## <a name="overview"></a>Overzicht

De StorSimple-apparaat Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere virtuele StorSimple-matrices. Tot op heden is StorSimple-apparaat Manager-service een Access Control-service (ACS) gebruikt voor verificatie van de service voor uw StorSimple-apparaat. De ACS-mechanisme wordt binnenkort afgeschaft en vervangen door een Azure Active Directory (AAD)-authenticatie.

De informatie in dit artikel is van toepassing op beide StorSimple 1200 reeks alleen virtuele Arrays. Dit artikel worden de details van de AAD-verificatie en de bijbehorende nieuwe serviceregistratiesleutel en wijzigingen in de firewallregels voor zover van toepassing op het StorSimple-apparaten.

De AAD-verificatie vindt plaats in het virtuele StorSimple-matrices (model 1200) met Update 1 of hoger.

Vanwege de introductie van de AAD-verificatie, worden wijzigingen optreden in:

- URL-patronen voor firewallregels.
- Serviceregistratiesleutel.

Deze wijzigingen worden beschreven in de volgende secties besproken.

## <a name="url-changes-for-aad-authentication"></a>Wijzigingen in de URL voor AAD-verificaties

Om ervoor te zorgen dat de service gebruikmaakt van verificatie op basis van AAD, moeten alle gebruikers in hun firewallregels voor de verificatie-URL's opnemen.

Als virtuele StorSimple-matrix, controleert u of de volgende URL is opgenomen in de firewall-regels:

| URL-patroon                         | Cloud | Onderdeel/functionaliteit         |
|------------------------------------|-------|---------------------------------|
| `https://login.windows.net`        | Azure Public |AAD authentication-service      |
| `https://login.microsoftonline.us` | Amerikaanse overheid |AAD authentication-service      |

Voor een volledige lijst met URL voor virtuele StorSimple-matrices patronen, gaat u naar [URL-patronen voor firewallregels](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules).

Als de verificatie-URL niet in de firewallregels voorbij de afschaffing datum opgenomen is, ziet de gebruikers een kritieke waarschuwing voor die hun StorSimple-apparaat kan niet worden geverifieerd met de service. De service zich niet kan communiceren met het apparaat. Als gebruikers deze waarschuwing wordt weergegeven, moeten deze de nieuwe URL voor webverificatie opnemen. Voor meer informatie over de waarschuwing, gaat u naar [waarschuwingen gebruiken voor het bewaken van uw StorSimple-apparaat](storsimple-virtual-array-manage-alerts.md#networking-alerts).

## <a name="device-version-and-authentication-changes"></a>Wijzigingen voor versie en verificatie van apparaat

Als u een virtueel StorSimple-matrix, gebruik de volgende tabel om te bepalen welke actie u moet uitvoeren op basis van het apparaat softwareversie die u uitvoert.

| Als uw apparaat wordt uitgevoerd  | De volgende actie ondernemen                                    |
|----------------------------|--------------------------------------------------------------|
| Update 1.0 of hoger en offline is. <br> Er wordt een waarschuwing dat de URL is niet wilt plaatsen.| Wijzig de firewall-regels zodanig dat de URL voor webverificatie. Zie [verificatie URL's](#url-changes-for-aad-authentication). |
| Update 1.0 of hoger en het apparaat is online.| Er is geen actie vereist.                                       |
| Update 0,6 of een oudere versie en het apparaat is offline. | [Download de Update 1.0 via catalogusserver](storsimple-virtual-array-install-update-1.md#download-the-update-or-the-hotfix).<br>[Update 1.0 toepassen via de lokale webgebruikersinterface](storsimple-virtual-array-install-update-1.md#install-the-update-or-the-hotfix). <br> [De registratiesleutel AAD ophalen uit de service](#aad-based-registration-keys). <br> Voer stappen 1-5 voor [verbinding maken met de Windows PowerShell-interface van de virtuele matrix](storsimple-virtual-array-deploy2-provision-hyperv.md#step-2-provision-a-virtual-array-in-hypervisor).<br> Gebruik `Invoke-HcsReRegister` Registreer het apparaat via de Windows PowerShell cmdlet. Geef de sleutel die u in de vorige stap hebt verkregen.|
| Update 0,6 of een oudere versie en het apparaat is online | Wijzig de firewall-regels zodanig dat de URL voor webverificatie.<br> Update 1.0 installeren via de Azure portal. |

## <a name="aad-based-registration-keys"></a>Op basis van AAD registratiesleutels

Vanaf Update 1.0 voor StorSimple virtuele matrices, nieuwe op basis van AAD-registratie sleutels worden gebruikt. De registratiesleutels kunt u uw service Manager voor StorSimple-apparaat registreren bij het apparaat.

U kunt de nieuwe registratiesleutels voor AAD-service niet gebruiken als u gebruikmaakt van een virtueel StorSimple-matrices met Update 0,6 of lager. U moet de service registratie-sleutel opnieuw genereren. Wanneer u de sleutel opnieuw genereren, wordt de nieuwe sleutel gebruikt voor het registreren van de volgende apparaten. De oude sleutel is niet meer geldig.

- De nieuwe registratiesleutel van AAD verloopt na drie dagen.
- Het werk AAD registratie sleutels alleen met de StorSimple 1200 reeks virtuele matrices met Update 1 of nieuwer. De registratiesleutel AAD van een StorSimple 8000 series apparaat werkt niet.
- De AAD-registratiesleutels zijn langer dan de bijbehorende ACS-registratiesleutels.

Voer de volgende stappen uit voor het genereren van een AAD-serviceregistratiesleutel.

#### <a name="to-generate-the-aad-service-registration-key"></a>Voor het genereren van de AAD-serviceregistratiesleutel

1. In **StorSimple Apparaatbeheer**, gaat u naar **Management &gt;**  **sleutels**.
    
    ![Ga naar sleutels](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key1.png)

2. Klik op **sleutel genereren**.

    ![Klik op opnieuw genereren](./media/storsimple-virtual-array-aad-registration-key/aad-click-generate-registration-key.png)

3. Kopieer de nieuwe sleutel. De oudere sleutel werkt niet meer.

    ![Opnieuw genereren bevestigen](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key2.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het implementeren van [virtuele StorSimple-matrix](storsimple-virtual-array-deploy1-portal-prep.md)
