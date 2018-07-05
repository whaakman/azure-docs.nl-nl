De service en uw abonnementtype bepalen hoeveel query’s u per seconde kunt uitvoeren (QPS). U dient ervoor te zorgen dat uw toepassing logica bevat die zorgt dat u binnen uw quotum blijft. Als u uw QPS overschrijdt, mislukt de aanvraag met HTTP-statuscode 429. Het antwoord bevat ook de Retry-After-header, die aangeeft hoeveel seconden u dient te wachten voordat u een andere aanvraag verzendt.  
  
### <a name="denial-of-service-dos-versus-throttling"></a>Denial of Service (DOS) versus beperking

De service maakt onderscheidt tussen een DOS-aanval en een QPS-overschrijding. Als de service een Denial of Service-aanval vermoedt, slaagt de aanvraag (HTTP-statuscode is 200 OK). De hoofdtekst van het antwoord is in dit geval echter leeg.