# Main module

This contain the main module exports for this package.


## Library imports

	curry = require 'ramped.curry'

	pipe = require 'ramped.pipe'

	set = require 'ramped.set'


## Add property

	add_type = set('type')


## Exports

	module.exports = curry (starter, sender)->
		error: pipe [
			starter,
			add_type('error'),
			sender
		]

		info: pipe [
			starter,
			add_type('info'),
			sender
		]

		log: pipe [
			starter,
			add_type('log'),
			sender
		]

		trace: pipe [
			starter,
			add_type('trace'),
			sender
		]

		warn: pipe [
			starter,
			add_type('warn'),
			sender
		]
