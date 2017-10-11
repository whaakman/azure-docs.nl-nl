---
title: Aan de slag met Azure AD aanmelden en afmelden met behulp van Node.js | Microsoft Docs
description: Informatie over het bouwen van een Node.js Express MVC-web-app die met Azure AD voor aanmelden integreert.
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 81deecec-dbe2-4e75-8bc0-cf3788645f99
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 13317b016f9ff3955f376b858645c42668b0de42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a>Node.js-web-app aanmelden en afmelden met Azure AD
We gebruiken hier Passport:

* De gebruiker zich aanmeldt bij de app met Azure Active Directory (Azure AD).
* Informatie over de gebruiker weergegeven.
* Meld u aan de gebruiker buiten de app.

Passport is verificatiemiddleware voor Node.js. Flexibel en modulair, Passport kan worden onopvallend verwijderd voor een Express- of restify-webtoepassing. Een uitgebreide set strategieën ondersteunt verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook, Twitter en meer. We hebben een strategie ontwikkeld voor Microsoft Azure Active Directory. We installeert deze module en voegt u de Microsoft Azure Active Directory `passport-azure-ad` invoegtoepassing.

U doet dit door de volgende stappen uitvoeren:

1. Een app te registreren.
2. Uw app instellen om te gebruiken de `passport-azure-ad` strategie.
3. U gebruikt Passport om aan- en afmeldingsaanvragen te verzenden naar Azure AD.
4. Gegevens over de gebruiker afdrukken.

De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).  Als u wilt volgen, kunt u [basis van de app downloaden als ZIP-bestand](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) of het geraamte:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

De voltooide toepassing wordt verstrekt aan het einde van deze zelfstudie ook.

## <a name="step-1-register-an-app"></a>Stap 1: Een app registreren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer in het menu aan de bovenkant van de pagina uw account. Onder de **Directory** kiest u de Active Directory-tenant waar u uw toepassing registreren.

3. Selecteer **meer Services** in het menu aan de linkerkant van het scherm en selecteer **Azure Active Directory**.

4. Selecteer **App registraties**, en selecteer vervolgens **toevoegen**.

5. Volg de aanwijzingen voor het maken van een **webtoepassing** en/of **WebAPI**.
  * De **naam** beschrijft uw toepassing voor gebruikers van de toepassing.

  * De **aanmeldings-URL** is de basis-URL van uw app.  Het basisproject standaardwaarde is "http://localhost: 3000/auth/openid/return ''.

6. Nadat u hebt geregistreerd, wijst Azure AD van uw app een unieke toepassings-ID. U moet deze waarde in de volgende secties, dus kopiëren van de toepassingspagina.
7. Van de **instellingen** -> **eigenschappen** pagina voor uw toepassing, het bijwerken van de App ID URI. De **App ID URI** is de unieke id voor uw toepassing. De overeenkomst is met de indeling `https://<tenant-domain>/<app-name>`, bijvoorbeeld: `https://contoso.onmicrosoft.com/my-first-aad-app`.

## <a name="step-2-add-prerequisites-to-your-directory"></a>Stap 2: Vereisten voor uw directory toevoegen
1. Vanaf de opdrachtregel, wijzig de mappen in de hoofdmap bent u niet al er, en voer de volgende opdrachten:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. U moet bovendien `passport-azure-ad`:
    * `npm install passport-azure-ad`

Hiermee installeert u de bibliotheken die `passport-azure-ad` is afhankelijk van.

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>Stap 3: Uw app instellen voor het gebruik van de strategie passport-knooppunt-js
We configureren hier snelle voor het gebruik van het OpenID Connect-verificatieprotocol.  Passport wordt gebruikt om verschillende dingen, met inbegrip van probleem aanmelden en afmeldingsaanvragen te verzenden, sessie van de gebruiker beheren en informatie ophalen over de gebruiker.

