# koop-socrata

> Socrata Provider for [Koop](https://github.com/koopjs/koop)

[![npm version][npm-img]][npm-url]
[![build status][travis-img]][travis-url]

[npm-img]: https://img.shields.io/npm/v/koop-socrata.svg?style=flat-square
[npm-url]: https://www.npmjs.com/package/koop-socrata
[travis-img]: https://img.shields.io/travis/koopjs/koop-socrata.svg?style=flat-square
[travis-url]: https://travis-ci.org/koopjs/koop-socrata

This provider makes it possible to access [Socrata's JSON API](http://dev.socrata.com/docs/formats/json.html) as either GeoJSON or an Esri FeatureService. This is particularly useful for making maps and doing analysis on the web.

## Install

To use this provider you first need a working installation of [Koop](https://github.com/koopjs/koop). Then from within the koop directory you'll need to run the following:

```
npm install koop-socrata --save
```

### Add your app key

Socrata allows 1,000 requests per rolling hour period if you have an app key. If not, there is no guarantee of the number of queries you can make. It is strongly recommended to include an app token if you plan to run Koop-Socrata in production. See:

http://dev.socrata.com/docs/app-tokens.html

1. Go to dev.socrata.com/register to create an app key
2. Edit the default.json in your koop-app config to add
```json
{
	"socrata": {
		"token": "your-app-token"
	}
}
```

## Usage

### Registering Socrata Hosts

Once this provider's been installed you need to register a particular instance of Socrata with your Koop instance. To do this you make `POST` request to the `/socrata` endpoint like so:

```
curl --data "host=https://data.nola.gov&id=nola" localhost:1337/socrata
```

*for Windows users, download cURL from http://curl.haxx.se/download.html or use a tool of your choice to generate the POST request*

What you'll need for that request to work is an ID and the URL of the Socrata instance. The ID is what you'll use to reference datasets that come from Socrata in Koop.

To make sure this works you can visit: http://localhost:1337/socrata and you should see all of the registered hosts.

### Accessing Socrata Data

To access a dataset hosted in Socrata you'll need a "Resource ID" from Socrata. Datasets in Socrata can be accessed as raw JSON like this:

* [https://data.nola.gov/Health-Education-and-Social-Services/NOLA-Grocery-Stores/fwm6-d78i](https://data.nola.gov/Health-Education-and-Social-Services/NOLA-Grocery-Stores/fwm6-d78i) translates into -> https://data.nola.gov/resource/fwm6-d78i.json

And then the ID `fwm6-d78i` can be referenced in Koop like so:

http://koop.dc.esri.com/socrata/nola/fwm6-d78i

If your Socrata data has more than one location column, you can specify the desired location column in the http request like this:

https://path_to_koop/socrata/socrataProvider/dataSetID!spatialColumn

### Handling Large Datasets

Koop-Socrata will page through large datasets to gather all the rows. The default is set to 10,000 rows per request, but the Socrata API handles up to 50,000 requests very well. For production deployments it is recommended to set the Koop Configuration for the Socrata page limit to 50,000.

```json
{
	"socrata": {
		"pageLimit": 50000
	}
}
```

### Clearing Koop's cache for individual resources

If you find yourself in a situation where Koop isn't returning data for a particular resource and you'd like to make sure it makes a fresh request, you can blow out the cobwebs by making the following request in the browser.

```json
http://[koop]/socrata/[provider]/[resourceID]/drop
>>> true
```

## Examples

Here are a few examples of data hosted in Socrata and accessed via Koop.

* GeoJSON: http://koop.dc.esri.com/socrata/nola/fwm6-d78i
* FeatureService: http://koop.dc.esri.com/socrata/nola/fwm6-d78i/FeatureServer/0
* All publicly registered Socrata instances: http://koop.dc.esri.com/socrata

## Contributing

Esri welcomes contributions from anyone and everyone. Please see our [guidelines for contributing](https://github.com/Esri/contributing).

## License

[Apache 2.0](LICENSE)
