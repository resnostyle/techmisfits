---
layout: post
title:  "Resque Cheatsheet"
date:   2014-11-30 14:02:01
categories: cheatsheet resque
---

#### Borrowed from: [whatcodecraves](http://whatcodecraves.com/posts/2010/06/22/resque-cheatsheet.html)

Working with Resque is best when you have a cheatsheet of the various commands you need to interact with it.

# Status

<pre>
  <code class="ruby">
Resque.info
Resque.queues
Resque.redis
Resque.size(queue_name)

# check out what's coming next in the queue
#    Resque.peek(archive_queue)
#    Resque.peek(archive_queue, 1, 5)
#    Resque.peek(archive_queue, 59, 30)
Resque.peek(queue_name, start=1, count=1)

  </code>
</pre>

# Workers

<pre>
  <code class="ruby">
Resque.workers
Resque.working
Resque.remove_worker(worker_id) # find worker_id from one of the above methods
  </code>
</pre>

# Queue Management 

<pre>
  <code class="ruby">
# For testing a worker, I usually call the 'perform' method directly.
#    Resque.enqueue(ArchiveWorker)
#    Resque.enqueue(ArchiveWorker, 'matching', 'arguments')
Resque.enqueue(klass, *args)
Resque.dequeue(klass, *args)
Resque.remove_queue(queue_name)
  </code>
</pre>

# Callbacks

<pre> 
  <code class="ruby">
# Each of these can either take a block, or be assigned to with a Proc
Resque.before_first_fork(&blk)
Resque.before_fork(&blk)
Resque.after_fork(&blk)
  </code>
</pre>

# Problems

Redis connects to wrong host - Redis connects to localhost:6379 by default. Customize this by doing the following:

<pre>
  <code class="ruby">
Resque.redis = 'hostname:port:db'  # all 3 values are optional
  </code>
</pre>

Workers die stop after first batch completes - This is caused by the workers losing their connection to MySQL. See this gist for a fix and an explanation. Alternatively, you can add this line at the beginning of your 'perform' method:

<pre>
  <code class="ruby">
ActiveRecord::Base.reconnect!
  </code>
</pre>
