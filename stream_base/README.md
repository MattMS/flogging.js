# Flogging.js Stream base

These are functions for Stream-based [flogging.js](https://github.com/MattMS/flogging.js) usage.

If you are unsure if you want this library, you should probably be using the
[main flogging.js package](https://github.com/MattMS/flogging.js).


## Installation

	npm i flogging.stream_base --save


## Usage

Since this includes only the core functionality, this example requires more steps than the main library.

	flogging_base = require('flogging.stream_base')

	R = require('ramda')

	through2_map = require('through2-map')

	logger = flogging_base.start()

You can customise your logging calls to do all kinds of message-mangling.

	info = flogging_base.make_note(flogging_base.send(logger))

Outputs an Object Stream.

	object_to_text_stream = through2_map.obj((chunk) => JSON.stringify(chunk))

	flogging_base.pipe_to_stream(logger, object_to_text_stream)

	object_to_text_stream.pipe(process.stdout)

Make logging calls.

	info('Purely informational', 12)
	// Returns 12
	// Logs {"label": "Purely informational", "value": 12}

It's good to clean up after yourself.

	flogging_base.stop(logger)


## License

[MIT](https://github.com/MattMS/flogging.js/blob/master/LICENSE)
