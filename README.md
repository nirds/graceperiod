# Grace Period

Grace Period lets you identify and pre-emptively respond to time-sensitive code in your Ruby projects. It is a specialization of the approach to software contracts taken in [contracts.ruby](https://github.com/egonSchiele/contracts.ruby) that focuses on ease-of-use, clear declarative syntax, and flexibility.

You can think of Grace Period as a tool for placing runtime assertions into your code that let you notify yourself about code that is about to, or already has, expired due to some time constraint. Those assertions can cause notifications from simple warnings to log entries to full-blown exceptions. You retain full control of the mechanism of notification and the logic that identifies assertion conditions.

## Installation

From the command line:

    gem install grace_period

For use in your Bundler Gemfile:

    gem 'grace_period'

## Hello World

A Grace Period is a section of code immediately preceding a method declaration. It identifies the expiration date and notification mechanism for the method, along with the grace period during which you would like to be warned about the imminent expiration.

Here is the simplest possible Grace Period:

```ruby
GracePeriod expires_at: Date.parse("Tue, 10 Aug 2016 01:20:19 -0500 (EDT)")
def my_method
...
```
That says that `my_method` will expire on Tuesday, August 10th, 2016, at the time indicated. Grace Period has reasonable defaults for grace period warnings preceding an expiration which can be adjusted via the configuration file, so the `expires_at` value is the only required value.

Here is that code sample with an immediate expiration, expressed more completely:

```ruby
require 'grace_period'

class Example
  include GracePeriod::Core

  GracePeriod expires_at: Time.now - 1.second
  def my_method
    puts "hello from my_method!"
  end
end

Example.new.my_method
WARNING: Invoking a method with an expired GracePeriod in example#my_method
```
Again, it is worth mentioning that Grace Period will use its default values unless instructed otherwise, and we strive to be informative but unobtrusive by default, so an expired method will execute with a warning, but you could alter the default behavior in the configuration or you could add additional arguments to specify the response to an encountered expiration, like this:
```ruby
GracePeriod expires_at: (Time.now - 1.second), respond_by: :exception
```
You can find the [complete list of arguments]('#') below. You can accomplish most typical use cases using the built-in parameters, but Grace Period also lets you pass in code blocks for most values, so that you can have maximum flexibility to add whatever logic makes sense for your app to identify and alert about expirations and grace periods. Find out more on our section on [blocks, procs and lambdas]('#').

## Tutorial

We've put together a [spiffy tutorial]('#') for you, as well.