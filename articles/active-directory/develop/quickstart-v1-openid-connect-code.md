---
title: Azure AD-Node.js web-app aan de slag | Microsoft Docs
description: Meer informatie over het bouwen van een Node.js Express-MVC-web-app die is geïntegreerd met Azure AD voor aanmelden.
services: active-directory
documentationcenter: nodejs
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 81deecec-dbe2-4e75-8bc0-cf3788645f99
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 04/20/2018
ms.author: celested
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 8bba58c3493bc8adc17c5d4bca103326808d5b8b
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506638"
---
# <a name="azure-ad-nodejs-web-app-getting-started"></a>Azure AD-Node.js web-app aan de slag
We gebruiken hier Passport aan:

* De gebruiker aanmelden bij de app met Azure Active Directory (Azure AD).
* Informatie over de gebruiker weergegeven.
* Meld u aan de gebruiker buiten de app.

Passport is verificatiemiddleware voor Node.js. Flexibel en modulair, en Passport kunt onopvallend neerzetten voor een Express- of restify-webtoepassing. Een uitgebreide set strategieën ondersteunt verificatie die gebruikmaakt van een gebruikersnaam en wachtwoord, Facebook, Twitter en meer. We hebben een strategie ontwikkeld voor Microsoft Azure Active Directory. We installeert deze module en voegt u de Microsoft Azure Active Directory `passport-azure-ad` invoegtoepassing.

U doet dit door de volgende stappen uitvoeren:

1. Een app te registreren.
2. Instellen van uw app te gebruiken de `passport-azure-ad` strategie.
3. U gebruikt Passport om aan- en afmeldingsaanvragen te verzenden naar Azure AD.
4. Gegevens over de gebruiker afdrukken.

De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS). Als u wilt volgen, kunt u [basis van de app als een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) of het geraamte:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

De voltooide toepassing wordt verstrekt aan het einde van deze zelfstudie ook.

## <a name="step-1-register-an-app"></a>Stap 1: Een app registreren
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. In het menu aan de bovenkant van de pagina, selecteer uw account. Onder de **Directory** kiest u de Active Directory-tenant waar u uw toepassing registreren.

3. Selecteer **alle services** in het menu aan de linkerkant van het scherm en selecteer vervolgens **Azure Active Directory**.

4. Selecteer **App-registraties**, en selecteer vervolgens **toevoegen**.

5. Volg de aanwijzingen voor het maken van een **webtoepassing** en/of **WebAPI**.
  * De **naam** van de toepassing wordt beschreven voor uw gebruikers.

  * De **aanmeldings-URL** is de basis-URL van uw app. De standaardwaarde van het basisproject is `http://localhost:3000/auth/openid/return`.

6. Nadat u hebt geregistreerd, wijst Azure AD uw app een unieke toepassings-ID. U moet deze waarde in de volgende secties, dus te kopiëren uit de toepassingspagina.
7. Uit de **instellingen** -> **eigenschappen** pagina voor uw toepassing, het bijwerken van de URI van de App-ID. De **App ID URI** is een unieke id voor uw toepassing. De overeenkomst is met de indeling `https://<tenant-domain>/<app-name>`, bijvoorbeeld: `https://contoso.onmicrosoft.com/my-first-aad-app`.

8. Uit de **instellingen** -> **antwoord-URL's** pagina voor uw toepassing, de URL die is toegevoegd in de aanmeldings-URL uit stap 5 toevoegen en klik op opslaan.

