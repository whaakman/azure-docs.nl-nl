---
title: 'Azure AD Domain Services: een virtueel netwerk maken of selecteren | Microsoft Docs'
description: Aan de slag met Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/03/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 9e933774e3b618b1584b4f24a0491eda49e42077


---
# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Een virtueel netwerk voor Azure AD Domain Services maken of selecteren
## <a name="guidelines-to-select-an-azure-virtual-network"></a>Richtlijnen voor het selecteren van een virtueel netwerk van Azure
> [!NOTE]
> **Voordat u begint**: raadpleeg [Networking considerations for Azure AD Domain Services](active-directory-ds-networking.md) (Overwegingen voor netwerken voor Azure AD Domain Services).
> 
> 

## <a name="task-2-create-an-azure-virtual-network"></a>Taak 2: een virtueel netwerk van Azure maken
De volgende configuratietaak bestaat uit het maken van een virtueel Azure-netwerk met daarbinnen een subnet. U schakelt Azure AD-domeinservices in dit subnet binnen uw virtuele netwerk in. Als u al een bestaand virtueel netwerk hebt dat u wilt gebruiken, kunt u deze stap overslaan.

> [!NOTE]
> Controleer of het virtuele netwerk van Azure dat u maakt of wilt gebruiken met Azure AD Domain Services, deel uitmaakt van een Azure-regio die wordt ondersteund door Azure AD Domain Services. Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te bekijken in welke Azure-regio's Azure AD Domain Services beschikbaar is.
> 
> 

Noteer de naam van het virtuele netwerk, zodat u het juiste virtuele netwerk selecteert wanneer u Azure AD Domain Services inschakelt in een volgende configuratiestap.

Voer de volgende configuratiestappen uit om een virtueel Azure-netwerk te maken waarin u Azure AD Domain Services wilt inschakelen.

1. Navigeer naar de **klassieke Azure-portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. Selecteer het knooppunt **Netwerken** in het linkerdeelvenster.
   
    ![Knooppunt Netwerken](./media/active-directory-domain-services-getting-started/networks-node.png)
3. Klik op **Nieuw** in het taakvenster onder aan de pagina.
   
    ![Knooppunt Virtuele netwerken](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. Selecteer bij het knooppunt **Netwerkservices** de optie **Virtueel netwerk**.
5. Klik op **Snel maken** om een virtueel netwerk te maken.
   
    ![Virtueel netwerk - Snel maken](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
6. Geef een **Naam** op voor het virtuele netwerk. U kunt ook de **Adresruimte** of het **Maximum aantal VM's** opgeven voor dit netwerk. U kunt de **DNS-serverinstelling** voorlopig op Geen laten staan. U kunt de DNS-serverinstelling bijwerken nadat u Azure AD Domain Services hebt ingeschakeld.
7. Zorg ervoor dat u een ondersteunde Azure-regio selecteert in de vervolgkeuzelijst **Locatie**. Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te bekijken in welke Azure-regio's Azure AD Domain Services beschikbaar is.
8. Klik op de knop **Een virtueel netwerk maken** om het virtuele netwerk te maken.
   
    ![Maak een virtueel netwerk voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. Nadat het virtuele netwerk is gemaakt, selecteert u het virtuele netwerk en klikt u op het tabblad **Configureren**.
   
    ![Een subnet maken](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. Navigeer naar de sectie **Adresruimten voor virtueel netwerk**. Klik op **Subnet toevoegen** en geef een subnet op met de naam **AaddsSubnet**. Klik op **Opslaan** om het subnet te maken.
    
    ![Maak een subnet voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Taak 3: Azure AD Domain Services inschakelen
Bij de volgende configuratietaak gaat u [Azure AD Domain Services inschakelen](active-directory-ds-getting-started-enableaadds.md).




<!--HONumber=Dec16_HO1-->


