# Distributed Text Services - Collection API

## Introduction

Here is the scheme for the current draft. Everything that is not marked as Optional is mandatory.

- `title` is a single string
- `@id` holds the identifier of the object
- `@type` should always be `Collection` or `Resource`
- `totalItems` represent the number of children hold by an object
- (Optional) `description` is a single description.
- (Optional) `dts:dublincore` holds Dublin Core Terms metadata
- (Optional) `dts:extensions` holds any supplementary information provided by other ontologies/domains
- (Optional) "@context" should be extending Hydra and providing DCT, TEI and DTS namespaces prefix
- (Optional) `dts:references` holds a links to the Navigation API route for current object (mandatory in children of `member` ?)
- (Optional) `dts:passage` holds a link to the Passage API for the current object
- (Optional) `dts:download` holds a link or a list of links to a downloadable format of the current object (*This may change in the future*)

### Note on Internationalization

Any internationalization of the title or the description should be written in `dts:dublincore` under `dct:title` and `dct:description`.

## Template

Here is a template of the URI for Collection API. The route itself (`/dts/api/collection/`) is up to the implementer.

```json
{
  "@context": "http://www.w3.org/ns/hydra/context.jsonld",
  "@type": "IriTemplate",
  "template": "/dts/api/collection/?id={collection_id}&page={page}",
  "variableRepresentation": "BasicRepresentation",
  "mapping": [
    {
      "@type": "IriTemplateMapping",
      "variable": "collection_id",
      "required": false
    },
    {
      "@type": "IriTemplateMapping",
      "variable": "page",
      "required": false
    }
  ]
}
```

## Examples

### Root collection

#### Example of url : 

- `/api/dts/collections/`
- `/api/dts/collections/?id=general`

#### Headers

| Key | Value | 
| --- | ----- |
| Content-Type | Content-Type: application/ld+json |

#### Response

```json
{
    "@context": {
        "@base": "http://www.w3.org/ns/hydra/context.jsonld",
        "dc": "http://purl.org/dc/terms/",
        "dts": "http://purl.org//dts-ontology/#",
        "tei": "http://www.tei-c.org/ns/1.0",
    },
    "@id": "general",
    "@type": "Collection",
    "totalItems": "2",
    "title": "Collection Générale de l'École Nationale des Chartes",
    "dts:dublincore": {
        "dc:publisher": ["École Nationale des Chartes", "https://viaf.org/viaf/167874585"],
        "dc:title": [
            {"fre" : "Collection Générale de l'École Nationale des Chartes"}
        ]
    },
    "member": [
        {
             "@id" : "cartulaires",
             "title" : "Cartulaires",
             "description": "Collection de cartulaires d'Île-de-France et de ses environs",
             "@type" : "Collection",
             "totalItems" : "10"
        },
        {
             "@id" : "lasciva_roma",
             "title" : "Lasciva Roma",
             "description": "Collection of primary sources of interest in the studies of Ancient World's sexuality",
             "@type" : "Collection",
             "totalItems" : "1"
        },
        {
             "@id" : "lettres_de_poilus",
             "title" : "Correspondance des poilus",
             "description": "Collection de lettres de poilus entre 1917 et 1918",
             "@type" : "Collection",
             "totalItems" : "10000"
        },
    ]
}
```


### Sub-collection, not readable but small

### Example of url : 

- `/api/dts/collections/?id=lasciva_roma`

### Headers

| Key | Value | 
| --- | ----- |
| Content-Type | Content-Type: application/ld+json |

### Response

