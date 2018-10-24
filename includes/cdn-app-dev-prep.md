## <a name="prerequisites"></a>Vereisten
Voordat het CDN management code schrijft, moet u de voorbereiding om in te schakelen van de code om te communiceren met de Azure Resource Manager doen. Als deze voorbereiding doet, moet u naar:

* Maak een resourcegroep om de CDN-profiel hebt gemaakt in deze zelfstudie
* Azure Active Directory voor de verificatie voor de toepassing configureren
* Machtigingen toepassen op de resourcegroep, zodat alleen gemachtigde gebruikers uit uw Azure AD-tenant met het CDN-profiel werken kunnen

### <a name="creating-the-resource-group"></a>De resourcegroep maken
1. Meld u aan bij [Azure-portal](https://portal.azure.com).
2. Klik op **Een resource maken**.
3. Zoeken naar **resourcegroep** en klik in het deelvenster van de groep Resource **maken**.

    ![Het maken van een nieuwe resourcegroep](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Noem uw resourcegroep *CdnConsoleTutorial*.  Selecteer uw abonnement en kies een locatie bij u in de buurt.  Als u wilt, kunt u klikken op de **vastmaken aan dashboard** selectievakje om de resourcegroep waarin het dashboard in de portal vast te maken.  Vast te maken, kunt u later gemakkelijk te vinden.  Nadat u uw selecties hebt gemaakt, klikt u op **maken**.

    ![Naamgeving van de resourcegroep](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Nadat de resourcegroep is gemaakt, als u deze niet hebt vastgemaakt aan uw dashboard, kunt u deze vinden door te klikken op **Bladeren**, klikt u vervolgens **resourcegroepen**.  Als u wilt openen, klikt u op de resourcegroep.  Maak een notitie van uw **abonnements-ID**. We nodig deze later.

    ![Naamgeving van de resourcegroep](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Het maken van de Azure AD-toepassing en machtigingen toe te passen
Er zijn twee methoden voor het app-verificatie met Azure Active Directory: individuele gebruikers of een service-principal. Een service-principal is vergelijkbaar met een service-account in Windows.  In plaats van een bepaalde gebruiker machtigingen verlenen aan de interactie met de CDN-profielen, worden in plaats daarvan machtigingen verleend aan de service-principal.  Service-principals worden meestal gebruikt voor geautomatiseerde, niet-interactieve processen.  Hoewel in deze zelfstudie een interactieve console-app schrijft is, ligt de focus op de aanpak van service-principal.

Het maken van een service-principal bestaat uit meerdere stappen, waaronder het maken van een Azure Active Directory-toepassing.  Als u wilt maken, gaan we [Volg deze zelfstudie](../articles/active-directory/develop/howto-create-service-principal-portal.md).

> [!IMPORTANT]
> Zorg ervoor dat u alle stappen in de [gekoppelde zelfstudie](../articles/active-directory/develop/howto-create-service-principal-portal.md).  Het is *belangrijk* u uit te voeren deze exact zoals wordt beschreven.  Zorg ervoor dat u Houd er rekening mee uw **tenant-ID**, **domeinnaam van tenant** (meestal een *. onmicrosoft.com* domein, tenzij u een aangepast domein hebt opgegeven), **client-ID** , en **client-verificatiesleutel**, zoals we deze gegevens later nodig.  Zorg ervoor dat u guard uw **client-ID** en **client-verificatiesleutel**, zoals deze referenties kunnen worden gebruikt door iedereen die bewerkingen uitvoeren als de service-principal.
>
> Wanneer u op de stap toepassing met meerdere tenants configureren met de naam, selecteer **Nee**.
>
> Wanneer u naar de stap krijgt [de toepassing toewijzen aan een rol](../articles/active-directory/develop/howto-create-service-principal-portal.md#assign-the-application-to-a-role), gebruikt u de resourcegroep die eerder hebt gemaakt, *CdnConsoleTutorial*, maar in plaats van de **lezer** rol toewijzen de **Inzender voor CDN-profiel** rol.  Na het toewijzen van de toepassing de **Inzender voor CDN-profiel** -rol op de resourcegroep, Ga terug naar deze zelfstudie. 
>
>

Nadat u hebt gemaakt van uw service-principal en toegewezen de **Inzender voor CDN-profiel** rol, de **gebruikers** blade voor de resourcegroep moet eruitzien als op de volgende afbeelding.

![Blade gebruikers](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interactieve gebruikersverificatie
Als u in plaats van een service-principal, hebt u liever interactieve afzonderlijke gebruikersverificatie, is het proces is vergelijkbaar met dat van een service-principal.  In feite, moet u dezelfde procedure volgen, maar een paar kleine wijzigingen aanbrengen.

> [!IMPORTANT]
> Volg deze volgende stappen alleen als u kiest voor het gebruik van afzonderlijke gebruikersverificatie in plaats van een service-principal.
>
>

1. Bij het maken van uw toepassing, in plaats van **webtoepassing**, kiest u **systeemeigen toepassing**.

    ![Systeemeigen toepassing](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. Op de volgende pagina wordt u gevraagd een **omleidings-URI**.  De URI wordt niet worden gevalideerd, maar vergeet niet dat u hebt ingevoerd. U hebt dit later nodig.
3. Er is niet nodig om te maken een **client-verificatiesleutel**.
4. In plaats van het toewijzen van een service-principal naar het **Inzender voor CDN-profiel** rol, gaan we afzonderlijke gebruikers of groepen toewijzen.  In dit voorbeeld kunt u zien dat ik heb toegewezen *CDN Demo-gebruiker* naar de **Inzender voor CDN-profiel** rol.  

    ![Afzonderlijke gebruikerstoegang](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
