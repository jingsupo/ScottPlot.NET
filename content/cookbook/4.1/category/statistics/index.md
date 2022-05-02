---
Title: Statistics - ScottPlot 4.1 Cookbook
Description: Statistics recipes
Source: https://github.com/ScottPlot/ScottPlot/tree/master/src/cookbook
---

## Histogram

ScottPlot.Statistics.Common contains methods for creating histograms.

```cs
var plt = new ScottPlot.Plot(600, 400);

// generate sample heights are based on https://ourworldindata.org/human-height
Random rand = new(0);
double[] values = ScottPlot.DataGen.RandomNormal(rand, pointCount: 1234, mean: 178.4, stdDev: 7.6);

// create a histogram
(double[] counts, double[] binEdges) = ScottPlot.Statistics.Common.Histogram(values, min: 140, max: 220, binSize: 1);
double[] leftEdges = binEdges.Take(binEdges.Length - 1).ToArray();

// display the histogram counts as a bar plot
var bar = plt.AddBar(values: counts, positions: leftEdges);
bar.BarWidth = 1;

// customize the plot style
plt.YAxis.Label("Count (#)");
plt.XAxis.Label("Height (cm)");
plt.SetAxisLimits(yMin: 0);

plt.SaveFig("stats_histogram.png");
```

<div class='text-center'>
<a href='../../images/stats_histogram.png'><img src='../../images/stats_histogram.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Histogram Probability

Histograms can be displayed as binned probability instead of binned counts. The ideal probability distribution can also be plotted.

```cs
var plt = new ScottPlot.Plot(600, 400);

// generate sample heights are based on https://ourworldindata.org/human-height
Random rand = new(0);
double[] values = ScottPlot.DataGen.RandomNormal(rand, pointCount: 1234, mean: 178.4, stdDev: 7.6);

// create a histogram
(double[] probabilities, double[] binEdges) = ScottPlot.Statistics.Common.Histogram(values, min: 140, max: 220, binSize: 1, density: true);
double[] leftEdges = binEdges.Take(binEdges.Length - 1).ToArray();

// display histogram probabability as a bar plot
var bar = plt.AddBar(values: probabilities, positions: leftEdges);
bar.BarWidth = 1;
bar.FillColor = ColorTranslator.FromHtml("#9bc3eb");
bar.BorderColor = ColorTranslator.FromHtml("#82add9");

// display histogram distribution curve as a line plot
double[] densities = ScottPlot.Statistics.Common.ProbabilityDensity(values, binEdges);
plt.AddScatterLines(
    xs: binEdges,
    ys: densities,
    color: Color.Black,
    lineWidth: 2,
    lineStyle: LineStyle.Dash);

// customize the plot style
plt.Title("Adult Male Height");
plt.YAxis.Label("Probability");
plt.XAxis.Label("Height (cm)");
plt.SetAxisLimits(yMin: 0);

plt.SaveFig("stats_histogramProbability.png");
```

<div class='text-center'>
<a href='../../images/stats_histogramprobability.png'><img src='../../images/stats_histogramprobability.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Histogram Multi-Axis

This example demonstrates how to display a histogram counts on the primary Y axis and the probability curve on the secondary Y axis.

```cs
var plt = new ScottPlot.Plot(600, 400);

// generate sample heights are based on https://ourworldindata.org/human-height
Random rand = new(0);
double[] values = ScottPlot.DataGen.RandomNormal(rand, pointCount: 1234, mean: 178.4, stdDev: 7.6);

// create a histogram
(double[] counts, double[] binEdges) = ScottPlot.Statistics.Common.Histogram(values, min: 140, max: 220, binSize: 1);
double[] leftEdges = binEdges.Take(binEdges.Length - 1).ToArray();

// display histogram probabability as a bar plot
var bar = plt.AddBar(values: counts, positions: leftEdges);
bar.BarWidth = .6;
bar.FillColor = ColorTranslator.FromHtml("#9bc3eb");
bar.BorderLineWidth = 0;

// display histogram distribution curve as a line plot on a secondary Y axis
double[] densities = ScottPlot.Statistics.Common.ProbabilityDensity(values, binEdges, percent: true);
var probPlot = plt.AddScatterLines(
    xs: binEdges,
    ys: densities,
    lineWidth: 2);
probPlot.YAxisIndex = 1;
plt.YAxis2.Ticks(true);
plt.YAxis2.Color(probPlot.Color);

// customize the plot style
plt.Title("Adult Male Height");
plt.YAxis.Label("Count (#)");
plt.YAxis2.Label("Probability (%)");
plt.XAxis.Label("Height (cm)");
plt.SetAxisLimits(yMin: 0);
plt.SetAxisLimits(yMin: 0, yAxisIndex: 1);

plt.SaveFig("stats_histogramMultiAxis.png");
```

