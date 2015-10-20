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
GracePeriod :expires_at Date.parse("Tue, 10 Aug 2016 01:20:19 -0500 (EDT)")
def my_method
```
