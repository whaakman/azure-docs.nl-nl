---
title: 'Azure Active Directory Domain Services: DNS-instellingen bijwerken voor het virtuele Azure-netwerk | Microsoft Docs'
description: Aan de slag met Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
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
ms.author: maheshu
ms.openlocfilehash: f683eeee05f264ca239b8f1da2bc5078e0146a17
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503482"
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
