<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="description" content="">

</head>


<body>

<div class="post-header">
   <h1 class="post-title-main">Hello world</h1>
</div>



<div class="post-content">

<div id="toc"></div>

  <p>After starting Ditto, we have a HTTP and WebSocket API for your Digital Twins at our hands.</p>

<h2 id="example">Example</h2>

<p>Assume we want to create a Digital Twin for a car. The twin should hold static metadata and dynamic state data. The state data should change as often as its real world counterpart does.</p>

<p>Those static and dynamic types of data are mapped in the Ditto model to “attributes” (for static metadata) and “features” (for dynamic state data).
A JSON representation of some metadata and state data could for example look like this:</p>

<div class="language-json highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="p">{</span><span class="w">
  </span><span class="s2">"attributes"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
    </span><span class="s2">"manufacturer"</span><span class="p">:</span><span class="w"> </span><span class="s2">"ACME"</span><span class="p">,</span><span class="w">
    </span><span class="s2">"VIN"</span><span class="p">:</span><span class="w"> </span><span class="s2">"0815666337"</span><span class="w">
  </span><span class="p">},</span><span class="w">
  </span><span class="s2">"features"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
    </span><span class="s2">"transmission"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
      </span><span class="s2">"properties"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
        </span><span class="s2">"automatic"</span><span class="p">:</span><span class="w"> </span><span class="kc">true</span><span class="p">,</span><span class="w">
        </span><span class="s2">"mode"</span><span class="p">:</span><span class="w"> </span><span class="s2">"eco"</span><span class="p">,</span><span class="w">
        </span><span class="s2">"cur_speed"</span><span class="p">:</span><span class="w"> </span><span class="mi">90</span><span class="p">,</span><span class="w">
        </span><span class="s2">"gear"</span><span class="p">:</span><span class="w"> </span><span class="mi">5</span><span class="w">
      </span><span class="p">}</span><span class="w">
    </span><span class="p">},</span><span class="w">
    </span><span class="s2">"environment-scanner"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
      </span><span class="s2">"properties"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
        </span><span class="s2">"temperature"</span><span class="p">:</span><span class="w"> </span><span class="mf">20.8</span><span class="p">,</span><span class="w">
        </span><span class="s2">"humidity"</span><span class="p">:</span><span class="w"> </span><span class="mi">73</span><span class="p">,</span><span class="w">
        </span><span class="s2">"barometricPressure"</span><span class="p">:</span><span class="w"> </span><span class="mf">970.7</span><span class="p">,</span><span class="w">
        </span><span class="s2">"location"</span><span class="p">:</span><span class="w"> </span><span class="p">{</span><span class="w">
          </span><span class="s2">"longitude"</span><span class="p">:</span><span class="w"> </span><span class="mf">47.682170</span><span class="p">,</span><span class="w">
          </span><span class="s2">"latitude"</span><span class="p">:</span><span class="w"> </span><span class="mf">9.386372</span><span class="w">
        </span><span class="p">},</span><span class="w">
        </span><span class="s2">"altitude"</span><span class="p">:</span><span class="w"> </span><span class="mi">399</span><span class="w">
      </span><span class="p">}</span><span class="w">
    </span><span class="p">}</span><span class="w">
  </span><span class="p">}</span><span class="w">
</span><span class="p">}</span><span class="w">
</span></code></pre></div></div>

<p>Background: Ditto only knows about “attributes” and “features”.</p>

<p>Inside “attributes” (the metadata) we can add as much JSON keys as we like with any JSON value we need.</p>

<p>Inside “features” (the state data) we can add as much features as we like - but each feature needs to have a “properties” JSON object.
Inside that JSON object we can add as much JSON keys as we like with any JSON value we need.</p>

<h2 id="creating-your-first-thing">Creating your first Thing</h2>

