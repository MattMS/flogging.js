# Test main module

## Imports

### Library imports

	concat_stream = require 'concat-stream'

	pipe_calls = require 'ramped.pipe'

	tape = require 'tape'


### Relative imports

	{start_console_stream, stop} = require './main'


## Run test

	tape 'Send messages to Stream', (t)->
		t.plan 1

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

		# log = start_console_text_stream(process.stdout)
		log = start_console_stream(out_stream)

		log.error('error message', 4)
		log.info('info message', 8)
		log.log('log message', 16)
		log.trace('trace message', 32)
		log.warn('warn message', 64)

		log.stop()
