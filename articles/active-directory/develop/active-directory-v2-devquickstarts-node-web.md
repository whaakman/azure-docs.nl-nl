---
title: Azure Active Directory-v2.0 Node.js web-app aanmelden | Microsoft Docs
description: Informatie over het bouwen van een Node.js-web-app die een gebruiker zich aanmeldt met behulp van zowel een persoonlijk Microsoft-account en een account voor werk of school.
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mtillman
editor: 
ms.assetid: 1b889e72-f5c3-464a-af57-79abf5e2e147
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 230d8ad16dc62564f3c1149443dd59fbb9974db5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="add-sign-in-to-a-nodejs-web-app"></a>Aanmelden toevoegen aan een Node.js-web-app

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en onderdelen werken met het v2.0-eindpunt. Meer informatie over om te bepalen of u het v2.0-eindpunt of het eindpunt v1.0 moet gebruiken, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 

In deze zelfstudie gebruiken we Passport naar de volgende taken uitvoeren:

* Meld u de gebruiker met behulp van Azure Active Directory (Azure AD) en het v2.0-eindpunt in een web-app.
* Informatie over de gebruiker weergegeven.
* Meld u aan de gebruiker buiten de app.

**Passport** is verificatiemiddleware voor Node.js. Flexibel en modulair, Passport kan onopvallend worden verwijderd in een snelle gebaseerde of restify-webtoepassing. In Passport, een uitgebreide set strategieën ondersteuning voor verificatie met behulp van een gebruikersnaam en wachtwoord, Facebook, Twitter of andere opties. We hebben een strategie ontwikkeld voor Azure AD. In dit artikel wordt beschreven hoe u installeert de module en voegt u de Azure AD `passport-azure-ad` invoegtoepassing.

## <a name="download"></a>Downloaden
De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs). Wilt u de zelfstudie, kunt u [basis van de app downloaden als ZIP-bestand](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) of het geraamte:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

U kunt ook de voltooide toepassing aan het einde van deze zelfstudie ophalen.

## <a name="1-register-an-app"></a>1: een app registreren
Maakt een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of voert u [deze gedetailleerde stappen](active-directory-v2-app-registration.md) om een app te registreren. Zorg ervoor dat u:

* Kopieer de **toepassings-Id** toegewezen aan uw app. U moet voor deze zelfstudie.
* Voeg de **Web** platform voor uw app.
* Kopieer de **omleidings-URI** vanuit de portal. Moet u de standaardwaarde van de URI van `urn:ietf:wg:oauth:2.0:oob`.

## <a name="2-add-prerequisities-to-your-directory"></a>2: prerequisities toevoegen aan uw directory
Bij een opdrachtprompt, wijzig de mappen naar de hoofdmap, als u niet al er. Voer de volgende opdrachten uit:

