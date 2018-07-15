```
                 _-====-__-=-__-===-__-=======-__
               _(                               _)
            OO(                                )
         . o  '===-______-===-____-==-__-====='
      .o
     . ______          _______________
   _()_||__|| __o^o___ | [] [] [] [] |
  (           |      | |             |o
 /-OO----OO""="OO--OO"="OO---------OO"
############################################################
```

# pfaedle

Precise map-matching for public transit schedules (GTFS data).

## Requirements

 * `cmake`
 * `gcc` >= 4.8

## Building and Installation

Fetch this repository and init submodules:

```
git clone --recurse-submodules https://github.com/ad-freiburg/pfaedle
```

```
mkdir build && cd build
cmake ..
make -j
```

To install, type
```
make install
```

# General Usage

## Generating shapes for a GTFS feed

```
pfaedle -c <CFG FILE> -x <OSM FILE> <GTFS INPUT FOLDER>
```

A shape'd version of the input GTFS feed will be written to `./gtfs-out`.

A default configuration file `pfaedle.cfg` can be found in this repo.

By default, shapes are only calculated for trips that don't have a shape in the
input feed. To drop all existing shapes, use the `-D` flag.

## Generating shapes for a specific MOT

To generate shapes only for a specific mot, use the `-m` option. Possible
values are either `tram`, `bus`, `rail`, `subway`, `ferry`, `funicular`,
`gondola`, `all`.

Multiple values can be specified (comma separated).

## OSM filtering

`pfaedle` comes with the ability to filter OpenStreetMap data. If you specify
the `-X` flag, `pfaedle` will filter the input OSM file and output a new OSM
file which contains *exactly* the data needed to calculate the shapes for the
input GTFS feed and the input configuration.

This can be used to avoid parsing (for example) the entire world.osm on each
run.

## Debugging

The following flags may be useful for debugging:

 * `-T <GTFS TRIP ID>` only calculate shape for a single trip (specified via its GTFS trip id) and output it as GeoJSON to
   `<dbg-path>/path.json`
 * `--write-graph` write the graph used for routing as GeoJSON to
   `<dbg-path>/graph.json`
 * `--write-cgraph` if `-T` is set, write the combination graph used for
   routing as GeoJSON to `<dbg-path>/combgraph.json`

# Configuration

The main config file distributed with this repository is `pfaedle.cfg`. The
config file has some comments which hopefully explain the meaning behind the
parameters

# Evaluation

You may run an entire evaluation of our testing datasets Vitoria-Gasteiz, Paris, Switzerland and
Stuttgart with

```
mkdir build && cd build
cmake ..
make -j
make eval
```

*Note:* this will download, and filter, the entire OSM files for Spain and the
Stuttgart region. Make sure you have enough space left on your hard drive.

## Evaluation requirements

 * zlib

On Debianesque systems, type

```
sudo apt-get install zlib1g-dev
```

to install the dependencies.
