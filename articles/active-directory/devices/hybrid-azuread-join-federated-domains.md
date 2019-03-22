---
title: Hybride Azure Active Directory-deelname configureren voor federatieve domeinen | Microsoft Docs
description: Hybride Azure Active Directory-koppeling configureren voor federatieve domeinen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: markvi
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: e83d692f19882b05c5aa9f5fd2299503abf034b9
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58336428"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Zelfstudie: Hybride Azure Active Directory-deelname configureren voor federatieve domeinen

Net zoals een gebruiker, wordt een apparaat ook een identiteit die u wilt beschermen en die u wilt gebruiken om uw bronnen altijd en overal te beschermen. U kunt dit doel bereiken door de identiteiten van uw apparaten naar Azure AD te brengen met een van de volgende methoden:

- Azure AD-koppeling
- Hybride Azure AD-koppeling
- Azure AD-registratie

Door uw apparaten naar Azure AD te brengen, optimaliseert u de productiviteit van uw gebruikers via eenmalige aanmelding (SSO) bij al uw on-premises bronnen en cloudbronnen. Tegelijk kunt u de toegang tot uw on-premises bronnen en cloudbronnen beveiligen met [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md).

In deze zelfstudie leert u hoe u hybride Azure AD-koppeling kunt configureren voor apparaten die zijn gefedereerd met ADFS.

> [!div class="checklist"]
> * Hybride Azure AD-koppeling configureren
> * Downlevel Windows-apparaten inschakelen
> * De registratie verifiëren
> * Problemen oplossen


## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat u bekend bent met:

-  [Inleiding tot apparaatbeheer in Azure Active Directory](../device-management-introduction.md)

-  [De implementatie van uw hybride Azure Active Directory-deelname plannen](hybrid-azuread-join-plan.md)

-  [De hybride Azure AD-deelname van uw apparaten beheren](hybrid-azuread-join-control.md)



Voor het configureren van het scenario in deze zelfstudie hebt u het volgende nodig:

- Windows Server 2012 R2 met AD FS

- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) versie 1.1.819.0 of hoger. 
 

Vanaf versie 1.1.819.0 bevat Azure AD Connect een wizard om hybride Azure AD-koppeling te configureren. Met de wizard kunt u het configuratieproces aanzienlijk vereenvoudigen. De gerelateerde wizard:

- Configureert de serviceverbindingspunten (SCP) voor apparaatregistratie

- Maakt een back-up van uw bestaande vertrouwensrelatie van de Relying Party van Azure AD

- Werkt de claimregels in uw Azure AD-vertrouwensrelatie bij

De configuratiestappen in dit artikel zijn gebaseerd op deze wizard. Als u een oudere versie van Azure AD Connect hebt geïnstalleerd, werkt u deze bij naar 1.1.819 of hoger. Zie [Apparaatregistratie handmatig configureren](../device-management-hybrid-azuread-joined-devices-setup.md) als het voor u niet mogelijk is om de nieuwste versie van Azure AD Connect te installeren.

Voor hybride Azure AD-koppeling moeten de apparaten toegang tot de volgende Microsoft-bronnen hebben vanuit het netwerk van uw organisatie:  

- https\://enterpriseregistration.windows.net
- https\://login.microsoftonline.com
- https\://device.login.microsoftonline.com
- De STS van uw organisatie (federatieve domeinen)
- https\://autologon.microsoftazuread-sso.com (als u naadloze eenmalige aanmelding gebruikt of van plan bent te gebruiken)

Vanaf Windows 10 1803 is het zo dat als de directe Hybrid Azure AD-koppeling mislukt voor federatieve domeinen zoals AD FS, we vertrouwen op Azure AD Connect om het computerobject in Azure AD te synchroniseren dat vervolgens wordt gebruikt om de apparaatregistratie voor Hybrid Azure AD-koppeling te voltooien.

