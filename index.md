---
layout: default
---

<div class="center-box">
  <form id="form" class="licenses-form">
    <div class="form-container">
      {% capture w %}{{site.defaultvalues.width}}{% endcapture %}
      {% include form_row.html name='Width' id='w' value=w %}
      
      {% capture h %}{{site.defaultvalues.height}}{% endcapture %}
      {% include form_row.html name='Height' id='h' value=h %}
      
      {% capture margin %}{{site.defaultvalues.margin}}{% endcapture %}
      {% include form_row.html name='Margin' id='margin' value=margin units="px" %}
      
      {% capture pages %}{{site.defaultvalues.pages}}{% endcapture %}
      {% include form_row.html name='Pages' id='pages' value=pages %}
      
      {% capture columns %}{{site.defaultvalues.columns}}{% endcapture %}
      {% include form_row.html name='Columns Per Page' id='columns' value=columns %}
      
      {% capture scale %}{{site.defaultvalues.scale}}{% endcapture %}
      {% include form_row.html name='Scale' id='scale' value=scale %}
    </div>
    
    <input class="submit-button" type="submit" value="Create images">
  </form>
</div>

<h2>Preview:</h2>
<div id="capture" class="licenses">

{% for license in site.data.licenses %}
<h2>
{{ license.name }}
</h2>
<pre>
{% capture x %}{% include licenses/{{ license.license }} %}{% endcapture %}
{{ x | escape }}
</pre>
{% if forloop.last == false %}
<hr>
{% endif %}
{% endfor %}

{% if site.data.sounds.size > 0 %}
<hr>
<h2>Sound Effect Attributions</h2>
<pre>
Below is a list of attributions for sounds which were used or referenced in the production of this software.
{% for sfx in site.data.sounds %}
{{sfx.name}}
- Author: {{sfx.author}}
- License: {{sfx.license}}
- URL: {{sfx.url}}
{% endfor %}
</pre>
{% endif %}


</div>

<h2 id="results-title">Results: (right click > Save As on each)</h2>
<div id="results">
</div>

<script
        src="https://code.jquery.com/jquery-3.4.1.slim.min.js"
        integrity="sha256-pasqAKBDmFT4eHoN2ndd6lN370kFiGUFyTiUHWhU7k8="
        crossorigin="anonymous"></script>
<script type="text/javascript" src="https://unpkg.com/html2canvas@1.0.0-rc.5/dist/html2canvas.js"></script>
<script>
function mainFunction(setuponly) {
  var w = $("#w").val();
  var h = $("#h").val();
  var pages = $("#pages").val();
  var columns = $("#columns").val();
  var margin = $("#margin").val();

  w -= margin * 2;
  h -= margin * 2;

  $("#capture").width(w*pages).height(h);
  $("#capture").css("column-count", pages*columns);

  if (setuponly) {
    $("#results-title").hide();
    return false;
  }
  
  $("#results-title").show();

  $("#results").empty();

  var scale = $("#scale").val();

  for (var i = 0; i < pages; i++) {
    var canvas = document.createElement('canvas');
    canvas.width = w*scale;
    canvas.height = h*scale;
    canvas.style.width = w + 'px';
    canvas.style.height = h + 'px';
    var context = canvas.getContext('2d');
    context.scale(scale,scale);

    var params = {
      canvas: canvas,
      scale: 1,
      x: i*w,
      backgroundColor: '#ffffff'
    };

    html2canvas(document.querySelector("#capture"), params).then(canvas => {
        $("#results").append(canvas);
    });
  }
}

$("form").submit(function(){
  mainFunction(false);
  
  alert("Success! Check the bottom of the page.");

  return false;
});

$(document).ready(function() {
  mainFunction(true);
});
</script>
