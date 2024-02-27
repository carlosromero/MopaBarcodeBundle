# README

changes to use with symfony 3.4

## Introduction

MopaBarcodeBundle integrates Laminas_Barcode and PHP QR Lib to be easily used in symfony2 via twig.
I did include phpqrcode form  http://sourceforge.net/projects/phpqrcode/ due to changes in its config.
Is just a shot and shouldnt be considered to be perfect. Feel free to fork and PR.

## Prerequisites

## Installation

1. Add this bundle to your composer.json:
```
{
    "require": {
        // ...
        "mopa/barcode-bundle": "dev-master",
        "avalanche123/imagine-bundle": "dev-master", // handles image installation via requirements
        // if you want to use the laminas barcodes
        "laminas/laminas-barcode": "dev-master",
        // optionally for playground
        "mopa/bootstrap-sandbox-bundle": "dev-master"
        // also read the readme:
        // https://github.com/phiamo/MopaBootstrapSandboxBundle
    }
}
```

2. Add this bundle to your app/AppKernel.php:

``` php
// application/ApplicationKernel.php
public function registerBundles()
{
    return array(
        // ...
        new Avalanche\Bundle\ImagineBundle\AvalancheImagineBundle(),
        new Mopa\Bundle\BarcodeBundle\MopaBarcodeBundle(),
    );
}
```

## Demo

Include MopaBoostrapBundle in your app: https://github.com/phiamo/MopaBootstrapBundle

Include this snipplet in your routing.yml

``` yaml
my_barcode_playground:
    resource: "@MopaBarcodeBundle/Resources/config/routing/barcode_playground.yml"
    prefix:   /
```

Add this to your config.yml:

``` yaml
imports:
    - { resource: @MopaBootstrapSandboxBundle/Resources/config/examples/example_menu.yml }
    - { resource: @MopaBootstrapSandboxBundle/Resources/config/examples/example_navbar.yml }
```

And try http://{yoursymfonyapp}/mopa/barcode/playground

## Usage

Have a look into the https://github.com/phiamo/MopaBarcodeBundle/blob/master/Controller/BarcodeController.php
to see it in action

Supported Barcode Types depend on your Zend2 installation

If you installed it have a look into
https://github.com/phiamo/MopaBarcodeBundle/blob/master/Model/BarcodeTypes.php
The Type given to the service is either the int or the string defined in the types arrays keys and values

To get the service in your controllers etc you can use

$bmanager = $this->container->get('mopa_barcode.barcode_service');

$bmanager->saveAs($type, $text, $file);
to save a Barcode of $type with $text as $file or

$bmanager->get($type, $enctext, $absolute = false);
to get the url to the file
where $enctext is urlencoded and $absolute is an boolean to get either the absolute or the relative path (default)

## Twig Helper

There is also a twig helper registered:

``` jinja
        <p><img alt="[barcode]" src="{{ mopa_barcode_url('code128', '123456789', {'barcodeOptions': {}, 'rendererOptions': {}}) }}"></p>
```

Of course the dict (3rd parameter is optional) have a look into http://framework.zend.com/manual/2.1/en/modules/zend.barcode.creation.html
to see what options can be set.

the dict also takes a noCache boolean, i wont explain it further

## Using the bundle directly

To Make usage e.g. of the Playground in your app, just copy the playground.html.twig to
app/Resources/MopaBootstrapBundle/views/Barcode/playground.html.twig
and modify as you like

## Using the Bundle as a urlservice

If you would like to generate the barcodes on the fly include
in your routing.yml

``` yaml
my_barcode_display:
    resource: "@MopaBarcodeBundle/Resources/config/routing/barcode_display.yml"
    prefix:   /
```
And just use Urls to generate your barcodes:

http://{yoursymfonyapp}/mopa/barcode/send/{type}/{enctext}

## Using QR code overlays

Add this to twig template.

``` jinja
    <img src="{{ mopa_barcode_url('qr', "Text to put in QR code", {'size':2, 'level':3, 'margin':0, 'useOverlay': true}) }}"/>
```

### Changing overlay images

Add and edit this to your parameters.yml file.

    mopa_barcode.overlay_images_path: Resources/qr_overlays

For each QR code level (size) you have to generate overlay image. Look in `Resources/qr_overlays' path of bundle for example overlay images.


## TODO

    - Load the different Barcode Libs in a different way. should't be done by ints :(

## Known Issues

    - Nothing what could not be done in another way, probably some will arise as soon as its published
      So make issues!
    - There are probably things missing, so make PR's
