# Hydra Collection Applied to DTS

## Introduction

I propose a slightly more fixed version of Hydra where the following would be always true for practicality :

- `title` are a list of dictionary where @lang and @value hold the information about the title. An optional `default` can state the default title to display.
- `description` is a list of dictionary where @lang and @value hold the information about the title.
- `metadata` holds DublinCore and Dublin Core Extended values
	- Questions :
		- are some DC and DCE keys required ?
		- what are the way to display there values
- `extended` holds any supplementary information provided by other ontologies/domains"
- "@context" should be 
	- extending Hydra
	- providing DC, DCT and DTS namespaces prefix

## Root collection

### Example of url : 

- `/api/dts/collections/`
- `/api/dts/collections/?id=general`

### Headers

### Response

```json
{
	"@context": "http://www.w3.org/ns/hydra/context.jsonld",
	"@id": "general",
	"@type": "Collection",
	"totalItems": "2",
	"title": [
		{"fre" : "Collection Générale de l'École Nationale des Chartes"}
	],
	"member": [
		{
			 "@id" : "cartulaires",
			 "title" : [
			 	{"@lang": "fre", "@value": "Cartulaires", "default": true},
			 	{"@lang": "unk", "@value": "Cartularia"},
			 ],
			 "description": [
			 	{"@lang": "fre", "@value": "Collection de cartulaires d'Île-de-France et de ses environs"}
			 ],
			 "@type" : "Collection",
			 "totalItems" : "10"
		}
		{
			 "@id" : "lasciva_roma",
			 "title" : [
			 	{"@lang": "lat", "@value": "Lasciva Roma", "default": true},
			 ],
			 "description": [{
			 	"eng": "Collection of primary sources of interest in the studies of Ancient World's sexuality" 
			 			 	}],
			 "@type" : "Collection",
			 "totalItems" : "1"
		}
	]
}
```

