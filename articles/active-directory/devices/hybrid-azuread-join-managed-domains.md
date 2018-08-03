---
title: Hoe het configureren van hybride Azure Active Directory apparaten gekoppelde | Microsoft Docs
description: Informatie over het configureren van hybride Azure Active Directory verbonden apparaten.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: f61f0649900c27a6aa3a873f60dc60b4985f5d4f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39424184"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Zelfstudie: Hybride Azure Active Directory join voor beheerde domeinen configureren

Een apparaat wordt op een soortgelijke manier aan een gebruiker een andere identiteit die u wilt beveiligen en ook gebruiken voor het beveiligen van uw resources op elk moment en de locatie. U kunt dit doel te bereiken door uw apparaten met identiteiten met Azure AD met behulp van een van de volgende methoden:

- Azure AD join
- Hybrid Azure AD Join
- Azure AD-registratie

Door uw apparaten meebrengen naar Azure AD, kunt u uw gebruikers productiviteit door middel van eenmalige aanmelding (SSO) in uw cloud en on-premises resources maximaliseren. Op hetzelfde moment, kunt u veilige toegang tot uw cloud en on-premises resources met [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md).

In deze zelfstudie leert u hoe u hybride Azure AD join voor apparaten in beheerde domeinen configureren.

> [!div class="checklist"]
> * Hybride Azure AD join configureren
> * Windows downlevel-apparaten inschakelen
> * Controleer of de gekoppelde apparaten 
> * Problemen oplossen 


## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat u bekend met bent:
    
-  [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md)
    
-  [De implementatie van uw hybride Azure Active Directory-deelname plannen](hybrid-azuread-join-plan.md)

Voor het configureren van het scenario in dit artikel, moet u de [meest recente versie van Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 of hoger) moet worden geïnstalleerd. 
 

Vanaf versie 1.1.819.0, biedt Azure AD Connect u de wizard een hybride Azure AD join configureren. De wizard kunt u het configuratieproces aanzienlijk te vereenvoudigen. De gerelateerde wizard configureert u de serviceverbindingspunten (SCP) voor device Registration service.

De configuratiestappen in dit artikel zijn gebaseerd op deze wizard. 

Hybride Azure AD join vereist dat de apparaten toegang hebben tot de volgende Microsoft-resources binnen het netwerk van uw organisatie:  

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- https://autologon.microsoftazuread-sso.com (Als u met behulp van of van plan te gebruiken van naadloze eenmalige aanmelding)

Als uw organisatie toegang tot Internet via een uitgaande proxy vereist, kunt beginnen met Windows 10 1709, u configureren proxy-instellingen op uw computer met behulp van een groepsbeleidsobject (GPO). Als uw computer wordt uitgevoerd dat alles ouder dan Windows 10 1709, moet u Web Proxy Auto-Discovery (WPAD) om in te schakelen van Windows 10-computers te doen met Azure AD-apparaatregistratie implementeren. 

Als uw organisatie toegang tot Internet via een geverifieerde uitgaande proxy vereist, moet u ervoor zorgen dat uw Windows 10-computers aan de uitgaande proxy verifiëren kunnen. Omdat Windows 10-computers worden uitgevoerd met behulp van de context van de computer het registreren van apparaten, is het nodig zijn om uitgaande proxy-verificatie met behulp van de context van de computer te configureren. Volgen met uw provider uitgaande proxy op de configuratievereisten. 



## <a name="configure-hybrid-azure-ad-join"></a>Hybride Azure AD join configureren

Als u wilt een hybride Azure AD join met behulp van Azure AD Connect hebt geconfigureerd, hebt u het volgende nodig:

- De referenties van een globale beheerder voor uw Azure AD-tenant.  

- De referenties voor ondernemingsadministrator voor elk van de forests.


**Een hybride Azure AD join met behulp van Azure AD Connect configureren:**

1. Azure AD Connect starten en klik vervolgens op **configureren**.

    ![Welkom](./media/hybrid-azuread-join-managed-domains/11.png)

2. Op de **extra taken** weergeeft, schakelt **Apparaatopties configureren**, en klik vervolgens op **volgende**. 

    ![Extra taken](./media/hybrid-azuread-join-managed-domains/12.png)

3. Op de **overzicht** pagina, klikt u op **volgende**. 

    ![Overzicht](./media/hybrid-azuread-join-managed-domains/13.png)

4. Op de **verbinding maken met Azure AD** pagina, voert u de referenties van een globale beheerder voor uw Azure AD-tenant.  

    ![Verbinding maken met Azure AD](./media/hybrid-azuread-join-managed-domains/14.png)

