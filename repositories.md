These are the major repositories with descriptions of how they interact with each other (via API calls, file data, or in-memory objects).

- Services
  - [cnx.org](#cnx-org)
  - [archive.cnx.org](#archive-cnx-org)
  - [publishing.cnx.org](#publishing-cnx-org)
  - [legacy.cnx.org](#legacy-cnx-org)
- Conversions
  - [PDF Generation](#pdf-generation)
  - [CNXML to XHTML](#cnxml-to-xhtml)
  - [CNXML validation](#cnxml-validation)
  - [XHTML validation](#xhtml-validation)

**Note:** descriptions about a repository should probably be copied into the top of the repository's `README.md` file.


## cnx.org

### Provides

- the single-page app for reading content (Books and Pages) in a browser.


### Repositories
- [webview](https://github.com/Connexions/webview)

### Talks to

- [archive.cnx.org](#archive-cnx-org)
- [exercises.openstax.org](https://github.com/openstax/exercises)
- [tutor.openstax.org](https://github.com/openstax/tutor)


## archive.cnx.org

This provides **read-only** access to the archive of published content.

### Provides

- retrieving content (Books and Pages)
- searching for content
- retrieving metadata about content


### Accepts

- published content (by virtue of being in the Postgres DB)


### Repositories

- [cnx-archive](https://github.com/Connexions/cnx-archive)
  - [cnx-db](https://github.com/Connexions/cnx-db)
  - [cnx-epub](https://github.com/Connexions/cnx-epub)
  - [cnx-query-grammar](https://github.com/Connexions/cnx-query-grammar)
  - [rhaptos.cnxmlutils](https://github.com/Connexions/rhaptos.cnxmlutils)


## publishing.cnx.org

This acts as the publishing endpoint. It can either accept a specially-formatted EPUB3 file, or it listens to database changes (when publishing using [legacy.cnx.org](#legacy-cnx-org)). If a book is marked as needing to be baked, [cnx-easybake](https://github.com/Connexions/cnx-easybake) runs using a CSS file in [cnx-recipes](https://github.com/Connexions/cnx-recipes)

### Requires
- [cnx-publishing](https://github.com/Connexions/cnx-publishing)
  - [cnx-archive](https://github.com/Connexions/cnx-archive)
  - [cnx-epub](https://github.com/Connexions/cnx-epub)
  - [cnx-easybake](https://github.com/Connexions/cnx-easybake)
  - [cnx-recipes](https://github.com/Connexions/cnx-recipes)

### Talks to
- [archive.cnx.org](#archive-cnx-org) using the Postgres DB

### Listens to
- [legacy.cnx.org](#legacy-cnx-org) publish events using the Postgres DB

### Accepts
- specially formatted epub3 documents for publishing


## legacy.cnx.org

This Zope application is how content is edited and published.

### Repositories
- All the ones in the https://github.com/Rhaptos org

### Talks to
- [publishing.cnx.org](#publishing-cnx-org) using the Postgres DB when content is published


## PDF Generation

This is a separate process that is spawned by [legacy.cnx.org](#legacy-cnx-org).

It uses XSLT transform files to convert CNXML `->` Docbook `->` XHTML and then uses CSS and PrinceXML to create a PDF.

### Repositories

- [oer.exports](https://github.com/Connexions/oer.exports) (Private)


## CNXML to XHTML

There are 4 different CNXML is converted to XHTML:

- CNXML `->` Raw XHTML
- CNXML `->` Raw XHTML `->` Baked XHTML
- CNXML `->` Docbook `->` PDF XHTML
- CNXML `->` Legacy XHTML


### CNXML -> Raw XHTML

This uses XSLT files in [rhaptos.cnxmlutils](https://github.com/Connexions/rhaptos.cnxmlutils)


### CNXML -> Raw XHTML -> Baked XHTML

This uses the Raw XHTML files and converts them using special CSS files in [cnx-recipes](https://github.com/Connexions/cnx-recipes) and the CSS engine in [cnx-easybake](https://github.com/Connexions/cnx-easybake)


### CNXML -> Docbook -> PDF XHTML

This uses Docbook-XSL files in [oer.exports](https://github.com/Connexions/oer.exports). It is being **deprecated** in favor of the _Baked XHTML_

### CNXML -> Legacy XHTML

This uses a custom XSLT file somewhere. It is being **deprecated** in favor of the _Raw or Baked XHTML_.


## CNXML Validation

This is described as a Relax-NG file in [cnxml](https://github.com/Connexions/cnxml).

## XHTML Validation

This is described as a Relax-NG file in [cnxml](https://github.com/Connexions/cnxml).