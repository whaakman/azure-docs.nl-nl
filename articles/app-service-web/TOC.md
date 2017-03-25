# Overzicht    
## [Meer informatie over Web Apps](app-service-web-overview.md)
## [Hostingopties vergelijken](choose-web-site-cloud-service-vm.md)

# Snelstartgids    
## [Statische HTML-site maken](app-service-web-get-started-html.md)
## [ASP.NET-app maken](app-service-web-get-started-dotnet.md)        
## [PHP-app maken](app-service-web-get-started-php.md) 
## [Node.js-app maken](app-service-web-get-started-nodejs.md) 
## [Java-app maken](app-service-web-get-started-java.md)        
## [Python-app maken](app-service-web-get-started-python.md)    

# Voorbeelden
## [Azure-CLI](app-service-cli-samples.md) 
## [PowerShell](app-service-powershell-samples.md) 

# Zelfstudies
## [Functionaliteit toevoegen aan een web-app](app-service-web-get-started-2.md)
## [ASP.NET-app met SQL-database](web-sites-dotnet-get-started.md)
## [Laravel-app met MySQL](app-service-web-php-get-started.md)
## [Sails.js-app met NOSQL-database](app-service-web-nodejs-sails.md)
## [Java-app met Eclipse](app-service-web-eclipse-create-hello-world-web-app.md)
## [Java-app met IntelliJ](app-service-web-intellij-create-hello-world-web-app.md)
## [Django-app met MySQL](web-sites-python-ptvs-django-mysql.md)

# Concepten
## [De werking van App Service](../app-service/app-service-how-works-readme.md?toc=%2fazure%2fapp-service-web%2ftoc.json)    
## [App Service-abonnementen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md?toc=%2fazure%2fapp-service-web%2ftoc.json)    
## [App Service-omgevingen](app-service-app-service-environment-intro.md)
## [Verificatie en autorisatie](../app-service/app-service-authentication-overview.md?toc=%2fazure%2fapp-service-web%2ftoc.json)
## [Verificatie met on-premises AD](web-sites-authentication-authorization.md)


# Handleidingen
## Uw app ontwikkelen    
### ASP.NET

#### [Een ASP.NET Core-app ontwikkelen met VS Code](web-sites-create-web-app-using-vscode.md)
### PHP
#### [Een PHP-project opzetten](web-sites-php-configure.md)
#### [Een WordPress-multisite instellen](web-sites-php-convert-wordpress-multisite.md) 
### Node.js
#### [IO.js gebruiken](web-sites-nodejs-iojs.md)
#### [Fouten opsporen in Node.js-app](web-sites-nodejs-debug.md)
### Java

#### [Azure SDK voor Java gebruiken](java-create-azure-website-using-java-sdk.md)
#### [Bestaande app uploaden](web-sites-java-add-app.md)
#### [Externe foutopsporing voor Eclipse](app-service-web-debug-java-web-app-in-eclipse.md)
#### [Externe foutopsporing voor IntelliJ](app-service-web-debug-java-web-app-in-intellij.md)

### [E-mails versturen met SendGrid](sendgrid-dotnet-how-to-send-email.md)

### Runtime configureren    
#### [PHP op Windows](web-sites-php-configure.md)

#### [Java](web-sites-java-add-app.md)
#### [Node.js op Linux](app-service-linux-using-nodejs-pm2.md)
#### [Python](web-sites-python-configure.md)
            
### Toepassing configureren
#### [App-instellingen gebruiken](web-sites-configure.md)
            
## [Implementeren naar Azure](web-sites-deploy.md)
### [Implementeren via FTP](app-service-deploy-content-sync.md)
### [Implementeren via cloudsynchronisatie](web-sites-deploy.md)
### [Doorlopend implementeren](app-service-continuous-deployment.md)
### [Implementeren in fasering](web-sites-staged-publishing.md)
### [Implementeren vanuit lokale Git](app-service-deploy-local-git.md)
### [Implementeren met sjabloon](app-service-deploy-complex-application-predictably.md)
### [Flexibele implementatie](app-service-agile-software-development.md)
### [Bètaversie testen](app-service-web-test-in-production-controlled-test-flight.md)

### [Implementatiereferenties instellen](app-service-deployment-credentials.md)

### Aangepast domein toewijzen
#### [Domein kopen](custom-dns-web-site-buydomains-web-app.md)
#### [Domein van derde toewijzen](web-sites-custom-domain-name.md)
#### [Domeinen toewijzen met Traffic Manager](web-sites-traffic-manager-custom-domain-name.md)
#### [GoDaddy-domeinen toewijzen](web-sites-godaddy-custom-domain-name.md)
#### [Een actief domein migreren](app-service-custom-domain-name-migrate.md)

### [Migreren vanuit IIS](web-sites-migration-from-iis-server.md)
### [Testen tijdens productie](app-service-web-test-in-production-get-start.md)

