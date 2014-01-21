# Geocodio [![Build Status](https://travis-ci.org/davidcelis/geocodio.png?branch=master)](https://travis-ci.org/davidcelis/geocodio)

Geocodio is a lightweight Ruby wrapper around [geocod.io][geocod.io]'s API.

## Installation

In your Gemfile:

```ruby
gem 'geocodio'
```

## Usage

The point of entry to geocod.io's API is the `Geocodio::Client` class. Initialize
one by passing your API key or allowing the initializer to automatically use
the `GEOCODIO_API_KEY` environment variable:

```ruby
geocodio = Geocodio::Client.new('0123456789abcdef')

# Or, if you've set GEOCODIO_API_KEY in your environment:
geocodio = Geocodio::Client.new
```

### Geocoding

```ruby
results = geocodio.geocode('1 Infinite Loop, Cupertino, CA 95014')
# => #<Geocodio::AddressSet:0x007fdf23a07f80 @query="1 Infinite Loop, Cupertino, CA 95014", @addresses=[...]>
address = results.first
# => #<Geocodio::Address:0x007fb062e7fb20 @number="1", @street="Infinite", @suffix="Loop", @city="Monta Vista", @state="CA", @zip="95014", @latitude=37.331669, @longitude=-122.03074, @accuracy=1, @formatted_address="1 Infinite Loop, Monta Vista CA, 95014">
puts address
# => 1 Infinite Loop, Cupertino CA, 95014
puts address.latitude # or address.lat
# => 37.331669
puts address.longitude # or address.lng
# => -122.03074
puts address.accuracy
# => 1
```

You can pass multiple addresses to `Geocodio::Client#geocode`:

```ruby
result_sets = geocodio.geocode('1 Infinite Loop, Cupertino, CA 95014', '54 West Colorado Boulevard, Pasadena, CA 91105')
# => [#<Geocodio::AddressSet:0x007fdf23a07f80 @query="1 Infinite Loop, Cupertino, CA 95014", @addresses=[...]>, #<Geocodio::AddressSet:0x007fdf23a07f80 @query="54 West Colorado Boulevard, Pasadena, CA 91105", @addresses=[...]>]
cupertino = result_sets.first.best
# => #<Geocodio::Address:0x007fb062e7fb20 @number="1", @street="Infinite", @suffix="Loop", @city="Monta Vista", @state="CA", @zip="95014", @latitude=37.331669, @longitude=-122.03074, @accuracy=1, @formatted_address="1 Infinite Loop, Monta Vista CA, 95014">
```

Geocoding will return one or more instances of `Geocodio::AddressSet` which represent a collection of addresses returned from geocod.io. Each address in the result set has an associated accuracy. If you just want whichever result was the most accurate, a `#best` convenience method is provided:

```ruby
results = geocodio.geocode('1 Infinite Loop, Cupertino, CA 95014')
# => #<Geocodio::AddressSet:0x007fdf23a07f80 @query="1 Infinite Loop, Cupertino, CA 95014", @addresses=[...]>
results.size
# => 2
results.best
# => #<Geocodio::Address:0x007fb062e7fb20 @number="1", @street="Infinite", @suffix="Loop", @city="Monta Vista", @state="CA", @zip="95014", @latitude=37.331669, @longitude=-122.03074, @accuracy=1, @formatted_address="1 Infinite Loop, Monta Vista CA, 95014">
```

### Parsing

```ruby
address = geocodio.parse('1 Infinite Loop, Cupertino, CA 95014')
# => #<Geocodio::Address:0x007fa3c15f41c0 @number="1", @street="Infinite", @suffix="Loop", @city="Cupertino", @state="CA", @zip="95014", @accuracy=nil, @formatted_address="1 Infinite Loop, Cupertino CA, 95014">
```

Note that this endpoint performs no geocoding; it merely formats a single provided address according to geocod.io's standards.

## Contributing

1. Fork it ( http://github.com/[my-github-username]/geocodio/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

[geocod.io]: http://geocod.io/