<div class='text-center'>
<a href='../../images/stats_histogrammultiaxis.png'><img src='../../images/stats_histogrammultiaxis.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Histogram Stdev

This example demonstrates how to display a histogram with labeled mean and standard deviations.

```cs
var plt = new ScottPlot.Plot(600, 400);

// generate sample heights are based on https://ourworldindata.org/human-height
Random rand = new(0);
double[] values = ScottPlot.DataGen.RandomNormal(rand, pointCount: 1234, mean: 178.4, stdDev: 7.6);

// create a histogram
(double[] counts, double[] binEdges) = ScottPlot.Statistics.Common.Histogram(values, min: 140, max: 220, binSize: 1);
double[] leftEdges = binEdges.Take(binEdges.Length - 1).ToArray();

// display histogram probabability as a bar plot
var bar = plt.AddBar(values: counts, positions: leftEdges);
bar.FillColor = ColorTranslator.FromHtml("#9bc3eb");
bar.BorderLineWidth = 0;

// display histogram distribution curve as a line plot on a secondary Y axis
double[] smoothEdges = ScottPlot.DataGen.Range(start: binEdges.First(), stop: binEdges.Last(), step: 0.1, includeStop: true);
double[] smoothDensities = ScottPlot.Statistics.Common.ProbabilityDensity(values, smoothEdges, percent: true);
var probPlot = plt.AddScatterLines(
    xs: smoothEdges,
    ys: smoothDensities,
    lineWidth: 2,
    label: "probability");
probPlot.YAxisIndex = 1;
plt.YAxis2.Ticks(true);

// display vertical lines at points of interest
var stats = new ScottPlot.Statistics.BasicStats(values);

plt.AddVerticalLine(stats.Mean, Color.Black, 2, LineStyle.Solid, "mean");

plt.AddVerticalLine(stats.Mean - stats.StDev, Color.Black, 2, LineStyle.Dash, "1 SD");
plt.AddVerticalLine(stats.Mean + stats.StDev, Color.Black, 2, LineStyle.Dash);

plt.AddVerticalLine(stats.Mean - stats.StDev * 2, Color.Black, 2, LineStyle.Dot, "2 SD");
plt.AddVerticalLine(stats.Mean + stats.StDev * 2, Color.Black, 2, LineStyle.Dot);

plt.AddVerticalLine(stats.Min, Color.Gray, 1, LineStyle.Dash, "min/max");
plt.AddVerticalLine(stats.Max, Color.Gray, 1, LineStyle.Dash);

plt.Legend(location: Alignment.UpperRight);

// customize the plot style
plt.Title("Adult Male Height");
plt.YAxis.Label("Count (#)");
plt.YAxis2.Label("Probability (%)");
plt.XAxis.Label("Height (cm)");
plt.SetAxisLimits(yMin: 0);
plt.SetAxisLimits(yMin: 0, yAxisIndex: 1);

plt.SaveFig("stats_histogramStdev.png");
```

<div class='text-center'>
<a href='../../images/stats_histogramstdev.png'><img src='../../images/stats_histogramstdev.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Multiple Histograms

This example demonstrates two histograms on the same plot. Note the use of fractional units on the vertical axis, allowing easy comparison of datasets with different numbers of points. Unlike the previous example, this one does not use multiple axes.

