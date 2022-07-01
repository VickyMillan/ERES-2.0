# ERES-2.0
Document d'Integració del servei ERES 2.0

# 1.Introducció

Per millorar la integració de la aplicació Genesys amb altres entorns, s’ha desenvolupat una API amb arquitectura REST.

## 1.1	Objectius

Aquest document té com objectiu proporcionar una guia tècnica completa per la utilització de la API REST de l’aplicació Genesys de Audifilm Consulting.
La documentació inclou la descripció detallada de totes les operacions així com exemples d’us que facilitin la integració.

## 1.2	Audiència

El present document va dirigit a equips o personal tècnic, encarregat de realitzar integracions amb l’aplicació Genesys de Audifilm Consulting.
Requeriments previs:
* Coneixement de l’arquitectura REST
* Estructures de dades en format JSON
* Coneixement funcional de l’aplicació Genesys.


# 2. Consideracions generals

La API s’ha construit seguint la arquitectura REST en la mesura del possible.

En els casos en que les peticions de dades requerien el pas d’estructures complexes de paràmetres, s’ha optat per definir-les amb la operació POST en lloc de GET. Això ens permet passar una estructura JSON complexa en el cos de la petició.

Les respostes correctes de la API s’indiquen amb un retorn HTTP 200.

Els error funcionals s’indiquen amb un HTTP 404 (Not Found) indicant el motiu de l’error.

Si es produeix algun altre tipus d’incidència, pot ser deguda a altres causes i retornar algun dels codis HTTP estàndards.

## 2.1 Swagger

La API ofereix una documentació interactiva en format Swagger v2 accessible per web. La web permet consumir directament la API des de la pròpia web i obtenir la seva definició en format Swagger.

Aquesta definició es pot utilitzar per generar automàticament les crides en clients com Postman o utilitzar generadors automàtics de codi.

Punts d’accés a la API:

| Paràmetre | Descripció |
| --------- | ---------- |
| *URL*    | http://xxxx      |
| *Endpoint*    | /JGenesysREST/api        |
| *Swagger UI*    | /JGenesysREST/api/swagger-ui.html        |
| *Swagger doc*    | /JGenesysREST/api/v2/api-docs        |


## 2.2 Dades generals

Hi ha tres paràmetres que solen enviar-se en totes les peticions:

| Paràmetre | Descripció | Proves de test |
| --------- | ---------- | -------------- |
| nivell | Indica el nivell de seguretat | Podem utilitzar 9999 |
| aplicacio | Indica l’aplicació a la que accedim | Per exemple: SDE - Seguiment D’Esxpedients RES - Registre d’Entrada i Sortida |
| usuari | Identificador de l’usuari |	Podem utilitzar G5Admin o qualsevol usuari de l’aplicació |
 


# 3. Recursos de la API

Els recursos que ofereix la API són:
* Carrers
* Domicilis
* Persones
* Persona-Domicili
* Registre
* Registre d’Entrada
* Registre de Sortida
* Expedients
* Documents

A continuació els descrivim en detall.

## 3.1 Carrers

* ***Descripció*** - Permet guardar i obtenir informació de carrers del territoris. També podem obtenir dades genèriques com partícules dels carrers o sigles

* ***Ruta*** -  /carrers 

### 3.1.1 Operacions

| Mètode | Ruta | Descripció |
| ------ | ---- | ---------- |
|POST |	/put			|Alta o modificació de Carrers.              |
|POST |	/get			|Recuperar un Carrer.                        |
|POST |	/search			|Cerca de Carrers per diferents criteris.    |
|POST |	/particules/put	|Modificació de partícules dels Carrers.     |
|POST |	/particules/get	|Recuperar partícules dels Carrers.          |
|POST |	/sigles/put		|Modificació de sigles dels Carrers.         |
|POST |	/sigles/get		|Recuperar sigles dels Carrers.              |

### 3.1.2 Alta o modificació

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/carrers/put |		

***Cos de la petició***:

```json
{
"nivell": "string",
"aplicacio": "string",
"usuari": "string",
"carrer": {
"baixa": { 
	"dataBaixa": "string",
	"esBaixa": "string"
 },
 "codiCarrer": 0,
 "codiParticula": "string",
 "codiSigla": "string",
 "municipi": { "codMunicipi": 0,
 "codPais": 0,
 "codProvincia": 0
 },
 "nom": "string",
 "nomAbreujat": "string"
 }
}
```

|Paràmetres| |
|----------|-|
|carrer.baixa			| Dades de baixa del Carrer.                                                         |
|carrer.codiCarrer		| Codi del Carrer.                                                                   |
|carrer.codiParticula	| Codi de Partícula del Carrer.                                                      |
|carrer.codiSigla		| Sigla del Carrer.                                                                  |
|carrer.municipi		| Municipi del Carrer indicant codi de país, codi de provícia i codi de municipi.    |
|carrer.nom				| Nom del Carrer.                                                                    |
|carrer.nomAbreujat		| Nom abreujaT del Carrer.                                                           |

***Exemple:***

```json
Request:
POST /carrers/put

Request body:
{
"carrer": { 
	"codiCarrer": 3, 
	"codiSigla": "C/",
	"municipi": { 
		"codMunicipi": 79,
		"codPais": 108,
		"codProvincia": 17
		},
	"nom": "MIGDIA PROVA",
	"nomAbreujat": "MP"
	},
"nivell": "9999",
"aplicacio": "NCL",
"usuari": "G5Admin"
}


Expected response:
{
"nom": "MIGDIA PROVA",
"codiCarrer": 3,
"baixa": { 
	"dataBaixa": "",
	"esBaixa": ""
	},
"municipi": { 
	"codPais": 108,
	"codProvincia": 17,
	"descPais": "ESPAÑA",
	"descProvincia": "GIRONA",
	"baixa": null,
	"codMunicipi": 79,
	"descMunicipi": "GIRONA"
	},
"codiSigla": "C/",
"codiParticula": "",
"nomAbreujat": "MP",
"descSigla": "C/",
"descParticula": ""
```

### 3.1.3 Recuperar un Carrer

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/carrers/get |		

***Cos de la petició***:

```json
Cos de la petició
{
"nivell": "string",
"aplicacio": "string",
"usuari": "string",
"carrer": { 
	"codCarrer": 0,
	"codMunicipi": 0,
	"codPais": 0,
	"codProvincia": 0
	}
}
```
|Paràmetres| |
|----------|-|
|carrer.codCarrer	|Codi del Carrer.              |
|carrer.codMunicipi	|Codi del Municipi.            |
|carrer.codPais		|Codi del País.                |
|carrer.codiProvincia|Codi de provincia.           |

***Exemple:***
```json
Request:
POST /carrers/get


Request body:
{
"carrer": { "codCarrer": 616,
"codMunicipi": 19,
"codPais": 108,
"codProvincia": 8
},
"nivell": "9999",
"aplicacio": "NCL", "usuari": "G5Admin"
}


Expected response body:
{
"nom": "PUIGMARTI",
"codiCarrer": 616, 
"baixa": { 
	"dataBaixa": "",
	"esBaixa": ""
	},
"municipi": { 
	"codPais": 108,
	"codProvincia": 8,
	"descPais": "ESPAÑA",
	"descProvincia": "BARCELONA",
	"baixa": null,
	"codMunicipi": 19,
	"descMunicipi": "BARCELONA"
	},
"codiSigla": "C/",
"codiParticula": "",
"nomAbreujat": "",
"descSigla": "C/",
"descParticula": ""
}

```


### 3.1.4 Cercar Carrers

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/carrers/search |

***Cos de la petició***:

```json
{
"codiCarrer": 0,
"maxResults": 0,
"municipi": {
	"codMunicipi": 0,
	"codPais": 0,
	"codProvincia": 0
	},
"nomCarrer": "string",
"tipusVia": "string", 
"nivell": "string", 
"aplicacio": "string", 
"usuari": "string"
}
```

|Paràmetres| |
|----------|-|
|codiCarrer	|Codi del Carrer.                                                                           |
|maxResults	|Número màxim de resultats retornats.                                                       |
|municipi	|Identificador de municipi. Inclou codi de país, codi de provincia i codi de municipi.      |
|nomCarrer	|Nom del carrer.                                                                            |
|tipusVia	|Tipus de via.                                                                              |



***Exemple:***

```json
Request:
POST /carrerc/search

Request body:

{
"maxResults": 1,
"municipi": {
	"codMunicipi": 79,
	"codPais": 108,
	"codProvincia": 17
	},
"nomCarrer": "MUNTAN",
"nivell": "9999",
"aplicacio": "NCL",
"usuari": "G5Admin"
}

Expected response body:
{
"carrerArray": [
{
"nom": "MUNTANYA",
"codiCarrer": 234,
"baixa": { 
	"dataBaixa": "",
	"esBaixa": ""
	},
"municipi": { 
	"codPais": 108,
	"codProvincia": 17, 
	"descPais": "ESPAÑA", 
	"descProvincia": "GIRONA", 
	"baixa": null, 
	"codMunicipi": 79, 
	"descMunicipi": "GIRONA"
	},
"codiSigla": "C/",
"codiParticula": "",
"nomAbreujat": "",
"descSigla": "C/", "descParticula": ""
},
....
]
}

```

### 3.1.5 Modificar Partícules dels Carrers

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/carrers/particules/put |

***Cos de la petició***:

```json
{
"nivell": "string", 
"aplicacio": "string",
"usuari": "string",
"particulaCarrer": { 
	"codi1": "string",
	"codi2": "string", 
	"comptador": 0, 
	"descripcio": "string", 
	"descripcio2": "string", 
	"qual": "string"
	}
}
```

|Paràmetres| |
|----------|-|
|codi1	           |Codi de la partícula|
|codi2	           ||
|comptador	       ||
|descripcio	       |Descripció de la partícula|
|descripcio2        ||	
|qual	||


***Exemple:***

```json
Request:
POST /carrers/particules/put


Request body:
{
"particulaCarrer":{
	"codi1": "A",
	"codi2": "A",
	"descripcio": "Partícula A"
},
"nivell": "9999",
"aplicacio": "NCL", 
"usuari": "G5Admin"
}

Expected response:
{
"descripcio": "Partícula A",
"descripcio2": "",
"codi1": "A",
"codi2": "A",
"qual": "",
"comptador": null
}
```


### 3.1.6 Recuperar Partícules dels Carrers

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/carrers/particules/get |		

***Cos de la petició***:

```json
{
"nivell": "string",
"aplicacio": "string",
"usuari": "string"
}
```` 

***Exemple:***

```json
Request:
POST /carrers/particules/get


Request body:
{
"nivell": "9999",
"aplicacio": "NCL",
"usuari": "G5Admin"
}


Expected response body:
{
	"particulaCarrerArray": [
	{
	"descripcio": "Partícula A",
	"descripcio2": "",
	"codi1": "A",
	"codi2": "A",
	"qual": "",
	"comptador": null
	}
	]
}
```


### 3.1.7 Modificar Sigles dels Carrers

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/carrers/sigles/put |	
 

***Cos de la petició***:

```json
{
"nivell": "string",
"aplicacio": "string",
"usuari": "string",
"sigla": {
	"baixa": {
		"dataBaixa": "string", 
		"esBaixa": "string"
		},
	"codiSigla": "string",
	"descAbreujada": "string",
	"descripcio": "string"
	}
}
```


|Paràmetres| |
|----------|-|
|baixa			|Informació de la baixa del registre.     |
|codiSigla		|Codi de Sigla.                           |
|descAbreujada	|Descripció abreujada.                    |
|descripcio		|Descripció completa.                     |

***Exemple:***

```json
Request:
POST /carrers/sigles/put


Request body:
{
"sigla": { 
	"codiSigla": "XX",
	"descAbreujada": "XX",
	"descripcio": "SIGLA XX"
	},
"nivell": "9999",
"aplicacio": "NCL",
"usuari": "G5Admin"
}


Expected response:
{
"descripcio": "SIGLA XX",
"baixa": null,
"codiSigla": "XX",
"descAbreujada": "XX"
}

