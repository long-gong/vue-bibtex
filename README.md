# BibTeX-js Combining with vue.js

"BibTeX-js can parse a BibTeX-file and render it as part of an HTML file. This way, you can easily add a list of publications to your private homepage or display a list of recommended publications for a seminar. The way the entries are display can be customized using a simple template system and CSS." -- from [BibTeX-js](https://github.com/pcooksey/bibtex-js)

"**Vue.js** (commonly referred to as Vue; pronounced /vjuː/, like view) is an open-source JavaScript framework for building user interfaces and single-page applications." -- from [Wikipedia](https://en.wikipedia.org/wiki/Vue.js)

## Why Not Just Use [BibTeX-js](https://github.com/pcooksey/bibtex-js)

`BibTeX-js` works as an excellent BibTeX parser which can parse a BibTeX file and render it as part of an HTML file. However, usually we do want to customize the "rendering". `BibTeX-js`indeed gives some degree of freedom to customization. For example, users can provide their own "rendering" template. However, the syntax for such a template is a little bit hard to adapt to. Besides, sometimes users might want customize certain BibTeX entry. More precisely, users might want to modify the "data" (BibTeX entries). For example, users might want to highlight certain authors for each of her/his publication or provide links for herself/himself and/or her/his colleagues. 

In this "project", we simplified `BibTeX-js` by removing its rendering  functionality. Now, it is a "pure" JavaScript-based BiBTeX parser. Instead, we use `vue.js` to do the rendering. Combining with `vue.js`, users can use the familiar [MVC](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) framework to build their webpages. More precisely, users can use `BibTeX-js` to parse a BibTeX file (might need `ajax` to get the content of the BibTeX file) or just a text area filling with BibTeX entries, and then passing the parsed entries as data to `vue.js`. Then, `vue.js`, according to user's customized template, to render users' publication.

## Demos

[Live Demo](https://network-theory-group.github.io)

### Usage

Put `BibTeX-js` and `vue.js` in your HTML.
```html
<script src="/path/to/bibtex_js.js" type="text/javascript" charset="utf-8"></script>
<!-- development version, includes helpful console warnings -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

Now you can use them in your HTML, the following presents an example.
```html
<div class="container">
  <div id="app" v-bind:class="row">
    <h1>Rendered</h1>
    <ul>
      <li v-for="entry in bib_entries">
        <span v-if="entry.TITLE" class="title">{{ entry.TITLE }}</span>,
        <div v-if="entry.AUTHOR">
          <span class="author">{{ entry.AUTHOR }}</span>
        </div>
        <div>
          <span v-if="entry.JOURNAL"
            ><em><span class="journal">{{ entry.JOURNAL }}</span></em></span
          >
          <span v-if="entry.MONTH"
            ><span class="month">{{ entry.MONTH }}</span>,</span
          >
          <span v-if="entry.YEAR"
            ><span class="year">{{ entry.YEAR }}</span></span
          >.
        </div>
      </li>
    </ul>
  </div>
</div>

<script>
  var app = new Vue({
    el: "#app",
    data: {
      bib_entries: []
    },
    created: function() {
      var self = this;
      $.ajax({
        url: "test.bib",
        method: "GET",
        success: function(data) {
          var b = new BibtexParser();
          b.setInput(data);
          b.bibtex();
          self.bib_entries = b.getEntries();
        },
        error: function(error) {
          console.log(error);
        }
      });
    }
  });
</script>
```

Complete codes can be found [here](demo/bibtex_vue.html). If you want to post-process those BibTeX entries before rendering them, please refer to 
[Live Demo](https://network-theory-group.github.io) where we provide a simple approach to accomplish it.

## Notice

[Modified from (c) 2010 Henrik Muehe. MIT License](https://code.google.com/p/bibtex-js/)
