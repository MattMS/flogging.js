# Test

## Imports

### Library imports

	concat_stream = require 'concat-stream'

	curry = require 'ramped.curry'

	{make_note, pipe_to_stream, send, start, stop} = require 'flogging.stream_base'

	pipe = require 'ramped.pipe'

	tape = require 'tape'


### Relative imports

	console_inputs = require './main'


## Helper functions

	add = curry (b, a)->
		a + b

	divide = curry (b, a)->
		a / b


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

		pipe_to_stream logger, out_stream

		log = console_inputs make_note, send(logger)

		log.error 'error message', 4
		log.info 'info message', 8
		log.log 'log message', 16
		log.trace 'trace message', 32
		log.warn 'warn message', 64

		stop logger


### Call in pipe

	tape 'Log message in pipe', (t)->
		t.plan 2

		logger = start()

		desired_output = [
			label: 'halfway'
			type: 'info'
			value: 15
		]

		out_stream = concat_stream (actual_output)->
			t.deepEqual actual_output, desired_output, 'All log messages are correct'

		pipe_to_stream logger, out_stream

		log = console_inputs make_note, send(logger)

		do_stuff = pipe [
			divide 2
			log.info 'halfway'
			add 7
		]

		result = do_stuff 30

		t.equal result, 22, 'Result returned correctly'

		stop logger


### Sender with pipe

	tape 'Sender with pipe', (t)->
		t.plan 2

		logger = start()

		desired_output = [
			label: 'halfway'
			name: 'my logger'
			type: 'error'
			value: 15
		]

		out_stream = concat_stream (actual_output)->
			t.deepEqual actual_output, desired_output, 'All log messages are correct'

		pipe_to_stream logger, out_stream

		add_field = curry (label, value, message)->
			message[label] = value
			message

		log = console_inputs make_note, pipe [
			add_field 'name', 'my logger'
			send logger
		]

		do_stuff = pipe [
			divide 2
			log.error 'halfway'
			add 7
		]

		result = do_stuff 30

		t.equal result, 22, 'Result returned correctly'

		stop logger
