# Test main module

## Imports

### Library imports

	concat_stream = require 'concat-stream'

	pipe_calls = require 'ramped.pipe'

	tape = require 'tape'


### Relative imports

	{make_note, pipe_to_stream, send, start, stop} = require './main'


## Run test

	tape 'Send messages to Stream', (t)->
		t.plan 1

		logger = start()

		desired_output = [
			label: 'test 1'
			value: 4
		,
			label: 'test 2'
			value: 8
		,
			label: 'test 3'
			value: 12
		]

		out_stream = concat_stream (actual_output)->
			t.deepEqual actual_output, desired_output

		pipe_to_stream(logger, out_stream)

		info = pipe_calls [
			make_note,
			send(logger)
		]

		info('test 1', 4)
		info('test 2', 8)
		info('test 3', 12)

		stop(logger)
