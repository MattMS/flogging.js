# Test

## Imports

### Library imports

	concat_stream = require 'concat-stream'

	{make_note, pipe_to_stream, send, start, stop} = require 'flogging.stream_base'

	pipe_calls = require 'ramped.pipe'

	tape = require 'tape'


### Relative imports

	console_inputs = require './main'


## Run test

	tape 'Send messages to Stream', (t)->
		t.plan 1

		logger = start()

		desired_output = [
			label: 'error message'
			type: 'error'
			value: 4
		,
			label: 'info message'
			type: 'info'
			value: 8
		,
			label: 'log message'
			type: 'log'
			value: 16
		,
			label: 'trace message'
			type: 'trace'
			value: 32
		,
			label: 'warn message'
			type: 'warn'
			value: 64
		]

		out_stream = concat_stream (actual_output)->
			t.deepEqual actual_output, desired_output

		pipe_to_stream(logger, out_stream)

		log = console_inputs(make_note, send(logger))

		log.error('error message', 4)
		log.info('info message', 8)
		log.log('log message', 16)
		log.trace('trace message', 32)
		log.warn('warn message', 64)

		stop(logger)