```
### 3.1.8 Recuperar Sigles dels Carrers

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/carrers/sigles/get |


***Cos de la petició***:

```json
{
"nivell": "string", 
"aplicacio": "string",
"usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /carrers/sigles/get

Request body:
{
"nivell": "9999",
"aplicacio": "NCL",
"usuari": "G5Admin"
}

Expected response:
{
"siglaArray": [
	{
	"descripcio": "L",
	"baixa": { 
		"dataBaixa": "",
		"esBaixa": "0"
		},
	"codiSigla": "L", 
	"descAbreujada": "L"
	},
{
"descripcio": "-", 
"baixa": { 
	"dataBaixa": "",
	"esBaixa": ""
	},
"codiSigla": "-",
"descAbreujada": "-"
},
....
]
}
```
## 3.2	Domicilis

* ***Descripció*** - Permet guardar i obtenir informació de domicilis del territoris. També podem obtenir dades genèriques com tipus de domicilis o tipus de locals. I finalment permet obtenir les persones d’un domicili.

* ***Ruta*** -  /domicilis

### 3.2.1 Operacions

| Mètode | Ruta | Descripció |
| ------ | ---- | ---------- |
|POST|	/put				|Alta d’un Domicili.                                   |
|POST|	/get				|Recuperar un Domicili.                                |
|POST|	/search				|Cerca de Domicilis per diferents criteris.            |
|POST|	/persones/get		|Recuperar les Persones d’un Domicili.                 |
|POST|	/getperterritori	|Recuperar Domicilis d’un Territori.                   |
|POST|	/tipus/get			|Recuperar els Tipus de Domicilis.                     |
|POST|	/tipuslocals/get	|Recuperar els Tipus de Locals                         |


### 3.2.2 Alta

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/domicilis/put |		

***Cos de la petició***:

```json
{
"nivell": "string",
"aplicacio": "string",
"usuari": "string",
"ambKm": true,
"domTip": [ 
"string"
],
"domTloc": [
 "string"
],
"domicili": { 
	"apartatCorreus": 0, 
	"baixa": {
		"dataBaixa": "string",
		"esBaixa": "string"
		},
	"bis1": "string",
	"bis2": "string",
	"bloc": "string", "codiCarrer": 0,
	"codiDomicili": 0, 
	"codiGIS": "string", 
	"codiPostal": "string", 
	"codiPseudovia": 0,
	"codiTipusDomicili": "string",
	"codiTipusLocal": "string",
	"escala": "string",
	"hm": 0,
	"km": 0,
	"municipi": { 
		"codMunicipi": 0,
		"codPais": 0,
		"codProvincia": 0
		},
	"num1": "string",
	"num2": "string", 
	"observacions": "string", 
	"pis": "string", 
	"pobldesc": "string", 
	"porta": "string",
	"portal": "string",
	"refCadastral": "string"
	},
"duplicarAcceptat": true,
"g_G5Municod": 0,
"g_G5Paiscod": 0,
"g_G5Provcod": 0,
"maxResults": 0, 
"nomesBaixa": true, 
"senseAproximadors": true, 
"senseNumero": true
}
```

|Paràmetres| |
|----------|-|
|ambKm	            |                                                                  |
|domTip	            |                                                                  |
|domTLoc	            |                                                                  |
|apartatCorreus		|Apartat de Correus.                                               |
|baixa				|Dades de baixa del Domicili.                                      |
|bis1	            |                                                                  |
|bis2	            |                                                                  |
|bloc				|Número de bloc.                                                   |
|codiCarrer			|Codi del Carrer.                                                  |
|codiDomicili		|Codi del Domicili.                                                |
|codiGIS	            |                                                                  |
|codiPostal			|Codi Postal.                                                      |
|codiPseudovia	    |                                                                  |
|codiTipusdomicili	|Tipus de Domicili.                                                |
|codiTipusLocal		|Tipus de Local.                                                   |
|escala	Escala.     |                                                                  |
|hm	                |                                                                  |
|km					|Kilòmetre per vies no numerades.                                  |
|municipi			|Dades del Municipi. Inclou país, província i municipi.            |
|num1				|Número 1 del Domicili.                                            |
|num2				|Número 2 del Domicili.                                            |
|observacions		|Observacions vàries.                                              |
|pis	Pis.            |                                                                  |
|pobledesc	        |                                                                  |
|porta				|Porta.                                                            |
|portal				|Número de portal.                                                 |
|refCadastral		|Referència Cadastral de l’immoble.                                |
|duplicarAcceptat	|                                                                  |
|g_G5Municod	        |                                                                  |
|g_G5Paiscod	        |                                                                  |
|g_G5provcod	        |                                                                  |
|maxResults	        |                                                                  |
|nomesBaixa	        |                                                                  |
|senseAproximadors	|                                                                  |
|senseNumero			|Indicador de Domicili sense número.                               |


***Exemple:***

```json
Request:
POST /domicilis/put

Request body:

Expected response body:

```


### 3.2.3 Recuperar un Domicili

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/domicilis/get |

***Cos de la petició***:

```json
{
"nivell": "string",
"aplicacio": "string",
"usuari": "string",
"codiDomicili": 0,
"codiMunicipi": 0,
"codiPais": 0,
"codiProvincia": 0
}

```

|Paràmetres| |
|----------|-|
|codiDomicili	|Codi delDomicili.             |
|codiMunicipi	|Codi del municipi.            |
|codiPais		|Codi del País.                |
|codiProvincia	|Codi de la Província.         |

***Exemple:***

```json
 Request:
POST /domicilis/get


Request body:
{
"codiDomicili": 555,
"nivell": "9999",
"aplicacio": "NCL",
"usuari": "G5Admin"
}

Expected response body:
{
"codiDomicili": 555,
"codiCarrer": 74,
"codiPseudovia": null,
"num1": "0138",
"num2": "",
"bis1": "",
"bis2": "",
"km": null,
"hm": null,
"bloc": "",
"portal": "",
"escala": "",
"pis": "",
"porta": "",
"descripcio": "C/ CREU N.0138 CP 17220 SANT FELIU DE GUIXOLS (GIRONA) - ESPAÑA",
"baixa": { "dataBaixa": "",
"esBaixa": "0"
},
"baixaRelacio": null, "municipi": {
	"codPais": 108,
	"codProvincia": 17, 
	"descPais": "ESPAÑA", 
	"descProvincia": "GIRONA", 
	"baixa": null, 
	"codMunicipi": 160,
	"descMunicipi": "SANT FELIU DE GUIXOLS"
	},
"observacions": "",
"codiTipusDomicili": "POST",
"codiTipusLocal": "1",
"codiPostal": "17220",
"apartatCorreus": null,
"codiGIS": "2561923",
"refCadastral": "",
"descCarrer": "C/ CREU",
"descPseudovia": "",
"descTipusDomicili": null,
"descTipusLocal": null, 
"pobldesc": "",
"paiscod": null,
"provcod": null,
"municod": null,
"stddgr": "20021129",
"stddmod": "20021129",
"stdhgr": "105010",
"stdhmod": "105010",
"stdugr": "traspas",
"stdumod": "traspas",
"stdapladd": "HAB",
"stdaplmod": "HAB",
"swRevisat": null
}
``` 

### 3.2.4 Cercar Domicilis

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/domicilis/search |	

***Cos de la petició***:

```json
{
"nivell": "string",
"aplicacio": "string",
"usuari": "string",
"ambKm": true,
"domTip": [
"string"
],
"domTloc": [
"string"
],
"domicili": { 
	"apartatCorreus": 0,
	"baixa": {
		"dataBaixa": "string", 
		"esBaixa": "string"
		},
	"bis1": "string",
	"bis2": "string",
	"bloc": "string",
	"codiCarrer": 0,
	"codiDomicili": 0,
	"codiGIS": "string",
	"codiPostal": "string",
	"codiPseudovia": 0,
	"codiTipusDomicili": "string",
	"codiTipusLocal": "string",
	"escala": "string",
	"hm": 0,
	"km": 0,
	"municipi": { 
		"codMunicipi": 0,
		"codPais": 0,
		"codProvincia": 0
		},
	"num1": "string",
	"num2": "string",
	"observacions": "string", 
	"pis": "string", 
	"pobldesc": "string",
	"porta": "string",
	"portal": "string",
	"refCadastral": "string"
	},
"duplicarAcceptat": true, 
"g_G5Municod": 0,
"g_G5Paiscod": 0,
"g_G5Provcod": 0,
"maxResults": 0, 
"nomesBaixa": true,
"senseAproximadors": true,
"senseNumero": true
}
```

|Paràmetres|
|----------|
|Veure la descrició de paràmetres de l’apartat [**Alta**](#322-alta).|

***Exemple:***

```json
Request:
POST /domicilis/search


Request body: {
    "domicili": {
        "codiCarrer": 74,
        "municipi": {
            "codMunicipi": 160,
            "codPais": 108,
            "codProvincia": 17
        },
        "num1": "0064",
        "pis": "01"
    },
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "domiciliArray": [
        {
            "codiDomicili": 1240,
            "codiCarrer": 74,
            "codiPseudovia": null,
            "num1": "0064",
            "num2": "",
            "bis1": "",
            "bis2": "",
            "km": null,
            "hm": null,
            "bloc": "",
            "portal": "",
            "escala": "",
            "pis": "01",
            "porta": "",
            "descripcio": "C/ CREU N.0064 Pis.01 CP 17220 SANT FELIU DE GUIXOLS (GIRONA) - ESPAÑA",
            "baixa": {
                "dataBaixa": "",
                "esBaixa": "0"
            },
            "baixaRelacio": null,
            "municipi": {
                "codPais": 108,
                "codProvincia": 17,
                "descPais": "ESPAÑA",
                "descProvincia": "GIRONA",
                "baixa": null,
                "codMunicipi": 160,
                "descMunicipi": "SANT FELIU DE GUIXOLS"
            },
            "observacions": "",
            "codiTipusDomicili": "POST",
            "codiTipusLocal": "1",
            "codiPostal": "17220",
            "apartatCorreus": null,
            "codiGIS": "2659213",
            "refCadastral": "",
            "descCarrer": "C/ CREU",
            "descPseudovia": "",
            "descTipusDomicili": null,
            "descTipusLocal": null,
            "pobldesc": "",
            "paiscod": null,
            "provcod": null,
            "municod": null,
            "stddgr": "20021129",
            "stddmod": "20021129",
            "stdhgr": "105819",
            "stdhmod": "105819",
            "stdugr": "traspas",
            "stdumod": "traspas &1",
            "stdapladd": "HAB",
            "stdaplmod": "HAB",
            "swRevisat": null
        },
....
    ]
}

```


### 3.2.5 Recuperar les Persones d’un Domicili

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/domicilis/persones/get |	


***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "codiDomicili": 0,
    "codiMunicipi": 0,
    "codiPais": 0,
    "codiProvincia": 0
}
```



|Paràmetres|
|----------|
|Veure apartat [**Recuperar un domicili**](#323-recuperar-un-domicili) |

***Exemple:***

```json
Request:
POST /domicilis/persones/get


Request body: {
    "codiDomicili": 555,
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "personaArray": [
        {
            "nom": "PERE",
            "nif": "40000000",
            "nifDC": "F",
            "particula1": "",
            "cognom1": "XXXX",
            "particula2": "",
            "cognom2": "YYYY",
            "baixa": {
                "dataBaixa": "",
                "esBaixa": "0"
            },
            "password": "",
            "descTipusPersona": null,
            "descNacionalitat": "ESPAÑA",
            "codi": 1820,
            "codTipusPersona": "F",
            "passaport": "",
            "dataNaixement": "19190414",
            "codNacionalitat": 108,
            "sexe": "1",
            "municipiNaixement": {
                "codPais": 108,
                "codProvincia": 17,
                "descPais": "ESPAÑA",
                "descProvincia": "GIRONA",
                "baixa": null,
                "codMunicipi": 160,
                "descMunicipi": "SANT FELIU DE GUIXOLS"
            },
            "nomComplet": "XXXX YYYY PERE",
            "contacte": null,
            "formaContacte": null,
            "codIdioma": "",
            "perssw": "0",
            "nifOrig": "",
            "esHabitant": false,
            "esPersnull": false,
            "esContribuent": false,
            "esNifModificable": false,
            "stddgr": "20021129",
            "stddmod": "20021129",
            "stdhgr": "105009",
            "stdhmod": "000002",
            "stdugr": "traspas",
            "stdumod": "traspas",
            "stdapladd": "HAB ",
            "stdaplmod": "HAB ",
            "codiEstudis": null,
            "nomesBaixa": false,
            "dataBaixa2": null,
            "gestCont": false
        }
    ]
}

```
### 3.2.6 Recuperar Domicilis d’un Territori

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/domicilis/getperterritori |		

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "codiTerritori": "string"
}
```


|Paràmetres| |
|----------|-|
|codiTerritori	|Codi de territori. |

***Exemple:***

```json
Request:
POST /domicilis/getperterritori


Request body: {
    "codiTerritori": "2163104",
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}
        
Expected response: [
    265,
    9607
]

```
### 3.2.7 Recuperar Tipus de Domicilis

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/domicilis/tipus/get |		

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```
***Exemple:***

```json
Request:
POST /domicilis/tipus/get


Request body: {
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}
Expected response: {
    "tipusDomiciliArray": [
        {
            "descripcio": "Apartat de correus",
            "descripcio2": "",
            "codi1": "APTC",
            "codi2": "",
            "qual": "",
            "comptador": null
        },
        {
            "descripcio": "Adreça postal",
            "descripcio2": "",
            "codi1": "POST",
            "codi2": "",
            "qual": "",
            "comptador": null
        }
    ]
}
```
### 3.2.8 Recuperar Tipus de Locals

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/domicilis/tipuslocals/get|		

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```
***Exemple:***

```json
Request:
POST /domicilis/tipuslocals/get


Request body: {
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "tipusLocalArray": [
        {
            "descripcio": "DOMICILIO FAMILIAR",
            "descripcio2": "",
            "codi1": "1",
            "codi2": "",
            "qual": "",
            "comptador": null
        },
        {
            "descripcio": "LOCAL SOCIAL",
            "descripcio2": "",
            "codi1": "2",
            "codi2": "",
            "qual": "",
            "comptador": null
        },
        {
            "descripcio": "Apartat de correus",
            "descripcio2": "",
            "codi1": "A",
            "codi2": "",
            "qual": "",
            "comptador": null
        }
    ]
}
```
## 3.3 Persones

* ***Descripció*** - Permet guardar i obtenir informació de persones i dades relacionades com el domicili i el domicili bancari. També permetn obtenir dades genèriques com partícules dels cognoms o tipus de persones.

* ***Ruta*** -  /persones

### 3.3.1 Operacions


| Mètode | Ruta | Descripció |
| ------ | ---- | ---------- |                                                                   
|POST|	/put					|Modificar les dades d’una Persona.                               |
|POST|	/putsimple				|Crea un registre de Persona amb les mínimes dades requerides.    |
|POST|	/get					|Recupera una Persona.                                            |
|POST|	/search					|Cerca Persones per diferents criteris.                           |
|POST|	/getmodificades			|Recupera les últimes Persones modificades per límits de dates.   |
|POST|	/espersonafisica		|Retorna l’indicador de Persona Física.                           |
|POST|	/particulescognom/put	|Crea noves Partícules de Cognom.                                 |
|POST|	/particulescognom/get	|Recupera les Partícules dels Cognoms.                            |
|POST|	/domicilis/get			|Recupera tots els Domicilis d’una Persona.                       |
|POST|	/domicilis/getone		|Recupera un Domicili d’una Persona.                              |
|POST|	/domicilis/getdefault	|Recupera el Domicili per defecte d’una Persona.                  |
|POST|	/domicilisbancaris/get	|Recupera els Domicilis Bancaris d’una Persona.                   |
|POST|	/noticies/get			|Recupera les Notícies d’una Persona.                             |
|POST|	/representants/get		|Recupera els Representants d’una Persona.                        |
|POST|	/tipus/get				|Recupera els tipus de Persones.                          |
 



### 3.3.2 Modificar

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/persones/put|		

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "g_G5Paiscod": 0,
    "maxResult": 0,
    "persona": {
        "baixa": {
            "dataBaixa": "string",
            "esBaixa": "string"
        },
        "codIdioma": "string",
        "codNacionalitat": 0,
        "codTipusPersona": "string",
        "codi": 0,
        "codiEstudis": "string",
        "cognom1": "string",
        "cognom2": "string",
        "contacte": "string",
        "dataBaixa2": "string",
        "dataNaixement": "string",
        "descNacionalitat": "string",
        "descTipusPersona": "string",
        "esContribuent": true,
        "esHabitant": true,
        "esNifModificable": true,
        "esPersnull": true,
        "formaContacte": "string",
        "gestCont": true,
        "municipiNaixement": {
            "baixa": {
                "dataBaixa": "string",
                "esBaixa": "string"
            },
            "codMunicipi": 0,
            "codPais": 0,
            "codProvincia": 0,
            "descMunicipi": "string",
            "descPais": "string",
            "descProvincia": "string"
        },
        "nif": "string",
        "nifDC": "string",
        "nifOrig": "string",
        "nom": "string",
        "nomComplet": "string",
        "nomesBaixa": true,
        "particula1": "string",
        "particula2": "string",
        "passaport": "string",
        "password": "string",
        "perssw": "string",
        "sexe": "string",
        "stdapladd": "string",
        "stdaplmod": "string",
        "stddgr": "string",
        "stddmod": "string",
        "stdhgr": "string",
        "stdhmod": "string",
        "stdugr": "string",
        "stdumod": "string"
    },
    "tipoPersona": [
        "string"
    ]
}
```
|Paràmetres| |
|----------|-|
|g_G5Paiscod	      |                                      |
|maxResult	      |                                      |
|baixa	          |                                      |
|codiIdioma			|Codi d’idioma (per defecte català). |
|codiNacionalitat	|Codi de país.                       |
|codiTipusPersona	|Cod de tipus de persona.            |
|codi				|Codi de persona.                    |
|codiEstudis			|Codi d’estudis                      |
|cognom1	          |                                      |
|cognom2	          |                                      |
|contacte	      |                                      |
|dataBaixa2	      |                                      |
|dataNaixement	  |                                      |
|descNacionalitat	|                                    |
|descTipusPersona	|                                    |
|esContribuent		|True/false.                         |
|esHabitant			|True/false.                         |
|esNifModificable	|True/false.                         |
|esPersnull			|True/false.                         |
|formaContacte	  |                                      |
|gesCont	          |                                      |
|municipiNaixement|                                       |
|nif					|Part numèrica del NIF.                 |
|nifDC				|Dígit de control de l NIF.             |
|nifOrig	          |                                       |
|nom	              |                                       |
|nomComplet			|Nom i cognoms.                         |
|nomesBaixa	      |                                       |
|particula1	      |                                       |
|particula2	      |                                       |
|passaport	      |                                       |
|password	      |                                       |
|perssw	          |                                       |
|sexe	          |                                       |
|stdapladd	      |                                       |
|stdasplmod	      |                                       |
|stdhgr	          |                                       |
|stdhmod	          |                                       |
|stdugr	          |                                       |
|stdumod	          |                                       |


***Exemple:***

```json
Request:
POST /persones/put

Request body: {
    "aplicacio": "NCL",
    "nivell": "9999",
    "persona": {
        "codNacionalitat": 108,
        "codTipusPersona": "F",
        "codi": 1678,
        "cognom1": "TESTC1",
        "cognom2": "TESTC2",
        "dataNaixement": "19251226",
        "descNacionalitat": "ESPAÑA",
        "esContribuent": false,
        "esHabitant": false,
        "esNifModificable": false,
        "esPersnull": false,
        "gestCont": false,
        "municipiNaixement": {
            "codMunicipi": 232,
            "codPais": 108,
            "codProvincia": 31,
            "descMunicipi": "TUDELA",
            "descPais": "ESPAÑA",
            "descProvincia": "NAVARRA"
        },
        "nif": "00507486",
        "nifDC": "Z",
        "nom": "PACO",
        "nomComplet": "PACO TESTC1 TESTC2",
        "nomesBaixa": false,
        "sexe": "1"
    },
    "tipoPersona": [
        "F"
    ],
    "usuari": "G5Admin"
}
     
Expected response: {
    "nom": "PACO",
    "nif": "00507486",
    "nifDC": "Z",
    "particula1": "",
    "cognom1": "TESTC1",
    "particula2": "",
    "cognom2": "TESTC2",
    "baixa": {
        "dataBaixa": "",
        "esBaixa": ""
    },
    "password": "",
    "descTipusPersona": null,
    "descNacionalitat": "ESPAÑA",
    "codi": 1678,
    "codTipusPersona": "F",
    "passaport": "",
    "dataNaixement": "19251226",
    "codNacionalitat": 108,
    "sexe": "1",
    "municipiNaixement": {
        "codPais": 108,
        "codProvincia": 31,
        "descPais": "ESPAÑA",
        "descProvincia": "NAVARRA",
        "baixa": null,
        "codMunicipi": 232,
        "descMunicipi": "TUDELA"
    },
    "nomComplet": "TESTC1 TESTC2 PACO",
    "contacte": null,
    "formaContacte": null,
    "codIdioma": "",
    "perssw": "",
    "nifOrig": "",
    "esHabitant": false,
    "esPersnull": false,
    "esContribuent": false,
    "esNifModificable": false,
    "stddgr": "20021129",
    "stddmod": "20200709",
    "stdhgr": "104936",
    "stdhmod": "092742",
    "stdugr": "traspas",
    "stdumod": "G5Admin",
    "stdapladd": "HAB ",
    "stdaplmod": "NCL",
    "codiEstudis": null,
    "nomesBaixa": false,
    "dataBaixa2": null,
    "gestCont": false
}

```
### 3.3.3 Creacio simple


| Mètode | Ruta |
| ------ | ---- | 
|POST |	/persones/putsimple|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "cognom1": "string",
    "cognom2": "string",
    "document": "string",
    "nom": "string"
}
    {
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "cognom1": "string",
    "cognom2": "string",
    "document": "string",
    "nom": "string"
}
```

|Paràmetres| |
|----------|-|
|cognom1		|Primer cognom.                   |
|cognom2		|Segon cognom.                     |
|document	|Document identificatiu            |
|nom			|Nom.                              |
Exemple:

### 3.3.4 Recuperar una Persones

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/persones/get|		

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "codiPersona": 0,
    "formatNomCognoms": true,
    "g_G5Paiscod": 0,
    "mostrarBaixes": true
}
```
|Paràmetres| |
|----------|-|
|codiPersona			|Codi de la persona a recuperar.        |
|formatNomCosnoms	|                                       |
|g_G5Paiscod	        |                                       |
|mostrarBaixes	    |                                       |


***Exemple:***

```json
Request:
POST /persones/get


Request body: {
    "codiPersona": 555,
    "formatNomCognoms": false,
    "mostrarBaixes": false,
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}


Expected response: {
    "nom": "MARIA",
    "nif": "41111111",
    "nifDC": "J",
    "particula1": "",
    "cognom1": "XXXX",
    "particula2": "",
    "cognom2": "YYYY",
    "baixa": {
        "dataBaixa": "",
        "esBaixa": "0"
    },
    "password": "",
    "descTipusPersona": null,
    "descNacionalitat": "ESPAÑA",
    "codi": 555,
    "codTipusPersona": "F",
    "passaport": "",
    "dataNaixement": "19950126",
    "codNacionalitat": 108,
    "sexe": "1",
    "municipiNaixement": {
        "codPais": 108,
        "codProvincia": 17,
        "descPais": "ESPAÑA",
        "descProvincia": "GIRONA",
        "baixa": null,
        "codMunicipi": 160,
        "descMunicipi": "SANT FELIU DE GUIXOLS"
    },
    "nomComplet": "XXXX YYYY MARIA",
    "contacte": null,
    "formaContacte": null,
    "codIdioma": "",
    "perssw": "0",
    "nifOrig": "",
    "esHabitant": false,
    "esPersnull": false,
    "esContribuent": false,
    "esNifModificable": false,
    "stddgr": "20021129",
    "stddmod": "20080508",
    "stdhgr": "104455",
    "stdhmod": "000002",
    "stdugr": "traspas",
    "stdumod": "ALFRED",
    "stdapladd": "HAB ",
    "stdaplmod": "HAB ",
    "codiEstudis": null,
    "nomesBaixa": false,
    "dataBaixa2": null,
    "gestCont": false
}

```


 

### 3.3.5 Cercar Persones

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/persones/search|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "g_G5Paiscod": 0,
    "maxResult": 0,
    "persona": {
        "baixa": {
            "dataBaixa": "string",
            "esBaixa": "string"
        },
        "codIdioma": "string",
        "codNacionalitat": 0,
        "codTipusPersona": "string",
        "codi": 0,
        "codiEstudis": "string",
        "cognom1": "string",
        "cognom2": "string",
        "contacte": "string",
        "dataBaixa2": "string",
        "dataNaixement": "string",
        "descNacionalitat": "string",
        "descTipusPersona": "string",
        "esContribuent": true,
        "esHabitant": true,
        "esNifModificable": true,
        "esPersnull": true,
        "formaContacte": "string",
        "gestCont": true,
        "municipiNaixement": {
            "baixa": {
                "dataBaixa": "string",
                "esBaixa": "string"
            },
            "codMunicipi": 0,
            "codPais": 0,
            "codProvincia": 0,
            "descMunicipi": "string",
            "descPais": "string",
            "descProvincia": "string"
        },
        "nif": "string",
        "nifDC": "string",
        "nifOrig": "string",
        "nom": "string",
        "nomComplet": "string",
        "nomesBaixa": true,
        "particula1": "string",
        "particula2": "string",
        "passaport": "string",
        "password": "string",
        "perssw": "string",
        "sexe": "string",
        "stdapladd": "string",
        "stdaplmod": "string",
        "stddgr": "string",
        "stddmod": "string",
        "stdhgr": "string",
        "stdhmod": "string",
        "stdugr": "string",
        "stdumod": "string"
    },
    "tipoPersona": [
        "string"
    ]
}

