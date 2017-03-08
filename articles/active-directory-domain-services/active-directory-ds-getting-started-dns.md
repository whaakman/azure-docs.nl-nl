---
title: 'Azure AD Domain Services: DNS-instellingen bijwerken voor het virtuele Azure-netwerk | Microsoft Docs'
description: Aan de slag met Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 16a0019990d4839a15acb4dcac747147a6d55e1d
ms.lasthandoff: 03/07/2017


---
# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Azure AD Domain Services: DNS-instellingen bijwerken voor het virtuele Azure-netwerk
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Taak 4: DNS-instellingen bijwerken voor het virtuele Azure-netwerk
In de voorgaande configuratietaken hebt u Azure AD Domain Services ingeschakeld voor uw map. De volgende taak is om ervoor te zorgen dat computers binnen het virtuele netwerk verbinding kunnen maken met deze services en ze kunnen gebruiken. Werk de DNS-serverinstellingen voor het virtuele netwerk zo bij dat deze verwijzen naar de twee IP-adressen waarop Azure AD Domain Services beschikbaar is op het virtuele netwerk.

> [!NOTE]
> Noteer de IP-adressen voor Azure AD Domain Services die worden weergegeven op het tabblad **Configureren** van uw directory nadat u Azure AD Domain Services hebt ingeschakeld voor de directory.
>
>

Voer de volgende configuratiestappen uit om de DNS-serverinstellingen bij te werken voor het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld.

1. Navigeer naar de **klassieke Azure-portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. Selecteer het knooppunt **Netwerken** in het linkerdeelvenster.

    ![Knooppunt Virtuele netwerken](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. Selecteer op het tabblad **Virtuele netwerken** het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld om de eigenschappen te bekijken.
4. Klik op het tabblad **Configureren**.

    ![Knooppunt Virtuele netwerken](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. Geef in de sectie **DNS-servers** de IP-adressen van Azure AD Domain Services op.
6. Zorg ervoor dat u beide IP-adressen opgeeft die werden weergegeven in de sectie **Domeinservices** op het tabblad **Configureren** van uw directory.
7. Klik op **Opslaan** in het taakvenster onder aan de pagina om de DNS-serverinstellingen voor dit virtuele netwerk op te slaan.

   ![De DNS-serverinstellingen bijwerken voor het virtuele netwerk](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
> Wanneer u de DNS-serverinstellingen voor het virtuele netwerk hebt bijgewerkt, kan het enige tijd duren voordat de bijgewerkte DNS-configuratie is doorgevoerd in de virtuele machines in het netwerk. Als een virtuele machine geen verbinding kan maken met het domein, kunt u de DNS-cache leegmaken (bijvoorbeeld 'ipconfig/flushdns') op de virtuele machine. Met deze opdracht worden de DNS-instellingen op de virtuele machine vernieuwd.
>
>

## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Taak 5: wachtwoordsynchronisatie inschakelen voor Azure AD Domain Services
Voor de volgende configuratietaak moet u [wachtwoordsynchronisatie inschakelen voor Azure AD Domain Services](active-directory-ds-getting-started-password-sync.md).

