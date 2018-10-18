---
title: Nieuwe verificatie gebruiken voor de StorSimple 8000 Device Manager-service in Azure | Microsoft Docs
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
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: b1ea195ab0b06c4ca0fab37fe7e5701229b34938
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49387035"
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>De nieuwe verificatie voor uw StorSimple gebruiken

## <a name="overview"></a>Overzicht

De StorSimple Device Manager-service wordt uitgevoerd in Microsoft Azure en verbindt met meerdere StorSimple-apparaten. Tot op heden heeft StorSimple Device Manager-service een Access Control service (ACS) gebruikt voor verificatie van de service voor uw StorSimple-apparaat. Het mechanisme voor ACS worden binnenkort worden afgeschaft en vervangen door een Azure Active Directory (AAD)-authenticatie. Ga voor meer informatie naar de volgende aankondigingen voor afschaffen ACS en het gebruik van AAD-verificatie.

- [De toekomst van Azure ACS is Azure Active Directory](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/12/the-future-of-azure-acs-is-azure-active-directory/)
- [Toekomstige wijzigingen in de Microsoft Access Control-Service](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/)

Dit artikel beschrijft de details van de AAD-verificatie en de bijbehorende nieuwe serviceregistratiesleutel en wijzigingen in de firewall-regels, zoals van toepassing op het StorSimple-apparaten. De informatie in dit artikel is van toepassing op StorSimple 8000-serie alleen apparaten.

De AAD-verificatie wordt uitgevoerd in de StorSimple 8000-apparaat met Update 5 of hoger. Vanwege de introductie van de AAD-verificatie optreden wijzigingen in:

- URL-patronen voor firewall-regels.
- Serviceregistratiesleutel.

Deze wijzigingen worden besproken in de volgende secties.

## <a name="url-changes-for-aad-authentication"></a>Wijzigingen in de URL voor AAD-verificatie

Om ervoor te zorgen dat de service op basis van AAD-verificatie gebruikt, moeten alle gebruikers de nieuwe verificatie-URL's opnemen in hun firewall-regels.

Als u met behulp van StorSimple 8000-serie, zorgt u ervoor dat de volgende URL wordt opgenomen in de firewall-regels:

| URL-patroon                         | Cloud | Onderdeel/functionaliteit         |
|------------------------------------|-------|----------------------------------|
| `https://login.windows.net`        | Azure openbaar |AAD authentication-service      |
| `https://login.microsoftonline.us` | Amerikaanse overheid |AAD authentication-service      |