* `npm install express`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install restify`
* `npm install mongoose`
* `npm install bunyan`
* `npm install assert-plus`
* `npm install passport`
* `npm install webfinger`
* `npm install body-parser`
* `npm install express-session`
* `npm install cookie-parser`

Daarnaast gebruiken wij `passport-azure-ad` in de basis van de Snelstartgids:

* `npm install passport-azure-ad`

Hiermee installeert u de bibliotheken die `passport-azure-ad` gebruikt.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3: uw app instellen voor gebruik van de strategie passport-knooppunt-js
De Express-middleware instellen voor gebruik van het OpenID Connect-verificatieprotocol. U kunt Passport aan- en afmeldingsaanvragen te verzenden en informatie ophalen over de gebruiker, onder andere de gebruikerssessie te beheren.

1.  Open het bestand Config.js file in de hoofdmap van het project. In de `exports.creds` sectie, voert u de configuratiewaarden van uw app.
  
  * `clientID`: De **toepassings-Id** die toegewezen aan uw app in de Azure-portal.
  * `returnURL`: De **omleidings-URI** die u hebt ingevoerd in de portal.
  * `clientSecret`: Het geheim dat u hebt gegenereerd in de portal.

2.  Open het bestand App.js in de hoofdmap van het project. Om aan te roepen de stratey OIDCStrategy, wordt geleverd met `passport-azure-ad`, voeg de volgende oproep verzenden:

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  Gebruik de strategie die u zojuist hebt om uw aanmelding verzoeken afhandelen waarnaar wordt verwezen:

  ```JavaScript
  // Use the OIDCStrategy within Passport (section 2)
  //
  //   Strategies in Passport require a `validate` function. The function accepts
  //   credentials (in this case, an OpenID identifier), and invokes a callback
  //   with a user object.
  passport.use( new OIDCStrategy({
      callbackURL: config.creds.returnURL,
      realm: config.creds.realm,
      clientID: config.creds.clientID,
      clientSecret: config.creds.clientSecret,
      oidcIssuer: config.creds.issuer,
      identityMetadata: config.creds.identityMetadata,
      responseType: config.creds.responseType,
      responseMode: config.creds.responseMode,
      skipUserProfile: config.creds.skipUserProfile
      scope: config.creds.scope
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
      log.info('Example: Email address we received was: ', profile.email);
      // Asynchronous verification, for effect...
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

Passport wordt een vergelijkbaar patroon gebruikt voor alle strategieën (Twitter, Facebook, enzovoort). Alle schrijvers van strategieën voor het patroon. Geeft de strategie voor een `function()` die gebruikmaakt van een token en `done` als parameters. De strategie terug nadat al het werk wordt. Sla de gebruiker en het token niet initialiseren zodat u niet hoeft te vragen voor het opnieuw.

  > [!IMPORTANT]
  > De voorafgaande code geldt voor elke gebruiker die kan worden geverifieerd met de server. Dit wordt automatische registratie genoemd. Op een productieserver wilt u niet dat iedereen, kunnen zonder dat zij een registratieproces die u kiest doorlopen eerst. Dit is doorgaans het patroon die u in consumenten-apps ziet. De app kunt u mogelijk registreren met Facebook, maar vervolgens wordt u gevraagd om in te voeren als u meer informatie. Als u een opdrachtregelprogramma zijn niet voor deze zelfstudie gebruikt, kunt u het e-mailbericht kan extraheren uit het Tokenobject dat wordt geretourneerd. Vervolgens vraagt u mogelijk de gebruiker in te voeren als u meer informatie. Omdat dit een testserver is, moet u de gebruiker rechtstreeks naar de database in het geheugen toevoegen.
  > 
  > 

4.  Voeg de methoden die u gebruikt voor het bijhouden van gebruikers die zijn aangemeld, zoals wordt vereist door Passport. Dit omvat het serialiseren en deserialiseren van gegevens van de gebruiker:

  ```JavaScript

  // Passport session setup (section 2)

  //   To support persistent login sessions, Passport needs to be able to
  //   serialize users into, and deserialize users out of, the session. Typically,
  //   this is as simple as storing the user ID when serializing, and finding
  //   the user by ID when deserializing.
  passport.serializeUser(function(user, done) {
    done(null, user.email);
  });

  passport.deserializeUser(function(id, done) {
    findByEmail(id, function (err, user) {
      done(err, user);
    });
  });

  // Array to hold signed-in users
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

5.  Voeg de code die de Express-engine wordt geladen. Gebruik van de standaard /views en /routes patroon dat Express:

  ```JavaScript

  // Set up Express (section 2)

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

6.  Toevoegen van het bericht van routes die lever de werkelijke aanmelden aanvragen voor de `passport-azure-ad` engine:

  ```JavaScript

  // Auth routes (section 3)

  // GET /auth/openid
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. The first step in OpenID authentication involves redirecting
  //   the user to the user's OpenID provider. After authenticating, the OpenID
  //   provider redirects the user back to this application at
  //   /auth/openid/return.

  app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Authentication was called in the sample');
      res.redirect('/');
    });

  // GET /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called.
  //   In this example, it redirects the user to the home page.
  app.get('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });

  // POST /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called. 
  //   In this example, it redirects the user to the home page.

  app.post('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4: gebruik Passport om aan- en afmeldingsaanvragen te verzenden naar Azure AD
Uw app is nu ingesteld om te communiceren met het v2.0-eindpunt met behulp van het OpenID Connect-verificatieprotocol. De `passport-azure-ad` strategie zorgt voor de details van verificatieberichten, het valideren van tokens van Azure AD en het onderhoud van de gebruikerssessie. Alle die nog moet doen om uw gebruikers aanmelden en afmelden en meer informatie over de gebruiker die is aangemeld verzamelen is.

1.  Voeg de **standaard**, **aanmelding**, **account**, en **afmelding** methoden voor het bestand App.js:

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
      log.info('Login was called in the sample');
      res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.logout();
    res.redirect('/');
  });

  ```

  Hier volgen de details:
    
    * De `/` route wordt omgeleid naar de weergave index.ejs. Dit wordt de gebruiker doorgegeven in de aanvraag (indien aanwezig).
    * De `/account` eerst routeren *zorgt ervoor dat u bent geverifieerd* (u implementeert die in de volgende code). Vervolgens wordt de gebruiker in de aanvraag doorgegeven. Dit is zodat u meer informatie over de gebruiker krijgt.
    * De `/login` gesprekken rondsturen uw `azuread-openidconnect` verificator van `passport-azuread`. Als dat niet lukt, wordt de gebruiker wordt omgeleid naar `/login`.
    * De `/logout` route roept de logout.ejs weergeven (en route). Hiermee worden cookies gewist en retourneert vervolgens de gebruiker terug naar index.ejs.

2.  Voeg de **EnsureAuthenticated** methode die u eerder in gebruikt `/account`:

  ```JavaScript

  // Route middleware to ensure the user is authenticated (section 4)

  //   Use this route middleware on any resource that needs to be protected. If
  //   the request is authenticated (typically via a persistent login session),
  //   the request proceeds. Otherwise, the user is redirected to the
  //   sign-in page.
  function ensureAuthenticated(req, res, next) {
    if (req.isAuthenticated()) { return next(); }
    res.redirect('/login')
  }

  ```

3.  Maak in App.js, de server:

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-the-views-and-routes-in-express-that-you-show-your-user-on-the-website"></a>5: Maak de weergaven en routes in Express dat u uw gebruikers worden weergegeven op de website
Voeg de routes en weergaven die aan de gebruiker alleen informatie weergegeven. De routes en weergaven verwerken ook de `/logout` en `/login` routes die u hebt gemaakt.

1. In de hoofdmap, maken de `/routes/index.js` route.

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  In de hoofdmap, maken de `/routes/user.js` route.

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  `/routes/index.js`en `/routes/user.js` zijn eenvoudige routes die de aanvraag aan uw weergaven doorgegeven, met inbegrip van de gebruiker, indien aanwezig.

3.  In de hoofdmap, maken de `/views/index.ejs` weergeven. Deze pagina roept de **aanmelding** en **afmelding** methoden. Ook gebruiken de `/views/index.ejs` weergave voor het vastleggen van gegevens van het account. U kunt de voorwaardelijke `if (!user)` als de gebruiker in de aanvraag wordt doorgegeven. Het is bewijs dat u een gebruiker aangemeld hebt.

  ```JavaScript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
      <h2>Hello, <%= user.displayName %>.</h2>
      <a href="/account">Account info</a></br>
      <a href="/logout">Sign out</a>
  <% } %>
  ```

4.  In de hoofdmap, maken de `/views/account.ejs` weergeven. De `/views/account.ejs` weergave kunt u aanvullende informatie weergeven die `passport-azuread` op aanvraag van de gebruiker worden geplaatst.

  ```Javascript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
  <p>displayName: <%= user.displayName %></p>
  <p>givenName: <%= user.name.givenName %></p>
  <p>familyName: <%= user.name.familyName %></p>
  <p>UPN: <%= user._json.upn %></p>
  <p>Profile ID: <%= user.id %></p>
  <p>Full Claimes</p>
  <%- JSON.stringify(user) %>
  <p></p>
  <a href="/logout">Sign out</a>
  <% } %>
  ```

5.  Toevoegen van een lay-out. In de hoofdmap, maken de `/views/layout.ejs` weergeven.

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
              <a href="/login">Sign in</a>
              </p>
          <% } else { %>
              <p>
              <a href="/">Home</a> |
              <a href="/account">Account</a> |
              <a href="/logout">Sign out</a>
              </p>
          <% } %>
          <%- body %>
      </body>
  </html>
  ```

6.  Voor het bouwen en uitvoeren van uw app, voer `node app.js`. Ga vervolgens naar `http://localhost:3000`.

7.  Aanmelden met een persoonlijk Microsoft-account of een account voor werk of school. Houd er rekening mee dat de identiteit van de gebruiker wordt weergegeven in de lijst /account. 

U hebt nu een web-app die is beveiligd met behulp van protocollen volgens de industrienorm. U kunt gebruikers in uw app verifiëren met behulp van hun persoonlijke en werk of school-account.

## <a name="next-steps"></a>Volgende stappen
Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) wordt geleverd als [een ZIP-bestand](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip). U kunt dit ook klonen vanuit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Vervolgens kunt u op verplaatsen met geavanceerdere onderwerpen. Het is raadzaam om te proberen:

[Een Node.js-web-API beveiligen met behulp van het v2.0-eindpunt](active-directory-v2-devquickstarts-node-api.md)

Hier volgen enkele aanvullende resources:

* [Handleiding voor Azure AD v2.0-ontwikkelaars](active-directory-appmodel-v2-overview.md)
* [Stack-overloop 'azure active directory' tag](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten downloaden
We raden u aan te melden om te worden geïnformeerd wanneer er beveiligingsincidenten optreden. Op de [Microsoft technische beveiligingsmeldingen](https://technet.microsoft.com/security/dd252948) pagina moet u zich abonneren op beveiligingswaarschuwingen aanbevelingen.

