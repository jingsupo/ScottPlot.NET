---
Title: Axis Line and Span - ScottPlot 4.1 Cookbook
Description: Plottable - Axis Line and Span recipes
Source: https://github.com/ScottPlot/ScottPlot/tree/master/src/cookbook
---

## Axis Line

An axis line marks a position on an axis. Axis lines extend to positive and negative infinity on the other axis.

```cs
var plt = new ScottPlot.Plot(600, 400);

// plot sample data
plt.AddSignal(DataGen.Sin(51));
plt.AddSignal(DataGen.Cos(51));

// add axis lines
plt.AddHorizontalLine(.85);
plt.AddVerticalLine(23);

// customize axis lines with optional arguments
plt.AddVerticalLine(x: 33, color: Color.Magenta, width: 3, style: LineStyle.Dot);

plt.SaveFig("axisLine_basics.png");
```

<div class='text-center'>
<a href='../../images/axisline_basics.png'><img src='../../images/axisline_basics.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Finite Axis Line

Axis lines can have lower and/or upper bounds. This can be useful for labeling points of interest.

```cs
var plt = new ScottPlot.Plot(600, 400);

plt.AddFunction(x => Math.Pow(x, 2), lineStyle: LineStyle.Dash);
plt.AddFunction(x => Math.Sqrt(x), lineStyle: LineStyle.Dash);

// mark a coordinate from the lower left
var point1 = plt.AddPoint(1, 1, size: 10, shape: MarkerShape.openCircle);
var hLine1 = plt.AddHorizontalLine(1, width: 2);
hLine1.Max = 1;
hLine1.Color = point1.Color;
var vLine1 = plt.AddVerticalLine(1, width: 2);
vLine1.Max = 1;
vLine1.Color = point1.Color;

// use finate upper and lower limits draw a cross on a point
var point2 = plt.AddPoint(4, 2, size: 10, shape: MarkerShape.openCircle);
var vLine2 = plt.AddVerticalLine(4, width: 2);
vLine2.Min = 1.5;
vLine2.Max = 2.5;
vLine2.Color = point2.Color;
var hLine2 = plt.AddHorizontalLine(2, width: 2);
hLine2.Min = 3.5;
hLine2.Max = 4.5;
hLine2.Color = point2.Color;

// mark a coordinate from the top right
var point3 = plt.AddPoint(2, 4, size: 10, shape: MarkerShape.openCircle);
var hLine3 = plt.AddHorizontalLine(4, width: 2);
hLine3.Min = 2;
hLine3.Color = point3.Color;
var vLine3 = plt.AddVerticalLine(2, width: 2);
vLine3.Min = 4;
vLine3.Color = point3.Color;

plt.SetAxisLimits(0, 5, 0, 5);

plt.SaveFig("axisLine_finite.png");
```

<div class='text-center'>
<a href='../../images/axisline_finite.png'><img src='../../images/axisline_finite.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Draggable Axis Lines

In GUI environments, axis lines can be draggable and moved with the mouse. Drag limits define the boundaries the lines can be dragged.

```cs
var plt = new ScottPlot.Plot(600, 400);

// plot sample data
plt.AddSignal(DataGen.Sin(51));
plt.AddSignal(DataGen.Cos(51));

// add axis lines and configure their drag settings
var hLine = plt.AddHorizontalLine(.85);
hLine.DragEnabled = true;
hLine.DragLimitMin = -1;
hLine.DragLimitMax = 1;

var vLine = plt.AddVerticalLine(23);
vLine.DragEnabled = true;
vLine.DragLimitMin = 0;
vLine.DragLimitMax = 50;

// you can access the position of an axis line at any time
string message = $"Vertical line is at X={vLine.X}";

plt.SaveFig("axisLine_draggable.png");
```

<div class='text-center'>
<a href='../../images/axisline_draggable.png'><img src='../../images/axisline_draggable.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Position Labels

Axis line positions can be labeled on the axis on top of axis ticks and tick labels. Custom position formatters allow for full customization of the text displayed in these labels. If using a DateTime axis, implement a custom formatter that uses DateTime.FromOADate().

```cs
var plt = new ScottPlot.Plot(600, 400);

plt.AddSignal(DataGen.Sin(51));
plt.AddSignal(DataGen.Cos(51));

var hline = plt.AddHorizontalLine(.85);
hline.LineWidth = 2;
hline.PositionLabel = true;
hline.PositionLabelBackground = hline.Color;
hline.DragEnabled = true;

var vline = plt.AddVerticalLine(23);
vline.LineWidth = 2;
vline.PositionLabel = true;
vline.PositionLabelBackground = vline.Color;
vline.DragEnabled = true;

Func<double, string> xFormatter = x => $"X={x:F2}";
vline.PositionFormatter = xFormatter;

plt.SaveFig("axisLine_positionLabels.png");
```

<div class='text-center'>
<a href='../../images/axisline_positionlabels.png'><img src='../../images/axisline_positionlabels.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Axis Span

Axis spans shade a portion of one axis. Axis spans extend to negative and positive infinity on the other axis.

```cs
var plt = new ScottPlot.Plot(600, 400);

// plot sample data
plt.AddSignal(DataGen.Sin(51));
plt.AddSignal(DataGen.Cos(51));

// add axis spans
plt.AddVerticalSpan(.15, .85);
plt.AddHorizontalSpan(10, 25);

plt.SaveFig("axisSpan_quickstart.png");
```

<div class='text-center'>
<a href='../../images/axisspan_quickstart.png'><img src='../../images/axisspan_quickstart.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Draggable Axis Span

Axis spans can be dragged using the mouse. Drag limits are boundaries over which the edges of spans cannot cross.

