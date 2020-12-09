# pygiftbit - A Python Interface for GiftBit

pygiftbit is a simple API wrapper for the [GiftBit](https://giftbit.com) Gift API so that you can easily send out e-giftcards of various kinds over e-mail. It is designed for you to be able to easily:

* Check for supported gift cards
* Check your account balance
* Fund your account
* Send a gift card of a desired brand and value combination
* Check the status of a sent gift card

Please note, however, that an account with GiftBit **is required** and this library will not function without a valid API key from them.

There are other projects intended for this same purpose. However, they appear to not be in active development and are incomplete. They do not offer the same set of commands or level of documentation as this project.

## Installation

Using `pip`:

```
pip install pygiftbit
```

## Usage

Usage requires simply importing the client and initializing it with your API key and letting it know whether or not you are using the testbed:

```python
from pygiftbit.giftbit import Client

gift_client = Client(api_key="<YOUR_API_KEY>", testing=False)
```

By default, the library uses the testbed. Make sure you declare `testing=false` in your production application.

From there, you have multiple commands available to you:

### Client.list_regions()

This command returns a `dict()` of the available regions as such:

```python
{
    1: 'Canda',
    2: 'USA',
    3: 'Global',
    4: 'Australia'
}
```

### Client.get_brand_codes(**search_arg_modifiers)

This command will list available brand codes with some simple search arguments. There are a few default values:

| Argument Name | Data Type | Default | Description |
| --- | --- | --- | --- |
| region | int | 3 | Specifies the region to search. Use `Client.list_regions()` for a valid list. |
| limit | int | 20 | Specifies how many results to return. |
| offset | int | 0 | Specifies the offset to search by with a limit. |

Other options to search by include:

| Argument Name | Data Type | Description |
| --- | --- | --- |
| search | str | A specific search term to be found in the brand name or description. |
| min_price_in_cents | int | The minium value a gift card can be set to. |
| max_price_in_cents | int | The maximum value a gift card can be set to. |
| currencyisocode | str | Search for gift cards available in either "CAD", "USD", or "AUD". |
| embeddable | bool | If set to true, brands that cannot be used for in-app delivery are omitted. |

For example, if you only wanted to find gift cards with a minimum value of $5 USD in the USA, you could search by:

```python
Client.get_brand_codes(region=2, currencyisocode='USD', min_price_in_cents=500)
```

### Client.brand_info(brand_code)

This command will retrieve detailed information about a brand. the `brand_code` argument is required and should be a string retrieved from `Client.get_brand_codes()`.