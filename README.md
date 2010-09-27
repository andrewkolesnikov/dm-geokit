# dm-geokit

A mixin for Datamapper models that enables geographic functionality.

* Search for content via DataMapper-style query methods, e.g. Location.all(:address.near => {:origin => 'Portland, OR', :distance => 5.mi})
* Sort by distance easily: Location.all(:address.near => {:origin => 'Portland, OR', :distance => 5.mi}, :order => [:address_distance.desc])
* Ability to specify multiple fields that are geocodable (mostly)

# Usage

Basic Class Definition:

    class Location
      include DataMapper::Resource
      include DataMapper::GeoKit
      property :id, Serial
      has_geographic_location :address
    end

This will automatically generate fields and methods for use with the DM Object, prefixed with the field name specified.
Since the above example used the field :address, the following fields would be generated:

* address_street_address
* address_city
* address_state
* address_zip
* address_country_code
* address_full_address
* address_lat
* address_lng

You can either reference those fields directly, or use the proxy object returned by calling .address on your object:

    l = Location.all(:address.near => {:origin => 'Portland, OR', :distance => 5.mi})

    l.each do |loc|
      puts loc.address # .to_s yields string representation of full address, e.g. "12345 My St. Portland, OR USA"
      puts loc.address.inspect # the proxy object, GeographicLocation, with matching methods for each property
      puts loc.address.street_address # getting the street_address from the proxy object
      puts loc.address_street_address # directly access the SQL column
    end

The GeographicLocation proxy object is a convenience to allow you to compare and sort results in Ruby.

Requirements
===========

* geokit >= 1.5.0
* dm-core >= 1.0.2
* dm-aggregates >= 1.0.2