5. Op de **Apparaatopties** weergeeft, schakelt **configureren Hybrid Azure AD join**, en klik vervolgens op **volgende**. 

    ![Apparaatopties](./media/hybrid-azuread-join-managed-domains/15.png)

6. Op de **SCP** pagina voor elk forest dat u met Azure AD Connect naar de SCP, voer de volgende stappen uit en klik vervolgens op **volgende**: 

    ![SCP](./media/hybrid-azuread-join-managed-domains/16.png)

    a. Selecteer het forest.

    b. Selecteer de service voor verificatie.

    c. Klik op **toevoegen** de referenties voor ondernemingsadministrator invoeren.


7. Op de **besturingssystemen van apparaten** pagina, selecteer de besturingssystemen die worden gebruikt door apparaten in uw Active Directory-omgeving en klik vervolgens op **volgende**. 

    ![Besturingssysteem apparaat](./media/hybrid-azuread-join-managed-domains/17.png)


8. Op de **klaar om te configureren** pagina, klikt u op **configureren**. 

    ![Klaar om te configureren](./media/hybrid-azuread-join-managed-domains/19.png)

9. Op de **configuratie voltooid** pagina, klikt u op **afsluiten**. 

    ![Configuratie voltooid](./media/hybrid-azuread-join-managed-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Windows downlevel-apparaten inschakelen

Als sommige van uw apparaten domein Windows downlevel-apparaten zijn, moet u naar:

- Apparaatinstellingen bijwerken
 
- De instellingen van lokaal intranet voor device Registration service configureren

### <a name="update-device-settings"></a>Apparaatinstellingen bijwerken 

Voor het registreren van Windows downlevel-apparaten, moet u om ervoor te zorgen dat de instellingen voor apparaten waarmee gebruikers apparaten registreren in Azure AD zijn ingesteld. U kunt deze instelling onder vinden in de Azure-portal:

`Home > [Name of your tenant] > Devices - Device settings`  


    
Het volgende beleid moet worden ingesteld op **alle**: **gebruikers hun apparaten kunnen registreren bij Azure AD**

![Apparaten registreren](media/hybrid-azuread-join-managed-domains/23.png)



### <a name="configure-the-local-intranet-settings-for-device-registration"></a>De instellingen van lokaal intranet voor device Registration service configureren

Met succes voltooid hybride Azure AD join van uw Windows downlevel-apparaten en om te voorkomen dat certificaat wordt gevraagd wanneer apparaten worden geverifieerd met Azure AD kunt u een beleid pushen naar uw domein apparaten de volgende URL's toevoegen aan het lokale Intranet verifiëren zone in Internet Explorer:

- `https://device.login.microsoftonline.com`

- `https://device.login.microsoftonline.com`

- `https://autologon.microsoftazuread-sso.com`.

Daarnaast moet u om in te schakelen **toestaan van updates op de statusbalk via script** in de lokale intranetzone van de gebruiker.

## <a name="verify-the-registration"></a>Controleer of de registratie

Als u wilt controleren of de status van de apparaatregistratie in uw Azure-tenant, kunt u de **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** cmdlet in de  **[Azure Active Directory PowerShell-module](/powershell/azure/install-msonlinev1?view=azureadps-2.0)**.

Wanneer u de **Get-MSolDevice** cmdlet om de servicedetails te controleren:

- Een object met de **apparaat-id** die overeenkomt met de ID op de Windows client moet bestaan.
- De waarde voor **DeviceTrustType** moet **domein**. Dit is gelijk aan de **toegevoegd aan Hybrid Azure AD** staat op de pagina apparaten in de Azure AD-portal.
- De waarde voor **ingeschakeld** moet **waar** voor apparaten die worden gebruikt voor de voorwaardelijke toegang. 


**De om servicedetails te controleren:**

1. Open **Windows PowerShell** als administrator.

2. Type `Connect-MsolService` verbinding maken met uw Azure-tenant.  

3. Typ `get-msoldevice -deviceId <deviceId>`.

6. Controleer **ingeschakeld** is ingesteld op **waar**.





## <a name="troubleshoot-your-implementation"></a>Uw implementatie oplossen

Als u problemen ondervindt bij het invullen van hybride Azure AD join voor domein gekoppelde Windows-apparaten, Zie:

- [Oplossen van problemen met Hybrid Azure AD join voor de huidige Windows-apparaten](troubleshoot-hybrid-join-windows-current.md)
- [Oplossen van problemen met Hybrid Azure AD join voor Windows downlevel-apparaten](troubleshoot-hybrid-join-windows-legacy.md)


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Configure hybride Azure Active Directory join voor federatieve domeinen](hybrid-azuread-join-federated-domains.md)
> [hybride Azure Active Directory join handmatig configureren](hybrid-azuread-join-manual-steps.md)

