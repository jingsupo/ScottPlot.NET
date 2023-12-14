---
Title: "SignalXY with Fill - ScottPlot 4.1 Cookbook"
Description: "Various options allow shading above/below the signal data."
Date: 2023-12-13
Version: ScottPlot 4.1.69
URL: /cookbook/4.1/recipes/signalxy_fillbelow/
BreadcrumbNames: ["ScottPlot 4.1 Cookbook", "SignalXY", "SignalXY with Fill"]
BreadcrumbUrls: ["/cookbook/4.1/", "/cookbook/4.1/category/plottable-signalxy", "/cookbook/4.1/recipes/signalxy_fillbelow/"]
SearchUrl: "/cookbook/4.1/search/"
OgImage: "/cookbook/4.1/images/signalxy_fillbelow.png"
---

<h2><a id='signalxy-with-fill' href='/cookbook/4.1/recipes/signalxy_fillbelow/'>SignalXY with Fill</a></h2>

Various options allow shading above/below the signal data.

```cs
var plt = new ScottPlot.Plot(600, 400);

(double[] xs, double[] ys) = DataGen.RandomWalk2D(new Random(0), 5_000);

var sigxy = plt.AddSignalXY(xs, ys);
sigxy.FillBelow();

plt.Margins(x: 0);

plt.SaveFig("signalxy_fillBelow.png");
```

<img src='../../images/signalxy_fillbelow.png' class='d-block mx-auto my-5' />

