Hier vindt u de beperkingen voor gebruik en andere servicelimieten voor de Azure Active Directory-service.

| Category | Limieten |
| --- | --- |
| Mappen |Eén gebruiker kan slechts worden gekoppeld aan maximaal twintig Azure Active Directory-mappen.<br />Voorbeelden van mogelijke combinaties: <ul> <li>Eén gebruiker maakt twintig mappen.</li><li>Eén gebruiker wordt als lid toegevoegd aan twintig mappen.</li><li>Eén gebruiker maakt tien mappen en wordt later door anderen aan tien andere mappen toegevoegd.</li></ul> |
| Objecten |<ul><li>Er kunnen door gebruikers van de gratis versie van Azure Active Directory maximaal 500.000 objecten worden gebruikt in één map.</li><li>Niet-beheerders mogen maximaal 250 objecten maken.</li></ul> |
| Schema-uitbreidingen |<ul><li>Uitbreidingen van het type 'tekenreeks' mogen uit maximaal 256 tekens bestaan. </li><li>Uitbreidingen van het type 'binair' mogen uit maximaal 256 bytes bestaan.</li><li>Er mogen maximaal&100; uitbreidingswaarden (voor ALLE typen en ALLE toepassingen) worden geschreven naar één object.</li><li>Alleen de entiteiten User, Group, TenantDetail, Device, Application en ServicePrincipal mogen worden uitgebreid met kenmerken met één waarde van het type 'tekenreeks' of 'binair'.</li><li>Schema-uitbreidingen zijn alleen beschikbaar in de Graph API-versie 1.21-preview. Er moet schrijftoegang worden verleend aan de toepassing om een uitbreiding te kunnen registreren.</li></ul> |
| Toepassingen |Maximaal tien gebruikers mogen eigenaar zijn van één toepassing. |
| Groepen |<ul><li>Maximaal tien gebruikers mogen eigenaar zijn van één groep.</li><li>Een willekeurig aantal objecten kan lid zijn van één groep in Azure Active Directory.</li><li>Het aantal leden in een groep dat u kunt synchroniseren vanaf uw on-premises Active Directory naar Azure Active Directory is beperkt tot 15.000 (met gebruik van Active Directory-directorysynchronisatie (DirSync)).</li><li>Het aantal leden in een groep dat u kunt synchroniseren vanaf uw on-premises Active Directory naar Azure Active Directory met Azure AD Connect is beperkt tot 50.000.</li></ul> |
| Toegangsvenster |<ul><li>Er is geen limiet voor het aantal toepassingen dat te zien is in het toegangsvenster per eindgebruiker, per aan een gebruiker toegewezen licentie voor Azure AD Premium en per Enterprise Mobility Suite.</li><li>Er zijn maximaal 10 app-tegels (voor bijvoorbeeld Box, Salesforce of Dropbox) te zien in het toegangspaneel voor elke eindgebruiker, voor gebruikers met een gratis licentie en voor Azure AD Basic-versies van Azure Active Directory. Deze limiet geldt niet voor beheerdersaccounts.</li></ul> |
| Rapporten | Er kunnen maximaal 1000 rijen worden bekeken of gedownload in elk rapport. Aanvullende gegevens worden afgekapt. |
| Beheereenheden | Een object kan lid zijn van maximaal dertig beheereenheden. |


<!--HONumber=Feb17_HO2-->


