# Shopify API

The ShopifyAPI library allows Python developers to programmatically
access the admin section of stores.

The API is accessed using pyactiveresource in order to provide an
interface similar to the
[ruby Shopify API](https://github.com/shopify/shopify_api) gem.
The data itself is sent as XML over HTTP to communicate with Shopify,
which provides a web service that follows the REST principles as
much as possible.

## Usage

### Requirements

All API usage happens through Shopify applications, created by
either shop owners for their own shops, or by Shopify Partners for
use by other shop owners:

* Shop owners can create applications for themselves through their
  own admin (under the Apps > Manage Apps).
* Shopify Partners create applications through their admin:
  <http://app.shopify.com/services/partners>

For more information and detailed documentation about the API visit
<http://api.shopify.com>

### Installation

To install or upgrade to the latest release:

```shell
easy_install -U ShopifyAPI
```

To install from source, first install the pre-requisites:

```shell
easy_install pyactiveresource PyYAML python-dateutil==1.5
```

Then use the setup script in the root of this source tree.

```shell
python setup.py install
```

### Getting Started

ShopifyAPI uses pyactiveresource to communicate with the REST web
service. pyactiveresource has to be configured with a fully authorized
URL of a particular store first. To obtain that URL you can follow
these steps:

1.  First create a new application in either the partners admin or
    your store admin and write down your `API_KEY` and `SHARED_SECRET`.

2.  You will need to supply two parameters to the Session class
    before you instantiate it:

    ```python
    shopify.Session.setup(api_key=API_KEY, secret=SHARED_SECRET)
    ```

3.  For application to access a shop via the API, they first need a
    "token" specific to the shop, which is obtained from Shopify after
    the owner has granted the application access to the shop. This can
    be done by redirecting the shop owner to permission URL obtained
    as follows:

    ```python
    shop_url = "yourshopname.myshopify.com"
    permission_url = shopify.Session.create_permission_url(shop_url)
    ```

4.  After visiting this URL, the shop redirects the owner to a custom
    URL of your application where the `token` gets sent to (it's param
    name is just `t`) along with other parameters to ensure it was sent
    by Shopify. That token is used to instantiate the session so that it
    is ready to make calls to that particular shop.

    ```python
    session = shopify.Session(shop_url, params)
    ```

5.  Now you can finally get the fully authorized URL for that shop.
    Use that URL to configure ActiveResource and you're set:

    ```python
    shopify.ShopifyResource.site = session.site
    ```

6.  Get data from that shop (returns ActiveResource instances):

    ```python
    shop = shopify.Shop.current()
    latest_orders = shopify.Order.find()
    ```

### Console

This package also includes the `shopify_api.py` script to make it easy to
open up an interactive console to use the API with a shop.

1.  Go to https\://*yourshopname*.myshopify.com/admin/api to generate a private
    application and obtain your API key and password to use with your shop.

2.  Use the `shopify_api.py` script to save the credentials for the
    shop to quickly login. The script uses [PyYAML](http://pyyaml.org/) to save
    and load connection configurations in the same format as the ruby
    shopify\_api.

    ```shell
    shopify_api.py add yourshopname
    ```

    Follow the prompts for the shop domain, API key and password.

3.  Start the console for the connection.

    ```shell
    shopify_api.py console
    ```

4.  Enter the following for the full list of the commands.

    ```shell
    shopify_api.py help
    ```

## Copyright

Copyright (c) 2011 "JadedPixel inc.". See LICENSE for details.
