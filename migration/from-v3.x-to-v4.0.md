---
description: Step-by-step guide for migrating from v3.x to v4.0
---

# From v3.x to v4.0

This project follows the [Semantic Versioning principles](https://semver.org) and, contrary to upgrade a minor version (where the middle number changes) where no difficulty should be encountered, upgrade a major version (where the first number changes) is subject to significant modifications.

## Update the libraries <a href="#update-the-libraries" id="update-the-libraries"></a>

First of all, you have to make sure you are using the last v3.x release (v3.3.4 at the time of writing).

In addition, you have to make sure you are using PHP `8.1+`.

## Symfony Flex

If you use the Symfony bundle and Symfony Flex, please note that Flex Recipes are now provided through a dedicated server.

It is highly recommended to declare this server within your application `composer.json` file.

```shell
composer config --json extra.symfony.endpoint '["https://api.github.com/repos/web-auth/recipes/contents/index.json?ref=main", "flex://defaults"]'
```

{% hint style="info" %}
You can adapt this command line depending on the other Flex servers you are using.
{% endhint %}

## Spot deprecations <a href="#spot-deprecations" id="spot-deprecations"></a>

Next, you have to verify you don’t use any deprecated class, interface, method or property. If you have PHPUnit tests, [you can easily get the list of deprecation used in your application](https://symfony.com/doc/current/components/phpunit\_bridge.html).

* The class `Webauthn\Server` is removed and there is no replacement.
* The Metadata Statement embraces the version 3 of the specification. There is no migration tool to convert the MDS from v2 to v3. We suggest to refer to [this blog post](https://medium.com/webauthnworks/webauthn-fido2-whats-new-in-mds3-migrating-from-mds2-to-mds3-a271d82cb774) from Yuriy Ackermann, a Fido Alliance staff member who regulary write articles on Webauthn.

## Dependency Changes:

* Added:
  * `symfony/http-client ^6.0`
  * `symfony/uid ^6.0`
* Bumped:
  * `spomky-labs/cbor-php` minimal version is now `^3.0`
  * `spomky-labs/cbor-bundle` minimal version is now `^3.0`
  * `symfony` components minimal version is now `^6.0`
  * `symfony/psr-http-message-bridge` minimal version is now `^2.0`
  * thecodingmachine/safe minimal version is now `^2.0`
* Removed:
  * `sensio/framework-extra-bundle`
  * `ramsey/uuid`

## Update your Configuration Files <a href="#upgrade-the-libraries" id="upgrade-the-libraries"></a>

As the bundle `sensio/framework-extra-bundle` is not required anymore, the associated configuration may become useless if you do not use this bundle in your project.

The most notable changes are related to the Metadata Statement section

```yaml
webauthn:
    metadata: ## Optional
        enabled: true
        mds_repository: 'App\MDS\MetadataStatementRepository'
        status_report_repository: 'App\MDS\MetadataStatementRepository'
        #certificate_chain_checker: 'Webauthn\CertificateChainChecker\PhpCertificateChainChecker::class'
```

## Upgrade the libraries <a href="#upgrade-the-libraries" id="upgrade-the-libraries"></a>

It is now time to upgrade the libraries. In your composer.json, change all `web-auth/*` dependencies from `v3.x` to `v4.0`. When done, execute `composer update`.

{% hint style="warning" %}
This may also update other dependencies. You can list upgradable libraries by calling `composer outdated`. Please make sure these libraries do not impact your upgrade.
{% endhint %}

## All Modifications In A Row

If you want to see all modifications at once, please [have a look at this page](https://github.com/web-auth/webauthn-framework/compare/v3.3.4...v4.0).