1. Om te beginnen, opent u de `config.js` bestand in de hoofdmap van het project en voer vervolgens de configuratiewaarden van uw app in de `exports.creds` sectie.

  * De `clientID` is de **toepassings-Id** die toegewezen aan uw app in de portal voor wachtwoordregistratie.

  * De `returnURL` is de **omleidings-Uri** die u hebt ingevoerd in de portal.

  * De `clientSecret` is het geheim die u hebt gegenereerd in de portal.

2. Open vervolgens de `app.js` bestand in de hoofdmap van het project. Voeg de volgende oproep verzenden om aan te roepen de `OIDCStrategy` strategie die wordt geleverd met `passport-azure-ad`.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. Gebruik daarna de strategie die we zojuist waarnaar wordt verwezen voor het afhandelen van onze aanvragen aanmelden.

    ```JavaScript
    // Use the OIDCStrategy within Passport. (Section 2)
    //
    //   Strategies in passport require a `validate` function that accepts
    //   credentials (in this case, an OpenID identifier), and invokes a callback
    //   with a user object.
    passport.use(new OIDCStrategy({
        callbackURL: config.creds.returnURL,
        realm: config.creds.realm,
        clientID: config.creds.clientID,
        clientSecret: config.creds.clientSecret,
        oidcIssuer: config.creds.issuer,
        identityMetadata: config.creds.identityMetadata,
        skipUserProfile: config.creds.skipUserProfile,
        responseType: config.creds.responseType,
        responseMode: config.creds.responseMode
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
        if (!profile.email) {
        return done(new Error("No email found"), null);
        }
        // asynchronous verification, for effect...
        process.nextTick(function () {
        findByEmail(profile.email, function(err, user) {
            if (err) {
            return done(err);
            }
            if (!user) {
            // "Auto-registration"
            users.push(profile);
            return done(null, profile);
            }
            return done(null, user);
        });
        });
    }
    ));
    ```
Passport wordt een vergelijkbaar patroon gebruikt voor alle strategieën (Twitter, Facebook, enzovoort) die alle schrijvers van strategieën voor. De strategie bekijkt, ziet u dat geven we een functie die een token en een gereed als parameters heeft. De strategie komt weer naar ons nadat deze zijn werk doet. We willen vervolgens om de gebruiker op te slaan en het token niet initialiseren zodat we niet hoeven te vragen voor het opnieuw.

> [!IMPORTANT]
De vorige code wordt elke gebruiker die plaatsvindt om te verifiëren met onze server. Dit wordt automatische registratie genoemd. Het is raadzaam dat u niet toestaan dat iedereen bij een productieserver zonder dat zij worden geregistreerd via een proces dat u kiest eerst geverifieerd. Dit is doorgaans het patroon die u ziet in consumenten-apps, waarmee u kunt registreren met Facebook, maar vervolgens vraagt u om aanvullende informatie te geven. Als dit niet een voorbeeldtoepassing, kan hebben we e-mailadres van de gebruiker opgehaald uit het Tokenobject dat wordt geretourneerd en wordt vervolgens gevraagd de gebruiker om aanvullende informatie in te vullen. Omdat dit een testserver is, wordt deze toevoegen aan de database in het geheugen.


4. Vervolgens laten we de methoden waarmee we voor het bijhouden van de aangemelde gebruikers zoals vereist door Passport toevoegen. Deze methoden omvatten serialiseren en deserialiseren van gegevens van de gebruiker.

    ```JavaScript

            // Passport session setup. (Section 2)

            //   To support persistent sign-in sessions, Passport needs to be able to
            //   serialize users into the session and deserialize them out of the session. Typically,
            //   this is done simply by storing the user ID when serializing and finding
            //   the user by ID when deserializing.
            passport.serializeUser(function(user, done) {
            done(null, user.email);
            });

            passport.deserializeUser(function(id, done) {
            findByEmail(id, function (err, user) {
                done(err, user);
            });
            });

            // array to hold signed-in users
            var users = [];

            var findByEmail = function(email, fn) {
            for (var i = 0, len = users.length; i < len; i++) {
                var user = users[i];
            log.info('we are using user: ', user);
                if (user.email === email) {
                return fn(null, user);
                }
            }
            return fn(null, null);
            };
    ```

