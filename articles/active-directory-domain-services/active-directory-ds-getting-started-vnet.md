---
title: 'Azure Active Directory Domain Services: een virtueel netwerk maken of selecteren | Microsoft Docs'
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
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: cb372232492e8f98ff1543798b92b4b60fc25021
ms.lasthandoff: 04/12/2017


---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a>Een virtueel netwerk voor Azure Active Directory Domain Services maken of selecteren
## <a name="before-you-begin"></a>Voordat u begint
Raadpleeg [Networking considerations for Azure AD Domain Services](active-directory-ds-networking.md) (Overwegingen voor netwerken voor Azure AD Domain Services).

## <a name="task-2-create-an-azure-virtual-network"></a>Taak 2: een virtueel netwerk van Azure maken
De volgende configuratietaak bestaat uit het maken van een virtueel Azure-netwerk met daarbinnen een subnet. U schakelt Azure Active Directory Domain Services in dit subnet binnen uw virtuele netwerk in. Als u een bestaand virtueel netwerk hebt dat u wilt gebruiken, kunt u deze stap overslaan.

> [!NOTE]
> Controleer of het virtuele netwerk van Azure dat u maakt of wilt gebruiken met Azure Active Directory Domain Services, deel uitmaakt van een Azure-regio die wordt ondersteund door Azure Active Directory Domain Services. Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te bekijken in welke Azure-regio's Azure Active Directory Domain Services beschikbaar is.
>
>Noteer de naam van het virtuele netwerk, zodat u het juiste virtuele netwerk selecteert wanneer u Azure Active Directory Domain Services inschakelt in een volgende configuratiestap.


Volg deze configuratie-instructies als u een virtueel Azure-netwerk wilt maken waarin Azure Active Directory Domain Services moet zijn ingeschakeld:

1. Ga naar de [klassieke Azure Portal](https://manage.windowsazure.com).
2. Selecteer **Netwerken** in het linkerdeelvenster.

    ![Knooppunt Netwerken](./media/active-directory-domain-services-getting-started/networks-node.png)  
    Het venster **Virtuele netwerken** wordt geopend.
3. Klik op **Nieuw** in het taakvenster onder in het venster.

    ![Venster Virtuele netwerken](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. Klik op **Netwerkservices** en selecteer **Virtueel netwerk**.
    
    ![Virtueel netwerk - Snel maken](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. Klik op **Snel maken** om een virtueel netwerk te maken.
    
6. Geef een **Naam** op voor het virtuele netwerk en stel desgewenst het volgende in: 
    * U kunt ervoor kiezen om **Adresruimte** of **Maximum aantal VM’s** op te geven voor dit netwerk. 
    * U kunt de instelling van **DNS-server** voorlopig op **Geen** laten staan. U kunt de instelling bijwerken nadat u Azure Active Directory Domain Services hebt ingeschakeld.
7. Selecteer een ondersteunde Azure-regio selecteert in de vervolgkeuzelijst **Locatie**.  
    Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te bekijken in welke Azure-regio's Azure Active Directory Domain Services beschikbaar is.
8. Klik op **Een virtueel netwerk maken** om het virtuele netwerk te maken.

    ![Een virtueel netwerk voor Azure Active Directory Domain Services maken](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. Nadat u het virtuele netwerk hebt gemaakt, selecteert u dit virtuele netwerk en klikt u op het tabblad **Configureren**.

    ![Een subnet maken](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. Klik onder **Virtual Network-adresruimten** op **Subnet toevoegen** en geef een subnet op met de naam **AaddsSubnet**. 

    ![Een subnet voor Azure Active Directory Domain Services maken](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. Klik op **Opslaan** om het subnet te maken.

## <a name="next-steps"></a>Volgende stappen
Taak 3: [Azure Active Directory Domain Services inschakelen](active-directory-ds-getting-started-enableaadds.md)

