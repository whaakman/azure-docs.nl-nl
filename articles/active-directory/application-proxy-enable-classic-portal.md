---
title: In de klassieke portal Azure AD-toepassingsproxy inschakelen | Microsoft Docs
description: Schakel de toepassingsproxy in de klassieke Azure-portal in en installeer de connectors voor de omgekeerde proxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: ea97fdc8d146ed524a932018b572ceda0982738b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="enable-application-proxy-in-the-classic-portal-and-download-connectors"></a>Toepassingsproxy inschakelen in de klassieke portal en download connectors
Dit artikel beschrijft de stappen die nodig zijn om Microsoft Azure AD-toepassingsproxy in te schakelen voor uw clouddirectory in Azure AD.

Als u niet bekend bent met de mogelijkheden van toepassingsproxy, gaat u voor meer informatie naar [How to provide secure remote access to on-premises applications](active-directory-application-proxy-get-started.md) (Engelstalig).

## <a name="application-proxy-prerequisites"></a>Vereisten voor toepassingsproxy
Voordat u de services voor toepassingsproxy kunt inschakelen en gebruiken, moet u over het volgende beschikken:

* Een [basis- of premiumabonnement op Microsoft Azure AD](active-directory-editions.md) en een Azure AD-directory waarvan u een globale beheerder bent.
* Een server met Windows Server 2012 R2 of 2016, waarop u de Connector voor toepassingsproxy kunt installeren. Via de server worden aanvragen naar de services voor toepassingsproxy in de cloud verzonden en er is een HTTP- of HTTPS-verbinding nodig voor de toepassingen die u publiceert.
  * Voor eenmalige aanmelding bij uw gepubliceerde toepassingen moet deze computer lid zijn van hetzelfde AD-domein als de toepassingen die u publiceert. Zie voor informatie [eenmalige aanmelding met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md)
* Als uw organisatie gebruikmaakt van proxy-servers verbinding maken met internet, leest u [werken met bestaande lokale proxyservers](application-proxy-working-with-proxy-servers.md) voor meer informatie over het configureren ervan.

## <a name="open-your-ports"></a>De poorten openen

Als u uw omgeving voorbereiden voor Azure AD-toepassingsproxy, moet u eerst de communicatie met Azure-datacenters inschakelen. Als er op het pad een firewall is, zorgt u ervoor dat deze openstaat zodat de connector HTTPS-aanvragen (TCP) kan versturen naar de toepassingsproxy.

