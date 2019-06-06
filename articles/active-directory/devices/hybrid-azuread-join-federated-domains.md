---
title: Hybride Azure Active Directory-deelname configureren voor federatieve domeinen | Microsoft Docs
description: Hybride Azure Active Directory-koppeling configureren voor federatieve domeinen.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 600d6b9f1eb8d8073e1658dd5b8196a3d8137e42
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733723"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Zelfstudie: Hybride Azure Active Directory-deelname configureren voor federatieve domeinen

Een apparaat is op een soortgelijke manier aan een gebruiker een andere core-identiteit die u wilt beveiligen en gebruiken om uw resources beveiligen op elk gewenst moment en vanaf elke locatie. U kunt dit doel te bereiken door te halen en het beheren van apparaat-id's in Azure AD met behulp van een van de volgende methoden:

- Azure AD-koppeling
- Hybride Azure AD-koppeling
- Azure AD-registratie

Door uw apparaten naar Azure AD te brengen, optimaliseert u de productiviteit van uw gebruikers via eenmalige aanmelding (SSO) bij al uw on-premises bronnen en cloudbronnen. Tegelijk kunt u de toegang tot uw on-premises bronnen en cloudbronnen beveiligen met [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md).

In deze zelfstudie leert u hoe u hybride Azure AD join voor AD Domeincomputers apparaten in een federatieve omgeving met behulp van AD FS configureren.

> [!NOTE]
> Als uw federatieve omgeving een id-provider dan AD FS gebruikt is, moet u ervoor te zorgen dat uw id-provider WS-Trust-protocol ondersteunt. WS-Trust nodig is voor het verifiëren van uw Windows huidige hybride Azure AD gekoppelde apparaten met Azure AD. Bovendien, als u Windows downlevel-apparaten die u nodig hebt in hybride Azure AD join hebt, moet uw id-provider WIAORMULTIAUTHN deze ondersteunt. 


> [!div class="checklist"]
> * Hybride Azure AD-koppeling configureren
> * Downlevel Windows-apparaten inschakelen
> * De registratie verifiëren
> * Problemen oplossen

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat u bekend bent met:

- [Inleiding tot identiteit Apparaatbeheer in Azure Active Directory](../device-management-introduction.md)
- [De implementatie van uw hybride Azure Active Directory-deelname plannen](hybrid-azuread-join-plan.md)
- [Hoe u gecontroleerde validatie van hybride Azure AD join doet](hybrid-azuread-join-control.md)

Voor het configureren van het scenario in deze zelfstudie hebt u het volgende nodig:

- Windows Server 2012 R2 met AD FS
- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) versie 1.1.819.0 of hoger.

Vanaf versie 1.1.819.0 bevat Azure AD Connect een wizard om hybride Azure AD-koppeling te configureren. Met de wizard kunt u het configuratieproces aanzienlijk vereenvoudigen. De gerelateerde wizard:

- Configureert de serviceverbindingspunten (SCP) voor apparaatregistratie
- Maakt een back-up van uw bestaande vertrouwensrelatie van de Relying Party van Azure AD
- Werkt de claimregels in uw Azure AD-vertrouwensrelatie bij

De configuratiestappen in dit artikel zijn gebaseerd op deze wizard. Als u een oudere versie van Azure AD Connect hebt geïnstalleerd, werkt u deze bij naar 1.1.819 of hoger. Als het installeren van de meest recente versie van Azure AD Connect kan niet worden gebruikt voor u, raadpleegt u [het handmatig configureren van hybride Azure AD join](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual).

Voor hybride Azure AD-koppeling moeten de apparaten toegang tot de volgende Microsoft-bronnen hebben vanuit het netwerk van uw organisatie:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- De STS van uw organisatie (federatieve domeinen)
- `https://autologon.microsoftazuread-sso.com` (als u naadloze eenmalige aanmelding gebruikt of van plan bent te gebruiken)

