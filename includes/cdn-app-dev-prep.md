## <a name="prerequisites"></a>Vereisten
Voordat we CDN management code schrijven kunt, moeten we voorbereiding om in te schakelen van onze code om te communiceren met de Azure Resource Manager.  Om dit te doen, moet u het:

* Een resourcegroep bevat het CDN-profiel we in deze zelfstudie maken maken
* Azure Active Directory voor de verificatie voor de toepassing configureren
* Machtigingen toepassen op de resourcegroep, zodat u van ons Azure AD-tenant alleen geautoriseerde gebruikers kunnen communiceren met onze CDN-profiel

### <a name="creating-the-resource-group"></a>De resourcegroep maken
1. Meld u aan bij de [Azure-Portal](https://portal.azure.com).
2. Klik op de **nieuw** knop in de linkerbovenhoek, en vervolgens **Management**, en **resourcegroep**.

    ![Een nieuwe resourcegroep maken](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Aanroepen van de resourcegroep *CdnConsoleTutorial*.  Selecteer uw abonnement en kies een locatie in de buurt.  Als u wilt, klikt u op de **vastmaken aan dashboard** selectievakje in de resourcegroep naar het dashboard vastmaken in de portal.  Hiermee wordt u later gemakkelijk te vinden.  Nadat u uw selecties hebt gemaakt, klikt u op **maken**.

    ![Naamgeving van de resourcegroep](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Nadat de resourcegroep wordt gemaakt, als u deze niet aan uw dashboard vastmaken, kunt u deze vinden door te klikken op **Bladeren**, klikt u vervolgens **resourcegroepen**.  Klik op de resourcegroep om deze te openen.  Maak een notitie van uw **abonnements-ID**.  We hebt deze later nodig.

    ![Naamgeving van de resourcegroep](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>De Azure AD-toepassing maken en toepassen van machtigingen
Er zijn twee benaderingen app verificatie met Azure Active Directory: individuele gebruikers of een service-principal. Een service-principal is vergelijkbaar met een serviceaccount in Windows.  In plaats van een bepaalde gebruiker machtigingen verlenen aan de interactie met de CDN-profielen, wordt in plaats daarvan de machtigingen verlenen voor de service-principal.  Service-principals worden meestal gebruikt voor geautomatiseerde, niet-interactieve processen.  Hoewel deze zelfstudie een interactieve console-app schrijft is, moet we zich richten op de service principal aanpak.

Maken van een service-principal bestaat uit verschillende stappen, zoals het maken van een Azure Active Directory-toepassing.  Om dit te doen, gaan we [Volg deze zelfstudie](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Volg de stappen in de [gekoppelde zelfstudie](../articles/resource-group-create-service-principal-portal.md).  Het is *zeer belangrijk* dat u deze hebt voltooid zoals beschreven.  Zorg ervoor dat u Let op uw **tenant-ID**, **tenant domeinnaam** (meestal een *. onmicrosoft.com* domein tenzij u een aangepast domein hebt opgegeven), **client-ID** , en **client-verificatiesleutel**, zoals we zullen deze later nodig hebt.  Wees voorzichtig te gebruiken zodat uw **client-ID** en **client-verificatiesleutel**, zoals deze referenties kunnen worden gebruikt door iedereen bewerkingen worden uitgevoerd als de service-principal.
>
> Als u de stap multitenant-toepassing configureren met de naam, selecteer **Nee**.
>
> Als u krijgt met de stap [toepassing toewijzen aan rol](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), gebruikt u de resourcegroep die we eerder hebt gemaakt, *CdnConsoleTutorial*, maar in plaats van de **lezer** rol, de toewijzen **CDN-profiel Inzender** rol.  Nadat u de toepassing toewijzen de **CDN-profiel Inzender** rol in de resourcegroep, Ga terug naar deze zelfstudie. 
>
>

Nadat u hebt uw service-principal gemaakt en toegewezen de **CDN-profiel Inzender** rol, de **gebruikers** blade voor de resourcegroep moet er ongeveer als volgt uitzien.

![Blade gebruikers](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interactieve gebruikersverificatie
Als u in plaats van een service-principal u liever interactieve afzonderlijke gebruikersverificatie, is het proces is vergelijkbaar met die voor een service-principal.  In feite, moet u dezelfde procedure volgen, maar een paar kleine wijzigingen aanbrengen.

> [!IMPORTANT]
> Deze stappen alleen volgende als u afzonderlijke gebruiker om verificatie te gebruiken in plaats van een service-principal.
>
>

1. Bij het maken van uw toepassing, in plaats van **webtoepassing**, kies **systeemeigen toepassing**.

    ![Systeemeigen toepassing](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. Op de volgende pagina wordt u gevraagd om een **omleidings-URI**.  De URI won't worden gevalideerd, maar onthouden wat u hebt ingevoerd.  U hebt deze later nodig.
3. Er is niet nodig voor het maken een **client-verificatiesleutel**.
4. In plaats van het toewijzen van een service-principal voor de **CDN-profiel Inzender** rol, gaan we individuele gebruikers of groepen toewijzen.  In dit voorbeeld kunt u zien dat ik hebt toegewezen *CDN Demo gebruiker* naar de **CDN-profiel Inzender** rol.  

    ![Afzonderlijke gebruikerstoegang](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
