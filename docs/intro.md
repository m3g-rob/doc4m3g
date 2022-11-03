# Short intro to M3G API

M<sup>3</sup>G offers some of its functionalities through an application programming interface (API). In particular, a [RESTful HTTP API](https://en.wikipedia.org/wiki/Representational_state_transfer) that allows you to interact with M<sup>3</sup>G purely as data that can be uploaded, accessed, searched for, etc. via [HTTP requests](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol).

HTTP basically allows you to `GET`, `POST`, `PUT`, `DELETE` etc. resources on a remote server. These resources are identified via URLs (=uniform resource locator). URLs usually consists of a protocol (e.g. HTTP), a domain (our servers), a path (a place on our servers), and query parameters (additional options).
## M<sup>3</sup>G API: available operations
At present, M<sup>3</sup>G API provides 5 main "groups" of operations:

 - [Network](https://gnss-metadata.eu/__test/site/api-docs#/Network){:target="_blank"}
 - [License](https://gnss-metadata.eu/__test/site/api-docs#/License){:target="_blank"}
 - [Metadata](https://gnss-metadata.eu/__test/site/api-docs#/Metadata){:target="_blank"}, [Product](https://gnss-metadata.eu/__test/site/api-docs#/Product){:target="_blank"} and [Update](https://gnss-metadata.eu/__test/site/api-docs#/Update){:target="_blank"} (i.e. to get info, export or update some sections of station site logs in M<sup>3</sup>G, respectively)

Within most of these resource "groups", you find endpoints that either allow you to retrieve info on a given M<sup>3</sup>G entry (i.e. a network, a station) or to ask a query to locate many M<sup>3</sup>G entries at the same time (see the [M<sup>3</sup>G Swagger user interface](https://gnss-metadata.eu/__test/site/api-docs){:target="_blank"}).
## M<sup>3</sup>G API: tools
One can start to test M<sup>3</sup>G API thanks to different tools and libraries. For example, one could use:

- an HTTP program like [*curl*](https://curl.haxx.se/) to directly use M<sup>3</sup>G API from within a shell
- a generic Python HTTP library such as [*requests*](https://requests.readthedocs.io/en/master/)
- the browser, via the [M<sup>3</sup>G Swagger UI](https://gnss-metadata.eu/__test/site/api-docs){:target="_blank"}, based on an [OpenAPI spec](https://swagger.io/specification/), describing some of M<sup>3</sup>G function calls

In the following sections, you'll find some examples for common M<sup>3</sup>G tasks using various tools/options.
