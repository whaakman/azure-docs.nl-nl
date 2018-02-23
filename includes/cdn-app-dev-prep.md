## <a name="prerequisites"></a>Vereisten
Voordat het CDN management code schrijft, moet u doen voorbereiding, zodat de code om te communiceren met de Azure Resource Manager. U doet dit voorbereiding, moet u:

* Een resourcegroep bevat het CDN-profiel dat is gemaakt in deze zelfstudie maken
* Azure Active Directory voor de verificatie voor de toepassing configureren
* Machtigingen toepassen op de resourcegroep, zodat alleen gemachtigde gebruikers van uw Azure AD-tenant met de CDN-profiel communiceren kunnen

### <a name="creating-the-resource-group"></a>De resourcegroep maken
1. Meld u aan bij de [Azure-Portal](https://portal.azure.com).
2. Klik op **maken van een resource**.
3. Zoeken naar **resourcegroep** en klik in het deelvenster van de groep Resource op **maken**.

    ![Een nieuwe resourcegroep maken](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Naam van de resourcegroep *CdnConsoleTutorial*.  Selecteer uw abonnement en kies een locatie in de buurt.  Als u wenst, kunt u de **vastmaken aan dashboard** selectievakje in de resourcegroep naar het dashboard vastmaken in de portal.  Vastmaakt, wordt later gemakkelijk te vinden.  Nadat u uw selecties hebt gemaakt, klikt u op **maken**.

    ![Naamgeving van de resourcegroep](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Nadat de resourcegroep wordt gemaakt, als u deze niet aan uw dashboard vastmaken, kunt u deze vinden door te klikken op **Bladeren**, klikt u vervolgens **resourcegroepen**.  Om deze te openen, klikt u op de resourcegroep.  Maak een notitie van uw **abonnements-ID**. We nodig deze later.

    ![Naamgeving van de resourcegroep](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>De Azure AD-toepassing maken en toepassen van machtigingen
Er zijn twee benaderingen app verificatie met Azure Active Directory: individuele gebruikers of een service-principal. Een service-principal is vergelijkbaar met een serviceaccount in Windows.  In plaats van een bepaalde gebruiker machtigingen verlenen aan de interactie met de CDN-profielen, in plaats daarvan machtigingen aan de service-principal.  Service-principals worden meestal gebruikt voor geautomatiseerde, niet-interactieve processen.  Hoewel deze zelfstudie een interactieve console-app schrijft is, moet we zich richten op de service principal aanpak.

Maken van een service-principal bestaat uit verschillende stappen, zoals het maken van een Azure Active Directory-toepassing.  Om deze te maken, gaan we [Volg deze zelfstudie](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Volg de stappen in de [gekoppelde zelfstudie](../articles/resource-group-create-service-principal-portal.md).  Het is *belangrijke* dat u deze hebt voltooid zoals beschreven.  Zorg ervoor dat u Let op uw **tenant-ID**, **tenant domeinnaam** (meestal een *. onmicrosoft.com* domein tenzij u een aangepast domein hebt opgegeven), **client-ID** , en **client-verificatiesleutel**, zoals we deze informatie later nodig.  Wees voorzichtig te gebruiken zodat uw **client-ID** en **client-verificatiesleutel**, zoals deze referenties kunnen worden gebruikt door iedereen bewerkingen worden uitgevoerd als de service-principal.
>
> Als u de stap multitenant-toepassing configureren met de naam, selecteer **Nee**.
>
> Als u krijgt met de stap [toepassing toewijzen aan rol](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), de resourcegroep die eerder hebt gemaakt, *CdnConsoleTutorial*, maar in plaats van de **lezer** rol, de toewijzen **CDN-profiel Inzender** rol.  Nadat u de toepassing toewijzen de **CDN-profiel Inzender** rol in de resourcegroep, Ga terug naar deze zelfstudie. 
>
>

Nadat u hebt uw service-principal gemaakt en toegewezen de **CDN-profiel Inzender** rol, de **gebruikers** blade voor de resourcegroep zijn vergelijkbaar met de volgende afbeelding.

![Blade gebruikers](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interactieve gebruikersverificatie
Als u in plaats van een service-principal u liever interactieve afzonderlijke gebruikersverificatie, is het proces is vergelijkbaar met dat van een service-principal.  In feite, moet u dezelfde procedure volgen, maar een paar kleine wijzigingen aanbrengen.

> [!IMPORTANT]
> Deze stappen alleen volgende als u afzonderlijke gebruiker om verificatie te gebruiken in plaats van een service-principal.
>
>

1. Bij het maken van uw toepassing, in plaats van **webtoepassing**, kies **systeemeigen toepassing**.

    ![Systeemeigen toepassing](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. Op de volgende pagina wordt u gevraagd een **omleidings-URI**.  De URI won't worden gevalideerd, maar onthouden wat u hebt ingevoerd. U hebt dit later nodig.
3. Er is niet nodig voor het maken een **client-verificatiesleutel**.
4. In plaats van het toewijzen van een service-principal voor de **CDN-profiel Inzender** rol, gaan we individuele gebruikers of groepen toewijzen.  In dit voorbeeld kunt u zien dat ik hebt toegewezen *CDN Demo gebruiker* naar de **CDN-profiel Inzender** rol.  

    ![Afzonderlijke gebruikerstoegang](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
