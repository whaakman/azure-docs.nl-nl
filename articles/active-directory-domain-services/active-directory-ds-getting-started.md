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
ms.date: 10/26/2017
ms.author: maheshu
ms.openlocfilehash: 16b206b97f71eeffbb4fddb9e447649034998672
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Inschakelen van Azure Active Directory Domain Services met Azure portal
Dit artikel ziet het inschakelen van Azure Active Directory Domain Services (Azure AD DS) met de Azure portal.

Starten de **inschakelen Azure AD Domain Services** wizard de volgende stappen uit:

1. Ga naar de [Azure Portal](https://portal.azure.com).
2. Klik in het linkerdeelvenster op **nieuw**.
3. In de **nieuw** typt **domeinservices** in de zoekbalk.

    ![Zoeken naar domeinservices](./media/getting-started/search-domain-services.png)

4. Schakel het selectievakje **Azure AD Domain Services** uit de lijst met zoeksuggesties. Op de **Azure AD Domain Services** pagina, klikt u op de **maken** knop.

    ![Domain services weergeven](./media/getting-started/domain-services-blade.png)

5. De **inschakelen Azure AD Domain Services** wizard wordt gestart.


## <a name="task-1-configure-basic-settings"></a>Taak 1: basisinstellingen configureren
In de **basisbeginselen** pagina van de wizard kunt u de DNS-domeinnaam voor het beheerde domein opgeven. U kunt ook de resourcegroep en Azure-locatie waarnaar het beheerde domein moet worden geïmplementeerd.

![Basisprincipes configureren](./media/getting-started/domain-services-blade-basics.png)

1. Kies de **DNS-domeinnaam** voor uw beheerde domein.

   > [!NOTE]
   > **Richtlijnen voor het selecteren van een DNS-domeinnaam**
   > * **Ingebouwde domeinnaam:** standaard de wizard geeft de standaard/ingebouwde-in domein van de map (met een **. onmicrosoft.com** achtervoegsel) voor u. Als u kiest voor beveiligde LDAP-toegang tot het beheerde domein via internet, verwacht dat de problemen met het maken van een openbare DNS-record of een beveiligde LDAP-certificaat verkrijgen van een openbare Certificeringsinstantie voor deze domeinnaam. Microsoft is eigenaar van de *. onmicrosoft.com* domein en de CA geeft geen certificaten garanderen van dit domein.
   * **Aangepaste domeinnamen:** u kunt ook typen in een aangepaste domeinnaam. In dit voorbeeld is de aangepaste domeinnaam *contoso100.com*.
   * **Niet-routeerbaar domeinachtervoegsels:** algemeen raden We het achtervoegsel voor een niet-routeerbare domeinnaam te vermijden. Bijvoorbeeld, is het beter om het maken van een domein met de DNS-domein de naam 'contoso.local'. Het achtervoegsel '.local' kan niet worden omgeleid en de oorzaak van problemen met DNS-omzetting.
   * **Domeinbeperkingen voorvoegsel:** het voorvoegsel van de opgegeven domeinnaam (bijvoorbeeld *contoso100* in de *contoso100.com* domeinnaam) moet 15 of minder tekens bevatten. U kunt een beheerd domein maken met een voorvoegsel langer is dan 15 tekens.
   * **Netwerk naamconflicten:** Zorg ervoor dat de DNS-domeinnaam die u hebt gekozen voor het beheerde domein niet in het virtuele netwerk bestaat nog. Controleer in het bijzonder of:
       * U hebt al een Active Directory-domein met dezelfde DNS-domeinnaam in het virtuele netwerk.
       * Het virtuele netwerk waar u van plan bent om in te schakelen van het beheerde domein heeft een VPN-verbinding met uw on-premises netwerk. In dit scenario, zorg ervoor dat u hebt geen een domein met dezelfde DNS-domeinnaam op uw on-premises netwerk.
       * Of het virtuele netwerk een bestaande cloudservice met deze naam bevat.
    >

2. Selecteer de Azure **abonnement** in die u wilt maken van het beheerde domein.

3. Selecteer de **resourcegroep** waartoe het beheerde domein moet behoren. U kunt ofwel de **nieuw** of **gebruik bestaande** opties voor het selecteren van de resourcegroep.

4. Kies het Azure **locatie** in het beheerde domein moet worden gemaakt. Op de **netwerk** pagina van de wizard, ziet u alleen virtuele netwerken die deel uitmaken van de locatie die u hebt geselecteerd.

5. Wanneer u klaar bent, klikt u op **OK** op naar de **netwerk** pagina van de wizard.


## <a name="next-step"></a>Volgende stap
[Taak 2: De netwerkinstellingen configureren](active-directory-ds-getting-started-network.md)