Als uw organisatie internettoegang via een uitgaande proxy vereist, kunt u vanaf Windows 10 1709 [proxyinstellingen op uw computer configureren met behulp van een groepsbeleidsobject (GPO)](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/). Als op uw computer een versie van Windows ouder dan 10 1709 wordt uitgevoerd, moet u Web Proxy Auto-Discovery (WPAD) implementeren om Windows 10-computers in staat te stellen apparaten te registreren met Azure AD. 

Als uw organisatie internettoegang via een geverifieerde uitgaande proxy vereist, moet u ervoor zorgen dat uw Windows 10-computers succesvol kunnen verifiëren bij de uitgaande proxy. Omdat Windows 10-computers apparaatregistratie uitvoeren via machinecontext, moet u verificatie van een uitgaande proxy configureren via machinecontext. Vraag uw provider van de uitgaande proxy naar de configuratievereisten. 


## <a name="configure-hybrid-azure-ad-join"></a>Hybride Azure AD-koppeling configureren

Voor het configureren van een hybride Azure AD-koppeling via Azure AD Connect hebt u het volgende nodig:

- De referenties van een globale beheerder voor uw Azure AD-tenant.  

- De referenties van een ondernemingsbeheerder voor elk van de forests.

- De referenties van uw AD FS-beheerder. 


**Ga als volgt te werk om een hybride Azure AD-koppeling te configureren via Azure AD Connect:**

1. Start Azure AD Connect en klik op **Configureren**.

    ![Welkom](./media/hybrid-azuread-join-federated-domains/11.png)

2. Selecteer op de pagina **Extra taken** de optie **Apparaatopties configureren** en klik op **Volgende**. 

    ![Extra taken](./media/hybrid-azuread-join-federated-domains/12.png)

3. Klik op de pagina **Overzicht** op **Volgende**. 

    ![Overzicht](./media/hybrid-azuread-join-federated-domains/13.png)

4. Voer op de pagina **Verbinding maken met Azure AD** de referenties in van een globale beheerder voor uw Azure AD-tenant en klik op **Volgende**.   

    ![Verbinding maken met Azure AD](./media/hybrid-azuread-join-federated-domains/14.png)

5. Selecteer op de pagina **Apparaatopties** de optie **Hybride Azure AD-koppeling configureren** en klik op **Volgende**. 

    ![Apparaatopties](./media/hybrid-azuread-join-federated-domains/15.png)

6. Voer op de pagina **SCP** de volgende stappen uit en klik op **Volgende**: 

    ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

    a. Selecteer de forest.

    b. Selecteer de verificatieservice. U moet AD FS-server selecteren tenzij uw organisatie alleen maar Windows 10-clients heeft en u de synchronisatie van computers/apparaten hebt geconfigureerd, of als uw organisatie van SeamlessSSO gebruikmaakt.

    c. Klik op **Toevoegen** om de referenties van een ondernemingsbeheerder in te voeren.


7. Selecteer op de pagina **Apparaatbesturingssystemen** de besturingssystemen die worden gebruikt door apparaten in uw Active Directory-omgeving en klik op **Volgende**. 

    ![Apparaatbesturingssysteem](./media/hybrid-azuread-join-federated-domains/17.png)

8. Voer op de pagina **Federatieconfiguratie** de referenties van uw AD FS-beheerder in en klik op **Volgende**. 

    ![Federatieconfiguratie](./media/hybrid-azuread-join-federated-domains/18.png)

9. Klik op de pagina **Gereed om te configureren** op **Configureren**. 

    ![Klaar om te configureren](./media/hybrid-azuread-join-federated-domains/19.png)

