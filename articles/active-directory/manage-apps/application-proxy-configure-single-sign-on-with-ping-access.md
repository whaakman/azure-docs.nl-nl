---
title: Verificatie op basis van een koptekst met PingAccess voor Azure AD-toepassingsproxy | Microsoft Docs
description: Publiceren van toepassingen met PingAccess en App-Proxy voor de ondersteuning van verificatie op basis van een koptekst.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/11/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: c5a69e0c15d6d75bb4f182c1d8f62863aae7c760
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140843"
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Koptekst gebaseerde verificatie voor eenmalige aanmelding met de toepassingsproxy en PingAccess

Azure Active Directory-toepassingsproxy en PingAccess zijn een partnerschap aangegaan samen zodat klanten Azure Active Directory met toegang tot nog meer toepassingen. PingAccess breidt de [bestaande Application Proxy-aanbiedingen](application-proxy.md) om op te nemen van eenmalige aanmelding toegang tot toepassingen die headers voor verificatie gebruiken.

## <a name="what-is-pingaccess-for-azure-ad"></a>Wat is PingAccess voor Azure AD?

PingAccess voor Azure Active Directory is een aanbieding van PingAccess waarmee u kunt gebruikerstoegang en eenmalige aanmelding bieden voor toepassingen die headers voor verificatie gebruiken. Application Proxy behandelt deze apps zoals elk ander, met behulp van Azure AD verifiëren van toegang en vervolgens verkeer via de connector-service. PingAccess bevindt zich voor de apps en zet het toegangstoken van Azure AD in een koptekst, zodat de toepassing ontvangt de verificatie in de indeling die kan gelezen.

Uw gebruikers wordt niet ziet u iets anders wanneer ze zich aanmelden om uw zakelijke apps te gebruiken. Ze kunnen nog steeds overal werken vanaf op elk apparaat. 

Omdat de Application Proxy connectors extern verkeer naar alle apps, ongeacht hun verificatietype direct, blijven deze gewoon verdelen automatisch, maar ook.

## <a name="how-do-i-get-access"></a>Hoe krijg ik toegang?

Aangezien dit scenario wordt aangeboden via een verbinding tussen Azure Active Directory en PingAccess, moet u licenties voor beide services. Azure Active Directory Premium-abonnementen hebben echter een basic PingAccess-licentie die betrekking heeft op maximaal 20 toepassingen. Als u meer dan 20 headers gebaseerde toepassingen publiceert wilt, kunt u een extra licentie van PingAccess kopen. 

Zie [Azure Active Directory-edities](../fundamentals/active-directory-whatis.md) voor meer informatie.

## <a name="publish-your-application-in-azure"></a>Uw toepassing publiceren in Azure