Beginnen met Windows 10-1803, als de momentopname Hybrid Azure AD-join voor federatieve omgeving met behulp van AD FS is mislukt, we zijn afhankelijk van de Azure AD Connect voor synchronisatie van het computerobject in Azure AD dat vervolgens gebruikt wordt voor het voltooien van de device Registration service voor hybride Azure AD Neem deel aan. Verifieer dat Azure AD Connect de computerobjecten heeft gesynchroniseerd van de apparaten die u hybride Azure AD-gekoppeld wilt maken. Als de computerobjecten bij specifieke organisatie-eenheden (OE) horen, moeten deze OE’s ook worden geconfigureerd voor synchronisatie in Azure AD Connect. Zie voor meer informatie over het synchroniseren van computerobjecten met Azure AD Connect het artikel op [Configure filtering met Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering#organizational-unitbased-filtering).

Als uw organisatie toegang tot Internet via een uitgaande proxy vereist, wordt aangeraden [implementatie van Web Proxy Auto-Discovery (WPAD)](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) Windows 10-computers te doen met Azure AD-apparaatregistratie inschakelen. Als u problemen hebt met het configureren en beheren van WPAD, gaat u naar [het oplossen van automatische detectie](https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10)). 

Als u niet met behulp van WPAD en proxy-instellingen configureren op uw computer wilt, kunt u doen dus beginnen met Windows 10 1709, door [WinHTTP-instellingen met behulp van een groepsbeleidsobject (GPO) configureren](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/).

> [!NOTE]
> Als u proxy-instellingen op uw computer met behulp van WinHTTP-instellingen configureert, mislukken alle computers die geen verbinding maken met de geconfigureerde proxy verbinding maken met internet.

Als uw organisatie internettoegang via een geverifieerde uitgaande proxy vereist, moet u ervoor zorgen dat uw Windows 10-computers succesvol kunnen verifiëren bij de uitgaande proxy. Omdat Windows 10-computers apparaatregistratie uitvoeren via machinecontext, moet u verificatie van een uitgaande proxy configureren via machinecontext. Vraag uw provider van de uitgaande proxy naar de configuratievereisten.

## <a name="configure-hybrid-azure-ad-join"></a>Hybride Azure AD-koppeling configureren

Voor het configureren van een hybride Azure AD-koppeling via Azure AD Connect hebt u het volgende nodig:

- De referenties van een globale beheerder voor uw Azure AD-tenant.  
- De referenties van een ondernemingsbeheerder voor elk van de forests.
- De referenties van uw AD FS-beheerder.

**Ga als volgt te werk om een hybride Azure AD-koppeling te configureren via Azure AD Connect:**

1. Start Azure AD Connect en klik op **Configureren**.

   ![Welkom](./media/hybrid-azuread-join-federated-domains/11.png)

1. Selecteer op de pagina **Extra taken** de optie **Apparaatopties configureren** en klik op **Volgende**.

   ![Extra taken](./media/hybrid-azuread-join-federated-domains/12.png)

1. Klik op de pagina **Overzicht** op **Volgende**.

   ![Overzicht](./media/hybrid-azuread-join-federated-domains/13.png)

1. Voer op de pagina **Verbinding maken met Azure AD** de referenties in van een globale beheerder voor uw Azure AD-tenant en klik op **Volgende**.

   ![Verbinding maken met Azure AD](./media/hybrid-azuread-join-federated-domains/14.png)

1. Selecteer op de pagina **Apparaatopties** de optie **Hybride Azure AD-koppeling configureren** en klik op **Volgende**.

   ![Apparaatopties](./media/hybrid-azuread-join-federated-domains/15.png)

1. Voer op de pagina **SCP** de volgende stappen uit en klik op **Volgende**:

   ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

   1. Selecteer de forest.
   1. Selecteer de verificatieservice. U moet AD FS-server selecteren tenzij uw organisatie alleen maar Windows 10-clients heeft en u de synchronisatie van computers/apparaten hebt geconfigureerd, of als uw organisatie van SeamlessSSO gebruikmaakt.
   1. Klik op **Toevoegen** om de referenties van een ondernemingsbeheerder in te voeren.

1. Selecteer op de pagina **Apparaatbesturingssystemen** de besturingssystemen die worden gebruikt door apparaten in uw Active Directory-omgeving en klik op **Volgende**.

   ![Apparaatbesturingssysteem](./media/hybrid-azuread-join-federated-domains/17.png)

1. Voer op de pagina **Federatieconfiguratie** de referenties van uw AD FS-beheerder in en klik op **Volgende**.

   ![Federatieconfiguratie](./media/hybrid-azuread-join-federated-domains/18.png)

1. Klik op de pagina **Gereed om te configureren** op **Configureren**.

   ![Klaar om te configureren](./media/hybrid-azuread-join-federated-domains/19.png)