1. De volgende poorten te openen **uitgaande** verkeer:

   | Poortnummer | Hoe deze wordt gebruikt |
   | --- | --- |
   | 80 | Downloaden van certificaatintrekking (CRL's) worden tijdens het valideren van het SSL-certificaat |
   | 443 | Alle uitgaande communicatie met de service-toepassingsproxy |

   Als met uw firewall verkeer wordt afgedwongen op basis van de herkomst van gebruiker, opent u deze poorten voor verkeer dat afkomstig is van Windows-services die als netwerkservice worden uitgevoerd.

   > [!IMPORTANT]
   > De tabel geeft de Poortvereisten voor de connector versies 1.5.132.0 en nieuwer. Als u nog steeds een oudere versie van de connector hebt, moet u ook de volgende poorten inschakelen: 5671, 8080 9090, 9091, 9350, 9352 en 10100 – 10120.
   >
   >Zie voor meer informatie over het bijwerken van uw connectors naar de nieuwste versie [inzicht in Azure AD-toepassingsproxy connectors](application-proxy-understand-connectors.md#automatic-updates).

2. Als uw firewall of proxyserver kunt DNS-whitelisting, kunt u geaccepteerde verbindingen met msappproxy.net en servicebus.windows.net. Als u niet het geval is, moet u toegang tot de [Azure DataCenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653), dat elke week worden bijgewerkt.

3. Gebruik de [Azure AD Application Proxy Connector poorten hulpprogramma Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) om te controleren of de connector de service voor toepassingsproxy kan bereiken. Ten minste Zorg ervoor dat de regio VS-midden en de regio die het dichtst bij u alle een groen vinkje. Daarna betekent meer een groen vinkje groter tolerantie.

## <a name="enable-application-proxy-in-azure-ad"></a>Toepassingsproxy inschakelen in Azure AD
1. Meld u als beheerder aan in de [klassieke Azure-portal](https://manage.windowsazure.com/).
2. Ga naar Active Directory en selecteer de directory waarin u toepassingsproxy wilt inschakelen.

    ![Active Directory - pictogram](./media/active-directory-application-proxy-enable/ad_icon.png)
3. Selecteer **Configureren** op de directorypagina en blader omlaag naar **Toepassingsproxy**.
4. Stel **Services voor toepassingsproxy inschakelen voor deze directory** in op **Ingeschakeld**.

    ![Toepassingsproxy inschakelen](./media/active-directory-application-proxy-enable/app_proxy_enable.png)
5. Selecteer **Nu downloaden**. De **Azure AD Application Proxy Connector downloaden** wordt geopend. Lees en accepteer de licentievoorwaarden en klik op **Downloaden** om het Windows Installer-bestand (.exe) van de connector op te slaan.

## <a name="install-and-register-the-connector"></a>Installeren en registreren van de Connector
1. Voer **AADApplicationProxyConnectorInstaller.exe** uit op de server die u hebt voorbereid volgens de vereisten.
2. Volg de instructies in de wizard om de toepassing te installeren.
3. Tijdens de installatie wordt u gevraagd de connector te registreren bij de toepassingsproxy van uw Azure AD-tenant.

   * Geef de globale-beheerdersreferentie voor Azure AD op. De referenties van uw globale beheerderstenant kunnen afwijken van uw Microsoft Azure-referenties.
   * Zorg ervoor dat de beheerder die de connector registreert zich in dezelfde map bevindt als waar u de service voor toepassingsproxy hebt ingeschakeld. Als het domein van de tenant bijvoorbeeld contoso.com is, moet de beheerder admin@contoso.com of een andere alias in dat domein zijn.
   * Als **verbeterde beveiliging van Internet Explorer** is ingesteld op **op** op de server het registratiescherm mogelijk geblokkeerd. Volg de instructies in het foutbericht voor toegang. Zorg ervoor dat de verbeterde beveiliging van Internet Explorer is uitgeschakeld.
   * Zie [Troubleshoot Application Proxy](active-directory-application-proxy-troubleshoot.md) (Engelstalig) als de registratie van de connector niet lukt.  
4. Wanneer de installatie is voltooid, worden er twee nieuwe services toegevoegd aan uw server:

   * Met **Microsoft AAD Application Proxy Connector** wordt de connectiviteit mogelijk gemaakt

     * **Microsoft AAD Application Proxy Connector Updater** is een automatische updateservice. Regelmatig gecontroleerd op nieuwe versies van de connector en updates van de connector, indien nodig.

     ![Connectorservices voor toepassingsproxy - schermafbeelding](./media/active-directory-application-proxy-enable/app_proxy_services.png)
5. Klik in het installatievenster op **Voltooien**.

Zie [Informatie over connectors voor de Azure AD-toepassingsproxy](application-proxy-understand-connectors.md) voor meer informatie over connectors.

Voor maximale beschikbaarheid dient u minstens nog twee extra connectoren te implementeren. Herhaal stappen 2 en 3 voor het implementeren van meer connectoren. Elke connector moet afzonderlijk worden geregistreerd.

Als u de connector wilt verwijderen, verwijdert u de connectorservice en de updaterservice. Start de computer opnieuw op om de service volledig te verwijderen.

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar om [toepassingen te publiceren met toepassingsproxy](active-directory-application-proxy-publish.md).

Als u toepassingen op afzonderlijke netwerken of verschillende locaties hebt, kunt u connectorgroepen gebruiken om de andere connectoren in te delen in logische eenheden. Meer informatie over [Working with Application Proxy connectors](active-directory-application-proxy-connectors.md) (Werken met connectoren voor toepassingsproxy).