```cs
var plt = new ScottPlot.Plot(600, 400);

// plot sample data
plt.AddSignal(DataGen.Sin(51));
plt.AddSignal(DataGen.Cos(51));

// dragging can be enabled and optionally limited to a range
var vSpan = plt.AddVerticalSpan(.15, .85);
vSpan.DragEnabled = true;
vSpan.DragLimitMin = -1;
vSpan.DragLimitMax = 1;
vSpan.BorderColor = Color.Red;
vSpan.BorderLineStyle = LineStyle.Dot;
vSpan.BorderLineWidth = 2;
vSpan.HatchColor = Color.FromArgb(100, Color.Blue);
vSpan.HatchStyle = Drawing.HatchStyle.SmallCheckerBoard;
vSpan.Label = "Customized vSpan";


// spans can be configured to allow dragging but disallow resizing
var hSpan = plt.AddHorizontalSpan(10, 25);
hSpan.DragEnabled = true;
hSpan.DragFixedSize = true;
hSpan.Label = "Standard hSpan";
plt.Legend(true);

plt.SaveFig("axisSpan_draggable.png");
```

<div class='text-center'>
<a href='../../images/axisspan_draggable.png'><img src='../../images/axisspan_draggable.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Ignore Axis Limits

Calling Plot.AxisAuto (or middle-clicking the plot) will set the axis limits automatically to fit the data on the plot. By default the position of axis lines and spans are included in automatic axis limit calculations, but setting the '' flag can disable this behavior.

```cs
var plt = new ScottPlot.Plot(600, 400);

plt.AddSignal(DataGen.Sin(51));
plt.AddSignal(DataGen.Cos(51));

var hline = plt.AddHorizontalLine(0.23);
hline.DragEnabled = true;
hline.IgnoreAxisAuto = true;

var hSpan = plt.AddHorizontalSpan(-10, 20);
hSpan.DragEnabled = true;
hSpan.IgnoreAxisAuto = true;

plt.SaveFig("axisSpan_ignore.png");
```

<div class='text-center'>
<a href='../../images/axisspan_ignore.png'><img src='../../images/axisspan_ignore.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Repeating Axis Line

Repeating axis lines allows to plot several axis lines, either horizontal or vertical, draggable or not, whose positions are linked

```cs
var plt = new ScottPlot.Plot(600, 400);

//Generate a single signal containing 3 harmonic signals
int sampleCount = 500;
double[] signal1 = ScottPlot.DataGen.Sin(sampleCount, 10);
double[] signal2 = ScottPlot.DataGen.Sin(sampleCount, 20);
double[] signal3 = ScottPlot.DataGen.Sin(sampleCount, 30);

double[] signal = new double[sampleCount];
for (int index = 0; index < sampleCount; index++)
{
    signal[index] = signal1[index] + signal2[index] + signal3[index];
}

// Plot the signal
plt.AddSignal(signal);

// Create a draggable RepeatingVLine with 5 lines spaced evenly by 50 X units, starting at position 0
// The first line will be thicker than the others
ScottPlot.Plottable.RepeatingVLine vlines1 = new();
vlines1.DragEnabled = true;
vlines1.count = 5;
vlines1.shift = 50;
vlines1.Color = System.Drawing.Color.Magenta;
vlines1.LineWidth = 2;
vlines1.LineStyle = LineStyle.Dash;
vlines1.PositionLabel = true;
vlines1.PositionLabelBackground = vlines1.Color;
vlines1.relativeposition = false;
plt.Add(vlines1);

// Create a draggable RepeatingVLine with 5 lines spaced evenly by 50 X units, starting at position 0, with a -4 offset
// The first line will be thicker than the others
ScottPlot.Plottable.RepeatingVLine vlines2 = new();
vlines2.DragEnabled = true;
vlines2.count = 3;
vlines2.shift = 50;
vlines2.offset = -1;
vlines2.Color = System.Drawing.Color.DarkGreen;
vlines2.LineWidth = 2;
vlines2.LineStyle = LineStyle.Dot;
vlines2.PositionLabel = true;
vlines2.PositionLabelBackground = vlines2.Color;
vlines2.relativeposition = false;
plt.Add(vlines2);

plt.SetAxisLimitsX(-100, 300);

plt.SaveFig("repeatingAxisLine_basics.png");
```

<div class='text-center'>
<a href='../../images/repeatingaxisline_basics.png'><img src='../../images/repeatingaxisline_basics.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Axis Line Vector

An AxisLineVector allows to setup a series of VLines or HLines, without hassle.These lines can optionally be dragged as their counterparts

```cs
var plt = new ScottPlot.Plot(600, 400);

Random rand = new Random(0);
double[] xs = DataGen.Random(rand, 50);
double[] ys = DataGen.Random(rand, 50);

var scatter = plt.AddScatterPoints(xs, ys, Color.Blue, 10);

var vlines = new ScottPlot.Plottable.VLineVector();
vlines.Xs = new double[] { xs[1], xs[12], xs[35] };
vlines.Color = Color.Red;
vlines.PositionLabel = true;
vlines.PositionLabelBackground = vlines.Color;

var hlines = new ScottPlot.Plottable.HLineVector();
hlines.Ys = new double[] { ys[1], ys[12], ys[35] };
hlines.Color = Color.DarkCyan;
hlines.PositionLabel = true;
hlines.PositionLabelBackground = hlines.Color;
hlines.DragEnabled = true;

plt.Add(scatter);
plt.Add(vlines);
plt.Add(hlines);

plt.SaveFig("axisLine_Vector.png");
```

<div class='text-center'>
<a href='../../images/axisline_vector.png'><img src='../../images/axisline_vector.png' /></a>
</div>