1. Klik op de pagina **Configuratie voltooid** op **Afsluiten**.

   ![Configuratie voltooid](./media/hybrid-azuread-join-federated-domains/20.png)

## <a name="enable-windows-down-level-devices"></a>Downlevel Windows-apparaten inschakelen

Als sommige van uw domein-gekoppelde apparaten downlevel Windows-apparaten zijn, moet u:

- De lokale intranetinstellingen voor apparaatregistratie configureren
- Microsoft Workplace Join voor Windows downlevel-computers installeren

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>De lokale intranetinstellingen voor apparaatregistratie configureren

Om de hybride Azure AD-koppeling van uw downlevel Windows-apparaten succesvol te voltooien en om certificaatprompts te vermijden wanneer apparaten worden geverifieerd bij Azure AD, kunt u een beleid naar uw domein-gekoppelde apparaten pushen om de volgende URL’s toe te voegen aan de zone Lokaal intranet in Internet Explorer:

- `https://device.login.microsoftonline.com`
- De beveiligingstokenservice (STS - federatieve domeinen) van uw organisatie
- `https://autologon.microsoftazuread-sso.com` (voor naadloze eenmalige aanmelding).

Daarnaast moet u **Statusbalkupdates via scripts toestaan** inschakelen in de zone Lokaal intranet van de gebruiker.

### <a name="install-microsoft-workplace-join-for-windows-down-level-computers"></a>Microsoft Workplace Join voor Windows downlevel-computers installeren

Voor het registreren van Windows downlevel-apparaten, organisaties moeten installeren [Microsoft Workplace Join voor Windows 10-computers](https://www.microsoft.com/download/details.aspx?id=53554) beschikbaar is op het Microsoft Download Center.

U kunt het pakket implementeren met behulp van een software-distributiesysteem zoals [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). Het pakket biedt ondersteuning voor de standaard installatie op de achtergrond-opties met de stille parameter. De huidige vertakking van Configuration Manager biedt voordelen ten opzichte van eerdere versies, zoals de mogelijkheid voor het bijhouden van voltooide registraties.

Het installatieprogramma maakt een geplande taak op het systeem die wordt uitgevoerd in de context van de gebruiker. De taak wordt geactiveerd wanneer de gebruiker een aanmelding voor Windows. De taak op de achtergrond lid wordt van het apparaat met Azure AD met de referenties van de gebruiker na verificatie met Azure AD.

## <a name="verify-the-registration"></a>De registratie verifiëren

Om de status van de apparaatregistratie in uw Azure-tenant te verifiëren, kunt u de cmdlet **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** in de **[Azure Active Directory PowerShell-module](/powershell/azure/install-msonlinev1?view=azureadps-2.0)** gebruiken.

Wanneer u de cmdlet **Get-MSolDevice** gebruikt om de servicedetails te controleren:

- Een object met de **apparaat-ID** die overeenkomt met de ID op de Windows client moet bestaan.
- Moet de waarde voor **DeviceTrustType** op **Toegevoegd aan domein** zijn ingesteld. Dit is equivalent aan de status **Toegevoegd aan hybride Azure AD** op de pagina Apparaten in de Azure AD-portal.
- De waarde voor **Ingeschakeld** moet op **Waar** zijn ingesteld en **DeviceTrustLevel** moet **Beheerd** zijn voor apparaten die voor voorwaardelijke toegang worden gebruikt.

**Ga als volgt te werk om de servicedetails te controleren:**

1. Open **Windows PowerShell** als beheerder.
1. Typ `Connect-MsolService` om verbinding te maken met uw Azure-tenant.  
1. Typ `get-msoldevice -deviceId <deviceId>`.
1. Verifieer dat **Ingeschakeld** is ingesteld op **Waar**.

## <a name="troubleshoot-your-implementation"></a>Problemen met uw implementatie oplossen

Als u problemen ondervindt met het voltooien van de hybride Azure AD-koppeling voor domein-gekoppelde Windows-apparaten, raadpleegt u:

- [Problemen met hybride Azure AD-koppeling oplossen voor actuele Windows-apparaten](troubleshoot-hybrid-join-windows-current.md)
- [Problemen met hybride Azure AD-koppeling oplossen voor downlevel Windows-apparaten](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het beheren van apparaat-id's in de Azure AD-portal [beheren van apparaat-id's met behulp van de Azure-portal](device-management-azure-portal.md).

<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
