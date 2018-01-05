---
title: Problemen met toepassingsproxy | Microsoft Docs
description: Bevat informatie over het oplossen van fouten in Azure AD-toepassingsproxy.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 970caafb-40b8-483c-bb46-c8b032a4fb74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: d9342194c41c551123a6eb307da98f7a9248a265
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="troubleshoot-application-proxy-problems-and-error-messages"></a>Oplossen van problemen met toepassingsproxy en foutberichten
Als er fouten optreden bij het openen van een gepubliceerde toepassing of in de publicatie van toepassingen, controleert u de volgende opties om te zien of Microsoft Azure AD-toepassingsproxy correct werkt:

* Open de console Windows-Services en controleer of de **Microsoft AAD Application Proxy Connector** -service is ingeschakeld en worden uitgevoerd. U kunt ook om te kijken naar de eigenschappenpagina van Application Proxy service zoals weergegeven in de volgende afbeelding:  
  ![Eigenschappen van Microsoft AAD Application Proxy Connector venster-schermafbeelding](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)
* Open Logboeken en zoekt u naar Application Proxy connector-gebeurtenissen in **logboeken toepassingen en Services** > **Microsoft** > **AadApplicationProxy** > **Connector** > **Admin**.
* Indien nodig, meer gedetailleerde logboeken kunnen worden [inschakelen van de logboeken van de Application Proxy connector sessie](application-proxy-understand-connectors.md#under-the-hood).

Zie voor meer informatie over het hulpprogramma Azure AD-probleemoplossing [hulpprogramma probleemoplossing netwerken vereisten connector valideren](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/03/troubleshooting-tool-to-validate-connector-networking-prerequisites).

## <a name="the-page-is-not-rendered-correctly"></a>De pagina is niet correct weergegeven
Mogelijk hebt u problemen met uw toepassing rendering of werkt niet goed zonder specifieke foutberichten worden weergegeven. Dit kan gebeuren als u het pad van het artikel gepubliceerd, maar de toepassing moet inhoud buiten dat pad.

Bijvoorbeeld, als u het pad https://yourapp/app publiceren, maar de toepassing afbeeldingen in https://yourapp/media roept, won't ze worden weergegeven. Zorg ervoor dat u de toepassingen met het hoogste niveau pad publiceren, moet u alle relevante inhoud bevatten. In dit voorbeeld zou worden http://yourapp/.

Als u het pad voor het opnemen van inhoud waarnaar wordt verwezen, maar nog steeds moeten gebruikers op een dieper koppeling in het pad wijzigt, Zie het blogbericht [-instelling van de juiste koppeling voor toepassingen in de Azure AD-toepassingsproxy toegang Configuratiescherm en Office 365 app linksboven tot](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="connector-errors"></a>Connector-fouten

Gebruik de [Azure AD Application Proxy Connector poorten hulpprogramma Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) om te controleren of de connector de service voor toepassingsproxy kan bereiken. Ten minste Zorg ervoor dat de regio VS-midden en de regio die het dichtst bij u alle een groen vinkje. Daarna betekent meer een groen vinkje groter tolerantie. 

Als de registratie is mislukt tijdens de installatie van de wizard Connector, zijn er twee manieren om de oorzaak van het probleem weer te geven. Een zoeken in het gebeurtenislogboek onder **toepassingen en Services Logs\Microsoft\AadApplicationProxy\Connector\Admin**, of Voer de volgende Windows PowerShell-opdracht:

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

Zodra u de Connector-fout in het gebeurtenislogboek vindt, gebruikt u deze tabel van veelvoorkomende fouten het probleem op te lossen:

| Fout | Aanbevolen stappen |
| ----- | ----------------- |
| Registratie van de connector is mislukt: Zorg ervoor dat u toepassingsproxy in de Azure-beheerportal en dat u uw Active Directory-gebruikersnaam en wachtwoord correct ingevoerd ingeschakeld. Fout: 'een of meer fouten opgetreden." | Als u het registratie-venster gesloten zonder het aanmelden bij Azure AD, de Connector-wizard opnieuw uitvoeren en registreert u de Connector. <br><br> Als het registratie-venster wordt geopend en wordt onmiddellijk afgesloten zonder dat u aan te melden, wordt u waarschijnlijk deze fout. Deze fout treedt op wanneer er een netwerkfout op uw systeem. Zorg ervoor dat het mogelijk om vanuit een browser met een openbare website is en dat de poorten zijn geopend zoals opgegeven in [toepassingsproxy vereisten](active-directory-application-proxy-enable.md). |
| Schakel fout wordt weergegeven in het venster van de registratie. Kan niet worden voortgezet | Als u deze fout te zien en vervolgens het venster sluit, u de onjuiste gebruikersnaam of het wachtwoord ingevoerd. Probeer het opnieuw. |
| Registratie van de connector is mislukt: Zorg ervoor dat u toepassingsproxy in de Azure-beheerportal en dat u uw Active Directory-gebruikersnaam en wachtwoord correct ingevoerd ingeschakeld. Fout: ' AADSTS50059: Er is geen tenant-identificatiegegevens gevonden in een van de aanvraag of impliciet door een opgegeven referenties en zoeken door de service principal URI is mislukt. | U probeert aan te melden met een Microsoft-Account en niet van een domein dat deel uitmaakt van de organisatie-ID van de map die u probeert te openen. Zorg ervoor dat de beheerder maakt deel uit van dezelfde domeinnaam als het domein van de tenant, bijvoorbeeld als de Azure AD-domein contoso.com is, de beheerder moet admin@contoso.com. |
| Kan niet ophalen van het huidige uitvoeringsbeleid voor het uitvoeren van PowerShell-scripts. | Als de installatie van de Connector is mislukt, controleert u om ervoor te zorgen dat er geen PowerShell-uitvoeringsbeleid is uitgeschakeld. <br><br>1. Open de Editor voor Groepsbeleid.<br>2. Ga naar **Computerconfiguratie** > **Beheersjablonen** > **Windows-onderdelen** > **Windows PowerShell** en dubbelklikt u op **uitvoering van Script inschakelen**.<br>3. Het uitvoeringsbeleid kan worden ingesteld op **niet geconfigureerd** of **ingeschakeld**. Indien ingesteld op **ingeschakeld**, zorg ervoor dat onder Opties, het uitvoeringsbeleid is ingesteld op **scripts lokale en externe ondertekende scripts toestaan** of **alle scripts toestaan**. |
| Downloaden van de configuratie van de connector is mislukt. | De Connector clientcertificaat dat wordt gebruikt voor verificatie, is verlopen. Dit kan ook gebeuren als u de Connector is geïnstalleerd achter een proxy hebt. In dit geval de Connector geen toegang tot het Internet en niet kunnen worden om te bieden aan externe gebruikers. Vernieuwen van de vertrouwensrelatie handmatig met behulp van de `Register-AppProxyConnector` cmdlet in Windows PowerShell. Als de Connector zich achter een proxy, is het nodig zijn voor het verlenen van toegang tot Internet op de Connector-accounts 'network services' en 'Lokaal systeem'. Dit kan worden bereikt door ze naar de Proxy toegang verlenen of geconfigureerd voor de proxyserver wordt overgeslagen. |
| Registratie van de connector is mislukt: Zorg ervoor dat u een globale beheerder van uw Active Directory de Connector te registreren. Fout: 'de aanvraag voor functieregistratie is geweigerd.' | De alias die u probeert aan te melden met een beheerder in dit domein niet. De Connector is altijd geïnstalleerd voor de map die eigenaar is van het domein van de gebruiker. Zorg ervoor dat het beheerdersaccount dat u probeert aan te melden met globale machtigingen voor de Azure AD-tenant heeft. |

## <a name="kerberos-errors"></a>Kerberos-fouten

Deze tabel bevat informatie over de meest voorkomende fouten die afkomstig van het Kerberos-installatie en configuratie zijn en bevat suggesties voor naamomzetting.

| Fout | Aanbevolen stappen |
| ----- | ----------------- |
| Kan niet ophalen van het huidige uitvoeringsbeleid voor het uitvoeren van PowerShell-scripts. | Als de installatie van de Connector is mislukt, controleert u om ervoor te zorgen dat er geen PowerShell-uitvoeringsbeleid is uitgeschakeld.<br><br>1. Open de Editor voor Groepsbeleid.<br>2. Ga naar **Computerconfiguratie** > **Beheersjablonen** > **Windows-onderdelen** > **Windows PowerShell** en dubbelklikt u op **uitvoering van Script inschakelen**.<br>3. Het uitvoeringsbeleid kan worden ingesteld op **niet geconfigureerd** of **ingeschakeld**. Indien ingesteld op **ingeschakeld**, zorg ervoor dat onder Opties, het uitvoeringsbeleid is ingesteld op **scripts lokale en externe ondertekende scripts toestaan** of **alle scripts toestaan**. |
| 12008 - azure AD overschrijdt het maximum aantal toegestane Kerberos verificatiepogingen op de back-endserver. | Deze fout duidt mogelijk onjuiste configuratie tussen Azure AD en de back-endserver toepassing of een probleem in de configuratie van de datum en tijd op beide computers. De back-endserver afgewezen het Kerberos-ticket gemaakt door Azure AD. Controleer of deze Azure AD en de back-end-toepassingsserver correct zijn geconfigureerd. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de back-end-toepassingsserver worden gesynchroniseerd. |
| 13016 - azure AD kan een Kerberos-ticket namens de gebruiker niet ophalen omdat er geen UPN in het randtoken of in de cookie toegang. | Er is een probleem met de configuratie van de STS. Los de configuratie van de claim UPN in de STS. |
| 13019 - azure AD kan een Kerberos-ticket namens de gebruiker niet ophalen vanwege de volgende algemene API-fout. | Deze gebeurtenis mogelijk een onjuiste configuratie tussen Azure AD en de domeincontrollerserver of een probleem in de configuratie van de datum en tijd op beide computers. De domeincontroller afgewezen het Kerberos-ticket gemaakt door Azure AD. Controleer of dat Azure AD en de back-end-toepassingsserver zijn met name de SPN-configuratie correct is geconfigureerd. Zorg ervoor dat de Azure AD is gekoppeld aan hetzelfde domein als de domeincontroller om ervoor te zorgen dat de domeincontroller stelt u de vertrouwensrelatie met Azure AD-domein. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de domeincontroller worden gesynchroniseerd. |
| 13020 - azure AD kan een Kerberos-ticket namens de gebruiker niet ophalen omdat de back-endserver SPN niet is gedefinieerd. | Deze gebeurtenis mogelijk een onjuiste configuratie tussen Azure AD en de domeincontrollerserver of een probleem in de configuratie van de datum en tijd op beide computers. De domeincontroller afgewezen het Kerberos-ticket gemaakt door Azure AD. Controleer of dat Azure AD en de back-end-toepassingsserver zijn met name de SPN-configuratie correct is geconfigureerd. Zorg ervoor dat de Azure AD is gekoppeld aan hetzelfde domein als de domeincontroller om ervoor te zorgen dat de domeincontroller stelt u de vertrouwensrelatie met Azure AD-domein. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de domeincontroller worden gesynchroniseerd. |
| 13022 - azure AD kan de gebruiker kan niet verifiëren omdat de back-endserver op pogingen tot Kerberos-verificatie met een 401 HTTP-fout reageert. | Deze gebeurtenis mogelijk een onjuiste configuratie tussen Azure AD en de back-endserver toepassing of een probleem in de configuratie van de datum en tijd op beide computers. De back-endserver afgewezen het Kerberos-ticket gemaakt door Azure AD. Controleer of deze Azure AD en de back-end-toepassingsserver correct zijn geconfigureerd. Zorg ervoor dat de configuratie van de datum en tijd op de Azure AD en de back-end-toepassingsserver worden gesynchroniseerd. |

## <a name="end-user-errors"></a>Fouten van de eindgebruiker

Deze lijst bevat informatie over fouten die uw eindgebruikers tegenkomen kunt wanneer ze proberen toegang tot de app en mislukt. 

| Fout | Aanbevolen stappen |
| ----- | ----------------- |
| De website kan de pagina niet weergeven. | Uw gebruikers foutmelding deze wanneer u probeert toegang tot de app die u als de toepassing een IWA is gepubliceerd. De gedefinieerde SPN voor deze toepassing is mogelijk onjuist. Geïntegreerde Windows-apps, zorg ervoor dat de SPN-naam geconfigureerd voor deze toepassing juist is. |
| De website kan de pagina niet weergeven. | Uw gebruikers foutmelding deze wanneer u probeert toegang tot de app die u hebt gepubliceerd als de toepassing een OWA-toepassing. Dit kan worden veroorzaakt door een van de volgende:<br><li>De gedefinieerde SPN voor deze toepassing is onjuist. Zorg ervoor dat de SPN-naam geconfigureerd voor deze toepassing juist is.</li><li>De gebruiker die toegang probeert te krijgen van de toepassing gebruikt een Microsoft-account in plaats van het juiste bedrijfsaccount aan te melden, of de gebruiker is een gastgebruiker. Zorg ervoor dat de gebruiker zich aanmeldt met hun bedrijfsaccount die overeenkomt met het domein van de gepubliceerde toepassing. Gebruikers van Microsoft-Account en Gast geen toegang tot toepassingen IWA.</li><li>De gebruiker die toegang probeert te krijgen van de toepassing is niet correct gedefinieerd voor deze toepassing aan de kant van de on-premises. Zorg ervoor dat deze gebruiker de juiste machtigingen heeft, zoals is gedefinieerd voor deze back-end-toepassing op de on-premises machine. |
| Deze zakelijke app kan niet worden geopend. U bent niet gemachtigd voor toegang tot deze toepassing. Autorisatie is mislukt. Zorg ervoor dat de gebruiker met toegang toewijzen aan deze toepassing. | Uw gebruikers foutmelding deze wanneer u probeert toegang tot de app die u hebt gepubliceerd als ze Microsoft-accounts in plaats van een bedrijfsaccount gebruiken aan te melden. Deze fout kunnen ook ophalen door gastgebruikers. Gebruikers van Microsoft-Account en gasten geen toegang tot toepassingen IWA. Zorg ervoor dat de gebruiker zich aanmeldt met hun bedrijfsaccount die overeenkomt met het domein van de gepubliceerde toepassing.<br><br>Mogelijk hebt u de gebruiker voor deze toepassing niet toegewezen. Ga naar de **toepassing** tabblad en klikt u onder **gebruikers en groepen**, deze gebruiker of groep toewijzen aan deze toepassing. |
| Deze zakelijke app kan niet nu worden geopend. Probeer het later opnieuw... Er is een time-out opgetreden voor de connector. | Uw gebruikers foutmelding deze wanneer u probeert toegang tot de app die u hebt gepubliceerd als ze zijn niet correct gedefinieerd voor deze toepassing aan de kant van de on-premises. Zorg ervoor dat uw gebruikers de juiste machtigingen hebben, zoals is gedefinieerd voor deze back-end-toepassing op de on-premises machine. |
| Deze zakelijke app kan niet worden geopend. U bent niet gemachtigd voor toegang tot deze toepassing. Autorisatie is mislukt. Zorg ervoor dat de gebruiker een licentie voor Azure Active Directory Premium of Basic. | Uw gebruikers foutmelding deze wanneer u probeert toegang tot de app die u hebt gepubliceerd als ze zijn niet expliciet met een Premium en eenvoudige licentie door de beheerder van de abonnee toegewezen. Ga naar de abonnee Active Directory **licenties** tabblad en zorg ervoor dat deze gebruiker of groep een Premium of Basic-licentie is toegewezen. |

## <a name="my-error-wasnt-listed-here"></a>Mijn fout niet is hier vermeld

Als er een fout of een probleem met Azure AD-toepassingsproxy die niet is vermeld in deze handleiding voor probleemoplossing optreden, willen we het gehoord. Een e-mail sturen naar onze [feedback team](mailto:aadapfeedback@microsoft.com) met de details van de fout.

## <a name="see-also"></a>Zie ook
* [Toepassingsproxy voor Azure Active Directory inschakelen](active-directory-application-proxy-enable.md)
* [Publiceren van toepassingen met toepassingsproxy](active-directory-application-proxy-publish.md)
* [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md)
* [Voorwaardelijke toegang inschakelen](application-proxy-enable-remote-access-sharepoint.md)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
