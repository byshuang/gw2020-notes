# How to develop Google Slide Add-on

I followed [the tutorial](https://developers.google.com/apps-script/quickstart/custom-functions) first to understand the follow of developing scripts for one document (doc, sheet, slide, form) only.

Then here's a couple of snippets of how things work

1. add menu
```js
function onOpen() {
  SlidesApp.getUi() 
      .createMenu('Custom Menu')
      .addItem('Show sidebar', 'showSidebar')
      .addToUi();
}
```

2. append new slide to the presentation
```js
function appendSlide() {
  SlidesApp.getActivePresentation()
      .appendSlide();
}
```

3. add an image to the last slide
```js
function addImage() {
  appendSlide();
  var slides = SlidesApp.getActivePresentation()
      .getSlides();
  
  var newSlide = slides[slides.length - 1];
  newSlide.insertImage("https://image.com/x.jpg");
}
```

4. serve a HTML on the side bar
```js
function showSidebar() {
  var html = HtmlService.createHtmlOutputFromFile('Page')
      .setTitle('My custom sidebar')
      .setWidth(300);
  SlidesApp.getUi() // Or DocumentApp or SlidesApp or FormApp.
      .showSidebar(html);
}
```

Add `Page.html` too in the folder
```html
<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    hello word
  <body>
    <div id="output"></div>
  </body>
</html>

```

5. Page.html ajax call
```html
    <script>
      function onSuccess(cats) {
        var div = document.getElementById('output');
     for (var i = 0; i < cats.length; i++) {
     div.innerHTML += '<img draggable src="' + cats[i].file + '" />';
     }
      
  
      }

      google.script.run.withSuccessHandler(onSuccess)
          .getCats();
    </script>

```
and in the `Code.gs`

```js
function getCats() {
  var cats = [];
  var url = "https://aws.random.cat/meow";
  for (var i = 0; i < 3; i ++) {
    var response = UrlFetchApp.fetch(url, {'muteHttpExceptions': true});
    var json = response.getContentText();
    var data = JSON.parse(json);
    cats.push(data);
  }
  return cats;
}
```

