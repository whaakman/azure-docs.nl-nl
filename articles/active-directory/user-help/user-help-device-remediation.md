---
title: Mogelijke oorzaken voor het foutbericht 'U kunt daar niet komen vanaf hier' in Azure Active Directory | Microsoft Docs
description: Los de mogelijke redenen waarom u het foutbericht 'U kunt daar niet komen vanaf hier krijgt'.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lizross
ms.reviewer: jairoc
ms.openlocfilehash: 668d69921fd2001cd52bc864e5ee272d179a7bbc
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55770466"
---
# <a name="potential-reasons-for-the-you-cant-get-there-from-here-error-message"></a>Mogelijke redenen voor het foutbericht 'U kunt daar niet komen vanaf hier'
Tijdens het openen van de interne web-apps of services van uw organisatie, krijgt u mogelijk een foutbericht dat aangeeft dat deze, **u daar niet komen vanaf hier**. Dit bericht betekent dat uw organisatie een beleid in plaats dat voorkomt dat uw apparaat toegang tot resources van uw organisatie heeft gezet. Hoewel u ontstaat mogelijk contact opnemen met de Helpdesk als dit probleem wilt oplossen, vindt hier u enkele dingen die u kunt eerst proberen.

## <a name="make-sure-youre-using-a-supported-browser"></a>Zorg ervoor dat u een ondersteunde browser
Als u de **u daar niet komen vanaf hier** bericht waarin wordt gemeld dat u wilt toegang krijgen tot uw organisatie sites vanuit een niet-ondersteunde browser, controleert u welke browser die u uitvoert.

![Foutbericht weergegeven die betrekking hebben op ondersteuning van de browser](media/user-help-device-remediation/browser-version.png)

U lost dit probleem, moet u installeren en uitvoeren van een ondersteunde browser, op basis van uw besturingssysteem. Als u Windows 10, wordt de ondersteunde browsers ook Microsoft Edge, Internet Explorer en Google Chrome. Als u een ander besturingssysteem gebruikt, kunt u de volledige lijst van controleren [ondersteunde browsers](../conditional-access/technical-reference.md#supported-browsers).

## <a name="make-sure-youre-using-a-supported-operating-system"></a>Zorg ervoor dat u een ondersteund besturingssysteem
Zorg ervoor dat u een ondersteunde versie van het besturingssysteem uitvoert, met inbegrip van:

- **Windows-Client.** Windows 7 of hoger.

- **Windows Server.** Windows Server 2008 R2 of hoger.

- **Mac OS.** Mac OS X of hoger

- **Android en iOS.** Meest recente versie van Android en iOS mobiele besturingssystemen

U lost dit probleem, moet u installeren en uitvoeren van een ondersteund besturingssysteem.

## <a name="make-sure-your-device-is-joined-to-your-network"></a>Zorg ervoor dat het apparaat is gekoppeld aan uw netwerk
Als u de **u daar niet komen vanaf hier** bericht waarin wordt gemeld dat uw apparaat-compatibel is met toegangsbeleid van uw organisatie, zorg ervoor dat u hebt uw apparaat toegevoegd aan het netwerk van uw organisatie.

![Foutbericht weergegeven die betrekking hebben op of u in uw netwerk bent](media/user-help-device-remediation/network-version.png)

### <a name="to-check-whether-your-device-is-joined-to-your-network"></a>Om te controleren of het apparaat is gekoppeld aan uw netwerk
1. Aanmelden bij Windows met uw werk- of schoolaccount. Bijvoorbeeld alain@contoso.com.

2. Verbinding maken met het netwerk van uw organisatie via een virtueel particulier netwerk (VPN) of DirectAccess.

3. Nadat u verbonden bent, drukt u op de **Windows-logotoets + l drukken** uw apparaat te vergrendelen.

4. Ontgrendel uw apparaat met uw werk of school-account en probeer het vervolgens toegang tot de problematische app of service opnieuw.

    Als u ziet de **u daar niet komen vanaf hier** foutbericht opnieuw uit, selecteer de **meer details** koppelen en vervolgens contact op met uw Helpdesk met de details.

### <a name="to-join-your-device-to-your-network"></a>Uw apparaat koppelen aan uw netwerk
Als uw apparaat niet is gekoppeld aan het netwerk van uw organisatie, kunt u een van twee dingen doen:

- **Lid worden van uw apparaat werken.** Toevoegen aan het netwerk van uw organisatie eigendom van het werk Windows 10-apparaat, zodat u toegang hebben tot mogelijk beperkte resources. Zie voor meer informatie en stapsgewijze instructies [uw werk-apparaat toevoegen aan het netwerk van uw organisatie](user-help-join-device-on-network.md).

- **Registreer uw persoonlijke apparaat voor het werk.** Registreer uw persoonlijke apparaat, doorgaans een telefoon of tablet, op het netwerk van uw organisatie. Nadat het apparaat is geregistreerd, deze toegang heeft tot beperkte resources van uw organisatie. Zie voor meer informatie en stapsgewijze instructies [registreren van uw persoonlijke apparaat op het netwerk van uw organisatie](user-help-register-device-on-network.md).

## <a name="next-steps"></a>Volgende stappen
- [Wat is de MyApps-portal?](active-directory-saas-access-panel-introduction.md)

- [Aanmelden met uw telefoon, niet met uw wachtwoord](user-help-auth-app-sign-in.md)