<p>We create a Thing for the example from above by using <a href="https://github.com/curl/curl">cURL</a>. Basic authentication will use the credentials of a user “ditto”. 
Those credentials have been created by default in the <a href="https://github.com/nginx/nginx">nginx</a> started via “docker”. 
(See <a href="https://github.com/eclipse/ditto/blob/master/deployment/docker/README.md">ditto/deployment/docker/README.md</a>)</p>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code>curl <span class="nt">-u</span> ditto:ditto <span class="nt">-X</span> PUT <span class="nt">-d</span> <span class="s1">'{
   "attributes": {
     "manufacturer": "ACME",
     "VIN": "0815666337"
   },
   "features": {
     "transmission": {
       "properties": {
         "automatic": true, 
         "mode": "eco",
         "cur_speed": 90, 
         "gear": 5
       }
     },
     "environment-scanner": {
       "properties": {
         "temperature": 20.8,
         "humidity": 73,
         "barometricPressure": 970.7,
         "location": {
           "longitude": 47.682170,
           "latitude": 9.386372
         },
         "altitude": 399
       }
     }
   }
 }'</span> <span class="s1">'http://localhost:8080/api/1/things/org.eclipse.ditto:fancy-car'</span>
</code></pre></div></div>

<p>The result is a Digital Twin in Thing notation. The Thing ID is <code class="highlighter-rouge">org.eclipse.ditto:fancy-car</code>. An ID must always contain a 
namespace before the <code class="highlighter-rouge">:</code>. That way Things are easier to organize.</p>

<h2 id="querying-an-existing-thing">Querying an existing Thing</h2>

<p>By creating the Digital Twin as a Thing with the specified JSON format, Ditto implicitly provides an API for
our Thing.</p>

<p>For Things we know the ID of, we can simply query them by their ID:</p>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code>curl <span class="nt">-u</span> ditto:ditto <span class="nt">-X</span> GET <span class="s1">'http://localhost:8080/api/1/things/org.eclipse.ditto:fancy-car'</span>

<span class="c"># if you have python installed, that's how to get a prettier response:</span>
curl <span class="nt">-u</span> ditto:ditto <span class="nt">-X</span> GET <span class="s1">'http://localhost:8080/api/1/things/org.eclipse.ditto:fancy-car'</span> | python <span class="nt">-m</span> json.tool
</code></pre></div></div>

<h2 id="querying-one-specific-state-value">Querying one specific state value</h2>

<p>The created API for our Thing also provides HTTP endpoints for each attribute and feature property.</p>

<p>That way we can for example just retrieve the <code class="highlighter-rouge">cur_speed</code> of our fancy car:</p>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code>curl <span class="nt">-u</span> ditto:ditto <span class="nt">-X</span> GET <span class="s1">'http://localhost:8080/api/1/things/org.eclipse.ditto:fancy-car/features/transmission/properties/cur_speed'</span>
</code></pre></div></div>

<h2 id="updating-one-specific-state-value">Updating one specific state value</h2>

<p>We can just as easy use the HTTP API to update one attribute or feature property, e.g. update the <code class="highlighter-rouge">cur_speed</code> to <code class="highlighter-rouge">77</code>:</p>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code>curl <span class="nt">-u</span> ditto:ditto <span class="nt">-X</span> PUT <span class="nt">-d</span> <span class="s1">'77'</span> <span class="s1">'http://localhost:8080/api/1/things/org.eclipse.ditto:fancy-car/features/transmission/properties/cur_speed'</span>
</code></pre></div></div>

<h2 id="searching-for-all-things">Searching for all Things</h2>

<p>When we lost the overview which Things we have already created, we can use the <code class="highlighter-rouge">search</code> HTTP endpoint,
e.g. searching all Things with the same <code class="highlighter-rouge">manufacturer</code> named <code class="highlighter-rouge">"ACME"</code>:</p>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code>curl <span class="nt">-u</span> ditto:ditto <span class="nt">-X</span> GET <span class="s1">'http://localhost:8080/api/1/search/things?filter=eq(attributes/manufacturer,"ACME")'</span>.
</code></pre></div></div>

</div>

</body>
</html>