```cs
var plt = new ScottPlot.Plot(600, 400);

// male and female heights are based on https://ourworldindata.org/human-height
Random rand = new(0);
double[] heightsMale = ScottPlot.DataGen.RandomNormal(rand, pointCount: 2345, mean: 178.4, stdDev: 7.6);
double[] heightsFemale = ScottPlot.DataGen.RandomNormal(rand, pointCount: 1234, mean: 164.7, stdDev: 7.1);

// calculate histograms for male and female datasets
(double[] probMale, double[] binEdges) = ScottPlot.Statistics.Common.Histogram(heightsMale, min: 140, max: 210, binSize: 1, density: true);
(double[] probFemale, _) = ScottPlot.Statistics.Common.Histogram(heightsFemale, min: 140, max: 210, binSize: 1, density: true);
double[] leftEdges = binEdges.Take(binEdges.Length - 1).ToArray();

// convert probabilities to percents
probMale = probMale.Select(x => x * 100).ToArray();
probFemale = probFemale.Select(x => x * 100).ToArray();

// plot histograms
var barMale = plt.AddBar(values: probMale, positions: leftEdges);
barMale.BarWidth = 1;
barMale.FillColor = Color.FromArgb(50, Color.Blue);
barMale.BorderLineWidth = 0;

var barFemale = plt.AddBar(values: probFemale, positions: leftEdges);
barFemale.BarWidth = 1;
barFemale.FillColor = Color.FromArgb(50, Color.Red);
barFemale.BorderLineWidth = 0;

// plot probability function curves
double[] pdfMale = ScottPlot.Statistics.Common.ProbabilityDensity(heightsMale, binEdges, percent: true);
plt.AddScatterLines(
    xs: binEdges,
    ys: pdfMale,
    color: Color.FromArgb(150, Color.Blue),
    lineWidth: 3,
    label: $"Male (n={heightsMale.Length:N0})");

double[] pdfFemale = ScottPlot.Statistics.Common.ProbabilityDensity(heightsFemale, binEdges, percent: true);
plt.AddScatterLines(
    xs: binEdges,
    ys: pdfFemale,
    color: Color.FromArgb(150, Color.Red),
    lineWidth: 3,
    label: $"Female (n={heightsFemale.Length:N0})");

// customize styling
plt.Title("Human Height by Sex");
plt.YLabel("Probability (%)");
plt.XLabel("Height (cm)");
plt.Legend(location: ScottPlot.Alignment.UpperLeft);
plt.SetAxisLimits(yMin: 0);

plt.SaveFig("stats_histogram2.png");
```

<div class='text-center'>
<a href='../../images/stats_histogram2.png'><img src='../../images/stats_histogram2.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## CPH

This example demonstrates how to plot a cumulative probability histogram (CPH) to compare the distribution of two datasets.

```cs
var plt = new ScottPlot.Plot(600, 400);

// create sample data for two datasets
Random rand = new Random(0);
double[] values1 = DataGen.RandomNormal(rand, pointCount: 1000, mean: 50, stdDev: 20);
double[] values2 = DataGen.RandomNormal(rand, pointCount: 1000, mean: 45, stdDev: 25);
(double[] hist1, double[] binEdges) = ScottPlot.Statistics.Common.Histogram(values1, min: 0, max: 100, binSize: 1, density: true);
(double[] hist2, _) = ScottPlot.Statistics.Common.Histogram(values2, min: 0, max: 100, binSize: 1, density: true);
double[] cph1 = ScottPlot.Statistics.Common.CumulativeSum(hist1);
double[] cph2 = ScottPlot.Statistics.Common.CumulativeSum(hist2);
double[] leftEdges = binEdges.Take(binEdges.Length - 1).ToArray();

// display datasets as step plots
plt.AddScatterStep(xs: leftEdges, ys: cph1, label: "Sample A");
plt.AddScatterStep(xs: leftEdges, ys: cph2, label: "Sample B");

// decorate the plot
plt.Legend();
plt.SetAxisLimits(yMin: 0, yMax: 1);
plt.Title("Cumulative Probability Histogram");
plt.XAxis.Label("Probability (fraction)");
plt.YAxis.Label("Value (units)");

plt.SaveFig("stats_cph.png");
```

