# Steps to create SVG from Server-Side Rendering

To get a basic understanding of SVG with SSR, read [this tutorial from smashingmagazine.com](https://www.smashingmagazine.com/2014/05/love-generating-svg-javascript-move-to-server/). According to the article, there are four reasons why one wants to move rendering of your SVG to server-side:

1. To enable the user to download a graphic.
2. To enable a graphic to inserted in an email. 
3. To enable a graphic to be displayed on another website.
4. To improve performance.

And it mentioned a few ways to achieve SVG with SSR:

1. render in the client and post it to a server
2. phantomjs 
3. jsdom
4. svable

----

## Highcharts

Highcharts used phantomjs approach, and it has created a [node-export-server](https://github.com/highcharts/node-export-server) to make it easy for developers to render highchart SVGs (or Map, Stock).

### Steps

1. Create `package.json`

```json
{
  "name": "ssr-svg",
  "version": "1.0.0",
  "dependencies": {
    "highcharts-export-server": "^2.0.28"
  }
}
```
and then run to install dependencies
```
yarn install
```

2. Create `index.js` file like this:

```js
const fs = require("fs");
const chartExporter = require("highcharts-export-server");
chartExporter.initPool();

const chartDetails = {
   type: "svg",
   options: {}
};
chartExporter.export(chartDetails, (err, res) => {
   let imageb64 = res.data;
   let outputFile = "test.svg";
   fs.writeFileSync(outputFile, imageb64, "base64", function(err) {
       if (err) console.log(err);
   });
   console.log("Saved image!");
   chartExporter.killPool();
});
```

3. Replace `chartDetails.options` with your current highchart options with data

4. Go to terminal and run

```bash
node index.js
```

Should see a file `undefinedchart.svg` is created. 

### Todo

1. To figure out how to start node server with it
2. The filename `undefinedchart.svg` looks like a broken value. Needs to find the place to enter the name.
