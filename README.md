# IRuby

This is a Ruby kernel for IPython.

It adds a special `iruby_profile` command for staging some customization
that enables the Ruby kernel by default, and sets syntax-highlighting in the notebook
to Ruby mode.

### Usage

Clone this repository and run `bin/iruby_profile` to create the profile, then
use IPython as usual:

```bash
git clone git://github.com/minrk/iruby
cd iruby
# build and install IRuby
gem build iruby.gemspec
$ gem install iruby-*.gem
# Create an IPython profile with default config
$ iruby_profile --create
$ ipython notebook --profile=ruby
```


## Background

### Building an in-browser REPL for Ruby (IRuby)

Hey, I'm Josh Adams.  I'm a partner and CTO at isotope|eleven.  We alo host
Birmingham, AL's Open Source Software meetup - BOSS.

At one of these sessions in early 2012, Tom Brander did a presentation and used
IPython in his browser to manage it (there was much code and it was executed
live).  This was the first time I'd seen IPython in the browser where it
actually worked like it was supposed to, and I was extremely impressed.

If you've not seen IPython, it looks like this <* Insert Screenshot Here *> in
its web-browser mode.  It also manages a lot of console-basd REPLs.

Anyway, it has notebooks that you can save out to execute later, and you can
pass them around as little code snippets for other people to check out.  It's
very impressive.

But I'm primarily a Rubyist, and I'm happy that way :)  I couldn't sit by while
Python had this awesome tool that we lacked.  I looked around for a bit, and
there was nothing like IPython in our ecosystem.  There were, however, quite a
few people asking about it.  So I figured I'd do something about it.

#### The Architecture

So the IPython guys did a great job explaining their core architecture, both in
words and in pared-down code, in a blog post they wrote concerning it.  In
general, it works like this <* Diagram *>

There's a kernel that runs in the background and gets connected to by a
frontend.  They communicate using zeromq, and they send json formatted messages
back and forth.  These messages are in a very well defined structure.  Anyway,
this way the frontend of the repl is disconnected from the environment that's
running it.

So the code repository they linked to in their blog post included the kernel and
the frontend as small-ish python files - around 300 and 200 lines respectively.
We had a hack weekend at isotope|eleven where myself and Robby Clements got
together and (when we weren't playing Counterstrike1.6) did the closest thing to
a straight port that we could swing.  Within about 2 hours of work, we had a
working proof of concept that was primarily a 1 to 1 port.

The next move was to build the web frontend.  This just consists of a websocket
server and a fairly basic frontend webpage.