```

|Paràmetres| 
|----------|
|Veure l’apartat **Modifcar.** |


***Exemple:***

```json
Request:
POST /persones/search


Request body: {
    "persona": {
        "cognom1": "SOLER"
    },
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "string"
}
Expected response: {
    "personaArray": [
        {
            "nom": "SOLER XXXXXX JAUME",
            "nif": "11111111",
            "nifDC": "V",
            "particula1": "",
            "cognom1": "SOLER",
            "particula2": "",
            "cognom2": "",
            "baixa": {
                "dataBaixa": "",
                "esBaixa": "0"
            },
            "password": "",
            "descTipusPersona": null,
            "descNacionalitat": "ESPAÑA",
            "codi": 32871,
            "codTipusPersona": "F",
            "passaport": "",
            "dataNaixement": "",
            "codNacionalitat": 108,
            "sexe": "",
            "municipiNaixement": null,
            "nomComplet": "SOLER XXXXX JAUME",
            "contacte": null,
            "formaContacte": null,
            "codIdioma": "",
            "perssw": "0",
            "nifOrig": "",
            "esHabitant": false,
            "esPersnull": false,
            "esContribuent": false,
            "esNifModificable": false,
            "stddgr": "20031113",
            "stddmod": "20031113",
            "stdhgr": "165753",
            "stdhmod": "000002",
            "stdugr": "traspas",
            "stdumod": "traspas",
            "stdapladd": "GTR ",
            "stdaplmod": "GTR ",
            "codiEstudis": null,
            "nomesBaixa": false,
            "dataBaixa2": null,
            "gestCont": false
        },
....
    ]
}

```

 

### 3.3.6 Recuperar les últimes Persones modificades

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/persones/getmodificades|

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "dataModificacio": "string",
    "horaModificacio": "string"
}
```
|Paràmetres| |
|----------|-|
|dataModificacio	|Dia de modificació.|
|horaModificacio		|Hora de modificació.|
 

***Exemple:***

```json
Request:
POST /persones/getmodificades


Request body: {
    "dataModificacio": "20200101",
    "horaModificacio": "000000",
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "persones": [
        106260
    ]
}

```

### 3.3.7 Indicador de Persona Física

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/persones/espersonafisica	|

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "codiPersona": 0,
    "gestCont": true
}
```

***Exemple:***

```json
Request:
POST /persones/espersonafisica


Request body: {
    "codiPersona": 555,
    "gestCont": true,
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "response": true
}

```

### 3.3.8 Crear Partícules de cognoms

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/persones/particulescognom/put	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "particulaCognom": {
        "codi1": "string",
        "codi2": "string",
        "comptador": 0,
        "descripcio": "string",
        "descripcio2": "string",
        "qual": "string"
    }
}
```


|Paràmetres| |
|----------|-|
|codi1		|Codi partícula                     |
|codi2		|No s’utilitza.                     |
|comptador	|No s’utilitza.                     |
|descripcio	|Descripció de la partícula.        |
|descripcio2	|No s’utilitza.                     |
|qual		|No s’utilitza.                     |

***Exemple:***

```json
Request:
POST /persones/particulescognom/put

Request body: {
    "particulaCognom": {
        "codi1": "XX",
        "descripcio": "XX"
    },
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "descripcio": "XX",
    "descripcio2": "",
    "codi1": "XX",
    "codi2": "",
    "qual": "",
    "comptador": null
}

```
### 3.3.9 Recuperar Partícules de cognoms

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/persones/particulescognom/get	|	

**Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /persones/particulescognom/get


Request body: {
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "particulaCognomArray": [
        {
            "descripcio": "D'",
            "descripcio2": "",
            "codi1": "D'",
            "codi2": "",
            "qual": "",
            "comptador": null
        },
        {
            "descripcio": "DE",
            "descripcio2": "",
            "codi1": "DE",
            "codi2": "",
            "qual": "",
            "comptador": null
        },
....
    ]
}

```
 
### 3.3.10 Recuperar tots els Domicilis

| Mètode | Ruta |
| ------ | ---- | 
|POST |	/persones/domicilis/get	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "codiPersona": ,
    "formatNomCognoms": true,
    "g_G5Paiscod": 0,
    "mostrarBaixes": true
}
```

***Exemple:***

```json
Request:
POST /persones/domicilis/get


Request body: {
    "codiPersona": 555,
    "formatNomCognoms": true,
    "g_G5Paiscod": 0,
    "mostrarBaixes": true,
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}


Expected response: {
    "domiciliArray": [
        {
            "codiDomicili": 52287,
            "codiCarrer": 465,
            "codiPseudovia": 10177,
            "num1": "0016",
            "num2": "",
            "bis1": "",
            "bis2": "",
            "km": null,
            "hm": null,
            "bloc": "",
            "portal": "",
            "escala": "2",
            "pis": "02",
            "porta": "02",
            "descripcio": null,
            "baixa": {
                "dataBaixa": "",
                "esBaixa": "0"
            },
            "baixaRelacio": {
                "dataBaixa": "",
                "esBaixa": ""
            },
            "municipi": {
                "codPais": 108,
                "codProvincia": 17,
                "descPais": "ESPAÑA",
                "descProvincia": "GIRONA",
                "baixa": null,
                "codMunicipi": 160,
                "descMunicipi": "SANT FELIU DE GUIXOLS"
            },
            "observacions": "",
            "codiTipusDomicili": "POST",
            "codiTipusLocal": "1",
            "codiPostal": "17220",
            "apartatCorreus": null,
            "codiGIS": "2355810",
            "refCadastral": "",
            "descCarrer": "PL LLUIS ESTEVA I CRUAÑAS",
            "descPseudovia": "- APARTAMENTS GASELA",
            "descTipusDomicili": null,
            "descTipusLocal": null,
            "pobldesc": null,
            "paiscod": null,
            "provcod": null,
            "municod": null,
            "stddgr": "20050330",
            "stddmod": "20050330",
            "stdhgr": "091613",
            "stdhmod": "091613",
            "stdugr": "RAFA",
            "stdumod": "RAFA",
            "stdapladd": "PAD ",
            "stdaplmod": "PAD ",
            "swRevisat": null
        },
    ....
    ]
}

```

### 3.3.11 Recuperar un Domicili


| Mètode | Ruta |
| ------ | ---- | 
|POST |	/persones/domicilis/getone	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "codiDomicili": 0,
    "perscod": 0,
    "persnd": 0
}

```

|Paràmetres| |
|----------|-|
|codiDomicili	|Codi del domicili.                          |
|perscod			|Codi de persona.                            |
|persnd			|Número de domicili de la persona            |


***Exemple:***

```json
Request:
POST /persones/domicilis/getone


Request body: {
    "perscod": 555,
    "persnd": 1,
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}
Expected response: {
    "codiDomicili": 150,
    "codiCarrer": 277,
    "codiPseudovia": null,
    "num1": "xx",
    "num2": "",
    "bis1": "",
    "bis2": "",
    "km": null,
    "hm": null,
    "bloc": "",
    "portal": "",
    "escala": "",
    "pis": "xx",
    "porta": "xx",
    "descripcio": "CARRETERA GIRONA N.xx Pis.xx Pta.x CP 17220 SANT FELIU DE GUIXOLS (GIRONA) - ESPAÑA",
    "baixa": {
        "dataBaixa": "",
        "esBaixa": "0"
    },
    "baixaRelacio": null,
    "municipi": {
        "codPais": 108,
        "codProvincia": 17,
        "descPais": "ESPAÑA",
        "descProvincia": "GIRONA",
        "baixa": null,
        "codMunicipi": 160,
        "descMunicipi": "SANT FELIU DE GUIXOLS"
    },
    "observacions": "",
    "codiTipusDomicili": "POST",
    "codiTipusLocal": "1",
    "codiPostal": "17220",
    "apartatCorreus": null,
    "codiGIS": "2064207",
    "refCadastral": "",
    "descCarrer": "CARRETERA GIRONA",
    "descPseudovia": "",
    "descTipusDomicili": null,
    "descTipusLocal": null,
    "pobldesc": "",
    "paiscod": null,
    "provcod": null,
    "municod": null,
    "stddgr": "20021129",
    "stddmod": "20030206",
    "stdhgr": "104455",
    "stdhmod": "134208",
    "stdugr": "traspas",
    "stdumod": "INTER3",
    "stdapladd": "HAB",
    "stdaplmod": "HAB",
    "swRevisat": null
}

```
### 3.3.12 Recuperar el Domicili per defecte

| Mètode | Ruta |
| ------ | ---- | 
|POST |/persones/domicilis/getdefault	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "perscod": 0
}

```

***Exemple:***

```json
Request:
POST /persones/domicilio/detdefault


Request body: {
    "perscod": 555,
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "codiDomicili": 52287,
    "codiCarrer": 465,
    "codiPseudovia": 10177,
    "num1": "xx",
    "num2": "",
    "bis1": "",
    "bis2": "",
    "km": null,
    "hm": null,
    "bloc": "",
    "portal": "",
    "escala": "x",
    "pis": "xx",
    "porta": "xx",
    "descripcio": "PL LLUIS ESTEVA I CRUAÑAS [- APARTAMENTS GASELA] N.xx Esc.x Pis.xx Pta.xx CP 17220 SANT FELIU DE GUIXOLS (GIRONA) - ESPAÑA",
    "baixa": {
        "dataBaixa": "",
        "esBaixa": "0"
    },
    "baixaRelacio": null,
    "municipi": {
        "codPais": 108,
        "codProvincia": 17,
        "descPais": "ESPAÑA",
        "descProvincia": "GIRONA",
        "baixa": null,
        "codMunicipi": 160,
        "descMunicipi": "SANT FELIU DE GUIXOLS"
    },
    "observacions": "",
    "codiTipusDomicili": "POST",
    "codiTipusLocal": "1",
    "codiPostal": "17220",
    "apartatCorreus": null,
    "codiGIS": "2355810",
    "refCadastral": "",
    "descCarrer": "PL LLUIS ESTEVA I CRUAÑAS",
    "descPseudovia": "- APARTAMENTS GASELA",
    "descTipusDomicili": null,
    "descTipusLocal": null,
    "pobldesc": "",
    "paiscod": null,
    "provcod": null,
    "municod": null,
    "stddgr": "20050330",
    "stddmod": "20050330",
    "stdhgr": "091613",
    "stdhmod": "091613",
    "stdugr": "RAFA",
    "stdumod": "RAFA",
    "stdapladd": "PAD",
    "stdaplmod": "PAD",
    "swRevisat": null
}

```
### 3.3.13 Recuperar els Domicilis Bancaris

| Mètode | Ruta |
| ------ | ---- | 
|POST | /persones/domicilisbancaris/get	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "codiPersona": 0,
    "formatNomCognoms": true,
    "g_G5Paiscod": 0,
    "mostrarBaixes": true
}
```

***Exemple:***

```json
Request:
POST /persones/domicilisbancaris/get


Request body: {
    "codiPersona": 555,
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "comptadorDomiciliBancariArray": [
        1
    ],
    "domiciliBancariArray": [
        {
            "codiBanc": "0131",
            "numeroCompte": "0000111122223333",
            "observacions": "",
            "numComptePrint": "0131 0000 11 1122223333",
            "dataDomiciliacio": "",
            "descBanc": "BANCO ESPIRITU SANTO",
            "bic": "",
            "iban": "",
            "dataValidesa": ""
        }
    ]
}

```

 
### 3.3.14 Recuperar les Notícies

| Mètode | Ruta |
| ------ | ---- | 
|POST | /persones/noticies/gett	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "codiPersona": 0,
    "idioma": 0
}
```

***Exemple:***

```json
Request:
POST /persones/noticies/get


Request body: {
    "idioma": 0,
    "codiPersona": 555,
    "nivell": "9999",
    "aplicacio": "NOTA",
    "usuari": "G5Admin"
}


Expected response: {
    "noticies": [
        {
            "notcod": 23,
            "notdesc": "X2012006089",
            "nottxt": "es passa full d'incidències pel tractament amb XXXXX.",
            "notsql": null,
            "dataini": "20120727",
            "datafi": null,
            "aplcod": null,
            "idiocod": null
        },
....
    ]
}

```
### 3.3.15 Recuperar els Representants

| Mètode | Ruta |
| ------ | ---- | 
|POST | /persones/representants/get	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "perscod": 0,
    "reprtip": "string",
    "swBaixa": true
}
```

***Exemple:***

```json
Request:
POST /persones/representants/get


Request body: {
    "perscod": 511,
    "reprtip": "",
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "personaArray": [
        {
            "nom": "ANGEL",
            "nif": "",
            "nifDC": "",
            "particula1": "",
            "cognom1": "XXXX",
            "particula2": "",
            "cognom2": "YYYY",
            "baixa": {
                "dataBaixa": "",
                "esBaixa": "0"
            },
            "password": "",
            "descTipusPersona": null,
            "descNacionalitat": null,
            "codi": 89561,
            "codTipusPersona": "Z",
            "passaport": "",
            "dataNaixement": "",
            "codNacionalitat": null,
            "sexe": "1",
            "municipiNaixement": null,
            "nomComplet": "XXXX YYYY ANGEL",
            "contacte": null,
            "formaContacte": null,
            "codIdioma": "",
            "perssw": "0",
            "nifOrig": "",
            "esHabitant": false,
            "esPersnull": false,
            "esContribuent": false,
            "esNifModificable": false,
            "stddgr": "20080709",
            "stddmod": "20080709",
            "stdhgr": "123730",
            "stdhmod": "000002",
            "stdugr": "IPONT",
            "stdumod": "IPONT",
            "stdapladd": "SDE ",
            "stdaplmod": "SDE ",
            "codiEstudis": null,
            "nomesBaixa": false,
            "dataBaixa2": null,
            "gestCont": false
        }
    ]
}

```
### 3.3.16 Recuperar Tipus de Persones

| Mètode | Ruta |
| ------ | ---- | 
|POST | /persones/tipus/get	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /persones/tipus/get


Request body: {
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "tipusPersonaArray": [
        {
            "descripcio": "ENTITAT",
            "descripcio2": "",
            "codi1": "E",
            "codi2": "",
            "qual": "",
            "comptador": null
        },
        {
            "descripcio": "PERSONA FÍSICA",
            "descripcio2": "",
            "codi1": "F",
            "codi2": "",
            "qual": "",
            "comptador": null
        },
....
    ]
}

```
## 3.4 Registre

* ***Descripció*** - Permet obtenir dades genèriques com àrees, departaments, unitats de treball, idiomes, assumptes i altres. També operacions com duplicar o reservar registres.

* ***Ruta*** -  /registre

### 3.4.1 Operacions

| Mètode | Ruta | Descripció |
| ------ | ---- | ---------- |                                                                   
|POST	| /duplicar						|Duplica registres.                                              |
|POST	| /reservar						|Reserva registres.                                              |
|POST	| /arees/get						|Recupera les Àrees.                                             |
|POST	| /departaments/get				|Recupera Departaments / Unitats de Treball d’una Àrea.          |
|POST	| /assumptes/get					|Recupera un Assumpte.                                           |
|POST	| /assumptes/getperdepartament	|Recupera els Assumptes d’un Departament / Unitat de Treball.    |
|POST	| /extractes/get					|Recupera Extractes.                                             |
|POST	| /idiomes/get					|Recupera Idiomes.                                               |
|POST	| /tipustransport/get				|Recupera Tipus de Transport.                                    |


### 3.4.2 Duplicar

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registre/duplicar	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "codiEntitat": "string",
    "copiarDownload4J": true,
    "copiarFileBase64": true,
    "crearPDF": true,
    "directori": "string",
    "idioma": "string",
    "numeroRegistre": "string",
    "plantilla": "string",
    "quantitat": 0,
    "recalcularDirectoriPlantilles": true,
    "urlServer": "string"
}
```

|Paràmetres| |
|----------|-|
|codiEntitat	                          |
|copiarDownload4J	                  |
|copiarFileBase64	                  |
|crearPDF	                          |
|directori	                          |
|idioma	                              |
|numeroRegistr 	                      |
|plantilla	                          |
|quantitat	                          |
|recalcularDirectoriPlantilles	      |
|urlServer	                          |

***Exemple:***

```json
Request:
POST /registre/duplicar
Request body: {
    "numeroRegistre": "E2020000001",
    "quantitat": 1,
    "crearPDF": false,
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "numeroRegistres": [
        "E2020000014"
    ],
    "fitxerPDF": null,
    "linkDownload4J": null,
    "fileBase64": null
}

```
### 3.4.3 Reservar

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registre/reservar	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "areacodCreador": "string",
    "assumcod": "string",
    "codiEntitat": "string",
    "copiarDownload4J": true,
    "copiarFileBase64": true,
    "crearPDF": true,
    "data": "string",
    "depcodCreador": "string",
    "directori": "string",
    "hora": "string",
    "idioma": "string",
    "plantilla": "string",
    "quantitat": 0,
    "recalcularDirectoriPlantilles": true,
    "tipusRegistre": "string",
    "urlServer": "string"
}

```
|Paràmetres| |
|----------|-|
|areacodCreador	               |                                                        |
|assumcod						|Codi d’assumpte.                                       |
|codiEntitat						|Codi d’entitat. Si no es multi-entitat, posar 1.       |
|copiaDownload4J	               |                                                        |
|copiarFileBase64	           |                                                        |
|crearPDF						|Indica si cal crear un PDF (true/false).               |
|data							|Data del registre reservat.                            |
|hora							|Hora del registre reservat.                            |
|idioma							|Codi d’idioma.                                         |
|plantilla	                   |                                                        |
|quantitat						|Número de registres a reservar.                        |
|recalcularDirectoriPlantilles	|                                                       |
|tipusRegistre					|Tipus de registre E/S (entrada o sortida).             |
|urlServer	                   |      |


***Exemple:***

```json
Request:
POST /registre/reservar


Request body: {
    "assumcod": "COMU",
    "codiEntitat": "1",
    "crearPDF": false,
    "data": "20200525",
    "hora": "090000",
    "quantitat": 1,
    "tipusRegistre": "E",
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Audi"
}


Expected response: {
    "numeroRegistres": [
        "E2020000015"
    ],
    "fitxerPDF": null,
    "linkDownload4J": null,
    "fileBase64": null
}

```

### 3.4.4 Recuperar Àrees

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registre/arees/get	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registre/arees/get


