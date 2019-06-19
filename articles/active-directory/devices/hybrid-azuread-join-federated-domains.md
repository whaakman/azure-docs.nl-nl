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
ms.openlocfilehash: 738b4f47054081f0fb1b1a530bdf21cbf07a7726
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204703"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Zelfstudie: Hybride Azure Active Directory-deelname configureren voor federatieve domeinen

Een apparaat is, zoals een gebruiker in uw organisatie, de identiteit van een kern die u wilt beveiligen. U kunt van een apparaat-id gebruiken voor de beveiliging van uw resources op elk gewenst moment en vanaf elke locatie. U kunt dit doel te bereiken door te halen van apparaat-id's en deze beheren in Azure Active Directory (Azure AD) met behulp van een van de volgende methoden:

- Azure AD-koppeling
- Hybride Azure AD-koppeling
- Azure AD-registratie

Uw apparaten meebrengen naar Azure AD maximaliseert de productiviteit van de gebruiker via eenmalige aanmelding (SSO) in uw cloud en on-premises resources. U kunt beveiligde toegang tot uw cloud en on-premises resources met [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) op hetzelfde moment.

In deze zelfstudie leert u hoe u hybride Azure AD join voor apparaten voor Active Directory-domein computers in een federatieve omgeving configureren met behulp van Active Directory Federation Services (AD FS).

> [!NOTE]
> Als uw federatieve omgeving gebruikmaakt van een id-provider dan AD FS, moet u ervoor zorgen dat de WS-Trust-protocol biedt ondersteuning voor uw id-provider. WS-Trust is vereist om te verifiëren van uw huidige Windows-hybride Azure AD gekoppelde apparaten met Azure AD. Als u Windows downlevel-apparaten die u koppelen aan hybrid Azure AD wilt hebt, moet uw id-provider de claim WIAORMULTIAUTHN ondersteunen. 

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Hybride Azure AD-koppeling configureren
> * Downlevel-apparaten met Windows inschakelen
> * De registratie verifiëren
> * Problemen oplossen

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat you'e bekend zijn met deze artikelen:

- [Wat is een apparaat-id?](overview.md)
- [Hoe u uw implementatie van hybride Azure AD join plannen](hybrid-azuread-join-plan.md)
- [Hoe u gecontroleerde validatie van hybride Azure AD join doet](hybrid-azuread-join-control.md)

Voor het configureren van het scenario in deze zelfstudie hebt u het volgende nodig:

- Windows Server 2012 R2 met AD FS
- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) versie 1.1.819.0 of hoger

Vanaf versie 1.1.819.0, bevat Azure AD Connect een wizard die u gebruiken kunt om te koppelen aan hybrid Azure AD configureren. De wizard maakt het configuratieproces. De gerelateerde wizard:

- Hiermee configureert u de serviceverbindingspunten (SCP's) voor device Registration service
- Maakt een back-up van uw bestaande vertrouwensrelatie van de Relying Party van Azure AD
- Werkt de claimregels in uw Azure AD-vertrouwensrelatie bij

De configuratiestappen in dit artikel zijn gebaseerd op met de Azure AD Connect-wizard. Als u een eerdere versie van Azure AD Connect geïnstalleerd hebt, moet u deze bijwerken naar 1.1.819 of later naar de wizard gebruiken. Als niet is een optie voor u de nieuwste versie van Azure AD Connect installeert, Zie [het handmatig configureren van hybride Azure AD join](hybrid-azuread-join-manual.md).

Hybride Azure AD join vereist dat apparaten toegang hebben tot de volgende Microsoft-resources binnen het netwerk van uw organisatie:  

- `https://enterpriseregistration.windows.net`
- `https://login.microsoftonline.com`
- `https://device.login.microsoftonline.com`
- Uw organisatie Security Token Service (STS) (voor federatieve domeinen)
- `https://autologon.microsoftazuread-sso.com` (Als u gebruikt of wilt gebruiken voor naadloze eenmalige aanmelding)

Beginnen met Windows 10-1803, als de momentopname hybride Azure AD join voor een federatieve omgeving met behulp van AD FS is mislukt, we zijn afhankelijk van de Azure AD Connect voor synchronisatie van het computerobject in Azure AD dat vervolgens is gebruikt voor het voltooien van de device Registration service voor hybride Azure AD-join. Controleer of Azure AD Connect heeft de computerobjecten van de apparaten die u wilt worden toegevoegd aan hybrid Azure AD toegevoegd aan Azure AD gesynchroniseerd. Als de computerobjecten deel uitmaken van specifieke organisatie-eenheden (OU's), moet u ook de organisatie-eenheden om te synchroniseren in Azure AD Connect configureren. Zie voor meer informatie over het synchroniseren van computerobjecten met behulp van Azure AD Connect, [filteren met behulp van Azure AD Connect configureren](../hybrid/how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering).

Als uw organisatie toegang tot internet via een uitgaande proxy vereist, wordt aangeraden [Web Proxy Auto-Discovery (WPAD) implementeren](https://docs.microsoft.com/previous-versions/tn-archive/cc995261(v%3dtechnet.10)) om in te schakelen van Windows 10-computers voor apparaatregistratie bij Azure AD. Als er problemen zijn met het configureren en beheren van WPAD, Zie [oplossen van automatische detectie](https://docs.microsoft.com/previous-versions/tn-archive/cc302643(v=technet.10)). 

Als u geen gebruik van WPAD en proxy-instellingen configureren op uw computer wilt, kunt u doen dus beginnen met Windows 10 1709. Zie voor meer informatie, [instellingen van WinHTTP configureren met behulp van een groepsbeleidsobject (GPO)](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/).

> [!NOTE]
> Als u proxy-instellingen op uw computer configureren via WinHTTP-instellingen, mislukken alle computers die geen verbinding met de geconfigureerde proxy maken verbinding maken met internet.

Als uw organisatie toegang tot internet via een geverifieerde uitgaande proxy vereist, moet u ervoor zorgen dat uw Windows 10-computers aan de uitgaande proxy verifiëren kunnen. Omdat Windows 10-computers kunt u device Registration service met behulp van de context van de computer uitvoert, moet u de uitgaande proxy-verificatie configureren met behulp van de context van de computer. Vraag uw provider van de uitgaande proxy naar de configuratievereisten.

## <a name="configure-hybrid-azure-ad-join"></a>Hybride Azure AD-koppeling configureren

Als u wilt een hybride Azure AD join configureren met behulp van Azure AD Connect, hebt u het volgende nodig:

- De referenties van een globale beheerder voor uw Azure AD-tenant  
- De referenties voor ondernemingsadministrator voor elk van de forests
- De referenties van uw AD FS-beheerder

**Een hybride Azure AD join configureren met behulp van Azure AD Connect**:

1. Start Azure AD Connect en selecteer vervolgens **configureren**.

   ![Welkom](./media/hybrid-azuread-join-federated-domains/11.png)

1. Op de **extra taken** weergeeft, schakelt **Apparaatopties configureren**, en selecteer vervolgens **volgende**.

   ![Extra taken](./media/hybrid-azuread-join-federated-domains/12.png)

1. Op de **overzicht** weergeeft, schakelt **volgende**.

   ![Overzicht](./media/hybrid-azuread-join-federated-domains/13.png)

1. Op de **verbinding maken met Azure AD** pagina, typ de referenties van een globale beheerder voor uw Azure AD-tenant en selecteer vervolgens **volgende**.

   ![Verbinding maken met Azure AD](./media/hybrid-azuread-join-federated-domains/14.png)

1. Op de **Apparaatopties** weergeeft, schakelt **configureren Hybrid Azure AD join**, en selecteer vervolgens **volgende**.

   ![Apparaatopties](./media/hybrid-azuread-join-federated-domains/15.png)

1. Op de **SCP** pagina, de volgende stappen uit en selecteer vervolgens **volgende**:

   ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

   1. Selecteer de forest.
   1. Selecteer de verificatieservice. U moet selecteren **AD FS-server** , tenzij uw organisatie exclusief Windows 10-clients heeft en u synchronisatie van computer/apparaat hebt geconfigureerd of uw organisatie gebruikmaakt van naadloze eenmalige aanmelding.
   1. Selecteer **toevoegen** de referenties voor ondernemingsadministrator invoeren.

1. Op de **besturingssystemen van apparaten** pagina, selecteert u de besturingssystemen die door de apparaten in uw Active Directory-omgeving gebruikt, en selecteer vervolgens **volgende**.

   ![Apparaatbesturingssysteem](./media/hybrid-azuread-join-federated-domains/17.png)

1. Op de **federatieconfiguratie** pagina, voert u de referenties van uw AD FS-beheerder en selecteer vervolgens **volgende**.

   ![Federatieconfiguratie](./media/hybrid-azuread-join-federated-domains/18.png)

1. Op de **klaar om te configureren** weergeeft, schakelt **configureren**.

   ![Klaar om te configureren](./media/hybrid-azuread-join-federated-domains/19.png)

1. Op de **configuratie voltooid** weergeeft, schakelt **afsluiten**.

   ![Configuratie voltooid](./media/hybrid-azuread-join-federated-domains/20.png)

## <a name="enable-windows-downlevel-devices"></a>Downlevel-apparaten met Windows inschakelen

Als sommige van uw domein apparaten downlevel-apparaten met Windows, moet u:

- De lokale intranetinstellingen voor apparaatregistratie configureren
- Microsoft Workplace Join voor Windows downlevel-computers installeren

### <a name="configure-the-local-intranet-settings-for-device-registration"></a>De lokale intranetinstellingen voor apparaatregistratie configureren

Aan hybride Azure AD join van uw downlevel-apparaten met Windows met succes wordt voltooid en om te voorkomen certificaatmeldingen wanneer apparaten worden geverifieerd bij Azure AD, kunt u een beleid pushen naar uw domein apparaten naar de volgende URL's toevoegen aan de zone Lokaal intranet in Internet Explorer:

- `https://device.login.microsoftonline.com`
- Uw organisatie STS (voor federatieve domeinen)
- `https://autologon.microsoftazuread-sso.com` (Voor naadloze eenmalige aanmelding)

U moet ook inschakelen **toestaan van updates op de statusbalk via script** in de lokale intranetzone van de gebruiker.

### <a name="install-microsoft-workplace-join-for-windows-downlevel-computers"></a>Microsoft Workplace Join voor Windows downlevel-computers installeren

Als u wilt registreren downlevel-apparaten met Windows, organisaties moeten installeren [Microsoft Workplace Join voor Windows 10-computers](https://www.microsoft.com/download/details.aspx?id=53554). Microsoft Workplace Join voor Windows 10-computers is beschikbaar in het Microsoft Download Center.

U kunt het pakket implementeren met behulp van een software-distributiesysteem zoals [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). Het pakket biedt ondersteuning voor de standaard installatie op de achtergrond-opties met de `quiet` parameter. De huidige vertakking van Configuration Manager biedt voordelen ten opzichte van eerdere versies, zoals de mogelijkheid voor het bijhouden van voltooide registraties.

Het installatieprogramma maakt een geplande taak op het systeem die wordt uitgevoerd in de context van de gebruiker. De taak wordt geactiveerd wanneer de gebruiker zich aanmeldt bij Windows. Het apparaat met Azure AD toevoegt de taak op de achtergrond met behulp van de gebruikersreferenties na verificatie met Azure AD.

## <a name="verify-the-registration"></a>De registratie verifiëren

Als u wilt controleren of de status van de apparaatregistratie in uw Azure-tenant, kunt u de **[Get-MsolDevice](/powershell/msonline/v1/get-msoldevice)** cmdlet in de [Azure Active Directory PowerShell-module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Wanneer u gebruikt de **Get-MSolDevice** cmdlet om de servicedetails te controleren:

- Een object met de **apparaat-ID** die overeenkomt met de ID op de Windows client moet bestaan.
- Moet de waarde voor **DeviceTrustType** op **Toegevoegd aan domein** zijn ingesteld. Deze instelling is gelijk aan de **toegevoegd aan Hybrid Azure AD** staat onder **apparaten** in de Azure AD-portal.
- Voor apparaten die worden gebruikt voor de voorwaardelijke toegang, de waarde voor **ingeschakeld** moet **waar** en **DeviceTrustLevel** moet **beheerde**.

**Om te controleren of de details van de service**:

1. Open Windows PowerShell als beheerder.
1. Voer `Connect-MsolService` verbinding maken met uw Azure-tenant.  
1. Voer `get-msoldevice -deviceId <deviceId>` in.
1. Verifieer dat **Ingeschakeld** is ingesteld op **Waar**.

## <a name="troubleshoot-your-implementation"></a>Problemen met uw implementatie oplossen

Als u problemen met het voltooien van hybride Azure AD join voor domein van de Windows-apparaten, Zie:

- [Hybride Azure AD join voor de huidige Windows-apparaten oplossen](troubleshoot-hybrid-join-windows-current.md)
- [Hybride Azure AD join voor downlevel-apparaten met Windows oplossen](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [apparaatidentiteiten beheren met behulp van de Azure-portal](device-management-azure-portal.md).

<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