5.  Vervolgens voegen we de code voor het laden van de Express-engine. Hier gebruiken we de standaard /views en /routes patroon dat Express biedt.

    ```JavaScript

        // configure Express (section 2)

            var app = express();
            app.configure(function() {
          app.set('views', __dirname + '/views');
          app.set('view engine', 'ejs');
          app.use(express.logger());
          app.use(express.methodOverride());
          app.use(cookieParser());
          app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
          app.use(bodyParser.urlencoded({ extended : true }));
          // Initialize Passport!  Also use passport.session() middleware, to support
          // persistent login sessions (recommended).
          app.use(passport.initialize());
          app.use(passport.session());
          app.use(app.router);
          app.use(express.static(__dirname + '/../../public'));
        });

    ```

6. Laten we Voeg de routes die aanlevert de werkelijke aanmelden aanvragen voor de `passport-azure-ad` engine:


       ```JavaScript

        // Our Auth routes (section 3)

        // GET /auth/openid
        //   Use passport.authenticate() as route middleware to authenticate the
        //   request. The first step in OpenID authentication involves redirecting
        //   the user to their OpenID provider. After authenticating, the OpenID
        //   provider redirects the user back to this application at
        //   /auth/openid/return.
        app.get('/auth/openid',
        passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
        function(req, res) {
            log.info('Authentication was called in the Sample');
            res.redirect('/');
        });

            // GET /auth/openid/return
            //   Use passport.authenticate() as route middleware to authenticate the
            //   request. If authentication fails, the user is redirected back to the
            //   sign-in page. Otherwise, the primary route function is called,
            //   which, in this example, redirects the user to the home page.
            app.get('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });

            // POST /auth/openid/return
            //   Use passport.authenticate() as route middleware to authenticate the
            //   request. If authentication fails, the user is redirected back to the
            //   sign-in page. Otherwise, the primary route function is called,
            //   which, in this example, redirects the user to the home page.
            app.post('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });
       ```


## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Stap 4: Gebruik Passport om aan- en afmeldingsaanvragen te verzenden naar Azure AD
Uw app is nu geconfigureerd om te communiceren met het eindpunt met behulp van het OpenID Connect-verificatieprotocol.  `passport-azure-ad`heeft gezorgd voor de details van verificatieberichten, het valideren van tokens van Azure AD en het onderhoud van gebruikerssessies. Alle resterende is door uw gebruikers te kunnen aanmelden en afmelden en verzamelen van aanvullende informatie over de aangemelde gebruikers.

1. Eerst gaan we de standaard, aanmelden, account en toevoegen afmeldingsmethoden toe aan onze `app.js` bestand:

    ```JavaScript

        //Routes (section 4)

        app.get('/', function(req, res){
          res.render('index', { user: req.user });
        });

        app.get('/account', ensureAuthenticated, function(req, res){
          res.render('account', { user: req.user });
        });

        app.get('/login',
          passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
          function(req, res) {
            log.info('Login was called in the Sample');
            res.redirect('/');
        });

        app.get('/logout', function(req, res){
          req.logout();
          res.redirect('/');
        });

    ```

2.  Laten we controleert deze onderdelen:

  * De `/`route wordt omgeleid naar de weergave index.ejs doorgeven van de gebruiker in de aanvraag (indien aanwezig).
  * De `/account` eerst routeren *zorgt ervoor dat we worden geverifieerd* (we implementeren die in het volgende voorbeeld), en wordt de gebruiker doorgegeven in de aanvraag zodat we extra informatie over de gebruiker kunt ophalen.
  * De `/login` route roept onze verificator azuread openidconnect van `passport-azuread`. Als dat niet lukt, wordt de gebruiker omgeleid naar /login.
  * De `/logout` gewoon aanroepen de logout.ejs (en route) die worden cookies gewist en retourneert vervolgens de gebruiker terug naar index.ejs routeren.

