## Process Helper

Provides a wrapper around external processes

It collects standard-out and standard-error, allowing:

* the output to be searched for particular output
* the parent process wait for particular output to appear

## License

This is licensed under the Apache 2.0 License

## Example usage

```ruby
executable_args = ['echo', 'hello $USER, ruby passed me "$V"']
wait_for = /(hello .*)/
process = ProcessHelper::ProcessHelper.new()
process.start(executable_args, wait_for, 30, {'V' => 'v'})
```

This will start the process `echo` and wait for it to have printed a line matching the regex to STDOUT.

For a longer running process that you want to interact with:

```ruby
process = ProcessHelper::ProcessHelper.new()
process.start(['java', '-jar', 'some.jar'], /(Server Started)/)

# Interactions with the java process...

process.kill
process.wait_for_exit
```

To retrieve the captured STDERR and STDOUT lines as arrays:

```ruby
process.get_log(:out)
process.get_log(:err)
```

You can also empty the stored buffer when retrieving the STDERR or STDOUT lines:

```ruby
process.get_log!(:out)
process.get_log!(:err)
```

To echo the STDOUT and STDERR lines of the process to STDOUT in real time as well as capturing them:

```ruby
process = ProcessHelper::ProcessHelper.new(print_lines: true)
```

To change the poll rate for printing lines:

```ruby
process = ProcessHelper::ProcessHelper.new(print_lines: true, poll_rate: 0.25)
```
 
To combine the STDOUT and STDERR lines:

```ruby
process.start(executable_args, wait_for, 30, {}, {stderr: true})
```

 
## Caveats

There should be one ProcessHelper instance per external process. For example,
the following will only kill the second process.

```ruby
process = ProcessHelper::ProcessHelper.new()
process.start(['long_running_process', 'one'])
process.start(['long_running_process', 'two'])
process.kill
```