Request body: {
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "areaArray": [
        {
            "codiArea": "ADM",
            "descArea": "ADMINISTRACIÓ ELECTRÒNICA",
            "sdeprioritat": null
        },
        {
            "codiArea": "AL",
            "descArea": "ALCALDIA",
            "sdeprioritat": null
        },
....
    ]
}

```

### 3.4.5 Recuperara Departaments

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registre/departaments/get	|	

***Cos de la petició***:

```json
{
    "codiArea": "string",
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registre/departaments/get


Request body: {
    "codiArea": "AL",
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "departamentArray": [
        {
            "codiArea": "AL",
            "descArea": "ALCALDIA",
            "codiDepartament": "XXXX",
            "descDepartament": "XXXXXXXXXXXXXXX"
        },
        {
            "codiArea": "AL",
            "descArea": "ALCALDIA",
            "codiDepartament": "YYYY",
            "descDepartament": "YYYYYYYYYYYYYYY"
        },
....
    ]
}

```
 
### 3.4.6 Recuperar un Assumpte

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registre/assumptes/get	|	

***Cos de la petició***:

```json
{
    "codiAssumpte": "string",
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registre/assumptes/get


Request body: {
    "codiAssumpte": "FACT",
    "nivell": "9999",
    "aplicacio": "NCL",
    "usuari": "G5Admin"
}


Expected response: {
    "assumpteArray": [
        {
            "descripcio": "FACTURES",
            "baixa": null,
            "codi": "FACT",
            "departament": {
                "codiArea": "AL",
                "descArea": "ALCALDIA",
                "codiDepartament": "G5AD",
                "descDepartament": "AUDIFILM"
            }
        }
    ]
}

```

### 3.4.7 Recuperar Assumptes per Departament

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registre/assumptes/getperdepartament	|	

***Cos de la petició***:

```json
{
    "codiArea": "string",
    "codiDepartament": "string",
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registre/assumptes/getperdepartament


Request body: {
    "codiArea": "AL",
    "codiDepartament": "G5AD",
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "assumpteArray": [
        {
            "descripcio": "AJUTS I BEQUES",
            "baixa": null,
            "codi": "AJBE",
            "departament": {
                "codiArea": "AL",
                "descArea": "ALCALDIA",
                "codiDepartament": "G5AD",
                "descDepartament": "AUDIFILM"
            }
        },
        {
            "descripcio": "ALTA RÀPIDA",
            "baixa": null,
            "codi": "ALTA",
            "departament": {
                "codiArea": "AL",
                "descArea": "ALCALDIA",
                "codiDepartament": "G5AD",
                "descDepartament": "AUDIFILM"
            }
        },
    ....
    ]
}
```
### 3.4.8 Recuperar Extractes

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registre/extracte/get	|	

***Cos de la petició***:

```json
{
    "codiArea": "string",
    "codiDepartament": "string",
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}

```

***Exemple:***

```json
Request:
POST /registre/assumptes/getperdepartament


Request body: {
    "codiArea": "AL",
    "codiDepartament": "G5AD",
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "assumpteArray": [
        {
            "descripcio": "AJUTS I BEQUES",
            "baixa": null,
            "codi": "AJBE",
            "departament": {
                "codiArea": "AL",
                "descArea": "ALCALDIA",
                "codiDepartament": "G5AD",
                "descDepartament": "AUDIFILM"
            }
        },
        {
            "descripcio": "ALTA RÀPIDA",
            "baixa": null,
            "codi": "ALTA",
            "departament": {
                "codiArea": "AL",
                "descArea": "ALCALDIA",
                "codiDepartament": "G5AD",
                "descDepartament": "AUDIFILM"
            }
        },
....
    ]
}

```

### 3.4.9 Recuperar Idiomes

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registre/idiomes/get	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registre/extracte/get


Request body: {
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "extracteArray": [
        {
            "codi1": "XXX",
            "codi2": "",
            "comptador": 0,
            "descripcio": "XXX",
            "descripcio2": "",
            "qual": ""
        }
    ]
}

```

### 3.4.10 Recuperar Tipus Transport


| Mètode | Ruta |
| ------ | ---- | 
|POST | /registre/tipustransport/get	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registre/tipustransport/get


Request body: {
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "tipusTransportArray": [
        {
            "descripcio": "Correu electrònic",
            "descripcio2": "",
            "codi1": "1",
            "codi2": "E",
            "qual": "",
            "comptador": null
        },
        {
            "descripcio": "Correu administratiu",
            "descripcio2": "",
            "codi1": "10",
            "codi2": "",
            "qual": "",
            "comptador": null
        },
        ....
    ]
}

```
## 3.5 Registre d’entrada

* ***Descripció*** - Operacions relacionades amb el registre d’entrada. Creació i modificació de registres, obtenir registres relacionats, documents, relacionar registres, relacionar expedients, cercar i altres.

* ***Ruta*** -  /registreentrada

### 3.5.1 Operacions


| Mètode | Ruta | Descripció |
| ------ | ---- | ---------- |                                                                   
|POST	|/put			|Crea o modifica un Registre d’Entrada.                                     |
|POST	|/newsimple		|Crea	un	Registre	d’Entrada	amb	les	dades	mínimes necessàries.   |
|POST	|/putsimple		|Modifica dades concretes d’un Registre d’Entrada.                          |
|POST	|/get			|Recupera un Registre d’Entrada.                                            |
|POST	|/search			|Cerca per diferents criteris.                                              |
|POST	|/expedients/put	|Relaciona un Expedient amb un Registre d’Entrada.                          |
|POST	|/expedients/get	|Recupera els Expedients relacionats.                                       |
|POST	|/interessat/put	|Assigna un Interessat a un Registre d’Entrada.                             |
|POST	|/documents/get	|Recupera els Documents d’un Registre d’Entrada.                            |
|POST	|/entrades/get	|Recupera els Registres d’Entrada relacionats.                              |
|POST	|/sortides/get	|Recupera els Registres de Sortida relacionats.                             |


### 3.5.2 Crear o modificar

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/put	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "gestCont": true,
    "idioma": 0,
    "nivell": "string",
    "nivellTaula": "string",
    "registreEntrada": {
        "arxiu": "string",
        "codiAreaAssumpte": "string",
        "codiAreaCreador": "string",
        "codiAssumpte": "string",
        "codiDepartamentAssumpte": "string",
        "codiDepartamentCreador": "string",
        "codiEntitat": "string",
        "codiExtracte": "string",
        "codiIdioma": "string",
        "codiOrganismeOrigen": 0,
        "codiPersona": 0,
        "codiRepresentant": 0,
        "codiTerritori": 0,
        "codiTipusTransport": "string",
        "comptDomiciliOrganisme": 0,
        "comptDomiciliPersona": 0,
        "comptDomiciliRepresentant": 0,
        "contingutExtracte": "string",
        "dataDocument": "string",
        "dataPresentacio": "string",
        "dataRegistreOrganisme": "string",
        "dataTransport": "string",
        "descAreaAssumpte": "string",
        "descDepartamentAssumpte": "string",
        "domNot": "string",
        "efactComptabilitat": "string",
        "efactEstat": "string",
        "efactNumfactura": "string",
        "efactObs": "string",
        "efactProveidor": "string",
        "fcontactMail": 0,
        "fcontactSms": 0,
        "firmaTopograficaArxiu": "string",
        "habilitarEFactura": true,
        "horaPresentacio": "string",
        "horaRegistre": "string",
        "horaRegistreOrganisme": "string",
        "horaTransport": "string",
        "ine10Origen": "string",
        "notMail": "string",
        "notSms": "string",
        "numPagines": 0,
        "numconordreMail": 0,
        "numconordreSms": 0,
        "numeroEntrada": "string",
        "numeroEntradaEntitat": "string",
        "numeroFormaContacte": 0,
        "numeroOrdreContacte": 0,
        "numeroRegistreOrganisme": "string",
        "numeroTransport": "string",
        "observacionsDocument": "string",
        "observacionsTransport": "string",
        "persNifFoto": "string",
        "persNomFoto": "string",
        "persNot": "string",
        "perscontacn": "string",
        "persndMail": 0,
        "persndSms": 0,
        "persnumconordre": "string",
        "plataforma": "string",
        "registreRelacionat": "string",
        "repcontac": 0,
        "repfcontac": 0,
        "resNumeExt": "string",
        "resorgcontac": 0,
        "resorgfcontac": 0,
        "responsable": "string",
        "tipusOrganisme": "string",
        "tractat": 0,
        "tramcod": 0,
        "tramdesc": "string"
    },
    "usuari": "string",
    "validarCampsObligatoris": true
}
```

|Paràmetres| |
|----------|-|
|arxiu	                          |
|codiAreaAssumpte	              |
|codiAreaCreador	                  |
|codiAssumpte	                  |
|codiDepartamentAssumpte	          |
|codiDepartamentCreador	          |
|coientitat	                      |
|codiExtracte	                  |
|codiIdioma	                      |
|codiOrganismeOrigen	              |
|codiPersona	                      |
|codiRepresentant	              |
|codiTerritori	                  |
|codiTipusTransport	              |
|comptDomiciliOrganisme	          |
|comptDomiciliPersona	           |
|comptDomiciliRepresentant00        |
|contingutExtracte	               |
|dataDocument	                   |
|dataPresentacio	                   |
|dataRegistreOrganisme	           |
|dataTransport	                   |
|descAreaAssumpte	               |
|descDepartamentAssumpte	           |
|domNot	                           |
|efactComptabilitat	               |
|efactEstat	                       |
|efactNumfactura	                   |
|efactObs	                       |
|efactProveidor	                   |
|fcontactMail	                   |
|fcontactSms	                       |
|numPagines	                       |
|numconordreMail	                   |
|numconordreSms	                   |
|numeroEntrada	                   |
|numeroEntradaEntitat	           |
|numeroFormaContacte	               |
|numeroRegistreOrganisme	           |
|numeroTransport	                   |
|observacionsDocumen t	           |
|observacionsTransport	           |
|persNifFoto	                       |
|persNomFoto	                       |
|persNot	                           |
|perscontacn	                       |
|persndMail	                       |
|persnmSms	                       |
|persnumconordre	                   |
|plataforma	                       |
|registreRelacionat	               |
|repcontac	                       |
|repfcontac	                       |
|resNumExt	                       |
|resorgcontac	                   |
|resorgfcontac	                   |
|responsable	                       |
|tipusOrganisme	                   |
|tractat	                           |
|tramcod	                           |
|tramdesc	                       |

***Exemple:***

```json
Request:POST /registreentrada/put


Request body: {
    "aplicacio": "RES",
    "gestCont": false,
    "idioma": 0,
    "nivell": "9999",
    "nivellTaula": "9999",
    "registreEntrada": {
        "codiAreaCreador": "AL",
        "codiAssumpte": "LLAC",
        "codiDepartamentAssumpte": "G5AD",
        "codiDepartamentCreador": "G5AD",
        "codiEntitat": "1",
        "codiPersona": 1678,
        "codiTerritori": 500,
        "codiTipusTransport": "5",
        "comptDomiciliPersona": 0,
        "descAreaAssumpte": "ALCALDIA",
        "descDepartamentAssumpte": "AUDIFILM",
        "fcontactMail": 0,
        "fcontactSms": 0,
        "firmaTopograficaArxiu": "string",
        "numPagines": 0,
        "numeroEntrada": "E2020000016",
        "numeroEntradaEntitat": "E2020000016",
        "observacionsDocument": "PROVA MODIFICACIO",
        "observacionsTransport": "",
        "persNifFoto": "00507486Z",
        "persNomFoto": "PEP",
        "persNot": "",
        "perscontacn": "",
        "persndMail": 0,
        "persndSms": 0,
        "repcontac": 0,
        "repfcontac": 0,
        "tractat": 0,
        "tramcod": 0,
        "horaRegistre": "124443"
    },
    "usuari": "G5Admin",
    "validarCampsObligatoris": false
}


Expected response: {
    "registreEntrada": "E2020000016",
    "registreEntradaEntitat": "E2020000016",
    "dataRegistreEntrada": "20200708",
    "horaRegistreEntrada": "124443",
    "dataPresentacioEntrada": null,
    "horaPresentacioEntrada": null
}

```

### 3.5.3 Creació simple.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/newsimple	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "areacodAssumpte": "string",
    "assumcod": "string",
    "codiTransport": "string",
    "contingut": "string",
    "depcodAssumpte": "string",
    "descripcioCurta": "string",
    "nivell": "string",
    "notificacioElectronica": true,
    "numeroTransport": "string",
    "representant": {
        "carcod": 0,
        "codiPostal": "string",
        "cognom1": "string",
        "cognom2": "string",
        "crearPersonaSiNoExisteix": true,
        "escala": "string",
        "mail": "string",
        "municipi": "string",
        "municod": 0,
        "nif": "string",
        "nom": "string",
        "nomCarrer": "string",
        "numero": "string",
        "pais": "string",
        "paiscod": 0,
        "passaport": "string",
        "pis": "string",
        "porta": "string",
        "provcod": 0,
        "provincia": "string",
        "telefon": "string"
    },
    "responsable": "string",
    "solicitant": {
        "carcod": 0,
        "codiPostal": "string",
        "cognom1": "string",
        "cognom2": "string",
        "crearPersonaSiNoExisteix": true,
        "escala": "string",
        "mail": "string",
        "municipi": "string",
        "municod": 0,
        "nif": "string",
        "nom": "string",
        "nomCarrer": "string",
        "numero": "string",
        "pais": "string",
        "paiscod": 0,
        "passaport": "string",
        "pis": "string",
        "porta": "string",
        "provcod": 0,
        "provincia": "string",
        "telefon": "string"
    },
    "tramcod": 0,
    "transObs": "string",
    "usuari": "string"
}
```
***Exemple:***

```json
Request:
POST /registreentrada/newsimple


Request body: {
    "aplicacio": "RES",
    "areacodAssumpte": "",
    "assumcod": "LLAC",
    "codiTransport": "5",
    "contingut": "",
    "depcodAssumpte": "",
    "descripcioCurta": "Proves registre entrada",
    "nivell": "9999",
    "notificacioElectronica": false,
    "numeroTransport": "",
    "solicitant": {
        "nif": "00507486Z"
    },
    "tramcod": 20,
    "transObs": "prova",
    "usuari": "G5Admin"
}
    
    
    Expected response: {
    "registreEntrada": "E2020000016",
    "registreEntradaEntitat": "E2020000016",
    "dataRegistreEntrada": "20200708",
    "horaRegistreEntrada": "124443",
    "dataPresentacioEntrada": "20200708",
    "horaPresentacioEntrada": "124442"
}

```

### 3.5.4 Modificació simple

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/putsimple	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "gestCont": true,
    "idioma": 0,
    "nivell": "string",
    "nivellTaula": "string",
    "registreEntrada": {
        "arxiu": "string",
        "codiAreaAssumpte": "string",
        "codiAreaCreador": "string",
        "codiAssumpte": "string",
        "codiDepartamentAssumpte": "string",
        "codiDepartamentCreador": "string",
        "codiEntitat": "string",
        "codiExtracte": "string",
        "codiIdioma": "string",
        "codiOrganismeOrigen": 0,
        "codiPersona": 0,
        "codiRepresentant": 0,
        "codiTerritori": 0,
        "codiTipusTransport": "string",
        "comptDomiciliOrganisme": 0,
        "comptDomiciliPersona": 0,
        "comptDomiciliRepresentant": 0,
        "contingutExtracte": "string",
        "dataDocument": "string",
        "dataPresentacio": "string",
        "dataRegistreOrganisme": "string",
        "dataTransport": "string",
        "descAreaAssumpte": "string",
        "descDepartamentAssumpte": "string",
        "domNot": "string",
        "efactComptabilitat": "string",
        "efactEstat": "string",
        "efactNumfactura": "string",
        "efactObs": "string",
        "efactProveidor": "string",
        "fcontactMail": 0,
        "fcontactSms": 0,
        "firmaTopograficaArxiu": "string",
        "habilitarEFactura": true,
        "horaPresentacio": "string",
        "horaRegistre": "string",
        "horaRegistreOrganisme": "string",
        "horaTransport": "string",
        "ine10Origen": "string",
        "notMail": "string",
        "notSms": "string",
        "numPagines": 0,
        "numconordreMail": 0,
        "numconordreSms": 0,
        "numeroEntrada": "string",
        "numeroEntradaEntitat": "string",
        "numeroFormaContacte": 0,
        "numeroOrdreContacte": 0,
        "numeroRegistreOrganisme": "string",
        "numeroTransport": "string",
        "observacionsDocument": "string",
        "observacionsTransport": "string",
        "persNifFoto": "string",
        "persNomFoto": "string",
        "persNot": "string",
        "perscontacn": "string",
        "persndMail": 0,
        "persndSms": 0,
        "persnumconordre": "string",
        "plataforma": "string",
        "registreRelacionat": "string",
        "repcontac": 0,
        "repfcontac": 0,
        "resNumeExt": "string",
        "resorgcontac": 0,
        "resorgfcontac": 0,
        "responsable": "string",
        "tipusOrganisme": "string",
        "tractat": 0,
        "tramcod": 0,
        "tramdesc": "string"
    },
    "usuari": "string",
    "validarCampsObligatoris": true
}
```

***Exemple:***

```json
Request:
POST /registreentrada/putsimple


Request body: {
    "aplicacio": "RES",
    "gestCont": true,
    "idioma": 0,
    "nivell": "9999",
    "nivellTaula": "9999",
    "registreEntrada": {
        "numeroEntrada": "E2020000016",
        "numeroEntradaEntitat": "E2020000016",
        "observacionsDocument": "MODIFICACIO SIMPLE",
        "observacionsTransport": "MODIFICACIO SIMPLE"
    },
    "usuari": "G5Admin",
    "validarCampsObligatoris": false
}


Expected response: {
    "registreEntrada": "E2020000016",
    "registreEntradaEntitat": "E2020000016",
    "dataRegistreEntrada": "20200708",
    "horaRegistreEntrada": "124443",
    "dataPresentacioEntrada": null,
    "horaPresentacioEntrada": null
}

```

### 3.5.5 Recuperar un Registre


| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/get	|	

***Cos de la petició***:

```json
{
    "idioma": 0,
    "numeroRegistreEntrada": "string",
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registreentrada/get


Request body: {
    "numeroRegistreEntrada": "E2020000001",
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "numeroRegistre": "E2020000001",
    "numeroRegistreEntitat": "E2020000001",
    "codiEntitat": "1",
    "codiPersona": 1,
    "fotoRepresentant": null,
    "fotoPersona": {
        "nom": "AJUNTAMENT DE PROVES",
        "nif": "P0826900C",
        "particula1": "",
        "cognom1": "",
        "particula2": "",
        "cognom2": "",
        "domicili": null,
        "esPersonaFisica": null
    },
    "descExtracte": null,
    "codiAssumpte": "CEDU",
    "responsable": "_RESPONSABLE_",
    "numeroFormaContacte": null,
    "numeroOrdreContacte": null,
    "codiRepresentant": null,
    "repcontac": null,
    "repfcontac": null,
    "repcontacdesc": null,
    "repfcontacdesc": null,
    "codiExtracte": null,
    "contingutExtracte": "test",
    "codiIdioma": "0",
    "codiOrganismeOrigen": null,
    "numeroRegistreOrganisme": "",
    "nomComplertOrganisme": null,
    "nifDcOrganisme": null,
    "domiciliOrganisme": null,
    "resNumeExt": "",
    "fotoAssumpte": {
        "codiArea": "AL",
        "descArea": "ALCALDIA",
        "codiDepartament": "G5AD",
        "descDepartament": "AUDIFILM",
        "descAssumpte": "CENTRES ESCOLARS"
    },
    "comptadorDomiciliPersona": 2,
    "codiDomiciliPersona": 11494,
    "baixaPersona": "0",
    "baixaDomiciliPersona": "0",
    "baixaRelacioDomiciliPersona": null,
    "fotoDomiciliPersona": "C/ ALMERIA N.0005 CP 17220 SANT FELIU DE GUIXOLS (GIRONA) - ESPAÑA",
    "comptadorDomiciliRepresentant": null,
    "codiDomiciliRepresentant": null,
    "baixaRepresentant": null,
    "baixaDomiciliRepresentant": null,
    "baixaRelacioDomiciliRepresentant": null,
    "fotoDomiciliRepresentant": null,
    "dataCreacio": "20200217",
    "horaCreacio": "104732",
    "dataPresentacio": "20200217",
    "horaPresentacio": "104722",
    "codiAreaCreador": "AL",
    "descAreaCreador": "ALCALDIA",
    "codiDepartamentCreador": "G5AD",
    "descDepartamentCreador": "AUDIFILM",
    "dataDocument": "20200217",
    "descIdioma": "CATALA",
    "numPagines": 1,
    "observacionsDocument": "",
    "comptadorDomiciliOrganisme": null,
    "codiDomiciliOrganisme": null,
    "dataRegistreOrganisme": "",
    "horaRegistreOrganisme": "",
    "codiTerritori": null,
    "descTerritori": null,
    "codiTipusTransport": "12",
    "descTipusTransport": "Notarial",
    "numeroTransport": "",
    "dataTransport": "",
    "horaTransport": "",
    "observacionsTransport": "ewrgewrg",
    "arxiu": "",
    "firmaTopograficaArxiu": "",
    "descTipusContacte": null,
    "descContacte": null,
    "interessats": [
        1
    ],
    "efactNumfactura": "",
    "registreTancat": true,
    "registreRelacionat": "X2020000017",
    "registreRelacionatEntitat": "X2020000017",
    "tipusOrganisme": "",
    "baixaOrganisme": null,
    "baixaDomiciliOrganisme": null,
    "baixaRelacioDomiciliOrganisme": null,
    "resorgcontac": null,
    "resorgfcontac": null,
    "perscontacn": null,
    "persnumconordre": null,
    "plataforma": null,
    "notSms": null,
    "notMail": null,
    "persndMail": null,
    "fcontactMail": null,
    "numconordreMail": null,
    "persndSms": null,
    "fcontactSms": null,
    "numconordreSms": null,
    "stddgr": "20200217",
    "stddmod": "20200416",
    "stdhgr": "104732",
    "stdhmod": "093823",
    "stdugr": "G5Admin",
    "stdumod": "G5Admin",
    "persNot": "I",
    "domNot": "I",
    "swAreaDepKO": 0,
    "areaDepKODesc": null,
    "swTractat": 0,
    "nifPersSw": "F",
    "nifReprSw": null,
    "tramcod": null,
    "tramdesc": null,
    "efactComptabilitat": "",
    "efactEstat": "",
    "efactObs": "",
    "efactProveidor": ""
}

```

### 3.5.6 Cercar Registres.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/search	|

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "areacoddes": "string",
    "cercaDadesFoto": true,
    "codiAreaAssumpte": "string",
    "codiAssumpte": "string",
    "codiDepartamentAssumpte": "string",
    "codiEntitat": "string",
    "codiExtracte": "string",
    "codiIdioma": "string",
    "codiOrganismeOrigen": 0,
    "codiPersonaInteressada": 0,
    "codiRepresentant": 0,
    "codiTerritori": 0,
    "cognom1Interessat": "string",
    "cognom1Representant": "string",
    "cognom2Interessat": "string",
    "cognom2Representant": "string",
    "comptDomiciliPersonaInteressada": 0,
    "contingutExtracte": "string",
    "copiarDownload4J": true,
    "copiarFileBase64": true,
    "dataDocumentFinal": "string",
    "dataDocumentInici": "string",
    "dataPresentacioFinal": "string",
    "dataPresentacioInici": "string",
    "dataRegistreFinal": "string",
    "dataRegistreInici": "string",
    "dataTransport": "string",
    "depcoddes": "string",
    "efactComptabilitat": "string",
    "efactEstat": "string",
    "efactNumFactura": "string",
    "entitats": [
        "string"
    ],
    "entorn": "string",
    "guidrebut": "string",
    "ignoreUsuariFiltreAdeaDept": true,
    "inclouInteressatsRelacionats": true,
    "inclouRepresentantsRelacionats": true,
    "maxResults": 0,
    "mitjaTransport": "string",
    "nifInteressat": "string",
    "nifRepresentant": "string",
    "nivell": "string",
    "noTractats": true,
    "nomInteressat": "string",
    "nomRepresentant": "string",
    "nomesMalClassificats": true,
    "numRelCertificatCorreus": "string",
    "numeroFormaContacte": 0,
    "numeroOrdreContacte": 0,
    "numeroRegistreEntitatFinal": "string",
    "numeroRegistreEntitatInici": "string",
    "numeroRegistreFinal": "string",
    "numeroRegistreInici": "string",
    "numeroRegistreOrganisme": "string",
    "numeroRegistreRelacionat": "string",
    "observacionsTransport": "string",
    "perscontacn": "string",
    "persnumconordre": "string",
    "resNumeExt": "string",
    "senseDocumentsAnnexats": true,
    "senseDocumentsRelacionats": true,
    "senseRegMalClassificats": true,
    "stddgr": "string",
    "stddmod": "string",
    "stdugr": "string",
    "stdumod": "string",
    "tipusOrganisme": "string",
    "tots": true,
    "tractats": true,
    "urlServer": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registreentrada/search


Request body: {
    "aplicacio": "RES",
    "nivell": "9999",
    "usuari": "G5Admin",
    "numeroRegistreInici": "E2020000001",
    "numeroRegistreFinal": "E2020000010"
}


Expected response: {
    "numeroRegistreArray": [
        "E2020000010",
        "E2020000009",
        "E2020000008",
        "E2020000007",
        "E2020000006",
        "E2020000005",
        "E2020000004",
        "E2020000003",
        "E2020000002",
        "E2020000001"
    ]
}

```

### 3.5.7 Relacionar Expedient.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/expedient/put	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "resnume": "string",
    "sdenum": "string",
    "usuari": "string"
}
```

|Paràmetres| |
|----------|-|
| 	resnume                         |Codi de registre d’entrada|
| 	sdenum                         |Codi de l’expedient al que el relacionarem|

***Exemple:***

```json
Request:
POST /registreentrada/expedient/put


Request body: {
    "aplicacio": "RES",
    "nivell": "9999",
    "resnume": "X2020000011",
    "sdenum": "E2020000011",
    "usuari": "G5Admin"
}


Expected response: {
    "response": true
}
```

### 3.5.8 Recuperar Expedient relacionat.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/expedients/get	|	

***Cos de la petició***:

```json
{
    "numeroRegistreEntrada": "string",
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registreentrada/expedients/get


Request body: {
    "aplicacio": "RES",
    "nivell": "9999",
    "numeroRegistreEntrada": "E2020000011",
    "usuari": "G5Admin"
}


Expected response: {
    "numeroRegistreEntrada": "E2020000011",
    "listExpRel": [
        "X2020000011"
    ]
}

```

### 3.5.9 Assignar Interessat.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/interessat/put	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "codiPersona": 0,
    "codiRepresentant": 0,
    "comptDomiciliPersona": 0,
    "comptDomiciliRepresentant": 0,
    "domNot": "string",
    "gestCont": true,
    "idioma": 0,
    "nivell": "string",
    "notMail": "string",
    "notSms": "string",
    "numeroRegistre": "string",
    "persNot": "string",
    "perscontac": 0,
    "persfcontac": 0,
    "plataforma": "string",
    "repcontac": 0,
    "repfcontac": 0,
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registreentrada/interessat/put
Request body: {
    "aplicacio": "RES",
    "codiPersona": 11,
    "codiRepresentant": 0,
    "comptDomiciliPersona": 0,
    "comptDomiciliRepresentant": 0,
    "domNot": null,
    "gestCont": true,
    "nivell": "9999",
    "notMail": null,
    "notSms": null,
    "numeroRegistre": "E2020000011",
    "persNot": null,
    "perscontac": null,
    "persfcontac": null,
    "repcontac": null,
    "repfcontac": null,
    "usuari": "G5Admin"
}
```

### 3.5.10 Recuperar Documents relacionats

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/documents/get	|	

***Cos de la petició***:

```json
{
    "numeroRegistreEntrada": "E2020000001",
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}
```

***Exemple:***

```json
Request:
POST /registreentrada/documents/get


Request body: {
    "numeroRegistreEntrada": "E2020000001",
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "documentArray": [
        {
            "descriptor": "ACTA TEST 01012020",
            "nomDocument": "AnnexarDocSelenium.doc",
            "idOrigen": null,
            "codiTipusDocument": "T006",
            "descTipusDocument": "DOCUMENT ANNEXAT",
            "motiuRebut": "",
            "idNReg": 6020506,
            "guid": "9f32932e-0c2f-4761-8a30-cb19ce1b44ad"
        },
        {
            "descriptor": "ACCEPTACIÓ TEST 01012020",
            "nomDocument": "DOC2.Pdf",
            "idOrigen": null,
            "codiTipusDocument": "T001",
            "descTipusDocument": "DOCUMENT ANNEXAT",
            "motiuRebut": "",
            "idNReg": 6020124,
            "guid": "3813b39d-d40b-4afb-8166-4d86c101bf51"
        },
....
    ]
}

```


### 3.5.11 Recupera Entrades relacionades

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/entrades/get	|	

***Cos de la petició***:

```json
{
    "numeroRegistreEntrada": "string",
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registreentrada/entrada/get


Request body: {
    "numeroRegistreEntrada": "E2020000001",
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "numeroRegistreEntradaArray": [
        "string"
    ],
    "regRelArray": [
        {
            "numeroRegEntRel": "string",
            "numeroRegistreEntrada": "string",
            "stddgr": "string",
            "stddmod": "string",
            "stdhgr": "string",
            "stdhmod": "string",
            "stdugr": "string",
            "stdumod": "string"
        }
    ]
}

```

### 3.5.12 Recupera Sortides relacionades

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registreentrada/sortides/get	|	

***Cos de la petició***:

```json
{
    "mostrarRelExpedients": true,
    "numeroRegistreEntrada": "string",
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}

```

***Exemple:***

```json
Request:
POST /registreentrada/sortides/get
Request body: {
    "numeroRegistreEntrada": "E2020000001",
    "nivell": "9999",
    "aplicacio": "RES",
    "usuari": "G5Admin"
}


Expected response: {
    "numeroRegistreSortidaArray": [
        "string"
    ],
    "regRelArray": [
        {
            "numeroRegSortRel": "string",
            "numeroRegistreEntrada": "string",
            "stddgr": "string",
            "stddmod": "string",
            "stdhgr": "string",
            "stdhmod": "string",
            "stdugr": "string",
            "stdumod": "string"
        }
    ]
}

```

## 3.6 Registre de sortida

* ***Descripció*** - Operacions relacionades amb el registre de sortida. Creació i modificació de registres, obtenir registres relacionats, documents, relacionar registres, relacionar expedients, cercar i altres.

* ***Ruta*** -  /registresortida

### 3.6.1 Operacions

| Mètode | Ruta | Descripció |
| ------ | ---- | ---------- |                                                                   
|POST	|/put			|Crea o modifica un Registre de Sortida.                                      |
|POST	|/newsimple		|Crea un Registre de Sortida amb les dades mínimes necessàries.               |
|POST	|/putsimple		|Modifica dades concretes d’un Registre de Sortida.                           |
|POST	|/get			|FALTA!!!                                                                     |
|POST	|/search			|Cerca per diferents criteris.                                                |
|POST	|/interessat/put	|Assigna un Interessat a un Registre de Sortida.                              |
|POST	|/documents/get	|Recupera els Documents d’un Registre de Sortida.                             |
|POST	|/entrades/get	|Recupera els Registres d’Entrada relacionats.                                |
|POST	|/sortides/get	|Recupera els Registres de Sortida relacionats.                               |


### 3.6.2 Crear o modificar.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registresortida/put	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "entcod": "string",
    "gestCont": true,
    "idioma": 0,
    "nivell": "string",
    "nivellTaula": "string",
    "registreSortida": {
        "arxiu": "string",
        "avisRebutTransport": "string",
        "codiAreaAssumpte": "string",
        "codiAreaCreador": "string",
        "codiAssumpte": "string",
        "codiDepartamentAssumpte": "string",
        "codiDepartamentCreador": "string",
        "codiEntitat": "string",
        "codiExtracte": "string",
        "codiIdioma": "string",
        "codiOrganismeDesti": 0,
        "codiPersona": 0,
        "codiRepresentant": 0,
        "codiTerritori": 0,
        "codiTipusTransport": "string",
        "comptDomiciliOrganisme": 0,
        "comptDomiciliPersona": 0,
        "comptDomiciliRepresentant": 0,
        "contingutExtracte": "string",
        "dataDocument": "string",
        "dataPresentacio": "string",
        "dataTransport": "string",
        "descAreaAssumpte": "string",
        "descDepartamentAssumpte": "string",
        "domNot": "string",
        "fcontactMail": 0,
        "fcontactSms": 0,
        "firmaTopograficaArxiu": "string",
        "horaRegistre": "string",
        "horaTransport": "string",
        "ine10Origen": "string",
        "modificarAreaDeptartamentCreador": true,
        "notMail": "string",
        "notSms": "string",
        "numPagines": 0,
        "numconordreMail": 0,
        "numconordreSms": 0,
        "numeroFormaContacte": 0,
        "numeroOrdreContacte": 0,
        "numeroSortida": "string",
        "numeroSortidaEntitat": "string",
        "numeroTransport": "string",
        "observacionsDocument": "string",
        "observacionsTransport": "string",
        "persNifFoto": "string",
        "persNomFoto": "string",
        "persNot": "string",
        "perscontacn": "string",
        "persndMail": 0,
        "persndSms": 0,
        "persnumconordre": "string",
        "plataforma": "string",
        "registreRelacionat": "string",
        "repcontac": 0,
        "repfcontac": 0,
        "resNumsExt": "string",
        "resorgcontac": 0,
        "resorgfcontac": 0,
        "responsable": "string",
        "tipusOrganisme": "string",
        "tractat": 0
    },
    "usuari": "string",
    "validarCampsObligatoris": true
}
```

***Exemple:***

```json
Request:
POST /registresortida/put
Request body: {
    "aplicacio": "RES",
    "idioma": 0,
    "nivell": "9999",
    "registreSortida": {
        "codiAreaAssumpte": "AOC",
        "codiAreaCreador": "AOC",
        "codiAssumpte": "XXVV",
        "codiDepartamentAssumpte": "AOC",
        "codiDepartamentCreador": "AOC",
        "codiEntitat": "1",
        "codiPersona": 1,
        "codiRepresentant": 0,
        "codiTerritori": 0
    },
    "usuari": "G5Admin",
    "validarCampsObligatoris": false
}


Expected response: {
    "registreSortida": "S2021000001",
    "registreSortidaEntitat": "S2021000001",
    "dataRegistreSortida": "20210504",
    "horaRegistreSortida": "105848"
}

```

### 3.6.3 Creació simple.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registresortida/newsimple	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "areacodAssumpte": "string",
    "assumcod": "string",
    "codiTransport": "string",
    "contingut": "string",
    "depcodAssumpte": "string",
    "descripcioCurta": "string",
    "nivell": "string",
    "numeroTransport": "string",
    "representant": {
        "carcod": 0,
        "codiPostal": "string",
        "cognom1": "string",
        "cognom2": "string",
        "crearPersonaSiNoExisteix": true,
        "escala": "string",
        "mail": "string",
        "municipi": "string",
        "municod": 0,
        "nif": "string",
        "nom": "string",
        "nomCarrer": "string",
        "numero": "string",
        "pais": "string",
        "paiscod": 0,
        "passaport": "string",
        "pis": "string",
        "porta": "string",
        "provcod": 0,
        "provincia": "string",
        "telefon": "string"
    },
    "solicitant": {
        "carcod": 0,
        "codiPostal": "string",
        "cognom1": "string",
        "cognom2": "string",
        "crearPersonaSiNoExisteix": true,
        "escala": "string",
        "mail": "string",
        "municipi": "string",
        "municod": 0,
        "nif": "string",
        "nom": "string",
        "nomCarrer": "string",
        "numero": "string",
        "pais": "string",
        "paiscod": 0,
        "passaport": "string",
        "pis": "string",
        "porta": "string",
        "provcod": 0,
        "provincia": "string",
        "telefon": "string"
    },
    "transObs": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registresortida/newsimple


Request body: {
    "aplicacio": "RES",
    "assumcod": "XXVV",
    "descripcioCurta": "PROVA ALTA SIMPLE",
    "nivell": "9999",
    "solicitant": {
        "nif": "39918681L"
    },
    "usuari": "G5Admin"
}


Expected response: {
    "registreSortida": "S2021000002",
    "registreSortidaEntitat": "S2021000002",
    "dataRegistreSortida": "20210504",
    "horaRegistreSortida": "112309"
}

```

#### 3.6.4 Modificació simple.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registresortida/putsimple	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "entcod": "string",
    "gestCont": true,
    "idioma": 0,
    "nivell": "string",
    "nivellTaula": "string",
    "registreSortida": {
        "arxiu": "string",
        "avisRebutTransport": "string",
        "codiAreaAssumpte": "string",
        "codiAreaCreador": "string",
        "codiAssumpte": "string",
        "codiDepartamentAssumpte": "string",
        "codiDepartamentCreador": "string",
        "codiEntitat": "string",
        "codiExtracte": "string",
        "codiIdioma": "string",
        "codiOrganismeDesti": 0,
        "codiPersona": 0,
        "codiRepresentant": 0,
        "codiTerritori": 0,
        "codiTipusTransport": "string",
        "comptDomiciliOrganisme": 0,
        "comptDomiciliPersona": 0,
        "comptDomiciliRepresentant": 0,
        "contingutExtracte": "string",
        "dataDocument": "string",
        "dataPresentacio": "string",
        "dataTransport": "string",
        "descAreaAssumpte": "string",
        "descDepartamentAssumpte": "string",
        "domNot": "string",
        "fcontactMail": 0,
        "fcontactSms": 0,
        "firmaTopograficaArxiu": "string",
        "horaRegistre": "string",
        "horaTransport": "string",
        "ine10Origen": "string",
        "modificarAreaDeptartamentCreador": true,
        "notMail": "string",
        "notSms": "string",
        "numPagines": 0,
        "numconordreMail": 0,
        "numconordreSms": 0,
        "numeroFormaContacte": 0,
        "numeroOrdreContacte": 0,
        "numeroSortida": "string",
        "numeroSortidaEntitat": "string",
        "numeroTransport": "string",
        "observacionsDocument": "string",
        "observacionsTransport": "string",
        "persNifFoto": "string",
        "persNomFoto": "string",
        "persNot": "string",
        "perscontacn": "string",
        "persndMail": 0,
        "persndSms": 0,
        "persnumconordre": "string",
        "plataforma": "string",
        "registreRelacionat": "string",
        "repcontac": 0,
        "repfcontac": 0,
        "resNumsExt": "string",
        "resorgcontac": 0,
        "resorgfcontac": 0,
        "responsable": "string",
        "tipusOrganisme": "string",
        "tractat": 0
    },
    "usuari": "string",
    "validarCampsObligatoris": true
}
```

***Exemple:***

```json
Request:
POST /registresortida/putsimple


Request body: {
    "aplicacio": "RES",
    "nivell": "9999",
    "registreSortida": {
        "numeroSortida": "S2021000001",
        "observacionsDocument": "PROVA MODIFICACIO SIMPLE"
    },
    "usuari": "G5Admin"
}


Expected response: {
    "registreSortida": "S2021000001",
    "registreSortidaEntitat": "S2021000001",
    "dataRegistreSortida": "20210504",
    "horaRegistreSortida": "105848"
}

```

### 3.6.5 Cercar Registres.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registresortida/get	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "idioma": 0,
    "nivell": "string",
    "numeroRegistreSortida": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registresortida/get


Request body: {
    "aplicacio": "RES",
    "nivell": "9999",
    "numeroRegistreSortida": "S2020000001",
    "usuari": "G5Admin"
}


Expected response: {
    "numeroRegistre": "S2020000001",
    "numeroRegistreEntitat": "S2020000001",
    "codiEntitat": "1",
    "codiPersona": 1,
    "fotoRepresentant": {
        "nom": "JOSEP",
        "nif": "39918681L",
        "particula1": "",
        "cognom1": "ESPELTA",
        "particula2": "",
        "cognom2": "PRIME",
        "domicili": null,
        "esPersonaFisica": null
    },
    "fotoPersona": {
        "nom": "JOSE FRANCISCO",
        "nif": "76194401R",
        "particula1": "",
        "cognom1": "GALLARDO",
        "particula2": "",
        "cognom2": "RUIZ",
        "domicili": null,
        "esPersonaFisica": null
    },
    "descExtracte": "0001 PRESENTACIO SOL·LICITUD",
    "codiAssumpte": "XXVV",
    "numeroFormaContacte": null,
    "numeroOrdreContacte": null,
    "codiRepresentant": 8,
    "repcontac": null,
    "repfcontac": null,
    "repcontacdesc": null,
    "repfcontacdesc": null,
    "codiExtracte": "0001",
    "contingutExtracte": "wertewrt",
    "codiIdioma": null,
    "fotoAssumpte": {
        "codiArea": "AOC",
        "descArea": "AOC",
        "codiDepartament": "AOC",
        "descDepartament": "AOC",
        "descAssumpte": "Assumpte de proves XE"
    },
    "comptadorDomiciliPersona": 2,
    "codiDomiciliPersona": 108619,
    "baixaPersona": "0",
    "baixaDomiciliPersona": "0",
    "baixaRelacioDomiciliPersona": null,
    "fotoDomiciliPersona": "CL CASTLA N.0011 Esc.1 Pis.03 Pta.01 CAMBRILS (TARRAGONA) - ESPANYA",
    "comptadorDomiciliRepresentant": 1,
    "codiDomiciliRepresentant": null,
    "baixaRepresentant": "0",
    "baixaDomiciliRepresentant": null,
    "baixaRelacioDomiciliRepresentant": null,
    "fotoDomiciliRepresentant": "CL CASTLA N.0007 CP 43850 CAMBRILS (TARRAGONA) - ESPANYA",
    "dataCreacio": "20200828",
    "horaCreacio": "101323",
    "codiAreaCreador": "AOC",
    "descAreaCreador": "AOC",
    "codiDepartamentCreador": "AOC",
    "descDepartamentCreador": "AOC",
    "dataDocument": "20200828",
    "descIdioma": null,
    "numPagines": null,
    "observacionsDocument": "",
    "comptadorDomiciliOrganisme": null,
    "codiDomiciliOrganisme": null,
    "nomComplertOrganisme": null,
    "nifDcOrganisme": null,
    "domiciliOrganisme": null,
    "codiTerritori": null,
    "descTerritori": null,
    "codiTipusTransport": null,
    "descTipusTransport": null,
    "numeroTransport": "",
    "dataTransport": "",
    "observacionsTransport": "",
    "arxiu": "",
    "firmaTopograficaArxiu": "",
    "descTipusContacte": null,
    "descContacte": null,
    "codiOrganismeDesti": null,
    "avisRebutTransport": "",
    "resNumsExt": "",
    "interessats": [],
    "registreTancat": true,
    "registreRelacionat": "E2019000031",
    "registreRelacionatEntitat": "E2019000024",
    "tipusOrganisme": "",
    "baixaOrganisme": null,
    "baixaDomiciliOrganisme": null,
    "baixaRelacioDomiciliOrganisme": null,
    "resorgcontac": null,
    "resorgfcontac": null,
    "perscontacn": null,
    "persnumconordre": null,
    "plataforma": null,
    "notSms": null,
    "notMail": null,
    "stddgr": "20200828",
    "stddmod": "20200828",
    "stdhgr": "101324",
    "stdhmod": "101324",
    "stdugr": "G5Admin",
    "stdumod": "G5Admin",
    "persNot": null,
    "domNot": null,
    "persndMail": null,
    "fcontactMail": null,
    "numconordreMail": null,
    "persndSms": null,
    "fcontactSms": null,
    "numconordreSms": null,
    "persSw": "F",
    "reprSw": null,
    "swTractat": null,
    "responsable": null,
    "dataPresentacio": null,
    "swDigilencia": null,
    "swDigilenciaDesc": null
}

```

### 3.6.6 Assignar Interessat.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registresortida/search	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "areacoddes": "string",
    "cercaDadesFoto": true,
    "codiAreaAssumpte": "string",
    "codiAssumpte": "string",
    "codiDepartamentAssumpte": "string",
    "codiEntitat": "string",
    "codiExtracte": "string",
    "codiIdioma": "string",
    "codiOrganismeDesti": 0,
    "codiPersonaInteressada": 0,
    "codiRepresentant": 0,
    "codiTerritori": 0,
    "cognom1Interessat": "string",
    "cognom1Representant": "string",
    "cognom2Interessat": "string",
    "cognom2Representant": "string",
    "comptDomiciliPersonaInteressada": 0,
    "contingutExtracte": "string",
    "dataDocumentFinal": "string",
    "dataDocumentInici": "string",
    "dataRegistreFinal": "string",
    "dataRegistreInici": "string",
    "dataTransport": "string",
    "depcoddes": "string",
    "entitats": [
        "string"
    ],
    "entorn": "string",
    "guidrebut": "string",
    "ignoreUsuariFiltreAdeaDept": true,
    "inclouInteressatsRelacionats": true,
    "inclouRepresentantsRelacionats": true,
    "maxResults": 0,
    "mitjaTransport": "string",
    "nifInteressat": "string",
    "nifRepresentant": "string",
    "nivell": "string",
    "noTractats": true,
    "nomInteressat": "string",
    "nomRepresentant": "string",
    "numRelCertificatCorreus": "string",
    "numeroFormaContacte": 0,
    "numeroOrdreContacte": 0,
    "numeroRegistreEntitatFinal": "string",
    "numeroRegistreEntitatInici": "string",
    "numeroRegistreFinal": "string",
    "numeroRegistreInici": "string",
    "numeroRegistreRelacionat": "string",
    "observacionsTransport": "string",
    "perscontacn": "string",
    "persnumconordre": "string",
    "resNumsExt": "string",
    "senseDocumentsAnnexats": true,
    "stddgr": "string",
    "stddmod": "string",
    "stdugr": "string",
    "stdumod": "string",
    "tipusOrganisme": "string",
    "tots": true,
    "tractats": true,
    "urlServer": "string",
    "usuari": "string",
    "usuariFiltreAdeaDept": "string"
}
```

***Exemple:***

```json
Request:
POST /registresortida/search


Request body: {
    "aplicacio": "RES",
    "codiPersonaInteressada": 1,
    "dataDocumentFinal": "20201231",
    "dataDocumentInici": "20200101",
    "nivell": "9999",
    "usuari": "G5Admin"
}


Expected response: {
    "numeroRegistreArray": [
        "S2020000001"
    ]
}

```

### 3.6.7 Recuperar Documents relacionats.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registresortida/interessat/put	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "codiPersona": 0,
    "codiRepresentant": 0,
    "comptDomiciliPersona": 0,
    "comptDomiciliRepresentant": 0,
    "domNot": "string",
    "gestCont": true,
    "idioma": 0,
    "nivell": "string",
    "notMail": "string",
    "notSms": "string",
    "numeroRegistre": "string",
    "persNot": "string",
    "perscontac": 0,
    "persfcontac": 0,
    "plataforma": "string",
    "repcontac": 0,
    "repfcontac": 0,
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registresortida/interessat/put


Request body: {
    "aplicacio": "RES",
    "codiPersona": 11,
    "codiRepresentant": 0,
    "comptDomiciliPersona": 0,
    "comptDomiciliRepresentant": 0,
    "domNot": null,
    "gestCont": true,
    "nivell": "9999",
    "notMail": null,
    "notSms": null,
    "numeroRegistre": "S2020000001",
    "persNot": null,
    "perscontac": null,
    "persfcontac": null,
    "repcontac": null,
    "repfcontac": null,
    "usuari": "G5Admin"
}


Expected response: {
    "response": true
}

```

# 3.6.8	Recuperar Entrades relacionades.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registresortida/documents/get	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "numeroRegistreSortida": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registresortida/documents/get


Request body: {
    "aplicacio": "RES",
    "nivell": "9999",
    "numeroRegistreSortida": "S2020000003",
    "usuari": "G5Admin"
}


Expected response: {
    "documentArray": [
        {
            "descriptor": "Document Test Selenium",
            "nomDocument": "AnnexarDocSelenium.pdf",
            "idOrigen": null,
            "codiTipusDocument": "",
            "descTipusDocument": "DOCUMENT ANNEXAT",
            "motiuRebut": "",
            "idNReg": 214883,
            "guid": null
        }
    ]
}
```

### 3.6.9 Recuperar Sortides relacionades. 

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registresortida/entrades/get	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "numeroRegistreSortida": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registresortida/entrades/get


Request body: {
    "aplicacio": "RES",
    "nivell": "9999",
    "numeroRegistreSortida": "S2020000002",
    "usuari": "G5Admin"
}


Expected response: {
    "numeroRegistreSortidaArray": [],
    "regRelArray": []
}

```


### 3.6.10 Recuperar Sortides relacionades. 

| Mètode | Ruta |
| ------ | ---- | 
|POST | /registresortida/sortides/get	|	

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "mostrarRelExpedients": true,
    "nivell": "string",
    "numeroRegistreSortida": "string",
    "usuari": "string"
}
```

***Exemple:***

```json
Request:
POST /registresortida/sortides/get


Request body: {
    "aplicacio": "RES",
    "mostrarRelExpedients": true,
    "nivell": "9999",
    "numeroRegistreSortida": "S2020000254",
    "usuari": "G5Admin"
}


Expected response: {
    "numeroRegistreEntradaArray": [
        "E2020000473"
    ],
    "regRelArray": [
        {
            "numeroRegistreSortida": "S2020000254",
            "numeroRegEntRel": "E2020000473",
            "stddgr": "20201108",
            "stddmod": "20201108",
            "stdhgr": "194708",
            "stdhmod": "194708",
            "stdugr": "G5Admin",
            "stdumod": "G5Admin"
        }
    ]
}
```

## 3.7	Expedients

* ***Descripció*** - Operacions relacionades amb expedients.

* ***Ruta*** -  /expedients

### 3.7.1 Operacions

| Mètode | Ruta | Descripció |
| ------ | ---- | ---------- |                                                                   
|POST	|/newsimple				|Crea un Expedient amb les dades mínimes necessàries.             |
|POST	|/putsimple				|Modifica dades concretes d’un Expedient.                         |
|POST	|/get					|Recupera un Expedient.                                           |
|POST	|/infoadicional/{sdenum}|Modificar o afegir dades addicionals d’un Expedient.             |
|DELETE	|/infoadicional/{sdenum}|Esborrar dades addicionals d’un Expedient                        |
|GET		|/infoadicional/{sdenum}|Obtenir les dades addicionals d’un expedient.                    |


### 3.7.2 Creació simple

| Mètode | Ruta |
| ------ | ---- | 
|POST | /expedients/newsimple	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "entcod": "string",
    "texpcod": "string",
    "perscod": 0,
    "sdenum": "string",
    "data": "2020-05-20T13:43:05.126Z"
}
```
 
|Paràmetres| |
|----------|-|
|entcod	|Identificador de l’entitat. Si només en tenim una utilitzar el valor 1.              |
|texpcod	|Identificador de tipus d’Expedient.                                              |
|perscod	|Codi de Persona.                                                                 |
|sdenum	|Codi d’Expedient. Permet utilitzar les variables d’un altre Expedient.               |
|data	|Data de l’Expedient. Per omisió la data actual.                                      |

***Exemple:***

```json
POST /expedients/newsimple
Cos de la petició: {
    "nivell": "9999",
    "aplicacio": "SDE",
    "usuari": "G5Admin",
    "entcod": "1",
    "texpcod": "GENE",
    "perscod": 1,
    "sdenum": "",
    "data": ""
}

Resposta esperada: {
    "sdenum": "X2020000759"
}

```


## 3.7.3 Modificació simple

| Mètode | Ruta |
| ------ | ---- | 
|POST | /expedients/putsimple	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "sdenum": "string",
    "perscod": 0,
    "persnd": 0,
    "reprcod": 0,
    "reprnd": 0,
    "areacod": "string",
    "depcod": "string",
    "grupcod": "string",
    "rol": "string",
    "entcod": "string",
    "texpcod": "string",
    "decnumreg": "string",
    "extrcod": "string",
    "sdetext": "string",
    "arxsigtop": "string",
    "identval": "string",
    "idiomacod": "string",
    "eventexpr": "string",
    "sdenumtexp": "string",
    "sdedomcod": 0,
    "sderel": "string",
    "resracodec": "string",
    "resrorg": "string",
    "sdedreg": "string",
    "sdehreg": "string",
    "resrdata": "string",
    "connum2": 0,
    "treccod": "string",
    "assumcod": "string",
    "subassumcod": "string",
    "sdenumcont": "string",
    "fcontacn": 0,
    "numconordre": 0,
    "tipocod": "string",
    "licitacion": "string",
    "descassumpte": "string",
    "descsubassumpte": "string",
    "transcod": "string",
    "transcodarea": "string",
    "assumcodorg": "string",
    "subassumcodorg": "string",
    "plataforma": "string",
    "sdenument": "string",
    "seccod": "string",
    "subseccod": "string",
    "sercod": "string",
    "subsercod": "string
}
```

|Paràmetres| |
|----------|-|
|sdenum		|Codi d’Expedient. Obligatori.                               |
|percod		|Codi de Persona.                                            |
|persnd		|Codi de Domicili de la Persona.                             |
|reprcod		|Codi de Representant.                                       |
|reprnd		|Codi de Domicili del Representant.                          |
|areacod		|Cod d’Àrea.                                                 |
|depcod		|Codi de Departament.                                        |
|grupcod		|Codi de Grup de Treball.                                    |
|rol			|Codi de Rol.                                                |
|entcod		|Codi Entitat. Per omissió: ‘1’                              |
|texpcod		|Codi del tipus d’expedient                                  |
|decnumreg	|Número decret vinculat.                                     |
|extrcod		|Codi Extracte                                               |
|sdetext		|Text de l’expedient                                         |
|arxsigtop	|Arxiu: signatura topogràfica                                |
|identval	|Valor identificador. Camp lliure                            |
|idiomacod	|Codi d’Idioma.                                              |
|eventexpr	|Valor intern ja obsolet.                                    |
|sdenumtexp	|Identificador a nivell de tipus d’expedient                 |
|sdedomcod	|Domicili vinculat a l’expedient                             |
|sderel		|Número d’expedient relacionat                               |
|resracodec	|                                                            |
|resrorg	    |                                                            |
|sdedreg		|Data alta expedient                                         |
|sdehreg		|Hora alta expedient                                         |
|resrdata	|                                                            |
|connum2	    |                                                            |
|treccod		|Tipus de recurs.                                            |
|assumcod	|Codi d’Assumpte.                                            |
|subassumcod	|Codi de Subassumpte.                                        |
|sdenumcont	|                                                            |
|fcontacn	|Forma de contacte de l’interessat principal                 |
|numconordre	|Número de forma de contacte de l’interessa principal.       |
|tipcod	    |                                                            |
|licitacion	|	Valor obsolet.                                          |
|descassumpte	|Descripció de l’assumpte                                |
|descsubassumpte	|Descripció del subassumpte                              |
|transcod		|Tipus de transport                                      |
|transcodarea	|Àrea de transport                                       |
|assumcodorg	    |                                                        |
|subassumcodorg	|                                                        |
|plataforma		|Plataforma de notificació.                              |
|sdenument		|Número d’expedient a nivell d’entitat                   |
|seccod			|Codi Secció (classificació documental)                  |
|subseccod		|Codi SubSecció (classificació documental)               |
|sercod			|Codi Sèrie (classificació documental)                   |
|subsercod		|Codi SubSerie (classificacio documental)                |
|tancamentData	|Data tancament                                          |
|tancamentHora	|Hora tancament                                          |
|tancamentMotiu	|Motiu tancament.                                        |
|tancamentObs	|Descripcio motiu tancament                              |
|texpcod	Tipus 	|Expedient                                               |
|texpdesc		|Descripció del Tipus Expedient                          |


En el cos de la petició s’indiquen totes les propietats possibles.
En la petició només cal indicar aquelles propietats que volem modificar, la resta quedaran inalterades.

Excepcions:

* Per canviar el Domicili cal enviar perscod i persnd.
* Per canviar el Domicili del Representant cal enviar reprcod i reprnd.
* Per canviar Àrea, Departament o Grup, cal enviar les tres dades areacod, depcod i grupcod.
* Per canviar el contacte cal enviar perscod, persnd, numconorde i fcontactn.

***Exemple:***

```json
Request:
POST /expedients/putsimple


Request body: {
    "nivell": "9999",
    "aplicacio": "SDE",
    "usuari": "G5Admin",
    "sdenum": "X2020000001",
    "perscod": 1,
    "persnd": 1,
    "sdetext": "Prova de test"
}


Expected response: {
    "response": true
}

```


### 3.7.4 Recuperar un Expedient

| Mètode | Ruta |
| ------ | ---- | 
|POST | /expedients/get	|	

***Cos de la petició***:

```json
{
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string",
    "idioma": 0,
    "nifPersona": "string",
    "numeroExpedient": "string"
}
```

|Paràmetres| |
|----------|-|
|idioma			|Codi d’idioma.                     |
|nifPersona		|NIF de la Persona.                 |
|numeroExpedient	|Codi d’expedient a recuperar.      |

***Exemple:***

```json
Request:
POST /expedients/get


Request body: {
    "numeroExpedient": "X2020000001",
    "nivell": "string",
    "aplicacio": "string",
    "usuari": "string"
}


Expected response body: {
    "perscod": 1,
    "entcod": "1",
    "fotoRepresentant": null,
    "fotoPersona": {
        "nom": "AJUNTAMENT DE PROVES",
        "nif": "P0826900C",
        "particula1": null,
        "cognom1": "",
        "particula2": null,
        "cognom2": "",
        "domicili": "C/ ALMERIA N.0005 Pis.02 CP 17220 SANT FELIU DE GUIXOLS (GIRONA) - ESPAÑA",
        "esPersonaFisica": null
    },
    "sdenum": "X2020000001",
    "texpcod": "GENE",
    "texpdesc": "PROCEDIMENT GENÈRIC",
    "decnumreg": "",
    "notes": null,
    "format": 0,
    "formatDesc": "Suport electrònic",
    "extrcod": "",
    "sdetext": "Prova de test",
    "arxsigtop": "",
    "identval": "",
    "idiomacod": "0",
    "eventexpr": "",
    "stdugr": "G5Admin",
    "stdumod": "",
    "persnd": 1,
    "reprcod": null,
    "sdenumtexp": "2480-000001-2020",
    "reprnd": null,
    "sdedomcod": null,
    "sderel": "",
    "resracodec": "",
    "resrorg": "",
    "stddgr": "20200101",
    "stdhgr": "203358",
    "stddmod": "",
    "stdhmod": "",
    "sdedreg": "20200101",
    "sdehreg": "203358",
    "resrdata": "",
    "connum2": 0,
    "treccod": "",
    "areacod": "AL",
    "depcod": "G5AD",
    "assumcod": "",
    "subassumcod": "",
    "tancamentData": "20200702",
    "tancamentHora": "125800",
    "tancamentMotiu": "Moti",
    "tancamentObs": "Tancament expedient via REST",
    "sdenumcont": "",
    "fcontacn": null,
    "numconordre": null,
    "tipocod": "",
    "licitacion": "",
    "descassumpte": "",
    "descsubassumpte": "",
    "transcod": "",
    "transcodarea": "",
    "assumcodorg": "",
    "subassumcodorg": "",
    "sdeExpedientdoms": {
        "domiciliArray": []
    },
    "resEntrelexps": {
        "registreArray": []
    },
    "estats": {
        "estatArray": []
    },
    "descTipusExpedient": "AUTORITZACIONS I CONCESSIONS",
    "descExtracte": null,
    "resSortidaRels": {
        "registreSortidaArray": []
    },
    "descPersona": ", AJUNTAMENT DE PROVES",
    "descDomiciliPersona": "C/ ALMERIA N.0005 Pis.02 CP 17220 SANT FELIU DE GUIXOLS (GIRONA) - ESPAÑA",
    "descDomiciliSdeDomCod": null,
    "descRepresentant": null,
    "descDomiciliRepresentant": null,
    "nifPersona": "P0826900C",
    "nifRepresentant": null,
    "plataforma": "",
    "sdenument": "2480-000001-2020",
    "seccod": "02",
    "subseccod": "04",
    "sercod": "02",
    "subsercod": "",
    "baixaSW": 0,
    "baixaData": null,
    "baixaHora": null,
    "grupcod": "GRUP",
    "descEstat": "INFORME",
    "esRAE": true,
    "descDepResp": null
}

```
### 3.7.5 Modificar o afegir dades addicionals

| Mètode | Ruta | Paràmetre de ruta |
| ------ | ---- |  ----- |
|POST | /infoadicional/{sdenum}	|	 sdenum - De tipus string. Identificador de l’expedient. |

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "usuari": "string",
    "infoAddicional": [
        {
            "variable": "string",
            "valor": "string",
            "varLabel": "string"
        }
    ]
}
```

|Paràmetres| |
|----------|-|
|variable	|Codi de la dada addicional.            |
|valor		|Valor a guardar.                       |
|varLabel	|Etiqueta de la dada.                   |

***Exemple:***

```json


```

### 3.7.6 Esborrar dades addicionals

| Mètode | Ruta | Paràmetre de ruta |
| ------ | ---- |  ----- |
|DELETE | /infoadicional/{sdenum}	|	 sdenum - De tipus string. Identificador de l’expedient. |

***Cos de la petició***:

```json
[
    "string"
]
```

|Paràmetres| |
|----------|-|
|array	|Llista de variables a eliminar.            |

***Exemple:***

```json
Request:
DELETE /expedients/infoaddicional/X2020000010


Request body: [
    "DAT1"
]


Expected response: HTTP 200. Empty body.

```

### 3.7.7 Obtenir dades addicionals

| Mètode | Ruta | Paràmetre de ruta |
| ------ | ---- |  ----- |
|GET | /infoadicional/{sdenum}	|	 sdenum - De tipus string. Identificador de l’expedient. |

***Exemple:***

```json
Request:
GET /expedients/infoaddicional/X2020000010


Expected response: [
    {
        "variable": "DAT1",
        "valor": "D1",
        "varLabel": "Dada 1"
    }
]
```
 
## 3.8 Documents

* ***Descripció*** - Permet guardar i obtenir documents. Consultar i modificar la informació associada a un document.

* ***Ruta*** -  /documents

### 3.8.1 Operacions

| Mètode | Ruta | Descripció |
| ------ | ---- | ---------- |                                                                   
|POST		|/file					|Guardar un Document.                                                           |
|GET			|/file/{guid}			|Obtenir un Document.                                                           |
|POST		|/info/{guid}			|Modificar la informació associada a un Document.                               |
|GET			|/info/{guid}			|Obtenir la informació associada a un Document.                                 |
|POST		|/infoadicional/{idnreg}	|Modificar o afegir dades addicionals a un Document.                            |
|DELETE		|/infoadicional/{idnreg}	|Esborrar dades adicionals a un Document.                                       |
|GET			|/infobyidnreg/{idnreg}	|Obtenir la informació associada a un Document a partir del seu identificador.  |

 **Notes**: el codi `guid` es una cadena alfanumèrica única de 36 caràcters que identifica de forma segura un document. L’`idnreg` es un codi numèric intern.
 
### 3.8.2 Camps especials

Estat Elaboració

Els possibles valors són: 

•	COPIA_ELEC_AUTENTICA : Còpia electrònica autèntica
•	COPIA_ELEC_AUTENTICA_CANVI_FORMAT : Còpia electrònica autèntica amb canvi de format
•	COPIA_ELEC_AUTENTICA_PAPER : Còpia electrònica autèntica de document paper
•	COPIA_ELECT_PARCIAL_AUTENTICA : Còpia electrònica parcial autèntica
•	COPIA_PAPER_DOCUMENT_PAC : Còpia paper autèntica de document públic administratiu electrònic
•	ORIGINAL : Original
•	ALTRES : Altres

Naturalesa del Document

Els possibles valors són:

•	COPIA : Còpia 
•	COPIA_AUTENTICA : Còpia autèntica 
•	ORIGINAL : original


### 3.8.3 Guardar un Document

| Mètode | Ruta | Paràmetre |
| ------ | ---- | ----|
|POST | /documents/file	| file Arxiu a guardar.|

|Paràmetres| |
|----------|-|
|AnnexarDocumentReq	|Cadena que conté una objecte JSON amb la següent estructura: `{"aplicacio": "string","nivell": "string","usuari": "string""aplcod": "string","docorigen": "string","doccod": "string","modelcod": "string","docnompc": "string","descriptor": "string","doctip": "string","identificador": "string"}` |


***Exemple:***

```json
Request:
POST /document/file

Parameters:
file : arxiu a enviar 
AnnexarDocumentReq : {
    "nivell": "9999",
    "usuari": "G5Admin",
    "aplicacio": "SDE",
    "aplcod": "SDE",
    "docorigen": "REGENT",
    "doccod": "INST",
    "modelcod": "****",
    "docnompc": "test_doc.pdf",
    "descriptor": "prova",
    "doctip": "EXP",
    "identificador": "E2020000001"
}

Expected response: {
    "idnreg": 6020720,
    "guid": "4bf18bd4-c014-4bc9-8933-94520c225d6f"
}

```

### 3.8.3 Obtenir un Document


| Mètode | Ruta |Paràmetre de ruta|
| ------ | ---- | ---- |
|GET | //documents/file/{guid}	|  guid  De tipus string. Identificador únic del document. |

***Exemple:***

Utilitzarem el codi `guid` obtingut en l’exemple de guradar un document

```json
Request:
GET /documents/file/4bf18bd4-c014-4bc9-8933-94520c225d6f


Expected response: File download.

```

### 3.8.4 Modificar la informació associada

| Mètode | Ruta |Paràmetre de ruta|
| ------ | ---- | ---- |
|POST | /documents/file/{guid}	|  guid  De tipus string. Identificador únic del document. |

***Cos de la petició***:

```json
{
    "guid": "string",
    "doctip": "string",
    "doctitol": "string",
    "descriptor": "string",
    "observacions": "string",
    "estatsig": "string",
    "motiurebu": "string",
    "perscod": 0,
    "sdenum": "string",
    "persnd": 0,
    "vidsignerGuid": "string",
    "vidsignerEstat": 0,
    "cmisDocId": 0,
    "orgcod": "string",
    "dataCaducitat": "string",
    "infoAddicional": [
        {
            "variable": "string",
            "valor": "string",
            "varLabel": "string"
        }
    ]
}
```


En el cos de la peticó s’indiquen totes les propietats possibles.
En la petició només cal indicar aquelles propietats que volem modificar, la resta quedaran inalterades.

***Exemple:***

```json
Request:
POST /documents/info/4bf18bd4-c014-4bc9-8933-94520c225d6f


Request body: {
    "guid": "",
    "doctip": "EXP",
    "doctitol": "Títol del document",
    "descriptor": "Això és un descriptor",
    "observacions": "Algun comentari",
    "infoAddicional": [
        {
            "variable": "CAMP1",
            "valor": "C1",
            "varLabel": ""
        },
        {
            "variable": "CAMP2",
            "valor": "C2",
            "varLabel": ""
        }
    ]
}


Expected response: HTTP 200. Empty body.


```

### 3.8.5 Obtenir la informació associada

| Mètode | Ruta |Paràmetre de ruta|
| ------ | ---- | ---- |
|GET | /documents/file/{guid}	|  guid  De tipus string. Identificador únic del document. |

***Exemple:***

```json
Request:
GET /documents/info/4bf18bd4-c014-4bc9-8933-94520c225d6f


Expected response: {
    "aplcod": "SDE ",
    "docorigen": "REGENT ",
    "doccod": "INST",
    "modelcod": "****",
    "docnompc": "test_pdf_22.pdf",
    "identificador": "test_pdf_22.pdf",
    "docorigenDesc": "Registre Entrada",
    "doccodDesc": "Instància",
    "modelcodDesc": "Document annexat",
    "doctip": "EXP ",
    "idnreg": 6020720,
    "doctitol": "Títol del document",
    "descriptor": "Això és un descriptor",
    "observacions": "Algun comentari",
    "stdugr": "G5Admin	",
    "stdumod": "G5Admin		",
    "stddgr": "20200519",
    "stdhgr": "080429",
    "stddmod": "20200519",
    "stdhmod": "080429",
    "estatsig": null,
    "motiurebu": null,
    "guid": "4bf18bd4-c014-4bc9-8933-94520c225d6f",
    "eliminat": null,
    "perscod": null,
    "sdenum": null,
    "persnd": null,
    "vidsignerGuid": null,
    "vidsignerEstat": null,
    "cmisDocId": null,
    "orgcod": null,
    "dataSessio": null,
    "dataCaducitat": null,
    "swCciuAmagar": false,
    "infoAddicional": [
        {
            "variable": "CAMP1",
            "valor": "C1",
            "varLabel": null
        },
        {
            "variable": "CAMP2",
            "valor": "C2",
            "varLabel": null
        }
    ]
}


```

### 3.8.6 Modificar o afegir dades addicionals

| Mètode | Ruta |Paràmetre de ruta|
| ------ | ---- | ---- |
|POST | /documents/infoadicional/{idnreg}	|  idnreg  De tipus integer. Identificador únic del document. |

***Cos de la petició***:

```json
[
    {
        "variable": "string",
        "valor": "string",
        "varLabel": "string"
    }
]

```

***Exemple:***

```json
Request:
POST /documents/infoadicional/6020720


Request body: [
    {
        "variable": "VARX",
        "valor": "XX",
        "varLabel": ""
    }
]
Expected response: HTTP 200. Empty body.

```

### 3.8.7 Esborrar dades addicionals

| Mètode | Ruta |Paràmetre de ruta|
| ------ | ---- | ---- |
|DELETE | /documents/infoadicional/{idnreg}	|  idnreg  De tipus integer. Identificador únic del document. |

***Cos de la petició***:

```json
[
"string"
]

```
En el cos de la petició indicarem les variables a esborrar.

***Exemple:***

```json 
 Request:
DELETE /documents/infoadicional/6020720


Request body: [
    "CAMP2",
    "VARX"
]

Expected response: HTTP 200. Empty body.
```

### 3.8.8	Obtenir la informació associada a partir de l’identificador de Document.

| Mètode | Ruta |Paràmetre de ruta|
| ------ | ---- | ---- |
|GET | /documents/info/{idnreg}	|  idnreg  De tipus integer. Identificador únic del document. |


***Exemple:***

```json
Request:
GET /documents/infobyidnreg/6020720
Expected response body: {
    "aplcod": "SDE ",
    "docorigen": "REGENT ",
    "doccod": "INST",
    "modelcod": "****",
    "docnompc": "test_pdf_22.pdf",
    "identificador": "test_pdf_22.pdf",
    "docorigenDesc": "Registre Entrada",
    "doccodDesc": "Instància",
    "modelcodDesc": "Document annexat",
    "doctip": "EXP ",
    "idnreg": 6020720,
    "doctitol": "Títol del document",
    "descriptor": "Això és un descriptor",
    "observacions": "Algun comentari",
    "stdugr": "G5Admin",
    "stdumod": "G5Admin",
    "stddgr": "20200519",
    "stdhgr": "080429",
    "stddmod": "20200519",
    "stdhmod": "080429",
    "estatsig": null,
    "motiurebu": null,
    "guid": "4bf18bd4-c014-4bc9-8933-94520c225d6f",
    "eliminat": null,
    "perscod": null,
    "sdenum": null,
    "persnd": null,
    "vidsignerGuid": null,
    "vidsignerEstat": null,
    "cmisDocId": null,
    "orgcod": null,
    "dataSessio": null,
    "dataCaducitat": null,
    "swCciuAmagar": false,
    "infoAddicional": [
        {
            "variable": "CAMP1",
            "valor": "C1",
            "varLabel": null
        }
    ]
}


```

## 3.9 Organigrama


* ***Descripció*** - Permet	consultar i	actualitzar	la	informació de l’organigrama	(àrees, departaments, grups de treball i assumptes).

* ***Ruta*** -  /organigrama

### 3.9.1 Operacions

| Mètode | Ruta | Descripció |
| ------ | ---- | ---------- |                                                                   
|POST	|/arees									|Crea o modifica una àrea.                                                            |
|GET		|/arees								|	Recupera totes les àrees.                                                            |
|GET		|/arees/{areacod}					|	Recupera una àrea.                                                                   |
|POST	|/arees/delete							|Elimina una àrea.                                                                    |
|POST	|/arees/departaments					|	Crea o modifica un departament.                                                      |
|GET		|/arees/{areacod}/departaments		|	Recupera tots els departaments d’una àrea.                                           |
|GET		|/arees/{areacod}/departaments/{depcod}	|Recupera un departament.                                                             |
|POST	|/arees/departaments/delete				|Elimina un departament.                                                              |
|POST	|/arees/departaments/grupstreball		|Crea o modifica un grup de treball.                                                  |
|GET		|/arees/{areacod}/departaments/{depcod}/grupstreball			|	Recupera tots els grups de treball d’un departments.         |
|GET		|/arees/{areacod}/departaments/{depcod}/grupstreball/{grupcod}	|Recupera un grup de treball.                                 |
|POST	|/arees/departaments/grupstreball/delete							|Elimina un grup de treball.                                  |
|POST	|/assumptes				|Crea o modifica un assumpte.                                                                         |
|GET		|/assumptes				|Recupera tots els assumptes                                                                          |
|GET		|/assumptes/{assumcod}	|Recupera un assumpte.                                                                                |
|POST	/assumptes/delete		|Elimina un assumpte.                                                                                 |


### 3.9.2 Crea o modifica una àrea.


| Mètode | Ruta |
| ------ | ---- | 
|POST | /organigrama/arees	| 

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "usuari": "string",
    "areacod": "string",
    "areadesc": "string",
    "sdeprioritat": 0,
    "entcod": "string"
}

```
|Paràmetres| |
|----------|-|
|areacod	|Codi d’àrea.                   |
|areadesc	|Descripció d’area.             |
|sdeprioritat|	                            |
|entcod	     |                              |

***Exemple:***

```json
Request:
POST /organigrama/arees


Request body: {
    "aplicacio": "ORG",
    "nivell": "9999",
    "usuari": "G5Admin",
    "areacod": "TST",
    "areadesc": "Area de test"
}


Expected response: {
    "nivell": "9999",
    "aplicacio": "ORG",
    "usuari": "G5Admin",
    "areacod": "TST",
    "areadesc": "Area de test",
    "sdeprioritat": null,
    "entcod": null
}
```

###3.9.3 Recupera totes les àrees.

| Mètode | Ruta |
| ------ | ---- | 
|GET | /organigrama/arees	| 

***Exemple:***

```json
Request:
GET /organigrama/arees


Expected response: [
    {
        "areacod": "AL",
        "areadesc": "ALCALDIA",
        "audit": {
            "usuariCreacio": null,
            "usuariModificacio": null,
            "dataCreacio": null,
            "dataModificacio": null,
            "horaCreacio": null,
            "horaModificacio": null
        },
        "sdeprioritat": null,
        "baixasw": 0,
        "entcod": "1 "
    },
...
]


```

#### 3.9.4 Recupera una àrea.

| Mètode | Ruta |
| ------ | ---- | 
|GET | /organigrama/arees/{areacod}	| 

|Paràmetres| |
|----------|-|
|areacod	|Codi d’àrea.                   |

***Exemple:***

```json
Request:
GET /organigrama/arees/TST


Expected response: {
    "areacod": "string",
    "areadesc": "string",
    "audit": {
        "dataCreacio": "string",
        "dataModificacio": "string",
        "horaCreacio": "string",
        "horaModificacio": "string",
        "usuariCreacio": "string",
        "usuariModificacio": "string"
    },
    "baixasw": 0,
    "entcod": "string",
    "sdeprioritat": 0
}


```

### 3.9.5 Elimina una àrea.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /organigrama/arees/delete	| 

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "usuari": "string",
    "areacod": "string",
    "areadesc": "string",
    "sdeprioritat": 0,
    "entcod": "string"
}
```
 
|Paràmetres| |
|----------|-|
|areacod		|Codi d’àrea              |
|areadesc	|No s’utilitza.           |
|sdeprioritat|No s’utilitza.           |
|entcod		|No s’utilitza            |

***Exemple:***

```json
Request:
POST /organigrama/arees/delete


