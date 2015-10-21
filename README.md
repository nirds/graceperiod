# Grace Period

Grace Period lets you identify and preemptively respond to time-sensitive code in your Ruby projects. It is a specialization of the approach to software contracts taken in [contracts.ruby](https://github.com/egonSchiele/contracts.ruby) that focuses on ease-of-use, clear declarative syntax, and flexibility.

You can think of Grace Period as a tool for placing runtime assertions into your code that let you notify yourself about code that is about to, or already has, expired due to some time constraint. Those assertions can cause notifications from simple warnings to log entries to full-blown exceptions. You retain full control of the mechanism of notification and the logic that identifies assertion conditions.
## Philosophy
Time is a pervasive concern in software development, yet is rarely called out explicitly. That is starting to change.

 We store many time-based fields in our databases, yet we overwrite data in place under the assumption that the data that is current *now* is the most important thing. In reaction to that, we are seeing the emergence of a new crop of immutable data stores that treat time as a first-class concern, never overwrting data and always letting users go back and forth in time interactively, rather than resorting to the manual retrieval of backups.

Similarly in our code there seems to often be the soft tyranny of *now*, the consideration of the current state of things with considerably less regard for the future or the past. We have background jobs that may be time dependent. We have business processes to support that need to embrace the concepts of days, weekdays, weeks, months, quarters and years. Yet we rarely see time used explicitly in defining the fundamental building blocks of our applications, in our functions or methods. Instead, we pepper our code with temporally conditional logic and conflate deployment times with shifts in behavior.

Grace Period, as a project, hopes to probe the intersection of executing code and temporality in new and interesting ways. The obvious low-hanging fruit of our efforts in this first phase is to allow users to declare a method as expired, providing a grace period of gentle warning before raising an exception.

If you are interested in discussing future efforts or letting us know how time impacts your codebase, we invite to join our [mailing list]('#').
## Installation

From the command line:

    gem install grace_period

For use in your Bundler Gemfile:

    gem 'grace_period'

## Hello World

A Grace Period is a section of code immediately preceding a method declaration. It identifies the expiration datetime and notification mechanism for the method, along with the grace period during which you would like to be warned about the imminent expiration.

Here is the simplest possible Grace Period:

```ruby
has_grace_period expires_at: Date.parse("Tue, 10 Aug 2016 01:20:19 -0500 (EDT)")
def my_method
...
```
That says that `my_method` will expire on Tuesday, August 10th, 2016, at the time indicated. Grace Period has reasonable defaults for grace period warnings preceding an expiration which can be adjusted via the configuration file, so the `expires_at` value is the only required value.

Here is that code sample with an immediate expiration, expressed more completely:

```ruby
require 'grace_period'

class Example
  include GracePeriod::Core

  has_grace_period expires_at: Time.now - 1.second
  def my_method
    puts "hello from my_method!"
  end
end

Example.new.my_method
```
...will produce the output:
```
CODE EXPIRATION WARNING: Invoking a method with an expired GracePeriod in example#my_method
```
Again, it is worth mentioning that Grace Period will use its default values unless instructed otherwise, and we strive to be informative but unobtrusive by default, so an expired method will execute with a warning, but you could alter the default behavior in the configuration or you could add additional arguments to specify the response to an encountered expiration, like this:
```ruby
has_grace_period expires_at: (Time.now - 1.second), respond_by: :exception
```
You can find the [complete list of arguments]('#') below. You can accomplish most typical use cases using the built-in parameters, but Grace Period also lets you pass in code blocks for most values, so that you can have maximum flexibility to add whatever logic makes sense for your app to identify and alert about expirations and grace periods. Find out more on our section on [blocks, procs and lambdas]('#').

## Tutorial

We've put together a [spiffy tutorial]('#') for you, as well.

## Getting Involved

To get started in contributing pull requests to improve this gem:

1. Fork this repository
2. Clone the forked repository to your local machine
3. Watch the tests pass by running:

```
bundle install
bundle exec rake
```
4. Make your modifications or improvements.
5. Submit a PR on GitHub.

## Need Help?
You can reach out to the gem authors at some.nird@nird.us.

## License

This work is licensed under the terms of TODO use-it-for-whatever-but-dont-steal-it-license.