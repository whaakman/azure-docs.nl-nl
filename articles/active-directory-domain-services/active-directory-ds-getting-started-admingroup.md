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
ms.date: 09/26/2017
ms.author: maheshu
ms.openlocfilehash: 2a171490faf9804196c93d33c5ee74d22533c044
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Inschakelen van Azure Active Directory Domain Services met Azure portal


## <a name="task-3-configure-administrative-group"></a>Taak 3: beheergroep configureren
In deze taak voor de configuratie maakt u een beheergroep in uw Azure AD-directory. Deze speciale beheerdersgroep heet *AAD DC beheerders*. Leden van deze groep worden verleend beheerdersmachtigingen op de computers die zijn met het domein zijn toegevoegd aan het beheerde domein. Op de machines domein, wordt deze groep toegevoegd aan de groep administrators. Leden van deze groep kunnen ook extern bureaublad gebruiken extern verbinding maken met domein-machines.

> [!NOTE]
> U bent niet gemachtigd domeinbeheerder of ondernemingsbeheerder op het beheerde domein die u hebt gemaakt met behulp van Azure Active Directory Domain Services. Op de beheerde domeinen, worden deze machtigingen zijn gereserveerd door de service en zijn niet beschikbaar gesteld aan gebruikers van de tenant. Echter kunt u de speciale beheergroep gemaakt in deze taak voor de configuratie kunt u sommige bevoorrechte bewerkingen uitvoeren. Deze bewerkingen zijn computers toevoegen aan het domein en Groepsbeleid configureren die deel uitmaken van de beheergroep op domein-machines.
>

Maakt de wizard automatisch de beheergroep in uw Azure AD-directory. Deze groep wordt 'AAD DC-beheerders' genoemd. Als u een bestaande groep met deze naam in uw Azure AD-directory hebt, wordt in de wizard deze groep selecteert. U kunt configureren groepslidmaatschap met de **beheerdersgroep** wizardpagina.

1. Voor het configureren van groepslidmaatschap, klikt u op **AAD DC beheerders**.

    ![Groepslidmaatschap configureren](./media/getting-started/domain-services-blade-admingroup.png)

2. Klik op de **leden toevoegen** knop gebruikers van uw Azure AD-directory toevoegen aan de groep Administrators.

3. Wanneer u klaar bent, klikt u op **OK** op naar de **samenvatting** pagina van de wizard.

4. Op de **samenvatting** pagina van de wizard controleert u de configuratie-instellingen voor het beheerde domein. U kunt teruggaan naar elke stap van de wizard te wijzigen, indien nodig. Wanneer u klaar bent, klikt u op **OK** om de nieuwe beheerde domein te maken.

    ![Samenvatting](./media/getting-started/domain-services-blade-summary.png)

5. U ziet een melding dat de voortgang van uw Azure AD Domain Services-implementatie geeft. Klik op de melding om te zien van gedetailleerde uitgevoerd voor de implementatie.

    ![Melding - implementatie wordt uitgevoerd](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a>Inrichten van uw beheerde domein
Het proces voor het leveren van uw beheerde domein kan een uur duren.

1. Tijdens de implementatie uitgevoerd wordt, kunt u zoeken naar 'domeinservices' in de **zoeken bronnen** zoekvak. Selecteer **Azure AD Domain Services** uit de zoekresultaten. De **Azure AD Domain Services** blade geeft een lijst van het beheerde domein dat wordt ingericht.

    ![Zoeken naar beheerde domein wordt ingericht](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Klik op de naam van het beheerde domein (bijvoorbeeld, contoso100.com') voor meer informatie over het beheerde domein.

    ![Domain Services - status voor inrichten heeft](./media/getting-started/domain-services-provisioning-state.png)

3. De **overzicht** tabblad ziet u het beheerde domein momenteel wordt ingericht. U kunt het beheerde domein niet configureren, totdat deze volledig is ingericht. Dit kan een uur duren voor uw beheerde domein om te worden volledig is ingericht.

    ![Domain Services - tabblad Overzicht tijdens de Inrichtingsstatus ](./media/getting-started/domain-services-provisioning-state-details.png)

4. Wanneer het beheerde domein volledig is ingericht, de **overzicht** tabblad ziet u de status van het domein als **met**.

    ![Domain Services: tabblad Overzicht nadat het domein volledig is ingericht](./media/getting-started/domain-services-provisioned.png)

5. Op de **eigenschappen** tabblad ziet u twee IP-adressen op welk domein domeincontrollers beschikbaar voor het virtuele netwerk zijn.

    ![Domain Services - tabblad Eigenschappen nadat het volledig is ingericht](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="need-help"></a>Hulp nodig?
Het duurt een uur of twee voor beide domeincontrollers voor uw beheerde domein moeten worden ingericht. Als uw implementatie is mislukt of is vastgelopen in de status 'In behandeling' voor meer dan een paar uur, gerust [voor hulp contact op met het productteam](active-directory-ds-contact-us.md).


## <a name="next-step"></a>Volgende stap
[Taak 4: DNS-instellingen bijwerken voor het virtuele Azure-netwerk](active-directory-ds-getting-started-dns.md)