## Verbinding maken met database/resources        

### [Verbinding maken met on-premises gegevens](web-sites-hybrid-connection-get-started.md) 
### [Verbinding maken met Azure VNET](web-sites-integrate-with-vnet.md)
### [Verbinding maken met Azure VNET met behulp van PowerShell](app-service-vnet-integration-powershell.md)
### [Verbinding maken met MongoDB in Azure VM](web-sites-dotnet-store-data-mongodb-vm.md)
            
##App beveiligen
### Gebruikers verifiëren        
#### [Verifiëren met Azure AD](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md?toc=%2fazure%2fapp-service-web%2ftoc.json)
#### [Verifiëren met Facebook](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md?toc=%2fazure%2fapp-service-web%2ftoc.json)
#### [Verifiëren met Google](../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md?toc=%2fazure%2fapp-service-web%2ftoc.json)
#### [Verifiëren met Microsoft-account](../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md?toc=%2fazure%2fapp-service-web%2ftoc.json)
#### [Verifiëren met Twitter](../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md?toc=%2fazure%2fapp-service-web%2ftoc.json)
#### [Verifiëren met on-premises AD](web-sites-authentication-authorization.md)
#### [App met een multitenant-database](web-sites-dotnet-entity-framework-row-level-security.md)
### Aangepaste SSL toewijzen
#### [SSL-certificaat kopen](web-sites-purchase-ssl-web-site.md)
#### [SSL-certificaat van derde configureren](web-sites-configure-ssl-certificate.md)
### [HTTPS afdwingen](web-sites-configure-ssl-certificate.md#enforce-https-on-your-app)
### [Wederzijdse TLS-verificatie configureren](app-service-web-configure-tls-mutual-auth.md)
            
##App schalen        
### [Omhoog schalen](web-sites-scale.md)
### [Uitschalen](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json)
### [Taken verdelen met Traffic Manager](web-sites-traffic-manager.md)
### [Schalen met App Service-omgevingen](../app-service/app-service-app-service-environments-readme.md?toc=%2fazure%2fapp-service-web%2ftoc.json)
### [Azure CDN gebruiken voor wereldwijd bereik](cdn-websites-with-cdn.md)
### [Verbinding maken met Redis Cache via Memcache](web-sites-connect-to-redis-using-memcache-protocol.md)
### [Een Redis-cache maken](../redis-cache/cache-redis-cache-arm-provision.md?toc=%2fazure%2fapp-service-web%2ftoc.json)
### [Sessiestatus beheren met Azure Redis Cache](web-sites-dotnet-session-state-caching.md)

## Bewaken 
### [Apps bewaken](web-sites-monitor.md)
### [Logboeken inschakelen](web-sites-enable-diagnostic-log.md)
### [Logboeken streamen](web-sites-streaming-logs-and-console.md)

## Back-up maken van inhoud        
### [Back-up maken van uw app](web-sites-backup.md)
### [Uw app terugzetten vanuit de back-up](web-sites-restore.md)
### [Back-up maken met REST-API](websites-csm-backup.md)

## App-resources beheren
### [App klonen met PowerShell](app-service-web-app-cloning.md)
### [App klonen met de portal](app-service-web-app-cloning-portal.md)
### [Resources verplaatsen](app-service-move-resources.md)
### [Azure Resource Manager gebruiken met PowerShell](app-service-web-app-azure-resource-manager-powershell.md)
### [Apps beheren met Azure Automation](automation-manage-web-app.md)



# Naslaginformatie    
## [CLI 2.0](/cli/azure/appservice)
## [PowerShell](/powershell)
## [REST API](/rest/api/appservice/) 
        
# Resources    
## Problemen oplossen        
### [Problemen met Visual Studio oplossen](web-sites-dotnet-troubleshoot-visual-studio.md)
### [Problemen met Node.js-app oplossen](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
### [Problemen met HTTP 502 en 503 oplossen](app-service-web-troubleshoot-http-502-http-503.md)
### [Prestatieproblemen oplossen](app-service-web-troubleshoot-performance-degradation.md)
## [Prijzen](https://azure.microsoft.com/pricing/details/app-service/)     
## [Quotumgegevens](../azure-subscription-service-limits.md#app-service-limits)    
## [Service-updates en release-opmerkingen](https://azure.microsoft.com/updates/?product=app-service)    
## [Aanbevolen procedures](app-service-best-practices.md)
## [Voorbeelden](https://azure.microsoft.com/resources/samples/?service=app-service)    
## [Video's](https://azure.microsoft.com/resources/videos/index/?services=app-service)
## Kookboeken    
### [Referentiearchitecturen](../guidance/guidance-ra-app-service.md)    
### [Implementatiescripts](https://azure.microsoft.com/documentation/scripts/)    
