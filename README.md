# Fun, furious, functional logging - flogging.js

This library is intended to make logging in functional programming a bit easier.
It should play nicely with [Ramda](http://ramdajs.com/).


## Installing

	npm i flogging --save


## Usage

	flogging = require('flogging')

	R = require('ramda')

	log = flogging.start_console_text_stream(process.stdout)

Logs the message *and* returns the value like it wasn't even there (the logger that is, your values are safe).

	lower = R.pipe(
		R.flip(R.divide(2)),
		log.info('Halfway there'),
		R.add(7)
	)

	what_it_is = lower(30)
	// Sets `what_it_is` to 22.
	// Prints `{"message": "Halfway there", "value": 15}` to the console.

It's good to clean up after yourself.

	log.stop()


## Object mode output

Use `start_console_stream` (note missing `_text_`) to output the logs to a Stream with `objectMode: true`.

	concat_stream = require('concat-stream')

	out_stream = concat_stream((array_of_log_objects) => console.log(array_of_log_objects))

	log = flogging.start_console_stream(out_stream)

	log.info('Super informational')

	log.stop()

I hope it is obvious that using `concat-stream` like this on all your logs is a **bad idea**.


## License

[MIT](./LICENSE)
