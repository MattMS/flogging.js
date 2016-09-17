# Flogging.js console-style inputs

This allows you to make function calls like the
[Node.js console](https://nodejs.org/api/console.html)
with [flogging.js](https://github.com/MattMS/flogging.js).

If you are unsure if you want this library, you should probably be using the
[main flogging.js package](https://github.com/MattMS/flogging.js).


## Installation

	npm i flogging.console_inputs --save

This only installs a function to create the logging inputs.

You will need to have a flogging.js base installed and organise your own output.


## Usage

This example assumes you have looked at the stream base example and know how to connect the logger to an output Stream.

	flogging = require('flogging.stream_base')

	logger = flogging.start()

	log = require('flogging.console_inputs')(flogging.make_note, flogging.send(logger))

	log.info('Purely informational', 12)
	// Returns 12
	// Logs {"label": "Purely informational", "type": "info", "value": 12}

	log.error('Something bad happened', {err: 60})
	// Returns {err: 60}
	// Logs {"label": "Something bad happened", "type": "error", "value": {err: 60}}

	flogging.stop(logger)


## Constructor

The setup function accepts 2 parameters:

- Starter: such as `flogging.make_note`.

- Sender: such as `flogging.send(logger)`.


### Starter

This provides the interface to logging calls.

Must accept the sender as the first parameter.
Should accept 2 more parameters: label and value.

Must create the log message Object and then call the sender with it.

Must return the value (last parameter) passed in.

For example:

	curry = require('ramped.curry')

	started = curry((sender, label, value) => {
		sender({label: label, value: value})
		return value
	})


### Sender

Receives a single parameter: the log message Object.

You can add common fields with a pipe here:

	curry = require('ramped.curry')
	flogging = require('flogging.stream_base')
	pipe = require('ramped.pipe')

	started = curry(pipe([
		curry((message) => {
			message['name'] = 'my logger'
			return message
		}),
		flogging.send(logger)
	]))

Make sure you always return the `message` Object!


## Functions

- error

- info

- log

- trace

- warn

Each function simply adds a `type` property to messages.


## License

[MIT](https://github.com/MattMS/flogging.js/blob/master/LICENSE)
