pnltri.js
=========

#### (Simple) Polygon Near-Linear Triangulation in JavaScript ####

This project implements an algorithm by Raimund Seidel [Sei91] for the triangulation of simple polygons in expected near-linear time with several improvements and extensions. It includes extensive unit and functional tests.

At present the algorithm handles simple polygons (no crossing edges) with holes. The contour and the holes are specified as vertex sequences.
The input can contain several polygons and or holes in any order. There is no need for a specific winding order clockwise (CW) or counterclockwise (CCW) as usual.
But repeating points (zero-length edges) and crossing edges are not allowed.

The output is a list of triangles. Each triangle is a triple of vertex indices. These refer to the order of the vertices in the input sequences, as if those had all been concatenated (numbering starts at 0). No Steiner points are added.
The output triangles for a specific polygon (with holes) are always the same, but since there is a random part in the algorithm and output is not sorted, their order changes.

At present this implementation handles some cases of co-linear segments gracefully, but no effort for completeness has been made yet.

In the future it shall handle crossing edges too - along the lines presented by Sigbjorn Vik in [Vik01].


### Usage ###

Download the [minified library](https://raw.github.com/jahting/pnltri.js/master/build/pnltri.min.js) and include it in your html.

```html
<script src="js/pnltri.min.js"></script>
```

This code specifies a polygon with 3 holes. It then creates the Triangulator and calls the algorithm. Finally it displays the result.

```html
<script type="text/javascript">
	// define polygon with holes
	var example_data = [
		// Contour
		[ { x:0.0, y:0.0 },	{ x:30.0, y:0.0 }, { x:30.0, y:40.0 }, { x:0.0, y:40.0 } ],
		// Holes
		[ { x: 2.5, y:26.0 },	{ x: 5.0, y:31.0 }, { x:10.0, y:28.5 } ],
		[ { x:27.5, y:25.0 },	{ x:25.0, y:30.0 }, { x:20.0, y:27.5 } ],
		[ { x:15.0, y:20.0 },	{ x:12.5, y:10.0 }, { x:18.0, y:10.0 } ],
						];

	// triangulate the polygon with holes
	var myTriangulator = new PNLTRI.Triangulator();
	var triangList = myTriangulator.triangulate_polygon( example_data );

	// output the result
	var resultStr = ( triangList ) ?
			triangList.map( function(tri) { return "[ "+tri.join(", ")+" ]" } ).join(", ") :
			'NO Triangle-List!';
	resultDiv = document.createElement('div');
	resultDiv.innerHTML = '<p/>'+resultStr+'<p/>';
	document.body.appendChild( resultDiv );
</script>
```

If everything went well you should see this list - probably in a different order.
```html
[ 0, 1, 11 ], [ 0, 4, 3 ], [ 0, 10, 4 ], [ 0, 11, 10 ], [ 1, 2, 7 ], [ 1, 7, 12 ],
[ 1, 12, 11 ], [ 2, 3, 8 ], [ 2, 8, 7 ], [ 3, 4, 5 ], [ 3, 5, 8 ], [ 4, 7, 9 ],
[ 4, 9, 6 ], [ 4, 10, 7 ], [ 5, 6, 8 ], [ 6, 9, 8 ], [ 7, 10, 12 ]
```

#### Integration into Three.js ####

In [Three.js](https://github.com/mrdoob/three.js) replace THREE.Shape.Utils.triangulateShape with:

```html
	triangulateShape: function ( contour, holes ) {
		var myTriangulator = new PNLTRI.Triangulator();
		return	myTriangulator.triangulate_polygon( [ contour ].concat(holes) );
	},
```

This works with all examples included in Three.js .

### Change log ###

[releases](https://github.com/jahting/pnltri.js/releases)

### References ###

[Sei91]
	AUTHOR = "Raimund Seidel",
	TITLE = "A simple and fast incremental randomized algorithm for computing trapezoidal decompositions and for triangulating polygons",
	JOURNAL = "Computational Geometry Theory & Applications",
	PAGES = "51-64",
	YEAR = 1991,
	NUMBER = 1,
	VOLUME = 1,
	[DOI](http://dx.doi.org/10.1016/0925-7721(91)90012-4)
	[Url](http://www.ime.usp.br/~walterfm/cursos/mac0331/2006/seidel.pdf)

[FoM84]
	AUTHOR = "Alain Fournier, Delfin Y. Montuno",
	TITLE = "Triangulating Simple Polygons and Equivalent Problems",
	JOURNAL = "ACM Transactions on Graphics",
	PAGES = "153-174",
	YEAR = 1984,
	VOLUME = 3
	NUMBER = 2,
	[Url](http://dl.acm.org/citation.cfm?doid=357337.357341)

[NaM95]
	AUTHOR = "Atul Narkhede, Dinesh Manocha",
	TITLE = "Fast polygon triangulation based on Seidel's Algorithm",
			Implementation report: UNC-CH, 1994.
	[Url](http://www.cs.unc.edu/~dm/CODE/GEM/chapter.html)

[Vik01]
	AUTHOR = "Sigbjorn Vik",
	TITLE = "An Implementation of a Near-Linear Polygon Triangulation Algorithm for General Polygons",
	YEAR = 2001,
	[Url](http://sigbjorn.vik.name/projects/Triangulation.pdf)


