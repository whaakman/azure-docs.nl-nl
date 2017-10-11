---
title: 'Azure Active Directory Domain Services: Azure Active Directory-toepassingsproxy implementeren | Microsoft Docs'
description: Azure AD-toepassingsproxy gebruiken op de beheerde domeinen Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: c158c67a82e12501386179e19bc75fd852d7e308
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a>Azure AD-toepassingsproxy op een beheerd domein van Azure AD Domain Services implementeren
Toepassingsproxy van Azure Active Directory (AD) kunt u ondersteuning voor externe werknemers door het publiceren van on-premises toepassingen via internet worden geopend. U kunt nu lift-en-shift oudere toepassingen met lokale naar Azure Infrastructure Services met Azure AD Domain Services. Vervolgens kunt u deze toepassingen de Azure AD-toepassingsproxy gebruiken voor veilige externe toegang tot gebruikers in uw organisatie publiceren.

Als u geen ervaring met de Azure AD-toepassingsproxy, meer informatie over deze functie met het volgende artikel: [het verstrekken van veilige externe toegang tot on-premises toepassingen](../active-directory/active-directory-application-proxy-get-started.md).


## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken worden in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldige **Azure-abonnement**.
2. Een **Azure AD-directory** -ofwel gesynchroniseerd met een on-premises adreslijst of een map alleen in de cloud.
3. Een **Azure AD Basic of Premium-licentie** is vereist voor het gebruik van de Azure AD-toepassingsproxy.
4. **Azure AD Domain Services** moet zijn ingeschakeld voor de Azure AD-directory. Als u dit nog niet hebt gedaan, volgt u alle taken die worden beschreven in de [handleiding](active-directory-ds-getting-started.md).

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a>Taak 1: toepassingsproxy van Azure AD inschakelen voor uw Azure AD-directory
Voer de volgende stappen uit zodat de Azure AD-toepassingsproxy voor uw Azure AD-directory.