Request body: {
    "aplicacio": "ORG",
    "nivell": "9999",
    "usuari": "G5Admin",
    "areacod": "TST"
}


Expected response: {
    "timestamp": "2020-07-31T08:06:33.733+0000",
    "status": 200,
    "error": "OK",
    "message": "Área eliminada.",
    "path": "/JGenesysREST/api/organigrama/arees/delete"
}


```

### 3.9.6 Crea o modifica un departament.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /organigrama/arees/departaments	| 

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "usuari": "string",
    "areacod": "string",
    "depcod": "string",
    "depdesc": "string",
    "depdesc2": "string",
    "sdeprioritat": 0,
    "responsable": "string"
}
```

|Paràmetres| |
|----------|-|
|areacod		|Codi d’àrea.                              |
|depcod		|Codi de departament.                      |
|depdesc		|Descripció del departament.               |
|depdesc2	|Segona descripció del departament.        |
|sdeprioritat|	                                      |
|responsable	|	Responsable del departament.          |

***Exemple:***

```json
Request:
POST /organigrama/arees/departaments


Request body: {
    "aplicacio": "ORG",
    "nivell": "9999",
    "usuari": "G5Admin",
    "areacod": "TST",
    "depcod": "DT",
    "depdesc": "Departament de test",
    "depdesc2": "Dep. test"
}


Expected response: {
    "nivell": "9999",
    "aplicacio": "ORG",
    "usuari": "G5Admin",
    "areacod": "TST",
    "depcod": "DT",
    "depdesc": "Departament de test",
    "depdesc2": "Dep. test",
    "sdeprioritat": null,
    "responsable": null
}


```

