---
title: Azure AD-selfservice wachtwoord opnieuw instellen van Windows 7- en 8.1
description: Het inschakelen van self-service voor wachtwoord opnieuw instellen met behulp van vergeten op Windows 7- of 8.1 aanmeldingsscherm-wachtwoord
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: ae2fab560ca9579860aeba91f9322c6521a83de1
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47167911"
---
# <a name="how-to-enable-password-reset-from-windows-7-8-and-81"></a>Hoe: inschakelen voor wachtwoord opnieuw instellen van Windows 7, 8 en 8.1

Als een beheerder u selfservice voor wachtwoordherstel (SSPR) hebt ingeschakeld, maar gebruikers blijven aanroepen van de helpdesk om hun wachtwoord opnieuw instellen omdat ze geen toegang hebt tot een browservenster geopend voor toegang tot de [SSPR-portal](https://aka.ms/sspr). Voor Windows 10-computers, kunt u de koppeling 'Wachtwoord opnieuw instellen' op het aanmeldingsscherm met behulp van de zelfstudie [Azure AD-wachtwoord opnieuw instellen vanuit het aanmeldingsscherm](tutorial-sspr-windows.md), de volgende richtlijnen, helpt u bij het inschakelen van Windows 7, 8 en 8.1 gebruikers opnieuw in te stellen het wachtwoord met behulp van self-service voor Wachtwoordherstel op het aanmeldingsscherm van Windows.

In tegenstelling tot Windows 10 machines, Windows 7, 8 en 8.1 computers hebben een Azure AD domein of Active Directory-domein vereiste voor wachtwoord opnieuw instellen.

![Voorbeeld van de Windows 7-aanmeldingsscherm met 'Vergeten wachtwoord?' koppeling](media/howto-sspr-windows-7-8/windows-7-logon-screen.png)

## <a name="prerequisites"></a>Vereisten

* Self-service voor wachtwoordherstel voor Azure AD moet zijn ingeschakeld.
* Gecorrigeerde Windows 7 of Windows 8.1-besturingssysteem.
* TLS 1.2 ingeschakeld met behulp van de richtlijnen die zijn gevonden in [registerinstellingen voor Transport Layer Security (TLS)](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12).

> [!WARNING]
> TLS 1.2 moet zijn ingeschakeld, niet alleen worden ingesteld op automatisch onderhandelen over.

## <a name="install"></a>Installeren

1. Download het juiste installatieprogramma voor de versie van Windows die u wilt inschakelen.

   1. Software is beschikbaar op het Microsoft download center op [https://aka.ms/sspraddin](https://aka.ms/sspraddin)

1. Aanmelden bij de computer waar u wilt installeren, en voer het installatieprogramma.
1. Opnieuw opstarten wordt na de installatie wordt aanbevolen.
1. Na het opnieuw opstarten, op het aanmeldingsscherm Kies een gebruiker en klik op 'Wachtwoord vergeten?' voor het starten van het wachtwoord opnieuw instellen van werkstroom.
1. De werkstroom die de stappen op het scherm om uw wachtwoord opnieuw in te voltooien.

![Voorbeeld van de Windows 7 geklikt 'Wachtwoord vergeten?' Self-service voor wachtwoord opnieuw instellen van flow](media/howto-sspr-windows-7-8/windows-7-sspr.png)

### <a name="silent-installation"></a>Installatie op de achtergrond

* Gebruik de opdracht 'msiexec /i SsprWindowsLogon.PROD.msi /qn' voor stille installatie
* Voor de achtergrond te verwijderen, gebruikt u de opdracht 'msiexec /x SsprWindowsLogon.PROD.msi /qn"

## <a name="caveats"></a>Aanvullende opmerkingen

U moet registreren voor self-service voor Wachtwoordherstel voordat u wordt mogelijk gebruikt u de koppeling 'Wachtwoord vergeten?'.

![Aanvullende beveiligingsgegevens nodig is voor uw wachtwoord opnieuw instellen](media/howto-sspr-windows-7-8/windows-7-sspr-need-security-info.png)

Uw wachtwoord opnieuw instellen met behulp van de Microsoft Authenticator-app voor meldingen en codes werkt niet in deze eerste release. Gebruikers moeten beschikken over alternatieve methoden geregistreerd en die voldoen aan de vereisten van uw beleid.

## <a name="troubleshooting"></a>Problemen oplossen

Gebeurtenissen worden geregistreerd op de machine én in Azure AD.

Azure AD-gebeurtenissen bevat informatie over de IP-adres en het ClientType waarop het wachtwoord opnieuw instellen is opgetreden.

![Voorbeeld van de Windows 7 schermwachtwoord opnieuw instellen in het auditlogboek van Azure AD](media/howto-sspr-windows-7-8/windows-7-sspr-azure-ad-audit-log.png)

Als aanvullende logboekregistratie vereist is, kan een registersleutel op de machine worden gewijzigd als uitgebreide logboekregistratie wilt inschakelen. Uitgebreide logboekregistratie in voor het oplossen van problemen alleen inschakelen.

```
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{86D2F0AC-2171-46CF-9998-4E33B3D7FD4F}
```

* Voor uitgebreide logboekregistratie inschakelen, maakt u een REG_DWORD: 'EnableLogging', en stel deze in op 1.
* Voor uitgebreide logboekregistratie uitschakelen, wijzigt u de REG_DWORD "EnableLogging" in 0.

## <a name="next-steps"></a>Volgende stappen

[Windows 10-gebruikers hun wachtwoord op het aanmeldingsscherm opnieuw in te schakelen](tutorial-sspr-windows.md)