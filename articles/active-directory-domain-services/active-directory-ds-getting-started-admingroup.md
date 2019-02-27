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
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ergreenl
ms.openlocfilehash: 734fb5ce641d48800cef68ea79cdb258e44ac267
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56867096"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory Domain Services met behulp van de Azure-portal inschakelen

## <a name="task-3-configure-administrative-group"></a>Taak 3: de beheergroep configureren

In deze taak voor de configuratie maakt u een beheergroep in uw Azure AD-directory. Deze speciale beheergroep heet *AAD DC Administrators*. Leden van deze groep zijn beheerdersmachtigingen nodig voor computers die toegevoegd aan het beheerde domein zijn-verleend. Op domein systemen, wordt deze groep toegevoegd aan de groep administrators. Leden van deze groep kunnen bovendien extern bureaublad gebruiken extern verbinding maken met domein machines.

> [!NOTE]
> U bent niet gemachtigd domeinbeheerder of ondernemingsadministrator in het beheerde domein die u hebt gemaakt met behulp van Azure Active Directory Domain Services. Op de beheerde domeinen, worden deze machtigingen zijn gereserveerd door de service en zijn niet beschikbaar gesteld voor gebruikers binnen de tenant. Echter, kunt u de speciale beheergroep hebt gemaakt in deze taak voor de configuratie sommige bevoorrechte bewerkingen uit te voeren. Deze bewerkingen zijn onder meer computers toevoegen aan het domein, die deel uitmaken van de beheergroep op een domein machines en het configureren van Groepsbeleid.
>

Maakt de wizard automatisch de beheergroep in uw Azure AD-directory. Deze groep wordt 'AAD DC Administrators' genoemd. Als u een bestaande groep met deze naam in uw Azure AD-directory hebt, selecteert u de wizard deze groep. U kunt configureren via lidmaatschap van de **beheerdersgroep** wizardpagina.

1. Groepslidmaatschap configureren, klikt u op **AAD DC Administrators**.

    ![Groepslidmaatschap configureren](./media/getting-started/domain-services-blade-admingroup.png)

2. Klik op de **leden toevoegen** knop gebruikers uit uw Azure AD-directory toevoegen aan de groep Administrators.

3. Wanneer u klaar bent, klikt u op **OK** op naar de **samenvatting** pagina van de wizard.

## <a name="configure-synchronization"></a>Synchronisatie configureren

Azure AD Domain Services kunnen voor een volledige synchronisatie van alle gebruikers en groepen die beschikbaar zijn in Azure AD of kunt u binnen het bereik synchronisatie om te synchroniseren van alleen specifieke groepen selecteren. Als u de volledige synchronisatie kiest, wordt u **niet** kunnen kiezen binnen het bereik synchronisatie op een later tijdstip. Voor meer informatie over scoped synchronisatie, gaat u naar de [Azure AD Domain Services binnen het bereik van synchronisatie artikel](active-directory-ds-scoped-synchronization.md).

### <a name="full-synchronization"></a>Volledige synchronisatie

1. Voor volledige synchronisatie, klikt u er gewoon 'OK' aan de onderkant van het scherm als volledige nog is geselecteerd.
    ![Volledige synchronisatie](./media/active-directory-domain-services-admin-guide/create-sync-all.PNG)

### <a name="scoped-synchronization"></a>Synchronisatie van binnen het bereik

1. In-/ uitschakelen met de synchronisatieknop 'Binnen bereik' en een pagina groepen selecteren wordt weergegeven. In dit ziet u welke groepen zijn al geselecteerd worden gesynchroniseerd met uw beheerde domein.
    ![Synchronisatie van binnen het bereik](media/active-directory-domain-services-admin-guide/create-sync-scoped.PNG)