### 3.9.7 Recupera tots els departaments d’una àrea.

| Mètode | Ruta |
| ------ | ---- | 
|GET | /organigrama/arees/{areacod}/departaments	| 

|Paràmetres| |
|----------|-|
|areacod		|Codi d’àrea.                              |

***Exemple:***

```json
Request:
GET /organigrama/arees/TST/departaments


Expected response: [
    {
        "id": {
            "areacod": "TST",
            "depcod": "DT"
        },
        "orgArea": {
            "areacod": "TST",
            "areadesc": "Area de test",
            "audit": {
                "usuariCreacio": "G5Admin",
                "usuariModificacio": "G5Admin",
                "dataCreacio": "20200731",
                "dataModificacio": "20200731",
                "horaCreacio": "101238",
                "horaModificacio": "101238"
            },
            "sdeprioritat": null,
            "baixasw": 0,
            "entcod": "	"
        },
        "depdesc": "Departament de test",
        "depdesc2": "Dep. test",
        "audit": {
            "usuariCreacio": "G5Admin",
            "usuariModificacio": "G5Admin",
            "dataCreacio": "20200731",
            "dataModificacio": "20200731",
            "horaCreacio": "103052",
            "horaModificacio": "103052"
        },
        "sdeprioritat": null,
        "baixasw": 0,
        "responsable": null
    },
    ...
]


```
 
