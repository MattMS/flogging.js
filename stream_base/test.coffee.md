# Test main module

## Imports

### Library imports

	concat_stream = require 'concat-stream'

	curry = require 'ramped.curry'

	pipe = require 'ramped.pipe'

	tape = require 'tape'


### Relative imports

	{make_note, pipe_to_stream, send, start, stop} = require './main'


## Helper functions

	add = curry (b, a)->
		a + b

	add_type_to_note = curry (value, note)->
		note['type'] = value
		note

	divide = curry (b, a)->
		a / b


## Run test

### Basic calls

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

		info = make_note send(logger)

		info('test 1', 4)
		info('test 2', 8)
		info('test 3', 12)

		stop(logger)


### Call in pipe

	tape 'Log message in pipe', (t)->
		t.plan 2

		logger = start()

		desired_output = [
			label: 'halfway'
			value: 15
		]

		out_stream = concat_stream (actual_output)->
			t.deepEqual actual_output, desired_output, 'All log messages are correct'

		pipe_to_stream(logger, out_stream)

		info = make_note send(logger)

		do_stuff = pipe [
			divide(2),
			info('halfway'),
			add(7)
		]

		result = do_stuff(30)

		t.equal result, 22, 'Result returned correctly'

		stop(logger)


### Call with pipe

	tape 'Log message with sender in pipe', (t)->
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

		info = make_note pipe [
			add_type_to_note 'info'
			send logger
		]

		do_stuff = pipe [
			divide 2
			info 'halfway'
			add 7
		]

		result = do_stuff 30

		t.equal result, 22, 'Result returned correctly'

		stop logger
