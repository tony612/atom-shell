# Technical differences to node-webkit

Like node-webkit, atom-shell provides a platform to write desktop applications
with JavaScript and HTML, and has node integration to grant access to low level
system in web pages.

But there are also fundamental differences between the two projects that making
atom-shell a completely product from node-webkit:

**1. Entry of application**

In node-webkit, the main entry of an application is a web page, you specify a
main page in the `package.json` and it would be opened in a browser window as
the application's main window.

While in atom-shell, the entry point is a JavaScript script, instead of
providing a URL directly, you need to manually create a browser window and load
html file in it with corresponding API. You also need to listen to window events
to decide when to quit the application.

So atom-shell works more like the node.js runtime, and APIs are more low level,
you can also use atom-shell for web testing purpose like
[phantomjs](http://phantomjs.org/),

**2. Build system**

In order to avoid the complexity of building the whole Chromium, atom-shell uses
[libchromiumcontent](https://github.com/brightray/libchromiumcontent) to access
Chromium's Content API, libchromiumcontent is a single, shared library that
includes the Chromium Content module and all its dependencies. So users don't
need a powerful machine to build atom-shell.

**3. Node integration**

In node-webkit, the node integration in web pages requires patching Chromium to
work, while in atom-shell we chose a different way to integrate libuv loop to
each platform's message loop to avoid hacking Chromium, see the
[`node_bindings`](../../atom/common/) code for how that was done.

**4. Multi-context**

If you are an experienced node-webkit user, you should be familiar with the
concept of node context and web context, these concepts were invented because
of how the node-webkit was implemented.

By using the [multi-context](http://strongloop.com/strongblog/whats-new-node-js-v0-12-multiple-context-execution/)
feature of node, atom-shell doesn't introduce a new JavaScript context in web
pages.
