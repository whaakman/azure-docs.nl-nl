---
title: 'Azure Active Directory Domain Services: DNS-instellingen bijwerken voor het virtuele Azure-netwerk | Microsoft Docs'
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
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: abb27292d4b5533fe6f3d66d6921fea8c82f18dd
ms.lasthandoff: 04/12/2017


---
# <a name="update-dns-settings-for-the-azure-virtual-network"></a>DNS-instellingen bijwerken voor het virtuele Azure-netwerk
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Taak 4: DNS-instellingen bijwerken voor het virtuele Azure-netwerk
In de voorgaande configuratietaken hebt u Azure Active Directory Domain Services ingeschakeld voor uw directory. De volgende taak is om ervoor te zorgen dat computers binnen het virtuele netwerk verbinding kunnen maken met deze services en ze kunnen gebruiken. In dit artikel werkt u de DNS-serverinstellingen voor het virtuele netwerk zo bij dat deze verwijzen naar de twee IP-adressen waarop Azure Active Directory Domain Services beschikbaar is op het virtuele netwerk.

> [!NOTE]
> Noteer de IP-adressen voor Azure Active Directory Domain Services die worden weergegeven op het tabblad **Configureren** van uw directory nadat u Azure Active Directory Domain Services hebt ingeschakeld voor de directory.
>
>

Voer de volgende stappen uit om de DNS-serverinstellingen bij te werken voor het virtuele netwerk waarin u Azure Active Directory Domain Services hebt ingeschakeld:

1. Ga naar de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Selecteer **Netwerken** in het linkerdeelvenster.  
    Het venster **Netwerken** wordt geopend.

    ![Venster met virtuele netwerken](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. Selecteer op het tabblad **Virtuele netwerken** het virtuele netwerk waarin u Azure Active Directory Domain Services hebt ingeschakeld om de eigenschappen te bekijken.
4. Klik op het tabblad **Configureren**.

    ![Venster met virtuele netwerken](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. In de sectie **DNS-servers** geeft u beide IP-adressen op die werden weergegeven in de sectie **Domeinservices** op het tabblad **Configureren** van uw directory.
6. Klik op **Opslaan** in het taakvenster onderin om de DNS-serverinstellingen voor dit virtuele netwerk op te slaan.

   ![De DNS-serverinstellingen bijwerken voor het virtuele netwerk](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
> Wanneer u de DNS-serverinstellingen voor het virtuele netwerk hebt bijgewerkt, kan het enige tijd duren voordat de bijgewerkte DNS-configuratie is doorgevoerd in de virtuele machines in het netwerk. Als een virtuele machine geen verbinding kan maken met het domein, kunt u de DNS-cache ('ipconfig /flushdns') op de virtuele machine leegmaken. Met deze opdracht worden de DNS-instellingen op de virtuele machine vernieuwd.
>
>

## <a name="next-steps"></a>Volgende stappen
Taak 5: [wachtwoordsynchronisatie inschakelen voor Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)