10. Klik op de pagina **Configuratie voltooid** op **Afsluiten**. 

    ![Configuratie voltooid](./media/hybrid-azuread-join-federated-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Downlevel Windows-apparaten inschakelen

Als sommige van uw domein-gekoppelde apparaten downlevel Windows-apparaten zijn, moet u:

- Apparaatinstellingen bijwerken
 
- De lokale intranetinstellingen voor apparaatregistratie configureren

- Downlevel Windows-apparaten beheren 


### <a name="update-device-settings"></a>Apparaatinstellingen bijwerken 

Om downlevel Windows-apparaten te kunnen registreren, moet u ervoor zorgen dat de apparaatinstellingen zijn geconfigureerd waarmee gebruikers apparaten mogen registreren in Azure AD. In de Azure-portal vindt u deze instelling onder:

`Home > [Name of your tenant] > Devices - Device settings`  


    
Het volgende beleid moet zijn ingesteld op **Alle**: **Gebruikers mogen hun apparaten met Azure AD registreren**

![Apparaten registreren](./media/hybrid-azuread-join-federated-domains/23.png)


### <a name="configure-the-local-intranet-settings-for-device-registration"></a>De lokale intranetinstellingen voor apparaatregistratie configureren

Om de hybride Azure AD-koppeling van uw downlevel Windows-apparaten succesvol te voltooien en om certificaatprompts te vermijden wanneer apparaten worden geverifieerd bij Azure AD, kunt u een beleid naar uw domein-gekoppelde apparaten pushen om de volgende URL’s toe te voegen aan de zone Lokaal intranet in Internet Explorer:

- `https://device.login.microsoftonline.com`

- De beveiligingstokenservice (STS - federatieve domeinen) van uw organisatie

- `https://autologon.microsoftazuread-sso.com` (voor naadloze eenmalige aanmelding).

Daarnaast moet u **Statusbalkupdates via scripts toestaan** inschakelen in de zone Lokaal intranet van de gebruiker.



### <a name="control-windows-down-level-devices"></a>Downlevel Windows-apparaten beheren 

Als u downlevel Windows-apparaten wilt registreren, dient u een Windows Installer-pakket (.msi) in het Downloadcentrum te downloaden en installeren. Klik [hier](hybrid-azuread-join-control.md#control-windows-down-level-devices) voor meer informatie. 

## <a name="verify-the-registration"></a>De registratie verifiëren

Om de status van de apparaatregistratie in uw Azure-tenant te verifiëren, kunt u de cmdlet **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** in de **[Azure Active Directory PowerShell-module](/powershell/azure/install-msonlinev1?view=azureadps-2.0)** gebruiken.

Wanneer u de cmdlet **Get-MSolDevice** gebruikt om de servicedetails te controleren:

- Moet er een object bestaan met de **apparaat-id** die overeenkomt met de id op de Windows-client.
- Moet de waarde voor **DeviceTrustType** op **Toegevoegd aan domein** zijn ingesteld. Dit is equivalent aan de status **Toegevoegd aan hybride Azure AD** op de pagina Apparaten in de Azure AD-portal.
- De waarde voor **Ingeschakeld** moet op **Waar** zijn ingesteld en **DeviceTrustLevel** moet **Beheerd** zijn voor apparaten die voor voorwaardelijke toegang worden gebruikt. 


**Ga als volgt te werk om de servicedetails te controleren:**

1. Open **Windows PowerShell** als beheerder.

2. Typ `Connect-MsolService` om verbinding te maken met uw Azure-tenant.  

3. Typ `get-msoldevice -deviceId <deviceId>`.

6. Verifieer dat **Ingeschakeld** is ingesteld op **Waar**.





## <a name="troubleshoot-your-implementation"></a>Problemen met uw implementatie oplossen

Als u problemen ondervindt met het voltooien van de hybride Azure AD-koppeling voor domein-gekoppelde Windows-apparaten, raadpleegt u:

- [Problemen met hybride Azure AD-koppeling oplossen voor actuele Windows-apparaten](troubleshoot-hybrid-join-windows-current.md)
- [Problemen met hybride Azure AD-koppeling oplossen voor downlevel Windows-apparaten](troubleshoot-hybrid-join-windows-legacy.md)



## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Hybride Azure Active Directory-koppeling configureren voor beheerde domeinen](hybrid-azuread-join-managed-domains.md)
> [Hybride Azure Active Directory-koppeling handmatig configureren](hybrid-azuread-join-manual.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
