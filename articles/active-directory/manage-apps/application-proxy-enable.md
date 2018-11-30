---
title: Azure AD App Proxy - aan de slag-connector installeren | Microsoft Docs
description: Toepassingsproxy inschakelen in Azure portal en installeer de Connectors voor de omgekeerde proxy.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: barbkess
ms.reviewer: japere
ms.custom: it-pro
ms.openlocfilehash: 683b5b24fe8e7da086e000ff38411d3eb1c2f781
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495751"
---
# <a name="get-started-with-application-proxy-and-install-the-connector"></a>Aan de slag met Application Proxy en de connector te installeren
In dit artikel leidt u door de stappen om in te schakelen Application Proxy in Azure Active Directory (Azure AD).

Als u zijn nog niet op de hoogte van de voordelen van beveiliging en productiviteit Application Proxy voor uw organisatie biedt, meer informatie over [het bieden van veilige externe toegang tot on-premises toepassingen](application-proxy.md).

## <a name="prerequisites"></a>Vereisten
Als u wilt inschakelen in Application Proxy, hebt u het volgende nodig:

* Een [Microsoft Azure AD basic of premium-abonnement](https://azure.microsoft.com/pricing/details/active-directory). 
* Een toepassing administrator-account.

### <a name="windows-server"></a>Windows server
U moet een server met Windows Server 2012 R2 of hoger waarop kunt u de Application Proxy-connector installeren. De server moet verbinding maken met de services voor toepassingsproxy in Azure en de on-premises toepassingen die u publiceert.

De windows-server moet TLS 1.2 is ingeschakeld voordat u de Application Proxy-connector installeert. Bestaande connectors met een versie lager dan 1.5.612.0 blijven werken in eerdere versies van TLS tot nader order van kracht. TLS 1.2 inschakelen:

1. De volgende registersleutels instellen:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

2. De server opnieuw opstarten

Voor single sign-on bij toepassingen die gebruikmaken van Kerberos waar delegatie (KCD), moeten de Windows-server en de toepassingen die u publiceert zich in hetzelfde Active Directory-domein. Zie voor meer informatie, [KCD voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md).
  
### <a name="proxy-servers"></a>Proxyservers

Als uw organisatie proxyservers gebruikt om verbinding met internet te maken, moet u ze configureren voor de toepassingsproxy.  Zie voor meer informatie, [werken met bestaande on-premises proxy-servers](application-proxy-configure-connectors-with-proxy-servers.md). 



## <a name="open-your-ports"></a>De poorten openen

Als u wilt uw omgeving voorbereiden voor Azure AD-toepassingsproxy, moet u eerst inschakelen van communicatie met Azure-datacenters. Als er op het pad een firewall is, zorgt u ervoor dat deze openstaat zodat de connector HTTPS-aanvragen (TCP) kan versturen naar de toepassingsproxy.

1. De volgende poorten te openen **uitgaande** verkeer:

   | Poortnummer | Hoe deze wordt gebruikt |
   | --- | --- |
   | 80 | Downloaden van certificaatintrekking (CRL's) worden tijdens het valideren van het SSL-certificaat |
   | 443 | Alle uitgaande communicatie met de service voor toepassingsproxy |

   Als uw firewall verkeer op basis van die afkomstig zijn van gebruikers wordt afgedwongen, opent u deze poorten voor verkeer via Windows-services die worden uitgevoerd als een netwerkservice.

   > [!IMPORTANT]
   > De tabel bevat de Poortvereisten voor connector versies 1.5.132.0 en nieuwer. Als u nog steeds een oudere versie van de connector hebt, moet u ook de volgende poorten 80 en 443: 5671 8080, naast het inschakelen 9090-9091, 9350, 9352, 10100 – 10120.
   >
   >Zie voor meer informatie over het bijwerken van uw connectors naar de nieuwste versie [over Azure AD Application Proxy connectors](application-proxy-connectors.md#automatic-updates).

2. Als uw firewall of proxyserver kunt DNS-opname in de whitelist, kunt u verbindingen van de goedgekeurde lijst met msappproxy.net en servicebus.windows.net. Als u niet het geval is, moet u toegang tot de [Azure DataCenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653), die elke week worden bijgewerkt.

3. Microsoft maakt gebruik van vier adressen om te controleren of certificaten. Toegang tot de volgende URL's toestaan als u dit nog niet hebt gedaan voor andere producten:
   * mscrl.microsoft.com:80
   * crl.microsoft.com:80
   * ocsp.msocsp.com:80
   * www.microsoft.com:80

4. De connector moet toegang hebben tot login.windows.net en login.microsoftonline.com voor het registratieproces.


## <a name="install-and-register-a-connector"></a>Installeren en registreren van een connector
1. Meld u aan als beheerder aan in de [Azure-portal](https://portal.azure.com/).
2. De huidige map wordt weergegeven onder uw gebruikersnaam in de rechterbovenhoek. Als u wijzigen van mappen wilt, selecteert u dit pictogram.
3. Ga naar **Azure Active Directory** > **toepassingsproxy**.

   ![Ga naar toepassingsproxy](./media/application-proxy-enable/app_proxy_navigate.png)

4. Selecteer **Connector downloaden**.

   ![Connector downloaden](./media/application-proxy-enable/download_connector.png)

5. Voer **AADApplicationProxyConnectorInstaller.exe** uit op de server die u hebt voorbereid volgens de vereisten.
6. Volg de instructies in de wizard om de toepassing te installeren. U wordt gevraagd tijdens de installatie van de connector te registreren bij de toepassingsproxy van uw Azure AD-tenant.

   * Geef de globale-beheerdersreferentie voor Azure AD op. De referenties van uw globale beheerderstenant kunnen afwijken van uw Microsoft Azure-referenties.
   * Zorg ervoor dat de beheerder die de connector registreert zich in dezelfde map bevindt als waar u de service voor toepassingsproxy hebt ingeschakeld. Als het domein van de tenant bijvoorbeeld contoso.com is, moet de beheerder admin@contoso.com of een andere alias in dat domein zijn.
   * Als **verbeterde beveiliging van Internet Explorer** is ingesteld op **op** op de server waarop u de connector installeert, ziet u niet de registratiescherm. Om toegang te krijgen, volg de instructies in het foutbericht. Zorg ervoor dat de verbeterde beveiliging van Internet Explorer is uitgeschakeld.

Voor maximale beschikbaarheid dient u minstens nog twee extra connectoren te implementeren. Elke connector moet afzonderlijk worden geregistreerd.

## <a name="test-that-the-connector-installed-correctly"></a>Test of de connector juist is geïnstalleerd

U kunt controleren of een nieuwe connector correct geïnstalleerd door te zoeken naar deze in de Azure portal of op uw server. 

In de Azure-portal, moet u zich aanmelden bij uw tenant en Ga naar **Azure Active Directory** > **Application Proxy**. Alle connectors en connectorgroepen weergegeven op deze pagina. Selecteer een connector voor de details ervan te bekijken of verplaats deze naar een andere connectorgroep. 

Controleer de lijst met actieve services voor de connector en de connector updater op uw server. De twee services moeten onmiddellijk worden gestart, maar als dit niet het geval is, schakelt deze op: 

   * Met **Microsoft AAD Application Proxy Connector** wordt de connectiviteit mogelijk gemaakt

   * **Microsoft AAD Application Proxy Connector Updater** is een automatische updateservice. De updater wordt gecontroleerd op nieuwe versies van de connector en de connector wordt bijgewerkt als dit nodig is.

   ![Connectorservices voor toepassingsproxy - schermafbeelding](./media/application-proxy-enable/app_proxy_services.png)

Zie voor meer informatie over connectors en hoe ze up-to-date blijven [over Azure AD Application Proxy connectors](application-proxy-connectors.md).


## <a name="next-steps"></a>Volgende stappen
U bent nu klaar om [toepassingen te publiceren met toepassingsproxy](application-proxy-publish-azure-portal.md).

Als u toepassingen die op afzonderlijke netwerken of verschillende locaties hebt, kunt u connectorgroepen gebruiken om te organiseren van de andere connectoren in logische eenheden. Meer informatie over [Working with Application Proxy connectors](application-proxy-connector-groups.md) (Werken met connectoren voor toepassingsproxy).
