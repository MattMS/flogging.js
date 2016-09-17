# Main module

This contain the main module exports for this package.


## Library imports

	flogging_base = require 'flogging.stream_base'

	flogging_console = require 'flogging.console_inputs'

	through2_map = require 'through2-map'


## Stream filter function

	fix_json_chunk = (chunk)->
		JSON.stringify(chunk) + '\n'


## Exports

	module.exports.make_json_text_stream = (output_stream)->
		json_text_stream = through2_map.obj fix_json_chunk

		json_text_stream.pipe output_stream

		json_text_stream

	module.exports.start_console_stream = (output_stream)->
		logger = flogging_base.start()

		flogging_base.pipe_to_stream logger, output_stream

		log = flogging_console flogging_base.make_note, flogging_base.send logger

		log.stop = ->
			flogging_base.stop logger

		log

	module.exports.start_console_text_stream = (output_stream)->
		module.exports.start_console_stream module.exports.make_json_text_stream output_stream
