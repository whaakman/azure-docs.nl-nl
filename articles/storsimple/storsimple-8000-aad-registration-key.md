---
title: Nieuwe authenticatie gebruiken voor Apparaatbeheer van StorSimple 8000-service in Azure | Microsoft Docs
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
ms.openlocfilehash: 37f44538d94ed78509bbcb09e726dc34a9e92e95
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>Gebruik de nieuwe verificatie voor uw StorSimple

## <a name="overview"></a>Overzicht

De StorSimple-apparaat Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple-apparaten. Tot op heden is StorSimple-apparaat Manager-service een Access Control-service (ACS) gebruikt voor verificatie van de service voor uw StorSimple-apparaat. De ACS-mechanisme wordt binnenkort afgeschaft en vervangen door een Azure Active Directory (AAD)-authenticatie. Ga voor meer informatie naar de volgende aankondigingen voor ACS afschaffing en het gebruik van AAD-verificaties.

- [De toekomst van Azure ACS is Azure Active Directory](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/12/the-future-of-azure-acs-is-azure-active-directory/)
- [Toekomstige wijzigingen in de Microsoft Access Control-Service](https://azure.microsoft.com/en-in/blog/acs-access-control-service-namespace-creation-restriction/)

Dit artikel worden de details van de AAD-verificatie en de bijbehorende nieuwe serviceregistratiesleutel en wijzigingen in de firewallregels voor zover van toepassing op het StorSimple-apparaten. De informatie in dit artikel is van toepassing op StorSimple 8000 series alleen apparaten.

De AAD-verificatie vindt plaats in StorSimple 8000 series apparaat met Update 5 of hoger. Vanwege de introductie van de AAD-verificatie, worden wijzigingen optreden in:

- URL-patronen voor firewallregels.
- Serviceregistratiesleutel.

Deze wijzigingen worden beschreven in de volgende secties besproken.

## <a name="url-changes-for-aad-authentication"></a>Wijzigingen in de URL voor AAD-verificaties

Om ervoor te zorgen dat de service gebruikmaakt van verificatie op basis van AAD, moeten alle gebruikers in hun firewallregels voor de verificatie-URL's opnemen.

Als StorSimple 8000-serie, controleert u of de volgende URL is opgenomen in de firewall-regels:

| URL-patroon                         | Cloud | Onderdeel/functionaliteit         |
|------------------------------------|-------|----------------------------------|
| `https://login.windows.net`        | Azure Public |AAD authentication-service      |
| `https://login.microsoftonline.us` | Amerikaanse overheid |AAD authentication-service      |

Voor een volledige lijst met URL voor StorSimple 8000 series apparaten patronen, gaat u naar [URL-patronen voor firewallregels](storsimple-8000-system-requirements.md#url-patterns-for-firewall-rules).

Als de verificatie-URL niet in de firewallregels voorbij de afschaffing datum opgenomen is en het apparaat Update 5 wordt uitgevoerd, Zie de gebruikers een waarschuwing voor een URL. De gebruikers moeten ook de nieuwe URL van de verificatie. Als het apparaat een versie vóór de Update 5 wordt uitgevoerd, ziet de gebruikers een heartbeat-waarschuwing. In elk geval kan het StorSimple-apparaat niet verifiëren met de service en de service is niet kan communiceren met het apparaat.

## <a name="device-version-and-authentication-changes"></a>Wijzigingen voor versie en verificatie van apparaat

Als een StorSimple 8000 series apparaat gebruikt, gebruikt u de volgende tabel om te bepalen welke actie u moet uitvoeren op basis van het apparaat softwareversie die u uitvoert.

| Als uw apparaat wordt uitgevoerd| De volgende actie ondernemen                                    |
|--------------------------|------------------------|--------------------|--------------------------------------------------------------|
| Update 5 of hoger en het apparaat is offline. <br> Er wordt een waarschuwing URL is niet wilt plaatsen.| Wijzig de firewall-regels zodanig dat de URL voor webverificatie.<br> Zie [verificatie URL's](#url-changes-for-aad-authentication). |
| Update 5 of hoger en het apparaat online is.| Er is geen actie vereist.                                       |
| Update 4 of eerder en het apparaat is offline. | Wijzig de firewall-regels zodanig dat de URL voor webverificatie.<br>[Update 5 downloaden via catalogusserver](storsimple-8000-install-update-5.md#download-updates-for-your-device).<br>[Update 5 toepassen met de hotfix-methode](storsimple-8000-install-update-5.md#install-update-5-as-a-hotfix). <br> [De registratiesleutel AAD ophalen uit de service](#aad-based-registration-keys). <br> [Verbinding maken met de Windows PowerShell-interface van het apparaat van de serie StorSimple 8000](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). <br>Gebruik `Redo-DeviceRegistration` Registreer het apparaat via de Windows PowerShell cmdlet. Geef de sleutel die u in de vorige stap hebt verkregen.|
| Update 4 of eerder en het apparaat is online. |Wijzig de firewall-regels zodanig dat de URL voor webverificatie.<br> Update 5 te installeren via de Azure portal.              |
| Fabrieksinstellingen terugzetten naar een versie voordat u Update 5.      |De portal ziet u een AAD gebaseerde registratiesleutel terwijl het apparaat oudere software wordt uitgevoerd. Volg de stappen in het voorgaande scenario voor wanneer het apparaat Update 4 of ouder wordt uitgevoerd.              |

## <a name="aad-based-registration-keys"></a>Op basis van AAD registratiesleutels

Vanaf Update 5 voor StorSimple 8000 series apparaten, nieuwe op basis van AAD-registratie sleutels worden gebruikt. De registratiesleutels kunt u uw service Manager voor StorSimple-apparaat registreren bij het apparaat.

U kunt de nieuwe registratiesleutels voor AAD-service niet gebruiken als u van een StorSimple 8000 series apparaat met Update 4 of eerder gebruikmaakt (inclusief een ouder apparaat nu wordt geactiveerd).
In dit scenario moet u de serviceregistratiesleutel genereren. Wanneer u de sleutel opnieuw genereren, wordt de nieuwe sleutel gebruikt voor het registreren van de volgende apparaten. De oude sleutel is niet meer geldig.

- De nieuwe registratiesleutel van AAD verloopt na drie dagen.
- De AAD-registratiesleutels werken alleen met StorSimple 8000 series apparaten met Update 5 of hoger.
- De AAD-registratiesleutels zijn langer dan de bijbehorende ACS-registratiesleutels.

Voer de volgende stappen uit voor het genereren van een AAD-serviceregistratiesleutel.

#### <a name="to-generate-the-aad-service-registration-key"></a>Voor het genereren van de AAD-serviceregistratiesleutel

1. In **StorSimple Apparaatbeheer**, gaat u naar **Management &gt;**  **sleutels**. U kunt ook de zoekbalk gebruiken om te zoeken naar _sleutels_.
    
2. Klik op **sleutel genereren**.

    ![Klik op opnieuw genereren](./media/storsimple-8000-aad-registration-key/aad-click-generate-registration-key.png)

3. Kopieer de nieuwe sleutel. De oudere sleutel werkt niet meer.

    ![Opnieuw genereren bevestigen](./media/storsimple-8000-aad-registration-key/aad-registration-key2.png)

    > [!NOTE] 
    > Als u een StorSimple-Cloud-apparaat op de service is geregistreerd met uw StorSimple 8000 series apparaat maakt, genereert u niet een registratiesleutel terwijl het maken uitgevoerd wordt. Wacht totdat het maken om te voltooien en vervolgens de registratiesleutel genereren.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het implementeren van [StorSimple 8000 series apparaat](storsimple-8000-deployment-walkthrough-u2.md).