2. Klik op **groepen selecteren** in de bovenste navigatiebalk. Hier, wordt het kiezen van een groep weergegeven aan de zijde. Gebruik dit om eventuele extra groepen moeten worden gesynchroniseerd met Azure AD Domain Services. Wanneer u klaar bent, klikt u op **Selecteer** om te sluiten van het kiezen van de groep en voegt u deze groepen toe aan de geselecteerde lijst.
    ![Scoped synchronisatie groepen selecteren](media/active-directory-domain-services-admin-guide/create-sync-scoped-groupselect.PNG)
3. Klik op **OK** verplaatsen naar de pagina overzicht.

## <a name="deploy-your-managed-domain"></a>Uw beheerde domein implementeren

1. Op de **samenvatting** pagina van de wizard controleert u de configuratie-instellingen voor het beheerde domein. U kunt teruggaan naar elke stap van de wizard te wijzigen, indien nodig. Wanneer u klaar bent, klikt u op **OK** om de nieuwe beheerde domein te maken.

    ![Samenvatting](./media/getting-started/domain-services-blade-summary.png)

2. U ziet een melding waarin de voortgang van uw Azure AD Domain Services-implementatie. Klik op de melding om te zien van gedetailleerde uitgevoerd voor de implementatie.

    ![Melding - implementatie wordt uitgevoerd](./media/getting-started/domain-services-blade-deployment-in-progress.png)

## <a name="check-the-deployment-status-of-your-managed-domain"></a>Controleer de implementatiestatus van uw beheerde domein

Het proces van inrichting van uw beheerde domein kan een uur duren.

1. Wanneer uw implementatie uitgevoerd wordt, kunt u zoeken naar 'domain services' in de **zoeken naar resources** zoekvak. Selecteer **Azure AD Domain Services** uit de zoekresultaten. De **Azure AD Domain Services** blade geeft een lijst van het beheerde domein dat wordt ingericht.

    ![Zoeken naar beheerde domein wordt ingericht](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Klik op de naam van het beheerde domein (bijvoorbeeld ' contoso100.com') voor meer informatie over het beheerde domein.

    ![Domain Services - Inrichtingsstatus](./media/getting-started/domain-services-provisioning-state.png)

3. De **overzicht** tabblad ziet u dat het beheerde domein wordt momenteel ingericht. U kunt het beheerde domein niet configureren, totdat deze volledig is ingericht. Het kan een uur duren voor uw beheerde domein volledig is ingericht.

    ![Domain Services: tabblad Overzicht tijdens de Inrichtingsstatus](./media/getting-started/domain-services-provisioning-state-details.png)

4. Wanneer het beheerde domein volledig is ingericht, de **overzicht** tabblad toont de domeinstatus als **met**.

    ![Domain Services: tabblad Overzicht nadat het domein volledig is ingericht](./media/getting-started/domain-services-provisioned.png)
    >[!NOTE]
    >Azure AD Domain Services maakt tijdens het inrichtingsproces, zakelijke toepassingen met de naam 'Domain Controller Services' en 'AzureActiveDirectoryDomainControllerServices' in de adreslijst. Deze zakelijke toepassingen nodig zijn om uw beheerde domein. Het is noodzakelijk dat deze niet worden verwijderd op elk gewenst moment.
    >

5. Op de **eigenschappen** tabblad ziet u twee IP-adressen waarop domeinservices domeincontrollers beschikbaar voor het virtuele netwerk zijn.

    ![Domain Services: tabblad Eigenschappen nadat volledig is ingericht](./media/getting-started/domain-services-provisioned-properties.png)

## <a name="need-help"></a>Hulp nodig?

Het duurt een uur of twee voor beide domeincontrollers voor uw beheerde domein moet worden ingericht. Als uw implementatie is mislukt of is vastgelopen in de status 'In behandeling' voor meer dan een paar uur, gerust [voor hulp contact op met het productteam](active-directory-ds-contact-us.md).

## <a name="next-step"></a>Volgende stap

[Taak 4: DNS-instellingen bijwerken voor het virtuele Azure-netwerk](active-directory-ds-getting-started-dns.md)
