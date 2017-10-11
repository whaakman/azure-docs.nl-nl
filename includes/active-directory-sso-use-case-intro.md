Met behulp van organisaties zijn meer [Software als een Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) toepassingen voor productiviteit omdat cloud-technologie en hulpprogramma's worden steeds vaker beschikbaar. Wanneer het aantal SaaS-apps groeit, wordt het lastig voor de accounts beheren en toegangsrechten-beheerders en voor de gebruikers om hun verschillende wachtwoorden te onthouden. Beheer van deze toepassingen afzonderlijk maakt extra werk en is minder veilig is.

* Werknemers die te veel wachtwoorden hebben meestal minder veilige methoden gebruiken om te onthouden zijn, of wachtwoorden schrijven met behulp van dezelfde wachtwoorden veel accounts.
* Wanneer een nieuwe werknemer binnenkomt of een verlaat, moeten alle hun accounts worden afzonderlijk ingericht of buiten gebruik gesteld.
* Bovendien werknemers kunnen aan de slag met SaaS-apps voor hun werk zonder tussenkomst van IT, wat betekent dat ze hun eigen accounts maken op systemen die de IT-beheerders nog niet goedgekeurd en worden niet controleren.  

Er is een oplossing voor al deze uitdagingen voor eenmalige aanmelding (SSO). Het is de eenvoudigste manier om meerdere apps beheren en gebruikers voorzien van een consistente ervaring voor eenmalige aanmelding. Azure Active Directory (Azure AD) biedt een robuuste oplossing voor eenmalige aanmelding en heeft veel beschikbaar vooraf geïntegreerde toepassingen, met zelfstudies voor beheerders om snel te instellen van een nieuwe app en start de inrichting van gebruikers.

## <a name="how-does-azure-active-directory-integrate-apps"></a>Hoe Azure Active Directory integreren van apps?
Azure AD kunt u uw apps en de ingerichte accounts integreren. Dit kan worden gedaan via een van twee methoden.

* Als de app vooraf geïntegreerde in de app-galerie is, kunt u deze portal apps instellen en configureer de instellingen voor het toestaan van SSO doorlopen. Voor elke app galerie, u kunt aan de slag door Volg de eenvoudige stapsgewijze instructies die zijn gepresenteerd in de app-galerie en in de Azure-portal voor eenmalige aanmelding inschakelen.
* Als de app niet in de galerie, kunt u de meeste apps nog steeds instellen in Azure AD als een aangepaste app. Hiervoor moet iets meer technische kennis om te configureren. U kunt elke toepassing die SAML 2.0 als federatieve app ondersteunt of elke toepassing die is een op basis van een HTML-aanmeldingspagina als wachtwoord SSO app toevoegen.

In het geval waar gebruikers hun eigen accounts voor SaaS-apps die niet worden beheerd door hebt gemaakt IT, de [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) hulpprogramma biedt een oplossing. Dit hulpprogramma controleert de webverkeer om te identificeren welke apps worden gebruikt in de organisatie en het aantal gebruikers van elk van deze. IT kunt deze informatie gebruiken voor meer informatie over welke apps gebruikers liever en bepalen welke integreren in Azure AD voor eenmalige aanmelding.  

Wanneer u een app in Azure AD integreren, kunt u de identiteit van de gebruikers tot stand gebrachte toepassing toewijzen aan hun respectieve identiteiten met Azure AD.  