9. Voor het maken van een geheime sleutel, volgt u stap 4 in [toepassing referenties of machtigingen voor toegang tot web-API's toe te voegen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis).

   > [!IMPORTANT]
   > Kopieer de waarde van de toepassing. Dit is de waarde voor de `clientSecret`, die u nodig hebt voor **stap 3** hieronder. 

## <a name="step-2-add-prerequisites-to-your-directory"></a>Stap 2: Vereisten aan de directory toevoegen
1. Vanaf de opdrachtregel, wijzig de mappen in de hoofdmap als u nog niet bent, en voer de volgende opdrachten:

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

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>Stap 3: De app instellen op het gebruik van de strategie passport-knooppunt-js
Hier, configureren we Express voor het gebruik van de OpenID Connect-verificatieprotocol. Passport wordt gebruikt om verschillende dingen, met inbegrip van aanvragen voor het aanmelden en afmelden van probleem doen, sessie van de gebruiker beheren en informatie over de gebruiker.

1. Als u wilt beginnen, opent u de `config.js` bestand in de hoofdmap van het project en voer vervolgens de configuratiewaarden van uw app in de `exports.creds` sectie.

  * De `clientID` is de **toepassings-Id** die toegewezen aan uw app in de portal voor wachtwoordregistratie.

  * De `returnURL` is de **antwoord-URL** die u hebt ingevoerd in de portal.

  * De `clientSecret` is het geheim dat u hebt gegenereerd in de portal.

2. Open vervolgens de `app.js` bestand in de hoofdmap van het project. Voeg de volgende oproep verzenden om aan te roepen de `OIDCStrategy` strategie die wordt geleverd met `passport-azure-ad`.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
        name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. Gebruik daarna de strategie die we zojuist waarnaar wordt verwezen voor het afhandelen van onze aanmeldingsaanvragen.

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
   In Passport wordt een vergelijkbaar patroon gebruikt voor alle strategieën (Twitter, Facebook, enzovoort) die alle schrijvers van strategieën voor. De strategie bekijkt, ziet u dat geven we een functie die een token en een gedaan als parameters heeft. De strategie komt weer naar ons nadat deze zijn werk doet. Vervolgens gaan we het opslaan van de gebruiker en het token niet initialiseren zodat we niet hoeft te vragen voor het opnieuw.

   > [!IMPORTANT]
   > De bovenstaande code wordt elke gebruiker die met onze server om te verifiëren. Dit wordt automatische registratie genoemd. Het is raadzaam dat u niet bij een productieserver iedereen zonder dat zij worden geregistreerd via een proces dat u eerst geverifieerd. Dit is doorgaans het patroon dat u ziet in consumenten-apps, waarmee u kunt registreren met Facebook, maar vervolgens vraagt u om aanvullende informatie te geven. Als dit niet een voorbeeldtoepassing, kan hebben we e-mailadres van de gebruiker opgehaald uit het Tokenobject dat wordt geretourneerd en wordt vervolgens gevraagd de gebruiker aanvullende informatie moet invullen. Omdat dit een testserver is, wordt deze toevoegen aan de database in het geheugen.


4. Vervolgens gaan we de methoden waarmee we voor het bijhouden van de aangemelde gebruikers zoals vereist door Passport toevoegen. Deze methoden omvatten het serialiseren en deserialiseren van gegevens van de gebruiker.

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

5. Vervolgens voegen we de code voor het laden van de Express-engine. Hier gebruiken we de standaard-/views en /routes-patroon dat Express biedt.

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

6. Voeg tot slot gaan we de routes die uit de daadwerkelijke aanmeldingsaanvragen aan de hand de `passport-azure-ad` engine:

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


## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Stap 4: Gebruikt Passport om uit te geven van aanmelding en afmeldingsaanvragen te verzenden naar Azure AD
Uw app is nu correct geconfigureerd om te communiceren met het eindpunt met behulp van de OpenID Connect-verificatieprotocol. `passport-azure-ad` de details van verificatieberichten, het valideren van tokens van Azure AD en het onderhoud van gebruikerssessies is afgehandeld. Alle dat resteert is en uw gebruikers een manier om te melden en meld u af en verzamelen van aanvullende informatie over de aangemelde gebruikers.

1. Eerst gaan we de standaard, aanmelden, -account en toevoegen afmeldingsmethoden aan onze `app.js` bestand:

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

2. Laten we bekijken dit uitvoerig beschreven:

  * De `/`route wordt omgeleid naar de weergave index.ejs, de gebruiker wordt doorgegeven in de aanvraag (indien aanwezig).
  * De `/account` eerst routeren *zorgt ervoor dat we worden geverifieerd* (we implementeren die in het volgende voorbeeld), en wordt de gebruiker doorgegeven in de aanvraag zodat we u meer informatie over de gebruiker kunnen krijgen.
  * De `/login` route roept onze authenticator azuread-openidconnect van `passport-azuread`. Als dat niet lukt, wordt de gebruiker omgeleid naar /login.
  * De `/logout` gewoon aanroepen de logout.ejs (en route) worden cookies gewist en retourneert vervolgens de gebruiker terug naar index.ejs routeren.

3. Voor het laatste deel van `app.js`, gaan we toevoegen de **EnsureAuthenticated** methode die wordt gebruikt in `/account`, zoals eerder besproken.

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

4. Maak ten slotte gaan we de server zelf in `app.js`:

```JavaScript
app.listen(3000);
```


## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a>Stap 5: Als de gebruiker in de website weergeven, maken de weergaven en routes in Express
Nu `app.js` is voltooid. We gewoon wilt toevoegen de routes en weergaven die de gegevens die we gaan naar de gebruiker, evenals verwerken de `/logout` en `/login` routes die we hebben gemaakt.

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

 Deze geven de aanvraag onze weergaven, met inbegrip van de gebruiker, indien aanwezig.

3. Maak de weergave `/views/index.ejs` onder de hoofddirectory. Dit is een eenvoudige pagina waarmee onze aanmelden en afmelden methoden aangeroepen en kan we het accountgegevens. U ziet dat het beleid kunnen worden gebruikt `if (!user)` als de gebruiker wordt doorgegeven in de aanvraag is bewijs hebben we een gebruiker is aangemeld.

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

4. Maak de `/views/account.ejs` weergeven onder de hoofddirectory, zodat we u meer informatie kunt weergeven die `passport-azure-ad` in de gebruikersaanvraag heeft geplaatst.

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

5. Laten we dit uiterlijk goed door het toevoegen van een lay-out. Maak de weergave `/views/layout.ejs` onder de hoofddirectory.

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

## <a name="next-steps"></a>Volgende stappen
Ten slotte, bouwen en uitvoeren van uw app. Voer `node app.js`, en ga vervolgens naar `http://localhost:3000`.

Meld u aan met een persoonlijk Microsoft-account of werk-of schoolaccount en u ziet hoe de identiteit van de gebruiker wordt weergegeven in de lijst met /account. U hebt nu een web-app die beveiligd met standaardprotocollen die gebruikers met hun persoonlijke en zakelijke/school accounts kunnen verifiëren.

Voor naslag is het voltooide voorbeeld (zonder uw configuratiewaarden) [geleverd als zip-bestand](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip). U kunt dit ook klonen vanuit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

U kunt nu verdergaan naar meer geavanceerde onderwerpen. Het is raadzaam om te proberen:

[Een Web-API met Azure AD beveiligen](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