### 3.9.8 Recupera un departament.

| Mètode | Ruta |
| ------ | ---- | 
|GET | /organigrama/arees/{areacod}/departaments/{depcod}	| 

|Paràmetres| |
|----------|-|
|areacod		|Codi d’àrea.                              |
|depcod		|Codi de departament.                      |

***Exemple:***

```json
Request:
GET /organigrama/arees/TST/departaments/DT


Expected response: {
    "id": {
        "areacod": "TST",
        "depcod": "DT"
    },
    "orgArea": {
        "areacod": "TST",
        "areadesc": "Area de test",
        "audit": {
            "usuariCreacio": "G5Admin",
            "usuariModificacio": "G5Admin",
            "dataCreacio": "20200731",
            "dataModificacio": "20200731",
            "horaCreacio": "101238",
            "horaModificacio": "101238"
        },
        "sdeprioritat": null,
        "baixasw": 0,
        "entcod": "	"
    },
    "depdesc": "Departament de test",
    "depdesc2": "Dep. test",
    "audit": {
        "usuariCreacio": "G5Admin",
        "usuariModificacio": "G5Admin",
        "dataCreacio": "20200731",
        "dataModificacio": "20200731",
        "horaCreacio": "103052",
        "horaModificacio": "103052"
    },
    "sdeprioritat": null,
    "baixasw": 0,
    "responsable": null
}
```

