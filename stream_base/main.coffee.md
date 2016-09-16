# Main module

This contain the main module exports for this package.


## Imports

### Core imports

	{Readable} = require 'stream'


### Library imports

	curry = require 'ramped.curry'


## Exports

### Start new log entry

	module.exports.make_note = curry (label, value)->
		label: label
		value: value


### Pipe log Stream to another Stream

	module.exports.pipe_to_stream = curry (logger, other_stream)->
		logger.pipe(other_stream)


### Send log entry to log Stream

	module.exports.send = curry (logger, message)->
		logger.push message

		message.value


### Start new log Stream

	module.exports.start = ->
		new Readable objectMode: true


### Stop log Stream

	module.exports.stop = (logger)->
		logger.push null
