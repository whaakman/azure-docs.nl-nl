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
ms.openlocfilehash: 07dbc96e94c736b4e4c80dd212f0674bfeffce45
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54850413"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory Domain Services met behulp van de Azure-portal inschakelen
In dit artikel laat zien hoe Azure Active Directory Domain Services inschakelen (Azure AD DS) via Azure portal.


## <a name="before-you-begin"></a>Voordat u begint
Voor de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

* Een geldige **Azure-abonnement**.
* Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
* De **Azure-abonnement moet worden gekoppeld aan de Azure AD-directory**.
* U moet **hoofdbeheerder** bevoegdheden in de Azure AD-directory in Azure AD Domain Services inschakelen.


## <a name="enable-azure-ad-domain-services"></a>Azure AD Domain Services inschakelen

Om te starten de **inschakelen Azure AD Domain Services** wizard de volgende stappen uit:

1. Ga naar de [Azure Portal](https://portal.azure.com).
2. Klik in het linkerdeelvenster op **Een resource maken**.
3. In de **nieuw** pagina, typt u **domeinservices** in de zoekbalk typt.

    ![Zoekactie voor domain services](./media/getting-started/search-domain-services.png)

4. Schakel het selectievakje **Azure AD Domain Services** uit de lijst met suggesties. Op de **Azure AD Domain Services** pagina, klikt u op de **maken** knop.

    ![Domain services weergeven](./media/getting-started/domain-services-blade.png)

5. De **inschakelen Azure AD Domain Services** wizard wordt gestart.


## <a name="task-1-configure-basic-settings"></a>Taak 1: de basisinstellingen configureren
In de **basisbeginselen** pagina van de wizard, geeft u de DNS-domeinnaam voor het beheerde domein. U kunt ook de resourcegroep en Azure-locatie waarnaar het beheerde domein moet worden geïmplementeerd.

![Basisbeginselen configureren](./media/getting-started/domain-services-blade-basics.png)

1. Kies de **DNS-domeinnaam** voor uw beheerde domein.

   > [!NOTE]
   > **Richtlijnen voor het selecteren van een DNS-domeinnaam**
   > * **Naam van de ingebouwde domein:** Standaard geeft de wizard de standaard/ingebouwde-in-naam van de map (met een **. onmicrosoft.com** achtervoegsel) voor u. Als u ervoor kiest om in te schakelen toegang van secure LDAP aan het beheerde domein via internet, kunt u problemen met het maken van een openbare DNS-record of het verkrijgen van een certificaat voor secure LDAP via een openbare Certificeringsinstantie voor deze domeinnaam verwachten. Microsoft is eigenaar van de *. onmicrosoft.com* domein en de CA geeft geen certificaten garandeert voor dit domein.
   * **Aangepaste domeinnamen:** U kunt ook typen in een aangepaste domeinnaam. In dit voorbeeld is de aangepaste domeinnaam *contoso100.com*.
   * **Niet-routeerbare domeinachtervoegsels:** Het algemeen wordt aangeraden een achtervoegsel voor niet-routeerbare domeinnaam te vermijden. Bijvoorbeeld, is het beter om te voorkomen dat het maken van een domein met de DNS-domein naam 'contoso.local'. De DNS-achtervoegsel '.local' kan niet worden omgeleid en kan problemen veroorzaken met DNS-omzetting.
   * **Domeinbeperkingen voorvoegsel:** Het voorvoegsel van de opgegeven domeinnaam (bijvoorbeeld contoso100 in de domeinnaam *contoso100.com*) mag maximaal 15 tekens bevatten. U kunt een beheerde domein maken met het voorvoegsel langer is dan 15 tekens.
   * **Netwerk naam een conflict veroorzaakt:** Controleer of de DNS-domeinnaam die u hebt gekozen voor het beheerde domein, nog niet bestaat in het virtuele netwerk. Controleer met name of:
       * U hebt al een Active Directory-domein met dezelfde DNS-domeinnaam op het virtuele netwerk.
       * Het virtuele netwerk waar u van plan bent om in te schakelen van het beheerde domein heeft een VPN-verbinding met uw on-premises netwerk. In dit scenario, zorg ervoor dat u hebt geen een domein met dezelfde DNS-domeinnaam voor uw on-premises netwerk.
       * Of het virtuele netwerk een bestaande cloudservice met deze naam bevat.
    >

2. Selecteer het Azure **abonnement** in die u wilt maken van het beheerde domein.

3. Selecteer de **resourcegroep** waartoe het beheerde domein moet behoren. Kies de **nieuw** of **gebruik bestaande** opties voor het selecteren van de resourcegroep.

4. Kies het Azure **locatie** waarin het beheerde domein moet worden gemaakt. Op de **netwerk** pagina van de wizard, ziet u alleen virtuele netwerken die deel uitmaken van de locatie die u hebt geselecteerd.

5. Klik op **OK** op naar de **netwerk** pagina van de wizard.


## <a name="next-step"></a>Volgende stap
[Taak 2: De netwerkinstellingen configureren](active-directory-ds-getting-started-network.md)
