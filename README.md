# machete-dsl

Are you tired of writing patterns for [Machete](https://github.com/openSUSE/machete)? You can use **Machete::DSL::Builder**

## Usage

To start working with Machete::DSL you need to execute method ```#build``` on ```Machete::DSL::Builder```

```ruby
Machete::DSL::Builder.build {} # => {}
```

### Simple assignment

When we want to create simple assignment

```ruby
Machete::DSL::Builder.build do
  key "value"
end # => {:key=>"value"}
```

### AST objects

When we want to use ```Rubinius::AST``` object for example ```FixnumLiteral``` we need to execute method ```fixnum_literal```.

```ruby
Machete::DSL::Builder.build do
  fixnum_literal do
    value "2"
  end
end # => {:FixnumLiteral=>{:value=>"2"}}
```

SendWithArguments ```send_with_arguments```
ActualArguments ```actual_arguments```
etc.

### Arrays

When value is a array you need pass ```:array``` symbol to key method

```ruby
Machete::DSL::Builder.build do
  key(:array) do
    value "2"
    value "3"
  end
end # => {:key=>[{:value=>"2"}, {:value=>"3"}]}
```

### Ruby keywords

Ruby have special words.

```ruby
for, if, alias, next, not, super, when, case,
while, yield, class, module, and, break, send
```

And we can't use this methods directly in DSL. You need execute method with underscore on front.

```ruby
  Machete::DSL::Builder.build do
    _class "2"
  end # => {:Class=>"2"}
```

### How to change ```Hash``` to ```Machete``` syntax?

After require ```machete-dsl``` you can use ```to_m``` method on ```Hash```.

### Example

```ruby
Machete::DSL::Builder.build do
  send_with_arguments do
    name ":redirect_to"
    arguments do
      actual_arguments do
        array(:array) do
          symbol_literal do
            value ":to"
          end
        end
        name ":[]"
        receiver do
          _send do
            name ":params"
          end
        end
      end
    end
  end
end
```

This code will produce

```ruby
SendWithArguments<
  name = :redirect_to,
  arguments = ActualArguments<
    array = [
      SymbolLiteral<value = :to>
    ],
    name = :[],
    receiver = Send<
      name = :params
    >
  >
>
```

## Installation

Add this line to your application's Gemfile:

    gem 'machete-dsl'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install machete-dsl

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
