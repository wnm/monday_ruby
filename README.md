# Monday API Library for Ruby

![Build Status](https://github.com/sanifhimani/monday_ruby/actions/workflows/ci.yml/badge.svg)
[![Gem Version](https://badge.fury.io/rb/monday_ruby.svg)](https://badge.fury.io/rb/monday_ruby)
[![Coverage Status](https://coveralls.io/repos/github/sanifhimani/monday_ruby/badge.svg?branch=main)](https://coveralls.io/github/sanifhimani/monday_ruby?branch=main)

This library provides convenient access to the monday.com API from the application written in Ruby.

The library provides:

1. A pre-defined set of methods to easily interact with the API resources.
2. Easy configuration path for fast setup and use.
3. Easy error handling.

**Check out the [Wiki](https://github.com/sanifhimani/monday_ruby/wiki) for detailed documentation on how to use the library.**

## Installation

```bash
gem install monday_ruby
```

## Usage

***Complete list of resources along with examples are provided in the [Wiki](https://github.com/sanifhimani/monday_ruby/wiki).***

The library needs to be configured with your account's authentication token which is available on the Admin tab on monday.com. Elaborate documentation can be found [here](https://developer.monday.com/api-reference/docs/authentication).

### Configuration

Once you have the authentication token, you can either globally configure the library or you can configure a specific client.

#### Global config

```ruby
require "monday_ruby"

Monday.configure do |config|
  config.token = "<AUTH_TOKEN>"
end
```

#### Client specific config
```ruby
require "monday_ruby"

client = Monday::Client.new(token: "<AUTH_TOKEN>")
```

The version configuration field allows you to optionally pass in the version of the API you want to use. By default, the latest stable version is used.

```ruby
require "monday_ruby"

Monday.configure do |config|
  config.token = "<AUTH_TOKEN>"
  config.version = "2023-07"
end
```

### Accessing a response object

Get access to response objects by initializing a client and using the appropriate action you want to perform:

```ruby
client = Monday::Client.new(token: "<AUTH_TOKEN>")
response = client.boards

puts response.success?
puts response.body
```

### Use cases

Here are some common use cases for the API client.

#### Fetching all the boards

Initialize the client with the auth token and call the `boards` method.

```ruby
client = Monday::Client.new(token: <AUTH_TOKEN>)

response = client.boards # => <Monday::Response ...>

# To check if the request was successful
response.success? # => true

# To get the boards from the response
response.dig("data", "boards") # => [...]
```

#### Creating a new board

Initialize the client with the auth token and call the `create_board` method.

```ruby
client = Monday::Client.new(token: <AUTH_TOKEN>)

args = {
  board_name: "Test board",
  board_kind: "public",
  description: "Test board description"
}

# => <Monday::Response ...>
response = client.create_board(args: args)

# To check if the request was successful
response.success? # => true

# To get the created board from the response
response.dig("data", "create_board") # => { ... }
```

#### Creating a new item on board

Initialize the client with the auth token and call the `create_item` method.

```ruby
client = Monday::Client.new(token: <AUTH_TOKEN>)

args = {
  board_id: <BOARD_ID>,
  item_name: "New item",
  column_values: {
    status: {
      label: "Working on it"
    },
    keywords: {
      labels: ["Tech team", "DevOps team"]
    }
  }
}

# => <Monday::Response ...>
response = client.create_item(args: args)

# To check if the request was successful
response.success? # => true

# To get the created item from the response
response.dig("data", "create_item") # => { ... }
```

## Development

Run all tests:

```bash
bundle exec rake spec
```

Run linter:

```bash
bundle exec rake rubocop
```

## Contributing

Bug reports and pull requests are welcome on [GitHub](https://github.com/sanifhimani/monday_ruby). This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [code of conduct](https://github.com/sanifhimani/monday_ruby/blob/main/CODE_OF_CONDUCT.md).

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
