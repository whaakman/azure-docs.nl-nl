## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>Hoe APIM Proxy Server reageert met SSL-certificaten in de TLS-handshake

### <a name="clients-calling-with-sni-header"></a>Clients aanroepen met de SNI-header
Als de klant een of meerdere aangepaste domeinen die zijn geconfigureerd voor Proxy heeft, APIM kan reageren op HTTPS-aanvragen van de aangepaste domeinen (bijvoorbeeld contoso.com) en de standaarddomein (bijvoorbeeld apim service-name.azure api.net). Op basis van de informatie in de header van de Server de naam van vermelding (SNI), reageert APIM met de juiste server-certificaat.

### <a name="clients-calling-without-sni-header"></a>Clients aanroepen zonder SNI-header
Als de klant met behulp van een client verzendt de [SNI](https://tools.ietf.org/html/rfc6066#section-3) header APIM maakt op basis van de volgende logica antwoorden:

* Als de service slechts één aangepaste domein die zijn geconfigureerd voor Proxy heeft, is het standaard certificaat het certificaat dat was uitgegeven aan het aangepaste domein Proxy.
* Als de service meerdere aangepaste domeinen is geconfigureerd voor Proxy (alleen ondersteund in de **Premium** laag), de klant kunt opgeven welk certificaat moet het certificaat van het standaarddomein. Instellen van het standaardcertificaat de [defaultSslBinding](https://docs.microsoft.com/rest/api/apimanagement/apimanagementservice/createorupdate#hostnameconfiguration) eigenschap moet worden ingesteld op true ('defaultSslBinding': 'true'). Als de klant heeft niet de eigenschap is ingesteld, is het standaardcertificaat het certificaat verleend aan Proxy standaarddomein gehost op *.azure api.net.

## <a name="support-for-putpost-request-with-large-payload"></a>Ondersteuning voor plaatsen/POST-aanvraag met grote nettolading

Proxyserver APIM ondersteunt aanvraag met grote nettolading bij gebruik van certificaten aan de clientzijde in HTTPS (bijvoorbeeld nettolading > 40 KB). Om te voorkomen dat de aanvraag van de server bevriezing klanten stelt de eigenschap ["negotiateClientCertificate": "true"](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/CreateOrUpdate#hostnameconfiguration) op de hostnaam van de Proxy. Als de eigenschap is ingesteld op true, wordt de client certificaat wordt aangevraagd bij verbindingstijd van SSL/TLS, voordat u een HTTP-aanvraag exchange. Omdat de instelling is van toepassing op de **Proxy hostnaam** niveau, alle verbindingsaanvragen vraag om het clientcertificaat. Klanten kunnen maximaal 20 aangepaste domeinen configureren voor Proxy (alleen ondersteund in de **Premium** laag) en werk deze beperking weg te nemen.

