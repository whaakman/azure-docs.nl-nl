---
title: Azure AD-toepassingsproxy inschakelen | Microsoft Docs
description: Schakel de toepassingsproxy in de klassieke Azure-portal in en installeer de connectors voor de omgekeerde proxy.
services: active-directory
documentationcenter: ''
author: kgremban
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/19/2016
ms.author: kgremban

---
# Toepassingsproxy inschakelen in de Azure-portal
Dit artikel beschrijft de stappen die nodig zijn om Microsoft Azure AD-toepassingsproxy in te schakelen voor uw clouddirectory in Azure AD.

Als u niet bekend bent met de mogelijkheden van toepassingsproxy, gaat u voor meer informatie naar [How to provide secure remote access to on-premises applications](active-directory-application-proxy-get-started.md) (Engelstalig).

## Vereisten voor toepassingsproxy
Voordat u de services voor toepassingsproxy kunt inschakelen en gebruiken, moet u over het volgende beschikken:

* Een [basis- of premiumabonnement op Microsoft Azure AD](active-directory-editions.md) en een Azure AD-directory waarvan u een globale beheerder bent.
* Een server met Windows Server 2012 R2 of Windows 8.1 of hoger, waarop u de connector voor toepassingsproxy kunt installeren. Via de server worden aanvragen naar de services voor toepassingsproxy in de cloud verzonden en er is een HTTP- of HTTPS-verbinding nodig voor de toepassingen die u publiceert.
  
  * Voor eenmalige aanmelding bij uw gepubliceerde toepassingen moet deze computer lid zijn van hetzelfde AD-domein als de toepassingen die u publiceert.
* Als er op het pad een firewall is, zorgt u ervoor dat deze openstaat zodat de connector HTTPS-aanvragen (TCP) kan versturen naar de toepassingsproxy. De connector gebruikt deze poorten samen met subdomeinen die deel uitmaken van de globale domeinen msappproxy.net en servicebus.windows.net. Zorg ervoor dat de hierna genoemde poorten zijn geopend voor **uitgaand** verkeer:
  
      | Poortnummer | Beschrijving |
      | --- | --- |
      | 80 | Uitgaand HTTP-verkeer voor beveiligingsvalidatie inschakelen |
      | 443 | Gebruikersverificatie met Azure AD inschakelen (alleen vereist voor het registratieproces voor de connector) |
      | 10100–10120 | LOB HTTP-responsen terugsturen naar de proxy inschakelen |
      | 9352, 5671 | Communicatie tussen de connector en de Azure-service voor binnenkomende aanvragen inschakelen |
      | 9350 | Optioneel, voor betere prestaties van binnenkomende aanvragen |
      | 8080 | De connectoropstartreeks en automatische updates voor de connector inschakelen |
      | 9090 | Connectorregistratie inschakelen (alleen vereist voor het registratieproces voor de connector) |
      | 9091 | Automatisch verlengen van het vertrouwenscertificaat van de connector inschakelen |
  
    Als met uw firewall verkeer wordt afgedwongen op basis van de herkomst van gebruiker, opent u deze poorten voor verkeer dat afkomstig is van Windows-services die als netwerkservice worden uitgevoerd. Zorg er ook voor dat poort 8080 is ingeschakeld voor NT Authority\System.
* Als uw organisatie proxyservers gebruikt om verbinding te maken met internet, leest u het blogbericht [Working with existing on-premises proxy servers](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) (Engelstalig) voor meer informatie over het configureren van proxyservers.

## Stap 1: toepassingsproxy inschakelen in Azure AD
1. Meld u als beheerder aan in de [klassieke Azure-portal](https://manage.windowsazure.com/).
2. Ga naar Active Directory en selecteer de directory waarin u toepassingsproxy wilt inschakelen.
   
    ![Active Directory - pictogram](./media/active-directory-application-proxy-enable/ad_icon.png)
3. Selecteer **Configureren** op de directorypagina en blader omlaag naar **Toepassingsproxy**.
4. Stel **Services voor toepassingsproxy inschakelen voor deze directory** in op **Ingeschakeld**.
   
    ![Toepassingsproxy inschakelen](./media/active-directory-application-proxy-enable/app_proxy_enable.png)
5. Selecteer **Nu downloaden**. Hiermee gaat u naar de **download voor de connector voor Azure AD-toepassingsproxy**. Lees en accepteer de licentievoorwaarden en klik op **Downloaden** om het Windows Installer-bestand (.exe) van de connector op te slaan.

## Stap 2: de connector installeren en registreren
1. Voer **AADApplicationProxyConnectorInstaller.exe** uit op de server die u hebt voorbereid volgens de vereisten.
2. Volg de instructies in de wizard om de toepassing te installeren.
3. Tijdens de installatie wordt u gevraagd de connector te registreren bij de toepassingsproxy van uw Azure AD-tenant.
   
   * Geef de globale-beheerdersreferentie voor Azure AD op. De referenties van uw globale beheerderstenant kunnen afwijken van uw Microsoft Azure-referenties.
   * Zorg ervoor dat de beheerder die de connector registreert zich in dezelfde map bevindt als waar u de service voor toepassingsproxy hebt ingeschakeld. Als het domein van de tenant bijvoorbeeld contoso.com is, moet de beheerder admin@contoso.com of een andere alias in dat domein zijn.
   * Als **Verbeterde beveiliging van Internet Explorer** is ingesteld op **Aan** op de server waarop u de connector installeert, is het registratiescherm mogelijk geblokkeerd. Volg de instructies in de foutmelding om de toegang toe te staan. Zorg ervoor dat de verbeterde beveiliging van Internet Explorer is uitgeschakeld.
   * Zie [Troubleshoot Application Proxy](active-directory-application-proxy-troubleshoot.md) (Engelstalig) als de registratie van de connector niet lukt.  
4. Wanneer de installatie is voltooid, worden er twee nieuwe services toegevoegd aan uw server:
   
   * Met **Microsoft AAD Application Proxy Connector** wordt de connectiviteit mogelijk gemaakt
   * **Microsoft AAD Application Proxy Connector Updater** is een automatische updateservice waarmee periodiek wordt gecontroleerd op nieuwe versies en updates van de connector.
     
     ![Connectorservices voor toepassingsproxy - schermafbeelding](./media/active-directory-application-proxy-enable/app_proxy_services.png)
5. Klik in het installatievenster op **Voltooien**.

Voor maximale beschikbaarheid dient u minstens nog twee extra connectoren te implementeren. Herhaal de bovenstaande stappen 2 en 3 om meer connectoren te implementeren. Elke connector moet afzonderlijk worden geregistreerd.

Als u de connector wilt verwijderen, verwijdert u de connectorservice en de updaterservice. Start de computer opnieuw op om de service volledig te verwijderen.

## Volgende stappen
U bent nu klaar om [toepassingen te publiceren met toepassingsproxy](active-directory-application-proxy-publish.md).

Als u toepassingen op afzonderlijke netwerken of verschillende locaties hebt, kunt u connectorgroepen gebruiken om de andere connectoren in te delen in logische eenheden. Meer informatie over [Working with Application Proxy connectors](active-directory-application-proxy-connectors.md) (Werken met connectoren voor toepassingsproxy).

<!--HONumber=Sep16_HO3-->