3. Voor het laatste deel van `app.js`, voegen we de **EnsureAuthenticated** methode die wordt gebruikt in `/account`, zoals eerder is weergegeven.

    ```JavaScript

        // Simple route middleware to ensure user is authenticated. (section 4)

        //   Use this route middleware on any resource that needs to be protected. If
        //   the request is authenticated (typically via a persistent sign-in session),
        //   the request proceeds. Otherwise, the user is redirected to the
        //   sign-in page.
        function ensureAuthenticated(req, res, next) {
          if (req.isAuthenticated()) { return next(); }
          res.redirect('/login')
        }
    ```

4. Maak ten slotte de server zelf laten we in `app.js`:

```JavaScript

        app.listen(3000);

```


## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a>Stap 5: Zodat de gebruiker in de website maken de weergaven en routes in Express
Nu `app.js` is voltooid. Moet u de routes en weergaven waarin de informatie die we u aan de gebruiker, evenals verwerken toevoegen de `/logout` en `/login` routes die we hebben gemaakt.

1. Maak de route `/routes/index.js` onder de hoofddirectory.

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. Maak de route `/routes/user.js` onder de hoofddirectory.

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 Deze doorgegeven de aanvraag aan onze weergaven, met inbegrip van de gebruiker, indien aanwezig.

3. Maak de weergave `/views/index.ejs` onder de hoofddirectory. Dit is een eenvoudige pagina waarmee onze aanmelding en afmelding methoden aangeroepen en kan we accountgegevens halen. U ziet dat we de voorwaardelijke kunnen gebruiken `if (!user)` als de gebruiker wordt doorgegeven in de aanvraag is bewijs hebben we een aangemelde gebruiker.

    ```JavaScript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
        <h2>Hello, <%= user.displayName %>.</h2>
        <a href="/account">Account Info</a></br>
        <a href="/logout">Log Out</a>
    <% } %>
    ```

4. Maak de `/views/account.ejs` weergeven onder de hoofddirectory, zodat we extra informatie kunt bekijken die `passport-azuread` in de gebruikersaanvraag heeft geplaatst.

    ```Javascript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
    <p>displayName: <%= user.displayName %></p>
    <p>givenName: <%= user.name.givenName %></p>
    <p>familyName: <%= user.name.familyName %></p>
    <p>UPN: <%= user._json.upn %></p>
    <p>Profile ID: <%= user.id %></p>
  ##Next steps  <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

5. We maken dit goed door het toevoegen van een lay-out. Maak de ' / views/layout.ejs' weergave onder de hoofdmap.

    ```HTML

    <!DOCTYPE html>
    <html>
        <head>
            <title>Passport-OpenID Example</title>
        </head>
        <body>
            <% if (!user) { %>
                <p>
                <a href="/">Home</a> |
                <a href="/login">Log In</a>
                </p>
            <% } else { %>
                <p>
                <a href="/">Home</a> |
                <a href="/account">Account</a> |
                <a href="/logout">Log Out</a>
                </p>
            <% } %>
            <%- body %>
        </body>
    </html>
    ```

##<a name="next-steps"></a>Volgende stappen
Ten slotte bouwen en uitvoeren van uw app. Voer `node app.js`, en ga vervolgens naar `http://localhost:3000`.

Meld u aan met een persoonlijk Microsoft-account of een account voor werk of school en zien hoe de identiteit van de gebruiker wordt weergegeven in de lijst /account. U hebt nu een web-app die beveiligd met standaardprotocollen die gebruikers met hun persoonlijke en zakelijke/school accounts kunnen worden geverifieerd.

Voor naslag is het voltooide voorbeeld (zonder uw configuratiewaarden) [geleverd als zip-bestand](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip). U kunt dit ook klonen vanuit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

U kunt nu verplaatsen naar geavanceerdere onderwerpen. Het is raadzaam om te proberen:

[Een Web-API met Azure AD beveiligen](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
