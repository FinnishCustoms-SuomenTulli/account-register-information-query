# Tiedonhakujärjestelmän kyselyrajapintakuvaus

*Dokumentin versio 0.8*

## Versiohistoria

Versio|Päivämäärä|Kuvaus|Tekijä
---|---|---|---
0.3|26.4.2019|Alustava versio|AP,TV
0.4|9.5.2019|Sanomarakenne päivitetty alustavasti ISO 20022 mukaillen|AP|
0.5|22.5.2019|Päivityksiä standardinmukaisuuteen liittyen|AP|
0.6|31.5.2019|Tarkennuksia rajapintakuvauksen käyttöohjeistukseen|AP|
0.7|10.6.2019|Tarkennus virtuaalivaluutan tarjoajien osalta|TV|
0.8|11.9.2019|Rajapintakuvaus jaettu kahdeksi dokumentiksi|AL|

## Sisällysluettelo

1. [Johdanto](#luku1)  
  1.1 Termit ja lyhenteet  
  1.2 Dokumentin tarkoitus ja kattavuus  
  1.3 Viittaukset  
  1.4 Yleiskuvaus  
2. [Aktiviteettien kuvaus](#luku2)  
  2.1 Pankki- ja maksutilitietojen kysely
3. [Tietoturva](#tietoturva)  
  3.1 Tunnistaminen 
4. [Tiedonhakujärjestelmän kyselyrajapinta](#kyselyrajapinta)   

## 1. Johdanto <a name="luku1"></a>

### 1.1 Termit ja lyhenteet

Lyhenne tai termi|Selite
---|---
Rajapinta|Standardin mukainen käytäntö tai yhtymäkohta, joka mahdollistaa tietojen siirron laitteiden, ohjelmien tai käyttäjän välillä. 
WS (Web Service)|Verkkopalvelimessa toimiva ohjelmisto, joka tarjoaa standardoitujen internetyhteyskäytäntöjen avulla palveluja sovellusten käytettäväksi. Tiedonhakujärjestelmä tarjoaa palveluna tietojen kyselyn.
Endpoint|Rajapintapalvelu, joka on saatavilla tietyssä verkko-osoitteessa

### 1.2 Dokumentin tarkoitus ja kattavuus

Tämä dokumentti on pankki- ja maksutilien valvontajärjestelmän kyselyrajapinnan rajapintakuvaus.

### 1.3 Viittaukset

[ISO 20022 External Code Sets](https://www.iso20022.org/external_code_list.page)

[ISO 20022 auth.001.001.01 InformationRequestOpeningV01 MDR](https://www.iso20022.org/documents/general/Payments_AFI_MDR_January2013.zip)

[ISO 20022 auth.002.001.01 InformationRequestResponseV01 MDR](https://www.iso20022.org/documents/general/Payments_AFI_MDR_January2013.zip)

[ISO 20022 head.001.001.01 skeema](https://www.iso20022.org/documents/messages/head/schemas/head.001.001.01.zip)

### 1.4 Yleiskuvaus

Tulli on perustanut Tilirekisterihankkeen, joka toteuttaa (EU) 2018/843 direktiivin ja sen täytäntöön panemiseksi säädetyn, Suomen lainsäädäntöön perustuvan pankki- ja maksutilien valvontajärjestelmän. 

Tässä dokumentissa kuvataan tiedonhakujärjestelmän kyselyrajapinnat.

## 2. Aktiviteettien kuvaus <a name="luku2"></a>

Tässä luvussa on esitettu pankki- ja maksutilitietojen kysely vuokaavioina.

### 2.1 Pankki- ja maksutilitietojen kysely tiedonhakujärjestelmästä

Kuvassa 2.1 on esitetty vuokaaviona pankki- ja maksutilitietojen kysely tiedonhakujärjestelmästä.

![Pankki- ja maksutilitietojen kysely](diagrams/flowchart_query.png "Pankki- ja maksutilitietojen kysely")  
*__Kuva2.1.__ Pankki- ja maksutilitietojen kysely*  

Kuvasta nähdään, että kyselyrajapinta on asynkroninen. Tietojen kysyminen tapahtuu kahdella SOAP-sanomalla. Ensimmäisessä sanomassa lähetetään kyselyparametrit ja muut vaaditut tiedot. Vaadittavat tiedot on listattu luvussa 4. Ensimmäisen sanoman vastaussanoma sisältää kyselytunnisteen, jonka perusteella vastaus on noudettavissa määrätyn ajan kuluttua.

## <a name="tietoturva"></a> 3. Tietoturva
  
### 3.1 Tunnistaminen

Tarkentuu.

Tiedonhakujärjestelmän kyselyrajapinnan hyödyntäjät tunnistetaan VRK:n myöntämillä X.509-sertifikaateilla. Kyselyrajapinnan Sanomat allekirjoitetaan XML-allekirjoituksella. Tarkempi sanomien allekirjoitusten kuvaus lisätään tähän dokumenttiin myöhemmin.

Mahdollisuus pyyntöjen IP-avaruuden rajoittamiseen tiedonhakujärjestelmässä tarkentuu.

## <a name="kyselyrajapinta"></a> 4. Tiedonhakujärjestelmän kyselyrajapinta

Kyselyrajapinta toteutetaan SOAP/XML Web Servicenä, josta julkaistaan WSDL.

Sanomissa käytetään ISO 20022 koodistoviittauksia. Koodistoviittaukset löytyvät ISO 20022 sivulta [External Code Sets](https://www.iso20022.org/external_code_list.page).

Kyselyrajapinnassa on yksi endpoint, jonka kysely- ja vastaussanomarakenne on kuvattu tässä luvussa.

### 4.1 Kyselyrajapinnan SOAP-operaatioiden sanomarakenne

SOAP body koostuu aina kahdesta osasta, ISO 20022 Business Application Headerista (BAH) ja varsinaisesta business-sanomasta.  

### 4.2 Business Application Header (BAH) 

Business Application Header sanoman tiedot on esitetty seuraavassa taulukossa.

|Sanoma-id|Sanoman nimi|
|:---|:---|
|head.001.001.01|Business Application Header|

BAH on oltava aina SOAP bodyn ensimmäinen elementti.

### 4.3 Kyselyrajapinnan sanomat

Tiedonhakujärjestelmän kyselyrajapinnassa käytetään [ISO 20022 -sanomia InformationRequestOpeningV01 ja InformationRequestResponseV01](https://www.iso20022.org/full_catalogue.page), joihin liitetään tarvittavat [Supplementary Datat](https://www.iso20022.org/supplementary_data.page).

Seuraavassa taulukossa on listattu käytettävät ISO 20022 -sanomat. 

|Sanoma-id|Sanoman nimi|Käyttötarkoitus|Vastaava organisaatio|Msg Def Report|
|---|---|---|---|---| 
|auth.001.001.01|InformationRequestOpeningV01|Kyselyrajapinnan kyselysanoma|FFI|[MDR](https://www.iso20022.org/documents/general/Payments_AFI_MDR_January2013.zip)|
|auth.002.001.01|InformationRequestResponseV01|Kyselyrajapinnan vastaussanoma|FFI|[MDR](https://www.iso20022.org/documents/general/Payments_AFI_MDR_January2013.zip)|

Seuraavassa taulukossa on listattu kyselysanomaan liitettävä Supplementary Data -sanomalaajennus. Sanomalaajennusten tarkempi sisältö ja tietueiden käyttöohjeet on listattu luvussa 4.

|Sanoma-id|Sanoman nimi|Laajennettavan ISO 20022 sanoman id|Tarkoitus ja toiminnallisuus|
|---|---|---|---|
|FIN012|InformationRequestFIN012|auth.001.001.01|ISO 20022 sanomalaajennus. Kyselyrajapinnan toimivaltaiset viranomaiset käyttävät tätä sanomaa tietojen kyselyyn tiedonhakurajapinnasta. |

Seuraavassa taulukossa on listattu vastaussanomaan liitettävät Supplementary Data -sanomalaajennukset.

|Sanoma-id|Sanoman nimi|Laajennettavan ISO 20022 sanoman id|Tarkoitus ja toiminnallisuus|
|---|---|---|---|
|supl.027.001.01|InformationResponseSD1V01|auth.002.001.01|Sisältää hakuparametreja vastaavat tilitiedot ja tileihin liittyvät edunsaajat|
|FIN002|InformationResponseFIN002|auth.002.001.01|Sisältää hakuparametreja vastaavat tallelokeroiden ja tallelokeroiden haltijoiden tiedot.|
|FIN013|InformationResponseFIN013|auth.002.001.01|Sisältää tili- ja tallelokerotiedoista erillisenä hakuparametreja vastaavat asiakkuustiedot|

Kyselyrajapinnan sanomavastauksiin sisällytetään kaikki hakukriteereitä vastaavat sanomarakenteen sellaiset tiedot, jotka liittyvät asiakkuuksiin, jotka ovat päättyneet alle 10 vuotta sitten (tietojen säilytysaika). Tileihin ja tallelokeroihin liittyvät kaikki osallisuustiedot palautetaan, eli kyselyparametrina annetun hakuehdon mukaisten henkilöiden (oikeus- tai luonnollinen) lisäksi palautetaan myös kaikki osalliset henkilöt. Osallisten henkilöiden muita kuin kyselyn hakuparametria vastaavia tilejä ja tallelokeroita sen sijaan ei palauteta, vaan niitä koskien on tehtävä uudet kyselyt lainsäädäntöperusteineen.

Seuraavassa on listattu kyselyrajapinnan sanomat tietueineen. Taulukoita luetaan siten, että sisennetyt rivit ovat ensimmäisen rivin sisällä sanomarakenteessa.

Taulukossa pakollisuus on merkitty seuraavasti:

- **R** Pakollinen (required)
- **A** Vaihtoehtoinen (alternative), valitaan yksi useammasta vaihtoehdosta
- **O** Vapaaehtoinen (optional)

### <a name="BusinessApplicationHeaderV01"></a> 4.4 BusinessApplicationHeaderV01

Seuraavassa taulukossa on esitetty BAH-elementtien käyttö. Elementtien tyypit on kuvattu [head.001.001.01-skeemassa](https://www.iso20022.org/documents/messages/head/schemas/head.001.001.01.zip).

|Nimi|Tyyppi|Käytössä|Kuvaus|
|:---|:---|:---|:---|
|BusinessApplicationHeaderV01| | | |
|CharSet|UnicodeChartsCode|kyllä|"UTF-8"|
|Fr|Party9Choice|kyllä|Käytetään seuraavasti: Elementti `Fr/OrgId/Id/OrgId/Othr/SchmeNm/Cd` sisältää arvon "COID" ja elementti `Fr/OrgId/Id/OrgId/Othr/Id` sisältää lähettäjän Y-tunnuksen.|
|To|Party9Choice|kyllä|Käytetään seuraavasti: Elementti `To/OrgId/Id/OrgId/Othr/SchmeNm/Cd` sisältää arvon "COID" ja elementti `To/OrgId/Id/OrgId/Othr/Id` sisältää vastaanottajan Y-tunnuksen (Esim. Tiedonhakujärjestelmässä Y-tunnus 0245442-8)|
|BizMsgIdr|Max35Text|kyllä|Käyttö standardin mukaisesti.|
|MsgDefIdr|Max35Text|kyllä|Sisältää sanoma-id:n. Kyselysanomassa käytetään "auth.001.001.01", vastaussanomassa sisältönä on "auth.002.001.01"|
|BizSvc||ei||
|CreDt|ISONormalisedDateTime|kyllä|BAH:n luomis päivämäärä ja aika. Normalisoitava Z-notaatiolla (UTC).|
|CpyDplct||ei| |
|PssblDplct| |ei| |
|Prty| |ei| |
|Sgntr| |kyllä|Business messagen lähettäjän XMl-allekirjoitus. Tarkentuu.|
|Rltd|BusinessApplicationHeader1|kyllä|Käytössä vastaussanomassa, sisältää kyselysanoman sisältämän BAH:in.|

### <a name="InformationRequestOpeningV01"></a> 4.5 InformationRequestOpeningV01

Taulukossa on kuvattu sanoman tietueiden käyttö.

|Nimi|Tyyppi|Käytössä|Kuvaus|
|:---|:---|:---|:---|
|InformationRequestOpeningV01| | | |
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnId|Max35Text|Kyllä|Tutkinnan case id|
|&nbsp;&nbsp;&nbsp;&nbsp;LglMndtBsis|LegalMandate1|Kyllä|Lainsäädäntöperuste. Arvojoukko tarkentuu.|
|&nbsp;&nbsp;&nbsp;&nbsp;CnfdtltySts|YesNoIndicator|Kyllä|Aina "true"|
|&nbsp;&nbsp;&nbsp;&nbsp;DueDt|DueDate1|Ei||
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnPrd|DateOrDateTimePeriodChoice|Ei|Ei huomioida. Skeemassa pakollinen, joten `/Document/InfReqOpng/InvstgtnPrd/Dt/FrDt` ja `ToDt` esim. kuluva pvm|
|&nbsp;&nbsp;&nbsp;&nbsp;SchCrit|SearchCriteria1Choice|Kyllä|Hakukriteeri. Käytettävä aina mahdollisimman täsmällistä hakukriteeriä. Esim. jos ei käytetä Y-tunnusta, vaan käytetään OtherOrganisationIdentification -kenttää, haku ei kohdistu Y-tunnuksiin lainkaan. Ks. [tarkempi erittely](#SearchCriteria1Choice) alla.|
|&nbsp;&nbsp;&nbsp;&nbsp;SplmtryData|SupplementaryData1|Kyllä|Sisältää sanomalaajennuksen [InformationRequestFIN012](#InformationRequestFIN012)|

#### <a name="SearchCriteria1Choice"></a> SearchCriteria1Choice

Hakukriteerien esittäminen SearchCriteria1Choice avulla.

|Hakukriteeri|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|:---|
|Hetu tai henkilötodistuksen tunnistenumero|\<Id\>|CstmrId/Pty/Id/Prvtld/Othr|Hetu tai henkilötodistuksen tunnistenumero|
||\<Cd\>|CstmrId/Pty/Id/Prvtld/Othr/SchemeNm|SSN, PIC (Person Identification Code), OTHER*|

|Hakukriteeri|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|:---|
|Y-tunnus tai muu oikeushenkilön tunniste|\<Id\>|CstmrId/Pty/Id/OrgId/Othr|Y-tunnus tai muu oikeushenkilön tunniste|
||\<Cd\>|CstmrId/Pty/Id/OrgId/Othr/SchemeNm|Y-tunnus, PRH, NAMESRCH, OTHER*|

|Hakukriteeri|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|:---|
|IBAN|\<IBAN\>|Acct/Id/Id||

|Hakukriteeri|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|:---|
|Tilin muu yksilöintitunnus|\<Id\>|Acct/Id/Id/Othr|Tunnus|
||\<Cd\>|Acct/Id/Id/Othr/SchemeNm|OTHER*|

*) OTHER arvojoukko kuvataan erillisessä taulukossa, Tulli lähettää taulukot Pankki- ja maksutilirekisterin
osalta yhteisen taulukon ylläpitäjälle

|Hakukriteeri|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|:---|
|Nimi+Kansalaisuus+syntymäaika|\<Nm\>|CstmrId/Pty|Nimi|
||\<Id\>|CstmrId/Pty/Id/PrvtId/Othr|Maakoodi|
||\<Cd\>|CstmrId/Pty/Id/PrvtId/Othr/SchemeNm|NATIONALITY|
||\<BirthDt\>|CstmrId/Pty/Id/PrvtId/DtAndPlcOfBirth **||

**) Document/InfReqOpng/SchCrit/CstmrId/Pty/Id/PrvtId/DtAndPlcOfBirth/CtryOfBirth ja
Document/InfReqOpng/SchCrit/CstmrId/Pty/Id/PrvtId/DtAndPlcOfBirth/CityOfBirth arvoksi asetetaan ”not
in use”

|Hakukriteeri|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|:---|
|Yrityksen nimi|\<Nm\>|CstmrId/Pty|Yrityksen nimi***|

***) Yrityksen nimellä haettaessa on aina annettava Y-tunnus tai muu oikeushenkilön tunniste. Kun
tehdään pelkkä nimihaku, annetaan arvo 1 ja koodi NAMESRCH.

### <a name="InformationRequestFIN012"></a> 4.6 Sanomalaajennus InformationRequestFIN012

Sanomalaajennus liitetään taulukossa listattuun ISO 20022 sanoman XPath-sijaintiin.

|Nimi|Pakollisuus (RAO)|[min..max]|Tyyppi|Kuvaus|Liitetään sanomaan|XPath|
|:---|:---|:---|:---|:---|:---|:---|
|InformationRequestFIN012| | | | |[auth.001](InformationRequestOpeningV01)|`/Document/InfReqOpng/SplmtryData/Envlp`|
|&nbsp;&nbsp;&nbsp;&nbsp;AuthorityInquiry|R|[1..1]|[AuthorityInquirySet](#AuthorityInquirySet)|Kyselyyn liittyvät viranomaisen tiedot| |
|&nbsp;&nbsp;&nbsp;&nbsp;AdditionalSearchCriteria|R|[1..*]|[SearchCriteriaChoice](#SearchCriteriaChoice)|Jos tarvitaan esimerkiksi tallelokero hakukriteeriksi, voidaan käyttää tätä elementtiä.| |

#### <a name="AuthorityInquirySet"></a> AuthorityInquirySet

|Nimi|Pakollisuus (RAO)|[min..max]|Tyyppi|Kuvaus|
|:---|:---|:---|:---|:---|
|AuthorityInquirySet| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;OfficialId|R|[0..1]|Max140Text|Viranomaisen (henkilön) tunniste|
|&nbsp;&nbsp;&nbsp;&nbsp;OfficialSuperiorId|R|[0..1]|Max140Text|Esimiehen tunniste|

### <a name="InformationRequestResponseV01"></a> 4.7 InformationRequestResponseV01

Taulukossa on kuvattu sanoman tietueiden käyttö.

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|InformationRequestResponseV01| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;RspnId|Max35Text|Kyllä|[1..1]|Vastaussanoman id|
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnId|Max35Text|Kyllä|[1..1]|Kyselysanomassa toimitettu Case Id|
|&nbsp;&nbsp;&nbsp;&nbsp;RspnSts|StatusResponse1Code|Kyllä|[1..1]|Vastaussanoman status|
|&nbsp;&nbsp;&nbsp;&nbsp;RtrInd|ReturnIndicator1|Kyllä|[3..3]|Vastaussanoman payload. `RtrInd/AuthrtyReqTp/MsgNmId` sisältää sanomalaajennuksen sanmoma-id:n, ja `RtrInd/InvstgtnRslt/Rslt`, sisältää itse sanomalaajennuksen [supl.027.001.01](#supl.027.001.01), [InformationResponseFIN002](#InformationResponseFIN002) tai [InformationResponseFIN013](#InformationResponseFIN013). Sääntö: Jokainen sanomalaajennus esiintyy yhden kerran.|
|&nbsp;&nbsp;&nbsp;&nbsp;SplmtryData|SupplementaryData1|Ei|[0..0]|-|

### <a name="InformationResponseSD1V01"></a> 4.8 InformationResponseSD1V01 supl.027.001.01

Taulukossa on kuvattu sanoman tietueiden käyttö.

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|InformationResponseSD1V01 supl.027.001.01| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnId|Max35Text|kyllä|[1..1]|Tutkinnan case-id|
|&nbsp;&nbsp;&nbsp;&nbsp;CreDtTm|ISODateTime|kyllä|[1..1]|Sanoman luomisaika|
|&nbsp;&nbsp;&nbsp;&nbsp;AcctSvcrId|BranchAndFinancialInstitutionIdentification4|kyllä|[1..1]|Käytetään seuraavasti: Elementti `AcctSvcrId/FinInstnId/Othr/SchmeNm/Cd` sisältää arvon "COID" ja elementti `AcctSvcrId/FinInstnId/Othr/Id` sisältää lähettäjän Y-tunnuksen.|
|&nbsp;&nbsp;&nbsp;&nbsp;AcctAndPties|AccountAndParties2|kyllä|[1..*]|Ks. taulukko alla|

#### AccountAndParties2 käyttö

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|AccountAndParties2| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;Acct|CustomerAccount1|kyllä|[1..*]|Tilin tiedot ks. CustomerAccount1 käyttö| 
|&nbsp;&nbsp;&nbsp;&nbsp;Role|AccountRole1|kyllä|[1..*]|Tiliin liittyvät roolit ks. toinen taulukko alla|
|&nbsp;&nbsp;&nbsp;&nbsp;AddtlInf|Max256Text|kyllä|[1..1]|Tilin avaamispäivämäärä merkkijonona ISODate-formaatissa|

#### CustomerAccount1 käyttö

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|CustomerAccount1| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;Id|AccountIdentification4Choice|kyllä|[1..1]|ks. [Id-elementin käyttö](#Id-elementin_kaytto)|
|&nbsp;&nbsp;&nbsp;&nbsp;Nm||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;Sts||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;Tp||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;Ccy||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;MnthlyPmtVal||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;MnthlyRcvdVal||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;MnthlyTxNb||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;AvrgBal||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;AcctPurp||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;FlrNtfctnAmt||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;ClngNtfctnAmt||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;StmtCycl||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;ClsgDt|ISODate|kyllä|[0..1]|Tilin sulkemispäivämäärä|
|&nbsp;&nbsp;&nbsp;&nbsp;Rstrctn||ei|||

#### AccountRole1 käyttö

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|AccountRole1| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;Pty|PartyIdentification41|[1..*]|ks. [Id-elementin käyttö](#Id-elementin_kaytto)|
|&nbsp;&nbsp;&nbsp;&nbsp;OwnrTp|OwnerType1|[1..1]|Käytetään `OwnrTp/Prtry/SchmeNm` arvolla "ROLETYPE", sekä `OwnrTp/Prtry/Id`, jossa arvot "POWN" (omistaja), "ACCE" (käyttöoikeus) tai "BENE" (edunsaaja)|
|&nbsp;&nbsp;&nbsp;&nbsp;StartDt|ISODate|[1..1]|Roolin alkamispäivämäärä|
|&nbsp;&nbsp;&nbsp;&nbsp;EndDt|ISODate|[0..1]|Roolin päättymispäivämäärä|

### <a name="InformationResponseFIN002"></a> 4.9 InformationResponseFIN002

Sanomalaajennus liitetään taulukossa listattuun ISO 20022 sanoman XPath-sijaintiin.

|Nimi|Pakollisuus (RAO)|[min..max]|Tyyppi|Index|Liitetään sanomaan|XPath|
|:---|:---|:---|:---|:---|:---|:---|
|InformationResponseFIN002| | | | |[auth.002](InformationRequestResponseV01)|/Document/InfReqRspn/RtrInd/InvstgtnRslt/Rslt|
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnId|Max35Text|kyllä|[1..1]|Tutkinnan case-id|
|&nbsp;&nbsp;&nbsp;&nbsp;CreDtTm|ISODateTime|kyllä|[1..1]|Sanoman luomisaika|
|&nbsp;&nbsp;&nbsp;&nbsp;SvcrId|BranchAndFinancialInstitutionIdentification4|kyllä|[1..1]|Käytetään seuraavasti: Elementti `SvcrId/FinInstnId/Othr/SchmeNm/Cd` sisältää arvon "COID" ja elementti `SvcrId/FinInstnId/Othr/Id` sisältää lähettäjän Y-tunnuksen.|
|&nbsp;&nbsp;&nbsp;&nbsp;SdBoxAndPties|O|[0..*]|SafetyDepositBoxAndParties|Tallelokero ja osalliset|


### <a name="InformationResponseFIN013"></a> 4.10 InformationResponseFIN013

Sanomalaajennus liitetään taulukossa listattuun ISO 20022 sanoman XPath-sijaintiin.

|Nimi|Pakollisuus (RAO)|[min..max]|Tyyppi|Index|Liitetään sanomaan|XPath|
|:---|:---|:---|:---|:---|:---|:---|
|InformationResponseFIN013| | | | |[auth.002](InformationRequestResponseV01)|/Document/InfReqRspn/RtrInd/InvstgtnRslt/Rslt|
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnId|Max35Text|kyllä|[1..1]|Tutkinnan case-id|
|&nbsp;&nbsp;&nbsp;&nbsp;CreDtTm|ISODateTime|kyllä|[1..1]|Sanoman luomisaika|
|&nbsp;&nbsp;&nbsp;&nbsp;SvcrId|BranchAndFinancialInstitutionIdentification4|kyllä|[1..1]|Käytetään seuraavasti: Elementti `SvcrId/FinInstnId/Othr/SchmeNm/Cd` sisältää arvon "COID" ja elementti `SvcrId/FinInstnId/Othr/Id` sisältää lähettäjän Y-tunnuksen.|
|&nbsp;&nbsp;&nbsp;&nbsp;Customer|O|[0..*]|Customer|Asiakas. Luonnollinen henkilö tai yritys. Ks. Customer-elementin käyttö taulukko alla|

#### Customer-elementin käyttö

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|Customer| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;Contract|Contract|kyllä|[1..1]| |
|&nbsp;&nbsp;&nbsp;&nbsp;Id|PartyIdentification41|kyllä|[1..1]|Ks. [Id-elementin käyttö](#Id-elementin_kaytto)|
|&nbsp;&nbsp;&nbsp;&nbsp;Beneficiaries|Beneficiaries|kyllä|[0..1]|Kun asiakas on oikeushenkilö, palautetaan myös kaikki edunsaajat.|

#### Beneficiaries käyttö

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|Beneficaries| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;lastName|string|kyllä|[1..1]|sukunimi|
|&nbsp;&nbsp;&nbsp;&nbsp;firstNames|string|kyllä|[1..1]|etunimet|
|&nbsp;&nbsp;&nbsp;&nbsp;hetu|string|kyllä|[0..1]|suomalainen henkilötunnus|
|&nbsp;&nbsp;&nbsp;&nbsp;birthdate|date-time|kyllä|[0..1]|syntymäaika|
|&nbsp;&nbsp;&nbsp;&nbsp;nationality|string|kyllä|[0..1]|kansalaisuus|

### <a name="Id-elementin_kaytto"></a> 4.11 Id-elementin käyttö

Kaikissa sanomissa käytetään vastaavaa oikeushenkilön ja luonnollisen henkilön tunnistamisrakennetta Id-elementin (Party8Choice) alla. Tässä on kuvattu Id-elementin käyttö kyselyrajapinnassa.

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|Id|Party8Choice|kyllä|[1..1]| |

#### Party8Choice

|Nimi|Pakollisuus (RAO)|Tyyppi|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|Party8Choice|| | | |
|&nbsp;&nbsp;&nbsp;&nbsp;OrgId|A||[0..1]|OrganisationIdentification6|Käytetään seuraavasti: Elementti `OrgId/Othr/SchmeNm/Prtry` sisältää organisaatiotunnuksen tyyppikoodin ja elementti `OrgId/Id/OrgId/Othr/Id` sisältää tunnuksen. Ks. koodit taulukko alla.|
|&nbsp;&nbsp;&nbsp;&nbsp;PrvtId|A|PersonIdentification5|[0..1]|Käytetään seuraavasti: Elementti `PrvtId/Othr/SchmeNm/Prtry` sisältää henkilötunnisteen tyyppikoodin. Elementti `PrvtId/Othr/Id` sisältää tunnuksen. Ks. koodit taulukko alla.|

OrgId koodit  

|Koodi|Kuvaus|
|:---|:---|
|YTUN|Y-tunnus|
|PRHY|Yhdistysrekisterinumero|
|OTHR|Muu yritystunnus, tässä tapauksessa annettava lisäksi maakoodi elementissä `OrgId/Othr/Issr`|

PrvtId koodit  

|Koodi|Kuvaus|
|:---|:---|
|HETU|Suomalainen henkilötunnus|
|PIDE|Muu henkilön tunnistusasiakirjan id|

### 4.12 Kyselyrajapinnan WS-sanomaliikenteen skenaariot

Tässä luvussa on esitelty kyselyrajapinnan WS-sanomaliikenteen skenaariot. 

#### Skenaario 1 - OK

| | |
|---|---|  
Kuvaus|Sanoma käsiteltiin kokonaisuudessaan onnistuneesti.  
HTTP status code|202
Seuraamus|-|

#### Skenaario 3 - Virheellinen kysely

| | |
|---|---|
|Kuvaus|Kyselysanoma on virheellinen|
|HTTP status code|400|
|Seuraamus|Lähetä korjattu sanoma|

#### Skenaario 4 - Tekninen virhe

| | |
|---|---|
|Kuvaus|Tapahtui tekninen virhe|
|HTTP status code|500|
|Seuraamus|Viestiä ei ole käsitelty. Vastaussanoma sisältää virheen kuvauksen.|
