# plaid-ruby

Ruby gem wrapper for the GustoPlaid API. For more details, please see the [full API documentation](https://plaid.com/docs).

## Installation

```
$ gem install gusto_plaid
```

or add it to your Gemfile like this:

```
gem 'gusto_plaid'
```

## Usage

```ruby
require 'gusto_plaid'
```
## Global Configuration
Pop this into your environment file.
```
GustoPlaid.config do |config|
  config.client_id = keys[CLIENT_ID]
  config.secret = keys[SECRET]
end
```

Now create a YML file that has your CLIENT_ID and your SECRET provided by GustoPlaid.

There are two different requests one can make using the gem: Call and Customer.

## Call Methods

Call is anything that does not require an access_token.

1) add_account_auth( type, username, password, email) <br>
    Returns a hash with keys: code, access_token, accounts, transactions all with embedded json from GustoPlaid.
    The account information is detailed and has full account and routing numbers, however, you will
    not receive transactions
```ruby
# if(code == 200) {returns {[:code => 'x'],[:access_token => 'y'],[:accounts => 'z']}
# Note: 'x','y','z' are all formatted as json.


2) add_account_connect( type , username , password , email ) <br>
    Returns a hash with keys: code, access_token, accounts, transactions all with embedded json from GustoPlaid.
    The account information returned doesn't contain full account and routing numbers
```ruby
# if(code == 200) {returns {[:code => 'x'],[:access_token => 'y'],[:accounts => 'z'],[:transactions => 'a']}
# Note: 'x','y','z','a' are all formatted as json.

Ex)
  new_account = GustoPlaid.call.add_account_connect("amex","plaid_test","plaid_good","test@gmail.com")
  puts new_account[:code]
  "200"
```
3) get_place( id ) <br>
     Returns a hash with keys: entity and location all with embedded json from GustoPlaid.
```ruby
# if(code == 200) {returns {[:entity => 'x'],[:location => 'y']}
# Note: 'x','y' are formatted as json.

Ex)
  location_deets = GustoPlaid.call.get_place("52a77fea4a2eab775f004109")
  puts new_account[:location]["address"]
  "125 Main St"
```

## Customer Methods

Customer defines anything that requires an access_token.

1) mfa_auth_step( access_token , code ) <br>
    Returns a hash with keys: code, access_token, accounts, transactions all with embedded json from GustoPlaid.
```ruby
# if(code == 200) {returns {[:code => 'x'],[:access_token => 'y'],[:accounts => 'z'],[:transactions => 'a']}
# Note: 'x','y','z','a' are all formatted as json.

Ex)
  new_account = GustoPlaid.customer.mfa_step("test","tomato")
  puts new_account[:code]
  "200"
```

2) mfa_connect_step( access_token , code ) <br>
    Returns a hash with keys: code, access_token, accounts, transactions all with embedded json from GustoPlaid.
```ruby
# if(code == 200) {returns {[:code => 'x'],[:access_token => 'y'],[:accounts => 'z'],[:transactions => 'a']}
# Note: 'x','y','z','a' are all formatted as json.

Ex)
  new_account = GustoPlaid.customer.mfa_auth_step("test","tomato")
  puts new_account[:code]
  "200"
```

To attain transactions you have to use mfa_connect, and mfa_connect_step
3) get_transactions( access_token ) <br>
    Returns a hash with key transaction
```ruby
# if(code == 200) {returns {[:transaction => 'x']}
# Note: 'x' is formatted as json.

Ex)
  transactions = GustoPlaid.customer.get_transactions("test")
  puts transactions[:transactions][1]["amount"]
  1334.99
```

4) delete_account( access_token ) <br>
    Returns a hash with key code
```ruby

Ex)
  message = GustoPlaid.customer.delete_account("test")
  puts message[:code]
  "200"
```

## Requirements

* Ruby 2.0.0 or higher
