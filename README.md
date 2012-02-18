This program reproduces a bug in trinidad_resque_extension. When resque cannot connect to Redis, the process hangs.

To this:

    $ redis-server
    [26298] 17 Feb 20:28:37 # Warning: no config file specified, using the default config. In order to specify a config file use 'redis-server /path/to/redis.conf'
    [26298] 17 Feb 20:28:37 * Server started, Redis version 2.2.12
    [26298] 17 Feb 20:28:37 * The server is now ready to accept connections on port 6379
    [26298] 17 Feb 20:28:37 - 0 clients connected (0 slaves), 922160 bytes in use
    [26298] 17 Feb 20:28:42 - 0 clients connected (0 slaves), 922160 bytes in use

Then do this in another terminal from the project root:

    $ bundle exec trinidad -t

Then do this in another terminal:

    $ ps aux | grep java
    user1        27563   0.5  3.1  3303172 130400 s004  S+   11:20AM   0:14.37 /System/Library/Frameworks/JavaVM.framework/Versions/1.6.0/Home/bin/java -Dfile.encoding=UTF-8 -Djdk.home= -Djruby.home=/Users/user1/.rvm/rubies/jruby-1.6.5 -Djruby.script=jruby -Djruby.shell=/bin/sh -Djffi.boot.library.path=/Users/user1/.rvm/rubies/jruby-1.6.5/lib/native/Darwin -Xmx500m -Xss2048k -Djruby.memory.max=500m -Djruby.stack.max=2048k -Dsun.java.command=org.jruby.Main -Djava.class.path= -Xbootclasspath/a:/Users/user1/.rvm/rubies/jruby-1.6.5/lib/jruby.jar org/jruby/Main /Users/user1/.rvm/gems/jruby-1.6.5/bin/bundle exec trinidad -t
    user1        27569   0.0  0.0  2434892    544 s003  S+   11:20AM   0:00.00 grep java
    user1        27565   0.0  4.3  3297476 180844 s004  S+   11:20AM   0:24.51 /System/Library/Frameworks/JavaVM.framework/Versions/1.6.0/Home/bin/java -Dfile.encoding=UTF-8 -Djdk.home= -Djruby.home=/Users/user1/.rvm/rubies/jruby-1.6.5 -Djruby.script=jruby -Djruby.shell=/bin/sh -Djffi.boot.library.path=/Users/user1/.rvm/rubies/jruby-1.6.5/lib/native/Darwin -Xmx500m -Xss2048k -Djruby.memory.max=500m -Djruby.stack.max=2048k -Dsun.java.command=org.jruby.Main -Djava.class.path= -Xbootclasspath/a:/Users/user1/.rvm/rubies/jruby-1.6.5/lib/jruby.jar org/jruby/Main /Users/user1/.rvm/gems/jruby-1.6.5/bin/trinidad -t

Then kill the Trinidad server with Ctrl+C and run this again:

    $ ps aux | grep java
    user1        27603   0.0  0.0  2434892    404 s003  R+   11:20AM   0:00.00 grep java

Now kill the Redis process with Crtl+C and restart the Trinidad server:

    $ bundle exec trinidad -t

After the server is running, kill it with Ctrl+C and check the processes again:

    $ ps aux | grep java
    user1        27489   0.0  0.0  2434892    460 s003  R+   11:19AM   0:00.00 grep java
    user1        27433   0.0  4.3  3297280 178872 s004  S    11:19AM   0:24.79 /System/Library/Frameworks/JavaVM.framework/Versions/1.6.0/Home/bin/java -Dfile.encoding=UTF-8 -Djdk.home= -Djruby.home=/Users/user1/.rvm/rubies/jruby-1.6.5 -Djruby.script=jruby -Djruby.shell=/bin/sh -Djffi.boot.library.path=/Users/user1/.rvm/rubies/jruby-1.6.5/lib/native/Darwin -Xmx500m -Xss2048k -Djruby.memory.max=500m -Djruby.stack.max=2048k -Dsun.java.command=org.jruby.Main -Djava.class.path= -Xbootclasspath/a:/Users/user1/.rvm/rubies/jruby-1.6.5/lib/jruby.jar org/jruby/Main /Users/user1/.rvm/gems/jruby-1.6.5/bin/trinidad -t

That one shouldn't be there!