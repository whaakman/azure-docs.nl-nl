---
title: 'Azure Active Directory Domain Services: Aan de slag | Microsoft Docs'
description: Azure Active Directory Domain Services met behulp van de Azure-portal inschakelen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2018
ms.author: ergreenl
ms.openlocfilehash: d73d9d269ce4c78c87e718aed752206f88276770
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54857029"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory Domain Services met behulp van de Azure-portal inschakelen


## <a name="before-you-begin"></a>Voordat u begint
Raadpleeg [Networking considerations for Azure AD Domain Services](active-directory-ds-networking.md) (Overwegingen voor netwerken voor Azure AD Domain Services).


## <a name="task-2-configure-network-settings"></a>Taak 2: de netwerkinstellingen configureren
De volgende configuratietaak is het maken van een Azure-netwerk en een toegewezen subnet in het. U schakelt Azure Active Directory Domain Services in dit subnet binnen uw virtuele netwerk in. U kunt ook een bestaand virtueel netwerk kiezen en maken van de toegewezen subnet in het.

1. Klik op **virtueel netwerk** om te selecteren van een virtueel netwerk.
    > [!NOTE]
    > **Klassieke virtuele netwerken worden niet ondersteund voor nieuwe implementaties.** Klassieke virtuele netwerken worden niet ondersteund voor nieuwe implementaties. Bestaande beheerde domeinen die zijn geïmplementeerd in klassieke virtuele netwerken worden nog steeds ondersteund. Microsoft kunt u voor het migreren van een beheerd domein van bestaande uit een klassiek virtueel netwerk met een virtueel netwerk van Resource Manager in de nabije toekomst.
    >

2. Op de **virtueel netwerk kiezen** pagina, ziet u alle bestaande virtuele netwerken. U ziet alleen de virtuele netwerken die deel uitmaken van de resourcegroep en Azure-locatie die u hebt geselecteerd op de **basisbeginselen** wizardpagina.
3. Kies het virtuele netwerk waarin Azure AD Domain Services moeten worden ingeschakeld. U kunt een bestaand virtueel netwerk kiezen of een nieuwe maken.

  > [!TIP]
  > **U kunt uw beheerde domein niet verplaatsen naar een ander virtueel netwerk nadat u Azure AD Domain Services hebt ingeschakeld.** Kies het juiste virtuele netwerk waarmee uw beheerde domein. Nadat u een beheerd domein hebt gemaakt, verplaatsen niet u deze naar een ander virtueel netwerk, zonder te verwijderen van het beheerde domein. Raadzaam de [aandachtspunten voor Azure Active Directory Domain Services voor netwerken](active-directory-ds-networking.md) voordat u doorgaat.  
  >

4. **Virtueel netwerk maken:** Klik op **nieuw** te maken van een nieuw virtueel netwerk. Gebruik een toegewezen subnet voor Azure AD Domain Services. Maak bijvoorbeeld een subnet met de naam DomainServices, zodat u eenvoudig voor andere beheerders om te begrijpen wat binnen het subnet is geïmplementeerd. Klik op **OK** wanneer u klaar bent.

    ![Virtueel netwerk kiezen](./media/getting-started/domain-services-blade-network-pick-vnet.png)

  > [!WARNING]
  > Zorg ervoor dat u kiest een adresruimte die valt binnen de privé IP-adresruimte. IP-adressen die u niet de eigenaar die zijn opgenomen in de openbare adresruimte leiden tot fouten in Azure AD Domain Services.

5. **Bestaand virtueel netwerk:** Als u van plan bent een bestaand virtueel netwerk kiezen [maken van een toegewezen subnet met de extensie van virtuele netwerken](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet), en kies dan op dat subnet. Klik op **Virtueelnetwerk** om te selecteren van het bestaande virtuele netwerk. Klik op **Subnet** om op te halen van de toegewezen subnet in uw bestaande virtuele netwerk waarin zodat uw nieuwe beheerde domein. Klik op **OK** wanneer u klaar bent.

    ![Subnet binnen het virtuele netwerk kiezen](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Richtlijnen voor het selecteren van een subnet**
  > 1. Gebruik een toegewezen subnet voor Azure AD Domain Services. Implementeer eventuele andere virtuele machines niet aan dit subnet. Deze configuratie kunt u netwerkbeveiligingsgroepen (nsg's) configureren voor uw workloads/virtuele machines zonder uw beheerde domein verstoren. Zie voor meer informatie, [aandachtspunten voor Azure Active Directory Domain Services voor netwerken](active-directory-ds-networking.md).
  2. Selecteer het gatewaysubnet voor het implementeren van Azure AD Domain Services, niet omdat het geen ondersteunde configuratie.
  3. Het subnet dat u hebt geselecteerd, moet ten minste 3 tot 5 beschikbare IP-adressen in de adresruimte hebben.
  >

6. Wanneer u klaar bent, klikt u op **OK** om door te gaan naar de **beheerdersgroep** pagina van de wizard.


## <a name="next-step"></a>Volgende stap
[Taak 3: configureren van de beheergroep en Azure AD Domain Services inschakelen](active-directory-ds-getting-started-admingroup.md)
