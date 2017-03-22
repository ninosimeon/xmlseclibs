#xmlseclibs_sunat

xmlseclibs_sunat es una librería hecha en PHP para la firma electrónica de documentos XML.

El autor de xmlseclibs es Rob Richards. Repositorio oficial: https://github.com/robrichards/xmlseclibs


# Requerimientos

xmlseclibs_sunat requiere PHP 5.3 o mayor.


## How to Install

Install with [`composer.phar`](http://getcomposer.org).

```sh
php composer.phar require "ninosimeon/xmlseclibs_sunat"
```


## Use cases

xmlseclibs is being used in many different software.

* [SimpleSAMLPHP](https://github.com/simplesamlphp/simplesamlphp)
* [LightSAML](https://github.com/lightsaml/lightsaml)

## Basic usage

The example below shows basic usage of xmlseclibs, with a SHA-256 signature.

```php
use RobRichards\XMLSecLibs\XMLSecurityDSig;
use RobRichards\XMLSecLibs\XMLSecurityKey;

// Load the XML to be signed
$doc = new DOMDocument();
$doc->load('./path/to/file/tobesigned.xml');

// Create a new Security object 
$objDSig = new XMLSecurityDSig('ds', $signId);
// Use the c14n exclusive canonicalization
$objDSig->setCanonicalMethod(XMLSecurityDSig::EXC_C14N);
// Sign using SHA-256
$objDSig->addReference(
    $doc, 
    XMLSecurityDSig::SHA256, 
    array('http://www.w3.org/2000/09/xmldsig#enveloped-signature')
);

// Create a new (private) Security key
$objKey = new XMLSecurityKey(XMLSecurityKey::RSA_SHA256, array('type'=>'private'));
// Load the private key
$objKey->loadKey('./path/to/privatekey.pem', TRUE);
/* 
If key has a passphrase, set it using 
$objKey->passphrase = '<passphrase>';
*/

// Sign the XML file
$objDSig->sign($objKey);

// Add the associated public key to the signature
$objDSig->add509Cert(file_get_contents('./path/to/file/mycert.pem'));

// Append the signature to the XML
$objDSig->appendSignature($doc->documentElement);
// Save the signed XML
$doc->save('./path/to/signed.xml');
```

## How to Contribute

* [Open Issues](https://github.com/robrichards/xmlseclibs/issues)
* [Open Pull Requests](https://github.com/robrichards/xmlseclibs/pulls)

Mailing List: https://groups.google.com/forum/#!forum/xmlseclibs
