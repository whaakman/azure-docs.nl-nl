### <a name="gwipnoconnection"></a>Wijzigen van het lokale netwerk gateway IP-adres - er is geen gatewayverbinding

Gebruik het voorbeeld om een lokale netwerkgateway die geen gatewayverbinding heeft te wijzigen. Wanneer u deze waarde wijzigt, kunt u tegelijkertijd ook de adresvoorvoegsels wijzigen.

1. Op de lokale netwerkgateway-bron in de **instellingen** sectie, klikt u op **configuratie**.
2. In de **IP-adres** wijzigt u het IP-adres.
3. Klik op **opslaan** de instellingen op te slaan.

### <a name="gwipwithconnection"></a>Het lokale netwerk gateway gateway IP-adres - gatewayverbinding bestaande wijzigen

Voor het wijzigen van een lokale netwerkgateway een verbinding heeft, moet u eerst de verbinding verwijderen. Nadat de verbinding is verwijderd, kunt u het IP-adres van de gateway wijzigen en een nieuwe verbinding maken. U kunt tegelijkertijd ook de adresvoorvoegsels wijzigen. Dit veroorzaakt enige downtime in uw VPN-verbinding. Als u het IP-adres van de gateway wijzigt, hoeft u de VPN-gateway niet te verwijderen. U hoeft alleen de verbinding te verwijderen.
 
#### <a name="1-remove-the-connection"></a>1. Verwijder de verbinding.

1. Op de lokale netwerkgateway-bron in de **instellingen** sectie, klikt u op **verbindingen**.
2. Klik op de **...**  op de regel voor de verbinding en klik vervolgens op **verwijderen**.
3. Klik op **opslaan** uw instellingen op te slaan.

#### <a name="2-modify-the-ip-address"></a>2. Wijzig het IP-adres.

U kunt tegelijkertijd ook de adresvoorvoegsels wijzigen.

1. In de **IP-adres** wijzigt u het IP-adres.
2. Klik op **opslaan** de instellingen op te slaan.

#### <a name="3-recreate-the-connection"></a>3. Maak de verbinding opnieuw.

1. Navigeer naar de virtuele netwerkgateway voor uw VNet. (Niet de lokale netwerkgateway.)
2. Op de virtuele netwerkgateway in de **instellingen** sectie, klikt u op **verbindingen**.
3. Klik op de **+ toevoegen** openen de **verbinding toevoegen** blade.
4. Maak opnieuw een verbinding.
5. Klik op **OK** om de verbinding te maken.