1. Meld u aan als een beheerder in de [Azure-portal](http://portal.azure.com).

2. Klik op **Azure Active Directory** online zetten van een overzicht van de directory. Klik op **bedrijfstoepassingen**.

    ![Azure AD-directory selecteren](./media/app-proxy/app-proxy-enable-start.png)
3. Klik op **toepassingsproxy**. Als u niet een Azure AD Basic of Azure AD Premium-abonnement hebt, ziet u een optie voor het inschakelen van een evaluatieversie. Wisselknop **toepassingsproxy inschakelen?** naar **inschakelen** en klik op **opslaan**.

    ![Toepassingsproxy inschakelen](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. De connector downloaden, klikt u op de **Connector** knop.

    ![Connector downloaden](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. Op de downloadpagina accepteer de licentievoorwaarden en privacyovereenkomst en klik op de **downloaden** knop.

    ![Download bevestigen](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-to-deploy-the-azure-ad-application-proxy-connector"></a>Taak 2: het domein Windows-servers voor het implementeren van de Azure AD-toepassingsproxy connector richten
U moet lid zijn van het domein Windows Server virtuele machines waarop u de Azure AD-toepassingsproxy-connector kunt installeren. Afhankelijk van de toepassingen die worden gepubliceerd, kunt u meerdere servers waarop de connector is geïnstalleerd inrichten. Deze Implementatieoptie krijgt u groter beschikbaarheid en helpt bij het verwerken van zwaardere belastingen voor verificatie.

Richt de Connectorservers voor hetzelfde virtuele netwerk (of een virtueel netwerk met verbonden/peer is ingesteld), waarin u uw beheerde domein van Azure AD Domain Services hebt ingeschakeld. De servers die als host fungeert voor de toepassingen die u via de toepassingsproxy publiceert moeten op deze manier worden geïnstalleerd op dezelfde virtuele netwerk van Azure.

Voor het inrichten van Connectorservers, volgt u de taken die worden beschreven in het artikel [een virtuele Windows-computer toevoegen aan een beheerd domein](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-3---install-and-register-the-azure-ad-application-proxy-connector"></a>Taak 3: Installeer en registreer de Azure AD Application Proxy Connector
Eerder ingerichte van een virtuele machine van Windows Server en deze toegevoegd aan het beheerde domein. In deze taak kunt u de Azure AD-toepassingsproxy-connector wilt installeren op deze virtuele machine.

1. Kopieer het installatiepakket van de connector met de virtuele machine waarop u de Azure AD Web Application Proxy connector installeert.

2. Voer **AADApplicationProxyConnectorInstaller.exe** op de virtuele machine. De gebruiksrechtovereenkomst accepteren.

    ![Accepteer de voorwaarden voor installatie](./media/app-proxy/app-proxy-install-connector-terms.png)
3. Tijdens de installatie wordt u gevraagd de connector te registreren bij de toepassingsproxy van uw Azure AD-directory.
    * Geef uw **Azure AD-referenties globale beheerder**. De referenties van uw globale beheerderstenant kunnen afwijken van uw Microsoft Azure-referenties.
    * Het administrator-account gebruikt voor het registreren van de connector moet behoren tot dezelfde map waarin u de service-toepassingsproxy hebt ingeschakeld. Bijvoorbeeld, als de tenant-domein contoso.com is, de beheerder moet admin@contoso.com of andere geldige alias in dat domein.
    * Als Verbeterde beveiliging van Internet Explorer is ingeschakeld voor de server waarop u de connector installeert, is het registratiescherm mogelijk geblokkeerd. Volg de instructies in het foutbericht voor toegang. Zorg ervoor dat de verbeterde beveiliging van Internet Explorer is uitgeschakeld.
    * Zie [Troubleshoot Application Proxy](../active-directory/active-directory-application-proxy-troubleshoot.md) (Engelstalig) als de registratie van de connector niet lukt.

    ![Connector is geïnstalleerd](./media/app-proxy/app-proxy-connector-installed.png)
4. Om te controleren of de connector worden werkt de Azure AD Application Proxy Connector probleemoplosser naar behoren uitgevoerd. U ziet een geslaagde rapport na het uitvoeren van de probleemoplosser.

    ![Probleemoplosser geslaagd](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. U ziet de nieuw geïnstalleerde weergegeven op de pagina Application proxy in uw Azure AD-directory-connector.

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> U kunt om connectors te installeren op meerdere servers om bescherming te bieden hoge beschikbaarheid voor het verifiëren van toepassingen die zijn gepubliceerd via de Azure AD-toepassingsproxy. De hierboven beschreven voor het installeren van de connector op andere servers toegevoegd aan uw beheerde domein dezelfde stappen uitvoeren.
>
>

## <a name="next-steps"></a>Volgende stappen
U hebt de Azure AD-toepassingsproxy instellen en deze geïntegreerd met uw Azure AD Domain Services beheerd domein.

* **Uw toepassingen met Azure virtuele machines migreren:** kunt u lift-en-shift uw toepassingen van lokale servers en virtuele machines in Azure die is gekoppeld aan uw beheerde domein. In dat geval kunt u de kosten van de infrastructuur van het uitvoeren van servers lokale weghalen.

* **Toepassingen publiceren met Azure AD-toepassingsproxy:** publiceren van toepassingen die worden uitgevoerd op uw virtuele machines in Azure met behulp van de Azure AD-toepassingsproxy. Zie voor meer informatie [toepassingen publiceren met Azure AD-toepassingsproxy](../active-directory/application-proxy-publish-azure-portal.md)


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a>Opmerking van de implementatie - publiceren IWA (geïntegreerde Windows-verificatie)-toepassingen die gebruikmaken van Azure AD-toepassingsproxy
Eenmalige aanmelding voor uw toepassingen met geïntegreerde Windows-verificatie (IWA) Application Proxy Connectors machtiging verlenen imiteren gebruikers, en verzenden en ontvangen van tokens namens hen inschakelen. Kerberos-beperkte delegatie (KCD) voor de connector voor het verlenen van de vereiste machtigingen voor toegang tot bronnen op het beheerde domein configureren. Gebruik het mechanisme KCD op basis van bronnen op de beheerde domeinen voor een betere beveiliging.


### <a name="enable-resource-based-kerberos-constrained-delegation-for-the-azure-ad-application-proxy-connector"></a>Op basis van bronnen kerberos-beperkte overdracht voor de Azure AD-toepassingsproxy-connector inschakelen
De Azure Application Proxy connector moet worden geconfigureerd voor kerberos-beperkte delegatie (KCD), zodat gebruikers kunnen worden geïmiteerd op het beheerde domein. Op een beheerd domein van Azure AD Domain Services er geen domeinadministratorrechten. Daarom **traditionele account niveau KCD kan niet worden geconfigureerd op een beheerd domein**.

Gebruik KCD op basis van bronnen zoals beschreven in dit [artikel](active-directory-ds-enable-kcd.md).

> [!NOTE]
> U moet lid zijn van de groep 'AAD DC-beheerders' voor het beheer van het beheerde domein met AD PowerShell-cmdlets.
>
>

Gebruik de cmdlet Get-ADComputer PowerShell voor het ophalen van de instellingen voor de computer waarop de Azure AD-toepassingsproxy-connector is geïnstalleerd.
```
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

Daarna gebruikt u de cmdlet Set-ADComputer KCD op basis van een resource voor de resource-server instellen.
```
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

Als u meerdere toepassingsproxy connectors voor uw beheerde domein hebt geïmplementeerd, moet u KCD op basis van bronnen voor elke keer dat deze connector configureert.


## <a name="related-content"></a>Gerelateerde inhoud
* [Azure AD Domain Services - handleiding aan de slag](active-directory-ds-getting-started.md)
* [Kerberos-beperkte overdracht configureren op een beheerd domein](active-directory-ds-enable-kcd.md)
* [Kerberos-beperkte overdracht overzicht](https://technet.microsoft.com/library/jj553400.aspx)
