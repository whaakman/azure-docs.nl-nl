---
title: 'Azure Active Directory Domain Services: DNS-instellingen bijwerken voor het virtuele Azure-netwerk | Microsoft Docs'
description: Aan de slag met Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: ergreenl
ms.openlocfilehash: 7d2902c997259fc115a1f204f123983038821887
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157340"
---
# <a name="enable-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services inschakelen

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Taak 4: DNS-instellingen bijwerken voor het virtuele Azure-netwerk
In de voorgaande configuratietaken hebt u Azure Active Directory Domain Services ingeschakeld voor uw directory. Zorg er nu voor dat computers binnen het virtuele netwerk verbinding kunnen maken met deze services en ze kunnen gebruiken. In dit artikel werkt u de DNS-serverinstellingen voor het virtuele netwerk zo bij dat deze verwijzen naar de twee IP-adressen waarop Azure Active Directory Domain Services beschikbaar is op het virtuele netwerk.

Voer de volgende stappen uit om de DNS-serverinstellingen bij te werken voor het virtuele netwerk waarin u Azure Active Directory Domain Services hebt ingeschakeld:


1. Het tabblad **Overzicht** bevat een reeks **vereiste configuratiestappen** die moeten worden uitgevoerd nadat uw beheerde domein volledig is ingericht. De eerste configuratiestap is **de DNS-serverinstellingen voor het virtuele netwerk bijwerken**.

    ![Domain Services - tabblad Overzicht](./media/getting-started/domain-services-provisioned-overview.png)

    > [!TIP]
    > Ziet u deze configuratiestap niet? Als de DNS-serverinstellingen voor het virtuele netwerk up-to-date zijn, ziet u niet de tegel DNS-serverinstellingen voor het virtuele netwerk bijwerken op het tabblad Overzicht.
    >
    >

2. Klik op de knop **Configureren** om de DNS-serverinstellingen bij te werken voor het virtuele netwerk.

> [!NOTE]
> De nieuwe DNS-instellingen worden pas op de virtuele machines toegepast na het opnieuw opstarten. Als u de bijgewerkte DNS-instellingen meteen wilt toepassen, activeert u opnieuw opstarten via de portal, PowerShell of de CLI.
>
>

## <a name="next-step"></a>Volgende stap
[Taak 5: Synchronisatie van wachtwoordhashes inschakelen voor Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)
