# BibTeX-js Combining with vue.js

"BibTeX-js can parse a BibTeX-file and render it as part of an HTML file. This way, you can easily add a list of publications to your private homepage or display a list of recommended publications for a seminar. The way the entries are display can be customized using a simple template system and CSS." -- from [BibTeX-js](https://github.com/pcooksey/bibtex-js)

"**Vue.js** (commonly referred to as Vue; pronounced /vjuː/, like view) is an open-source JavaScript framework for building user interfaces and single-page applications." -- from [Wikipedia](https://en.wikipedia.org/wiki/Vue.js)

## Why Not Just Use [BibTeX-js](https://github.com/pcooksey/bibtex-js)

To be added.

## Demos

[Live Demo](https://network-theory-group.github.io)

### Usage

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
    mounted: function() {
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

Complete codes can be found [here](demo/bibtex_vue.html).

## Notice

[Modified from (c) 2010 Henrik Muehe. MIT License](https://code.google.com/p/bibtex-js/)
