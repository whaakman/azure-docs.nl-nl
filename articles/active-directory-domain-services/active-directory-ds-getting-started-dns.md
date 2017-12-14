---
title: 'Azure Active Directory Domain Services: DNS-instellingen bijwerken voor het virtuele Azure-netwerk | Microsoft Docs'
description: Aan de slag met Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/26/2017
ms.author: maheshu
ms.openlocfilehash: c99d42eaf52a13afef6df76b6bb1a714e719fa64
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="enable-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services inschakelen

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Taak 4: DNS-instellingen bijwerken voor het virtuele Azure-netwerk
In de voorgaande configuratietaken hebt u Azure Active Directory Domain Services ingeschakeld voor uw directory. De volgende taak is om ervoor te zorgen dat computers binnen het virtuele netwerk verbinding kunnen maken met deze services en ze kunnen gebruiken. In dit artikel werkt u de DNS-serverinstellingen voor het virtuele netwerk zo bij dat deze verwijzen naar de twee IP-adressen waarop Azure Active Directory Domain Services beschikbaar is op het virtuele netwerk.

Voer de volgende stappen uit om de DNS-serverinstellingen bij te werken voor het virtuele netwerk waarin u Azure Active Directory Domain Services hebt ingeschakeld:

1. Het tabblad **Overzicht** bevat een reeks **vereiste configuratiestappen** die moeten worden uitgevoerd nadat uw beheerde domein volledig is ingericht. De eerste configuratiestap is **de DNS-serverinstellingen voor het virtuele netwerk bijwerken**.

    ![Domain Services: tabblad Overzicht nadat het domein volledig is ingericht](./media/getting-started/domain-services-provisioned-overview.png)

2. Wanneer uw domein volledig is ingericht, worden er twee IP-adressen weergegeven in deze tegel. Elk IP-adres vertegenwoordigt een domeincontroller voor uw beheerde domein.

3. Klik op de kopieerknop naast het eerste IP-adres om het naar het klembord te kopiëren. Klik vervolgens op de knop **DNS-servers configureren**.

4. Plak het eerste IP-adres in het tekstvak **DNS-server toevoegen** in de blade **DNS-servers**. Schuif horizontaal naar links om het tweede IP-adres te kopiëren en plak het in het tekstvak **DNS-server toevoegen**.

    ![Domain Services: DNS bijwerken](./media/getting-started/domain-services-update-dns.png)

5. Klik wanneer u klaar bent op **Opslaan** om de DNS-servers voor het virtuele netwerk bij te werken.

> [!NOTE]
> De nieuwe DNS-instellingen worden pas op de virtuele machines toegepast na het opnieuw opstarten. Als u de bijgewerkte DNS-instellingen meteen wilt toepassen, activeert u opnieuw opstarten via de portal, PowerShell of de CLI.
>
>

## <a name="next-step"></a>Volgende stap
[Taak 5: wachtwoordsynchronisatie inschakelen voor Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)
