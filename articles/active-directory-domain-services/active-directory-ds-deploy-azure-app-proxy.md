---
title: 'Azure Active Directory Domain Services: Azure Active Directory-toepassingsproxy implementeren | Microsoft Docs'
description: Azure AD-toepassingsproxy gebruiken op Azure Active Directory Domain Services beheerde domeinen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: e787d946aa62f0bcb6615d89192e7d9e4d91ffe4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197036"
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a>Azure AD-toepassingsproxy in een Azure AD Domain Services beheerde domein implementeren
Azure Active Directory (AD)-toepassingsproxy helpt u externe werknemers te ondersteunen door het publiceren van on-premises toepassingen kunnen worden benaderd via internet. Met Azure AD Domain Services kunt u nu lift-and-shift oudere toepassingen die on-premises uitgevoerd met Azure Infrastructure Services. Vervolgens kunt u deze toepassingen met behulp van de Azure AD Application Proxy voor veilige externe toegang voor gebruikers in uw organisatie.

Als u geen ervaring met de Azure AD-toepassingsproxy, meer informatie over deze functie met het volgende artikel: [Het bieden van veilige externe toegang tot on-premises toepassingen](../active-directory/manage-apps/application-proxy.md).

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -een gesynchroniseerd met een on-premises directory of een map alleen in de cloud.
3. Een **Azure AD Basic of Premium-licentie** is vereist voor het gebruik van de Azure AD-toepassingsproxy.
4. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [introductiehandleiding](active-directory-ds-getting-started.md).

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a>Taak 1: toepassingsproxy van Azure AD inschakelen voor uw Azure AD-directory
Voer de volgende stappen uit om in te schakelen van de Azure AD-toepassingsproxy voor Azure Active directory.