```json
{
    "@context": {
        "@base": "http://www.w3.org/ns/hydra/context.jsonld",
        "dc": "http://purl.org/dc/terms/",
        "dts": "http://purl.org//dts-ontology/#",
        "tei": "http://www.tei-c.org/ns/1.0",
    },
    "@id": "lasciva_roma",
    "@type": "Collection",
    "totalItems": "2",
    "title" : "Lasciva Roma",
    "description": "Collection of primary sources of interest in the studies of Ancient World's sexuality",
    "dts:dublincore": {
        "dc:creator": [
            "Thibault Clérice", "http://orcid.org/0000-0003-1852-9204"
        ],
        "dc:title" : [
            {"@lang": "lat", "@value": "Lasciva Roma"},
        ],
        "dc:description": [
            {
                "@lang": "eng",
                "@value": "Collection of primary sources of interest in the studies of Ancient World's sexuality"
            }
        ],
    },
    "member": [
        {
            "@id" : "urn:cts:latinLit:phi1103.phi001",
            "title" : "Priapeia",
            "dts:dublincore": {
                "dc:type": [
                    "http://chs.harvard.edu/xmlns/cts#work"
                ],
                "dc:creator": [
                    {"@lang": "eng", "@value": "Anonymous"}
                ],
                "dc:language": ["lat", "eng"],
                "dc:description": [
                    { "@lang": "eng", "@value": "Anonymous lascivious Poems" }
                ],
            },
            "@type" : "Collection",
            "totalItems": 1
        }
    ]
}
```

### Sub-collection, not readable but with readable members

#### Note

Although, this is optional, the expansion of `@type:Resource`'s metadata is advised to avoid multiple API calls. 

#### Example of url : 

- `/api/dts/collections/?id=urn:cts:latinLit:phi1103.phi001`

#### Headers

| Key | Value | 
| --- | ----- |
| Content-Type | Content-Type: application/ld+json |

#### Response

```json
{
    "@context": {
        "@base": "http://www.w3.org/ns/hydra/context.jsonld",
        "dc": "http://purl.org/dc/terms/",
        "dts": "http://purl.org//dts-ontology/#",
        "tei": "http://www.tei-c.org/ns/1.0",
    },
    "@id": "urn:cts:latinLit:phi1103.phi001",
    "@type": "Collection",
    "title" : "Priapeia",
    "dts:dublincore": {
        "dc:type": ["http://chs.harvard.edu/xmlns/cts#work"],
        "dc:creator": [
            {"@lang": "eng", "@value": "Anonymous"}
        ],
        "dc:language": ["lat", "eng"],
        "dc:title": [{"@lang": "lat", "@value": "Priapeia"}],
        "dc:description": [{
           "@lang": "eng",
            "@value": "Anonymous lascivious Poems "
        }]
    },
    "totalItems" : "1",
    "member": [
        {
            "@id" : "urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
            "@type": "Resource",
            "title" : "Priapeia",
            "description": "Priapeia based on the edition of Aemilius Baehrens",
            "totalItems": 0,
            "dts:dublincore": {
                "dc:title": [{"@lang": "lat", "@value": "Priapeia"}],
                "dc:description": [{
                   "@lang": "eng",
                    "@value": "Anonymous lascivious Poems "
                }],
                "dc:type": [
                    "http://chs.harvard.edu/xmlns/cts#edition",
                    "dc:Text"
                ],
                "dc:source": ["https://archive.org/details/poetaelatinimino12baeh2"],
                "dct:dateCopyrighted": 1879,
                "dc:creator": [
                    {"@lang": "eng", "@value": "Anonymous"}
                ],
                "dc:contributor": ["Aemilius Baehrens"],
                "dc:language": ["lat", "eng"]
            },
            "dts:passage": "/api/dts/documents?id=urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
            "dts:references": "/api/dts/navigation?id=urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
            "dts:download": "https://raw.githubusercontent.com/lascivaroma/priapeia/master/data/phi1103/phi001/phi1103.phi001.lascivaroma-lat1.xml",
            "tei:refsDecl": [
                {
                    "tei:matchPattern":  "(\\w+)",
                    "tei:replacementPattern": "#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1'])",
                    "@type": "poem"
                },
                {
                    "tei:matchPattern":  "(\\w+)\\.(\\w+)",
                    "tei:replacementPattern": "#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1']//tei:l[@n='$2'])",
                    "@type": "line"
                }
            ]
        }
    ]
}
```

### Sub-collection readable

#### Example of url : 

- `/api/dts/collections/?id=urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1`

