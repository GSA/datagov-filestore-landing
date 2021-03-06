# datagov-filestore-landing

Landing pages for the [Data.gov filestore](https://filestore.data.gov/).
Data.gov filestore serves several purposes:

- Static hosting for CloudFront error pages
- Large file hosting

In the past, we've also used filestore.data.gov for static hosting when under
a government shutdown.


## CloudFront error pages

Static error pages for catalog.data.gov and www.data.gov are stored on
filestore.data.gov. If CloudFront serves an error (500), it will serve the html
content from these files. See the AWS production account for the configuration.

These files are stored under `/gsa/static-pages/errors` along with a few static
assets.

- **500.html** (catalog.data.gov)
- **500-wp.html** (www.data.gov)

_Note: these files are served from filestore.data.gov/errors, so beware when
updating links._

Tip: you can test locally with [Jekyll](https://jekyllrb.com/):

    $ cd gsa/static-pages
    $ jekyll serve
    $ open http://localhost:4000/errors/500.html


## Large file hosting

catalog.data.gov stores large files on filestore under `/gsa/catalog`. They are:

- **/metrics** metrics generated by CKAN
- **/jsonl** a JSONL export of the entire catalog
- **/sitemap** the catalog sitemap


## Deployment

Pages are deployed manually to S3. From catalog-harvester2p, run these commands:

    $ cd datagov-filestore-landing
    $ git checkout master
    $ git pull --ff-only
    $ aws s3 sync --exclude '.git/*' . s3://bsp-ocsit-prod-east-data-site-filestore
