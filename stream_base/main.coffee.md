# Main module

This contain the main module exports for this package.


## Library imports

	curry = require 'ramped.curry'

	through2 = require 'through2'


## Exports

### Start new log entry

	module.exports.make_note = curry (sender, label, value)->
		sender
			label: label
			value: value

		value


### Pipe log Stream to another Stream

	module.exports.pipe_to_stream = curry (logger, other_stream)->
		logger.pipe other_stream


### Send log entry to log Stream

	module.exports.send = curry (logger, message)->
		logger.push message

		message


### Start new log Stream

	module.exports.start = ->
		through2.obj()
		# through2.obj (chunk, encoding, callback)->
		# 	callback null, chunk


### Stop log Stream

	module.exports.stop = (logger)->
		logger.push null