<div class='text-center'>
<a href='../../images/stats_cph.png'><img src='../../images/stats_cph.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Linear Regression

A regression module is available to simplify the act of creating a linear regression line fitted to the data.

```cs
var plt = new ScottPlot.Plot(600, 400);

// Create some linear but noisy data
double[] ys = DataGen.NoisyLinear(null, pointCount: 100, noise: 30);
double[] xs = DataGen.Consecutive(ys.Length);
double x1 = xs[0];
double x2 = xs[xs.Length - 1];

// use the linear regression fitter to fit these data
var model = new ScottPlot.Statistics.LinearRegressionLine(xs, ys);

// plot the original data and add the regression line
plt.Title("Linear Regression\n" +
    $"Y = {model.slope:0.0000}x + {model.offset:0.0} " +
    $"(R² = {model.rSquared:0.0000})");
plt.AddScatter(xs, ys, lineWidth: 0);
plt.AddLine(model.slope, model.offset, (x1, x2), lineWidth: 2);

plt.SaveFig("stats_linearRegression.png");
```

<div class='text-center'>
<a href='../../images/stats_linearregression.png'><img src='../../images/stats_linearregression.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Nth Order Statistics

The Nth order statistic of a set is the Nth smallest value of the set (indexed from 1).

```cs
var plt = new ScottPlot.Plot(600, 400);

Random rand = new Random(0);
int pointCount = 500;
double[] xs = DataGen.Consecutive(pointCount);
double[] ys = DataGen.Random(rand, pointCount);

int n = 200;
double nthValue = Statistics.Common.NthOrderStatistic(ys, n);

plt.Title($"{n}th Smallest Value (of {pointCount})");
plt.AddScatter(xs, ys, lineWidth: 0, markerShape: MarkerShape.openCircle);
plt.AddHorizontalLine(nthValue, width: 3, style: LineStyle.Dash);

plt.SaveFig("stats_orderStatistics.png");
```

<div class='text-center'>
<a href='../../images/stats_orderstatistics.png'><img src='../../images/stats_orderstatistics.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Percentiles

Percentiles are a good tool to analyze the distribution of your data and filter out extreme values.

```cs
var plt = new ScottPlot.Plot(600, 400);

Random rand = new Random(0);
int pointCount = 500;
double[] xs = DataGen.Consecutive(pointCount);
double[] ys = DataGen.Random(rand, pointCount);

double tenthPercentile = Statistics.Common.Percentile(ys, 10);

plt.Title("10th Percentile");
plt.AddScatter(xs, ys, lineWidth: 0, markerShape: MarkerShape.openCircle);
plt.AddHorizontalLine(tenthPercentile, width: 3, style: LineStyle.Dash);

plt.SaveFig("stats_percentiles.png");
```

<div class='text-center'>
<a href='../../images/stats_percentiles.png'><img src='../../images/stats_percentiles.png' /></a>
</div>


<div class='m-2'>&nbsp;</div>

## Quantiles

A q-Quantile is a generalization of quartiles and percentiles to any number of buckets.

```cs
var plt = new ScottPlot.Plot(600, 400);

Random rand = new Random(0);
int pointCount = 500;
double[] xs = DataGen.Consecutive(pointCount);
double[] ys = DataGen.Random(rand, pointCount);

// A septile is a 7-quantile
double secondSeptile = Statistics.Common.Quantile(ys, 2, 7);

plt.Title("Second Septile");
plt.AddScatter(xs, ys, lineWidth: 0, markerShape: MarkerShape.openCircle);
plt.AddHorizontalLine(secondSeptile, width: 3, style: LineStyle.Dash);

plt.SaveFig("stats_quantiles.png");
```

<div class='text-center'>
<a href='../../images/stats_quantiles.png'><img src='../../images/stats_quantiles.png' /></a>
</div>
