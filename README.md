# Assigment4
Implement 3 graphs of your choice for Fisher’s Iris dataset, using D3
<!DOCTYPE html>
<html>
<title>W3.CSS</title>
</style>
<header class="w3-container w3-red">
  <h1>Assigment #4 </h1>
</header>
<meta name="viewport" content="width=device-width, initial-scale=1">
<body>
<script src="//d3js.org/d3.v4.js"></script>
<script src="../build/d3-annotate.js"></script>
<script>
d3.csv("iris.csv", function (err, data) {
	if (!err)
		handleData(data);
});

var setosaColor = 'rgba(255, 0, 0, 0.7)',
	versicolorColor = 'rgba(204, 102, 0, 0.7)',
	virginicaColor = 'rgba(0, 200, 0, 0.7)';

$('#setosa').css('background-color', setosaColor);
$('#versicolor').css('background-color', versicolorColor);
$('#virginica').css('background-color', virginicaColor);

function handleData(data) {
	var 	w = 400,
		h = 400,
		padding = {
			top: 20,
			right: 40,
			bottom: 50,
			left: 40
		};

	var xScale = d3.scale.linear()
		.domain([0, 
			d3.max(data, 
			function (d) {
				return Math.max(d.sepalLength, d.petalLength);
			})])
		.range([padding.left, w - padding.right]);

	var yScale = d3.scale.linear()
		.domain([0, 
			d3.max(data, 
			function (d) {
				return Math.max(d.sepalWidth, d.petalWidth);
			})])
		.range([h - padding.bottom, padding.top]);

	var xAxis = d3.svg.axis()
		.scale(xScale)
		.orient('bottom')
		.ticks(8);

	var yAxis = d3.svg.axis()
		.scale(yScale)
		.orient('left')
		.ticks(5);

	var drawSpecies = function(svg, species, xParam, yParam, color) {
		svg.append('g').selectAll('circle')
			.data(data)
			.enter()
			.append('circle')
			.filter(function (d) { return d.species === species; })
			.attr('cx', function (d) { return xScale(d[xParam]); })
			.attr('cy', function (d) { return yScale(d[yParam]);})
			.attr('r', 3.5)
			.style('fill', color);
	};
    // lets create a scatter plot


	var createScatterPlot = function (title, attr1, attr2) {
		var svg = d3.select('#plots')
		.append('svg')
		.attr('width', w)
		.attr('height', h);

		svg.append('g')
			.attr('class', 'axis')
			.attr('transform', 'translate(0,' + (h - padding.bottom) + ')')
			.call(xAxis);

		svg.append('g')
			.attr('class', 'axis')
			.attr('transform', 'translate(' + padding.left + ', 0)')
			.call(yAxis);

		svg.append('g')
			.attr('class', 'label')
			.append('text')
			.attr('class', 'xlabel')
			.attr('text-anchor', 'middle')
			.attr('x', w / 2)
			.attr('y', h - 10)
			.text("Length (cm)");

		svg.append('g')
			.attr('class', 'label')
			.append('text')
			.attr('class', 'xlabel')
			.attr('text-anchor', 'middle')
			.attr('x', - h / 2)
			.attr('y', 10)
			.attr('transform', 'rotate(-90)')
			.text("Width (cm)");

		svg.append('g')
			.append('text')
			.attr('class', 'title')
			.attr('text-anchor', 'middle')
			.attr('x', w / 2)
			.attr('y', 15)
			.text(title);
//visualizing data set 
		drawSpecies(svg, "setosa", attr1, attr2, setosaColor);
		drawSpecies(svg, "versicolor", attr1, attr2, versicolorColor);
		drawSpecies(svg, "virginica", attr1, attr2, virginicaColor);
	};
//make a scatterplot from data
	createScatterPlot("Sepal", "sepalLength", "sepalWidth");
	createScatterPlot("Petal", "petalLength", "petalWidth");
}

