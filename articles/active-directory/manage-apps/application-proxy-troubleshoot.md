---
title: Application Proxy oplossen | Microsoft Docs
description: Bevat informatie over het oplossen van fouten in Azure AD-toepassingsproxy.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: 2eb59d9b07587fe2cfddb088e0cf82bb6762caa4
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54912677"
---
# <a name="troubleshoot-application-proxy-problems-and-error-messages"></a>Oplossen van problemen met Application Proxy- en foutberichten
Als er fouten optreden bij het openen van een gepubliceerde toepassing of in de publicatie van toepassingen, controleert u de volgende opties om te zien of Microsoft Azure AD Application Proxy correct werkt:

* Open de console Windows-Services en controleer de **Microsoft AAD Application Proxy Connector** -service is ingeschakeld en worden uitgevoerd. U kunt ook om te kijken naar de pagina van de eigenschappen in de Application Proxy-service, zoals wordt weergegeven in de volgende afbeelding:  
  ![Schermafbeelding van de eigenschappen van Microsoft AAD Application Proxy Connector-venster](./media/application-proxy-troubleshoot/connectorproperties.png)
* Open Logboeken en zoek naar gebeurtenissen van de Application Proxy-connector in **logboeken toepassingen en Services** > **Microsoft** > **AadApplicationProxy**  >  **Connector** > **Admin**.
* Indien nodig, meer gedetailleerde logboeken zijn beschikbaar met [inschakelen van de logboeken van de sessie van de Application Proxy connector](application-proxy-connectors.md#under-the-hood).

Zie voor meer informatie over het hulpprogramma Azure AD-probleemoplossing [hulpprogramma voor probleemoplossing voor het valideren van de connector netwerken vereisten](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/03/troubleshooting-tool-to-validate-connector-networking-prerequisites).

## <a name="the-page-is-not-rendered-correctly"></a>De pagina wordt niet correct weergegeven
U mogelijk problemen met uw toepassing rendering of werkt niet goed zonder specifieke foutberichten worden weergegeven. Dit kan gebeuren als u het pad van het artikel hebt gepubliceerd, maar de toepassing vereist inhoud die buiten het opgegeven pad bestaat.

Bijvoorbeeld, als u het pad publiceert https://yourapp/app , maar de toepassing voert aanroepen afbeeldingen https://yourapp/media, ze worden niet weergegeven. Zorg ervoor dat u de toepassing met behulp van het hoogste niveau pad, moet u alle relevante inhoud bevatten. In dit voorbeeld zou zijn http://yourapp/.

Als u het pad op waarnaar wordt verwezen, inhoud bevatten, maar nog steeds moeten gebruikers op een dieper koppeling in het pad wijzigt, Zie het blogbericht [de juiste koppeling instellen voor toepassingsproxy-toepassingen in de Azure AD toegang Configuratiescherm en startprogramma voor Office 365-apptot](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="connector-errors"></a>Connector-fouten

Als de registratie is mislukt tijdens de installatie van de wizard Connector, zijn er twee manieren om de oorzaak van het probleem weer te geven. Een zoeken in het gebeurtenislogboek onder **toepassingen en Services Logs\Microsoft\AadApplicationProxy\Connector\Admin**, of Voer de volgende Windows PowerShell-opdracht:

    Get-EventLog application –source "Microsoft AAD Application Proxy Connector" –EntryType "Error" –Newest 1

Als u de Connector-fout in het gebeurtenislogboek hebt gevonden, kunt u deze tabel van veelvoorkomende fouten gebruiken om het probleem te verhelpen:

| Fout | Aanbevolen stappen |
| ----- | ----------------- |
| Registratie van de connector is mislukt: Zorg ervoor dat u Application Proxy in de Azure Management Portal en of u uw Active Directory-gebruikersnaam en wachtwoord juist ingevoerd ingeschakeld. Fout: 'Een of meer fouten opgetreden.' | Als u het registratie-venster gesloten zonder dat naar Azure AD bent aangemeld, wordt de Connector-wizard opnieuw uitvoeren en registreert u de Connector. <br><br> Als de registratie-venster wordt geopend en klikt u vervolgens onmiddellijk wordt gesloten zonder dat u zich aanmelden, krijgt u waarschijnlijk deze fout. Deze fout treedt op wanneer er een netwerkfout op uw systeem. Zorg ervoor dat het is mogelijk verbinding maken tussen een browser en een openbare website en dat de poorten zijn geopend die zijn opgegeven in [Application Proxy-vereisten](application-proxy-add-on-premises-application.md). |
| Schakel fout wordt weergegeven in het venster van de registratie. Kan niet worden voortgezet | Als deze fout wordt weergegeven en vervolgens het venster wordt gesloten, u hebt ingevoerd de onjuiste gebruikersnaam of het wachtwoord. Probeer het opnieuw. |
| Registratie van de connector is mislukt: Zorg ervoor dat u Application Proxy in de Azure Management Portal en of u uw Active Directory-gebruikersnaam en wachtwoord juist ingevoerd ingeschakeld. Fout: 'AADSTS50059: Er is geen tenant-identificatiegegevens gevonden in een van de aanvraag of impliciet door een opgegeven referenties en zoeken op service principal URI is mislukt. | U probeert aan te melden met een Microsoft-Account en niet van een domein dat deel uitmaakt van de organisatie-ID van de map die u probeert te openen. Zorg ervoor dat de beheerder maakt deel uit van dezelfde domeinnaam als de tenantdomein, bijvoorbeeld, als de Azure AD-domein contoso.com is, moet de beheerder admin@contoso.com. |
| Kan niet ophalen van het huidige beleid worden uitgevoerd voor het uitvoeren van PowerShell-scripts. | Als de installatie van de Connector is mislukt, controleert u om ervoor te zorgen dat de PowerShell-uitvoeringsbeleid niet is uitgeschakeld. <br><br>1. Open de Editor voor Groepsbeleid.<br>2. Ga naar **Computerconfiguratie** > **Beheersjablonen** > **Windows-onderdelen**  >   **Windows PowerShell** en dubbelklikt u op **uitvoering van Script inschakelen**.<br>3. Het uitvoeringsbeleid kan worden ingesteld op **niet geconfigureerd** of **ingeschakeld**. Indien ingesteld op **ingeschakeld**, zorg ervoor dat onder Opties voor het uitvoeringsbeleid is ingesteld op **scripts voor lokale en externe ondertekende scripts toestaan** of **alle scripts toestaan**. |
| Downloaden van de configuratie van de connector is mislukt. | De client-certificaat van de Connector, die wordt gebruikt voor verificatie, is verlopen. Dit kan ook gebeuren als u de Connector is geïnstalleerd achter een proxy. In dit geval de Connector geen toegang tot het Internet en is niet mogelijk om toegang te bieden aan externe gebruikers. Vernieuwen van de vertrouwensrelatie handmatig met de `Register-AppProxyConnector` cmdlet in Windows PowerShell. Als uw Connector zich achter een proxy bevindt, is het nodig zijn om toegang te verlenen voor toegang tot het Internet naar de Connector-accounts 'Netwerkservices' en 'Lokaal systeem'. Dit kan worden bereikt door hen toegang verlenen tot de Proxy of door het instellen van deze naar de proxy overslaan. |
| Registratie van de connector is mislukt: Zorg ervoor dat u bent een beheerder van de toepassing van uw Active Directory om de Connector te registreren. Fout: 'De aanvraag voor functieregistratie is geweigerd.' | De alias die u probeert aan te melden met is niet een beheerder zijn van dit domein. De Connector is altijd geïnstalleerd voor de map die eigenaar is van het domein van de gebruiker. Zorg ervoor dat het beheerdersaccount dat u zich aanmelden wilt met machtigingen voor de beheerder van ten minste van de toepassing bij Azure AD-tenant heeft. |

## <a name="kerberos-errors"></a>Kerberos-fouten

Deze tabel bevat informatie over de meest voorkomende fouten die afkomstig van de Kerberos-installatie en configuratie zijn, en de tabel bevat suggesties voor het omzetten van.

| Fout | Aanbevolen stappen |
| ----- | ----------------- |
| Kan niet ophalen van het huidige beleid worden uitgevoerd voor het uitvoeren van PowerShell-scripts. | Als de installatie van de Connector is mislukt, controleert u om ervoor te zorgen dat de PowerShell-uitvoeringsbeleid niet is uitgeschakeld.<br><br>1. Open de Editor voor Groepsbeleid.<br>2. Ga naar **Computerconfiguratie** > **Beheersjablonen** > **Windows-onderdelen**  >   **Windows PowerShell** en dubbelklikt u op **uitvoering van Script inschakelen**.<br>3. Het uitvoeringsbeleid kan worden ingesteld op **niet geconfigureerd** of **ingeschakeld**. Indien ingesteld op **ingeschakeld**, zorg ervoor dat onder Opties voor het uitvoeringsbeleid is ingesteld op **scripts voor lokale en externe ondertekende scripts toestaan** of **alle scripts toestaan**. |
| 12008 - azure AD overschrijdt het maximum aantal toegestane Kerberos-verificatiepogingen om de back-endserver. | Deze fout kan duiden op een onjuiste configuratie tussen Azure AD en de back-end-toepassingsserver of een probleem in de configuratie van datum en tijd op beide computers. De back-endserver afgewezen het Kerberos-ticket gemaakt door Azure AD. Controleer of dat door Azure AD en de back-end-toepassingsserver correct zijn geconfigureerd. Zorg ervoor dat de configuratie datum en tijd op de Azure AD en de back-end-toepassingsserver zijn gesynchroniseerd. |
| 13016 - azure AD kan een Kerberos-ticket namens de gebruiker niet ophalen omdat er geen UPN in het randtoken of in de cookie toegang. | Er is een probleem met de STS-configuratie. Corrigeer de configuratie van de UPN-claim in de STS. |
| 13019 - azure AD kan een Kerberos-ticket namens de gebruiker niet ophalen vanwege de volgende algemene API-fout. | Deze gebeurtenis kan duiden op een onjuiste configuratie tussen Azure AD en de domeincontrollerserver of een probleem in de configuratie van datum en tijd op beide computers. De domeincontroller afgewezen het Kerberos-ticket gemaakt door Azure AD. Controleer of dat door Azure AD en de back-end-toepassingsserver zijn correct geconfigureerd, met name de SPN-configuratie. Zorg ervoor dat de Azure AD is gekoppeld aan hetzelfde domein als de domeincontroller om ervoor te zorgen dat de domeincontroller wordt tot stand vertrouwensrelatie met Azure AD gebracht-domein. Zorg ervoor dat de configuratie datum en tijd op de Azure AD en de domeincontroller zijn gesynchroniseerd. |
| 13020 - azure AD kan een Kerberos-ticket namens de gebruiker niet ophalen omdat de back-endserver SPN niet is gedefinieerd. | Deze gebeurtenis kan duiden op een onjuiste configuratie tussen Azure AD en de domeincontrollerserver of een probleem in de configuratie van datum en tijd op beide computers. De domeincontroller afgewezen het Kerberos-ticket gemaakt door Azure AD. Controleer of dat door Azure AD en de back-end-toepassingsserver zijn correct geconfigureerd, met name de SPN-configuratie. Zorg ervoor dat de Azure AD is gekoppeld aan hetzelfde domein als de domeincontroller om ervoor te zorgen dat de domeincontroller wordt tot stand vertrouwensrelatie met Azure AD gebracht-domein. Zorg ervoor dat de configuratie datum en tijd op de Azure AD en de domeincontroller zijn gesynchroniseerd. |
| 13022 - azure AD kan de gebruiker kan niet verifiëren omdat de back-endserver op Kerberos-verificatie geprobeerd met een 401 HTTP-fout reageert. | Deze gebeurtenis kan duiden op een onjuiste configuratie tussen Azure AD en de back-end-toepassingsserver of een probleem in de configuratie van datum en tijd op beide computers. De back-endserver afgewezen het Kerberos-ticket gemaakt door Azure AD. Controleer of dat door Azure AD en de back-end-toepassingsserver correct zijn geconfigureerd. Zorg ervoor dat de configuratie datum en tijd op de Azure AD en de back-end-toepassingsserver zijn gesynchroniseerd. Zie voor meer informatie, [oplossen Kerberos-beperkte overdracht configuraties voor toepassingsproxy](application-proxy-back-end-kerberos-constrained-delegation-how-to.md).  |

## <a name="end-user-errors"></a>Eindgebruikers fouten

Deze lijst bevat informatie over fouten die uw eindgebruikers tegenkomen kunt wanneer ze proberen te krijgen tot de app en mislukken. 

| Fout | Aanbevolen stappen |
| ----- | ----------------- |
| De website kan de pagina niet weergeven. | De gebruiker kan deze foutmelding krijgt bij het openen van de app die u als de toepassing een IWA is gepubliceerd. De opgegeven SPN-naam voor deze toepassing kan onjuist zijn. Zorg ervoor dat de SPN-naam geconfigureerd voor deze toepassing juist is voor geïntegreerde Windows-apps. |
| De website kan de pagina niet weergeven. | De gebruiker kan deze foutmelding krijgt bij het openen van de app die u als de toepassing een OWA-toepassing is hebt gepubliceerd. Dit wordt mogelijk veroorzaakt door een van de volgende:<br><li>De opgegeven SPN-naam voor deze toepassing is onjuist. Zorg ervoor dat de SPN-naam geconfigureerd voor deze toepassing juist is.</li><li>De gebruiker die toegang probeert te krijgen van de toepassing gebruikmaakt van een Microsoft-account in plaats van het juiste bedrijfsaccount aan te melden bij, of de gebruiker een gastgebruiker is. Zorg ervoor dat de gebruiker zich aanmeldt met een bedrijfsaccount die overeenkomt met het domein van de gepubliceerde toepassing. Gebruikers van Microsoft-Account en de Gast geen toegang tot IWA-toepassingen.</li><li>De gebruiker die toegang probeert te krijgen van de toepassing is niet correct gedefinieerd voor deze toepassing aan de on-premises. Zorg ervoor dat deze gebruiker de juiste machtigingen heeft, zoals gedefinieerd voor deze back van endtoepassing op de on-premises machine. |
| Deze zakelijke app kan niet worden geopend. U bent niet gemachtigd voor toegang tot deze toepassing. Autorisatie is mislukt. Zorg ervoor dat de gebruiker met toegang toewijzen aan deze toepassing. | Uw gebruikers foutmelding deze wanneer u probeert toegang tot de app die u hebt gepubliceerd als ze aan te melden bij Microsoft-accounts gebruiken in plaats van een bedrijfsaccount. Gastgebruikers kunnen deze fout ook krijgen. Gebruikers van Microsoft-Account en gasten geen toegang tot IWA-toepassingen. Zorg ervoor dat de gebruiker zich aanmeldt met een bedrijfsaccount die overeenkomt met het domein van de gepubliceerde toepassing.<br><br>U hebt mogelijk de gebruiker voor deze toepassing niet toegewezen. Ga naar de **toepassing** tabblad, en klikt u onder **gebruikers en groepen**, deze gebruiker of groep toewijzen aan deze toepassing. |
| Deze zakelijke app momenteel niet toegankelijk. Probeer het later opnieuw... Er is een time-out opgetreden voor de connector. | Uw gebruikers foutmelding deze wanneer u probeert toegang tot de app die u als ze zijn niet correct gedefinieerd voor deze toepassing aan de on-premises hebt gepubliceerd. Zorg ervoor dat uw gebruikers de juiste machtigingen hebben, zoals gedefinieerd voor deze back van endtoepassing op de on-premises machine. |
| Deze zakelijke app kan niet worden geopend. U bent niet gemachtigd voor toegang tot deze toepassing. Autorisatie is mislukt. Zorg ervoor dat de gebruiker een licentie voor Azure Active Directory Premium of Basic heeft. | Uw gebruikers foutmelding deze wanneer u probeert toegang tot de app die u hebt gepubliceerd als ze niet zijn expliciet met een Premium/Basic-licentie door de beheerder van de abonnee toegewezen. Ga naar de Active Directory van de abonnee **licenties** tabblad en zorg ervoor dat deze gebruiker of groep een Premium of Basic-licentie is toegewezen. |

## <a name="my-error-wasnt-listed-here"></a>Hieronder vindt u de fout is niet

Als er een fout of een probleem met Azure AD-toepassingsproxy die niet wordt vermeld in deze handleiding voor probleemoplossing, graag willen we horen erover. Stuur een e-mail naar onze [feedback team](mailto:aadapfeedback@microsoft.com) met de details van de fout is opgetreden.

## <a name="see-also"></a>Zie ook
* [Schakel de toepassingsproxy voor Azure Active Directory](application-proxy-add-on-premises-application.md)
* [Publiceren van toepassingen met toepassingsproxy](application-proxy-add-on-premises-application.md)
* [Eenmalige aanmelding inschakelen](application-proxy-configure-single-sign-on-with-kcd.md)
* [Voorwaardelijke toegang inschakelen](application-proxy-integrate-with-sharepoint-server.md)


<!--Image references-->
[1]: ./media/application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
