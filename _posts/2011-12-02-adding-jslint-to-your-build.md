---
id: 818
title: Adding JSLint To Your Build
date: 2011-12-02T05:30:32+00:00
author: derekgreer
layout: post
guid: http://www.aspiringcraftsman.com/?p=818
permalink: /2011/12/02/adding-jslint-to-your-build/
dsq_thread_id:
  - "628049894"
categories:
  - Uncategorized
tags:
  - JavaScript
  - JSLint
  - Node
  - Rake
---
[JSLint](http://jslint.com/), <noindex></noindex> the popular JavaScript static code analysis tool written by Douglas Crockford, can fairly easily be incorporated into your builds through the use of [node](http://nodejs.org/) and the [jslint node module](https://github.com/reid/node-jslint).&#160; The following steps will show you how to add JSLint to an existing rake build:

## Step 1: Install Node

If you don’t already have it, go download and install node from <http://nodejs.org/>.

## Step 2: Add JSLint Method

The following jslint method uses the [Node Package Manager](http://npmjs.org/) (npm) to install the jslint module into the current folder if it does not already exist.&#160; If node or npm are not found in the execution path then an error message is printed and the method simply returns.&#160; The method uses a hash config parameter for its parameters.

<pre class="prettyprint">def jslint(config)
    tool = 'jslint'
    flags = config['flags'] || ''

    node = which('node')
    if(node.nil?)
        puts "Could not find node in your path."
        return
    end

    npm = which('npm')
    if(npm.nil?)
        puts "Could not find npm in your path."
        return
    end

    if(!File.directory?("node_modules/#{tool}"))
        sh "\"#{npm}\" install #{tool}"
    end

    scripts = config['scripts'] || [""]
    sh "\"#{node}\" node_modules/#{tool}/bin/#{tool}.js #{flags} #{scripts}"
end

def which(cmd)
    exts = ENV['PATHEXT'] ? ENV['PATHEXT'].split(';') : ['']
    ENV['PATH'].split(File::PATH_SEPARATOR).each do |path|
    exts.each { |ext|
        sep = File::ALT_SEPARATOR || File::SEPARATOR
        exe = "#{path}#{sep}#{cmd}#{ext}"
        return exe if File.executable? exe
    }
    end
    return nil
end</pre>

## Step 3: Add a Lint Task

After including the jslint method defined above, add a new rake task which calls the method with the desired config values:

<pre class="prettyprint">task :lint do
        config = { 'flags'   =&gt; '--white',
                   'scripts' =&gt; FileList.new("src/**/app/*.js") }
        jslint(config)
    end</pre>

That’s all you need.&#160; Now you can run “rake lint” or tie this into to your existing process as you see fit.&#160; Enjoy!