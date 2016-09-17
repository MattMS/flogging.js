# Main module

This contain the main module exports for this package.


## Library imports

	curry = require 'ramped.curry'

	pipe = require 'ramped.pipe'

	set = require 'ramped.set'


## Add property

- https://github.com/trentm/node-bunyan#levels

	# add_level = set 'level'

	add_type = set 'type'


## Exports

	module.exports = curry (starter, sender)->
		error: starter pipe [
			# add_level 50
			add_type 'error'
			sender
		]

		info: starter pipe [
			# add_level 30
			add_type 'info'
			sender
		]

		log: starter pipe [
			# add_level 20 # `debug` level in Bunyan.
			add_type 'log'
			sender
		]

		trace: starter pipe [
			# add_level 10
			add_type 'trace'
			sender
		]

		warn: starter pipe [
			# add_level 40
			add_type 'warn'
			sender
		]
