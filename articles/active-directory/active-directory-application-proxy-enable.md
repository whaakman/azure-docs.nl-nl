---
title: Azure AD-App-Proxy - aan de slag-connector installeren | Microsoft Docs
description: Toepassingsproxy inschakelen in de Azure-portal en installeer de Connectors voor de omgekeerde proxy.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2017
ms.author: billmath
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 09497144ef4047c1191f3c02afccb881d48aaf3c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-application-proxy-and-install-the-connector"></a>Aan de slag met Application Proxy en installeer de connector
Dit artikel beschrijft de stappen die nodig zijn om Microsoft Azure AD-toepassingsproxy in te schakelen voor uw clouddirectory in Azure AD.

Als u niet bent nog op de hoogte van de voordelen van beveiliging en productiviteit toepassingsproxy aan uw organisatie toevoegt meer informatie over [het verstrekken van veilige externe toegang tot on-premises toepassingen](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Vereisten voor toepassingsproxy
Voordat u de services voor toepassingsproxy kunt inschakelen en gebruiken, moet u over het volgende beschikken:

* Een [basis- of premiumabonnement op Microsoft Azure AD](active-directory-editions.md) en een Azure AD-directory waarvan u een globale beheerder bent.
* Een server met Windows Server 2012 R2 of 2016, waarop u de Connector voor toepassingsproxy kunt installeren. De server moet verbinding maken met de services voor toepassingsproxy in de cloud en de on-premises toepassingen die u wilt publiceren.
  * Voor eenmalige aanmelding voor uw gepubliceerde toepassingen met behulp van Kerberos-beperkte overdracht, moet deze machine worden domein in hetzelfde AD-domein als de toepassingen die u wilt publiceren. Zie voor informatie [KCD voor eenmalige aanmelding met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md).

Als uw organisatie gebruikmaakt van proxy-servers verbinding maken met internet, leest u [werken met bestaande lokale proxyservers](application-proxy-working-with-proxy-servers.md) voor meer informatie over het configureren voordat u aan de slag met Application Proxy.

## <a name="open-your-ports"></a>De poorten openen

Als u uw omgeving voorbereiden voor Azure AD-toepassingsproxy, moet u eerst de communicatie met Azure-datacenters inschakelen. Als er op het pad een firewall is, zorgt u ervoor dat deze openstaat zodat de connector HTTPS-aanvragen (TCP) kan versturen naar de toepassingsproxy.

1. De volgende poorten te openen **uitgaande** verkeer:

   | Poortnummer | Hoe deze wordt gebruikt |
   | --- | --- |
   | 80 | Downloaden van certificaatintrekking (CRL's) worden tijdens het valideren van het SSL-certificaat |
   | 443 | Alle uitgaande communicatie met de service-toepassingsproxy |

   Als uw firewall verkeer volgens de herkomst van gebruiker wordt afgedwongen, opent u deze poorten voor verkeer van Windows-services die worden uitgevoerd als een netwerkservice.

   > [!IMPORTANT]
   > De tabel geeft de Poortvereisten voor de connector versies 1.5.132.0 en nieuwer. Als u nog steeds een oudere versie van de connector hebt, moet u ook de volgende poorten 80 en 443: 5671 8080, naast het inschakelen 9090-9091 9350, 9352, 10100 – 10120.
   >
   >Zie voor meer informatie over het bijwerken van uw connectors naar de nieuwste versie [inzicht in Azure AD-toepassingsproxy connectors](application-proxy-understand-connectors.md#automatic-updates).

2. Als uw firewall of proxyserver kunt DNS-whitelisting, kunt u geaccepteerde verbindingen met msappproxy.net en servicebus.windows.net. Als u niet het geval is, moet u toegang tot de [Azure DataCenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653), dat elke week worden bijgewerkt.

3. Microsoft maakt gebruik van vier adressen certificaten controleren. Toegang tot de volgende URL's toestaan als u dit nog niet hebt gedaan voor andere producten:
   * mscrl.Microsoft.com:80
   * CRL.Microsoft.com:80
   * OCSP.msocsp.com:80
   * www.Microsoft.com:80

4. De connector moet toegang tot login.windows.net en login.microsoftonline.com voor het registratieproces.

5. Gebruik de [Azure AD Application Proxy Connector poorten hulpprogramma Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) om te controleren of de connector de service voor toepassingsproxy kan bereiken. Ten minste Zorg ervoor dat de regio VS-midden en de regio die het dichtst bij u alle een groen vinkje. Daarna betekent meer een groen vinkje groter tolerantie.

## <a name="install-and-register-a-connector"></a>Installeren en registreren van een connector
1. Meld u aan als een beheerder in de [Azure-portal](https://portal.azure.com/).
2. De huidige map wordt weergegeven onder uw gebruikersnaam in de rechterbovenhoek. Als u wijzigen, mappen wilt, selecteert u dit pictogram.
3. Ga naar **Azure Active Directory** > **toepassingsproxy**.

   ![Navigeer naar de toepassingsproxy](./media/active-directory-application-proxy-enable/app_proxy_navigate.png)

4. Selecteer **Connector downloaden**.

   ![Connector downloaden](./media/active-directory-application-proxy-enable/download_connector.png)

5. Voer **AADApplicationProxyConnectorInstaller.exe** uit op de server die u hebt voorbereid volgens de vereisten.
6. Volg de instructies in de wizard om de toepassing te installeren. Tijdens de installatie wordt u gevraagd de connector te registreren bij de toepassingsproxy van uw Azure AD-tenant.

   * Geef de globale-beheerdersreferentie voor Azure AD op. De referenties van uw globale beheerderstenant kunnen afwijken van uw Microsoft Azure-referenties.
   * Zorg ervoor dat de beheerder die de connector registreert zich in dezelfde map bevindt als waar u de service voor toepassingsproxy hebt ingeschakeld. Als het domein van de tenant bijvoorbeeld contoso.com is, moet de beheerder admin@contoso.com of een andere alias in dat domein zijn.
   * Als **verbeterde beveiliging van Internet Explorer** is ingesteld op **op** op de server waarop u de connector installeert, ziet u niet de registratiescherm. Om toegang te krijgen, volg de instructies in het foutbericht. Zorg ervoor dat de verbeterde beveiliging van Internet Explorer is uitgeschakeld.

Voor maximale beschikbaarheid dient u minstens nog twee extra connectoren te implementeren. Elke connector moet afzonderlijk worden geregistreerd.

## <a name="test-that-the-connector-installed-correctly"></a>Test die de connector juist is geïnstalleerd

U kunt bevestigen dat een nieuwe connector goed geïnstalleerd door te controleren op het in de Azure portal of op uw server. 

In de Azure portal, moet u zich aanmelden bij uw tenant en navigeer naar **Azure Active Directory** > **toepassingsproxy**. Alle connectors en groepen van de connector weergegeven op deze pagina. Selecteer een connector voor de details te zien of verplaatsen naar een andere connector-groep. 

Controleer de lijst met actieve services voor de connector en de connector updater op uw server. De twee services moeten onmiddellijk worden gestart, maar zo niet, ze inschakelen: 

   * Met **Microsoft AAD Application Proxy Connector** wordt de connectiviteit mogelijk gemaakt

   * **Microsoft AAD Application Proxy Connector Updater** is een automatische updateservice. De updater gecontroleerd op nieuwe versies van de connector en updates van de connector, indien nodig.

   ![Connectorservices voor toepassingsproxy - schermafbeelding](./media/active-directory-application-proxy-enable/app_proxy_services.png)

Zie voor meer informatie over connectors en hoe deze actueel blijven [inzicht in Azure AD-toepassingsproxy connectors](application-proxy-understand-connectors.md).


## <a name="next-steps"></a>Volgende stappen
U bent nu klaar om [toepassingen te publiceren met toepassingsproxy](application-proxy-publish-azure-portal.md).

Als u toepassingen die op afzonderlijke netwerken of verschillende locaties hebt, kunt u de connector groepen gebruiken voor de verschillende connectors delen in logische eenheden. Meer informatie over [Working with Application Proxy connectors](active-directory-application-proxy-connectors-azure-portal.md) (Werken met connectoren voor toepassingsproxy).