#### Headers

| Key | Value | 
| --- | ----- |
| Content-Type | Content-Type: application/ld+json |

#### Response

```json
{
    "@context": {
        "@base": "http://www.w3.org/ns/hydra/context.jsonld",
        "dct": "http://purl.org/dc/terms/",
        "dts": "http://purl.org//dts-ontology/#",
        "dc": "http://purl.org/dc/elements/1.1/",
        "tei": "http://www.tei-c.org/ns/1.0",
    },
    "@id": "urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
    "@type" : "Resource",
    "title" : "Priapeia",
    "description": "Priapeia based on the edition of Aemilius Baehrens",
    "totalItems": 0,
    "dts:dublincore": {
        "dc:title": [{"@lang": "lat", "@value": "Priapeia"}],
        "dc:description": [{
           "@lang": "eng",
            "@value": "Anonymous lascivious Poems "
        }],
        "dc:type": [
            "http://chs.harvard.edu/xmlns/cts#edition",
            "dc:Text"
        ],
        "dc:source": ["https://archive.org/details/poetaelatinimino12baeh2"],
        "dct:dateCopyrighted": 1879,
        "dc:creator": [
            {"@lang": "eng", "@value": "Anonymous"}
        ],
        "dc:contributor": ["Aemilius Baehrens"],
        "dc:language": ["lat", "eng"]
    },
    "dts:passage": "/api/dts/documents?id=urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
    "dts:references": "/api/dts/navigation?id=urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
    "dts:download": "https://raw.githubusercontent.com/lascivaroma/priapeia/master/data/phi1103/phi001/phi1103.phi001.lascivaroma-lat1.xml",
    "tei:refsDecl": [
        {
            "tei:matchPattern":  "(\\w+)",
            "tei:replacementPattern": "#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1'])",
            "@type": "poem"
        },
        {
            "tei:matchPattern":  "(\\w+)\\.(\\w+)",
            "tei:replacementPattern": "#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1']//tei:l[@n='$2'])",
            "@type": "line"
        }
    ]
}
```

### Paginated sub-collection

#### Example of url : 

- `/api/dts/collections/?id=lettres_de_poilus&page=19`

#### Headers

| Key | Value | 
| --- | ----- |
| Content-Type | Content-Type: application/ld+json |
| Link | </api/dts/collections/?id=lettres_de_poilus&page=1>; rel="first", </api/dts/collections/?id=lettres_de_poilus&page=18>; rel="previous", </api/dts/collections/?id=lettres_de_poilus&page=20>; rel="next", </api/dts/collections/?id=lettres_de_poilus&page=500>; rel="last" | 

#### Response

```json
{
    "@context": {
        "@base": "http://www.w3.org/ns/hydra/context.jsonld",
        "dct": "http://purl.org/dc/terms/",
        "dts": "http://purl.org//dts-ontology/#",
        "dc": "http://purl.org/dc/elements/1.1/",
        "tei": "http://www.tei-c.org/ns/1.0",
    },
    "@id": "general",
    "@type": "Collection",
    "totalItems": "2",
    "title": "Collection Générale de l'École Nationale des Chartes",
    "dts:dublincore": {
        "dc:publisher": ["École Nationale des Chartes", "https://viaf.org/viaf/167874585"],
        "dc:title": [
            {"fre" : "Collection Générale de l'École Nationale des Chartes"}
        ]
    },
    "@id" : "lettres_de_poilus",
    "@type" : "Collection",
    "totalItems" : "10000",
    "member": ["member 190 up to 200"],
    "view": {
        "@id": "/api/dts/collections/?id=lettres_de_poilus&page=19",
        "@type": "PartialCollectionView",
        "first": "/api/dts/collections/?id=lettres_de_poilus&page=1",
        "previous": "/api/dts/collections/?id=lettres_de_poilus&page=18",
        "next": "/api/dts/collections/?id=lettres_de_poilus&page=20",
        "last": "/api/dts/collections/?id=lettres_de_poilus&page=500"
    }
}
```
