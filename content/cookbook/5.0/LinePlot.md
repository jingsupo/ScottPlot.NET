---
Title: Line Plot - ScottPlot 5.0 Cookbook
Description: Line plots can be placed on the plot in coordinate space using a Start, End, and an optional LineStyle.
URL: /cookbook/5.0/LinePlot
BreadcrumbNames: ["ScottPlot 5.0 Cookbook", "Line Plot"]
BreadcrumbUrls: ["/cookbook/5.0/", "/cookbook/5.0/LinePlot"]
Date: 2023-12-24
Version: ScottPlot 5.0.11-beta
Version: ScottPlot 5.0.11-beta
SearchUrl: "/cookbook/5.0/search/"
---

# Line Plot



<div class='alert alert-warning' role='alert'><h4 class='alert-heading py-0 my-0'>⚠️ ScottPlot 5.0.11-beta is a preview package</h4><hr /><p class='mb-0'><span class='fw-semibold'>This page describes a beta release of ScottPlot.</span> It is available on NuGet as a preview package, but its API is not stable and it is not recommended for production use. See the <a href='https://scottplot.net/versions/'>ScottPlot Versions</a> page for more information. </p></div>



## Line Plot Quickstart

Line plots are placed with a start and end location in coordinate space. Their styles can be customized.

[![](/cookbook/5.0/images/LineQuickStart.png)](/cookbook/5.0/images/LineQuickStart.png)

```cs
ScottPlot.Plot myPlot = new();

myPlot.Add.Line(1, 12, 12, 0);
myPlot.Add.Line(7, 9, 42, 9);
myPlot.Add.Line(30, 17, 30, 1);

myPlot.SavePng("demo.png");

```


## Line Plot Shapes

Line plots can be styled using a LineStyle.

[![](/cookbook/5.0/images/LinePlotStyles.png)](/cookbook/5.0/images/LinePlotStyles.png)

```cs
ScottPlot.Plot myPlot = new();

ScottPlot.Colormaps.Viridis colormap = new();

for (int i = 0; i < 10; i++)
{
    // add a line
    Coordinates start = Generate.RandomCoordinates();
    Coordinates end = Generate.RandomCoordinates();
    var line = myPlot.Add.Line(start, end);

    // customize the line
    line.LineStyle.Color = Generate.RandomColor(colormap);
    line.LineStyle.Width = Generate.RandomInteger(1, 4);
    line.LineStyle.Pattern = Generate.RandomLinePattern();

    // customize markers
    line.MarkerStyle.Fill.Color = line.LineStyle.Color;
    line.MarkerStyle.Shape = Generate.RandomMarkerShape();
    line.MarkerStyle.Size = Generate.RandomInteger(5, 15);
}

myPlot.SavePng("demo.png");

```


## Line Plot Legend 123

Line plots with labels appear in the legend.

[![](/cookbook/5.0/images/LinePlotLegendQWER.png)](/cookbook/5.0/images/LinePlotLegendQWER.png)

```cs
ScottPlot.Plot myPlot = new();

var sin = myPlot.Add.Signal(Generate.Sin());
var cos = myPlot.Add.Signal(Generate.Cos());

var line = myPlot.Add.Line(1, 12, 12, 0);
line.LineStyle.Width = 3;
line.MarkerStyle.Size = 10;

sin.Label = "Sine";
cos.Label = "Cosine";
line.Label = "Line Plot";

myPlot.ShowLegend(Alignment.UpperRight);

myPlot.SavePng("demo.png");

```