### 3.9.9 Elimina un departament.


| Mètode | Ruta |
| ------ | ---- | 
|POST | /organigrama/arees/departaments/delete	| 

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "usuari": "string",
    "areacod": "string",
    "depcod": "string",
    "depdesc": "string",
    "depdesc2": "string",
    "sdeprioritat": 0,
    "responsable": "string"
}
```
 
|Paràmetres| |
|----------|-|
|areacod		|Codi d’area.                 |
|epcod		|Codi de departament.         |
|epdesc		|No s’utilitza.               |
|epdesc2	|No s’utilitza.               |
|deprioritat|	No s’utilitza.            |
|esponsable	|No s’utilitza.               |

***Exemple:***

```json
Request:
POST /organigrama/arees/departaments/delete


Request body: {
    "aplicacio": "ORG",
    "nivell": "9999",
    "usuari": "G5Admin",
    "areacod": "TST",
    "depcod": "DT"
}


Expected response: {
    "timestamp": "2020-07-31T08:52:13.488+0000",
    "status": 200,
    "error": "OK",
    "message": "Departamento eliminado.",
    "path": "/JGenesysREST/api/organigrama/arees/departaments/delete"
}


```
 

### 3.9.10 Crea o modifica un grup de treball.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /organigrama/arees/departaments/grupstreball	| 

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "usuari": "string",
    "areacod": "string",
    "depcod": "string",
    "grupcod": "string",
    "grupdesc": "string"
}
```

|Paràmetres| |
|----------|-|                                         
|areacod	|Codi d’àrea.                              |
|depcod		|Codi de departament.                      |
|grupcod	|Codi de grup de treball.                  |
|grupdesc	|Descripció del grup de treball.           |

***Exemple:***

```json
Request:
POST /organigrama/arees/departaments/grupstreball


Request body: {
    "aplicacio": "ORG",
    "nivell": "9999",
    "usuari": "G5Admin",
    "areacod": "TST",
    "depcod": "DT",
    "grupcod": "GT",
    "grupdesc": "Grup de test"
}


Expected response: {
    "nivell": "9999",
    "aplicacio": "ORG",
    "usuari": "G5Admin",
    "areacod": "TST",
    "depcod": "DT",
    "grupcod": "GT",
    "grupdesc": "Grup de test"
}
```
 
### 3.9.11 Recupera tots els grups de treball d’un departments.

| Mètode | Ruta |
| ------ | ---- | 
|GET | /organigrama/arees/{areacod}/departaments/{depcod}/grupstreball	| 

***Exemple:***

```json
Request:
GET /organigrama/arees/TST/departaments/DT/grupstreball


Expected response: [
    {
        "id": {
            "areacod": "TST",
            "depcod": "DT",
            "grupcod": "GT"
        },
        "orgDepartament": {
            "id": {
                "areacod": "TST",
                "depcod": "DT"
            },
            "orgArea": {
                "areacod": "TST",
                "areadesc": "Area de test",
                "audit": {
                    "usuariCreacio": "G5Admin",
                    "usuariModificacio": "G5Admin",
                    "dataCreacio": "20200731",
                    "dataModificacio": "20200731",
                    "horaCreacio": "101238",
                    "horaModificacio": "101238"
                },
                "sdeprioritat": null,
                "baixasw": 0,
                "entcod": "	"
            },
            "depdesc": "Departament de test",
            "depdesc2": "Dep. test",
            "audit": {
                "usuariCreacio": "G5Admin",
                "usuariModificacio": "G5Admin",
                "dataCreacio": "20200731",
                "dataModificacio": "20200731",
                "horaCreacio": "105333",
                "horaModificacio": "105333"
            },
            "sdeprioritat": null,
            "baixasw": 0,
            "responsable": null
        },
        "grupdesc": "Grup de test",
        "audit": {
            "usuariCreacio": "G5Admin",
            "usuariModificacio": "G5Admin",
            "dataCreacio": "20200731",
            "dataModificacio": "20200731",
            "horaCreacio": "105947",
            "horaModificacio": "105947"
        },
        "baixasw": 0
    },
...
]
```


### 3.9.12 Recupera un grup de treball

| Mètode | Ruta |
| ------ | ---- | 
|GET | /organigrama/arees/{areacod}/departaments/{depcod}/grupstreball/{grupcod}| 

***Exemple:***

```json
Request:
GET /organigrama/arees/TST/departaments/DT/grupstreball/GT


Expected response: {
    "id": {
        "areacod": "TST",
        "depcod": "DT",
        "grupcod": "GT"
    },
    "orgDepartament": {
        "id": {
            "areacod": "TST",
            "depcod": "DT"
        },
        "orgArea": {
            "areacod": "TST",
            "areadesc": "Area de test",
            "audit": {
                "usuariCreacio": "G5Admin",
                "usuariModificacio": "G5Admin",
                "dataCreacio": "20200731",
                "dataModificacio": "20200731",
                "horaCreacio": "101238",
                "horaModificacio": "101238"
            },
            "sdeprioritat": null,
            "baixasw": 0,
            "entcod": "	"
        },
        "depdesc": "Departament de test",
        "depdesc2": "Dep. test",
        "audit": {
            "usuariCreacio": "G5Admin",
            "usuariModificacio": "G5Admin",
            "dataCreacio": "20200731",
            "dataModificacio": "20200731",
            "horaCreacio": "105333",
            "horaModificacio": "105333"
        },
        "sdeprioritat": null,
        "baixasw": 0,
        "responsable": null
    },
    "grupdesc": "Grup de test",
    "audit": {
        "usuariCreacio": "G5Admin",
        "usuariModificacio": "G5Admin",
        "dataCreacio": "20200731",
        "dataModificacio": "20200731",
        "horaCreacio": "105947",
        "horaModificacio": "105947"
    },
    "baixasw": 0
}
```

### 3.9.13 Elimina un grup de treball

| Mètode | Ruta |
| ------ | ---- | 
|POST | /organigrama/arees/departaments/grupstreball/delete	| 

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "usuari": "string",
    "areacod": "string",
    "depcod": "string",
    "grupcod": "string",
    "grupdesc": "string"
}
```

|Paràmetres| |
|----------|-|                                         
|areacod		|Codi d’area.                    |
|depcod		|Codi de departament.            |
|grupcod		|Codi de grup de treball.        |
|grupdesc	|No s’utilitza.                  |

***Exemple:***

```json
Request:
POST /organigrama/arees/departaments/grupstreball/delete


Request body: {
    "aplicacio": "ORG",
    "nivell": "9999",
    "usuari": "G5Admin",
    "areacod": "TST",
    "depcod": "DT",
    "grupcod": "GT"
}


Expected response: {
    "timestamp": "2020-07-31T09:14:27.871+0000",
    "status": 200,
    "error": "OK",
    "message": "Grupo de Trabajo eliminado.",
    "path": "/JGenesysREST/api/organigrama/arees/departaments/grupstreball/delete"
}
```

### 3.9.14 Crea o modifica un assumpte

| Mètode | Ruta |
| ------ | ---- | 
|POST | /organigrama/assumptes	| 

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "usuari": "string",
    "assumcod": "string",
    "depcod": "string",
    "areacod": "string",
    "assumdesc": "string"
}
```
|Paràmetres| |
|----------|-|                                         
|assumcod	|Codi d’assumpte.               |
|depcod		|Codi de departament.           |
|areacod	|Codi d’àrea |
|assumdesc	|Descripció de l’assumpte. |

***Exemple:***

```json
Request:
POST /organigrama/assumptes


Request body: {
    "aplicacio": "ORG",
    "nivell": "9999",
    "usuari": "G5Admin",
    "assumcod": "AST",
    "depcod": "DT",
    "areacod": "TST",
    "assumdesc": "Assumpte de test"
}


Expected response: {
    "nivell": "9999",
    "aplicacio": "ORG",
    "usuari": "G5Admin",
    "assumcod": "AST",
    "depcod": "DT",
    "areacod": "TST",
    "assumdesc": "Assumpte de test"
}
```

### 3.9.15 Recupera tots els assumptes

| Mètode | Ruta |
| ------ | ---- | 
| GET | /organigrama/assumptes| 
 
***Exemple:***

```json
Request:
GET /organigrama/assumptes


Expected response: [
    {
        "assumcod": "EDIC",
        "depcod": "G5AD",
        "areacod": "AL",
        "assumdesc": "EDICTE",
        "orgArea": {
            "areacod": "AL",
            "areadesc": "ALCALDIA",
            "audit": {
                "usuariCreacio": null,
                "usuariModificacio": null,
                "dataCreacio": null,
                "dataModificacio": null,
                "horaCreacio": null,
                "horaModificacio": null
            },
            "sdeprioritat": null,
            "baixasw": 0,
            "entcod": "1 "
        },
        "orgDepartament": {
            "id": {
                "areacod": "AL",
                "depcod": "G5AD"
            },
            "orgArea": {
                "areacod": "AL",
                "areadesc": "ALCALDIA",
                "audit": {
                    "usuariCreacio": null,
                    "usuariModificacio": null,
                    "dataCreacio": null,
                    "dataModificacio": null,
                    "horaCreacio": null,
                    "horaModificacio": null
                },
                "sdeprioritat": null,
                "baixasw": 0,
                "entcod": "1 "
            },
            "depdesc": "AUDIFILM",
            "depdesc2": null,
            "audit": {
                "usuariCreacio": null,
                "usuariModificacio": null,
                "dataCreacio": null,
                "dataModificacio": null,
                "horaCreacio": null,
                "horaModificacio": null
            },
            "sdeprioritat": null,
            "baixasw": 0,
            "responsable": null
        },
        "baixasw": 0,
        "regswa": null,
        "audit": {
            "usuariCreacio": null,
            "usuariModificacio": null,
            "dataCreacio": null,
            "dataModificacio": null,
            "horaCreacio": null,
            "horaModificacio": null
        }
    },
...
]
```


### 3.9.16 Recupera un assumpte.

| Mètode | Ruta |
| ------ | ---- | 
|GET | /organigrama/assumptes/{assumcod}	| 

|Paràmetres| |
|----------|-|                                         
|assumcod	|Codi d’assumpte.               |

***Exemple:***

```json
Request:
GET /organigrama/assumptes/AST


Expected response: {
    "assumcod": "AST",
    "depcod": "DT",
    "areacod": "TST",
    "assumdesc": "Assumpte de test",
    "orgArea": {
        "areacod": "TST",
        "areadesc": "Area de test",
        "audit": {
            "usuariCreacio": "G5Admin",
            "usuariModificacio": "G5Admin",
            "dataCreacio": "20200731",
            "dataModificacio": "20200731",
            "horaCreacio": "101238",
            "horaModificacio": "101238"
        },
        "sdeprioritat": null,
        "baixasw": 0,
        "entcod": "	"
    },
    "orgDepartament": {
        "id": {
            "areacod": "TST",
            "depcod": "DT"
        },
        "orgArea": {
            "areacod": "TST",
            "areadesc": "Area de test",
            "audit": {
                "usuariCreacio": "G5Admin",
                "usuariModificacio": "G5Admin",
                "dataCreacio": "20200731",
                "dataModificacio": "20200731",
                "horaCreacio": "101238",
                "horaModificacio": "101238"
            },
            "sdeprioritat": null,
            "baixasw": 0,
            "entcod": "	"
        },
        "depdesc": "Departament de test",
        "depdesc2": "Dep. test",
        "audit": {
            "usuariCreacio": "G5Admin",
            "usuariModificacio": "G5Admin",
            "dataCreacio": "20200731",
            "dataModificacio": "20200731",
            "horaCreacio": "105333",
            "horaModificacio": "105333"
        },
        "sdeprioritat": null,
        "baixasw": 0,
        "responsable": null
    },
    "baixasw": 0,
    "regswa": null,
    "audit": {
        "usuariCreacio": "G5Admin",
        "usuariModificacio": "G5Admin",
        "dataCreacio": "20200731",
        "dataModificacio": "20200731",
        "horaCreacio": "112107",
        "horaModificacio": "112107"
    }
}
```

### 3.9.17 Elimina un assumpte.

| Mètode | Ruta |
| ------ | ---- | 
|POST | /organigrama/assumptes/delete	| 

***Cos de la petició***:

```json
{
    "aplicacio": "string",
    "nivell": "string",
    "usuari": "string",
    "assumcod": "string",
    "depcod": "string",
    "areacod": "string",
    "assumdesc": "string"
}
```

|Paràmetres| |
|----------|-|                                         
|assumcod	|Codi d’assumpte.               |
|depcod	|No s’utilitza.|
|areacod	|No s’utilitza.|
|assumdesc	|No s’utilitza.|


***Exemple:***

```json
Request:
POST /organigrama/assumptes/delete


Request body: {
    "aplicacio": "ORG",
    "nivell": "9999",
    "usuari": "G5Admin",
    "assumcod": "AST"
}


Expected response: {
    "timestamp": "2020-07-31T09:35:07.784+0000",
    "status": 200,
    "error": "OK",
    "message": "Asunto eliminado.",
    "path": "/JGenesysREST/api/organigrama/assumptes/delete"
}
```
 