In dit artikel is bedoeld voor mensen die een app met dit scenario voor het eerst wilt publiceren. Dit helpt u bij het aan de slag met de toepassings- en PingAccess, naast de publishing stappen. Als u beide services al hebt geconfigureerd, maar een opfrisser over het publiceren stappen wilt, kunt u de installatie van de connector overslaan en verplaatsen naar [toevoegen aan uw app naar Azure AD met Application Proxy](#add-your-app-to-Azure-AD-with-Application-Proxy).

>[!NOTE]
>Omdat dit scenario een partnerschap tussen Azure AD is en PingAccess, enkele van de instructies aanwezig zijn op de Ping Identity-site.

### <a name="install-an-application-proxy-connector"></a>Een Application Proxy-connector installeren

Als u al hebt van de toepassingsproxy is ingeschakeld, en een connector is geïnstalleerd, kunt u deze sectie overslaan en verplaatsen naar [toevoegen aan uw app naar Azure AD met Application Proxy](#add-your-app-to-azure-ad-with-application-proxy).

De Application Proxy-connector is een Windows Server-service die zorgt ervoor het verkeer van uw externe medewerkers naar uw gepubliceerde apps dat. Zie voor meer installatie-instructies gedetailleerde, [toepassingsproxy inschakelen in Azure portal](application-proxy-add-on-premises-application.md).

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als globale beheerder.
2. Selecteer **Azure Active Directory** > **toepassingsproxy**.
3. Selecteer **Connector downloaden** om de Application Proxy connector downloaden te starten. Volg de installatie-instructies.

   ![Toepassingsproxy inschakelen en de connector downloaden](./media/application-proxy-configure-single-sign-on-with-ping-access/install-connector.png)

4. Downloaden van de connector moet automatisch toepassingsproxy inschakelen voor uw directory, maar als u dit niet kunt selecteren **Enable Application Proxy**.


### <a name="add-your-app-to-azure-ad-with-application-proxy"></a>Uw app toevoegen aan Azure AD met Application Proxy

Er zijn twee acties die u moet uitvoeren in Azure portal. Eerst moet u uw toepassing met Application Proxy publiceren. Vervolgens moet u enkele gegevens over de app die u tijdens de stappen PingAccess gebruiken kunt verzamelen.

Volg deze stappen voor het publiceren van uw app. Voor een meer gedetailleerd overzicht van de stappen 1-8, Zie [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](application-proxy-add-on-premises-application.md).

1. Als u niet hebt gedaan in de laatste sectie, aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder.
2. Selecteer **Azure Active Directory** > **bedrijfstoepassingen**.
3. Selecteer **toevoegen** aan de bovenkant van de blade.
4. Selecteer **On-premises toepassing**.
5. Vul de vereiste velden met informatie over de nieuwe app. Gebruik de volgende richtlijnen voor de instellingen:
   - **Interne URL**: normaal gesproken bieden u de URL die u naar de aanmeldingspagina van de app gaat wanneer u zich in het bedrijfsnetwerk bevinden. Voor dit scenario moet de connector de proxy PingAccess behandelen als de front-pagina van de app. Gebruik de volgende notatie: `https://<host name of your PA server>:<port>`. De poort is 3000 standaard, maar u kunt deze configureren in PingAccess.

    > [!WARNING]
    > Voor dit type eenmalige aanmelding, de interne URL moet gebruikmaken van https en http niet gebruiken.

   - **Methode voor verificatie vooraf**: Azure Active Directory
   - **Vertalen van URL in de Headers**: Nee

   >[!NOTE]
   >Als dit uw eerste toepassing, moet u poort 3000 gebruiken om te starten en keert u terug naar het bijwerken van deze instelling als u de configuratie van uw PingAccess wijzigt. Als dit uw app tweede of hoger, moet deze overeenkomen met de Listener die u hebt geconfigureerd in PingAccess. Meer informatie over [listeners in PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).

6. Selecteer **toevoegen** aan de onderkant van de blade. Uw toepassing wordt toegevoegd en het menu Snel starten wordt geopend.
7. Selecteer in het menu snelle start **een gebruiker toewijzen voor het testen van**, en ten minste één gebruiker toevoegen aan de toepassing. Zorg ervoor dat deze testaccount toegang heeft tot de on-premises toepassing.
8. Selecteer **toewijzen** om op te slaan van de toewijzing van de gebruiker testen.
9. Selecteer op de blade van de app-beheer **eenmalige aanmelding**.
10. Kies **headers gebaseerde aanmelding** uit de vervolgkeuzelijst. Selecteer **Opslaan**.

   >[!TIP]
   >Als dit de eerste keer is met behulp van headers gebaseerde eenmalige aanmelding, moet u PingAccess installeren. Als u wilt controleren of dat uw Azure-abonnement wordt automatisch gekoppeld aan uw PingAccess-installatie, gebruikt u de koppeling op deze pagina voor eenmalige aanmelding voor het downloaden van PingAccess. U kunt nu openen van de site voor het downloaden of keert u terug naar deze pagina later opnieuw. 

   ![Selecteer headers gebaseerde aanmelding](./media/application-proxy-configure-single-sign-on-with-ping-access/sso-header.PNG)

11. Sluit de blade voor Enterprise-toepassingen of schuif helemaal naar links om terug te keren naar het menu van Azure Active Directory.
12. Selecteer **App-registraties**.

   ![App-registraties selecteren](./media/application-proxy-configure-single-sign-on-with-ping-access/app-registrations.png)

13. Selecteer de app die u zojuist hebt toegevoegd, klikt u vervolgens **antwoord-URL's**.

   ![Antwoord-URL's selecteren](./media/application-proxy-configure-single-sign-on-with-ping-access/reply-urls.png)

14. Controleer om te zien of de externe URL die u hebt toegewezen aan uw app in stap 5 in de lijst met antwoord-URL's. Als dat niet het geval is, moet u deze nu toevoegen.
15. Selecteer op de blade app-instellingen **vereiste machtigingen**.

  ![Selecteer de vereiste machtigingen](./media/application-proxy-configure-single-sign-on-with-ping-access/required-permissions.png)

16. Selecteer **Toevoegen**. Kies voor de API, **Windows Azure Active Directory**, klikt u vervolgens **Selecteer**. Voor de machtigingen, kies **lezen en schrijven van alle toepassingen** en **aanmelden en gebruikersprofiel lezen**, vervolgens **selecteren** en **gedaan**.  

  ![Machtigingen selecteren](./media/application-proxy-configure-single-sign-on-with-ping-access/select-permissions.png)

17. Machtigingen verlenen voordat u de machtigingen scherm sluit. 
![Machtigingen verlenen](./media/application-proxy-configure-single-sign-on-with-ping-access/grantperms.png)

### <a name="collect-information-for-the-pingaccess-steps"></a>Voor de stappen PingAccess informatie verzamelen

1. Selecteer op de blade van uw app-instellingen **eigenschappen**. 

  ![Eigenschappen selecteren](./media/application-proxy-configure-single-sign-on-with-ping-access/properties.png)

2. Sla de **toepassings-Id** waarde. Dit wordt gebruikt voor de client-ID bij het configureren van PingAccess.
3. Selecteer op de blade app-instellingen **sleutels**.

  ![Selecteer sleutels](./media/application-proxy-configure-single-sign-on-with-ping-access/Keys.png)

4. Een sleutel maken door te voeren van een sleutel beschrijving en een vervaldatum te kiezen in de vervolgkeuzelijst.
5. Selecteer **Opslaan**. Er wordt een GUID weergegeven in de **waarde** veld.

  Deze waarde wordt nu, opslaan als u het niet mogelijk om te zien het opnieuw nadat u dit venster sluiten.

  ![Een nieuwe sleutel maken](./media/application-proxy-configure-single-sign-on-with-ping-access/create-keys.png)

6. Sluit de blade App-registraties of schuif helemaal naar links om terug te keren naar het menu van Azure Active Directory.
7. Selecteer **eigenschappen**.
8. Sla de **map-ID** GUID.

### <a name="optional---update-graphapi-to-send-custom-fields"></a>Optioneel - Update GraphAPI voor het verzenden van aangepaste velden

Zie voor een lijst van beveiligingstokens die Azure AD wordt verzonden voor verificatie, [naslaginformatie over Azure AD-tokens](../develop/v1-id-and-access-tokens.md). Als u een aangepaste claim waarmee andere tokens worden verzonden, gebruikt u Graph Explorer of het manifest voor de toepassing in Azure Portal om in te stellen van het veld app *acceptMappedClaims* naar **waar**.    

In dit voorbeeld maakt gebruik van Graph Explorer:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```
In dit voorbeeld wordt de [Azure-portal](https://portal.azure.com) om bij te werken de *acceptedMappedClaims* veld:
1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als globale beheerder.
2. Selecteer **Azure Active Directory** > **App-registraties**.
3. Selecteer uw toepassing > **Manifest**.
4. Selecteer **bewerken**, zoek de *acceptedMappedClaims* veld en wijzig de waarde in **waar**.
![App-manifest](./media/application-proxy-configure-single-sign-on-with-ping-access/application-proxy-ping-access-manifest.PNG)
1. Selecteer **Opslaan**.

>[!NOTE]
>Voor het gebruik van een aangepaste claim, moet u ook een aangepast beleid gedefinieerd en toegewezen aan de toepassing hebben.  Dit beleid moet alle vereiste aangepaste kenmerken bevatten.
>
>Beleidsdefinitie en de toewijzing kunnen worden gedaan via PowerShell, Azure AD Graph Explorer of MS Graph.  Als u dit in PowerShell doet, moet u mogelijk eerst met `New-AzureADPolicy `en deze vervolgens toewijzen aan de toepassing met `Set-AzureADServicePrincipalPolicy`.  Zie voor meer informatie de [documentatie voor Azure AD-Policy](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

### <a name="optional---use-a-custom-claim"></a>Optioneel: gebruik een aangepaste claim
Als u wilt dat uw toepassing met een aangepaste claim en aanvullende velden opnemen, zorg ervoor dat u ook hebt [een aangepaste claims toewijzen van beleid gemaakt en toegewezen aan de toepassing](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

## <a name="download-pingaccess-and-configure-your-app"></a>PingAccess downloaden en configureren van uw app

Nu dat u alle stappen van de Azure Active Directory-instelling hebt voltooid, kunt u met het configureren van PingAccess op verplaatsen. 

De gedetailleerde stappen voor het PingAccess die deel uitmaken van dit scenario blijven in de documentatie van Ping Identity [PingAccess voor Azure AD configureren](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).

Deze stappen helpen u bij het proces van het ophalen van een PingAccess-account als u er nog geen hebt, de PingAccess-Server installeren en maken van een van de Azure AD OIDC-providerverbinding met de ID van de Directory die u hebt gekopieerd vanuit Azure portal. Vervolgens, gebruikt u de waarden toepassings-ID en sleutel te maken van een websessie op PingAccess. U kunt hierna Identiteitstoewijzing instellen en maken van een virtuele host, de site en de toepassing.

### <a name="test-your-app"></a>Uw app testen

Als u al deze stappen hebt voltooid, moet uw app te starten. Als u wilt testen, open een browser en navigeer naar de externe URL die u hebt gemaakt toen u de app hebt gepubliceerd in Azure. Meld u aan met de testaccount dat u hebt toegewezen aan de app.

## <a name="next-steps"></a>Volgende stappen

- [PingAccess voor Azure AD configureren](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Hoe biedt Azure AD-toepassingsproxy eenmalige aanmelding?](application-proxy-single-sign-on.md)
- [Application Proxy oplossen](application-proxy-troubleshoot.md)