Voor een volledige lijst van URL voor apparaten uit de StorSimple 8000-serie patronen, gaat u naar [URL-patronen voor firewallregels](storsimple-8000-system-requirements.md#url-patterns-for-firewall-rules).

Als de verificatie-URL niet in de firewallregels voorbij de afschaffing datum opgenomen is en het apparaat Update 5 wordt uitgevoerd, zien de gebruikers een waarschuwing voor een URL. De gebruikers moeten de nieuwe verificatie-URL. Als het apparaat wordt uitgevoerd een eerdere versie dan Update 5, zien de gebruikers een waarschuwing voor een heartbeat. In elk geval wordt het StorSimple-apparaat kan niet worden geverifieerd met de service en de service is niet kan communiceren met het apparaat.

## <a name="device-version-and-authentication-changes"></a>Wijzigingen in apparaat-versie en -verificatie

Als een apparaat voor StorSimple 8000-serie gebruikt, gebruikt u de volgende tabel om te bepalen welke actie u dient te ondernemen op basis van de software-versie van het apparaat die wordt uitgevoerd.

| Als uw apparaat wordt uitgevoerd| De volgende actie ondernemen                                    |
|--------------------------|------------------------|--------------------|--------------------------------------------------------------|
| Update 5 of hoger en het apparaat is offline. <br> U ziet een waarschuwing die URL is niet opgenomen in de whitelist.|1. Wijzig de firewall-regels zodanig dat de verificatie-URL. Zie [verificatie-URL's](#url-changes-for-aad-authentication).<br>2. [De registratiesleutel AAD ophalen van de service](#aad-based-registration-keys).<br>3. [Verbinding maken met de Windows PowerShell-interface van de StorSimple 8000-apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).<br>4. Gebruik `Redo-DeviceRegistration` cmdlet voor het registreren van het apparaat via de Windows PowerShell. Geef de sleutel die u hebt verkregen in de vorige stap.|
| Update 5 of hoger en het apparaat online.| Er is geen actie vereist.                                       |
| Update 4 of een eerdere versie en het apparaat is offline. |1. Wijzig de firewall-regels zodanig dat de verificatie-URL.<br>2. [Update 5 downloaden via catalogusserver](storsimple-8000-install-update-5.md#download-updates-for-your-device).<br>3. [Update 5 toepassen via de methode hotfix](storsimple-8000-install-update-5.md#install-update-5-as-a-hotfix).<br>4. [De registratiesleutel AAD ophalen van de service](#aad-based-registration-keys).<br>5. [Verbinding maken met de Windows PowerShell-interface van de StorSimple 8000-apparaat](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). <br>6. Gebruik `Redo-DeviceRegistration` cmdlet voor het registreren van het apparaat via de Windows PowerShell. Geef de sleutel die u hebt verkregen in de vorige stap.|
| Update 4 of een eerdere versie en het apparaat is online. |Wijzig de firewall-regels zodanig dat de verificatie-URL.<br> Update 5 installeren via de Azure-portal.              |
| Fabrieksinstellingen terugzetten naar een versie voordat u Update 5.      |De portal ziet u een basis van AAD-registratiesleutel terwijl het apparaat oudere software wordt uitgevoerd. Volg de stappen in het voorgaande scenario voor wanneer het apparaat Update 4 of ouder wordt uitgevoerd.              |

## <a name="aad-based-registration-keys"></a>Van AAD op basis van registratiesleutels

Vanaf Update 5 voor apparaten in de StorSimple 8000-serie, op basis van AAD registratie van nieuwe sleutels worden gebruikt. U de registratiesleutels gebruiken voor het registreren van uw StorSimple Device Manager-service met het apparaat.

U kunt de nieuwe registratiesleutels van de AAD-service niet gebruiken als u een StorSimple 8000-serie-apparaat met Update 4 of eerder (met inbegrip van een ouder apparaat nu wordt geactiveerd).
In dit scenario moet u de serviceregistratiesleutel opnieuw genereren. Nadat u de sleutel opnieuw genereert, wordt de nieuwe sleutel wordt gebruikt voor het registreren van de volgende apparaten. De oude sleutel is niet meer geldig.

- De nieuwe AAD-registratiesleutel verloopt na drie dagen.
- De AAD-registratiesleutels werken alleen met de StorSimple 8000-serie apparaten met Update 5 of hoger.
- De AAD-registratiesleutels zijn langer zijn dan de bijbehorende sleutels van de ACS-registratie.

Voer de volgende stappen uit voor het genereren van een AAD-serviceregistratiesleutel.

#### <a name="to-generate-the-aad-service-registration-key"></a>Voor het genereren van de AAD-serviceregistratiesleutel

1. In **StorSimple Device Manager**, gaat u naar **Management &gt;**  **sleutels**. U kunt ook de zoekbalk gebruiken om te zoeken naar _sleutels_.
    
2. Klik op **sleutel genereren**.

    ![Klik op sleutel opnieuw genereren](./media/storsimple-8000-aad-registration-key/aad-click-generate-registration-key.png)

3. Kopieer de nieuwe sleutel. De oudere sleutel werkt niet meer.

    ![Controleer of de sleutel opnieuw genereren](./media/storsimple-8000-aad-registration-key/aad-registration-key2.png)

    > [!NOTE] 
    > Als u een StorSimple-Cloudapparaat op de service geregistreerd bij de StorSimple 8000-apparaat maakt, genereert u niet een registratiesleutel terwijl het wordt gemaakt. Wachten op voor het maken om te voltooien en vervolgens de registratiesleutel genereren.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het implementeren [StorSimple 8000-apparaat](storsimple-8000-deployment-walkthrough-u2.md).

