<h1><a name="Overview"></a>Overview<a href="#Overview" class="section_anchor"></a></h1><p>Drop-in server for <a href="http://code.google.com/appengine/" rel="nofollow">Google App Engine</a> applications which exposes your data model via a REST  API with no extra work. </p><h2><a name="Basic_Features"></a>Basic Features<a href="#Basic_Features" class="section_anchor"></a></h2><ul><li>Metadata browsing </li><ul><li>List models: <tt>GET &quot;/metadata&quot;</tt> </li><li>Define model in XML Schema: <tt>GET &quot;/metadata/MyModel&quot;</tt> </li></ul><li>Reading data (XML output) </li><ul><li>Get specific instance: <tt>GET &quot;/MyModel/&lt;key&gt;&quot;</tt> </li><li>Get all instances (paged results): <tt>GET &quot;/MyModel&quot;</tt> </li><ul><li>Results can be ordered using the &quot;ordering&quot; query param: <tt>&quot;?ordering=&lt;propertyName&gt;&quot;</tt> </li></ul><li>Find multiple instances (paged results): <tt>GET &quot;/MyModel?&lt;queryParams&gt;&quot;</tt> </li><ul><li>Results can be ordered (same as get all) </li></ul></ul><li>Modifying data (XML input) </li><ul><li>Create new instance (returns key): <tt>POST &quot;/MyModel&quot;</tt> </li><ul><li>Supports batch create by surrounding multiple instances with <tt>&lt;list&gt;</tt> element (returns keys) </li></ul><li>Partial update instance (returns key): <tt>POST &quot;/MyModel/&lt;key&gt;&quot;</tt> </li><ul><li>Supports batch update (same as create) </li></ul><li>Complete update instance (returns key): <tt>PUT &quot;/MyModel/&lt;key&gt;&quot;</tt> </li><ul><li>Supports batch update (sames as create) </li></ul><li>Output of all update operations may be altered with &quot;type&quot; query param </li><ul><li>Return the keys in an XML format: <tt>&quot;?type=structured&quot;</tt> </li><li>Return the entire updated models: <tt>&quot;?type=full&quot;</tt> </li></ul><li>Delete instance: <tt>&quot;DELETE &quot;/MyModel/&lt;key&quot;</tt> </li></ul></ul><p>Check out the <a href="/p/appengine-rest-server/wiki/Features">Features</a> page for a more complete list of the features supported by the appengine rest server, including advanced features. </p><h2><a name="Example"></a>Example<a href="#Example" class="section_anchor"></a></h2><p>The <a href="http://boomi-demo.appspot.com/" rel="nofollow">Boomi Demo</a> App Engine application is a fully working example application (based on the Google App Engine Greeting demo application). </p><ul><li>Available types: <a href="http://boomi-demo.appspot.com/rest/metadata" rel="nofollow">http://boomi-demo.appspot.com/rest/metadata</a> </li><li>Greeting schema: <a href="http://boomi-demo.appspot.com/rest/metadata/Greeting" rel="nofollow">http://boomi-demo.appspot.com/rest/metadata/Greeting</a> </li><li>Greeting instances: <a href="http://boomi-demo.appspot.com/rest/Greeting" rel="nofollow">http://boomi-demo.appspot.com/rest/Greeting</a> </li><li>Greeting instances with filter: <a href="http://boomi-demo.appspot.com/rest/Greeting?feq_author=bob@example.com&amp;feq_date=2008-11-03T00:21:19.080553" rel="nofollow">http://boomi-demo.appspot.com/rest/Greeting?feq_author=bob@example.com&amp;feq_date=2008-11-03T00:21:19.080553</a> </li><li>Data accessible in both XML and JSON format (input can be specified using the HTTP &quot;Content-Type&quot; header, output can be specified using the HTTP &quot;Accept&quot; request header, e.g. &quot;application/xml&quot; or &quot;application/json&quot;) </li></ul><h2><a name="Setup"></a>Setup<a href="#Setup" class="section_anchor"></a></h2><p>Utilizing this library is extremely simple.  See the <a href="/p/appengine-rest-server/wiki/GettingStarted">Getting Started</a> page to find out how to integrate the appengine rest server into your project. </p>

Copyright 2008 Boomi, Inc.
All rights reserved.

Basic REST server for Google App Engine Applications using the builtin Datastore API.

For extended feature list, see http://code.google.com/p/appengine-rest-server/

========

For example client usage, see example.txt.

========


Getting Started
---------------

To use with an existing application:


import rest

# add a handler for "rest" calls
application = webapp.WSGIApplication([
  <... existing webservice urls ...>
  ('/rest/.*', rest.Dispatcher)
], ...)

# configure the rest dispatcher to know what prefix to expect on request urls
rest.Dispatcher.base_url = "/rest"

# add all models from the current module, and/or...
rest.Dispatcher.add_models_from_module(__name__)
# add all models from some other module, and/or...
rest.Dispatcher.add_models_from_module(my_model_module)
# add specific models
rest.Dispatcher.add_models({
  "foo": FooModel,
  "bar": BarModel})
# add specific models (with given names) and restrict the supported methods
rest.Dispatcher.add_models({
  "foo" : (FooModel, rest.READ_ONLY_MODEL_METHODS),
  "bar" : (BarModel, ["GET_METADATA", "GET", "POST", "PUT"],
  "cache" : (CacheModel, ["GET", "DELETE"] })

# use custom authentication/authorization
rest.Dispatcher.authenticator = MyAuthenticator()
rest.Dispatcher.authorizer = MyAuthorizer()