1. Meld u aan als beheerder in de [Azure-portal](https://portal.azure.com).

2. Klik op **Azure Active Directory** om een overzicht van de directory. Klik op **bedrijfstoepassingen**.

    ![Azure AD-directory selecteren](./media/app-proxy/app-proxy-enable-start.png)
3. Klik op **toepassingsproxy**. Als u een Azure AD Basic of Azure AD Premium-abonnement hebt, ziet u een optie voor het inschakelen van een evaluatieversie. In-/ uitschakelen **toepassingsproxy inschakelen?** naar **inschakelen** en klikt u op **opslaan**.

    ![App-Proxy inschakelen](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. Als u wilt de connector downloaden, klikt u op de **Connector** knop.

    ![Connector downloaden](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. Op de downloadpagina, accepteer de licentievoorwaarden en privacyovereenkomst en klik op de **downloaden** knop.

    ![Controleer of downloaden](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-to-deploy-the-azure-ad-application-proxy-connector"></a>Taak 2: Windows-servers voor het implementeren van de Azure AD Application Proxy connector domein inrichten
U moet een domein Windows Server virtuele machines waarop u de Azure AD Application Proxy-connector kunt installeren. Voor sommige toepassingen, kunt u kiezen voor het inrichten van meerdere servers waarop de connector is geïnstalleerd. Deze Implementatieoptie kunt u grotere beschikbaarheid en helpt verwerken zwaardere belastingen voor verificatie.

Richt de Connectorservers voor hetzelfde virtuele netwerk (of een verbonden/gekoppeld virtueel netwerk), waarin u uw beheerde domein van Azure AD Domain Services hebt ingeschakeld. Op dezelfde manier de servers die als host fungeert voor de toepassingen die u via de toepassingsproxy publiceert moeten worden geïnstalleerd op dezelfde Azure virtual network.

Voor het inrichten van servers met de connector, volgt u de taken die worden beschreven in het artikel [Windows virtuele machine toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-3---install-and-register-the-azure-ad-application-proxy-connector"></a>Taak 3: Installeer en registreer de Azure AD Application Proxy-Connector
Eerder, een Windows Server-machine ingericht en deze toegevoegd aan het beheerde domein. In deze taak kunt u de Azure AD Application Proxy-connector installeren op deze virtuele machine.

1. Kopieer het installatiepakket voor de connector met de virtuele machine waarop u de Azure AD Web Application Proxy-connector installeert.

2. Voer **AADApplicationProxyConnectorInstaller.exe** op de virtuele machine. Accepteer de licentievoorwaarden voor software.

    ![Accepteer de voorwaarden voor het installeren van](./media/app-proxy/app-proxy-install-connector-terms.png)
3. U wordt gevraagd tijdens de installatie van de connector te registreren bij de toepassingsproxy van uw Azure AD-directory.
    * Geef uw **Azure AD-hoofdbeheerdersreferenties**. De referenties van uw globale beheerderstenant kunnen afwijken van uw Microsoft Azure-referenties.
    * Het administrator-account gebruikt om de connector te registreren moet behoren tot dezelfde map waarin u de service voor toepassingsproxy hebt ingeschakeld. Bijvoorbeeld, als het domein van de tenant contoso.com is, moet de beheerder admin@contoso.com of een andere geldige alias in dat domein.
    * Als Verbeterde beveiliging van Internet Explorer is ingeschakeld voor de server waarop u de connector installeert, is het registratiescherm mogelijk geblokkeerd. Als u wilt toegang toestaat, volg de instructies in het foutbericht. Zorg ervoor dat de verbeterde beveiliging van Internet Explorer is uitgeschakeld.
    * Zie [Troubleshoot Application Proxy](../active-directory/manage-apps/application-proxy-troubleshoot.md) (Engelstalig) als de registratie van de connector niet lukt.

    ![Connector is geïnstalleerd](./media/app-proxy/app-proxy-connector-installed.png)
4. Om te controleren of de connector uitvoeren werkt goed, de Azure AD Application Proxy Connector probleemoplosser. U ziet een geslaagde rapport na het uitvoeren van de probleemoplosser voor.

    ![Probleemoplosser voor geslaagd](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. U ziet de nieuw geïnstalleerde weergegeven op de pagina Application proxy in uw Azure AD-directory-connector.

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> Kunt u besluiten om connectors te installeren op meerdere servers om te waarborgen hoge beschikbaarheid voor het verifiëren van toepassingen die zijn gepubliceerd via de Azure AD-toepassingsproxy. De hierboven beschreven voor het installeren van de connector op andere servers toegevoegd aan uw beheerde domein dezelfde stappen uitvoeren.
>
>

## <a name="next-steps"></a>Volgende stappen
U hebt de Azure AD-toepassingsproxy instellen en deze geïntegreerd met uw Azure AD Domain Services beheerde domein.

* **Uw toepassingen op virtuele Azure-machines migreren:** U kunt lift-and-shift uw toepassingen van on-premises servers naar virtuele Azure-machines die zijn gekoppeld aan uw beheerde domein. In dat geval kunt u het beleid van de infrastructuurkosten van servers on-premises uitgevoerd.

* **Met Azure AD-toepassingsproxy-toepassingen publiceren:** Publiceren van toepassingen die worden uitgevoerd op uw Azure virtual machines met behulp van de Azure AD-toepassingsproxy. Zie voor meer informatie, [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](../active-directory/manage-apps/application-proxy-publish-azure-portal.md)


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a>Houd er rekening mee implementatie - publiceren IWA (geïntegreerde Windows-verificatie)-toepassingen met Azure AD-toepassingsproxy
Eenmalige aanmelding voor uw toepassingen met behulp van geïntegreerde Windows-verificatie (IWA) verleent machtiging Toepassingsproxyconnectors imiteren van gebruikers, en verzendt en ontvangt van tokens namens hen inschakelen. Configureer Kerberos-beperkte delegatie (KCD) voor de connector voor het verlenen van de vereiste machtigingen voor toegang tot resources in het beheerde domein. Gebruik het mechanisme KCD op basis van een resource op beheerde domeinen voor een betere beveiliging.


### <a name="enable-resource-based-kerberos-constrained-delegation-for-the-azure-ad-application-proxy-connector"></a>Kerberos-beperkte overdracht op basis van een resource voor de Azure AD Application Proxy-connector inschakelen
De Azure Application Proxy-connector moet worden geconfigureerd voor Kerberos-beperkte delegatie (KCD), zodat gebruikers kunnen worden geïmiteerd in het beheerde domein. Op een Azure AD Domain Services beheerde domein, doet u domeinbeheerdersbevoegdheden hebt. Daarom **traditionele accountniveau KCD kan niet worden geconfigureerd in een beheerd domein**.

Gebruik van KCD op basis van een resource, zoals beschreven in dit [artikel](active-directory-ds-enable-kcd.md).

> [!NOTE]
> U moet een lid van de groep 'AAD DC Administrators' voor het beheer van het beheerde domein met AD PowerShell-cmdlets.
>
>

Gebruik de cmdlet Get-ADComputer PowerShell om op te halen van de instellingen voor de computer waarop de Azure AD Application Proxy-connector is geïnstalleerd.
```powershell
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

Gebruik daarna de cmdlet Set-ADComputer om in te stellen op basis van een resource KCD voor de resource-server.
```powershell
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

Als u meerdere Application Proxy connectors hebt geïmplementeerd op uw beheerde domein, moet u KCD op basis van een resource voor elke keer dat deze connector te configureren.


## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Kerberos-beperkte overdracht configureren op een beheerd domein](active-directory-ds-enable-kcd.md)
* [Kerberos-beperkte overdracht-overzicht](https://technet.microsoft.com/library/jj553400.aspx)
