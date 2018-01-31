# Hydra Collection Applied to DTS

## Introduction

I propose a slightly more fixed version of Hydra where the following would be always true for practicality :

- `title` are a list of dictionary where @lang and @value hold the information about the title. An optional `default` can state the default title to display.
- `description` is a list of dictionary where @lang and @value hold the information about the title.
- `dts:metadata` holds DublinCore and Dublin Core Extended values
    - Questions :
        - are some DC and DCE keys required ?
        - what are the way to display their values
- `dts:extended` holds any supplementary information provided by other ontologies/domains"
- "@context" should be 
    - extending Hydra
    - providing DC, DCT and DTS namespaces prefix
- `dts:navigate` holds a links to the Navigation API route for current object (mandatory in children of `member` ?)
- `dts:read` holds a link to the Passage API for the current object
- `dts:download` holds a link or a list of links to a downloadable format of the current object


## Examples

### Root collection

#### Example of url : 

- `/api/dts/collections/`
- `/api/dts/collections/?id=general`

#### Headers

*Need to be written and need help as I don't get the URI templating thingie*

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
    "title": [
        {"fre" : "Collection Générale de l'École Nationale des Chartes"}
    ],
    "dts:metadata": {
        "dc:publisher": ["École Nationale des Chartes", "https://viaf.org/viaf/167874585"]
    },
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
        },
        {
             "@id" : "lasciva_roma",
             "title" : [
                 {"@lang": "lat", "@value": "Lasciva Roma", "default": true},
             ],
             "description": [
                {
                    "@lang": "eng",
                    "@value": "Collection of primary sources of interest in the studies of Ancient World's sexuality"
                }
             ],
             "@type" : "Collection",
             "totalItems" : "1"
        },
        {
             "@id" : "lettres_de_poilus",
             "title" : [
                 {"@lang": "fre", "@value": "Correspondance des poilus", "default": true},
                 {"@lang": "unk", "@value": "French Soldiers of the Great War Mails"},
             ],
             "description": [
                 {"@lang": "fre", "@value": "Collection de lettres de poilus entre 1917 et 1918"}
             ],
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

*Need to be written and need help as I don't get the URI templating thingie*

### Response

```json
{
    "@context": {
        "@base": "http://www.w3.org/ns/hydra/context.jsonld",
        "dct": "http://purl.org/dc/terms/",
        "dts": "http://purl.org//dts-ontology/#",
        "dc": "http://purl.org/dc/elements/1.1/",
        "tei": "http://www.tei-c.org/ns/1.0",
    },
    "@id": "lasciva_roma",
    "@type": "Collection",
    "totalItems": "2",
    "title" : [
        {"@lang": "lat", "@value": "Lasciva Roma", "default": true},
    ],
    "description": [
        {
            "@lang": "eng",
            "@value": "Collection of primary sources of interest in the studies of Ancient World's sexuality"
        }
    ],
    "dts:metadata": {
        "dc:creator": [
            "Thibault Clérice", "http://orcid.org/0000-0003-1852-9204"
        ]
    },
    "member": [
        {
            "@id" : "urn:cts:latinLit:phi1103.phi001",
            "title" : [
                {"@lang": "lat", "@value": "Priapeia", "default": true},
            ],
            "dts:metadata": {
                "dc:type": [
                    "http://chs.harvard.edu/xmlns/cts#work"
                ],
                "dc:creator": [
                    {"@lang": "eng", "@value": "Anonymous"}
                ],
                "dc:language": ["lat", "eng"]
            },
            "description": [
                {
                   "@lang": "eng",
                    "@value": "Anonymous lascivious Poems "
                }
            ],
            "@type" : "Collection",
            "totalItems" : "1"
        }
    ]
}
```


### Sub-collection, not readable but with readable members

#### Note

As a provider of a small collection, I would do the choice here to expand the metadata to more than readable to avoid successive calls of the API

#### Example of url : 

- `/api/dts/collections/?id=urn:cts:latinLit:phi1103.phi001`

#### Headers

*Need to be written and need help as I don't get the URI templating thingie*

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
    "@id": "urn:cts:latinLit:phi1103.phi001",
    "@type": "Collection",
    "title" : [
        {"@lang": "lat", "@value": "Priapeia", "default": true},
    ],
    "dts:metadata": {
        "dc:type": [
            "http://chs.harvard.edu/xmlns/cts#work"
        ],
        "dc:creator": [
            {"@lang": "eng", "@value": "Anonymous"}
        ],
        "dc:language": ["lat", "eng"]
    },
    "description": [
        {
           "@lang": "eng",
            "@value": "Anonymous lascivious Poems "
        }
    ],
    "@type" : "Collection",
    "totalItems" : "1"
    "member": [
        {
            "@id" : "urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
            "title" : [
                {"@lang": "lat", "@value": "Priapeia", "default": true},
            ],
            "dts:metadata": {
                "dc:type": [
                    "http://chs.harvard.edu/xmlns/cts#edition",
                    "dc:Text"
                ],
                "dc:source": [{"@id": "https://archive.org/details/poetaelatinimino12baeh2"}],
                "dct:dateCopyrighted": 1879,
                "dc:creator": [
                    {"@lang": "eng", "@value": "Anonymous"}
                ],
                "dc:contributor": ["Aemilius Baehrens"]
                "dc:language": ["lat", "eng"]
            },
            "description": [
                {
                   "@lang": "eng",
                    "@value": "Priapeia based on the edition of Aemilius Baehrens"
                }
            ],
            "@type" : "Collection",
            "totalItems" : "1",
            "dts:read": "/api/dts/documents?id=urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
            "dts:navigate": "/api/dts/navigation?id=urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
            "tei:refsDecl": [
                {
                    "tei:matchPattern":  "\\w+",
                    "tei:replacementPattern": "#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1']/tei:div[@n='$2'])",
                    "@type": "book"
                },
                {
                    "tei:matchPattern": "\\w+\\.\\w+",
                    "tei:replacementPattern": "#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1']/tei:div[@n='$2'])",
                    "@type": "poem"
                },
                {
                    "tei:matchPattern":  "\\w+\\.\w+\\.\\w+",
                    "tei:replacementPattern": "#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1']/tei:div[@n='$2']//tei:l[@n='$3'])",
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

*Need to be written and need help as I don't get the URI templating thingie*

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
    "@id" : "urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
    "title" : [
        {"@lang": "lat", "@value": "Priapeia", "default": true},
    ],
    "dts:metadata": {
        "dc:type": [
            "http://chs.harvard.edu/xmlns/cts#edition",
            "dc:Text"
        ],
        "dc:source": [{"@id": "https://archive.org/details/poetaelatinimino12baeh2"}],
        "dct:dateCopyrighted": 1879,
        "dc:creator": [
            {"@lang": "eng", "@value": "Anonymous"}
        ],
        "dc:contributor": ["Aemilius Baehrens"]
        "dc:language": ["lat", "eng"]
    },
    "description": [
        {
           "@lang": "eng",
            "@value": "Priapeia based on the edition of Aemilius Baehrens"
        }
    ],
    "@type" : "Collection",
    "totalItems" : "1",
    "dts:read": "/api/dts/documents?id=urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
    "dts:navigate": "/api/dts/navigation?id=urn:cts:latinLit:phi1103.phi001.lascivaroma-lat1",
    "tei:refsDecl": [
        {
            "tei:matchPattern":  "\\w+",
            "tei:replacementPattern": "#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1']/tei:div[@n='$2'])",
            "@type": "book"
        },
        {
            "tei:matchPattern": "\\w+\\.\\w+",
            "tei:replacementPattern": "#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1']/tei:div[@n='$2'])",
            "@type": "poem"
        },
        {
            "tei:matchPattern":  "\\w+\\.\w+\\.\\w+",
            "tei:replacementPattern": "#xpath(/tei:TEI/tei:text/tei:body/tei:div/tei:div[@n='$1']/tei:div[@n='$2']//tei:l[@n='$3'])",
            "@type": "line"
        }
    ]
}
```