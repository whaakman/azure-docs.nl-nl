---
title: 'Azure Active Directory Domain Services: Aan de slag | Microsoft Docs'
description: Inschakelen van Azure Active Directory Domain Services met Azure portal
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: maheshu
ms.openlocfilehash: 858adf2b0a0431078561a6f0edf80860927f8233
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Inschakelen van Azure Active Directory Domain Services met Azure portal


## <a name="before-you-begin"></a>Voordat u begint
Raadpleeg [Networking considerations for Azure AD Domain Services](active-directory-ds-networking.md) (Overwegingen voor netwerken voor Azure AD Domain Services).


## <a name="task-2-configure-network-settings"></a>Taak 2: netwerkinstellingen configureren
De volgende configuratietaak is het maken van een virtuele Azure-netwerk en een specifieke subnet in het. U schakelt Azure Active Directory Domain Services in dit subnet binnen uw virtuele netwerk in. U kunt ook kiezen een bestaand virtueel netwerk en maakt u een specifieke subnet in het.

1. Klik op **virtueel netwerk** selecteren van een virtueel netwerk.
    > [!NOTE]
    > **Klassieke virtuele netwerken worden niet ondersteund voor nieuwe implementaties.** Klassieke virtuele netwerken worden niet ondersteund voor nieuwe implementaties. Bestaande beheerde domeinen die zijn geïmplementeerd in de klassieke virtuele netwerken nog steeds worden ondersteund. We leveren de mogelijkheid voor het migreren van een beheerd domein van bestaande uit een klassiek virtueel netwerk met een virtueel netwerk van Resource Manager in de nabije toekomst.
    >

2. Op de **virtueel netwerk kiezen** pagina ziet u alle bestaande virtuele netwerken. U ziet alleen de virtuele netwerken die deel uitmaken van de resourcegroep en Azure-locatie die u hebt geselecteerd op de **basisbeginselen** wizardpagina.
3. Kies het virtuele netwerk waarin Azure AD Domain Services moeten worden ingeschakeld. U kunt een bestaand virtueel netwerk kiezen of een nieuwe maken.

  > [!TIP]
  > **U kunt uw beheerde domein niet verplaatsen naar een ander virtueel netwerk nadat u Azure AD Domain Services hebt ingeschakeld.** Kies het juiste virtuele netwerk te maken voor uw beheerde domein. Nadat u een beheerd domein hebt gemaakt, niet u het verplaatsen naar een ander virtueel netwerk, zonder te verwijderen van het beheerde domein. Het is raadzaam controleren de [networking overwegingen voor Azure Active Directory Domain Services](active-directory-ds-networking.md) voordat u doorgaat.  
  >

4. **Virtueel netwerk maken:** klikt u op **nieuw** een nieuw virtueel netwerk maken. We raden met behulp van een toegewezen subnet voor Azure AD Domain Services. Maak bijvoorbeeld een subnet met de naam DomainServices, zodat u eenvoudig andere beheerders om te begrijpen wat binnen het subnet is geïmplementeerd. Klik op **OK** wanneer u klaar bent.

    ![Virtueel netwerk kiezen](./media/getting-started/domain-services-blade-network-pick-vnet.png)

  > [!WARNING]
  > Zorg ervoor dat een adresruimte die binnen de privé IP-adresruimte te kiezen. IP-adressen die u niet de eigenaar die in de openbare adresruimte zijn er fouten optreden in Azure AD Domain Services.

5. **Bestaand virtueel netwerk:** als u van plan bent om op te halen van een bestaand virtueel netwerk [een toegewezen subnet met de extensie van virtuele netwerken maken](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet), en selecteer vervolgens dat subnet. Klik op **virtueel netwerk** selecteren van de bestaande virtuele netwerk. Klik op **Subnet** het toegewezen subnet kiest in uw bestaande virtuele netwerk te maken voor uw nieuwe beheerde domein. Klik op **OK** wanneer u klaar bent.

    ![Kies subnet binnen het virtuele netwerk](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Richtlijnen voor het selecteren van een subnet**
  > 1. Gebruik een toegewezen subnet voor Azure AD Domain Services. Implementeer geen eventuele andere virtuele machines met dit subnet. Deze configuratie kunt u netwerkbeveiligingsgroepen (nsg's) configureren voor uw werkbelastingen/virtuele machines zonder te verstoren uw beheerde domein. Zie voor meer informatie [networking overwegingen voor Azure Active Directory Domain Services](active-directory-ds-networking.md).
  2. Selecteer het gatewaysubnet voor het implementeren van Azure AD Domain Services niet omdat het is niet een ondersteunde configuratie.
  3. Zorg ervoor dat het subnet dat u hebt geselecteerd voldoende beschikbare-adresruimte - ten minste 3-5 beschikbare IP-adressen en in de privé IP-adresruimte bestaat.
  >

6. Wanneer u klaar bent, klikt u op **OK** om door te gaan naar de **beheerdersgroep** pagina van de wizard.


## <a name="next-step"></a>Volgende stap
[Taak 3: Configureer beheergroep en Azure AD Domain Services inschakelen](active-directory-ds-getting-started-admingroup.md)
