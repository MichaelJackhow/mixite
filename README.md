# Mixite

Mixite is a hexagonal grid library. The motivation behind it is to have an optimized, simple and usable library for drawing
hexagonal grids without being tied to any GUI framework.

This means that you can use Mixite on Android, your backend or your desktop app.

There is a REST-based web example which you can tinker with [here][herokurestlink]. *(not recommended, this is under rewrite)*

You can also check out the mixite.example.swt project [here][exampleprojectslink].

Mixite currently supports a maximum grid size of 1000 * 1000 (1.000.000 cells) with the default implementation but
you can provide your own storage implementation to alleviate this limitation.

[![][maven img]][maven]
[![][license img]][license]

## Getting started

This library uses [Amit's guide to hexagonal grids][amitlink]. The coordinate system used by this library is the Cubic
coordinate system. Please check [here][cubecoords] for further details.

Hexagonal grids come in **flat topped** and **pointy topped** shapes. The grid can have several layouts:
 - Hexagonal: the width and height of a this layout has to be equal and both have to be an odd number.
 - Triangular: the width and height of a this layout has to be equal.
 - Rectangular: no special rules
 - Trapezoid: no special rules

All layouts have *with* and *height* values of at least **1**.
You can consult [HexagonalGridLayout][hexgridlayout] if you need further details.

This library is not tied to any GUI implementation. All operations provided by the [API][api] are working using the most
abstract concept possible.

## Basic usage

### Maven dependency
Let's start by adding Mixite as a Maven dependency to your project:

    <dependency>
	    <groupId>org.hexworks.mixite</groupId>
	    <artifactId>mixite.core</artifactId>
	    <version>2018.1.0</version>
    </dependency>

You can also use Gradle:

    'org.hexworks.mixite:mixite.core:2018.1.0'


### Creating a grid

You can use the [HexagonalGridBuilder][hexgridbuilder] from the API package to create a [HexagonalGrid][hexgrid]:

    import org.hexworks.mixite.core.api.HexagonOrientation;
    import org.hexworks.mixite.core.api.HexagonalGrid;
    import org.hexworks.mixite.core.api.HexagonalGridBuilder;
    import org.hexworks.mixite.core.api.HexagonalGridLayout;
    import org.hexworks.mixite.core.api.contract.SatelliteData;
    
    // ...
    private static final int GRID_HEIGHT = 9;
    private static final int GRID_WIDTH = 9;
    private static final HexagonalGridLayout GRID_LAYOUT = HexagonalGridLayout.RECTANGULAR;
    private static final HexagonOrientation ORIENTATION = HexagonOrientation.FLAT_TOP;
    private static final double RADIUS = 30;

    // ...
    HexagonalGridBuilder<SatelliteData> builder = new HexagonalGridBuilder<>()
            .setGridHeight(GRID_HEIGHT)
            .setGridWidth(GRID_WIDTH)
            .setGridLayout(GRID_LAYOUT)
            .setOrientation(ORIENTATION)
            .setRadius(RADIUS);

    HexagonalGrid grid = builder.build();

You can also use the [HexagonalGridBuilder][hexgridbuilder] to create a [HexagonalGridCalculator][hexgridcalc] for you which
supports advanced operations on [HexagonalGrid][hexgrid]s:

    import org.hexworks.mixite.core.api.HexagonalGridCalculator;
    // ...
    HexagonalGridCalculator calculator = builder.buildCalculatorFor(grid);

### Drawing a grid

You can fetch the `Hexagon`s stored on a grid using the `getHexagons` method:

    Iterable hexagons = grid.getHexagons();

After that you can iterate over all the `Point`s of your `Hexagon`s:

    for (Object hexagon : grid.getHexagons()) {
        // do your stuff
    }


Note that each `Point` represents a coordinate in 2D space. You can use them for drawing.

### Manipulating your grid

There are basically only one operation for manipulating your data on the grid:
The `Hexagon#setSatelliteData(T data)` operation with which you can add your own arbitrary
data to a `Hexagon` object. This means that once created a `HexagonalGrid` is immutable apart from the
satellite data you add.

There is also a `HexagonalGrid#clearSatelliteData()` method for clearing all satellite data from your grid.

The implementation of the `HexagonalGrid` is lazy. This means that it only stores data which is absolutely necessary
to keep in memory (the coordinates and your satellite data). Everything else is generated on the fly. The only limiting
factor of a grid at the moment is the coordinates (which consume memory) and the satellite data.

### GUI example:

You can find a simple GUI example in the `mixite.example.swt` project. Run it by doing the following steps.

1. Clone the project: `git clone git@github.com:Hexworks/mixite.git`
2. cd to the newly created `mixite` folder: `cd mixite/`
3. build the project: `./gradlew clean build` (or `gradlew clean build` on Windows)
4. run the created uberjar: `java -jar mixite.example.swt/build/libs/mixite.example.swt.jar`


### Supported operations
 - Querying the characteristics of the `HexagonGrid`
 - Fetching all the `Hexagon` objects from the grid
 - Getting a subset of Hexagons (using cube or offset coordinate range) from the grid
 - Checking whether a Hexagon is on a grid or not
 - Getting a `Hexagon` by its grid coordinate (cube)
 - Getting a `Hexagon` by its pixel coordinate
 - Getting the neighbors of a hexagon (also by index)

### Advanced operations
 - Calculating the distance between two `Hexagon`s
 - Calculating the movement range from a `Hexagon` to an other
 - Rotating a `Hexagon`
 - Calculating a ring from a `Hexagon`
 - Draw a line from a `Hexagon` to an other
 - Checking visibility of a `Hexagon` from an other
 - Adding custom data to a Hexagon
 - Clearing all custom data from the HexagonalGrid

Check these interfaces for more details:

- [HexagonalGridBuilder][hexgridbuilder]
- [HexagonalGrid][hexgrid]
- [HexagonalGridCalculator][hexgridcalc]
- [Hexagon][hex]

### Usage tips
 - You can add satellite data (any arbitrary data you have) to a `Hexagon`. By implementing the [`SatelliteData`][satdatlink]
   interface you gain operations like visibility checking
 - Mixite comes with a sensible default implementation of [`SatelliteData`][satdatlink] so if you don't want to add extra data
   you can use [`DefaultSatelliteData`][defsatdatlink].
 - You can use your own implementation of [`HexagonDataStorage`][hexdatstorlink] for storing your `Hexagon`s
 - Mixite comes with a sensible [`DefaultHexagonDataStorage`][defhexdatstorlink] implementation which stores all data in memory
 - You don't have to fetch all `Hexagon` objects by using the `getHexagons` method. You can query `Hexagon`s by a range using
   offset or cube coordinates

## Road map:
 - Path finding with obstacles  (blocking movement)
 - Movement range with obstacles and movement cost calculation
 - Android example

## Contributors

This project exists thanks to all the people who contribute. [[Contribute](CONTRIBUTING.md)].

## License
Mixite is made available under the [MIT License](http://www.opensource.org/licenses/mit-license.php).

## Credits
Mixite is created and maintained by Adam Arold

*I'm open to suggestions, feel free to comment or to send me a message.
Pull requests are also welcome!*


[maven]:http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22org.hexworks.mixite%22%20AND%20a%3A%22mixite.core%22
[maven img]:https://maven-badges.herokuapp.com/maven-central/org.hexworks.mixite/mixite.core/badge.svg

[license]:https://github.com/Hexworks/mixite/blob/master/LICENSE
[license img]:https://img.shields.io/badge/License-Apache-blue.svg

[amitlink]:http://www.redblobgames.com/grids/hexagons/
[cubecoords]:http://www.redblobgames.com/grids/hexagons/#coordinates
[herokurestlink]:http://mixite-rest-example.herokuapp.com/
[exampleprojectslink]:https://github.com/Hexworks/mixite.example.swt

[hexgridlayout]:https://github.com/Hexworks/mixite/blob/master/mixite.core/core/src/main/kotlin/org/hexworks/mixite/core/api/HexagonalGridLayout.kt
[hexgridbuilder]:https://github.com/Hexworks/mixite/blob/master/mixite.core/core/src/main/kotlin/org/hexworks/mixite/core/api/HexagonalGridBuilder.kt
[api]:https://github.com/Hexworks/mixite/tree/master/src/main/kotlin/org/hexworks/mixite/core/api
[hexgrid]:https://github.com/Hexworks/mixite/blob/master/mixite.core/core/src/main/kotlin/org/hexworks/mixite/core/api/HexagonalGrid.kt
[hexgridcalc]:https://github.com/Hexworks/mixite/blob/master/mixite.core/core/src/main/kotlin/org/hexworks/mixite/core/api/HexagonalGridCalculator.kt
[hex]:https://github.com/Hexworks/mixite/blob/master/mixite.core/core/src/main/kotlin/org/hexworks/mixite/core/api/Hexagon.kt
[satdatlink]:https://github.com/Hexworks/mixite/blob/master/mixite.core/core/src/main/kotlin/org/hexworks/mixite/core/api/contract/SatelliteData.kt
[defsatdatlink]:https://github.com/Hexworks/mixite/blob/master/mixite.core/core/src/main/kotlin/org/hexworks/mixite/core/api/defaults/DefaultSatelliteData.kt
[hexdatstorlink]:https://github.com/Hexworks/mixite/blob/master/mixite.core/core/src/main/kotlin/org/hexworks/mixite/core/api/contract/HexagonDataStorage.kt
[defhexdatstorlink]:https://github.com/Hexworks/mixite/blob/master/mixite.core/core/src/main/kotlin/org/hexworks/mixite/core/api/defaults/DefaultHexagonDataStorage.kt
